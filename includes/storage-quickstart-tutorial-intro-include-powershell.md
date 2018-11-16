---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 027b370d2497822dcbd6f3958556357957f9e8f5
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/16/2018
ms.locfileid: "51817882"
---
## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão na sua subscrição do Azure com o comando `Connect-AzureRmAccount` e siga as instruções no ecrã.

```powershell
Connect-AzureRmAccount
```

Se não souber qual a localização que quer utilizar, pode listar as localizações disponíveis. Apresente a lista de localizações com o seguinte exemplo de código e encontre o que pretende utilizar. Este exemplo utiliza **eastus**. Armazene a localização numa variável e utilize-a de forma a poder alterá-la num único local.

```powershell
Get-AzureRmLocation | select Location 
$location = "eastus"
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos do Azure com [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos. 

```powershell
$resourceGroup = "myResourceGroup"
New-AzureRmResourceGroup -Name $resourceGroup -Location $location 
```

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento

Crie uma conta de armazenamento para fins gerais padrão com replicação LRS através do cmdlet [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/New-AzureRmStorageAccount). Em seguida, obtenha o contexto da conta de armazenamento que define a conta de armazenamento que pretende utilizar. Ao realizar ações em contas de armazenamento, referencia o contexto em vez de transmitir repetidamente as credenciais. Utilize o exemplo a seguir para criar uma conta de armazenamento denominada *mystorageaccount* com armazenamento localmente redundante (LRS) e encriptação de blobs (ativada por predefinição).

```powershell
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
  -Name "mystorageaccount" `
  -SkuName Standard_LRS `
  -Location $location `
  -Kind Storage

$ctx = $storageAccount.Context
```
