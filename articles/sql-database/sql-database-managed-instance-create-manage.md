---
title: Referência de API de gestão para a instância gerida da base de dados SQL do Azure | Documentos da Microsoft
description: Saiba mais sobre criar e gerir instâncias geridas da base de dados SQL do Azure.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 01/03/2019
ms.openlocfilehash: 30efbcd5ba36ea912a7238eef9e300085cb1d3d1
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54040205"
---
# <a name="managed-api-reference-for-azure-sql-database-managed-instances"></a>Referência da API gerida para instâncias geridas da base de dados SQL do Azure

Pode criar e gerenciar instâncias do Azure SQL da base de dados geridos com o portal do Azure, PowerShell, CLI do Azure, REST API e Transact-SQL. Neste artigo, pode encontrar uma descrição geral das funções e API que pode utilizar para criar e configurar a instância gerida.

## <a name="azure-portal-create-a-managed-instance"></a>Portal do Azure: Criar uma Instância Gerida

Para um início rápido mostra-lhe como criar uma instância de gerida de base de dados do Azure SQL, consulte [início rápido: Criar uma instância gerida de base de dados SQL do Azure](sql-database-managed-instance-get-started.md).

## <a name="powershell-create-and-manage-a-managed-instance"></a>PowerShell: Criar e gerir uma instância gerida

Para criar e gerir o servidor SQL do Azure, bases de dados e firewalls com o Azure PowerShell, utilize os seguintes cmdlets do PowerShell. Se precisar de instalar ou atualizar o PowerShell, veja [módulo de instalar o Azure PowerShell](/powershell/azure/install-azurerm-ps).

> [!TIP]
> Para scripts de exemplo do PowerShell, consulte [script de início rápido: Criar instância de gerida de SQL em Azure usando a biblioteca do PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/).

| Cmdlet | Descrição |
| --- | --- |
|[Novo AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlinstance)|Cria uma instância gerida de base de dados SQL do Azure |
|[Get-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlinstance)|Devolve informações sobre a instância gerida do SQL do Azure|
|[Conjunto AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/set-azurermsqlinstance)|Define as propriedades para uma instância de gerida de base de dados do Azure SQL|
|[Remove-AzureRmSqlInstance](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqlinstance)|Remove uma instância de base de dados SQL do Azure|
|[Novo AzureRmSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqlinstancedatabase)|Cria uma base de dados de instância gerida da base de dados SQL do Azure|
|[Get-AzureRmSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlinstancedatabase)|Devolve informações sobre a base de dados de instância gerida do SQL do Azure|
|[Remove-AzureRmSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/remove-azurermsqlinstancedatabase)|Remove uma base de dados de instância de base de dados gerida do Azure SQL|
|[Restauro-AzureRmSqlInstanceDatabase](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqlinstancedatabase)|Restaura uma base de dados de instância de base de dados gerida do Azure SQL|

## <a name="azure-cli-manage-logical-servers-and-databases"></a>CLI do Azure: Gerir servidores lógicos e bases de dados

Para criar e gerir o servidor SQL do Azure, bases de dados e firewalls com [CLI do Azure](/cli/azure), utilize o seguinte procedimento [instância gerida SQL da CLI do Azure](/cli/azure/sql/mi) comandos. Utilize o [Cloud Shell](/azure/cloud-shell/overview) para executar a CLI no seu browser ou [instale-a](/cli/azure/install-azure-cli) no macOS, Linux ou Windows.

> [!TIP]
> Para um guia de introdução do CLI do Azure, consulte [trabalhar com a instância gerida de SQL com a CLI do Azure](https://medium.com/azure-sqldb-managed-instance/working-with-sql-managed-instance-using-azure-cli-611795fe0b44).

| Cmdlet | Descrição |
| --- | --- |
|[Criar AZ sql mi](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-create) |Cria uma instância gerida|
|[lista de mi AZ sql](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-list)|Listas de instâncias de geridas disponíveis|
|[show de mi AZ sql](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-show)|Obtenha os detalhes para uma instância gerida|
|[atualização de mi AZ sql](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-update)|Atualizações de uma instância gerida|
|[eliminação de mi AZ sql](https://docs.microsoft.com/cli/azure/sql/mi#az-sql-mi-delete)|Remove uma instância gerida|
|[Criar AZ sql midb](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-create) |Cria uma base de dados gerida|
|[lista de midb AZ sql](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-list)|Bases de dados geridas de listas disponíveis|
|[restauro de midb AZ sql](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-restore)|Restaurar uma base de dados gerida|
|[atualização de midb AZ sql](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-update)|Atualiza uma base de dados gerida|
|[eliminação de midb AZ sql](https://docs.microsoft.com/cli/azure/sql/midb#az-sql-midb-delete)|Remove uma base de dados gerida|

## <a name="transact-sql-manage-logical-servers-and-databases"></a>Transact-SQL: Gerir servidores lógicos e bases de dados

Para criar e gerir a base de dados de instância gerida da base de dados SQL do Azure após a instância gerida é criada, utilize os seguintes comandos T-SQL. Pode emitir estes comandos no portal do Azure, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Estúdio de dados do Azure](https://docs.microsoft.com/sql/azure-data-studio/what-is). [Visual Studio Code](https://code.visualstudio.com/docs), ou qualquer outro programa que pode ligar a um servidor de base de dados do Azure SQL e passar comandos Transact-SQL.

> [!TIP]
> Para guias de início rápido que mostra de tem de configurar e ligar a uma instância gerida com o SQL Server Management Studio no Microsoft Windows, consulte [início rápido: Configurar a VM do Azure para ligar a um Azure SQL Database Managed Instance](sql-database-managed-instance-configure-vm.md) e [início rápido: Configurar uma ligação de ponto a site para uma instância de gerida de base de dados do Azure SQL no local](sql-database-managed-instance-configure-p2s.md).
> [!IMPORTANT]
> Não é possível criar ou eliminar uma instância gerida com o Transact-SQL.

| Comando | Descrição |
| --- | --- |
|[CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-mi-current)|Cria uma nova base de dados de instância gerida. Tem de estar ligado à base de dados principal para criar uma nova base de dados.|
| [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-mi-current) |Modifica uma base de dados de instância gerida do SQL do Azure.|

## <a name="rest-api-manage-logical-servers-and-databases"></a>REST API: Gerir servidores lógicos e bases de dados

Para criar e gerir a instância gerida da base de dados SQL do Azure, utilize estes pedidos de REST API.

| Comando | Descrição |
| --- | --- |
|[Instâncias - geridas criar ou atualizar](https://docs.microsoft.com/rest/api/sql/managedinstances/createorupdate)|Cria ou atualiza uma instância gerida.|
|[Instâncias geridas - Delete](https://docs.microsoft.com/rest/api/sql/managedinstances/delete)|Elimina uma instância gerida.|
|[Instâncias geridas - Get](https://docs.microsoft.com/rest/api/sql/managedinstances/get)|Obtém uma instância gerida.|
|[Lista de instâncias - geridas](https://docs.microsoft.com/rest/api/sql/managedinstances/list)|Devolve uma lista de instâncias geridas numa subscrição.|
|[Instâncias - lista por grupo de recursos geridas](https://docs.microsoft.com/rest/api/sql/managedinstances/listbyresourcegroup)|Devolve uma lista de instâncias geridas num grupo de recursos.|
|[Instâncias geridas - atualização](https://docs.microsoft.com/rest/api/sql/managedinstances/update)|Atualiza uma instância gerida.|

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre como migrar uma base de dados do SQL Server para o Azure, veja [migre para o Azure SQL Database](sql-database-cloud-migrate.md).
- Para obter informações sobre as funcionalidades suportadas, veja [Funcionalidades](sql-database-features.md).
