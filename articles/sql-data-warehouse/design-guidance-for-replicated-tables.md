---
title: Conceber de orientações para tabelas replicadas - Azure SQL Data Warehouse | Microsoft Docs
description: Recomendações para estruturar replicadas tabelas no seu esquema de Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/23/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 1cc796061056ff017e3d778ebb2e50e13d55a4c1
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="design-guidance-for-using-replicated-tables-in-azure-sql-data-warehouse"></a>Conceber de orientações para a utilização de tabelas replicadas no Azure SQL Data Warehouse
Este artigo fornece recomendações para estruturar tabelas replicadas no seu esquema de armazém de dados do SQL Server. Utilize estas recomendações para melhorar o desempenho de consulta ao reduzir a complexidade de movimento e a consulta de dados.

## <a name="prerequisites"></a>Pré-requisitos
Este artigo pressupõe que está familiarizado com a distribuição de dados e conceitos de movimento de dados no armazém de dados do SQL Server.  Para obter mais informações, consulte o [arquitetura](massively-parallel-processing-mpp-architecture.md) artigo. 

Como parte da estrutura da tabela, compreenda quanto possível sobre dados e como os dados está a ser consultados.  Por exemplo, considere estas perguntas:

- Como grande é a tabela?   
- Frequência é atualizada a tabela?   
- Alguns têm tabelas de factos e de dimensões no armazém de dados?   

## <a name="what-is-a-replicated-table"></a>O que é uma tabela não replicada?
Uma tabela não replicada tem uma cópia completa da tabela acessível em cada nó de computação. Uma tabela a ser replicada remove a necessidade para transferir dados entre os nós de computação antes de uma associação ou agregação. Uma vez que a tabela tem várias cópias, tabelas replicadas funcionam melhor quando o tamanho de tabela é inferior a 2 GB comprimido.

O diagrama seguinte mostra uma tabela não replicada que seja acessível em cada nó de computação. No SQL Data Warehouse, a tabela não replicada totalmente é copiada para uma base de dados de distribuição em cada nó de computação. 

![Tabela de replicados](media/guidance-for-using-replicated-tables/replicated-table.png "replicados tabela")  

Replicadas trabalho tabelas bem para tabelas de pequena dimensão de um esquema em estrela. As tabelas de dimensões são, normalmente, um tamanho que torna viável para armazenar e manter várias cópias. As dimensões armazenam dados descritivos que altera lentamente, tais como o nome do cliente e o endereço e detalhes do produto. A natureza lentamente a alteração dos dados leva a menos Reconstrói da tabela replicada. 

Considere a utilização de um replicadas tabela quando:

- O tamanho de tabela no disco é inferior a 2 GB, independentemente do número de linhas. Para localizar o tamanho de uma tabela, pode utilizar o [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql) comando: `DBCC PDW_SHOWSPACEUSED('ReplTableCandidate')`. 
- A tabela for utilizada nas associações que necessitem, caso contrário, movimento de dados. Ao associar as tabelas que não forem distribuídas na mesma coluna, tais como uma tabela hash distribuída a uma tabela de round robin, movimento de dados é necessário para concluir a consulta.  Se uma das tabelas for pequena, considere uma tabela não replicada. Recomendamos que utilize tabelas replicadas em vez de tabelas de round robin na maioria dos casos. Para ver as operações de movimento de dados nos planos de consulta, utilize [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql).  O BroadcastMoveOperation é a operação de movimento de dados típicas que pode ser eliminada utilizando uma tabela não replicada.  
 
As tabelas replicadas não poderão produzir o melhor desempenho de consulta quando:

- A tabela tem frequente inserir, atualizar e eliminar operações. Estas operações de idioma (DML) de manipulação de dados exigem uma reconstrução da tabela replicada. A reconstrução com frequência pode causar um desempenho mais lento.
- O armazém de dados é dimensionado com frequência. Dimensionamento de um armazém de dados é alterado o número de nós de computação, o que implica uma reconstrução.
- A tabela tem um grande número de colunas, mas as operações de dados aceder, normalmente, apenas um pequeno número de colunas. Neste cenário, em vez de replicar a tabela inteira, poderá ser mais eficiente para distribuir a tabela e, em seguida, criar um índice de colunas acedidas com frequência. Quando uma consulta requer o movimento de dados, o SQL Data Warehouse apenas os dados para as colunas pedidas. 

## <a name="use-replicated-tables-with-simple-query-predicates"></a>Utilizar tabelas replicadas com predicados de consulta simples
Antes de optar por distribuir ou replicar uma tabela, pensa sobre os tipos de consultas que pretende executar na tabela. Sempre que possível,

- Utilize tabelas replicadas para consultas com predicados de consulta simples, tais como igualdade ou inequality.
- Utilize tabelas distribuídas para consultas com predicados de consulta complexas, tais como semelhante ou como não.

Consultas de intensivas em CPU melhor efetuar quando o trabalho é distribuído por todos os nós de computação. Por exemplo, as consultas que executam cálculos em cada linha de uma tabela efetuar melhor em tabelas distribuídas que tabelas replicadas. Uma vez que uma tabela não replicada é armazenada no completo em cada nó de computação, uma consulta de CPU intensivas em relação a uma tabela não replicada é executada relativamente a toda a tabela em cada nó de computação. A computação adicional pode abrandar o desempenho das consultas.

Por exemplo, esta consulta tem um predicado complexo.  É executada mais rapidamente quando o fornecedor é uma tabela distribuída em vez de uma tabela não replicada. Neste exemplo, o fornecedor pode ser distribuído round robin.

```sql

SELECT EnglishProductName 
FROM DimProduct 
WHERE EnglishDescription LIKE '%frame%comfortable%'

```

## <a name="convert-existing-round-robin-tables-to-replicated-tables"></a>Converter tabelas round robin existentes em tabelas replicadas
Se já tiver tabelas round robin, recomendamos a convertê-los para tabelas replicadas se cumprem com os critérios descritos neste artigo. As tabelas replicadas melhoram o desempenho através de tabelas de round robin porque se eliminar a necessidade de movimento de dados.  Uma tabela de round robin requer sempre movimento de dados de associações. 

Este exemplo utiliza [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) para alterar a tabela de DimSalesTerritory para uma tabela não replicada. Neste exemplo funciona independentemente se DimSalesTerritory é distribuído de hash ou round robin.

```sql
CREATE TABLE [dbo].[DimSalesTerritory_REPLICATE]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]
OPTION  (LABEL  = 'CTAS : DimSalesTerritory_REPLICATE') 

--Create statistics on new table
CREATE STATISTICS [SalesTerritoryKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryKey]);
CREATE STATISTICS [SalesTerritoryAlternateKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryAlternateKey]);
CREATE STATISTICS [SalesTerritoryRegion] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryRegion]);
CREATE STATISTICS [SalesTerritoryCountry] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryCountry]);
CREATE STATISTICS [SalesTerritoryGroup] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryGroup]);

-- Switch table names
RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[DimSalesTerritory_REPLICATE] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```  

### <a name="query-performance-example-for-round-robin-versus-replicated"></a>Exemplo de desempenho de consulta para round-robin versus replicado 

Uma tabela não replicada não requer qualquer movimento de dados para associações porque a tabela inteira já está presente em cada nó de computação. Se as tabelas de dimensão são distribuído round robin, uma associação copia a tabela de dimensão de completa para cada nó de computação. Para mover os dados, o plano de consulta contém uma operação chamada BroadcastMoveOperation. Este tipo de operação de movimento de dados abrandar o desempenho das consultas e é eliminado utilizando tabelas replicadas. Para ver os passos do plano de consulta, utilize o [sys.dm_pdw_request_steps](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql) vista de catálogo de sistema. 

Por exemplo, na seguinte consulta no esquema AdventureWorks, o ` FactInternetSales` tabela é distribuído de hash. O `DimDate` e `DimSalesTerritory` as tabelas são tabelas de dimensão mais pequena. Esta consulta devolve as totais vendas na América do Norte para ano fiscal 2004:
 
```sql
SELECT [TotalSalesAmount] = SUM(SalesAmount)
FROM dbo.FactInternetSales s
INNER JOIN dbo.DimDate d
  ON d.DateKey = s.OrderDateKey
INNER JOIN dbo.DimSalesTerritory t
  ON t.SalesTerritoryKey = s.SalesTerritoryKey
WHERE d.FiscalYear = 2004
  AND t.SalesTerritoryGroup = 'North America'
```
Criámos novamente `DimDate` e `DimSalesTerritory` como tabelas de round robin. Como resultado, a consulta mostrou o plano de consulta seguinte, que tem vários difusão move operações: 
 
![Plano de consulta de round robin](media/design-guidance-for-replicated-tables/round-robin-tables-query-plan.jpg) 

Criámos novamente `DimDate` e `DimSalesTerritory` como tabelas replicadas e foi executada a consulta novamente. O plano de consulta resultante é muito mais curto e não tem qualquer difusão move.

![Replicado plano de consulta](media/design-guidance-for-replicated-tables/replicated-tables-query-plan.jpg) 


## <a name="performance-considerations-for-modifying-replicated-tables"></a>Considerações de desempenho para modificar as tabelas replicadas
O SQL Data Warehouse implementa uma tabela não replicada, mantendo uma versão principal da tabela. Copia a versão principal para uma base de dados de distribuição em cada nó de computação. Quando existe uma alteração, o SQL Data Warehouse atualiza primeiro a tabela principal. Em seguida, ela recria as tabelas em cada nó de computação. Inclui uma reconstrução de uma tabela replicada copiar a tabela para cada nó de computação e, em seguida, criar os índices.  Por exemplo, uma tabela não replicada num DW400 tem 5 cópias dos dados.  Uma cópia principal e uma cópia completa em cada nó de computação.  Todos os dados são armazenados nas bases de dados de distribuição. SQL Data Warehouse utiliza este modelo para suportar as operações de dimensionamento flexíveis e instruções de modificação de dados mais rápidas. 

Recria é necessário após:
- Os dados são carregados ou modificados
- O armazém de dados é dimensionado para um nível diferente
- Definição da tabela está atualizada

Recria não é necessário após:
- Operação de colocação em pausa
- A operação de retoma

A reconstrução não ocorrer imediatamente depois dos dados são modificados. Em vez disso, a reconstrução é acionada pela primeira vez, que seleciona uma consulta da tabela.  A consulta que acionou a reconstrução lê imediatamente da versão principal da tabela, enquanto os dados de forma assíncrona são copiados para cada nó de computação. Até que a cópia de dados estiver concluída, consultas subsequentes vão continuar a utilizar a versão principal da tabela.  Se qualquer atividade acontecer na tabela replicada que força a reconstrução outra, a cópia de dados é invalidada e a seguinte instrução select é acionado dados a ser copiado novamente. 

### <a name="use-indexes-conservatively"></a>Utilizar moderadamente índices
Práticas de indexação padrão se aplica a tabelas replicadas. O SQL Data Warehouse Reconstrói cada índice de tabela não replicada como parte da reconstrução. Utilize apenas índices quando os ganhos de desempenho prevalece sobre o custo de reconstruir os índices.  
 
### <a name="batch-data-loads"></a>Cargas de dados do batch
Quando o carregamento de dados em tabelas replicadas, tente minimizar Reconstrói através da criação de batches cargas em conjunto. Efetue todas as cargas de batches antes de executar instruções.

Por exemplo, neste padrão de carga carrega dados a partir de origens de quatro e invoca quatro Reconstrói. 

- Carregar a partir da origem de 1.
- Acionadores de instrução SELECT reconstruir 1.
- Carregar a partir da origem 2.
- Acionadores de instrução SELECT reconstruir 2.
- Carregar a partir da origem de 3.
- Acionadores de instrução SELECT reconstruir 3.
- Carregar a partir da origem de 4.
- Acionadores de instrução SELECT reconstruir 4.

Por exemplo, neste padrão de carga carrega dados a partir de origens de quatro, mas apenas invoca uma reconstrução.

- Carregar a partir da origem de 1.
- Carregar a partir da origem 2.
- Carregar a partir da origem de 3.
- Carregar a partir da origem de 4.
- Reconstrua a acionadores de instrução SELECT.


### <a name="rebuild-a-replicated-table-after-a-batch-load"></a>Reconstruir uma tabela replicada após uma carga de batch
Para garantir que os tempos de execução de consulta consistente, considere forçar a compilação das tabelas replicadas após uma carga de batch. Caso contrário, a primeira consulta irá continuar a utilizar o movimento de dados para concluir a consulta. 

Esta consulta utiliza a [sys.pdw_replicated_table_cache_state](/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql) DMV para listar as tabelas replicadas que modificada, mas não reconstruídas.

```sql 
SELECT [ReplicatedTable] = t.[name]
  FROM sys.tables t  
  JOIN sys.pdw_replicated_table_cache_state c  
    ON c.object_id = t.object_id 
  JOIN sys.pdw_table_distribution_properties p 
    ON p.object_id = t.object_id 
  WHERE c.[state] = 'NotReady'
    AND p.[distribution_policy_desc] = 'REPLICATE'
```
 
Para acionar uma reconstrução, execute a seguinte instrução em cada tabela na saída anterior. 

```sql
SELECT TOP 1 * FROM [ReplicatedTable]
``` 
 
## <a name="next-steps"></a>Passos Seguintes 
Para criar uma tabela não replicada, utilize um destas instruções:

- [Criar tabela (armazém de dados SQL do Azure)](/sql/t-sql/statements/create-table-azure-sql-data-warehouse)
- [Criar TABLE AS SELECT (armazém de dados SQL do Azure)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)

Para obter uma descrição geral das tabelas distribuídas, consulte [distribuídas tabelas](sql-data-warehouse-tables-distribute.md).



