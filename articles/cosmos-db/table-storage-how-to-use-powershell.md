---
title: "Efetuar operações de armazenamento de tabelas do Azure com o PowerShell | Microsoft Docs"
description: "Efetuar operações de armazenamento de tabelas do Azure com o PowerShell"
services: cosmos-db
documentationcenter: storage
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2018
ms.author: robinsh
ms.openlocfilehash: c09809e9cf513dbb9420f675bbf431c176f740bd
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="perform-azure-table-storage-operations-with-azure-powershell"></a>Efetuar operações de armazenamento de tabelas do Azure com o Azure PowerShell 
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

Table storage do Azure é um arquivo de dados NoSQL que pode utilizar para armazenar e consultar conjuntos enormes de dados estruturados não relacionais. Componentes principais do serviço são propriedades, tabelas e entidades. Uma tabela é uma coleção de entidades. Uma entidade é um conjunto de propriedades. Cada entidade pode ter até 252 propriedades que são todos os pares nome-valor. Este artigo pressupõe que já estiver familiarizado com os conceitos do serviço de armazenamento de tabela do Azure. Para obter informações detalhadas, consulte [compreender o modelo de dados do serviço tabela](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model) e [introdução ao Table storage do Azure através do .NET](table-storage-how-to-use-dotnet.md).

Este artigo procedimentos abrange operações comuns do armazenamento de tabelas do Azure. Saiba como: 

> [!div class="checklist"]
> * Criar uma tabela
> * Obter uma tabela
> * Adicione as entidades da tabela
> * Consulta uma tabela
> * Eliminar entidades da tabela
> * Eliminar uma tabela

Este artigo procedimentos mostra como criar uma nova conta de armazenamento do Azure num novo grupo de recursos, pelo que pode remover facilmente quando tiver terminado. Se em vez disso seria utilizar uma conta de armazenamento existente, pode fazê-lo em vez disso.

Os exemplos necessitam do Azure PowerShell versão do módulo 4.4.0 ou posterior. Na janela do PowerShell, execute `Get-Module -ListAvailable AzureRM` para localizar a versão. Se será apresentado nada ou terá de atualizar, consulte [módulo Azure PowerShell instalar](/powershell/azure/install-azurerm-ps). 

Depois do Azure PowerShell está instalado ou atualizado, tem de instalar o módulo **AzureRmStorageTable**, que tem os comandos para gerir as entidades. Para instalar este módulo, executar o PowerShell como administrador e utilize o **Install-Module** comando.

```powershell
Install-Module AzureRmStorageTable
```

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão na sua subscrição do Azure com o comando `Login-AzureRmAccount` e siga as instruções no ecrã.

```powershell
Login-AzureRmAccount
```

## <a name="retrieve-list-of-locations"></a>Obter a lista de localizações

Se não souber qual a localização que quer utilizar, pode listar as localizações disponíveis. Depois de a lista ser apresentada, localize a que quer utilizar. Utilizam estes exemplos **eastus**. Armazenar este valor na variável **localização** para utilização futura.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Criar grupo de recursos

Crie um grupo de recursos com o comando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/New-AzureRmResourceGroup). 

Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. Armazene o nome do grupo de recursos numa variável para utilização futura. Neste exemplo, um grupo de recursos denominado *pshtablesrg* é criado no *eastus* região.

```powershell
$resourceGroup = "pshtablesrg"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Criar conta de armazenamento

Criar uma conta de armazenamento para fins gerais standard com a utilização do armazenamento localmente redundante (LRS) [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount). Obter o contexto da conta de armazenamento que define a conta de armazenamento a ser utilizado. Quando a atuar numa conta de armazenamento, referenciar o contexto em vez de repetidamente fornecer as credenciais.

```powershell
$storageAccountName = "pshtablestorage"
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
```

## <a name="create-a-new-table"></a>Criar uma nova tabela

Para criar uma tabela, utilize o [New-AzureStorageTable](/powershell/module/azure.storage/New-AzureStorageTable) cmdlet. Neste exemplo, a tabela é chamada `pshtesttable`.

```powershell
$tableName = "pshtesttable"
New-AzureStorageTable –Name $tableName –Context $ctx
```

## <a name="retrieve-a-list-of-tables-in-the-storage-account"></a>Obter uma lista de tabelas na conta de armazenamento

Obter uma lista de tabelas a conta de armazenamento utilizando [Get-AzureStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable).

```powershell
Get-AzureStorageTable –Context $ctx | select Name
```

## <a name="retrieve-a-reference-to-a-specific-table"></a>Obter uma referência a uma tabela específica

Para executar operações numa tabela, precisa de uma referência à tabela específica. Obter uma referência utilizando [Get-AzureStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable). 

```powershell
$storageTable = Get-AzureStorageTable –Name $tableName –Context $ctx
```

[!INCLUDE [storage-table-entities-powershell-include](../../includes/storage-table-entities-powershell-include.md)]

## <a name="delete-a-table"></a>Eliminar uma tabela

Para eliminar uma tabela, utilize [remover AzureStorageTable](/powershell/module/azure.storage/Remove-AzureStorageTable). Este cmdlet Remove a tabela, incluindo todos os seus dados.

```powershell
Remove-AzureStorageTable –Name $tableName –Context $ctx

# Retrieve the list of tables to verify the table has been removed.
Get-AzureStorageTable –Context $Ctx | select Name
```

## <a name="clean-up-resources"></a>Limpar recursos

Se tiver criado uma nova conta de armazenamento e de grupo de recursos no início deste procedimentos, pode remover todos os recursos que criou neste exercício ao remover o grupo de recursos. Este comando elimina todos os recursos contidos no grupo, bem como o grupo de recursos em si.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Passos Seguintes

Este artigo procedimentos, aprendeu sobre operações comuns do armazenamento de tabelas do Azure com o PowerShell, incluindo como: 

> [!div class="checklist"]
> * Criar uma tabela
> * Obter uma tabela
> * Adicione as entidades da tabela
> * Consulta uma tabela
> * Eliminar entidades da tabela
> * Eliminar uma tabela

Para obter mais informações, consulte os artigos seguintes

* [Cmdlets do Armazenamento do PowerShell](/powershell/module/azurerm.storage#storage)

* [Trabalhar com tabelas de armazenamento do Azure a partir do PowerShell](https://blogs.technet.microsoft.com/paulomarques/2017/01/17/working-with-azure-storage-tables-from-powershell/)

* O [Explorador de Armazenamento do Microsoft Azure](../vs-azure-tools-storage-manage-with-storage-explorer.md) é uma aplicação autónoma e gratuita da Microsoft, que lhe permite trabalhar visualmente com dados do Armazenamento do Azure no Windows, macOS e Linux.