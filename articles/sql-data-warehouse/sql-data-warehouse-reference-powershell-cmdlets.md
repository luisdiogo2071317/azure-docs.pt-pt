---
title: Cmdlets do PowerShell para o Azure SQL Data Warehouse
description: Encontre os principais cmdlets do PowerShell para o Azure SQL Data Warehouse, incluindo como colocar em pausa e retomar uma base de dados.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: f38c9e3bed93a77cd9b35c6d23983ee5785a34a7
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44714473"
---
# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>Cmdlets do PowerShell e APIs REST para o SQL Data Warehouse
Muitas tarefas de administração do SQL Data Warehouse podem ser geridas através de cmdlets do Azure PowerShell ou REST APIs.  Seguem-se alguns exemplos de como utilizar comandos do PowerShell para automatizar tarefas comuns no seu armazém de dados SQL.  Para alguns bons exemplos REST, consulte o artigo [gerir escalabilidade com REST][Manage scalability with REST].

> [!NOTE]
> Para utilizar o Azure PowerShell com o SQL Data Warehouse, é necessário o Azure PowerShell, versão 1.0.3 ou superior.  Pode verificar a sua versão, executando **Get-Module - ListAvailable-Name Azure**.  Pode ser instalada a versão mais recente a partir [Microsoft Web Platform Installer][Microsoft Web Platform Installer].  Para mais informações sobre como instalar a versão mais recente, consulte [How to install and configure Azure PowerShell (Como instalar e configurar o Azure PowerShell)][How to install and configure Azure PowerShell].
> 
> 

## <a name="get-started-with-azure-powershell-cmdlets"></a>Introdução aos cmdlets do PowerShell do Azure
1. Abra o Windows PowerShell.
2. Na linha de comandos do PowerShell, execute estes comandos para iniciar sessão para o Azure Resource Manager e selecione a sua subscrição.
   
    ```PowerShell
    Connect-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Exemplo de armazém de dados SQL de colocar em pausa
Colocar em pausa uma base de dados com o nome "Database02" alojada num servidor com o nome "Servidor01."  O servidor está num grupo de recursos do Azure com o nome "ResourceGroup1."

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Uma variação, neste exemplo de objeto recuperado para canaliza [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].  Como resultado, a base de dados está em pausa. O comando final mostra os resultados.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Inicie o exemplo de armazém de dados SQL
Retomar a operação de uma base de dados com o nome "Database02" alojada num servidor com o nome "Servidor01." O servidor está contido num grupo de recursos com o nome "ResourceGroup1."

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Uma variação, este exemplo obtém uma base de dados com o nome "Database02" de um servidor com o nome "Servidor01" que está contido num grupo de recursos com o nome "ResourceGroup1." Ele canaliza o objeto obtido [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase].

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [!NOTE]
> Tenha em atenção que se o servidor for foo.database.windows.net, utilize "foo" como ServerName - os cmdlets do PowerShell.
> 
> 

## <a name="other-supported-powershell-cmdlets"></a>Suporte dos outros cmdlets do PowerShell
Estes cmdlets do PowerShell são suportados com o Azure SQL Data Warehouse.

* [Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase]
* [Get-AzureRmSqlDeletedDatabaseBackup][Get-AzureRmSqlDeletedDatabaseBackup]
* [Get-AzureRmSqlDatabaseRestorePoints][Get-AzureRmSqlDatabaseRestorePoints]
* [Novo-AzureRmSqlDatabase][New-AzureRmSqlDatabase]
* [Remove-AzureRmSqlDatabase][Remove-AzureRmSqlDatabase]
* [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase]
* [Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase]
* [SELECT-AzureRmSubscription][Select-AzureRmSubscription]
* [Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase]
* [Suspender-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]

## <a name="next-steps"></a>Passos Seguintes
Para obter mais exemplos do PowerShell, consulte:

* [Criar um SQL Data Warehouse com o PowerShell][Create a SQL Data Warehouse using PowerShell]
* [Restauro de base de dados][Database restore]

Para outras tarefas que podem ser automatizadas com o PowerShell, consulte [Cmdlets de base de dados SQL do Azure][Azure SQL Database Cmdlets]. Tenha em atenção que nem todos os cmdlets da SQL Database do Azure são suportados para o Azure SQL Data Warehouse.  Para obter uma lista de tarefas que pode ser automatizada com REST, consulte [operações para a base de dados do Azure SQL][Operations for Azure SQL Database].

<!--Image references-->

<!--Article references-->
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Create a SQL Data Warehouse using PowerShell]: ./create-data-warehouse-powershell.md
[Database restore]: ./sql-data-warehouse-restore-database-powershell.md
[Manage scalability with REST]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL Database Cmdlets]: https://docs.microsoft.com/powershell/module/azurerm.sql
[Operations for Azure SQL Database]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-azurermsqldatabase
[Get-AzureRmSqlDeletedDatabaseBackup]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-AzureRmSqlDeletedDatabaseBackup
[Get-AzureRmSqlDatabaseRestorePoints]: https://docs.microsoft.com/powershell/module/azurerm.sql/get-AzureRmSqlDatabaseRestorePoints
[New-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/New-AzureRmSqlDatabase
[Remove-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Remove-AzureRmSqlDatabase
[Restore-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Restore-AzureRmSqlDatabase
[Resume-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Resume-AzureRmSqlDatabase
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Select-AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Set-AzureRmSqlDatabase
[Suspend-AzureRmSqlDatabase]: https://docs.microsoft.com/powershell/module/azurerm.sql/Suspend-AzureRmSqlDatabase

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
