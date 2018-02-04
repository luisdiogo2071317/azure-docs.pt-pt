---
title: "Início rápido: Aumentar horizontalmente computação no Azure SQL Data Warehouse - PowerShell | Microsoft Docs"
description: "As tarefas do PowerShell para aumentar horizontalmente recursos de computação ao ajustar unidades do data warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 01/31/2018
ms.author: elbutter;barbkess
ms.openlocfilehash: ff6c14ced42385d81f3c42b4b4bf1fd06464c0d1
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/03/2018
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-powershell"></a>Início rápido: Escala de computação no Azure SQL Data Warehouse no PowerShell

Dimensionar a computação no Azure SQL Data Warehouse no PowerShell. Aumentar horizontalmente computação para um melhor desempenho ou a escala de computação novamente para reduzir os custos. 

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

Este tutorial requer o Azure PowerShell versão do módulo 5.1.1 ou posterior. Executar `Get-Module -ListAvailable AzureRM` para encontrar a versão de que tem atualmente. Se precisar de instalar ou atualizar, veja [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps.md)(Instalar o módulo do Azure PowerShell). 

## <a name="before-you-begin"></a>Antes de começar

Este guia de introdução pressupõe que já tiver um armazém de dados do SQL Server que podem ser dimensionados. Se precisar de criar um, utilize [criar e ligar - portal](create-data-warehouse-portal.md) para criar um armazém de dados chamado **mySampleDataWarehouse**. 

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão na sua subscrição do Azure com o comando [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) e siga as instruções no ecrã.

```powershell
Add-AzureRmAccount
```

Para ver a subscrição que está a utilizar, execute [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription).

```powershell
Get-AzureRmSubscription
```

Se precisar de utilizar uma subscrição diferente que não o predefinido, execute [Select-AzureRmSubscription](/powershell/module/azurerm.profile/select-azurermsubscription).

```powershell
Select-AzureRmSubscription -SubscriptionName "MySubscription"
```

## <a name="look-up-data-warehouse-information"></a>Procurar informações do armazém de dados

Localize o nome de base de dados, o nome do servidor e o grupo de recursos para o armazém de dados que pretende colocar em pausa e retomar. 

Siga estes passos para encontrar informações de localização para o seu armazém de dados.

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
2. Clique em **bases de dados SQL** na página da esquerda do portal do Azure.
3. Selecione **mySampleDataWarehouse** do **bases de dados SQL** página. Esta ação abre o armazém de dados. 

    ![Grupo de recursos e nome de servidor](media/pause-and-resume-compute-powershell/locate-data-warehouse-information.png)

4. Anote o nome do armazém de dados que será utilizado como o nome de base de dados. Também anote o nome do servidor e o grupo de recursos. Irá utilizá-las na colocar em pausa e retomar comandos.
5. Se o servidor for foo.database.windows.net, utilize apenas a primeira parte como o nome do servidor nos cmdlets do PowerShell. Na imagem anterior, o nome de servidor completo é newserver 20171113.database.windows.net. Utilizamos **newserver 20171113** como o nome do servidor no cmdlet do PowerShell.

## <a name="scale-compute"></a>Dimensionar a computação

No SQL Data Warehouse, pode aumentar ou diminuir os recursos de computação ao ajustar unidades do data warehouse. O [criar e ligar - portal](create-data-warehouse-portal.md) criado **mySampleDataWarehouse** e inicializado-lo com 400 DWUs. Os seguintes passos ajustar as DWUs para **mySampleDataWarehouse**.

Para alterar as unidades do data warehouse, utilize o [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) cmdlet do PowerShell. O exemplo seguinte define as unidades de armazém de dados para DW300 para a base de dados **mySampleDataWarehouse** que está alojado no grupo de recursos **myResourceGroup** no servidor  **mynewserver 20171113**.

```Powershell
Set-AzureRmSqlDatabase -ResourceGroupName "myResourceGroup" -DatabaseName "mySampleDataWarehouse" -ServerName "mynewserver-20171113" -RequestedServiceObjectiveName "DW300"
```

## <a name="check-database-state"></a>Verificar o estado da base de dados

Para ver o estado atual do armazém de dados, utilize o [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) cmdlet do PowerShell. Este obtém o estado do **mySampleDataWarehouse** base de dados no ResourceGroup **myResourceGroup** e servidor **mynewserver 20171113.database.windows.net**.

```powershell
Get-AzureRmSqlDatabase -ResourceGroupName myResourceGroup -ServerName mynewserver-20171113 -DatabaseName mySampleDataWarehouse
```

Que irá resultar no algo semelhante ao seguinte:

```powershell   
ResourceGroupName             : myResourceGroup
ServerName                    : mynewserver-20171113
DatabaseName                  : mySampleDataWarehouse
Location                      : North Europe
DatabaseId                    : 34d2ffb8-b70a-40b2-b4f9-b0a39833c974
Edition                       : DataWarehouse
CollationName                 : SQL_Latin1_General_CP1_CI_AS
CatalogCollation              :
MaxSizeBytes                  : 263882790666240
Status                        : Online
CreationDate                  : 11/20/2017 9:18:12 PM
CurrentServiceObjectiveId     : 284f1aff-fee7-4d3b-a211-5b8ebdd28fea
CurrentServiceObjectiveName   : DW300
RequestedServiceObjectiveId   : 284f1aff-fee7-4d3b-a211-5b8ebdd28fea
RequestedServiceObjectiveName :
ElasticPoolName               :
EarliestRestoreDate           :
Tags                          :
ResourceId                    : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/
                                resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/mynewserver-20171113/databases/mySampleDataWarehouse
CreateMode                    :
ReadScale                     : Disabled
ZoneRedundant                 : False
```

Em seguida, pode verificar para ver o **estado** da base de dados. Neste caso, pode ver que esta base de dados está online.  Quando executar este comando, deverá receber um valor de estado de Online, Pausing, a retomar, dimensionamento ou em pausa.

## <a name="next-steps"></a>Passos Seguintes
Agora que tem aprendeu como dimensionar a computação para o seu armazém de dados. Para saber mais sobre o Azure SQL Data Warehouse, avance para o tutorial para carregar dados.

> [!div class="nextstepaction"]
>[Carregar dados para o SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
