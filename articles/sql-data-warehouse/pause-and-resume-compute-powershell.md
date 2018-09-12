---
title: 'Início rápido: Colocar em pausa e retomar a computação no Azure SQL Data Warehouse - PowerShell | Documentos da Microsoft'
description: Utilize o PowerShell para colocar em pausa a computação do Azure SQL Data Warehouse para reduzir os custos. Retomar a computação quando estiver pronto para utilizar o armazém de dados.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: c035ef1a79cde7c594c66964052c0653c5c709d9
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44377977"
---
# <a name="quickstart-pause-and-resume-compute-in-azure-sql-data-warehouse-with-powershell"></a>Início rápido: Colocar em pausa e retomar a computação no Azure SQL Data Warehouse com o PowerShell
Utilize o PowerShell para colocar em pausa a computação do Azure SQL Data Warehouse para reduzir os custos. [Retomar a computação](sql-data-warehouse-manage-compute-overview.md) quando estiver pronto para utilizar o armazém de dados.

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

Este tutorial requer a versão 5.1.1 ou posterior do módulo do Azure PowerShell. Execute ` Get-Module -ListAvailable AzureRM` para localizar a versão que tem atualmente. Se precisar de instalar ou atualizar, veja [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps)(Instalar o módulo do Azure PowerShell).

## <a name="before-you-begin"></a>Antes de começar

Este guia de introdução pressupõe que já tiver um armazém de dados SQL que pode colocar em pausa e retomar. Se precisar de criar uma, pode utilizar [criar e ligar - portal](create-data-warehouse-portal.md) para criar um armazém de dados chamado **mySampleDataWarehouse**.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie a sessão na sua subscrição do Azure com o comando [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) e siga as instruções no ecrã.

```powershell
Connect-AzureRmAccount
```

Para ver a subscrição que está a utilizar, execute [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription).

```powershell
Get-AzureRmSubscription
```

Se precisar de utilizar uma subscrição diferente que não a predefinida, execute [Select-AzureRmSubscription](/powershell/module/azurerm.profile/select-azurermsubscription).

```powershell
Select-AzureRmSubscription -SubscriptionName "MySubscription"
```

## <a name="look-up-data-warehouse-information"></a>Procure pelas informações de armazém de dados

Localize o nome da base de dados, nome do servidor e grupo de recursos para o armazém de dados que planeia colocar em pausa e retomar.

Siga estes passos para encontrar as informações de localização para o seu armazém de dados.

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
2. Clique em **bases de dados SQL** na página da esquerda do Portal do Azure.
3. Selecione **mySampleDataWarehouse** da página de **bases de dados SQL**. O armazém de dados é aberto.

    ![O nome do servidor e grupo de recursos](media/pause-and-resume-compute-powershell/locate-data-warehouse-information.png)

4. Anote o nome de armazém de dados, o que é o nome de base de dados. Anote também o nome do servidor e do grupo de recursos. Você
5.  eles nos comandos colocar em pausa e retomar.
6. Se o servidor for foo.database.windows.net, utilize apenas a primeira parte como nome de servidor nos cmdlets do PowerShell. Na imagem anterior, o nome completo do servidor é newserver-20171113.database.windows.net. Remova o sufixo e utilizar **newserver 20171113** como o nome do servidor no cmdlet do PowerShell.

## <a name="pause-compute"></a>Computação em pausa
Para reduzir os custos, pode colocar em pausa e retomar a computação recursos sob demanda. Por exemplo, se não estiver a utilizar a base de dados durante a noite e no fim de semana, pode colocar em pausa durante essas horas e retomá-lo durante o dia. Não existe nenhum custo associado para recursos de computação, enquanto a base de dados está em pausa. No entanto, continuar a ser cobrado o armazenamento.

Para colocar em pausa uma base de dados, utilize o [Suspend-AzureRmSqlDatabase](/powershell/module/azurerm.sql/suspend-azurermsqldatabase) cmdlet. O exemplo seguinte coloca em pausa um armazém de dados com o nome **mySampleDataWarehouse** alojada num servidor com o nome **newserver 20171113**. O servidor está num grupo de recursos do Azure com o nome **myResourceGroup**.


```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" –DatabaseName "mySampleDataWarehouse"
```

Uma variação, neste exemplo seguinte obtém a base de dados para o objeto de $database. Ele encaminha, então, o objeto [Suspend-AzureRmSqlDatabase](/powershell/module/azurerm.sql/suspend-azurermsqldatabase). Os resultados são armazenados no resultDatabase objeto. O comando final mostra os resultados.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" –DatabaseName "mySampleDataWarehouse"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```


## <a name="resume-compute"></a>Computação de retomar
Para iniciar uma base de dados, utilize o [Resume-AzureRmSqlDatabase](/powershell/module/azurerm.sql/resume-azurermsqldatabase) cmdlet. O exemplo seguinte inicia um banco de dados chamado mySampleDataWarehouse alojada num servidor com o nome newserver 20171113. O servidor está num grupo de recursos do Azure com o nome myResourceGroup.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "myResourceGroup" `
–ServerName "newserver-20171113" -DatabaseName "mySampleDataWarehouse"
```

Uma variação, neste exemplo seguinte obtém a base de dados para o objeto de $database. Ele encaminha, então, o objeto [Resume-AzureRmSqlDatabase](/powershell/module/azurerm.sql/resume-azurermsqldatabase) e armazena os resultados em $resultDatabase. O comando final mostra os resultados.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

## <a name="clean-up-resources"></a>Limpar recursos

Estão a ser-lhe cobradas as unidades do armazém de dados e os dados armazenados no mesmo. Estes recursos de computação e armazenamento são faturados em separado.

- Se pretender manter os dados no armazenamento, interromper a computação.
- Se quiser remover futuras cobranças, pode eliminar o armazém de dados.

Siga estes passos para limpar os recursos conforme quiser.

1. Inicie sessão para o [portal do Azure](https://portal.azure.com)e clique no seu armazém de dados.

    ![Limpar recursos](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

1. Para interromper a computação, clique no botão **Pausar**. Quando o armazém de dados estiver em pausa, verá um botão **Iniciar**.  Para retomar a computação, clique em **Iniciar**.

2. Para remover o armazém de dados para não lhe ser cobrada a computação ou o armazenamento, clique em **Eliminar**.

3. Para remover o servidor SQL que criou, clique em **mynewserver 20171113.database.windows.net**e, em seguida, clique em **eliminar**.  Tenha cuidado com esta eliminação, uma vez que eliminar o servidor também elimina todas as bases de dados atribuídas ao mesmo.

4. Para remover o grupo de recursos, clique em **myResourceGroup** e, em seguida, clique em **Eliminar grupo de recursos**.


## <a name="next-steps"></a>Passos Seguintes
Agora está em pausa e retomar a computação para o seu armazém de dados. Para saber mais sobre o Azure SQL Data Warehouse, avance para o tutorial para carregar dados.

> [!div class="nextstepaction"]
>[Carregar dados para o SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
