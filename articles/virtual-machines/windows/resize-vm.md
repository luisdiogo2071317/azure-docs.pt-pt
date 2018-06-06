---
title: Utilizar o PowerShell para redimensionar uma VM do Windows no Azure | Microsoft Docs
description: Redimensione a máquina virtual do Windows criada no modelo de implementação Resource Manager, com o Azure Powershell.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 057ff274-6dad-415e-891c-58f8eea9ed78
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: cynthn
ms.openlocfilehash: 8854f694ce067aaa80a159430a9f48440bcdd95a
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34701875"
---
# <a name="resize-a-windows-vm"></a>Redimensionar uma VM do Windows

Este artigo mostra como mover uma VM para outro [tamanho da VM](sizes.md) com o Azure Powershell.

Depois de criar uma máquina virtual (VM), pode dimensionar a VM ou reduzir verticalmente ao alterar o tamanho da VM. Em alguns casos, tem de Desalocação pela primeira vez a VM. Isto pode acontecer se o novo tamanho não está disponível no cluster de hardware que está atualmente a alojar a VM.

Se a VM utiliza armazenamento Premium, certifique-se de que escolhe um **s** versão do tamanho para obter suporte de armazenamento Premium. Por exemplo, escolha Standard_E4**s**_v3 em vez de Standard_E4_v3.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Redimensionar uma VM do Windows não num conjunto de disponibilidade

Defina algumas variáveis. Substitua os valores pelas suas informações.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

Lista os tamanhos VM que estão disponíveis no cluster de hardware em que a VM está alojada. 
   
```powershell
Get-AzureRmVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

Se o tamanho pretendido estiver listado, execute os seguintes comandos para redimensionar a VM. Se o tamanho pretendido não estiver listado, avance para o passo 3.
   
```powershell
$vm = Get-AzureRmVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMsize>"
Update-AzureRmVM -VM $vm -ResourceGroupName $resourceGroup
```

Se o tamanho que pretende não estiver listado, execute os seguintes comandos ao anular atribuição de VM, redimensioná-la e reiniciar a VM. Replease **<newVMsize>** com o tamanho pretendido.
   
```powershell
Stop-AzureRmVM -ResourceGroupName $resourceGroup -Name $vmName -Force
$vm = Get-AzureRmVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMSize>"
Update-AzureRmVM -VM $vm -ResourceGroupName $resourceGroup
Start-AzureRmVM -ResourceGroupName $resourceGroup -Name $vmName
```

> [!WARNING]
> Desalocação da VM versões quaisquer endereços IP dinâmicos atribuídos à VM. Os SO e discos de dados não são afetados. 
> 
> 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Redimensionar uma VM do Windows num conjunto de disponibilidade

Se o novo tamanho de uma VM num conjunto de disponibilidade não está disponível no cluster de hardware atualmente a alojar a VM, em seguida, todas as VMs no conjunto de disponibilidade terá de ser desalocada para redimensionar a VM. Também poderá ter de atualizar o tamanho das outras VMs no conjunto depois de uma VM foi redimensionada de disponibilidade. Redimensionar uma VM num conjunto de disponibilidade, execute os seguintes passos.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

Lista os tamanhos VM que estão disponíveis no cluster de hardware em que a VM está alojada. 
   
```powershell
Get-AzureRmVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

Se o tamanho pretendido estiver listado, execute os seguintes comandos para redimensionar a VM. Se não estiver listado, consulte a secção seguinte.
   
```powershell
$vm = Get-AzureRmVM -ResourceGroupName $resourceGroup -VMName $vmName 
$vm.HardwareProfile.VmSize = "<newVmSize>"
Update-AzureRmVM -VM $vm -ResourceGroupName $resourceGroup
```
    
Se o tamanho que pretende não estiver listado, continue com os seguintes passos para anular a atribuição de todas as VMs no conjunto de disponibilidade, redimensionar VMs e reiniciá-las.

Pare todas as VMs no conjunto de disponibilidade.
   
```powershell
$as = Get-AzureRmAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    Stop-AzureRmVM -ResourceGroupName $resourceGroup -Name $vmName -Force
    } 
```

Redimensionar e reiniciar as VMs no conjunto de disponibilidade.
   
```powershell
$newSize = "<newVmSize>"
$as = Get-AzureRmAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
  foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    $vm = Get-AzureRmVM -ResourceGroupName $resourceGroup -Name $vmName
    $vm.HardwareProfile.VmSize = $newSize
    Update-AzureRmVM -ResourceGroupName $resourceGroup -VM $vm
    Start-AzureRmVM -ResourceGroupName $resourceGroup -Name $vmName
    }
```

## <a name="next-steps"></a>Passos Seguintes

Para escalabilidade adicional, execute várias instâncias VM e aumentar horizontalmente. Para obter mais informações, consulte [Dimensionar automaticamente máquinas Windows de um conjunto de dimensionamento de Máquina Virtual](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).

