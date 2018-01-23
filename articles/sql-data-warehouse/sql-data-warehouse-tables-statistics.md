---
title: "Gerir as estatísticas em tabelas no armazém de dados SQL | Microsoft Docs"
description: "Introdução ao estatísticas de tabelas do Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 11/06/2017
ms.author: barbkess
ms.openlocfilehash: b007e1894f163d50dbf31e3c09b4b5ff329adb59
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="managing-statistics-on-tables-in-sql-data-warehouse"></a>Gerir as estatísticas em tabelas no armazém de dados do SQL Server
> [!div class="op_single_selector"]
> * [Descrição geral][Overview]
> * [Tipos de dados][Data Types]
> * [Distribuir][Distribute]
> * [Índice][Index]
> * [Partição][Partition]
> * [Estatísticas][Statistics]
> * [Temporário][Temporary]
> 
> 

Mais Azure SQL Data Warehouse conhece os dados, quanto mais depressa pode executar consultas nele. Recolher estatísticas nos seus dados e, em seguida, carregá-lo para o SQL Data Warehouse são uma das ações que mais importante, pode fazê-lo a otimizar as suas consultas. Isto acontece porque o otimizador de consultas do SQL Data Warehouse é um otimizador baseada no custo. Compara o custo de vários planos de consulta e, em seguida, escolhe o plano com o menor custo, que é, na maioria dos casos, o plano que executa o mais rápido. Por exemplo, se o otimizador estimativas de que a data que está a filtrar na sua consulta irá devolver uma linha, este pode escolher um plano diferente que o se que calcula a data selecionada devolverá 1 milhões de linhas.

O processo de criação e a atualizar as estatísticas atualmente é um processo manual, mas é simple para o fazer.  Em breve poderá criar e atualizar a estatística na único colunas e índices automaticamente.  Ao utilizar as seguintes informações, pode automatizar bastante a gestão das estatísticas nos seus dados. 

## <a name="getting-started-with-statistics"></a>Introdução ao estatísticas
Criar estatísticas amostras em cada coluna é uma forma fácil para começar a utilizar. Estatísticas Desatualizadas levar para o desempenho da consulta inferior ao ideal. No entanto, a atualizar as estatísticas em todas as colunas à medida que aumenta os dados pode consumir memória. 

Seguem-se recomendações para diferentes cenários:
| **Cenário** | Recomendação |
|:--- |:--- |
| **Introdução** | Atualizar todas as colunas depois de migrar para o SQL Data Warehouse |
| **Coluna mais importante para estatísticas** | Tecla de distribuição do hash |
| **Segundo mais importante coluna para estatísticas** | Chave de partição |
| **Outras colunas importantes para estatísticas** | Associações de data, frequente, GROUP BY, HAVING e onde |
| **Frequência de atualizações de estatísticas**  | Conservador: diária <br></br> Depois de carregar ou transformar os seus dados |
| **Amostragem** |  Inferior a mil milhões de 1 de linhas, utilize amostragem predefinido (20 por cento) <br></br> Com mais de mil milhões de 1 de linhas, é boa estatísticas de um intervalo de percentagem de 2 |

## <a name="updating-statistics"></a>A atualizar as estatísticas

Um procedimento recomendado é atualizar estatísticas de colunas de data por dia datas novas são adicionadas. Cada novas linhas de tempo são carregados para o armazém de dados, datas de carga novo ou datas de transação são adicionadas. Estes alterar a distribuição de dados e tome as estatísticas desatualizada. Por outro lado, as estatísticas de uma coluna de país de uma tabela de cliente nunca poderão ter de ser atualizado, porque a distribuição dos valores, geralmente, não irá alterar. Partindo do princípio que de distribuição é constante entre os clientes, adicionar novas linhas para a variação de tabela não se encontra vai alterar a distribuição de dados. No entanto, se o armazém de dados contém apenas um país e colocar dados de um novo país, resultando em dados a partir de vários países que está a ser armazenados, em seguida, terá de atualizar as estatísticas da coluna de país.

Uma das primeiras questões colocadas pedir ao está a resolução de problemas de uma consulta é, **"São as estatísticas atualizadas?"**

Esta questão não é um que pode ser respondida pela idade dos dados. Um objeto de estatísticas atualizados pode ser antigo não se tiver ocorrido nenhuma alteração essenciais para os dados subjacentes. Quando o número de linhas mudou substancialmente ou existe uma alteração essenciais da distribuição dos valores de coluna de *, em seguida,* está na altura de atualizar as estatísticas.

Porque não existe nenhuma vista de gestão dinâmica para determinar se os dados na tabela foi alterada desde que foram atualizadas as última estatísticas de tempo, saber a idade dos seus estatísticas pode fornecer com parte da imagem.  Pode utilizar a seguinte consulta para determinar a última vez foram atualizadas as estatísticas em cada tabela.  

> [!NOTE]
> Lembre-se de que, se houver uma alteração essenciais da distribuição dos valores para uma coluna, deve atualizar estatísticas, independentemente da última vez que foram atualizados.  
> 
> 

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc    
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co    
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty    
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb    
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm    
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

**Data colunas** num armazém de dados, por exemplo, normalmente, tem de induzirem frequentes estatísticas atualizações. Cada novas linhas de tempo são carregados para o armazém de dados, datas de carga novo ou datas de transação são adicionadas. Estes alterar a distribuição de dados e tome as estatísticas desatualizada.  Por outro lado, estatísticas de uma coluna de sexo numa tabela cliente nunca poderão ter de ser atualizados. Partindo do princípio que de distribuição é constante entre os clientes, adicionar novas linhas para a variação de tabela não se encontra vai alterar a distribuição de dados. No entanto, se o armazém de dados contém apenas um sexo e um novo resultados de requisito no genders vários, em seguida, terá de atualizar as estatísticas na coluna sexo.

Para uma explicação mais, consulte [estatísticas] [ Statistics] no MSDN.

## <a name="implementing-statistics-management"></a>Implementar a gestão de estatísticas
Muitas vezes, é uma boa ideia para expandir o processo de carregamento de dados para se certificar de que as estatísticas são atualizadas no fim de carga. O carregamento de dados é quando tabelas com mais frequência a alterar o tamanho e/ou a respetiva distribuição dos valores. Por conseguinte, este é um local lógico ao implementar alguns processos de gestão.

Os seguintes princípios de orientação para são fornecidos para atualizar as estatísticas durante o processo de carga:

* Certifique-se de que cada tabela carregada tem, pelo menos, um objeto de estatísticas atualizado. Isto atualiza as informações de tamanho (contagem de linhas e contagem de páginas) da tabela como parte da atualização de estatísticas.
* Concentre-se em colunas que participam em cláusulas associação, GROUP BY, ORDER BY e DISTINCT.
* Pondere atualizar as colunas "chave ascendente" como transação datas com mais frequência, porque estes valores não serão incluídos em histograma de estatísticas.
* Considere atualizar colunas de distribuição estático menos frequência.
* Lembre-se de que cada objeto de estatística é atualizado na sequência. Basta implementar `UPDATE STATISTICS <TABLE_NAME>` nem sempre é ideal, especialmente para tabelas wide com muitos objetos de estatísticas.

Para uma explicação mais, consulte [estimativa de cardinalidade] [ Cardinality Estimation] no MSDN.

## <a name="examples-create-statistics"></a>Exemplos: Criar estatísticas
Estes exemplos mostram como utilizar várias opções para criar estatísticas. As opções que utilizar para cada coluna dependem as características de dados e a forma como a coluna será utilizada nas consultas.

### <a name="create-single-column-statistics-with-default-options"></a>Criar estatísticas de coluna única com opções predefinidas
Para criar estatísticas numa coluna, só tem de fornecer um nome para o objeto de estatísticas e o nome da coluna.

Esta sintaxe utiliza todas as opções predefinidas. Por predefinição, os exemplos de armazém de dados do SQL Server **20 por cento** da tabela quando cria estatísticas.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Por exemplo:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="create-single-column-statistics-by-examining-every-row"></a>Criar estatísticas de coluna única, examinando cada linha
A frequência de amostragem da predefinição de 20 por cento é suficiente para a maioria das situações. No entanto, pode ajustar a frequência de amostragem.

Para a tabela completa de exemplo, utilize esta sintaxe:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Por exemplo:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="create-single-column-statistics-by-specifying-the-sample-size"></a>Criar estatísticas de coluna única, especificando o tamanho da amostra
Em alternativa, pode especificar o tamanho da amostra como uma percentagem:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="create-single-column-statistics-on-only-some-of-the-rows"></a>Criar estatísticas de coluna única na apenas algumas das linhas
Também pode criar estatísticas sobre uma parte das linhas na tabela. Esta opção é denominada uma estatística filtrada.

Por exemplo, pode utilizar as estatísticas filtradas quando planeia uma partição específica de uma tabela particionada grande de consulta. Ao criar estatísticas em apenas os valores de partição, a precisão das estatísticas melhorar e, por conseguinte, melhorar o desempenho das consultas.

Este exemplo cria estatísticas num intervalo de valores. Os valores facilmente podem ser definidos para corresponder ao intervalo de valores numa partição.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [!NOTE]
> Para o otimizador de consultas ponderar a utilização de estatísticas filtradas quando escolhe o plano de consulta distribuída, a consulta deve caber a definição do objeto de estatísticas. Utilizando o exemplo anterior, a qual cláusula tem de especificar valores de col1 entre 2000101 e 20001231 da consulta.
> 
> 

### <a name="create-single-column-statistics-with-all-the-options"></a>Criar estatísticas de coluna única com todas as opções
Também pode combinar as opções em conjunto. O exemplo seguinte cria um objeto de estatísticas filtradas com um tamanho de amostra personalizado:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Para referência completa, consulte [CREATE STATISTICS] [ CREATE STATISTICS] no MSDN.

### <a name="create-multi-column-statistics"></a>Criar estatísticas de várias colunas
Para criar um objeto de estatísticas de várias colunas, basta utilizar exemplos anteriores, mas especificar mais colunas.

> [!NOTE]
> Histograma, que é utilizada para estimar o número de linhas no resultado da consulta, só está disponível para a primeira coluna listada na definição do objeto de estatísticas.
> 
> 

Neste exemplo, a histograma é no *produto\_categoria*. Estatísticas de coluna em vários locais são calculadas *produto\_categoria* e *produto\_sub_category*:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

Porque não existe uma correlação entre *produto\_categoria* e *produto\_sub\_categoria*, um objeto de estatísticas de várias colunas pode ser úteis se estes as colunas são acedidas ao mesmo tempo.

### <a name="create-statistics-on-all-columns-in-a-table"></a>Criar estatísticas em todas as colunas numa tabela
É uma forma de criar estatísticas emitir comandos CREATE STATISTICS depois de criar a tabela:

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### <a name="use-a-stored-procedure-to-create-statistics-on-all-columns-in-a-database"></a>Utilizar um procedimento armazenado para criar estatísticas em todas as colunas numa base de dados
Armazém de dados do SQL Server não tem um procedimento de sistema armazenados equivalente ao sp_create_stats no SQL Server. Este procedimento armazenado cria um objeto de estatísticas de coluna única em cada coluna da base de dados que já não tem as estatísticas.

O exemplo seguinte irá ajudá-lo a começar com o design da sua base de dados. Não hesite adaptá-lo para as suas necessidades:

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
    DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN    sys.[external_tables] e    ON    e.[object_id]        = t.[object_id]
WHERE       l.[object_id] IS NULL
AND            e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+@sample_pct+'PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

Para criar estatísticas em todas as colunas na tabela com este procedimento, basta chame o procedimento.

```sql
prc_sqldw_create_stats;
```

## <a name="examples-update-statistics"></a>Exemplos: Estatísticas de atualização
Para atualizar as estatísticas, pode:

- Atualize um objeto de estatísticas. Especifique o nome do objeto que pretende atualizar as estatísticas.
- Atualize todos os objetos de estatísticas numa tabela. Especifique o nome da tabela em vez de um objeto de estatísticas específico.

### <a name="update-one-specific-statistics-object"></a>Atualizar um objeto de estatísticas específico
Utilize a sintaxe seguinte para atualizar um objeto de estatísticas específico:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Por exemplo:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Ao atualizar objetos de estatísticas específico, pode minimizar o tempo e recursos necessários para gerir as estatísticas. Isto requer alguns considerar escolher os objetos de estatísticas melhor a atualizar.

### <a name="update-all-statistics-on-a-table"></a>Atualizar todas as estatísticas numa tabela
Isto mostra um método simples para atualizar todos os objetos de estatísticas numa tabela:

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Por exemplo:

```sql
UPDATE STATISTICS dbo.table1;
```

É fácil de utilizar esta instrução. Apenas Lembre-se de que atualiza *todos os* estatísticas na tabela e, por isso, poderá efetuar trabalho mais do que é necessário. Se o desempenho não é um problema, esta é a forma mais fácil e mais completa para garantir que as estatísticas são atualizadas.

> [!NOTE]
> Ao atualizar todas as estatísticas numa tabela, o SQL Data Warehouse efetua uma análise para a tabela para cada objeto de estatísticas de exemplo. Se a tabela é grande e tem demasiadas colunas e muitas das estatísticas, poderá ser mais eficiente para atualizar as estatísticas de individuais com base na necessidade.
> 
> 

Para uma implementação de um `UPDATE STATISTICS` procedimento, consulte [tabelas temporárias][Temporary]. O método de implementação é ligeiramente diferente do precedente `CREATE STATISTICS` procedimento, mas o resultado é o mesmo.

Para a sintaxe completa, consulte [Update Statistics] [ Update Statistics] no MSDN.

## <a name="statistics-metadata"></a>Metadados de estatísticas
Existem várias vistas de sistema e as funções que pode utilizar para localizar informações sobre as estatísticas. Por exemplo, pode ver se um objeto de estatísticas poderá estar desatualizado utilizando a função de data estatísticas para ver quando estatísticas foram criadas ou atualizadas pela última vez.

### <a name="catalog-views-for-statistics"></a>Vistas de catálogo para estatísticas
Estas vistas de sistema fornecem informações sobre as estatísticas:

| Vista de catálogo | Descrição |
|:--- |:--- |
| [sys.columns][sys.columns] |Uma linha para cada coluna. |
| [sys.objects][sys.objects] |Uma linha para cada objeto na base de dados. |
| [sys.schemas][sys.schemas] |Uma linha para cada esquema na base de dados. |
| [sys.stats][sys.stats] |Uma linha para cada objeto de estatísticas. |
| [sys.stats_columns][sys.stats_columns] |Uma linha para cada coluna no objeto de estatísticas. Fornece hiperligações para sys.columns. |
| [sys.tables][sys.tables] |Uma linha para cada tabela (inclui as tabelas externas). |
| [sys.table_types][sys.table_types] |Uma linha para cada tipo de dados. |

### <a name="system-functions-for-statistics"></a>Funções de sistema para estatísticas
Estas funções de sistema são úteis para trabalhar com as estatísticas:

| Função de sistema | Descrição |
|:--- |:--- |
| [STATS_DATE][STATS_DATE] |Data que da última atualização o objeto de estatísticas. |
| [DBCC SHOW_STATISTICS][DBCC SHOW_STATISTICS] |Resumidas nível e informações detalhadas sobre a distribuição dos valores como compreendido pelo objeto estatísticas. |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a>Combinar as colunas de estatísticas e as funções para uma vista
Esta vista inclui colunas relacionados com as estatísticas e resulta na função STATS_DATE() em conjunto.

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_defiinition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## <a name="dbcc-showstatistics-examples"></a>Exemplos de DBCC SHOW_STATISTICS()
DBCC SHOW_STATISTICS() mostra os dados retidos dentro de um objeto de estatísticas. Estes dados é apresentada no três partes:

- Cabeçalho
- Vetor de densidade
- Histograma

Os metadados de cabeçalho sobre as estatísticas. A histograma mostra a distribuição dos valores na primeira coluna chave do objeto de estatísticas. O vetor de densidade mede a correlação de coluna em vários locais. O SQL Data Warehouse calcula estimativas de cardinalidade com quaisquer dados no objeto de estatísticas.

### <a name="show-header-density-and-histogram"></a>Mostrar o cabeçalho, densidade e histograma
Neste exemplo simples mostra todos os três partes de um objeto de estatísticas:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Por exemplo:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a>Mostrar uma ou mais partes de DBCC SHOW_STATISTICS()
Se apenas estiver interessado em partes específicas de visualização, utilize o `WITH` cláusula e especifique as peças que pretende ver:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Por exemplo:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a>Diferenças de DBCC SHOW_STATISTICS()
DBCC SHOW_STATISTICS() mais estritamente é implementado no armazém de dados SQL relativamente ao SQL Server:

- Sinalizadores de funcionalidades não são suportadas.
- Não é possível utilizar Stats_stream.
- Não é possível associar resultados para subconjuntos específicos de dados de estatísticas. Por exemplo, (STAT_HEADER associar DENSITY_VECTOR).
- Não é possível definir NO_INFOMSGS para supressão de mensagem.
- Não é possível utilizar parênteses Retos à volta de nomes de estatísticas.
- Não é possível utilizar nomes de colunas para identificar objetos de estatísticas.
- Erro personalizado 2767 não é suportado.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais detalhes, consulte [DBCC SHOW_STATISTICS] [ DBCC SHOW_STATISTICS] no MSDN.

  Para obter mais informações, consulte os artigos no [descrição geral da tabela][Overview], [tipos de dados de tabela][Data Types], [distribuir uma tabela] [ Distribute], [Uma tabela de indexação][Index], [uma tabela de criação de partições][Partition]e [Tabelas temporárias][Temporary].
  
   Para obter mais informações sobre as melhores práticas, consulte [melhores práticas do SQL Data Warehouse][SQL Data Warehouse Best Practices].  

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->  
[Cardinality Estimation]: https://msdn.microsoft.com/library/dn600374.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[DBCC SHOW_STATISTICS]:https://msdn.microsoft.com/library/ms174384.aspx
[Statistics]: https://msdn.microsoft.com/library/ms190397.aspx
[STATS_DATE]: https://msdn.microsoft.com/library/ms190330.aspx
[sys.columns]: https://msdn.microsoft.com/library/ms176106.aspx
[sys.objects]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.schemas]: https://msdn.microsoft.com/library/ms190324.aspx
[sys.stats]: https://msdn.microsoft.com/library/ms177623.aspx
[sys.stats_columns]: https://msdn.microsoft.com/library/ms187340.aspx
[sys.tables]: https://msdn.microsoft.com/library/ms187406.aspx
[sys.table_types]: https://msdn.microsoft.com/library/bb510623.aspx
[UPDATE STATISTICS]: https://msdn.microsoft.com/library/ms187348.aspx

<!--Other Web references-->  
