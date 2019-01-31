---
title: Efetuar operações de armazenamento de tabelas do Azure com o PowerShell | Documentos da Microsoft
description: Efetue operações de armazenamento de tabelas do Azure com o PowerShell.
services: cosmos-db
author: roygara
ms.service: cosmos-db
ms.topic: article
ms.date: 03/14/2018
ms.author: rogarana
ms.subservice: cosmosdb-table
ms.openlocfilehash: 8993aea208e4ccdcf92f676cc07f2912979da606
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55477001"
---
# <a name="perform-azure-table-storage-operations-with-azure-powershell"></a>Efetuar operações de armazenamento de tabelas do Azure com o Azure PowerShell 
[!INCLUDE [storage-table-cosmos-db-tip-include](../../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

Armazenamento de tabelas do Azure é um arquivo de dados NoSQL que pode utilizar para armazenar e consultar conjuntos enormes de dados estruturados não relacionais. Os principais componentes do serviço são tabelas, entidades e propriedades. Uma tabela é uma coleção de entidades. Uma entidade é um conjunto de propriedades. Cada entidade pode ter até 252 propriedades, o que são todos os pares de nome-valor. Este artigo pressupõe que já está familiarizado com os conceitos do serviço de armazenamento de tabelas do Azure. Para obter informações detalhadas, consulte [Noções básicas sobre o modelo de dados do serviço de tabela](/rest/api/storageservices/Understanding-the-Table-Service-Data-Model) e [introdução ao armazenamento de tabelas do Azure com o .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md).

Este artigo que mostra como abrange operações comuns de armazenamento de tabelas do Azure. Saiba como: 

> [!div class="checklist"]
> * Criar uma tabela
> * Recuperar uma tabela
> * Adicionar entidades de tabelas
> * Consultar uma tabela
> * Eliminar entidades de tabelas
> * Eliminar uma tabela

Este artigo de procedimento mostra-lhe como criar uma nova conta de armazenamento do Azure num grupo de recursos novo para que pode facilmente removê-lo quando tiver terminado. Se usaria em vez disso, uma conta de armazenamento existente, pode fazer isso em vez disso.

Os exemplos exigem o módulo Azure PowerShell `AzureRM` versão 4.4.0 ou posterior. Numa janela do PowerShell, execute `Get-Module -ListAvailable AzureRM` para localizar a versão. Se não será apresentado nada ou precisar de atualizar, veja [módulo de instalar o Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps).

[!INCLUDE [requires-azurerm](../../../includes/requires-azurerm.md)]

Depois do Azure PowerShell está instalado ou atualizado, tem de instalar o módulo **AzureRmStorageTable**, que tem os comandos para gerir as entidades. Para instalar este módulo, execute o PowerShell como administrador e utilize o **Install-Module** comando.

```powershell
Install-Module AzureRmStorageTable
```

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão na sua subscrição do Azure com o comando `Connect-AzureRmAccount` e siga as instruções no ecrã.

```powershell
Connect-AzureRmAccount
```

## <a name="retrieve-list-of-locations"></a>Obter a lista de localizações

Se não souber qual a localização que quer utilizar, pode listar as localizações disponíveis. Depois de a lista ser apresentada, localize a que quer utilizar. Estes exemplos utilizam **eastus**. Store este valor na variável **localização** para utilização futura.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-resource-group"></a>Criar grupo de recursos

Crie um grupo de recursos com o comando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/New-AzureRmResourceGroup). 

Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. Store o nome do grupo de recursos numa variável para utilização futura. Neste exemplo, um grupo de recursos chamado *pshtablesrg* é criado na *eastus* região.

```powershell
$resourceGroup = "pshtablesrg"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

## <a name="create-storage-account"></a>Criar conta de armazenamento

Criar uma conta de armazenamento para fins gerais com armazenamento localmente redundante (LRS) usando [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount). Obtenha o contexto de conta de armazenamento que define a conta de armazenamento a ser utilizado. Ao efetuar ações em contas de armazenamento, referencia o contexto em vez de fornecer repetidamente as credenciais.

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

Obter uma lista de tabelas na conta de armazenamento através de [Get-AzureStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable).

```powershell
Get-AzureStorageTable –Context $ctx | select Name
```

## <a name="retrieve-a-reference-to-a-specific-table"></a>Recuperar uma referência a uma tabela específica

Para efetuar operações numa tabela, precisa de uma referência para a tabela específica. Obter uma referência a utilizar [Get-AzureStorageTable](/powershell/module/azure.storage/Get-AzureStorageTable). 

```powershell
$storageTable = Get-AzureStorageTable –Name $tableName –Context $ctx
```

[!INCLUDE [storage-table-entities-powershell-include](../../../includes/storage-table-entities-powershell-include.md)]

## <a name="delete-a-table"></a>Eliminar uma tabela

Para eliminar uma tabela, utilize [Remove-AzureStorageTable](/powershell/module/azure.storage/Remove-AzureStorageTable). Este cmdlet Remove a tabela, incluindo todos os seus dados.

```powershell
Remove-AzureStorageTable –Name $tableName –Context $ctx

# Retrieve the list of tables to verify the table has been removed.
Get-AzureStorageTable –Context $Ctx | select Name
```

## <a name="clean-up-resources"></a>Limpar recursos

Se tiver criado uma nova conta de armazenamento e de grupo de recursos no início nesta explicação de procedimento, pode remover todos os ativos que criou neste exercício, removendo o grupo de recursos. Este comando elimina todos os recursos contidos dentro do grupo, bem como o grupo de recursos.

```powershell
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo de procedimentos, aprendeu sobre operações comuns do armazenamento de tabelas do Azure com o PowerShell, incluindo como: 

> [!div class="checklist"]
> * Criar uma tabela
> * Recuperar uma tabela
> * Adicionar entidades de tabelas
> * Consultar uma tabela
> * Eliminar entidades de tabelas
> * Eliminar uma tabela

Para obter mais informações, veja os artigos seguintes

* [Cmdlets do Armazenamento do PowerShell](/powershell/module/azurerm.storage#storage)

* [Trabalhar com tabelas de armazenamento do Azure a partir do PowerShell](https://blogs.technet.microsoft.com/paulomarques/2017/01/17/working-with-azure-storage-tables-from-powershell/)

* O [Explorador de Armazenamento do Microsoft Azure](../../vs-azure-tools-storage-manage-with-storage-explorer.md) é uma aplicação autónoma e gratuita da Microsoft, que lhe permite trabalhar visualmente com dados do Armazenamento do Azure no Windows, macOS e Linux.
