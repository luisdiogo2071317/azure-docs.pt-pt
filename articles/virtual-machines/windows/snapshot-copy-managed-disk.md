---
title: Criar um instantâneo de um VHD no Azure | Documentos da Microsoft
description: Saiba como criar uma cópia de uma VM do Azure para utilizar como uma cópia de segurança ou para solução de problemas.
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: cynthn
ms.openlocfilehash: 0f223660f8dc99b590f8aa80a03d60fcbf3aa5d3
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50232584"
---
# <a name="create-a-snapshot"></a>Criar um instantâneo

Um instantâneo é uma cópia completa, só de leitura de uma unidade de disco rígida virtual (VHD). Pode tirar um instantâneo de um disco de SO ou dados VHD para utilizar como uma cópia de segurança ou para resolver problemas de máquina virtual (VM).

Se pretender utilizar o instantâneo para criar uma nova VM, recomendamos que encerrada corretamente a VM antes de tirar um instantâneo, para limpar todos os processos que estão em curso.

## <a name="use-the-azure-portal"></a>Utilizar o portal do Azure 

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No menu à esquerda, selecione **criar um recurso**e, em seguida, procure e selecione **instantâneo**.
3. Na **instantâneo** janela, selecione **criar**. O **criar instantâneo** é apresentada a janela.
4. Introduza um **nome** para o instantâneo.
5. Selecione um existente [grupo de recursos](../../azure-resource-manager/resource-group-overview.md#resource-groups) ou introduza o nome de um novo. 
6. Selecione uma **Localização** para o datacenter do Azure.  
7. Para **disco de origem**, selecione o disco gerido para instantâneo.
8. Selecione o **tipo de conta** utilizar para armazenar o instantâneo. Selecione **Standard_HDD**, a menos que precisa que o instantâneo sejam armazenados num disco de alto desempenho.
9. Selecione **Criar**.

## <a name="use-powershell"></a>Utilizar o PowerShell

Os passos seguintes mostram como copiar o disco VHD, crie a configuração de instantâneo e tirar um instantâneo do disco com o [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet. 

Antes de começar, certifique-se de que tem a versão mais recente do módulo Azurerm PowerShell, que deve ser a versão 5.7.0 ou posterior. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver executando o PowerShell localmente, execute [Connect-AzureRmAccount](https://docs.microsoft.com/powershell/module/azurerm.profile/connect-azurermaccount) para criar uma ligação com o Azure.

1. Defina alguns parâmetros: 

 ```azurepowershell-interactive
$resourceGroupName = 'myResourceGroup' 
$location = 'eastus' 
$vmName = 'myVM'
$snapshotName = 'mySnapshot'  
```

2. Obter a VM:

 ```azurepowershell-interactive
$vm = get-azurermvm `
   -ResourceGroupName $resourceGroupName 
   -Name $vmName
```

3. Crie a configuração de instantâneo. Neste exemplo, é o instantâneo do disco do SO:

 ```azurepowershell-interactive
$snapshot =  New-AzureRmSnapshotConfig 
   -SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id 
   -Location $location 
   -CreateOption copy
```
   
   > [!NOTE]
   > Se gostaria de armazenar o instantâneo no armazenamento resiliente para a zona, crie-o numa região que suporte [zonas de disponibilidade](../../availability-zones/az-overview.md) e incluir o `-SkuName Standard_ZRS` parâmetro.   
   
4. Tire o instantâneo:

 ```azurepowershell-interactive
New-AzureRmSnapshot 
   -Snapshot $snapshot 
   -SnapshotName $snapshotName 
   -ResourceGroupName $resourceGroupName 
```


## <a name="next-steps"></a>Passos Seguintes

Crie uma máquina virtual a partir de um instantâneo por criar um disco gerido a partir de um instantâneo e, em seguida, anexar o novo disco gerido como disco do SO. Para obter mais informações, consulte o exemplo na [criar uma VM a partir de um instantâneo com o PowerShell](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json).
