---
title: "Conceber de orientações para tabelas distribuídas - Azure SQL Data Warehouse | Microsoft Docs"
description: "Recomendações para estruturar tabelas hash distribuída e round robin no Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 01/18/2018
ms.author: barbkess
ms.openlocfilehash: 692d92f2e45e04a4eb284b43797b5b468cd9ec1b
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="guidance-for-designing-distributed-tables-in-azure-sql-data-warehouse"></a>Orientações para estruturar tabelas distribuídas no Azure SQL Data Warehouse

Este artigo fornece recomendações para estruturar tabelas distribuídas no Azure SQL Data Warehouse. As tabelas hash distribuída melhorar o desempenho das consultas em tabelas de factos grande e são o foco deste artigo. Round robin tabelas são úteis para melhorar a velocidade de carregamento. Estas opções de estrutura de ter um impacto significativo em melhorar a consulta e carregar o desempenho.

## <a name="prerequisites"></a>Pré-requisitos
Este artigo pressupõe que está familiarizado com a distribuição de dados e conceitos de movimento de dados no armazém de dados do SQL Server.  Para obter mais informações, consulte o [arquitetura](massively-parallel-processing-mpp-architecture.md) artigo. 

Como parte da estrutura da tabela, compreenda quanto possível sobre dados e como os dados está a ser consultados.  Por exemplo, considere estas perguntas:

- Como grande é a tabela?   
- Frequência é atualizada a tabela?   
- Alguns têm tabelas de factos e de dimensões no armazém de dados?   

## <a name="what-is-a-distributed-table"></a>O que é uma tabela distribuída?
Uma tabela distribuída aparece como uma única tabela, mas as linhas, na verdade, são armazenadas em 60 distribuições. As linhas são distribuídas com um hash ou o algoritmo de round robin. 

É outra opção de armazenamento de tabela replicar uma tabela pequena em todos os nós de computação. Para obter mais informações, consulte [conceber de orientações para tabelas replicadas](design-guidance-for-replicated-tables.md). Rapidamente escolher entre três opções, consulte as tabelas de distribuídas no [descrição geral de tabelas](sql-data-warehouse-tables-overview.md). 


### <a name="hash-distributed"></a>Hash distribuído
Uma tabela hash distribuída distribui as linhas da tabela em todos os nós de computação, utilizando uma função de hash deterministas para atribuir a cada linha a um [distribuição](massively-parallel-processing-mpp-architecture.md#distributions). 

![Tabela de distribuídas](media/sql-data-warehouse-distributed-data/hash-distributed-table.png "tabela distribuídas")  

Uma vez que os valores idênticos sempre hash para a distribuição do mesma, o armazém de dados tem incorporado conhecimento das localizações de linha. O SQL Data Warehouse utiliza estes conhecimentos para minimizar o movimento de dados durante consultas, que melhora o desempenho de consulta. 

As tabelas hash distribuída funcionam bem para tabelas de factos grande de um esquema em estrela. Podem ter um grande número de linhas e ainda alcançar elevado desempenho. Obviamente, existem algumas considerações de design que ajudam a obter o desempenho que do sistema distribuído foi concebido para fornecer. Escolher uma coluna de boa distribuição é um desse consideração descrita neste artigo. 

Considere a utilização de um distribuído de hash de tabela quando:

- O tamanho de tabela no disco é mais de 2 GB.
- A tabela tem frequente inserir, atualizar e eliminar operações. 

### <a name="round-robin-distributed"></a>Round robin distribuído
Uma tabela distribuída round robin distribui as linhas da tabela uniformemente por todas as distribuições. A atribuição de linhas para as distribuições é aleatória. Ao contrário das tabelas hash distribuída, linhas com valores iguais não são garantidas que sejam atribuídos a distribuição da mesma. 

Como resultado, o sistema, por vezes, tem de invocar uma operação de movimento de dados para organizar melhor os dados antes de pode resolver uma consulta.  Este passo adicional pode atrasar as suas consultas. Por exemplo, associar uma tabela de round robin, normalmente, requer reshuffling as linhas, que é um acessos de desempenho.

Considere utilizar a distribuição de round robin para a sua tabela nos seguintes cenários:

- Quando introdução como um ponto de partida simple, uma vez que é a predefinição
- Se não houver nenhuma chave joining óbvios
- Se não houver coluna bom candidato para distribuir a tabela de hash
- Se a tabela não partilha uma chave de associação comuns com outras tabelas
- Se a associação é menos significativa que outros associações na consulta
- Quando a tabela é uma tabela de transição temporária

O tutorial [carregamento de dados do blob Storage do Azure](load-data-from-azure-blob-storage-using-polybase.md#load-the-data-into-your-data-warehouse) fornece um exemplo de carregamento de dados para uma tabela de testes de round robin.


## <a name="choosing-a-distribution-column"></a>Escolher uma coluna de distribuição
Uma tabela hash distribuída tem uma coluna de distribuição que é a chave de hash. Por exemplo, o código seguinte cria uma tabela hash distribuída com ProductKey como a coluna de distribuição.

```SQL
CREATE TABLE [dbo].[FactInternetSales]
(   [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,  DISTRIBUTION = HASH([ProductKey])
)
;
``` 

Uma coluna de distribuição é uma decisão de design importantes, uma vez que os valores desta coluna determinam a forma como as linhas são distribuídas. A melhor opção depende de vários fatores e, normalmente, envolve fala. No entanto, se não escolher a melhor coluna pela primeira vez, pode utilizar [criar tabela AS SELECIONE (CTAS)](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) recriar a tabela com uma coluna de distribuição diferente. 

### <a name="choose-a-distribution-column-that-does-not-require-updates"></a>Escolha uma coluna de distribuição que não necessitem de atualizações
Não é possível atualizar uma coluna de distribuição, a menos que a eliminar a linha e inserir uma nova linha pelos valores atualizados. Por conseguinte, selecione uma coluna com valores estáticos. 

### <a name="choose-a-distribution-column-with-data-that-distributes-evenly"></a>Escolha uma coluna de distribuição com dados que distribui equitativamente

Para melhor desempenho, todas as distribuições devem ter aproximadamente o mesmo número de linhas. Quando um ou mais distribuições têm um número desproporcionado de linhas, algumas distribuições concluir a respetiva parte de uma consulta paralela antes de outros utilizadores. Uma vez que a consulta não é possível concluir até que todas as distribuições tem concluído o processamento, cada consulta apenas é tão rápida como a distribuição mais lentas.

- O desfasamento de dados, significa que os dados não são distribuídos uniformemente entre as distribuições
- Processamento dissimetrias significa que algumas distribuições demorar mais do que outras pessoas quando executar consultas paralelas. Isto pode acontecer quando é skewed os dados.
  
Para balancear o processamento paralelo, selecione uma coluna de distribuição que:

- **Tem vários valores exclusivos.** A coluna pode ter alguns valores duplicados. No entanto, todas as linhas com o mesmo valor são atribuídas a distribuição do mesma. Uma vez que existem 60 distribuições, a coluna deve ter, pelo menos, 60 valores exclusivos.  Normalmente, o número de valores exclusivos é muito maior.
- **Não tem valores NULL ou tem apenas de alguns valores nulos.** Para obter um exemplo extremos, se todos os valores na coluna são NULL, todas as linhas são atribuídas a distribuição do mesma. Como resultado, o processamento da consulta é skewed a distribuição de uma e não beneficiar de processamento paralelo. 
- **Não é uma coluna de data**. Todos os dados para a mesma data seja colocado no sítio a distribuição do mesma. Se vários utilizadores são filtragem na mesma data, só 1 as 60 distribuições fazer todo o trabalho de processamento. 

### <a name="choose-a-distribution-column-that-minimizes-data-movement"></a>Escolha uma coluna de distribuição que minimiza o movimento de dados

Para obter as consultas de resultado de consulta correta pode mover dados de um nó de computação para outro. Movimento de dados ocorre normalmente quando consultas ter associações e agregações em tabelas distribuídas. Escolher uma coluna de distribuição que ajuda a minimizar o movimento de dados é um das estratégias mais importantes para otimizar o desempenho do seu SQL Data Warehouse.

Para minimizar o movimento de dados, selecione uma coluna de distribuição que:

- É utilizado em `JOIN`, `GROUP BY`, `DISTINCT`, `OVER`, e `HAVING` cláusulas. Quando duas tabelas de factos grande tem associações frequentes, melhora o desempenho das consultas, quando distribuir ambas as tabelas das colunas de associação.  Quando uma tabela não é utilizada em associações, considere a distribuição de uma coluna que está a ser frequentemente a tabela a `GROUP BY` cláusula.
- É *não* utilizado no `WHERE` cláusulas. Isto pode limitar a consulta para não executar em todas as distribuições. 
- É *não* uma coluna de data. Cláusulas WHERE, muitas vezes, filtrar por data.  Quando isto acontecer, todos os o processamento conseguiu executar em apenas alguns distribuições.

### <a name="what-to-do-when-none-of-the-columns-are-a-good-distribution-column"></a>O que fazer quando nenhuma das colunas é uma coluna de distribuição boa

Quando não existe nenhuma coluna bom candidato, em seguida, considere utilizar o round robin como método de distribuição.

Depois de criar uma tabela hash distribuída, o passo seguinte é carregar dados para a tabela.  Para carregar orientações, consulte [carregamento](sql-data-warehouse-overview-load.md). 

## <a name="how-to-tell-if-your-distribution-column-is-a-good-choice"></a>Como saber se a coluna de distribuição é uma boa opção
Depois dos dados são carregados para uma tabela hash distribuída, certifique-se como as linhas são distribuídas uniformemente entre as distribuições de 60. As linhas por distribuição podem variar até 10% sem um impacto evidente no desempenho. 

### <a name="determine-if-the-table-has-data-skew"></a>Determinar se a tabela contém dados desfasamento
Uma forma rápida de verificar de desfasamento de dados consiste em utilizar [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql). O seguinte código do SQL Server devolve o número de linhas de tabela que estão armazenados em cada um das distribuições de 60. Para um desempenho equilibrado, as linhas na tabela distribuída devem ser distribuídas uniformemente por todas as distribuições.

```sql
-- Find data skew for a distributed table
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Para identificar que tabelas tem mais do que o desfasamento de 10% de dados:

1. Criar a vista dbo.vTableSizes que é apresentado no [descrição geral de tabelas](sql-data-warehouse-tables-overview.md#table-size-queries) artigo.  
2. Execute a seguinte consulta:

```sql
select *
from dbo.vTableSizes
where two_part_name in
    (
    select two_part_name
    from dbo.vTableSizes
    where row_count > 0
    group by two_part_name
    having min(row_count * 1.000)/max(row_count * 1.000) > .10
    )
order by two_part_name, row_count
;
```

### <a name="check-query-plans-for-data-movement"></a>Verifique os planos de consulta para o movimento de dados
Permite que uma coluna de distribuição boa associações e agregações para que o movimento de dados mínimo. Este procedimento afeta a forma como devem ser escritas associações. Para obter o movimento de dados mínima para uma associação em duas tabelas hash distribuída, uma das colunas de associação tem de ser a coluna de distribuição.  Quando duas tabelas hash distribuída associa uma coluna de distribuição do mesmo tipo de dados, a associação não necessita de movimento de dados. Associações podem utilizar as colunas adicionais sem incorrer em movimento de dados.

Para evitar o movimento de dados durante uma associação:

- As tabelas envolvidos na União tem de ser hash distribuído **um** das colunas participar na União.
- Os tipos de dados das colunas de associação tem de corresponder entre ambas as tabelas.
- As colunas tem de ser associadas com um operador igual a.
- O tipo de associação não pode ser um `CROSS JOIN`.

Para ver se as consultas estão a experienciar movimento de dados, pode examinar o plano de consulta.  


## <a name="resolve-a-distribution-column-problem"></a>Resolver um problema de coluna de distribuição
Não é necessário resolver o que desfasamento de todos os casos de dados. Distribuição de dados é um fim de encontrar o equilíbrio certo entre minimizando dados dissimetrias e o movimento de dados. Não sempre é possível minimizar o desfasamento de dados e o movimento de dados. Por vezes, a vantagem de ter o movimento de dados mínimo poderá compensar o impacto da eliminação de dados desfasamento.

Para decidir se deve resolver dados dissimetrias numa tabela, deve compreender quanto possível sobre os volumes de dados e consultas na sua carga de trabalho. Pode utilizar os passos a [consulta monitorização](sql-data-warehouse-manage-monitor.md) artigo para monitorizar o impacto da dissimetrias no desempenho das consultas. Especificamente, procure o período de tempo que demora consultas grandes em distribuições individuais.

Uma vez que não é possível alterar a coluna de distribuição numa tabela existente, a forma típica para resolver o desfasamento de dados é recriar a tabela com uma coluna de distribuição diferente.  

### <a name="re-create-the-table-with-a-new-distribution-column"></a>Voltar a criar a tabela com uma nova coluna de distribuição
Este exemplo utiliza [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md) voltar a criar uma tabela com uma coluna de distribuição hash diferentes.

```sql
CREATE TABLE [dbo].[FactInternetSales_CustomerKey]
WITH (  CLUSTERED COLUMNSTORE INDEX
     ,  DISTRIBUTION =  HASH([CustomerKey])
     ,  PARTITION       ( [OrderDateKey] RANGE RIGHT FOR VALUES (   20000101, 20010101, 20020101, 20030101
                                                                ,   20040101, 20050101, 20060101, 20070101
                                                                ,   20080101, 20090101, 20100101, 20110101
                                                                ,   20120101, 20130101, 20140101, 20150101
                                                                ,   20160101, 20170101, 20180101, 20190101
                                                                ,   20200101, 20210101, 20220101, 20230101
                                                                ,   20240101, 20250101, 20260101, 20270101
                                                                ,   20280101, 20290101
                                                                )
                        )
    )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
OPTION  (LABEL  = 'CTAS : FactInternetSales_CustomerKey')
;

--Create statistics on new table
CREATE STATISTICS [ProductKey] ON [FactInternetSales_CustomerKey] ([ProductKey]);
CREATE STATISTICS [OrderDateKey] ON [FactInternetSales_CustomerKey] ([OrderDateKey]);
CREATE STATISTICS [CustomerKey] ON [FactInternetSales_CustomerKey] ([CustomerKey]);
CREATE STATISTICS [PromotionKey] ON [FactInternetSales_CustomerKey] ([PromotionKey]);
CREATE STATISTICS [SalesOrderNumber] ON [FactInternetSales_CustomerKey] ([SalesOrderNumber]);
CREATE STATISTICS [OrderQuantity] ON [FactInternetSales_CustomerKey] ([OrderQuantity]);
CREATE STATISTICS [UnitPrice] ON [FactInternetSales_CustomerKey] ([UnitPrice]);
CREATE STATISTICS [SalesAmount] ON [FactInternetSales_CustomerKey] ([SalesAmount]);

--Rename the tables
RENAME OBJECT [dbo].[FactInternetSales] TO [FactInternetSales_ProductKey];
RENAME OBJECT [dbo].[FactInternetSales_CustomerKey] TO [FactInternetSales];
```

## <a name="next-steps"></a>Passos Seguintes

Para criar uma tabela distribuída, utilize um destas instruções:

- [Criar tabela (armazém de dados SQL do Azure)](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse)
- [Criar TABLE AS SELECT (Azure SQL Data Warehouse](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)


