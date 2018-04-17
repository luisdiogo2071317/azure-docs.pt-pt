---
title: Exemplo do PowerShell - Atualizar o esquema de sincronização da Sincronização de Dados SQL (Pré-visualização) | Microsoft Docs
description: Script de exemplo do Azure PowerShell para atualizar o esquema de sincronização da Sincronização de Dados SQL
services: sql-database
documentationcenter: sql-database
author: jognanay
manager: craigg
editor: ''
tags: ''
ms.assetid: ''
ms.service: sql-database
ms.custom: load & move data, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 01/10/2018
ms.author: jognanay
ms.reviewer: douglasl
ms.openlocfilehash: 6b117484eaa1a9ac3ed85dfe547b3f6163d702cf
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="use-powershell-to-update-the-sync-schema-in-an-existing-sync-group"></a>Utilizar o PowerShell para atualizar o esquema de sincronização num grupo de sincronização existente

Este exemplo do PowerShell atualiza o esquema de sincronização num grupo de sincronização da Sincronização de Dados SQL (Pré-visualização). Este script ajuda-o a atualizar de forma eficiente o esquema de sincronização se estiver a sincronizar vários tablets.

Este exemplo demonstra a utilização do script **UpdateSyncSchema**, que está disponível no GitHub como [UpdateSyncSchema.ps1](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-data-sync/UpdateSyncSchema.ps1).

Para obter uma descrição geral da Sincronização de Dados SQL, veja [Sincronizar dados em várias bases de dados na cloud e no local com a Sincronização de Dados SQL do Azure (Pré-visualização)](../sql-database-sync-data.md).
## <a name="prerequisites"></a>Pré-requisitos

Este exemplo requer a versão 4.2 ou posterior do módulo do Azure PowerShell. Execute `Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de instalar ou atualizar, veja [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)(Instalar o módulo do Azure PowerShell).
 
Execute `Login-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="examples"></a>Exemplos

### <a name="example-1---add-all-tables-to-the-sync-schema"></a>Exemplo 1 - Adicionar todas as tabelas ao esquema de sincronização

O exemplo seguinte atualiza o esquema da base de dados e adiciona todas as tabelas válidas na base de dados de hub ao esquema de sincronização.

```powershell
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -RefreshDatabaseSchema $true -AddAllTables $true
```

### <a name="example-2---add-and-remove-tables-and-columns"></a>Exemplo 2 - Adicionar e remover tabelas e colunas

O exemplo seguinte adiciona `[dbo].[Table1]` e `[dbo].[Table2].[Column1]` ao esquema de sincronização e remove `[dbo].[Table3]`.

```powershell
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -TablesAndColumnsToAdd "[dbo].[Table1],[dbo].[Table2].[Column1]" -TablesAndColumnsToRemove "[dbo].[Table3]"
```

## <a name="script-parameters"></a>Parâmetros do script

O script **UpdateSyncSchema** tem os seguintes parâmetros:

| Parâmetro | Notas |
|---|---|
| $SubscriptionId | A subscrição na qual o grupo de sincronização é criado. |
| $ResourceGroupName | O grupo de recursos no qual o grupo de sincronização é criado.|
| $ServerName | O nome do servidor da base de dados de hub.|
| $DatabaseName | O nome da base de dados de hub. |
| $SyncGroupName | O nome do grupo de sincronização. |
| $MemberName | Indique o nome do membro se quiser carregar o esquema da base de dados a partir do membro de sincronização em vez da base de dados de hub. Se pretender carregar o esquema da base de dados do hub, deixe este parâmetro em branco. |
| $TimeoutInSeconds | O tempo limite para a atualização do esquema da base de dados por parte do script. A predefinição são 900 segundos. |
| $RefreshDatabaseSchema | Especifique se o script tem de atualizar o esquema da base de dados. Se o esquema da base de dados tiver mudado da configuração anterior (se, por exemplo, tiver adicionado uma tabela ou uma coluna nova), tem de atualizá-lo antes de poder reconfigurá-lo. A predefinição é falso. |
| $AddAllTables | Se este valor for verdadeiro, todas as tabelas e colunas válidas são adicionadas ao esquema de sincronização. Os valores de $TablesAndColumnsToAdd e $TablesAndColumnsToRemove são ignorados. |
| $TablesAndColumnsToAdd | Especifique as tabelas ou colunas que vão ser adicionadas ao esquema de sincronização. O nome de cada tabela ou coluna tem de ser totalmente delimitado pelo nome do esquema. Por exemplo: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Podem ser especificados vários nomes de tabelas ou colunas, separados por vírgulas (,). |
| $TablesAndColumnsToRemove | Especifique as tabelas ou colunas que vão ser removidas do esquema de sincronização. O nome de cada tabela ou coluna tem de ser totalmente delimitado pelo nome do esquema. Por exemplo: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Podem ser especificados vários nomes de tabelas ou colunas, separados por vírgulas (,). |
|||

## <a name="script-explanation"></a>Explicação do script

O script **UpdateSyncSchema** utiliza os seguintes comandos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [Get-AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncgroup) | Devolve informações sobre um grupo de sincronizações. |
| [Update-AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncgroup) | Atualiza um grupo de sincronização. |
| [Get-AzureRmSqlSyncMember](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncmember) | Devolve informações sobre um membro de sincronização. |
| [Get-AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncschema) | Devolve informações sobre um esquema de sincronização. |
| [Update-AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncschema) | Atualiza um esquema de sincronização. |
|||

## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Pode ver exemplos do script do PowerShell da Base de Dados SQL adicionais nos [scripts do PowerShell da Base de Dados SQL do Azure](../sql-database-powershell-samples.md).

Para obter mais informações sobre a Sincronização de Dados SQL, veja:

-   [Sincronizar dados em várias bases de dados na cloud e no local com a Sincronização de Dados SQL do Azure](../sql-database-sync-data.md)
-   [Configurar a Sincronização de Dados SQL do Azure](../sql-database-get-started-sql-data-sync.md)
-   [Melhores práticas da Sincronização de Dados SQL do Azure](../sql-database-best-practices-data-sync.md)
-   [Monitorizar a Sincronização de Dados SQL do Azure com o Log Analytics](../sql-database-sync-monitor-oms.md)
-   [Resolver problemas da Sincronização de Dados SQL do Azure](../sql-database-troubleshoot-data-sync.md)

-   Conclua os exemplos do PowerShell que mostram como configurar a Sincronização de Dados SQL:
    -   [Utilizar o PowerShell para sincronizar entre várias bases de dados SQL do Azure](sql-database-sync-data-between-sql-databases.md)
    -   [Utilizar o PowerShell para sincronizar entre uma Base de Dados SQL do Azure e uma base de dados do SQL Server no local](sql-database-sync-data-between-azure-onprem.md)

-   [Transferir a documentação da API REST da Sincronização de Dados SQL](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Para obter mais informações sobre a Base de Dados SQL, veja:

-   [Descrição Geral da Base de Dados SQL](../sql-database-technical-overview.md)
-   [Gestão do Ciclo de Vida da Base de Dados](https://msdn.microsoft.com/library/jj907294.aspx)
