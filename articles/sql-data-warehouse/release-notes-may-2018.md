---
title: Notas de versão de armazém de dados SQL do Azure, pode de 2018 | Microsoft Docs
description: Notas de versão para o Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 05/28/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 843621a8f6e08b2b51d4b7abd05d0ae6c3393fe1
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726038"
---
# <a name="whats-new-in-azure-sql-data-warehouse-may-2018"></a>Novidades no Azure SQL Data Warehouse (Maio de 2018)?
O Azure SQL Data Warehouse recebe melhoramentos continuamente. Este artigo descreve as novas funcionalidades e as alterações que foram introduzidas na Maio de 2018. 

## <a name="gen-2-instances"></a>Instâncias de geração 2
![ALT](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/2528b41b-f09f-45b1-aa65-fc60d562d3bd.png) Azure SQL Data Warehouse computação otimizada Gen2 camada define novos padrões de desempenho para o armazenamento de dados de nuvem. Os clientes obter agora até cinco vezes um melhor desempenho das consultas, quatro vezes mais simultaneidade e cinco vezes energia informático superior em comparação com a geração atual. Agora que possa servir 128 consultas em simultâneo a partir de um único cluster, o mais elevado de qualquer serviço warehousing de dados em nuvem.

Consulte o [Turbocharge análise de nuvem com o Azure SQL Data Warehouse](https://azure.microsoft.com/blog/turbocharge-cloud-analytics-with-azure-sql-data-warehouse/) blogue anúncio Rohan Kumar, empresarial vice-versa President, Azure dados.

## <a name="features"></a>Funcionalidades

### <a name="auto-statistics"></a>Estatísticas de automática
As estatísticas são críticas para otimizar a geração do plano de consulta no moderl baseada no custo otimizadores, tais como o motor do armazém de dados do SQL Server. Quando todas as consultas são conhecidas seguinte com antecedência, determinar quais as estatísticas objetos têm de ser criado é uma tarefa alcançável. No entanto, quando o sistema é confrontado com ad-hoc e consultas aleatórias que é comum para os cargas de trabalho do armazém de dados, os administradores de sistema podem dificuldade prever o estatísticas precisam de ser criado à esquerda para planos de execução da consulta potencialmente inferior ao ideal e tempos de resposta de consulta mais. Uma forma de atenuar este problema é criar objetos de estatísticas em todas as colunas da tabela seguinte com antecedência. No entanto, esse processo inclui uma penalidade como objetos de estatísticas têm de ser mantidos durante a tabela a carregar o processo, fazendo com que os tempos de carregamento mais.

Armazém de dados do SQL Server suporta agora a criação automática de objetos de estatísticas fornecer maior produtividade, flexibilidade e facilidade de utilização para os administradores de sistema e os programadores, garantindo que o sistema continua a oferecer planos de execução de qualidade e melhor tempos de resposta.

Para ativar ou desativar a criação automática de estatísticas no armazém de dados do SQL Server, execute a seguinte instrução:
```sql
ALTER DATABASE { database_name } SET { AUTO_CREATE_STATISTICS { OFF | ON } } [;]
```

Como uma melhor prática e orientações, recomendamos que a definição `AUTO_CREATE_STATISTICS` opção para `ON`.

> [!NOTE]
> Criação automática de estatística é *ativada por predefinição* para todos os armazéns de dados de novo.
>  

Consulte o [opções definido de ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options) artigo para obter detalhes adicionais.

### <a name="rejected-row-support"></a>Rejeitou o suporte de linha
Os clientes utilizam, frequentemente, [PolyBase (tabelas externas) para carregar dados](design-elt-data-loading.md) para o SQL Data Warehouse devido ao elevado desempenho, paralela natureza do carregamento de dados. O PolyBase é o modelo de carregamento predefinido quando carregar os dados através de [do Azure Data Factory](http://azure.com/adf) bem. 

SQL Data Warehouse adiciona a capacidade de definir uma localização de linha rejeitadas através de `REJECTED_ROW_LOCATION` parâmetro com o [criar tabela externa](https://docs.microsoft.com/sql/t-sql/statements/create-external-table-transact-sql) instrução. Após a execução de um [criar tabela AS SELECIONE (CTAS)](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) da tabela externa, quaisquer linhas que não foi possível carregar serão armazenadas num ficheiro quase a origem para investigação adicional. 

Consulte o [carregar confidencialidade com SQL Server do armazém PolyBase rejeitado linha a localização de dados](https://azure.microsoft.com/blog/load-confidently-with-sql-data-warehouse-polybase-rejected-row-location/) blogue para obter mais detalhes sobre o comportamento de linha de rejeitado.

O exemplo seguinte mostra a sintaxe de novo para especificar rejeitado linhas.

```sql
CREATE EXTERNAL TABLE [dbo].[Reject_Example]
(
    ...
)
WITH
(
    ...
    ,REJECTED_ROW_LOCATION=‘/Reject_Directory'
)
```

### <a name="alter-view"></a>ALTERAR A VISTA
[Vista de ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-view-transact-sql) permite ao utilizador modificar uma vista criada anteriormente sem ter de eliminar/cria a vista e volte a aplicar as permissões. 

O exemplo seguinte modifica uma vista criada anteriormente.
```sql
ALTER VIEW test_view AS SELECT 1 [data];
```

### <a name="concatws"></a>CONCAT_WS
[O CONCAT_WS()](https://docs.microsoft.com/sql/t-sql/functions/concat-ws-transact-sql) função devolve uma cadeia resultante da concatenação de dois ou mais valores de forma ponto-a-ponto. Os valores concatenados-separa com o delimitador especificado no primeiro argumento. O `CONCAT_WS` função é útil para gerar a saída de Comma-Separated. o valor (CSV).

O exemplo seguinte mostra a concatenar um conjunto de valores de int com uma vírgula.
```sql
SELECT CONCAT_WS(',', 1, 2, 3) [result];
```
A instrução devolve o resultado seguinte:
```
result
---------
1,2,3
```
O exemplo seguinte mostra a concatenar um conjunto de valores de tipo de dados mistos com uma vírgula.
```sql
SELECT CONCAT_WS(',', 1, 2, 'String', NEWID()) [result]
```
A instrução devolve o resultado seguinte:
```
result
---------
1,2,String,26E1F74D-5746-44DC-B47F-2FC1DA1B6E49
```
### <a name="spdatatypeinfo"></a>SP_DATATYPE_INFO
O [sp_datatype_info](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-datatype-info-transact-sql) sistema procedimento armazenado devolve informações sobre os tipos de dados suportados pelo ambiente atual. É frequentemente utilizado pelas ferramentas que se ligam através de ligações de ODBC para investigação de tipo de dados.

O exemplo seguinte obtém os detalhes para todos os tipos de dados suportados pelo SQL Data Warehouse.

```sql
EXEC sp_datatype_info
```

## <a name="behavior-changes"></a>Alterações de comportamento
### <a name="select-into-with-order-by"></a>SELECT INTO com ORDER BY
Agora irá bloquear o SQL Data Warehouse `SELECT INTO` consultas que contêm um `ORDER BY` cláusula. Anteriormente, esta operação iria concluída com êxito pelo primeiro ordenar os dados na memória e, em seguida, inserir a tabela de destino reordenação os dados para corresponder a forma de tabela.

#### <a name="previous-behavior"></a>Comportamento anterior
A seguinte instrução seria concluída com êxito com sobrecarga de processamento adicional.
```sql
SELECT * INTO table2 FROM table1 ORDER BY 1;
```

#### <a name="current-behavior"></a>Comportamento atual
A seguinte instrução irá gerar um erro que indica que o `ORDER BY` cláusula não é suportada um `SELECT INTO` instrução.
```sql
SELECT * INTO table2 FROM table1 ORDER BY 1;
```
A instrução de erro devolvida:
```
Msg 104381, Level 16, State 1, Line 1
The ORDER BY clause is invalid in views, CREATE TABLE AS SELECT, INSERT SELECT, SELECT INTO, inline functions, derived tables, subqueries, and common table expressions, unless TOP or FOR XML is also specified.
```

### <a name="set-parseonly-on-query-status"></a>Definir PARSEONLY no estado de consulta
Utilizar o `SET PARSEONLY ON` sintaxe permite ao utilizador para que o motor de armazém de dados do SQL Server, examine a sintaxe de cada instrução de T-SQL e devolver quaisquer mensagens de erro sem compilar ou ao executar a instrução. Anteriormente, no [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) vista de sistema, o estado para estas instruções permanecerá no `Running` estado. O `sys.dm_pdw_exec_requests` Vista agora irá devolver o estado como `Complete`.