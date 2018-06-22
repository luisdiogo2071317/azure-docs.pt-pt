---
title: Criar, gerir servidores SQL do Azure e bases de dados individuais | Microsoft Docs
description: Saiba mais sobre como criar e gerir servidores lógicos e bases de dados individuais.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 918cfd0257c82e84451e07ef904dbda331f47b95
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313312"
---
# <a name="create-and-manage-logical-servers-and-single-databases-in-azure-sql-database"></a>Criar e gerir servidores lógicos e bases de dados individuais na SQL Database do Azure 

Pode criar e gerir servidores de bases de dados do Azure SQL lógicos e bases de dados individuais utilizando o portal do Azure, PowerShell, CLI do Azure, REST API e Transact-SQL.

## <a name="azure-portal-manage-logical-servers-and-databases"></a>Portal do Azure: Gerir lógicas servidores e bases de dados

Pode criar o grupo de recursos da base de dados SQL do Azure antecedência ou ao criar o próprio servidor. Existem vários métodos para obter a um novo formulário de servidor do SQL Server, ao criar um novo SQL server ou como parte da criação de uma nova base de dados. 

### <a name="create-a-blank-sql-server-logical-server"></a>Criar um SQL server em branco (servidor lógico)

Para criar um servidor (sem uma base de dados) de SQL Database do Azure utilizando o [portal do Azure](https://portal.azure.com), navegue para um formulário de servidor (servidor lógico) do SQL Server em branco.  

### <a name="create-a-blank-or-sample-sql-database"></a>Criar uma base de dados do SQL Server em branco ou de exemplo

Para criar uma base de dados SQL do Azure utilizando o [portal do Azure](https://portal.azure.com), navegue para um formulário de base de dados do SQL Server em branco e forneça as informações pedidas. Pode criar o grupo de recursos e servidor lógico de antecedência ou ao criar a base de dados da base de dados SQL do Azure. Pode criar uma base de dados em branco ou criar uma base de dados de exemplo com base no Adventure Works LT. 

  ![criar base de dados-1](./media/sql-database-get-started-portal/create-database-1.png)

> [!IMPORTANT]
> Para informações sobre o escalão de preço da base de dados, consulte [DTU com base no modelo de compra](sql-database-service-tiers-dtu.md) e [vCore com base no modelo de compra (pré-visualização)](sql-database-service-tiers-vcore.md).

Para criar uma instância geridos, consulte [criar uma instância geridos](sql-database-managed-instance-create-tutorial-portal.md)

### <a name="manage-an-existing-sql-server"></a>Gerir um servidor existente do SQL Server

Para gerir um servidor existente, navegue para o servidor utilizando um número de métodos -, tais como a página de base de dados SQL específica, o **servidores SQL** página, ou o **todos os recursos** página. 

Para gerir uma base de dados existente, navegue para o **bases de dados SQL** página e clique na base de dados que pretende gerir. Captura de ecrã seguinte mostra como começar a definição de uma firewall ao nível do servidor de bases de dados do **descrição geral** página para uma base de dados. 

   ![regra de firewall do servidor](./media/sql-database-get-started-portal/server-firewall-rule.png) 

> [!IMPORTANT]
> Para configurar propriedades de desempenho para uma base de dados, consulte [DTU com base no modelo de compra](sql-database-service-tiers-dtu.md) e [vCore com base no modelo de compra (pré-visualização)](sql-database-service-tiers-vcore.md).
>

> [!TIP]
> Para um guia de introdução portal do Azure, consulte [criar uma base de dados SQL do Azure no portal do Azure](sql-database-get-started-portal.md).

## <a name="powershell-manage-logical-servers-and-databases"></a>PowerShell: Gerir lógicas servidores e bases de dados

Para criar e gerir o servidor SQL do Azure, as bases de dados e firewalls com o Azure PowerShell, utilize os seguintes cmdlets PowerShell. Se precisar de instalar ou atualizar o PowerShell, consulte [módulo Azure PowerShell instalar](/powershell/azure/install-azurerm-ps). 

> [!TIP]
> Para um guia de introdução do PowerShell, consulte [criar uma base de dados SQL do Azure único através do PowerShell](sql-database-get-started-portal.md). Para scripts de exemplo do PowerShell, consulte [utilize o PowerShell para criar uma base de dados SQL do Azure e configurar uma regra de firewall](scripts/sql-database-create-and-configure-database-powershell.md) e [monitorizar e dimensionar um único SQL da base de dados com o PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).
>

| Cmdlet | Descrição |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Cria uma base de dados |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Obtém um ou mais bases de dados|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Define as propriedades para uma base de dados ou move de uma base de dados existente para um conjunto elástico|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Remove uma base de dados|
|[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)|Cria um grupo de recursos|
|[New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver)|Cria um servidor|
|[Get-AzureRmSqlServer](/powershell/module/azurerm.sql/get-azurermsqlserver)|Devolve informações sobre servidores|
|[Set-AzureRmSqlServer](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqlserver)|Modifica as propriedades de um servidor|
|[Remove-AzureRmSqlServer](/powershell/module/azurerm.sql/remove-azurermsqlserver)|Remove um servidor|
|[New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule)|Cria uma regra de firewall ao nível do servidor |
|[Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule)|Obtém as regras de firewall para um servidor|
|[Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule)|Modifica uma regra de firewall num servidor|
|[Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule)|Elimina uma regra de firewall de um servidor.|
| New-AzureRmSqlServerVirtualNetworkRule | Cria um [ *regra de rede virtual*](sql-database-vnet-service-endpoint-rule-overview.md), com base numa sub-rede que é um ponto final de serviço de rede Virtual. |

## <a name="azure-cli-manage-logical-servers-and-databases"></a>CLI do Azure: Gerir lógicas servidores e bases de dados

Para criar e gerir o servidor SQL do Azure, as bases de dados e firewalls com [CLI do Azure](/cli/azure), utilize o seguinte [SQL Database do Azure CLI](/cli/azure/sql/db) comandos. Utilize o [Cloud Shell](/azure/cloud-shell/overview) para executar a CLI no seu browser ou [instale-a](/cli/azure/install-azure-cli) no macOS, Linux ou Windows. Para criar e gerir conjuntos elásticos, consulte [conjuntos elásticos](sql-database-elastic-pool.md).

> [!TIP]
> Para um guia de introdução do CLI do Azure, consulte [criar uma base de dados SQL do Azure único utilizando a CLI do Azure](sql-database-get-started-cli.md). Para scripts de exemplo da CLI do Azure, consulte [CLI de utilização para criar uma base de dados SQL do Azure e configurar uma regra de firewall](scripts/sql-database-create-and-configure-database-cli.md) e [CLI de utilização para monitorizar e dimensionar uma base de dados do SQL Server único](scripts/sql-database-monitor-and-scale-database-cli.md).
>

| Cmdlet | Descrição |
| --- | --- |
|[az sql db create](/cli/azure/sql/db#az_sql_db_create) |Cria uma base de dados|
|[az sql db list](/cli/azure/sql/db#az_sql_db_list)|Apresenta uma lista de todas as bases de dados e os armazéns de dados num servidor, ou todas as bases de dados num agrupamento elástico|
|[az sql db list-editions](/cli/azure/sql/db#az_sql_db_list_editions)|Apresenta uma lista de serviços disponíveis objetivos e limites de armazenamento|
|[az sql db list-usages](/cli/azure/sql/db#az_sql_db_list_usages)|Devolve as utilizações de base de dados|
|[Mostrar de BD do SQL Server AZ](/cli/azure/sql/db#az_sql_db_show)|Obtém um armazém de dados ou base de dados|
|[az sql db update](/cli/azure/sql/db#az_sql_db_update)|Atualizações de uma base de dados|
|[az sql db delete](/cli/azure/sql/db#az_sql_db_delete)|Remove uma base de dados|
|[az group create](/cli/azure/group#az_group_create)|Cria um grupo de recursos|
|[az sql server create](/cli/azure/sql/server#az_sql_server_create)|Cria um servidor|
|[az sql server list](/cli/azure/sql/server#az_sql_server_list)|Apresenta uma lista de servidores|
|[az sql server list-usages](/cli/azure/sql/server#az_sql_server_list_usages)|Devolve as utilizações de servidor|
|[Mostrar do AZ sql server](/cli/azure/sql/server#az_sql_server_show)|Obtém um servidor|
|[az sql server update](/cli/azure/sql/server#az_sql_server_update)|Um servidor de atualizações|
|[eliminação do AZ sql server](/cli/azure/sql/server#az_sql_server_delete)|Elimina um servidor|
|[Criar AZ sql server-regra de firewall](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create)|Cria uma regra de firewall do servidor|
|[lista de regras de firewall de servidor de sql de AZ](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_list)|Apresenta as regras de firewall num servidor|
|[Mostrar de regra de firewall AZ sql server](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_show)|Mostra os detalhes de uma regra de firewall|
|[atualização de regra de firewall AZ sql server](/cli/azure/sql/server/firewall-rule##az_sql_server_firewall_rule_update)|Uma regra de firewall de atualizações|
|[Eliminar regra de firewall do AZ sql server](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_delete)|Elimina uma regra de firewall|

## <a name="transact-sql-manage-logical-servers-and-databases"></a>Transact-SQL: Gerir lógicas servidores e bases de dados

Para criar e gerir o servidor SQL do Azure, as bases de dados e firewalls com Transact-SQL, utilize os seguintes comandos de T-SQL. Pode emitir estes comandos no portal do Azure, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), ou qualquer outro programa que pode ligar a um servidor da SQL Database do Azure e passar comandos Transact-SQL. Para gerir conjuntos elásticos, consulte [conjuntos elásticos](sql-database-elastic-pool.md).


> [!TIP]
> Para um início rápido utilizando o SQL Server Management Studio no Microsoft Windows, consulte [SQL Database do Azure: Utilize o SQL Server Management Studio para ligar e consultar dados](sql-database-connect-query-ssms.md). Para um início rápido utilizando o Visual Studio Code no macOS, Linux ou Windows, consulte [SQL Database do Azure: utilize Visual Studio Code para ligar e consultar dados](sql-database-connect-query-vscode.md).

> [!IMPORTANT]
> Não é possível criar ou eliminar um servidor com o Transact-SQL.
>

| Comando | Descrição |
| --- | --- |
|[Criar base de dados (SQL Database do Azure)](/sql/t-sql/statements/create-database-azure-sql-database)|Cria uma nova base de dados. Tem de estar ligado à base de dados principal para criar uma nova base de dados.|
| [Falha de ALTER DATABASE (base de dados SQL do Azure)](/sql/t-sql/statements/alter-database-azure-sql-database) |Modifica uma base de dados SQL do Azure. |
|[Alterar base de dados (armazém de dados SQL do Azure)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse)|Modifica um armazém de dados SQL do Azure.|
|[REMOVER a base de dados (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Elimina uma base de dados.|
|[sys.database_service_objectives (SQL Database do Azure)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Devolve o edition (camada de serviço), o objetivo de serviço (escalão de preço) e o nome do conjunto elástico, se existir, para uma base de dados SQL do Azure ou um Azure SQL Data Warehouse. Se tem sessão iniciada base de dados mestra num servidor de base de dados do Azure SQL, devolve informações sobre todas as bases de dados. Para o Azure SQL Data Warehouse, tem de estar ligado à base de dados mestra.|
|[sys.dm db_resource_stats (SQL Database do Azure)](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)| Devolve o consumo de CPU, memória e e/s, para uma base de dados do SQL Database do Azure. Não existe uma linha para cada 15 segundos, mesmo se não houver nenhuma atividade na base de dados.|
|[resource_stats (SQL Database do Azure)](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)|Devolve os dados de armazenamento e utilização de CPU para uma base de dados do SQL do Azure. Os dados são recolhidos e agregados em intervalos de cinco minutos.|
|[sys.database_connection_stats (SQL Database do Azure)](/sql/relational-databases/system-catalog-views/sys-database-connection-stats-azure-sql-database)|Contém as estatísticas de eventos de conectividade da base de dados de base de dados do SQL Server, que fornece uma descrição geral da base de dados ligação sucessos e falhas. |
|[sys.event_log (SQL Database do Azure)](/sql/relational-databases/system-catalog-views/sys-event-log-azure-sql-database)|Devolve as ligações de base de dados de SQL Database do Azure com êxito, falhas de ligação e impasses. Pode utilizar estas informações para controlar ou resolver problemas relacionados com a atividade de base de dados com base de dados do SQL Server.|
|[sp_set_firewall_rule (SQL Database do Azure)](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database)|Cria ou atualiza as definições de firewall ao nível do servidor para o servidor de base de dados SQL. Este procedimento armazenado só está disponível na base de dados mestra para o início de sessão principal ao nível do servidor. Só é possível criar uma regra de firewall ao nível do servidor com o Transact-SQL após a primeira regra de firewall ao nível do servidor foi criada por um utilizador com permissões ao nível do Azure|
|[sys.firewall_rules (SQL Database do Azure)](/sql/relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database)|Devolve informações sobre as definições de firewall ao nível do servidor associado à sua base de dados de SQL do Microsoft Azure.|
|[sp_delete_firewall_rule (SQL Database do Azure)](/sql/relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database)|Remove as definições de firewall ao nível do servidor do seu servidor de base de dados SQL. Este procedimento armazenado só está disponível na base de dados mestra para o início de sessão principal ao nível do servidor.|
|[sp_set_database_firewall_rule (SQL Database do Azure)](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)|Cria ou atualiza as regras de firewall ao nível da base de dados para o seu SQL Database do Azure ou SQL Data Warehouse. Regras de firewall de base de dados podem ser configuradas para a base de dados mestra e para as bases de dados de utilizador na base de dados do SQL Server. Regras de firewall de base de dados são úteis quando utilizar continha os utilizadores de base de dados. |
|[sys.database_firewall_rules (SQL Database do Azure)](/sql/relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database)|Devolve informações sobre as definições de firewall ao nível da base de dados associado à sua base de dados de SQL do Microsoft Azure. |
|[sp_delete_database_firewall_rule (SQL Database do Azure)](/sql/relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database)|Remove a definição de firewall ao nível da base de dados do seu SQL Database do Azure ou SQL Data Warehouse. |



## <a name="rest-api-manage-logical-servers-and-databases"></a>API de REST: Gerir lógicas servidores e bases de dados

Para criar e gerir o servidor SQL do Azure, as bases de dados e as firewalls, utilize estes pedidos de REST API.

| Comando | Descrição |
| --- | --- |
|[Servidores – criar ou atualizar](/rest/api/sql/servers/createorupdate)|Cria ou atualiza um novo servidor.|
|[Servidores - eliminar](/rest/api/sql/servers/delete)|Elimina um SQL server.|
|[Servidores - Get](/rest/api/sql/servers/get)|Obtém um servidor.|
|[Servidores - lista](/rest/api/sql/servers/list)|Devolve uma lista de servidores.|
|[Servidores - lista por grupo de recursos](/rest/api/sql/servers/listbyresourcegroup)|Devolve uma lista de servidores num grupo de recursos.|
|[Servidores - atualização](/rest/api/sql/servers/update)|As atualizações de um servidor existente.|
|[Bases de dados - criar ou atualizar](/rest/api/sql/databases/createorupdate)|Cria uma nova base de dados ou atualiza uma base de dados existente.|
|[Get - bases de dados](/rest/api/sql/databases/get)|Obtém uma base de dados.|
|[Para obter agrupamento elástico de bases de dados-](/rest/api/sql/databases/getbyelasticpool)|Obtém uma base de dados dentro de um conjunto elástico.|
|[Para obter as bases de dados - recomendado conjunto elástico](/rest/api/sql/databases/getbyrecommendedelasticpool)|Obtém uma base de dados dentro de um conjunto elástico recommented.|
|[Bases de dados - lista por agrupamento elástico](/rest/api/sql/databases/listbyelasticpool)|Devolve uma lista de bases de dados num agrupamento elástico.|
|[Bases de dados - lista por conjunto elástico recomendado](/rest/api/sql/databases/listbyrecommendedelasticpool)|Devolve uma lista de bases de dados dentro de um conjunto elástico recomendado.|
|[Bases de dados - lista pelo servidor](/rest/api/sql/databases/listbyserver)|Devolve uma lista de bases de dados num servidor.|
|[Bases de dados - atualização](/rest/api/sql/databases/update)|Atualiza a base de dados existente.|
|[Firewall regras - criar ou atualizar](/rest/api/sql/firewallrules/createorupdate)|Cria ou atualiza uma regra de firewall.|
|[Regras de firewall - eliminar](/rest/api/sql/firewallrules/delete)|Elimina uma regra de firewall.|
|[Regras de firewall - Get](/rest/api/sql/firewallrules/get)|Obtém uma regra de firewall.|
|[Regras de firewall - lista pelo servidor](/rest/api/sql/firewallrules/listbyserver)|Devolve uma lista de regras de firewall.|

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre como migrar uma base de dados do SQL Server para o Azure, consulte [migrar para a SQL Database do Azure](sql-database-cloud-migrate.md).
- Para obter informações sobre as funcionalidades suportadas, veja [Funcionalidades](sql-database-features.md).
