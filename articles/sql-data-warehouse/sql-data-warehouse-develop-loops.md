---
title: Usando loops de T-SQL no Azure SQL Data Warehouse | Documentos da Microsoft
description: Dicas para o uso de loops de T-SQL e substituindo cursores no armazém de dados SQL do Azure para o desenvolvimento de soluções.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: b7c21566916c9728900e69dc6480098fadae7622
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43301213"
---
# <a name="using-t-sql-loops-in-sql-data-warehouse"></a>Usando loops de T-SQL no SQL Data Warehouse
Dicas para o uso de loops de T-SQL e substituindo cursores no armazém de dados SQL do Azure para o desenvolvimento de soluções.

## <a name="purpose-of-while-loops"></a>Finalidade de loops WHILE

SQL Data Warehouse suporta o [enquanto](/sql/t-sql/language-elements/while-transact-sql) loop para repetidamente executar blocos de instrução. Esse loop WHILE continua para, desde que as condições especificadas são true ou até que o código especificamente termina o loop usando a palavra-chave BREAK. Loops são úteis para substituir os cursores definidas no código SQL. Felizmente, cursores quase todos os que são escritos em código SQL são da gama progressiva rápidos, só de leitura. Por conseguinte, [enquanto] loops são uma ótima alternativa para substituir os cursores.

## <a name="replacing-cursors-in-sql-data-warehouse"></a>Substituição de cursores no SQL Data Warehouse
No entanto, antes de mergulhar na memória, primeiro deve se perguntar a seguinte pergunta: "deste cursor poderia ser reescrito a utilizar operações baseadas em conjunto?." Em muitos casos, a resposta é Sim e, muitas vezes, é a melhor abordagem. Muitas vezes, uma operação baseada em conjunto, executa mais rapidamente do que uma abordagem iterativa, linha por linha.

Cursores de somente avanço rápido podem ser facilmente substituídos com uvozuje konstruktor cyklu. Segue-se um exemplo simples. Este exemplo de código atualiza as estatísticas de cada tabela na base de dados. Iterando sobre as tabelas no loop, cada comando é executado na sequência.

Primeiro, crie uma tabela temporária que contém um número de linha exclusivo utilizado para identificar as instruções individuais:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Em segundo lugar, inicialize as variáveis necessárias para executar o loop:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Agora um loop através de instruções, executando-os um de cada vez:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Por fim remover a tabela temporária criada no primeiro passo

```
DROP TABLE #tbl;
```

## <a name="next-steps"></a>Passos Seguintes
Para obter mais sugestões de desenvolvimento, consulte [descrição geral do desenvolvimento](sql-data-warehouse-overview-develop.md).

