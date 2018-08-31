---
title: Restaurar um armazém de dados SQL do Azure | Documentos da Microsoft
description: Como guia para restaurar um armazém de dados SQL do Azure.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 08/29/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 583346f2297f590d8e9484c0a3c19c947de7f740
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43191147"
---
# <a name="restoring-azure-sql-data-warehouse"></a>Restaurar o armazém de dados SQL do Azure 
Neste artigo, aprenderá como fazer o seguinte:

- Criar um ponto de restauro
- Restaurar a partir de um ponto de restauro automático ou ponto de restauro definidas pelo utilizador
- Restaurar a partir de uma base de dados eliminada
- Restaurar a partir de uma cópia de segurança geo
- Criar uma cópia do seu armazém de dados a partir de um ponto de restauro definidas pelo utilizador

## <a name="before-you-begin"></a>Antes de começar
**Verifique a sua capacidade DTU.** Cada SQL Data Warehouse é hospedado por um SQL server (por exemplo, myserver.database.windows.net), que tem uma quota DTU predefinida.  Antes de pode restaurar um armazém de dados SQL, certifique-se de que o seu SQL server tem quota suficiente DTU restante para a base de dados a ser restaurada. Para saber como calcular a DTU necessária ou para pedir DTU mais, veja [solicitar uma alteração de quota DTU][Request a DTU quota change].

# <a name="restore-through-powershell"></a>Restaurar através do PowerShell

## <a name="install-powershell"></a>Instalar o PowerShell
Para utilizar o Azure PowerShell com o SQL Data Warehouse, terá de instalar o Azure PowerShell versão 1.0 ou superior.  Pode verificar a sua versão, executando **Get-Module - ListAvailable-Name AzureRM**.  Pode ser instalada a versão mais recente a partir [Microsoft Web Platform Installer][Microsoft Web Platform Installer].  Para mais informações sobre como instalar a versão mais recente, consulte [How to install and configure Azure PowerShell (Como instalar e configurar o Azure PowerShell)][How to install and configure Azure PowerShell].

## <a name="restore-an-active-or-paused-database"></a>Restaurar uma base de dados do Active Directory ou em pausa
Para restaurar uma base de dados a partir de uma utilização de ponto de restauro a [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet do PowerShell.

1. Abra o Windows PowerShell.

2. Ligue a sua conta do Azure e listar todas as subscrições associadas à sua conta.

3. Selecione a subscrição que contém a base de dados para serem restaurados.

4. Liste os pontos de restauro da base de dados.

5. Escolha o ponto de restauro desejado usando o RestorePointCreationDate.

   > [!NOTE]
   > Ao restaurar, pode especificar um ServiceObjectiveName diferentes (DWU) ou um servidor diferente que residem numa região diferente.

6. Restaure a base de dados para o ponto de restauro pretendido.

7. Certifique-se de que a base de dados restaurada está online.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName)).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

> [!NOTE]
> Depois do restauro foi concluída, pode configurar a base de dados recuperada seguindo [configurar a sua base de dados após a recuperação][Configure your database after recovery].
>

## <a name="copy-your-data-warehouse-with-user-defined-restore-points"></a>Copie o seu armazém de dados com pontos de restauro definidas pelo utilizador
Para restaurar uma base de dados a partir de uma utilização de ponto de restauro definidas pelo utilizador a [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet do PowerShell.

1. Abra o Windows PowerShell.
2. Ligue a sua conta do Azure e listar todas as subscrições associadas à sua conta.
3. Selecione a subscrição que contém a base de dados para serem restaurados.
4. Criar um ponto de restauro para um imediata cópia da base de dados
5. Mudar o nome da sua base de dados para um nome temporário.
5. Obter o ponto de restauro mais recente, o RestorePointLabel especificado.
6. Obter o id de recurso da base de dados para iniciar o restauro
6. Restaure a base de dados para o ponto de restauro pretendido.
7. Certifique-se de que a base de dados restaurada está online.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$TempDatabaseName = "<YourTemporaryDatabaseName>"
$Label = "<YourRestorePointLabel"

Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Create a restore point of the original database
New-AzureRmSqlDatabaseRestorePoint -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -RestorePointLabel $Label

# Rename the database to a temporary name
Set-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -NewName $TempDatabaseName

# Get the most recent restore point with the specified label
$LabelledRestorePoint = Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $TempDatabaseName | where {$_.RestorePointLabel -eq $Label} | sort {$_.RestorePointCreationDate} | select -Last 1

# Get the resource id of the database
$ResourceId = (Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $TempDatabaseName).ResourceId

# Restore the database to its original name from the labelled restore point from the temporary database
$RestoredDatabase = Restore-AzureRmSqlDatabase -FromPointInTimeBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -ResourceId $ResourceId -PointInTime $LabelledRestorePoint.RestorePointCreationDate -TargetDatabaseName $DatabaseName

# Verify the status of restored database
$RestoredDatabase.status

# The original temporary database can be deleted at this point

```

## <a name="restore-a-deleted-database"></a>Restaurar uma base de dados eliminada
Para restaurar uma base de dados eliminada, utilize o [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet.

1. Abra o Windows PowerShell.
2. Ligue a sua conta do Azure e listar todas as subscrições associadas à sua conta.
3. Selecione a subscrição que contém a base de dados eliminada para serem restaurados.
4. Obtenha a base de dados eliminada específico.
5. Restaure a base de dados eliminada.
6. Certifique-se de que a base de dados restaurada está online.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

> [!NOTE]
> Depois do restauro foi concluída, pode configurar a base de dados recuperada seguindo [configurar a sua base de dados após a recuperação][Configure your database after recovery].
>

## <a name="restore-from-an-azure-geographical-region"></a>Restaurar a partir de uma região geográfica do Azure
Para recuperar uma base de dados, utilize o [Restore-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] cmdlet.

> [!NOTE]
> Pode efetuar um georrestauro para ger2! Para tal, especifique um ServiceObjectiveName de geração 2 (por exemplo, DW1000**c**) como um parâmetro opcional.
>

1. Abra o Windows PowerShell.
2. Ligue a sua conta do Azure e listar todas as subscrições associadas à sua conta.
3. Selecione a subscrição que contém a base de dados para serem restaurados.
4. Obtenha a base de dados que pretende recuperar.
5. Crie o pedido de recuperação para a base de dados.
6. Verifique se o estado da base de dados geo-restaurada.

```Powershell
Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID -ServiceObjectiveName "<YourTargetServiceLevel>"

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> Para configurar a sua base de dados após o restauro foi concluída, consulte [configurar a sua base de dados após a recuperação][Configure your database after recovery].
>

A base de dados recuperada serão habilitados para TDE se a base de dados de origem é habilitado para TDE.

# <a name="restore-through-the-azure-portal"></a>Restaurar através do Portal do Azure

## <a name="create-a-user-defined-restore-point"></a>Criar um ponto de restauro definidas pelo utilizador
1. Inicie sessão no [Portal do Azure][Azure portal].

2. Navegue para o SQL data warehouse que pretende criar um ponto de restauro para.

3. Na parte superior do painel de descrição geral, selecione **+ novo ponto de restauro**.

    ![Novo ponto de restauro](./media/sql-data-warehouse-restore-database-portal/creating_restore_point_0.png)
    
4. Especifique um nome para o ponto de restauro.

    ![Nome do ponto de restauro](./media/sql-data-warehouse-restore-database-portal/creating_restore_point_1.png)

## <a name="restore-an-active-or-paused-database"></a>Restaurar uma base de dados do Active Directory ou em pausa
1. Inicie sessão no [Portal do Azure][Azure portal].
2. Navegue para o SQL data warehouse que pretende restaurar a partir de.
3. Na parte superior do painel de descrição geral, selecione **restaurar**.

    ![ Descrição geral do Restauro](./media/sql-data-warehouse-restore-database-portal/restoring_0.png)
    
4. Selecione **pontos de restauro automático** ou **pontos de restauro definidas pelo utilizador**.

    ![Pontos de Restauro Automático](./media/sql-data-warehouse-restore-database-portal/restoring_1.png)
    
5. Para pontos de restauração de definidas pelo utilizador, **selecione um ponto de restauro** ou **criar um novo ponto de restauro definidas pelo utilizador**.

    ![Pontos de restauro definidas pelo utilizador](./media/sql-data-warehouse-restore-database-portal/restoring_2_udrp.png)

## <a name="restore-a-deleted-database"></a>Restaurar uma base de dados eliminada
1. Inicie sessão no [Portal do Azure][Azure portal].
2. Navegue até à sua base de dados eliminada encontrava alojado no SQL server.
3. Selecione o ícone de bases de dados de eliminados no índice.

    ![Bases de dados eliminadas](./media/sql-data-warehouse-restore-database-portal/restoring_deleted_0.png)
    
4. Selecione a base de dados eliminada que pretende restaurar.

    ![Selecione as bases de dados eliminadas](./media/sql-data-warehouse-restore-database-portal/restoring_deleted_1.png)
    
5. Especifique um novo nome de base de dados.

    ![Especifique o nome de base de dados](./media/sql-data-warehouse-restore-database-portal/restoring_deleted_2.png)

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Restore-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
