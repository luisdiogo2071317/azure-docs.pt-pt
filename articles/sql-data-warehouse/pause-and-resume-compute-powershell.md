---
title: "Início rápido: Colocar em pausa e retomar de computação no armazém de dados de SQL do Azure - PowerShell | Microsoft Docs"
description: "As tarefas do PowerShell que colocar em pausa para um Azure SQL Data Warehouse reduzir os custos de computação. Retomar a computação quando estiver pronto para utilizar o armazém de dados."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 01/25/2018
ms.author: barbkess
ms.openlocfilehash: 799210366978c68a390fa6d671184e94cf021301
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="quickstart-pause-and-resume-compute-for-an-azure-sql-data-warehouse-in-powershell"></a>Início rápido: Colocar em pausa e retomar a computação para um Azure SQL Data Warehouse no PowerShell
Utilize o PowerShell para colocar em pausa computação para um Azure SQL Data Warehouse reduzir os custos. Retomar a computação quando estiver pronto para utilizar o armazém de dados.

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

Este tutorial requer o Azure PowerShell versão do módulo 5.1.1 ou posterior. Executar ` Get-Module -ListAvailable AzureRM` para encontrar a versão de que tem atualmente. Se precisar de instalar ou atualizar, veja [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps.md)(Instalar o módulo do Azure PowerShell). 

## <a name="before-you-begin"></a>Antes de começar

Este guia de introdução pressupõe que já tiver um armazém de dados do SQL Server que pode colocar em pausa e retomar. Se precisar de criar uma, pode utilizar [criar e ligar - portal](create-data-warehouse-portal.md) para criar um armazém de dados chamado **mySampleDataWarehouse**. 

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
5. Se o servidor for foo.database.windows.net, utilize apenas a primeira parte como o nome do servidor nos cmdlets do PowerShell. Na imagem anterior, o nome de servidor completo é newserver 20171113.database.windows.net. Utilizaremos **newserver 20171113** como o nome do servidor no cmdlet do PowerShell.

## <a name="pause-compute"></a>Computação pausa
Para reduzir os custos, pode colocar em pausa e retomar a computação recursos a pedido. Por exemplo, se não utilizar a base de dados durante a noite e no fim de semana, pode colocar em pausa-lo durante essas horas e retomá-lo durante o dia. Não lhe será cobrado recursos de computação enquanto a base de dados está em pausa. No entanto, continuará a ser-lhe cobrados de armazenamento. 

Para colocar em pausa uma base de dados, utilize o [Suspend-AzureRmSqlDatabase](/powershell/module/azurerm.sql/suspend-azurermsqldatabase.md) cmdlet. O exemplo seguinte interrompe um armazém de dados com o nome **mySampleDataWarehouse** alojada num servidor com o nome **newserver 20171113**. O servidor está num grupo de recursos do Azure com o nome **myResourceGroup**.


```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" –DatabaseName "mySampleDataWarehouse"
```

Uma variação, neste exemplo seguinte obtém a base de dados para o objeto de $database. -Lo, em seguida, encaminha o objecto a [Suspend-AzureRmSqlDatabase](/powershell/module/azurerm.sql/suspend-azurermsqldatabase). Os resultados são armazenados no resultDatabase objeto. O comando final mostra os resultados.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" –DatabaseName "mySampleDataWarehouse"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```


## <a name="resume-compute"></a>Retomar de computação
Para iniciar uma base de dados, utilize o [retoma-AzureRmSqlDatabase](/powershell/module/azurerm.sql/resume-azurermsqldatabase) cmdlet. O exemplo seguinte inicia uma base de dados com o nome mySampleDataWarehouse alojada num servidor com o nome newserver 20171113. O servidor está num grupo de recursos do Azure com o nome myResourceGroup.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" -DatabaseName "mySampleDataWarehouse"
```

Uma variação, neste exemplo seguinte obtém a base de dados para o objeto de $database. -Lo, em seguida, encaminha o objecto a [retoma-AzureRmSqlDatabase](/powershell/module/azurerm.sql/resume-azurermsqldatabase.md) e armazena os resultados no $resultDatabase. O comando final mostra os resultados.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

## <a name="clean-up-resources"></a>Limpar recursos

Estão a ser-lhe cobradas as unidades do armazém de dados e os dados armazenados no mesmo. Estes recursos de computação e armazenamento são faturados em separado. 

- Se pretender manter os dados no armazenamento, coloque em pausa computação.
- Se quiser remover futuras cobranças, pode eliminar o armazém de dados. 

Siga estes passos para limpar os recursos conforme quiser.

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com)e clique no seu armazém de dados.

    ![Limpar recursos](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

1. Para interromper a computação, clique no botão **Pausar**. Quando o armazém de dados estiver em pausa, verá um botão **Iniciar**.  Para retomar a computação, clique em **Iniciar**.

2. Para remover o armazém de dados para não lhe ser cobrada a computação ou o armazenamento, clique em **Eliminar**.

3. Para remover o servidor SQL que criou, clique em **mynewserver 20171113.database.windows.net**e, em seguida, clique em **eliminar**.  Tenha cuidado com esta eliminação, uma vez que eliminar o servidor também elimina todas as bases de dados atribuídas ao mesmo.

4. Para remover o grupo de recursos, clique em **myResourceGroup** e, em seguida, clique em **Eliminar grupo de recursos**.


## <a name="next-steps"></a>Passos Seguintes
Agora que tem em pausa e retomar a computação para o seu armazém de dados. Para saber mais sobre o Azure SQL Data Warehouse, avance para o tutorial para carregar dados.

> [!div class="nextstepaction"]
>[Carregar dados para o SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)