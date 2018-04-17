---
title: Grupo a utilizar pelas opções no Azure SQL Data Warehouse | Microsoft Docs
description: Sugestões para implementar o grupo por opções no Azure SQL Data Warehouse para desenvolver soluções.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/12/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 98d2ecfd2f38d086e50f3103b8598b1dccc9323b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="group-by-options-in-sql-data-warehouse"></a>Agrupar por opções no SQL Data Warehouse
Sugestões para implementar o grupo por opções no Azure SQL Data Warehouse para desenvolver soluções.

## <a name="what-does-group-by-do"></a>O que faz GROUP BY?

O [GROUP BY](/sql/t-sql/queries/select-group-by-transact-sql) cláusula de T-SQL agrega os dados para um resumo de conjunto de linhas. GROUP BY tem algumas opções que não suporta o SQL Data Warehouse. Estas opções têm soluções.

Estas opções são

* GROUP BY com ROLLUP
* CONJUNTOS DE AGRUPAMENTO
* GROUP BY cubo

## <a name="rollup-and-grouping-sets-options"></a>Rollup e grouping define as opções
Aqui a opção mais simples consiste em utilizar UNION ALL em vez disso, para executar o rollup em vez de depender a sintaxe explícita. O resultado é exatamente o mesmo

O exemplo seguinte utilizando a instrução GROUP BY com a opção de agregação:
```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

Através da utilização de agregação, o exemplo anterior pedidos agregações seguintes:

* País e região
* País
* Total geral

A substituir o ROLLUP e devolver os mesmos resultados, pode utilizar UNION ALL e especificar explicitamente agregações necessárias:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

Para substituir GROUPING SETS, aplica-se do princípio de exemplo. Só tem de criar secções UNION ALL para os níveis de agregação que pretende ver.

## <a name="cube-options"></a>Opções do cubo
É possível criar um grupo por com cubo utilizando a abordagem UNION ALL. O problema é que o código pode rapidamente tornar-se complexa e unwieldy. Para atenuar isto, pode utilizá-lo mais avançadas abordagem.

Vamos utilizar o exemplo acima.

O primeiro passo consiste em definir a 'cube' que define todos os níveis de agregação que pretendemos criar. É importante tome nota do CROSS JOIN das duas tabelas derivadas. Isto gera todos os níveis de-nos. O resto do código existe realmente para formatação.

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

O seguinte mostra os resultados do CTAS:

![Agrupar por cubo](media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png)

O segundo passo é especificar uma tabela de destino para armazenar os resultados intermédio:

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

O terceiro passo é cíclicas através do nosso cubo de colunas de executar a agregação. A consulta será executada uma vez para cada linha na tabela temporária #Cube e armazenar os resultados na tabela temporária #Results

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Por último, pode devolver os resultados por simplesmente ao ler a partir da tabela temporária #Results

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Ao dividi o código em secções e gerar uma construção ciclo, o código torna-se mais fácil de gerir e sustentável.

## <a name="next-steps"></a>Passos Seguintes
Para mais sugestões de desenvolvimento, consulte [descrição geral do desenvolvimento](sql-data-warehouse-overview-develop.md).

