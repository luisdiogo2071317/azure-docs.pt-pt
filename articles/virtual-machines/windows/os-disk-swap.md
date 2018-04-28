---
title: Disco de SO de comutação para uma VM do Azure com o PowerShell | Microsoft Docs
description: Altere o disco do sistema operativo utilizado por uma máquina virtual do Azure com o PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/24/2018
ms.author: cynthn
ms.openlocfilehash: caa8fe2088995e3d30c9b808f639b9280e3a74be
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-powershell"></a>Alterar o disco do SO utilizado por uma VM do Azure com o PowerShell

Se tiver uma VM existente, mas pretende trocar o disco para um disco de cópia de segurança ou de outro disco do SO, pode utilizar o Azure PowerShell para trocar de discos de SO. Não tem de eliminar e recriar a VM. Mesmo pode utilizar um disco gerido no outro grupo de recursos, desde que não está já em utilização.

A VM tem de ser stopped\deallocated, em seguida, o ID de recurso do disco gerido pode ser substituído com o ID de recurso de disco gerido diferente.

Certifique-se de que o tipo de armazenamento e tamanho VM são compatíveis com o disco que pretende ligar. Por exemplo, se o disco que pretende utilizar no armazenamento Premium, em seguida, a VM tem de ser capaz de armazenamento Premium (como um tamanho de série DS). 

Obter uma lista de discos de um grupo de recursos utilizando [Get AzureRmDisk](/powershell/module/azurerm.compute/get-azurermdisk)

```azurepowershell-interactive
Get-AzureRmDisk -ResourceGroupName myResourceGroup | Format-Table -Property Name
```
 
Quando tiver o nome do disco que pretende utilizar, defina que como disco do SO para a VM. Este exemplo stop\deallocates VM com o nome *myVM* e atribui o disco com o nome *newDisk* como o novo disco de SO. 
 
```azurepowershell-interactive 
# Get the VM 
$vm = Get-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM 

# Make sure the VM is stopped\deallocated
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name $vm.Name -Force

# Get the new disk that you want to swap in
$disk = Get-AzureRmDisk -ResourceGroupName myResourceGroup -Name newDisk

# Set the VM configuration to point to the new disk  
Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $disk.Id -Name $disk.Name 

# Update the VM with the new OS disk
Update-AzureRmVM -ResourceGroupName myResourceGroup -VM $vm 

# Start the VM
Start-AzureRmVM -Name $vm.Name -ResourceGroupName myResourceGroup

```

**Passos seguintes?**

Para criar uma cópia de um disco, consulte [instantâneo de um disco](snapshot-copy-managed-disk.md).