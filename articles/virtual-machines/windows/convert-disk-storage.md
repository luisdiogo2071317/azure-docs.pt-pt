---
title: Converter os Azure managed armazenamento de discos de standard para premium e vice-versa | Documentos da Microsoft
description: Como converter do Azure gerido discos de standard para premium e vice-versa, com o Azure PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: ramankumarlive
manager: kavithag
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/04/2018
ms.author: ramankum
ms.subservice: disks
ms.openlocfilehash: 676adb05178097c1802c0ff5b3a5da1bbb7b188c
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2019
ms.locfileid: "55978379"
---
# <a name="update-the-storage-type-of-a-managed-disk"></a>Atualizar o tipo de armazenamento de um disco gerido

Managed Disks do Azure oferece três opções de tipo de armazenamento: [Premium SSD](../windows/premium-storage.md), [Standard SSD](../windows/disks-standard-ssd.md), e [HDD padrão](../windows/standard-storage.md). Pode optar por um disco gerido entre tipos de armazenamento com o tempo de inatividade mínimo, com base nas suas necessidades de desempenho. Alternar entre tipos de armazenamento não é suportada para um disco não gerido; No entanto, pode facilmente [converter um disco não gerido para um disco gerido](convert-unmanaged-to-managed-disks.md).

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="prerequisites"></a>Pré-requisitos

* Uma vez que a conversão necessita de um reinício da máquina virtual (VM), deve agendar a migração do armazenamento de discos durante uma janela de manutenção pré-existente. 
* Se estiver a utilizar um disco não gerido, comece [convertê-lo para um disco gerido](convert-unmanaged-to-managed-disks.md) para que possa alterá-la entre os tipos de armazenamento. 
* Os exemplos neste artigo requerem o Azure PowerShell versão do módulo 6.0.0 ou posterior. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/azurerm/install-az-ps). Execute [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) para criar uma ligação com o Azure.


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium"></a>Converter todos os discos geridos de uma VM de standard para premium

O exemplo seguinte mostra como mudar todos os discos de uma VM de standard para o armazenamento premium. Para utilizar discos geridos premium, a VM tem de utilizar um [tamanho da VM](sizes.md) que suporta o armazenamento premium. Este exemplo também muda para um tamanho que suporte o armazenamento premium:

```azurepowershell-interactive
# Name of the resource group that contains the VM
$rgName = 'yourResourceGroup'

# Name of the your virtual machine
$vmName = 'yourVM'

# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'Premium_LRS'

# Premium capable size
# Required only if converting storage from standard to premium
$size = 'Standard_DS2_v2'

# Stop and deallocate the VM before changing the size
Stop-AzVM -ResourceGroupName $rgName -Name $vmName -Force

$vm = Get-AzVM -Name $vmName -resourceGroupName $rgName

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to premium storage
foreach ($disk in $vmDisks)
{
    if ($disk.ManagedBy -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzDiskUpdateConfig –AccountType $storageType
        Update-AzDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
        -DiskName $disk.Name
    }
}

Start-AzVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="convert-a-managed-disk-from-standard-to-premium"></a>Converter um disco gerido de standard para premium

Para sua carga de trabalho de programador/teste, poderá ter uma mistura de discos standard e premium para reduzir o custo. Para tal, atualize para o armazenamento premium só os discos que exigem um melhor desempenho. O exemplo seguinte mostra como alternar de um único disco de uma VM do plano padrão para o armazenamento premium e vice-versa. Para utilizar discos geridos premium, a VM tem de utilizar um [tamanho da VM](sizes.md) que suporta o armazenamento premium. Este exemplo também mostra como mudar para um tamanho que suporte o armazenamento premium:

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'Premium_LRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzVM -VM $vm -ResourceGroupName $rgName

# Update the storage type
$diskUpdateConfig = New-AzDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="convert-a-managed-disk-from-standard-hdd-to-standard-ssd"></a>Converter um disco gerido standard HDD para standard SSD

O exemplo seguinte mostra como alternar de um único disco de uma VM do padrão HDD para SSD standard e vice-versa:

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between Standard_LRS and StandardSSD_LRS based on your scenario
$storageType = 'StandardSSD_LRS'

$disk = Get-AzDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

# Update the storage type
$diskUpdateConfig = New-AzDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="next-steps"></a>Passos Seguintes

Faça uma cópia só de leitura de uma VM com um [instantâneo](snapshot-copy-managed-disk.md).

