---
title: Criar um instantâneo de um VHD no Azure | Microsoft Docs
description: Saiba como criar uma cópia de uma VM do Azure para utilizar como um back cópias de segurança ou de resolução de problemas.
documentationcenter: ''
author: cynthn
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 15eb778e-fc07-45ef-bdc8-9090193a6d20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/10/2018
ms.author: cynthn
ms.openlocfilehash: d7315d3fb7fc156beb85271d0e5aa19ec6baa7a9
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="create-a-snapshot"></a>Criar um instantâneo

Tire um instantâneo de um disco de SO ou dados problemas de VHD para cópia de segurança ou para resolver problemas de VM. Um instantâneo é uma cópia completa só de leitura de um VHD. 

## <a name="use-azure-portal-to-take-a-snapshot"></a>Utilize o portal do Azure para criar um instantâneo 

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. A partir do canto superior esquerdo, clique em **crie um recurso** e procure **instantâneo**.
3. No painel do instantâneo, clique em **criar**.
4. Introduza um **nome** para o instantâneo.
5. Selecione um [Grupo de recursos](../../azure-resource-manager/resource-group-overview.md#resource-groups) ou escreva o nome para um novo. 
6. Selecione um localização do datacenter do Azure.  
7. Para **disco de origem**, selecione o disco de gerido para instantâneo.
8. Selecione o **tipo de conta** utilizar para armazenar o instantâneo. Recomendamos **Standard_LRS** , a menos que o ficheiro necessário armazenados num disco elevado desempenho.
9. Clique em **Criar**.

## <a name="use-powershell-to-take-a-snapshot"></a>Utilize o PowerShell para criar um instantâneo

Os passos seguintes mostram como obter o disco VHD para ser copiado, criar as configurações de instantâneo e tirar um instantâneo do disco utilizando o [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet. 

Antes de começar, certifique-se de que tem a versão mais recente do módulo do AzureRM.Compute PowerShell. Este artigo requer a versão do módulo 5.7.0 de AzureRM ou posterior. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Connect-AzureRmAccount` para criar uma ligação com o Azure.

Defina alguns parâmetros. 

 ```azurepowershell-interactive
$resourceGroupName = 'myResourceGroup' 
$location = 'eastus' 
$vmName = 'myVM'
$snapshotName = 'mySnapshot'  
```

Obter a VM.

 ```azurepowershell-interactive
$vm = get-azurermvm `
   -ResourceGroupName $resourceGroupName `
   -Name $vmName
```

Crie a configuração de instantâneo. Neste exemplo, vamos instantâneo o disco do SO.

 ```azurepowershell-interactive
$snapshot =  New-AzureRmSnapshotConfig `
   -SourceUri $vm.StorageProfile.OsDisk.ManagedDisk.Id `
   -Location $location `
   -CreateOption copy
```
   
> [!NOTE]
> Se pretende armazenar o instantâneo no armazenamento resiliente para a zona, terá de criá-la numa região que suporte [zonas de disponibilidade](../../availability-zones/az-overview.md) e incluir o `-SkuName Standard_ZRS` parâmetro.   

   
Tire o instantâneo.

```azurepowershell-interactive
New-AzureRmSnapshot `
   -Snapshot $snapshot `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName 
```




## <a name="next-steps"></a>Passos Seguintes

Crie uma máquina virtual a partir de um instantâneo ao criar um disco gerido a partir de um instantâneo e, em seguida, anexar o disco novo gerido como disco do SO. Para obter mais informações, consulte o [criar uma VM a partir de um instantâneo](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) exemplo.
