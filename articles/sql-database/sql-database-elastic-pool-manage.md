---
title: Criar e gerir conjuntos elásticos - base de dados SQL do Azure | Microsoft Docs
description: Criar e gerir conjuntos elásticos SQL do Azure.
keywords: várias bases de dados, os recursos de base de dados e de desempenho de base de dados
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.date: 06/20/2018
ms.author: ninarn
ms.topic: conceptual
ms.reviewer: carlrab
ms.openlocfilehash: 3cdc82d71c05298aa5822b87c22edcc5d8e73385
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313326"
---
# <a name="create-and-manage-elastic-pools-in-azure-sql-database"></a>Criar e gerir conjuntos elásticos na SQL Database do Azure

Com um conjunto elástico, determinar a quantidade de recursos que o conjunto elástico necessita para processar a carga de trabalho das suas bases de dados e a quantidade de recursos para cada base de dados agrupado. 

## <a name="azure-portal-manage-elastic-pools-and-pooled-databases"></a>Portal do Azure: Gerir conjuntos elásticos e bases de dados agrupados

Todas as definições de agrupamento podem ser encontradas no mesmo local: o **configurar conjunto** painel. Para obter aqui, localizar um conjunto elástico no portal e clique **configurar conjunto** da parte superior do painel ou no menu de recursos, à esquerda.

Aqui pode efetuar qualquer combinação das seguintes alterações e guarde-los todos num batch:
1. Alterar o escalão de serviço do conjunto
2. Aumentar o desempenho (DTU ou vCores) e armazenamento ou para baixo
3. Adicionar ou remover bases de dados do agrupamento
4. Definir um mínimo (garantido) e máximo de limite de desempenho para os conjuntos de bases de dados
5. Reveja o resumo de custos para ver as alterações a fatura como resultado as suas seleções novo

![Painel de configuração do conjunto elástico](./media/sql-database-elastic-pool-manage-portal/configure-pool.png)

## <a name="powershell-manage-elastic-pools-and-pooled-databases"></a>PowerShell: Gerir conjuntos elásticos e bases de dados agrupados 

Para criar e gerir conjuntos elásticos SQL Database e agrupados bases de dados com o Azure PowerShell, utilize os seguintes cmdlets PowerShell. Se precisar de instalar ou atualizar o PowerShell, consulte [módulo Azure PowerShell instalar](/powershell/azure/install-azurerm-ps). Para criar e gerir bases de dados, servidores e as regras de firewall, consulte [criar e gerir servidores SQL Database do Azure e as bases de dados com o PowerShell](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-powershell).

> [!TIP]
> Para scripts de exemplo do PowerShell, consulte [criar conjuntos elásticos e mover bases de dados entre conjuntos e fora de um conjunto com o PowerShell](scripts/sql-database-move-database-between-pools-powershell.md) e [utilize o PowerShell para monitorizar e dimensionar um agrupamento elástico de SQL na base de dados do Azure SQL](scripts/sql-database-monitor-and-scale-pool-powershell.md).
>

| Cmdlet | Descrição |
| --- | --- |
|[New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool)|Cria um conjunto de bases de dados elásticas num servidor lógico SQL.|
|[Get-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/get-azurermsqlelasticpool)|Obtém os conjuntos elásticos e os respetivos valores de propriedade num servidor SQL lógico.|
|[Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool)|Modifica as propriedades de um conjunto de bases de dados elásticas num servidor lógico SQL. Por exemplo, utilizar o **StorageMB** propriedade para modificar o armazenamento máximo do conjunto elástico.|
|[Remove-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/remove-azurermsqlelasticpool)|Elimina um conjunto de bases de dados elásticas num servidor lógico SQL.|
|[Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity)|Obtém o estado das operações num conjunto elástico num servidor SQL lógico.|
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Cria uma nova base de dados num conjunto existente ou como uma base de dados. |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Obtém uma ou mais bases de dados.|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Define as propriedades para uma base de dados ou move de uma base de dados para fora do ou entre conjuntos elásticos.|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Remove uma base de dados.|


> [!TIP]
> Criação de muitas bases de dados num agrupamento elástico pode demorar concluída ao utilizar o portal ou cmdlets do PowerShell que criam apenas uma única base de dados de cada vez. Para automatizar a criação de para um conjunto elástico, consulte o artigo [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).
>

## <a name="azure-cli-manage-elastic-pools-and-pooled-databases"></a>CLI do Azure: Gerir conjuntos elásticos e bases de dados agrupados

Para criar e gerir conjuntos elásticos da base de dados SQL com o [CLI do Azure](/cli/azure), utilize o seguinte [SQL Database do Azure CLI](/cli/azure/sql/db) comandos. Utilize o [Cloud Shell](/azure/cloud-shell/overview) para executar a CLI no seu browser ou [instale-a](/cli/azure/install-azure-cli) no macOS, Linux ou Windows.

> [!TIP]
> Para scripts de exemplo da CLI do Azure, consulte [CLI de utilização para mover uma base de dados SQL do Azure num agrupamento elástico de SQL](scripts/sql-database-move-database-between-pools-cli.md) e [CLI do Azure de utilização para dimensionar um agrupamento elástico de SQL na base de dados do Azure SQL](scripts/sql-database-scale-pool-cli.md).
>

| Cmdlet | Descrição |
| --- | --- |
|[az sql elastic-pool create](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_create)|Cria um conjunto elástico.|
|[az sql elastic-pool list](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list)|Devolve uma lista de conjuntos elásticos num servidor.|
|[az sql elastic-pool list-dbs](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_dbs)|Devolve uma lista de bases de dados num agrupamento elástico.|
|[az sql elastic-pool list-editions](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_list_editions)|Também inclui definições de DTU do agrupamento disponíveis, os limites de armazenamento e por definições de base de dados. Para reduzir verbosidade, limites de armazenamento adicional e por base de dados, as definições estão ocultadas por predefinição.|
|[az sql elastic-pool update](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update)|As atualizações de um conjunto elástico.|
|[az sql elastic-pool delete](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_delete)|Elimina o conjunto elástico.|

## <a name="transact-sql-manage-pooled-databases"></a>Transact-SQL: Gerir bases de dados agrupados

Para criar e mover bases de dados dentro de conjuntos elásticos existentes ou para devolver informações sobre um conjunto elástico da base de dados SQL com o Transact-SQL, utilize os seguintes comandos de T-SQL. Pode emitir estes comandos no portal do Azure, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), ou qualquer outro programa que pode ligar a um servidor da SQL Database do Azure e passar comandos Transact-SQL. Para criar e gerir bases de dados, servidores e as regras de firewall, consulte [criar e gerir servidores SQL Database do Azure e as bases de dados com o Transact-SQL](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-transact-sql).

> [!IMPORTANT]
> Não é possível criar, atualizar ou eliminar um agrupamento elástico de SQL Database do Azure com o Transact-SQL. Pode adicionar ou remover bases de dados a partir de um conjunto elástico e pode utilizar DMVs para devolver informações sobre conjuntos elásticos existentes.
>

| Comando | Descrição |
| --- | --- |
|[Criar base de dados (SQL Database do Azure)](/sql/t-sql/statements/create-database-azure-sql-database)|Cria uma nova base de dados num conjunto existente ou como uma base de dados. Tem de estar ligado à base de dados principal para criar uma nova base de dados.|
| [Falha de ALTER DATABASE (base de dados SQL do Azure)](/sql/t-sql/statements/alter-database-azure-sql-database) |Mova uma base de dados para, de ou entre conjuntos elásticos.|
|[REMOVER a base de dados (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Elimina uma base de dados.|
|[sys.elastic_pool_resource_stats (SQL Database do Azure)](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)|Devolve estatísticas de utilização de recursos para todos os conjuntos de bases de dados elásticas um servidor lógico. Para cada conjunto de bases de dados elásticas, não há uma linha para cada segundo 15 reporting janela (quatro linhas por minuto). Isto inclui a CPU, e/s, registo, o consumo de armazenamento e utilização de pedido/sessão em simultâneo por todas as bases de dados no conjunto.|
|[sys.database_service_objectives (SQL Database do Azure)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Devolve o edition (camada de serviço), o objetivo de serviço (escalão de preço) e o nome do conjunto elástico, se existir, para uma base de dados SQL do Azure ou um Azure SQL Data Warehouse. Se tem sessão iniciada base de dados mestra num servidor de base de dados do Azure SQL, devolve informações sobre todas as bases de dados. Para o Azure SQL Data Warehouse, tem de estar ligado à base de dados mestra.|

## <a name="rest-api-manage-elastic-pools-and-pooled-databases"></a>API de REST: Gerir conjuntos elásticos e bases de dados agrupados

Para criar e gerir conjuntos elásticos SQL Database e bases de dados agrupados, utilize estes pedidos de REST API.

| Comando | Descrição |
| --- | --- |
|[Conjuntos elásticos - criar ou atualizar](/rest/api/sql/elasticpools/createorupdate)|Cria um novo conjunto elástico ou atualiza um conjunto elástico existente.|
|[Eliminar conjuntos elásticos-](/rest/api/sql/elasticpools/delete)|Elimina o conjunto elástico.|
|[Obter conjuntos elásticos-](/rest/api/sql/elasticpools/get)|Obtém um conjunto elástico.|
|[Conjuntos elásticos - lista pelo servidor](/rest/api/sql/elasticpools/listbyserver)|Devolve uma lista de conjuntos elásticos num servidor.|
|[Conjuntos elásticos - atualização](/rest/api/sql/elasticpools/update)|As atualizações de um conjunto elástico existente.|
|[Recomenda conjuntos elásticos - Get](/rest/api/sql/recommendedelasticpools/get)|Obtém um conjunto elástico recomendado.|
|[Recomenda conjuntos elásticos - lista pelo servidor](/rest/api/sql/recommendedelasticpools/listbyserver)|Devolve os conjuntos elásticos recomendados.|
|[Recomenda conjuntos elásticos - as métricas de lista](/rest/api/sql/recommendedelasticpools/listmetrics)|Devolve recomendado métricas do conjunto elástico.|
|[Atividades do conjunto elástico](/rest/api/sql/elasticpoolactivities)|Devolve as atividades do conjunto elástico.|
|[Atividades de base de dados do conjunto elástico](/rest/api/sql/elasticpooldatabaseactivities)|Devolve a atividade em bases de dados dentro de um conjunto elástico.|
|[Bases de dados - criar ou atualizar](/rest/api/sql/databases/createorupdate)|Cria uma nova base de dados ou atualiza uma base de dados existente.|
|[Get - bases de dados](/rest/api/sql/databases/get)|Obtém uma base de dados.|
|[Para obter agrupamento elástico de bases de dados-](/rest/api/sql/databases/getbyelasticpool)|Obtém uma base de dados dentro de um conjunto elástico.|
|[Para obter as bases de dados - recomendado conjunto elástico](/rest/api/sql/databases/getbyrecommendedelasticpool)|Obtém uma base de dados dentro de um conjunto elástico recomendado.|
|[Bases de dados - lista por agrupamento elástico](/rest/api/sql/databases/listbyelasticpool)|Devolve uma lista de bases de dados num agrupamento elástico.|
|[Bases de dados - lista por conjunto elástico recomendado](/rest/api/sql/databases/listbyrecommendedelasticpool)|Devolve uma lista de bases de dados dentro de um conjunto elástico recomendado.|
|[Bases de dados - lista pelo servidor](/rest/api/sql/databases/listbyserver)|Devolve uma lista de bases de dados num servidor.|
|[Bases de dados - atualização](/rest/api/sql/databases/update)|Atualiza a base de dados existente.|

## <a name="next-steps"></a>Passos Seguintes

* Para obter um vídeo, consulte [decorrer vídeo do Microsoft Virtual Academy nas capacidades de elásticas de SQL Database do Azure](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
* Para saber mais sobre os padrões de estrutura de aplicações SaaS que utilizam conjuntos elásticos, consulte o artigo [Padrões de Estrutura de Aplicações SaaS Multi-inquilino com a Base de Dados SQL do Azure](sql-database-design-patterns-multi-tenancy-saas-applications.md).
* Para um tutorial de SaaS que utilizam conjuntos elásticos, consulte [introdução à aplicação Wingtip SaaS](sql-database-wtp-overview.md).
