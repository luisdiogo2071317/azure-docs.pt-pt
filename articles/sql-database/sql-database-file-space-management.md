---
title: Gestão de espaço de ficheiro de base de dados SQL do Azure | Documentos da Microsoft
description: Esta página descreve como gerir o espaço de ficheiro com a base de dados do Azure SQL e fornece exemplos de código para saber como determinar se necessita de reduzir uma base de dados, bem como a forma como para executar uma base de dados reduzir a operação.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: how-to
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: carlrab
ms.openlocfilehash: 1ecc0ce08ef42f5f5935bca29e8269be2ea142f0
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39415149"
---
# <a name="manage-file-space-in-azure-sql-database"></a>Gerir o espaço de ficheiro na base de dados do Azure SQL

Este artigo descreve os diferentes tipos de espaço de armazenamento na base de dados do Azure SQL e os passos que podem ser realizados quando o espaço de ficheiro alocado para bases de dados e precisa ser gerenciado pelo cliente de conjuntos elásticos.

## <a name="overview"></a>Descrição geral

Na base de dados SQL do Azure, as métricas de tamanho de armazenamento apresentadas no portal do Azure e as seguintes APIs medem o número de páginas de dados utilizados para conjuntos elásticos e bases de dados:
- O Azure Resource Manager com base em métricas de APIs incluindo PowerShell [get-métricas](https://docs.microsoft.com/powershell/module/azurerm.insights/get-azurermmetric)
- T-SQL: [DM db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)
- T-SQL: [resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)
- T-SQL: [sys.elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)

Existem padrões de carga de trabalho em que a alocação de espaço nos arquivos de dados subjacente para bases de dados se torna maior do que o número de páginas de dados utilizados nos arquivos de dados. Este cenário pode ocorrer quando o espaço utilizado aumenta e, em seguida, os dados são eliminados. Quando os dados são eliminados, o espaço de ficheiro alocado não será automaticamente recuperado quando os dados são eliminados. Em tais cenários, o espaço alocado para um banco de dados ou um conjunto pode exceder os limites máximos suportados conjunto (ou suportadas) para a base de dados e, como resultado, evitar o crescimento de dados ou impedir alterações de escalão de desempenho, mesmo que o espaço de base de dados, na verdade, utilizado é menor que o máximo limite de espaço. Para atenuar, se pretender reduzir a base de dados para reduzir o espaço alocado mas não utilizado no banco de dados.

O serviço de base de dados SQL não reduzir automaticamente os ficheiros de base de dados para recuperar espaço alocado não utilizado devido ao potencial impacto no desempenho da base de dados. No entanto, pode diminuir o ficheiro numa base de dados num momento à sua escolha, seguindo os passos descritos em [recuperar não utilizada de espaço em atribuído](#reclaim-unused-allocated-space). 

> [!NOTE]
> Ao contrário dos arquivos de dados, o serviço de base de dados SQL diminui automaticamente ficheiros de registo, uma vez que essa operação não afeta o desempenho de base de dados.

## <a name="understanding-the-types-of-storage-space-for-a-database"></a>Compreender os tipos de espaço de armazenamento para uma base de dados

Para gerir o espaço de ficheiro, terá de compreender os seguintes termos relacionados com a base de dados armazenamento para ambas as bases de dados individuais e para um conjunto elástico.

|Termo do espaço de armazenamento|Definição|Comentários|
|---|---|---|
|**Espaço de dados utilizado**|A quantidade de espaço utilizado para armazenar dados de base de dados em páginas de 8 KB.|Geralmente, este espaço utilizado aumenta (diminuições) no inserções (eliminações). Em alguns casos, o espaço utilizado não é alterado no inserções ou elimina dependendo da quantidade e o padrão de dados envolvidos na operação e qualquer fragmentação. Por exemplo, a eliminar uma linha de cada página de dados não necessariamente reduzir o espaço utilizado.|
|**Espaço alocado**|A quantidade de formatado disponibilizado para armazenar dados de base de dados de espaço de ficheiro|O espaço alocado cresce automaticamente, mas nunca diminui após eliminações. Esse comportamento garante que futuras inserções são mais rápidas, uma vez que o espaço não precisa de ser reformatado.|
|**Espaço alocado, mas não utilizado**|A quantidade de espaço de ficheiro de dados não utilizados alocado para a base de dados.|Esta quantidade é a diferença entre a quantidade de espaço alocado e espaço utilizado e representa a quantidade máxima de espaço que pode ser recuperada pelo reduzindo ficheiros de base de dados.|
|**Tamanho máx.**|A quantidade máxima de espaço de dados que pode ser utilizado pela base de dados.|O espaço de dados alocado não pode ultrapassar o tamanho máximo de dados.|
||||

O diagrama seguinte ilustra a relação entre os tipos de espaço de armazenamento.

![tipos de espaço de armazenamento e as relações](./media/sql-database-file-space-management/storage-types.png)

## <a name="query-a-database-for-storage-space-information"></a>Consultar uma base de dados para obter informações de espaço de armazenamento

Para determinar se alocou, mas não utilizado espaço de dados para uma base de dados individual que pretende recuperar, utilize as seguintes consultas:

### <a name="database-data-space-used"></a>Espaço de dados de base de dados utilizado
Modifique a consulta seguinte para devolver a quantidade de espaço de dados de base de dados utilizada em MB.

```sql
-- Connect to master
-- Database data space used in MB
SELECT TOP 1 storage_in_megabytes AS DatabaseDataSpaceUsedInMB
FROM sys.resource_stats
WHERE database_name = 'db1'
ORDER BY end_time DESC
```

### <a name="database-data-allocated-and-allocated-space-unused"></a>Dados de base de dados alocados e alocado espaço não utilizado
Modifique a consulta seguinte para devolver a quantidade de dados de base de dados alocados e alocado espaço não utilizado.

```sql
-- Connect to database
-- Database data space allocated in MB and database data space allocated unused in MB
SELECT SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB, 
SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB 
FROM sys.database_files
GROUP BY type_desc
HAVING type_desc = 'ROWS'
```
 
### <a name="database-max-size"></a>Tamanho máximo da base de dados
Modifique a consulta seguinte para devolver o tamanho máximo da base de dados em bytes.

```sql
-- Connect to database
-- Database data max size in bytes
SELECT DATABASEPROPERTYEX('db1', 'MaxSizeInBytes') AS DatabaseDataMaxSizeInBytes
```

## <a name="query-an-elastic-pool-for-storage-space-information"></a>Consultar um conjunto elástico para obter informações de espaço de armazenamento

Para determinar se alocou, mas não utilizado com o espaço de dados num conjunto elástico e cada base de dados agrupadas que pode desejar recuperar, utilize as seguintes consultas:

### <a name="elastic-pool-data-space-used"></a>Espaço de dados do conjunto elástico utilizado
Modifique a consulta seguinte para devolver a quantidade de espaço de dados do conjunto elástico utilizado em MB.

```sql
-- Connect to master
-- Elastic pool data space used in MB  
SELECT TOP 1 avg_storage_percent * elastic_pool_storage_limit_mb AS ElasticPoolDataSpaceUsedInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

### <a name="elastic-pool-data-allocated-and-allocated-space-unused"></a>Dados do conjunto elástico alocados e alocado espaço não utilizado

Modifique o seguinte script do PowerShell para devolver uma tabela que lista o total de espaço alocada e não utilizado espaço alocado para cada base de dados num conjunto elástico. A tabela orders bases de dados de aqueles com a maior quantidade de espaço alocado não utilizados para a quantidade de espaço alocado não utilizados.  

Os resultados da consulta para determinar o espaço alocado para cada base de dados no agrupamento podem ser adicionados em conjunto para determinar o conjunto elástico espaço alocado. O espaço do conjunto elástico alocado não deve exceder o tamanho máximo do conjunto elástico.  

```powershell
# Resource group name
$resourceGroupName = "rg1" 
# Server name
$serverName = "ls2" 
# Elastic pool name
$poolName = "ep1"
# User name for server
$userName = "name"
# Password for server
$password = "password"

# Get list of databases in elastic pool
$databasesInPool = Get-AzureRmSqlElasticPoolDatabase `
    -ResourceGroupName $resourceGroupName `
    -ServerName $serverName `
    -ElasticPoolName $poolName
$databaseStorageMetrics = @()

# For each database in the elastic pool,
# get its space allocated in MB and space allocated unused in MB.
# Requires SQL Server PowerShell module – see here to install.  
foreach ($database in $databasesInPool)
{
    $sqlCommand = "SELECT DB_NAME() as DatabaseName, `
    SUM(size/128.0) AS DatabaseDataSpaceAllocatedInMB, `
    SUM(size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0) AS DatabaseDataSpaceAllocatedUnusedInMB `
    FROM sys.database_files `
    GROUP BY type_desc `
    HAVING type_desc = 'ROWS'"
    $serverFqdn = "tcp:" + $serverName + ".database.windows.net,1433"
    $databaseStorageMetrics = $databaseStorageMetrics + 
        (Invoke-Sqlcmd -ServerInstance $serverFqdn `
        -Database $database.DatabaseName `
        -Username $userName `
        -Password $password `
        -Query $sqlCommand)
}
# Display databases in descending order of space allocated unused
Write-Output "`n" "ElasticPoolName: $poolName"
Write-Output $databaseStorageMetrics | Sort `
    -Property DatabaseDataSpaceAllocatedUnusedInMB `
    -Descending | Format-Table
```

Captura de ecrã seguinte é um exemplo da saída do script:

![conjunto elástico alocado espaço e exemplo de espaço alocado não utilizado](./media/sql-database-file-space-management/elastic-pool-allocated-unused.png)

### <a name="elastic-pool-max-size"></a>Tamanho máximo do conjunto elástico

Utilize a seguinte consulta de T-SQL para retornar o tamanho máximo de bases de dados elásticas em MB.

```sql
-- Connect to master
-- Elastic pools max size in MB
SELECT TOP 1 elastic_pool_storage_limit_mb AS ElasticPoolMaxSizeInMB
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = 'ep1'
ORDER BY end_time DESC
```

## <a name="reclaim-unused-allocated-space"></a>Recuperar espaço alocado não utilizado

Depois de ter determinado que tem espaço alocado não utilizado que deseja recuperar, utilize o seguinte comando para reduzir o espaço de base de dados atribuído. 

> [!IMPORTANT]
> Bases de dados num conjunto elástico, bases de dados com mais espaço alocado não utilizado deve ser encolhido primeiro para recuperar o espaço de ficheiro mais rapidamente.  

Utilize o seguinte comando para reduzir todos os ficheiros de dados na base de dados especificado:

```sql
-- Shrink database data space allocated.
DBCC SHRINKDATABASE (N'<database_name>')
```

Para obter mais informações sobre este comando, consulte [SHRINKDATABASE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql).

> [!IMPORTANT] 
> Considere a reconstrução índices de base de dados depois de arquivos de dados do banco de dados são Encolher, índices podem se tornar fragmentados e perdem a eficácia de otimização de desempenho. Se isto ocorrer, os índices devem ser reconstruídos. Para obter mais informações sobre a fragmentação e reconstruir índices, consulte [Reorganize e reconstruir índices](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).

## <a name="next-steps"></a>Passos Seguintes

- Para informações sobre os tamanhos de base de dados máximo, consulte:
  - [Base de dados do SQL do Azure limites de modelo para uma base de dados de compra baseado em vCore](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-single-databases)
  - [Limites de recursos para bases de dados individuais com o modelo de compra baseado em DTU](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits-single-databases)
  - [Base de dados do SQL do Azure limites de modelo para conjuntos elásticos de compra baseado em vCore](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools)
  - [Limites de recursos para conjuntos elásticos com o modelo de compra baseado em DTU](https://docs.microsoft.com/azure/sql-database/sql-database-dtu-resource-limits-elastic-pools)
- Para obter mais informações sobre o `SHRINKDATABASE` comando este problema, consulte [SHRINKDATABASE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql). 
- Para obter mais informações sobre a fragmentação e reconstruir índices, consulte [Reorganize e reconstruir índices](https://docs.microsoft.com/sql/relational-databases/indexes/reorganize-and-rebuild-indexes).