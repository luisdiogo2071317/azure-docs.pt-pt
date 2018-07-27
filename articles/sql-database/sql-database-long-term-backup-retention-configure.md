---
title: Gerir a retenção de cópia de segurança de longa duração de SQL Database do Azure | Documentos da Microsoft
description: Saiba como armazenar cópias de segurança automáticas no armazenamento do SQL Azure e, em seguida, restaurá-las
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 07/25/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 6f0d8a3a09ce000ddff078614c4febfc44ac941f
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/26/2018
ms.locfileid: "39264948"
---
# <a name="manage-azure-sql-database-long-term-backup-retention"></a>Gerir a retenção de cópia de segurança de longa duração de base de dados do Azure SQL

Pode configurar a base de dados SQL do Azure com um [retenção de cópia de segurança de longo prazo](sql-database-long-term-retention.md) política (LTR) para manter automaticamente cópias de segurança no armazenamento de Blobs do Azure para até 10 anos. Em seguida, pode recuperar uma base de dados com estas cópias de segurança com o portal do Azure ou o PowerShell.

## <a name="use-the-azure-portal-to-configure-long-term-retention-policies-and-restore-backups"></a>Utilizar o portal do Azure para configurar políticas de retenção de longo prazo e restaurar cópias de segurança

As secções seguintes mostram como utilizar o portal do Azure para configurar a retenção de longa duração, ver cópias de segurança em retenção de longo prazo e restaurar a cópia de segurança de retenção de longa duração.

### <a name="configure-long-term-retention-policies"></a>Configurar políticas de retenção de longo prazo

Pode configurar a base de dados SQL [reter cópias de segurança automáticas](sql-database-long-term-retention.md) durante um período maior do que o período de retenção para a camada de serviço. 

1. No portal do Azure, selecione o seu servidor SQL e, em seguida, clique em **gerir cópias de segurança**. Sobre o **configurar políticas de** separador, selecione a base de dados no qual pretende definir ou modificar as políticas de retenção de cópia de segurança de longo prazo.

   ![gerir cópias de segurança de ligação](./media/sql-database-long-term-retention/ltr-configure-ltr.png)

2. Na **configurar políticas de** painel, selecione se pretende manter semanalmente, mensalmente ou anualmente cópias de segurança e especifique o período de retenção para cada um. 

   ![configurar políticas](./media/sql-database-long-term-retention/ltr-configure-policies.png)

3. Quando terminar, clique em **aplicar**.

### <a name="view-backups-and-restore-from-a-backup-using-azure-portal"></a>Ver cópias de segurança e restaurar a partir de uma cópia de segurança através do portal do Azure

Ver as cópias de segurança que são mantidas para um banco de dados específico com uma política LTR e o restauro a partir dessas cópias de segurança. 

1. No portal do Azure, selecione o seu servidor SQL e, em seguida, clique em **gerir cópias de segurança**. Sobre o **cópias de segurança disponíveis** separador, selecione a base de dados para o qual pretende ver cópias de segurança disponíveis.

   ![Selecione a base de dados](./media/sql-database-long-term-retention/ltr-available-backups-select-database.png)

3. Na **cópias de segurança disponíveis** painel, reveja as cópias de segurança disponíveis. 

   ![Ver cópias de segurança](./media/sql-database-long-term-retention/ltr-available-backups.png)

4. Selecione a cópia de segurança a partir do qual pretende restaurar e, em seguida, especifique o novo nome de base de dados.

   ![restore](./media/sql-database-long-term-retention/ltr-restore.png)

5. Clique em **OK** para restaurar a base de dados da cópia de segurança no armazenamento de SQL do Azure para a nova base de dados.

6. Na barra de ferramentas, clique no ícone de notificação para ver o estado da tarefa de restauro.

   ![progresso da tarefa de restauro](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Quando a tarefa de restauro está concluída, abra a **bases de dados SQL** página para ver a base de dados recentemente restaurada.

> [!NOTE]
> A partir daqui, pode ligar à base de dados restaurada através do o SQL Server Management Studio para efetuar tarefas necessárias, bem como para [extrair alguns dados da base de dados restaurada para copiá-los para a base de dados existente ou para eliminar a base de dados existente e mudar o nome da base de dados restaurada para o nome da base de dados existente](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="use-powershell-to-configure-long-term-retention-policies-and-restore-backups"></a>Utilizar o PowerShell para configurar políticas de retenção de longo prazo e restaurar cópias de segurança

As secções seguintes mostram como utilizar o PowerShell para configurar a retenção de cópia de segurança de longo prazo, ver cópias de segurança no armazenamento de SQL do Azure e o restauro a partir de uma cópia de segurança no armazenamento de SQL do Azure.

> [!IMPORTANT]
> API do LTR V2 é suportada nas seguintes versões do PowerShell:
- [Azurerm. SQL 4.5.0](https://www.powershellgallery.com/packages/AzureRM.Sql/4.5.0) ou mais recente
- [AzureRM 6.1.0](https://www.powershellgallery.com/packages/AzureRM/6.1.0) ou mais recente
> 

### <a name="create-an-ltr-policy"></a>Criar uma política LTR

```powershell
# Get the SQL server 
# $subId = “{subscription-id}”
# $serverName = “{server-name}”
# $resourceGroup = “{resource-group-name}” 
# $dbName = ”{database-name}”

Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId $subId

# get the server
$server = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroup

# create LTR policy with WeeklyRetention = 12 weeks. MonthlyRetention and YearlyRetention = 0 by default.
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W 

# create LTR policy with WeeklyRetention = 12 weeks, YearlyRetetion = 5 years and WeekOfYear = 16 (week of April 15). MonthlyRetention = 0 by default.
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W -YearlyRetention P5Y -WeekOfYear 16
```

### <a name="view-ltr-policies"></a>Ver políticas de LTR
Este exemplo mostra como listar as políticas LTR dentro de um servidor

```powershell
# Get all LTR policies within a server
$ltrPolicies = Get-AzureRmSqlDatabase -ResourceGroupName Default-SQL-WestCentralUS -ServerName trgrie-ltr-server | Get-AzureRmSqlDatabaseLongTermRetentionPolicy -Current 

# Get the LTR policy of a specific database 
$ltrPolicies = Get-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName  -ResourceGroupName $resourceGroup -Current
```
### <a name="clear-an-ltr-policy"></a>Limpar uma política LTR
Este exemplo mostra como limpar a uma política LTR de um banco de dados

```powershell
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -RemovePolicy
```

### <a name="view-ltr-backups"></a>Ver cópias de segurança LTR

Este exemplo mostra como listar as cópias de segurança LTR dentro de um servidor. 

```powershell
# Get the list of all LTR backups in a specific Azure region 
# The backups are grouped by the logical database id.
# Within each group they are ordered by the timestamp, the earliest
# backup first.  
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location 

# Get the list of LTR backups from the Azure region under 
# the named server. 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -ServerName $serverName

# Get the LTR backups for a specific database from the Azure region under the named server 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -ServerName $serverName -DatabaseName $dbName

# List LTR backups only from live databases (you have option to choose All/Live/Deleted)
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -DatabaseState Live

# Only list the latest LTR backup for each database 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -ServerName $serverName -OnlyLatestPerDatabase
```

### <a name="delete-ltr-backups"></a>Eliminar cópias de segurança LTR

Este exemplo mostra como eliminar um LTR cópia de segurança na lista de cópias de segurança.

```powershell
# remove the earliest backup 
$ltrBackup = $ltrBackups[0]
Remove-AzureRmSqlDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```

### <a name="restore-from-ltr-backups"></a>Restaurar a partir de cópias de segurança LTR
Este exemplo mostra como restaurar a partir de uma cópia de segurança LTR. Tenha em atenção que esta interface não foi alterado, mas o parâmetro de id de recurso requer agora que o id de recurso de cópia de segurança de LTR. 

```powershell
# Restore LTR backup as an S3 database
Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId -ServerName $serverName -ResourceGroupName $resourceGroup -TargetDatabaseName $dbName -ServiceObjectiveName S3
```

> [!NOTE]
> A partir daqui, pode ligar à base de dados restaurada com o SQL Server Management Studio para efetuar tarefas necessárias, como para extrair um pouco de dados da base de dados restaurada para copiar para a base de dados existente ou para eliminar o existente da base de dados e mudar o nome do restaurada base de dados para o nome de base de dados existente. Ver [ponto no tempo](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre cópias de segurança automáticas geradas pelo serviço, veja [cópias de segurança automáticas](sql-database-automated-backups.md)
- Para saber mais sobre a retenção de longa duração de cópia de segurança, veja [retenção de longa duração de cópia de segurança](sql-database-long-term-retention.md)
