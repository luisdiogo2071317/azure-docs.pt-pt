---
title: Como resolver problemas de desempenho de consulta na base de dados do Azure para MySQL
description: Este artigo descreve como utilizar EXPLICATIVO para resolver problemas de desempenho de consulta na base de dados do Azure para MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 72b047c37ac88e4b33c8723f8df14c6794e84399
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266181"
---
# <a name="how-to-use-explain-to-profile-query-performance-in-azure-database-for-mysql"></a>Como utilizar EXPLICATIVO para desempenho de consulta do perfil na base de dados do Azure para MySQL
**EXPLICAR** é uma ferramenta útil para otimizar as consultas. EXPLICAM instrução pode ser utilizada para obter informações sobre como as instruções SQL são executadas. O resultado seguinte mostra um exemplo da execução de uma instrução EXPLICATIVO.

```sql
mysql> EXPLAIN SELECT * FROM tb1 WHERE id=100\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 995789
     filtered: 10.00
        Extra: Using where
```

Como podem ser vistas neste exemplo, o valor de *chave* é NULL. Este resultado significa MySQL não é possível localizar qualquer índices otimizados para a consulta e executa uma análise completa de tabela. Vamos otimizar esta consulta, adicionando um índice no **ID** coluna.

```sql
mysql> ALTER TABLE tb1 ADD KEY (id);
mysql> EXPLAIN SELECT * FROM tb1 WHERE id=100\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ref
possible_keys: id
          key: id
      key_len: 4
          ref: const
         rows: 1
     filtered: 100.00
        Extra: NULL
```

O novo EXPLICATIVO mostra que o MySQL utiliza agora um índice para limitar o número de linhas a 1, o que por sua vez significativamente shortened a hora de pesquisa.
 
## <a name="covering-index"></a>Abrangendo o índice
Um índice abrangente é constituído por todas as colunas de uma consulta no índice para reduzir a obtenção de valor de tabelas de dados. Eis uma ilustração a seguir **GROUP BY** instrução.
 
```sql
mysql> EXPLAIN SELECT MAX(c1), c2 FROM tb1 WHERE c2 LIKE '%100' GROUP BY c1\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 995789
     filtered: 11.11
        Extra: Using where; Using temporary; Using filesort
```

Como podem ser vistos a partir da saída, MySQL não utiliza índices porque nenhuma índices adequados estão disponíveis. Também mostra *utilizando temporário; Utilizar a ordenação de ficheiro*, que significa que o MySQL cria uma tabela temporária para satisfazer o **GROUP BY** cláusula.
 
Criar um índice numa coluna **c2** torna individualmente nenhuma diferença e o MySQL ainda tem de criar uma tabela temporária:

```sql 
mysql> ALTER TABLE tb1 ADD KEY (c2);
mysql> EXPLAIN SELECT MAX(c1), c2 FROM tb1 WHERE c2 LIKE '%100' GROUP BY c1\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 995789
     filtered: 11.11
        Extra: Using where; Using temporary; Using filesort
```

Neste caso, um **índice coberto** no **c1** e **c2** podem ser criadas whereby adicionar o valor de **c2**"diretamente no índice para eliminar ainda mais a pesquisa de dados.

```sql 
mysql> ALTER TABLE tb1 ADD KEY covered(c1,c2);
mysql> EXPLAIN SELECT MAX(c1), c2 FROM tb1 WHERE c2 LIKE '%100' GROUP BY c1\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: index
possible_keys: covered
          key: covered
      key_len: 108
          ref: NULL
         rows: 995789
     filtered: 11.11
        Extra: Using where; Using index
```

Como mostra a EXPLICAR acima, MySQL utiliza o índice coberto agora e evitar a criação de uma tabela temporária. 

## <a name="combined-index"></a>Índice combinado
Um índice combinado consiste em valores de várias colunas e pode ser considerado uma matriz de linhas que são ordenados por concatenar valores das colunas indexadas. Este método pode ser útil para um **GROUP BY** instrução.

```sql
mysql> EXPLAIN SELECT c1, c2 from tb1 WHERE c2 LIKE '%100' ORDER BY c1 DESC LIMIT 10\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 995789
     filtered: 11.11
        Extra: Using where; Using filesort
```

MySQL efetua uma *ordenação ficheiro* operação é bastante mais lenta, especialmente quando tiver que ordenar o número de linhas. Para otimizar a esta consulta, pode ser criar um índice combinado em ambas as colunas que estão a ser ordenadas.

```sql 
mysql> ALTER TABLE tb1 ADD KEY my_sort2 (c1, c2);
mysql> EXPLAIN SELECT c1, c2 from tb1 WHERE c2 LIKE '%100' ORDER BY c1 DESC LIMIT 10\G
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: tb1
   partitions: NULL
         type: index
possible_keys: NULL
          key: my_sort2
      key_len: 108
          ref: NULL
         rows: 10
     filtered: 11.11
        Extra: Using where; Using index
```

A EXPLICAR mostra agora que MySQL é capaz de utilizar o índice combinado para evitar a ordenação adicionais, desde que o índice já esteja ordenado.
 
## <a name="conclusion"></a>Conclusão
 
Utilizar EXPLICATIVO e um tipo diferente de índices pode aumentar o desempenho significativamente. Apenas porque tem um índice em tabela não significa necessariamente que MySQL deverá conseguir utilizá-lo para as suas consultas. Validar a sua pressupostos utilizando EXPLICATIVO e otimizar as suas consultas com índices sempre.


## <a name="next-steps"></a>Passos Seguintes
- Para encontrar respostas de ponto a ponto para as perguntas mais pretendidas ou após uma nova pergunta/resposta, visite [fórum MSDN](https://social.msdn.microsoft.com/forums/security/en-US/home?forum=AzureDatabaseforMySQL) ou [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-database-mysql).
