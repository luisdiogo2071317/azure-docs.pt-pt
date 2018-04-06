---
title: Retenção de cópias de segurança de longa duração & ARS cofre - SQL Database do Azure | Microsoft Docs
description: Saiba como armazenar cópias de segurança automatizadas do SQL Server do armazenamento do Azure e, em seguida, restaurá-las
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: article
ms.date: 04/10/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 80dd58a9c0267975c9e4df74c77d60ac861a1fdb
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="configure-and-restore-backups-from-azure-sql-database-long-term-backup-retention-using-azure-sql-storage"></a>Configurar e restaurar cópias de segurança da base de dados do Azure SQL cópia de segurança retenção de longo prazo utilizando o armazenamento de SQL do Azure

Pode configurar a base de dados SQL do Azure com um [retenção de cópias de segurança de longa duração](sql-database-long-term-retention.md) política (imediatamente disponíveis) para manter automaticamente cópias de segurança no armazenamento de Blobs do Azure para até 10 anos. Em seguida, pode recuperar uma base de dados utilizando estas cópias de segurança utilizando o portal do Azure ou o PowerShell.

> [!NOTE]
> Como parte da versão inicial do preview desta funcionalidade Outubro de 2016, as cópias de segurança foram armazenadas no cofre do serviço de recuperação de serviços do Azure. Esta atualização remove esta dependência, mas para compatibilidade com versões anteriores a API original é suportada até 31 de Maio de 2018. Se precisar de interagir com as cópias de segurança no Cofre de recuperação de serviços do Azure, consulte [retenção de cópias de segurança de longo prazo com o Cofre de serviço de recuperação de serviços do Azure](sql-database-long-term-backup-retention-configure-vault.md). 

## <a name="use-the-azure-portal-to-configure-long-term-retention-policies-and-restore-backups"></a>Utilizar o portal do Azure para configurar políticas de retenção de longo prazo e restaurar cópias de segurança

As secções seguintes mostram como utilizar o portal do Azure para configurar a retenção de longo prazo, ver as cópias de segurança na retenção de longo prazo e restaurar a cópia de segurança de retenção de longo prazo.

### <a name="configure-long-term-retention-policies"></a>Configurar políticas de retenção de longo prazo

Pode configurar a base de dados do SQL Server para [manter cópias de segurança automatizadas](sql-database-long-term-retention.md) durante um período mais longo do que o período de retenção para o escalão de serviço. 

1. No portal do Azure, selecione o SQL server e, em seguida, clique em **retenção de cópias de segurança de longa duração**.

   ![ligação de retenção de longa duração de cópia de segurança](./media/sql-database-long-term-retention/ltr-configure-ltr.png)

2. No **configurar políticas** separador, selecione a base de dados no qual pretende definir ou modificar as políticas de retenção de cópias de segurança de longa duração.

   ![Selecione a base de dados](./media/sql-database-long-term-retention/ltr-configure-select-database.png)

3. No **configurar políticas** painel, selecione se pretende manter semanais, mensais ou anuais cópias de segurança e especifique o período de retenção para cada. 

   ![configurar políticas](./media/sql-database-long-term-retention/ltr-configure-policies.png)

4. Quando terminar, clique em **aplicar**.

### <a name="view-backups-and-restore-from-a-backup-using-azure-portal"></a>Ver as cópias de segurança e restaurar a partir de uma cópia de segurança através do portal do Azure

Ver as cópias de segurança que são mantidas para uma base de dados específico com uma política de imediatamente disponíveis e o restauro dessas cópias de segurança. 

1. No portal do Azure, selecione o SQL server e, em seguida, clique em **retenção de cópias de segurança de longa duração**.

   ![ligação de retenção de longa duração de cópia de segurança](./media/sql-database-long-term-retention/ltr-configure-ltr.png)

2. No **cópias de segurança disponíveis** separador, selecione a base de dados para o qual pretende ver as cópias de segurança disponíveis.

   ![Selecione a base de dados](./media/sql-database-long-term-retention/ltr-available-backups-select-database.png)

3. No **cópias de segurança disponíveis** painel, reveja as cópias de segurança disponíveis. 

   ![cópias de segurança de vista](./media/sql-database-long-term-retention/ltr-available-backups.png)

4. Selecione a cópia de segurança a partir do qual pretende restaurar e, em seguida, especifique o novo nome de base de dados.

   ![restore](./media/sql-database-long-term-retention/ltr-restore.png)

5. Clique em **OK** para restaurar a base de dados da cópia de segurança no armazenamento de SQL do Azure para a nova base de dados.

6. Na barra de ferramentas, clique no ícone de notificação para ver o estado da tarefa de restauro.

   ![progresso da tarefa de restauro a partir do cofre](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Quando a tarefa de restauro está concluída, abra o **bases de dados SQL** página para ver a base de dados recentemente restaurado.

> [!NOTE]
> A partir daqui, pode ligar à base de dados restaurada através do o SQL Server Management Studio para efetuar tarefas necessárias, bem como para [extrair alguns dados da base de dados restaurada para copiá-los para a base de dados existente ou para eliminar a base de dados existente e mudar o nome da base de dados restaurada para o nome da base de dados existente](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="use-powershell-to-configure-long-term-retention-policies-and-restore-backups"></a>Utilizar o PowerShell para configurar políticas de retenção de longo prazo e restaurar cópias de segurança

As secções seguintes mostram como utilizar o PowerShell para configurar a retenção de cópias de segurança de longa duração, ver as cópias de segurança no armazenamento de SQL do Azure e o restauro a partir de uma cópia de segurança no armazenamento de SQL do Azure.

### <a name="create-an-ltr-policy"></a>Criar uma política de imediatamente disponíveis

```powershell
# Get the SQL server 
# $subId = “{subscription-id}”
# $serverName = “{server-name}”
# $resourceGroup = “{resource-group-name}” 
# $dbName = ”{database-name}”

Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId $subId

# get the server
$server = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroup

# create LTR policy with WeeklyRetention = 12 weeks. MonthlyRetention and YearlyRetention = 0 by default.
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W 

# create LTR policy with WeeklyRetention = 12 weeks, YearlyRetetion = 5 years and WeekOfYear = 16 (week of April 15). MonthlyRetention = 0 by default.
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W -YearlyRetention P5Y -WeekOfYear 16
```

### <a name="view-ltr-policies"></a>Vista imediatamente disponíveis políticas
Este exemplo mostra como listar as políticas de imediatamente disponíveis dentro de um servidor

```powershell
# Get all LTR policies within a server
$ltrPolicies = Get-AzureRmSqlDatabase -ResourceGroupName Default-SQL-WestCentralUS -ServerName trgrie-ltr-server | Get-AzureRmSqlDatabaseLongTermRetentionPolicy -Current 

# Get the LTR policy of a specific database 
$ltrPolicies = Get-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName  -ResourceGroupName $resourceGroup -Current
```

### <a name="view-ltr-backups"></a>Cópias de segurança do Vista imediatamente disponíveis

Este exemplo mostra como listar as cópias de segurança imediatamente disponíveis dentro de um servidor. 

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

### <a name="delete-ltr-backups"></a>Elimine as cópias de segurança imediatamente disponíveis

Este exemplo mostra como eliminar uma imediatamente disponíveis cópia de segurança da lista de cópias de segurança.

```powershell
# remove the earliest backup 
$ltrBackup = $ltrBackups[0]
Remove-AzureRmSqlDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```

### <a name="restore-from-ltr-backups"></a>Restaurar a partir de cópias de segurança imediatamente disponíveis
Este exemplo mostra como restaurar a partir de uma cópia de segurança imediatamente disponíveis. Tenha em atenção que esta interface não mudou, mas o parâmetro de id de recurso requer agora o id de recurso de cópia de segurança imediatamente disponíveis. 

```powershell
# Restore LTR backup as an S3 database
Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId -ServerName $serverName -ResourceGroupName $resourceGroup -TargetDatabaseName $dbName -ServiceObjectiveName S3
```

> [!NOTE]
> Aqui, pode ligar à base de dados restaurada, utilizando o SQL Server Management Studio para efetuar tarefas de necessárias, como juntar extrair um bits de dados da base de dados restaurada para copiar para a base de dados existente ou elimine o existente da base de dados e mudar o nome de restaurada base de dados para o nome de base de dados existente. Consulte [ponto de restauro de tempo](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre cópias de segurança automáticas geradas pelo serviço, veja [cópias de segurança automáticas](sql-database-automated-backups.md)
- Para saber mais sobre a retenção de longa duração de cópia de segurança, veja [retenção de longa duração de cópia de segurança](sql-database-long-term-retention.md)
