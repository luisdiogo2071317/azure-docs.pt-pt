---
title: "Introdução de estrutura de tabela - Azure SQL Data Warehouse | Microsoft Docs"
description: "Introdução ao conceber a tabelas no armazém de dados SQL do Azure."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 01/18/2018
ms.author: barbkess
ms.openlocfilehash: 5c163880a7508d69bce0019cc5379bca8c704d59
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="introduction-to-designing-tables-in-azure-sql-data-warehouse"></a>Introdução ao conceber a tabelas no armazém de dados SQL do Azure

Saiba conceitos chave para estruturar a tabelas no armazém de dados SQL do Azure. 

## <a name="determining-table-category"></a>Determinar a categoria de tabela 

A [esquema em estrela](https://en.wikipedia.org/wiki/Star_schema) organiza os dados nas tabelas de factos e de dimensões. Algumas tabelas são utilizadas para dados de integração ou da transição antes, é movido para uma tabela de factos ou de dimensão. Como estruturar uma tabela, decida se os dados da tabela pertence num facto, dimensão ou tabela de integração. Esta decisão informa a estrutura da tabela adequado e a distribuição. 

- **As tabelas de factos** contêm dados quantitativos que são normalmente gerados num sistema transacional e, em seguida, são carregados para o armazém de dados. Por exemplo, uma empresa de revenda gera transações vendas todos os dias e, em seguida, carrega os dados para uma tabela de factos de armazém de dados para análise.

- **As tabelas de dimensão** contêm dados de atributos que podem ser alterado, mas normalmente, as alterações com pouca frequência. Por exemplo, nome e o endereço do cliente são armazenados numa tabela de dimensão e atualizar apenas ao perfil do cliente é alterado. Para minimizar o tamanho de uma tabela de factos grande, nome e o endereço do cliente não precisa de estar em cada linha de uma tabela de factos. Em vez disso, as tabelas de factos e a tabela de dimensão podem partilhar uma ID de cliente. Uma consulta pode associar as duas tabelas para associar o perfil e transações do cliente. 

- **Tabelas de integração** fornecem um local para integrar ou dados de teste. Pode criar uma tabela de integração como uma tabela normal, uma tabela externa ou uma tabela temporária. Por exemplo, pode carregar dados para uma tabela de testes, efetuar transformações de dados no modo de transição e, em seguida, inserir os dados de uma tabela de produção.

## <a name="schema-and-table-names"></a>Nomes de tabela e esquema
No SQL Data Warehouse, um armazém de dados é um tipo de base de dados. Todas as tabelas no armazém de dados estão contidas na mesma base de dados.  Não é possível associar as tabelas em vários armazéns de dados. Este comportamento é diferente do SQL Server, que suporta as associações entre bases de dados. 

Numa base de dados do SQL Server, poderá utilizar facto, Dim. do, ou integrar os nomes de esquema. Se estiver a migrar uma base de dados do SQL Server para o SQL Data Warehouse, funciona melhor para migrar todas as tabelas de facto, dimensão e de integração para um esquema no armazém de dados do SQL Server. Por exemplo, pode armazenar todas as tabelas a [WideWorldImportersDW](/sql/sample/world-wide-importers/database-catalog-wwi-olap) do armazém de dados de exemplo dentro de um esquema chamado wwi. O código seguinte cria um [definido pelo utilizador esquema](/sql/t-sql/statements/create-schema-transact-sql) chamado wwi.

```sql
CREATE SCHEMA wwi;
```

Para mostrar a organização das tabelas no armazém de dados do SQL Server, pode utilizar facto, Dim. do e int como prefixos para os nomes de tabela. A tabela seguinte mostra alguns dos nomes de tabela e esquema de WideWorldImportersDW. Compara os nomes no SQL Server com nomes no armazém de dados do SQL Server. 

| Tabela de WideWorldImportersDW  | Tipo de tabela | SQL Server | SQL Data Warehouse |
|:-----|:-----|:------|
| Cidade | Dimensão | Dimension.City | wwi.DimCity |
| Ordem | Facto de | Fact.Order | wwi.FactOrder |


## <a name="table-persistence"></a>Persistência da tabela 

Tabelas armazenam dados de forma permanente no armazenamento do Azure, temporariamente no armazenamento do Azure ou num arquivo de dados externo para o armazém de dados.

### <a name="regular-table"></a>Tabela normal

Uma tabela normal armazena os dados no armazenamento do Azure como parte do armazém de dados. A tabela e os dados são mantidas independentemente se encontra aberta uma sessão.  Este exemplo cria uma tabela normal com duas colunas. 

```sql
CREATE TABLE MyTable (col1 int, col2 int );  
```

### <a name="temporary-table"></a>Tabela temporária
Só existe uma tabela temporária para a duração da sessão. Pode utilizar uma tabela temporária para impedir que outras utilizações ver resultados temporários e também reduzir a necessidade de limpeza.  Uma vez que a tabelas temporárias também utilizam o armazenamento local, pode oferecem um desempenho mais rápido para algumas operações.  Para obter mais informações, consulte [tabelas temporárias](sql-data-warehouse-tables-temporary.md).

### <a name="external-table"></a>Tabela externa
Pontos de uma tabela externa aos dados localizados no blob Storage do Azure ou do Azure Data Lake Store. Quando utilizado em conjunto com a instrução CREATE TABLE AS SELECT, selecionando a partir de uma tabela externa importa dados para o SQL Data Warehouse. As tabelas externas, por isso, são úteis para carregar dados. Para um tutorial de carregamento, consulte [PolyBase de utilização para carregar dados do blob storage do Azure](load-data-from-azure-blob-storage-using-polybase.md).

## <a name="data-types"></a>Tipos de dados
Armazém de dados SQL suporta utilizadas mais frequentemente tipos de dados. Para obter uma lista dos tipos de dados suportadas, consulte [tipos de dados de referência de CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes) na instrução CREATE TABLE. Minimizar o tamanho dos tipos de dados ajuda a melhorar o desempenho de consulta. Para obter orientações sobre como utilizar os tipos de dados, consulte [tipos de dados](sql-data-warehouse-tables-data-types.md).

## <a name="distributed-tables"></a>Tabelas distribuídas
Uma funcionalidade fundamental do SQL Data Warehouse é a forma como pode armazenar e operar em tabelas em 60 [distribuições](massively-parallel-processing-mpp-architecture.md#distributions).  As tabelas são distribuídas utilizando um método de replicação, hash ou round robin.

### <a name="hash-distributed-tables"></a>Tabelas hash distribuída
A distribuição de hash distribui as linhas com base no valor da coluna de distribuição. A tabela hash distribuída foi concebida para elevado desempenho para associações de consulta em tabelas grandes. Existem vários fatores que afetam a escolha da coluna de distribuição. 

Para obter mais informações, consulte [conceber de orientações para tabelas distribuídas](sql-data-warehouse-tables-distribute.md).

### <a name="replicated-tables"></a>Tabelas replicadas
Uma tabela não replicada tem uma cópia completa da tabela disponível em cada nó de computação. Consultas executadas rápido em tabelas replicadas, uma vez que as associações em tabelas replicadas não necessitam de movimento de dados. A replicação necessita de armazenamento adicional, embora e não é prática para tabelas grandes. 

Para obter mais informações, consulte [conceber de orientações para tabelas replicadas](design-guidance-for-replicated-tables.md).

### <a name="round-robin-tables"></a>Round robin tabelas
Uma tabela de round robin distribui as linhas da tabela uniformemente por todas as distribuições. As linhas são distribuídas aleatoriamente. Carregar dados para uma tabela de round robin é rápida.  No entanto, consultas podem exigir mais de movimento de dados que os outros métodos de distribuição. 

Para obter mais informações, consulte [conceber de orientações para tabelas distribuídas](sql-data-warehouse-tables-distribute.md).


### <a name="common-distribution-methods-for-tables"></a>Métodos de distribuição comuns para tabelas
A categoria de tabela, muitas vezes, determina qual a opção para escolher para distribuir a tabela. 

| Categoria da tabela | Opção de distribuição recomendada |
|:---------------|:--------------------|
| Facto de           | Utilize a distribuição de hash com o índice columnstore em cluster. Melhora o desempenho quando duas tabelas de hash forem associadas na mesma coluna de distribuição. |
| Dimensão      | Utilize replicada para tabelas mais pequenas. Se as tabelas são demasiado grandes para armazenar em cada nó de computação, utilize distribuído de hash. |
| Testes        | Utilize o round robin para a tabela de testes. A carga com CTAS é rápida. Depois dos dados na tabela de teste, utilize INSERT... SELECIONE para mover os dados para um tabelas de produção. |

## <a name="table-partitions"></a>Partições da tabela
Uma tabela particionada armazena e efetua operações nas linhas de tabela, de acordo com os intervalos de dados. Por exemplo, uma tabela pode ser particionada por dia, mês ou ano. Pode melhorar o desempenho das consultas através de eliminação de partição, o que limita uma análise de consulta para dados dentro de uma partição. Também pode manter os dados através de mudança de partições. Uma vez que os dados no armazém de dados do SQL Server já tenham sido distribuídos, partições demasiados podem abrandar o desempenho das consultas. Para obter mais informações, consulte [orientações de criação de partições](sql-data-warehouse-tables-partition.md).

## <a name="columnstore-indexes"></a>Índices Columnstore
Por predefinição, o SQL Data Warehouse armazena uma tabela como um índice columnstore em cluster. Esta forma de armazenamento de dados permite compressão de dados de elevado e o desempenho das consultas em tabelas grandes.  O índice columnstore em cluster costuma é a melhor opção, mas em alguns casos, um índice em cluster ou de uma área dinâmica para dados é a estrutura de armazenamento adequado.

Para obter uma lista das funcionalidades de columnstore, consulte [que há de novo para índices columnstore](/sql/relational-databases/indexes/columnstore-indexes-what-s-new). Para melhorar o desempenho de índice columnstore, consulte o artigo [maximizando a qualidade de rowgroup para índices columnstore](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

## <a name="statistics"></a>Estatísticas
O otimizador de consultas utiliza estatísticas de nível de coluna quando cria o plano para executar uma consulta. Para melhorar o desempenho das consultas, é importante criar estatísticas em colunas individuais, especialmente as colunas utilizadas nas associações de consulta. Criar e atualizar as estatísticas não ocorrem automaticamente. [Criar estatísticas](/sql/t-sql/statements/create-statistics-transact-sql) depois de criar uma tabela. Atualize as estatísticas após um número significativo de linhas é adicionado ou alterado. Por exemplo, Atualize as estatísticas após uma carga. Para obter mais informações, consulte [orientações de estatísticas](sql-data-warehouse-tables-statistics.md).

## <a name="commands-for-creating-tables"></a>Comandos para criar tabelas
Pode criar uma tabela como uma nova tabela vazia. Também pode criar e preencher uma tabela com os resultados de uma instrução select. Seguem-se os comandos de T-SQL para criar uma tabela.

| Declaração de T-SQL | Descrição |
|:----------------|:------------|
| [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse) | Cria uma tabela vazia, definindo as colunas da tabela e as opções. |
| [CRIAR TABELA EXTERNA](/sql/t-sql/statements/create-external-table-transact-sql) | Cria uma tabela externa. A definição da tabela é armazenada no SQL Data Warehouse. Os dados da tabela são armazenados no armazenamento de Blobs do Azure ou do Azure Data Lake Store. |
| [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) | Preenche uma nova tabela com os resultados de uma instrução select. Os tipos de dados e de colunas de tabela baseiam-se nos resultados da instrução select. Para importar dados, pode selecionar esta declaração de uma tabela externa. |
| [CRIAR EXTERNO TABLE AS SELECT](/sql/t-sql/statements/create-external-table-as-select-transact-sql) | Cria uma nova tabela externa ao exportar os resultados de uma instrução select para uma localização externa.  A localização é Blob storage do Azure ou do Azure Data Lake Store. |

## <a name="aligning-source-data-with-the-data-warehouse"></a>Alinhar a origem de dados no armazém de dados

As tabelas do armazém de dados são preenchidas pelo carregar dados a partir de outra origem de dados. Para efetuar um carregamento com êxito, os tipos de dados e o número de colunas nos dados de origem devem estar alinhadas com a definição de tabela no armazém de dados. Obter os dados para assegurar a poderão a parte hardest de conceber as tabelas. 

Se data for proveniente de vários arquivos de dados, pode colocar os dados no armazém de dados e guarde-o numa tabela de integração. Depois de dados se encontra na tabela de integração, pode utilizar o poder do armazém de dados do SQL Server para efetuar operações de transformação. Depois dos dados são preparados, pode inseri-lo em tabelas de produção.

## <a name="unsupported-table-features"></a>Funcionalidades de tabela não suportado
Armazém de dados SQL suporta muitas, mas não todas, as tabela funcionalidades oferecidas pelas outras bases de dados.  A lista seguinte mostra algumas das funcionalidades de tabela que não são suportadas no SQL Data Warehouse.

- Chave primária, chaves externas, exclusivos, a verificação de [restrições de tabela](/sql/t-sql/statements/alter-table-table-constraint-transact-sql)

- [Colunas calculadas](/sql/t-sql/statements/alter-table-computed-column-definition-transact-sql)
- [Vistas indexadas](/sql/relational-databases/views/create-indexed-views)
- [Sequence](/sql/t-sql/statements/create-sequence-transact-sql)
- [Colunas dispersas](/sql/relational-databases/tables/use-sparse-columns)
- [Caráter de substituição de chaves](). Implementar com [identidade](sql-data-warehouse-tables-identity.md).
- [Sinónimos](/sql/t-sql/statements/create-synonym-transact-sql)
- [Acionadores](/sql/t-sql/statements/create-trigger-transact-sql)
- [Índices exclusivos](/sql/t-sql/statements/create-index-transact-sql)
- [Tipos definidos pelo utilizador](/sql/relational-databases/native-client/features/using-user-defined-types)

## <a name="table-size-queries"></a>Consultas de tamanho da tabela
Uma forma simples para identificar o espaço e linhas consumidas por uma tabela em cada uma das 60 distribuições, é utilizar [DBCC PDW_SHOWSPACEUSED] [DBCC PDW_SHOWSPACEUSED].

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

No entanto, utilizar comandos DBCC pode ser bastante limitação.  Vistas de gestão dinâmica (DMVs) mostram mais detalhes daqueles comandos DBCC. Comece por criar esta vista.

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a>Resumo de espaço de tabela

Esta consulta devolve as linhas e espaço por tabela.  Permite-lhe ver as tabelas são suas tabelas de maiores e são round robin, replicados, ou hash - distribuído.  Para as tabelas hash distribuída, a consulta mostra a coluna de distribuição.  

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>Espaço de tabela por tipo de distribuição

```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>Espaço de tabela por tipo de índice

```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>Resumo de espaço de distribuição

```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>Passos Seguintes
Depois de criar as tabelas para o seu armazém de dados, o passo seguinte é carregar dados para a tabela.  Para um tutorial de carregamento, consulte [carregamento de dados do blob storage do Azure com o PolyBase](load-data-from-azure-blob-storage-using-polybase.md).