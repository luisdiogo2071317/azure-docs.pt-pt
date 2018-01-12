---
title: "Exemplo do PowerShell - esquema de sincronização de sincronização de dados do SQL Server de atualização | Microsoft Docs"
description: "Script de exemplo do PowerShell do Azure para atualizar o esquema de sincronização de sincronização de dados do SQL Server"
services: sql-database
documentationcenter: sql-database
author: jognanay
manager: craigg
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data, mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 01/10/2018
ms.author: jognanay
ms.reviewer: douglasl
ms.openlocfilehash: 66bf084f585b86979e6521321daf466c571de10c
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="use-powershell-to-update-the-sync-schema-in-an-existing-sync-group"></a>Utilize o PowerShell para atualizar o esquema de sincronização num grupo de sincronização existente

Neste exemplo do PowerShell atualiza o esquema de sincronização num grupo de sincronização existentes. Quando estiver a sincronizar a várias tabelas, este script ajuda-o para atualizar o esquema de sincronização de forma eficiente.

Este exemplo demonstra a utilização do **UpdateSyncSchema** script, que está disponível no GitHub como [UpdateSyncSchema.ps1](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-data-sync/UpdateSyncSchema.ps1).

Para obter uma descrição geral da sincronização de dados do SQL Server, consulte [sincronizar os dados em várias bases de dados na nuvem e no local com sincronização de dados de SQL do Azure (pré-visualização)](../sql-database-sync-data.md).
## <a name="prerequisites"></a>Pré-requisitos

Este exemplo requer o Azure PowerShell versão do módulo 4.2 ou posterior. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de instalar ou atualizar, veja [Install Azure PowerShell module](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)(Instalar o módulo do Azure PowerShell).
 
Executar `Login-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="examples"></a>Exemplos

### <a name="example-1---add-all-tables-to-the-sync-schema"></a>Exemplo 1 - adicionar todas as tabelas para o esquema de sincronização

O exemplo seguinte atualiza o esquema de base de dados e adiciona todas as tabelas válidas na base de dados do hub para o esquema de sincronização.

```powershell
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -RefreshDatabaseSchema $true -AddAllTables $true
```

### <a name="example-2---add-and-remove-tables-and-columns"></a>Exemplo 2 - adicionar e remover as tabelas e colunas

O exemplo seguinte adiciona `[dbo].[Table1]` e `[dbo].[Table2].[Column1]` para o esquema de sincronização e remove `[dbo].[Table3]`.

```powershell
UpdateSyncSchema.ps1 -SubscriptionId <subscription_id> -ResourceGroupName <resource_group_name> -ServerName <server_name> -DatabaseName <database_name> -SyncGroupName <sync_group_name> -TablesAndColumnsToAdd "[dbo].[Table1],[dbo].[Table2].[Column1]" -TablesAndColumnsToRemove "[dbo].[Table3]"
```

## <a name="script-parameters"></a>Parâmetros do script

O **UpdateSyncSchema** script tem os seguintes parâmetros:

| Parâmetro | Notas |
|---|---|
| $SubscriptionId | A subscrição em que o grupo de sincronização é criado. |
| $ResourceGroupName | O grupo de recursos em que o grupo de sincronização é criado.|
| $ServerName | O nome do servidor da base de dados do hub.|
| $DatabaseName | O nome de base de dados do hub. |
| $SyncGroupName | O nome do grupo de sincronização. |
| $MemberName | Especifique o nome de membro, se pretender carregar o esquema de base de dados do membro em vez de sincronização da base de dados do hub. Se pretender carregar o esquema de base de dados do hub, deixe em branco este parâmetro. |
| $TimeoutInSeconds | Tempo limite quando o script atualiza o esquema de base de dados. Predefinição é 900 segundos. |
| $RefreshDatabaseSchema | Especifique se o script tem de atualizar o esquema de base de dados. Se o esquema de base de dados foi alterado da configuração anterior - por exemplo, se tiver adicionado uma nova tabela ou anew coluna), terá de atualizar o esquema antes de pode reconfigurá-lo. Predefinição é falsa. |
| $AddAllTables | Se este valor for VERDADEIRO, todos os válido tabelas e colunas são adicionadas para o esquema de sincronização. Os valores de $TablesAndColumnsToAdd e $TablesAndColumnsToRemove são ignorados. |
| $TablesAndColumnsToAdd | Especifica as tabelas ou colunas para ser adicionado para o esquema de sincronização. Cada nome de tabela ou coluna tem de ser delimitados totalmente com o nome do esquema. Por exemplo: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Vários nomes de tabela ou coluna podem ser especificados e separados por vírgulas (,). |
| $TablesAndColumnsToRemove | Especifica as tabelas ou colunas para ser removido o esquema de sincronização. Cada nome de tabela ou coluna tem de ser delimitados totalmente com o nome de esquema. Por exemplo: `[dbo].[Table1]`, `[dbo].[Table2].[Column1]`. Vários nomes de tabela ou coluna podem ser especificados e separados por vírgulas (,). |
|||

## <a name="script-explanation"></a>Explicação de script

O **UpdateSyncSchema** script utiliza os seguintes comandos. Cada comando nas ligações de tabela para a documentação específica do comando.

| Comando | Notas |
|---|---|
| [Get-AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncgroup) | Devolve informações sobre um grupo de sincronização. |
| [Atualização AzureRmSqlSyncGroup](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncgroup) | Um grupo de sincronização de atualizações. |
| [Get-AzureRmSqlSyncMember](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncmember) | Devolve informações sobre um membro de sincronização. |
| [Get-AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqlsyncschema) | Devolve informações sobre um esquema de sincronização. |
| [Atualização AzureRmSqlSyncSchema](https://docs.microsoft.com/powershell/module/azurerm.sql/update-azurermsqlsyncschema) | Um esquema de sincronização de atualizações. |
|||

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Azure PowerShell, consulte [documentação do Azure PowerShell](/powershell/azure/overview).

Exemplos de script do PowerShell de base de dados do SQL adicionais podem ser encontrados na [scripts do PowerShell de base de dados do SQL Azure](../sql-database-powershell-samples.md).

Para obter mais informações sobre a sincronização de dados do SQL Server, consulte:

-   [Sincronizar os dados em várias bases de dados na nuvem e no local com sincronização de dados SQL do Azure](../sql-database-sync-data.md)
-   [Configurar a sincronização de dados SQL do Azure](../sql-database-get-started-sql-data-sync.md)
-   [Melhores práticas para a sincronização de dados SQL do Azure](../sql-database-best-practices-data-sync.md)
-   [Monitor sincronização de dados SQL do Azure com a análise de registos do OMS](../sql-database-sync-monitor-oms.md)
-   [Resolver problemas com a sincronização de dados SQL do Azure](../sql-database-troubleshoot-data-sync.md)

-   Conclua os exemplos do PowerShell que mostram como configurar a sincronização de dados do SQL Server:
    -   [Utilizar o PowerShell para sincronizar entre várias bases de dados SQL do Azure](sql-database-sync-data-between-sql-databases.md)
    -   [Utilizar o PowerShell para sincronizar entre uma base de dados do SQL do Azure e uma base de dados do SQL Server no local](sql-database-sync-data-between-azure-onprem.md)

-   [Transferir a documentação da API de REST de sincronização de dados SQL](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Para obter mais informações sobre a base de dados SQL, consulte:

-   [Descrição geral da base de dados do SQL Server](../sql-database-technical-overview.md)
-   [Gestão de ciclo de vida de base de dados](https://msdn.microsoft.com/library/jj907294.aspx)
