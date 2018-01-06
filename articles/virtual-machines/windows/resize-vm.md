---
title: Utilizar o PowerShell para redimensionar uma VM do Windows no Azure | Microsoft Docs
description: "Redimensione a máquina virtual do Windows criada no modelo de implementação Resource Manager, com o Azure Powershell."
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 057ff274-6dad-415e-891c-58f8eea9ed78
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: drewm
ms.openlocfilehash: a3d4b6e5db8835b23b014b344f0c9daae68adc09
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/06/2018
---
# <a name="resize-a-windows-vm"></a>Redimensionar uma VM do Windows
Este artigo mostra como redimensionar uma VM do Windows, criado no modelo de implementação Resource Manager com o Azure Powershell.

Depois de criar uma máquina virtual (VM), pode dimensionar a VM ou reduzir verticalmente ao alterar o tamanho da VM. Em alguns casos, tem de Desalocação pela primeira vez a VM. Isto pode acontecer se o novo tamanho não está disponível no cluster de hardware que está atualmente a alojar a VM.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Redimensionar uma VM do Windows não num conjunto de disponibilidade
1. Lista os tamanhos VM que estão disponíveis no cluster de hardware em que a VM está alojada. 
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```
2. Se o tamanho pretendido estiver listado, execute os seguintes comandos para redimensionar a VM. Se o tamanho pretendido não estiver listado, avance para o passo 3.
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -Name <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. Se o tamanho pretendido não estiver listado, execute os seguintes comandos ao anular atribuição de VM, redimensioná-la e reiniciar a VM.
   
    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -Name $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [!WARNING]
> Desalocação da VM versões quaisquer endereços IP dinâmicos atribuídos à VM. Os SO e discos de dados não são afetados. 
> 
> 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Redimensionar uma VM do Windows num conjunto de disponibilidade
Se o novo tamanho de uma VM num conjunto de disponibilidade não está disponível no cluster de hardware atualmente a alojar a VM, em seguida, todas as VMs no conjunto de disponibilidade terá de ser desalocada para redimensionar a VM. Também poderá ter de atualizar o tamanho das outras VMs no conjunto depois de uma VM foi redimensionada de disponibilidade. Redimensionar uma VM num conjunto de disponibilidade, execute os seguintes passos.

1. Lista os tamanhos VM que estão disponíveis no cluster de hardware em que a VM está alojada.
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```
2. Se o tamanho pretendido estiver listado, execute os seguintes comandos para redimensionar a VM. Se não estiver listado, avance para o passo 3.
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -Name <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. Se o tamanho pretendido não estiver listado, continue com os seguintes passos para anular a atribuição de todas as VMs no conjunto de disponibilidade, redimensionar VMs e reiniciá-las.
4. Pare todas as VMs no conjunto de disponibilidade.
   
   ```powershell
   $rg = "<resourceGroupName>"
   $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
   $vmIds = $as.VirtualMachinesReferences
   foreach ($vmId in $vmIDs){
     $string = $vmID.Id.Split("/")
     $vmName = $string[8]
     Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
   } 
   ```
5. Redimensionar e reiniciar as VMs no conjunto de disponibilidade.
   
   ```powershell
   $rg = "<resourceGroupName>"
   $newSize = "<newVmSize>"
   $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
   $vmIds = $as.VirtualMachinesReferences
   foreach ($vmId in $vmIDs){
     $string = $vmID.Id.Split("/")
     $vmName = $string[8]
     $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
     $vm.HardwareProfile.VmSize = $newSize
     Update-AzureRmVM -ResourceGroupName $rg -VM $vm
     Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
   }
   ```

## <a name="next-steps"></a>Passos Seguintes
* Para escalabilidade adicional, execute várias instâncias VM e aumentar horizontalmente. Para obter mais informações, consulte [Dimensionar automaticamente máquinas Windows de um conjunto de dimensionamento de Máquina Virtual](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).

