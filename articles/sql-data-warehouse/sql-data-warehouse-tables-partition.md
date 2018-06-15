---
title: A criação de partições de tabelas do Azure SQL Data Warehouse | Microsoft Docs
description: As recomendações e os exemplos de utilização de partições de tabela no Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: ada55950ee36222e70809e2ef423c63612cd61ed
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
ms.locfileid: "31526782"
---
# <a name="partitioning-tables-in-sql-data-warehouse"></a>A criação de partições de tabelas no armazém de dados do SQL Server
As recomendações e os exemplos de utilização de partições de tabela no Azure SQL Data Warehouse.

## <a name="what-are-table-partitions"></a>Quais são as partições de tabela?
As partições de tabela permitem-lhe dividir os dados em grupos mais pequenos de dados. Na maioria dos casos, são criadas partições de tabela numa coluna de data. Criação de partições é suportada em todos os tipos de tabela do SQL Data Warehouse; incluindo columnstore em cluster, índice em cluster e área dinâmica para dados. Criação de partições também é suportada em todos os tipos de distribuição, incluindo hash ou o round robin distribuído.  

Criação de partições pode beneficiar desempenho de consulta e de manutenção de dados. Indica se beneficia ambos ou apenas um é depende da forma como os dados são carregados e se a mesma coluna pode ser utilizada para ambos os fins, desde que a criação de partições só pode ser efetuada numa coluna.

### <a name="benefits-to-loads"></a>Vantagens para cargas
A principal vantagem de criação de partições no SQL Data Warehouse é melhorar a eficiência e o desempenho de carregamento de dados através de eliminação de partição, mudar e intercalação. Na maioria dos casos dados é divididos em partições uma coluna de data que esteja ligada diretamente para a ordem na qual os dados são carregados na base de dados. Uma das vantagens da utilização de partições para manter os dados mais compridas-evitação de ciclos de registo de transações. Embora simplesmente a inserir, atualizar ou eliminar dados possam ser a abordagem mais simples, com um pequeno profundamente e esforço, a criação de partições durante o processo de carga a utilizar pode substancialmente melhorar o desempenho.

Mudar de partição pode ser utilizado para rapidamente remover ou substituir uma secção de uma tabela.  Por exemplo, uma tabela de factos vendas poderá conter dados apenas para os últimos meses 36. No final de cada mês, o mês de dados de vendas mais antigo é eliminado da tabela.  Foi possível eliminar estes dados utilizando uma instrução de eliminação para eliminar os dados para o mês mais antigo. No entanto, a eliminação de uma grande quantidade de dados linha por linha com a instrução delete pode demorar demasiado tempo, bem como criar o risco de transações de grandes dimensões que demorar muito tempo a reversão se algo não bate certo. Uma abordagem mais ideal é remova a partição mais antiga dos dados. Em que a eliminação de linhas individuais pode demorar horas, eliminar uma partição completa pode demorar algum segundos.

### <a name="benefits-to-queries"></a>Vantagens para consultas
Criação de partições também pode ser utilizada para melhorar o desempenho de consulta. Uma consulta que aplica um filtro aos dados particionados pode limitar a análise para apenas as partições de qualificação. Este método de filtragem pode evitar uma análise completa de tabela e apenas um subconjunto mais pequeno de dados de análise. Com a introdução de índices columnstore em cluster, os benefícios de desempenho de eliminação de predicado são menos úteis, mas em alguns casos pode haver uma vantagem de consultas. Por exemplo, se a tabela de factos de vendas está particionada em meses 36 utilizando o campo de data de vendas, em seguida, consulta esse filtro a data de venda pode ignorar a procurar em partições não correspondem ao filtro.

## <a name="sizing-partitions"></a>Partições de dimensionamento
Durante a criação de partições pode ser utilizada para melhorar o desempenho de alguns cenários, criar uma tabela com **demasiados** partições pacote podem prejudicar o desempenho em algumas circunstâncias.  Estas questões são particularmente verdadeiras para tabelas columnstore em cluster. Para a criação de partições para ser útil, é importante compreender quando deve utilizar a criação de partições e o número de partições para criar. Há nenhuma regra rápida rígida relativamente a partições quantas for demasiado elevado, depende de dados e o número de partições que carregar em simultâneo. Geralmente, a um esquema de partições com êxito tem dezenas para centenas de partições, não milhares.

Durante a criação de partições no **columnstore em cluster** tabelas, é importante considerar o número de linhas pertence a cada partição. Para compressão ideal e o desempenho das tabelas columnstore em cluster, é necessário um mínimo de 1 milhões de linhas por partição e de distribuição. Antes da criação de partições, o SQL Data Warehouse divide já cada tabela em 60 bases de dados distribuídas. É qualquer criar partições adicionado a uma tabela para além das distribuições criadas em segundo plano. Com este exemplo, se a tabela de factos vendas contidos 36 partições mensais e uma vez que o SQL Data Warehouse tem 60 distribuições, em seguida, as tabelas de factos vendas devem conter 60 milhões de linhas por mês ou 2.1 milhões de linhas quando todos os meses são povoados. Se uma tabela contém menos do que o número mínimo recomendado de linhas por partição, considere utilizar menos partições para aumentar o número de linhas por partição. Para obter mais informações, consulte o [Indexing](sql-data-warehouse-tables-index.md) artigo, o que inclui as consultas que podem avaliar a qualidade dos índices columnstore de cluster.

## <a name="syntax-differences-from-sql-server"></a>Diferenças de sintaxe do SQL Server
SQL Data Warehouse introduz uma forma de definir as partições que é mais simples do que o SQL Server. As funções de criação de partições e de esquemas não são utilizados no armazém de dados do SQL dado que estão no SQL Server. Em vez disso, tudo o que precisa de fazer é identificar coluna particionada e os pontos de limites. Enquanto a sintaxe de criação de partições poderão ser ligeiramente diferente do SQL Server, os conceitos básicos são iguais. SQL Server e do armazém de dados do SQL Server suportam uma partição coluna por tabela, o que pode ser ranged partição. Para saber mais sobre a criação de partições, consulte o artigo [índices e tabelas Particionadas](/sql/relational-databases/partitions/partitioned-tables-and-indexes).

O exemplo seguinte utiliza o [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse) instrução para particionar a tabela FactInternetSales na coluna OrderDateKey:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
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
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>Migrar a criação de partições do SQL Server
Para migrar as definições de partição do SQL Server para o SQL Data Warehouse simplesmente:

- Eliminar o SQL Server [esquema de partição](/sql/t-sql/statements/create-partition-scheme-transact-sql).
- Adicionar o [função de partição](/sql/t-sql/statements/create-partition-function-transact-sql) definição para a tabela de criar.

Se estiver a migrar uma tabela particionada necessitar de uma instância do SQL Server, o SQL seguinte pode ajudar a descobrir o número de linhas que em cada partição. Tenha em atenção que, se for utilizada a mesma granularidade de criação de partições no SQL Data Warehouse, o número de linhas por partição diminui por um fator de 60.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="workload-management"></a>Gestão de cargas de trabalhos
É uma consideração final para pesar a sua decisão de partição da tabela [gestão da carga de trabalho](resource-classes-for-workload-management.md). Gestão de carga de trabalho do armazém de dados do SQL Server é principalmente a gestão de memória e concorrência. No SQL Data Warehouse, o máximo de memória alocado a cada distribuição durante a execução de consulta é regido pelas classes de recursos. Idealmente, as partições são dimensionadas in consideration of outros fatores, como as necessidades de memória da criação de índices columnstore em cluster. Colocar em cluster benefício de índices columnstore significativamente quando estiverem alocados mais memória. Por conseguinte, pretender certificar-se de que uma reconstrução do índice de partição não é starved de memória. Aumentar a quantidade de memória disponível para a consulta pode ser alcançado ao mudar da função predefinida, smallrc, para uma das outras funções, tais como largerc.

Informações sobre a alocação de memória por distribuição estão disponíveis consultando as vistas de gestão dinâmica do Governador de recursos. Na realidade, a concessão de memória é inferior aos resultados da consulta seguinte. No entanto, esta consulta fornece um nível de orientação que pode utilizar ao dimensionar a sua partições para operações de gestão de dados. Tente evitar as partições para além de concessão de memória fornecido pela classe de recursos muito grande de dimensionamento. Se as partições de crescimento para além desta figura, corre o risco de pressão de memória, que por sua vez origina menos compressão ideal.

```sql
SELECT  rp.[name]                                AS [pool_name]
,       rp.[max_memory_kb]                        AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                    AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576                AS [mex_memory_gb]
,       rp.[max_memory_percent]                    AS [max_memory_percent]
,       wg.[name]                                AS [group_name]
,       wg.[importance]                            AS [group_importance]
,       wg.[request_max_memory_grant_percent]    AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups    wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools    rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a>Mudar de partição
Armazém de dados SQL suporta partição dividir, intercalar e mudança. Cada uma destas funções for executada com o [ALTER TABLE](/sql/t-sql/statements/alter-table-transact-sql) instrução.

Para mudar partições entre duas tabelas, tem de garantir que as partições alinhar os respetivos limites e que correspondem às definições de tabela. Como verificar restrições não estão disponíveis para impor o intervalo de valores numa tabela, a tabela de origem tem de conter os mesmo limites de partição como a tabela de destino. Se, em seguida, os limites de partição não são iguais, a mudança de partições irá falhar, não serão sincronizados os metadados da partição.

### <a name="how-to-split-a-partition-that-contains-data"></a>Como dividir uma partição que contém os dados
O método mais eficaz para dividir uma partição que contém os dados já está a utilizar um `CTAS` instrução. Se a tabela particionada um columnstore em cluster, em seguida, a partição de tabela deve estar vazia antes que pode ser dividido.

O exemplo seguinte cria uma tabela particionada columnstore. -Insere uma linha para cada partição:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
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
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [!NOTE]
> Ao criar o objeto de estatística, os metadados de tabela são mais exato. Se omitir estatísticas, o SQL Data Warehouse utilizará os valores predefinidos. Para obter detalhes sobre as estatísticas, consulte [estatísticas](sql-data-warehouse-tables-statistics.md).
> 
> 

A consulta seguinte localiza o número de linhas utilizando o `sys.partitions` vista de catálogo:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

O seguinte comando de divisão recebe uma mensagem de erro:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Tarifas de mensagens 35346, 15 nível, 1 de estado, linha 44 DIVIDIR cláusula da instrução ALTER PARTITION falhou porque a partição não está vazia. Apenas as partições vazia podem ser divididas quando existe um índice columnstore na tabela. Considere desativar o índice columnstore antes de emitir a instrução ALTER PARTITION, em seguida, reconstrua o índice columnstore depois de ALTER PARTITION ter sido concluída.

No entanto, pode utilizar `CTAS` para criar uma nova tabela para armazenar os dados.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Como os limites de partição estão alinhados, é permitido um comutador. Isto irá deixar a tabela de origem com uma partição vazia que pode, subsequentemente, dividir.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Tudo o que está à esquerda é para alinhar com os dados para os novos limites de partição utilizando `CTAS`e, em seguida, mude os dados de volta para a tabela principal.

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

Depois de concluir o movimento de dados, é uma boa ideia para atualizar as estatísticas na tabela de destino. A atualizar as estatísticas assegura que as estatísticas refletem com precisão a distribuição nova dos dados na respetiva respetivas partições.

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>Tabela de criação de partições de controlo de código fonte
Para evitar a definição da tabela de **rusting** no seu sistema de controlo de origem, poderá pretender considerar a seguinte abordagem:

1. Criar a tabela como uma tabela particionada mas não existem valores de partição

    ```sql
    CREATE TABLE [dbo].[FactInternetSales]
    (
        [ProductKey]            int          NOT NULL
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
    ,   DISTRIBUTION = HASH([ProductKey])
    ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES () )
    )
    ;
    ```

1. `SPLIT` a tabela como parte do processo de implementação:

    ```sql
     -- Create a table containing the partition boundaries

    CREATE TABLE #partitions
    WITH
    (
        LOCATION = USER_DB
    ,   DISTRIBUTION = HASH(ptn_no)
    )
    AS
    SELECT  ptn_no
    ,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
    FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
    ) a
    ;

     -- Iterate over the partition boundaries and split the table

    DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
    ,       @i INT = 1                                 --iterator for while loop
    ,       @q NVARCHAR(4000)                          --query
    ,       @p NVARCHAR(20)     = N''                  --partition_number
    ,       @s NVARCHAR(128)    = N'dbo'               --schema
    ,       @t NVARCHAR(128)    = N'FactInternetSales' --table
    ;

    WHILE @i <= @c
    BEGIN
        SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
        SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

        -- PRINT @q;
        EXECUTE sp_executesql @q;
        SET @i+=1;
    END

     -- Code clean-up

    DROP TABLE #partitions;
    ```

Com esta abordagem o código de controlo de origem permanece estático e os valores de limite de criação de partições têm permissão para ser dinâmico; com o armazém a evolução ao longo do tempo.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre como desenvolver tabelas, consulte os artigos no [descrição geral da tabela](sql-data-warehouse-tables-overview.md).

