---
title: Converter os Azure managed armazenamento de discos de standard para premium e vice-versa | Documentos da Microsoft
description: Como converter do Azure gerido discos de standard para premium e vice-versa, com o Azure PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: ramankum
manager: kavithag
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: ramankum
ms.openlocfilehash: 62cb906000999e6ed97be76c4fe9b15e9ec1cb91
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39092475"
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Converter os Azure managed armazenamento de discos de standard para premium e vice-versa

Discos geridos oferece três opções de armazenamento: [Premium SSD](../windows/premium-storage.md), SSD(Preview) padrão, e [Standard HDD](../windows/standard-storage.md). Permite-lhe alternar facilmente entre as opções com o tempo de inatividade mínimo, com base nas suas necessidades de desempenho. Isto não é suportado para discos não geridos. Mas pode facilmente [converter para discos geridos](convert-unmanaged-to-managed-disks.md) alternar facilmente entre os tipos de disco.

Este artigo mostra-lhe como converter discos geridos de standard para premium e vice-versa com o Azure PowerShell. Se precisar de instalar ou atualizá-lo, veja [instalar e configurar o Azure PowerShell](/powershell/azure/install-azurerm-ps.md).

## <a name="before-you-begin"></a>Antes de começar

* A conversão necessita de um reinício da VM, por isso, agende a migração do armazenamento de discos durante uma janela de manutenção pré-existente. 
* Se estiver a utilizar discos não geridos, primeiro [converter para discos geridos](convert-unmanaged-to-managed-disks.md) para utilizar este artigo para alternar entre as opções de armazenamento. 
* Este artigo requer o Azure PowerShell versão do módulo 6.0.0 ou posterior. Executar ` Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Também precisa de executar `Connect-AzureRmAccount` para criar uma ligação ao Azure.


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a>Converter todos os discos geridos de uma VM de standard para premium e vice-versa

O exemplo seguinte mostra como mudar todos os discos de uma VM de standard para o armazenamento premium. Para utilizar discos geridos premium, a VM tem de utilizar um [tamanho da VM](sizes.md) que suporta o armazenamento premium. Este exemplo também muda para um tamanho que suporte o armazenamento premium.

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
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force

$vm = Get-AzureRmVM -Name $vmName -resourceGroupName $rgName

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzureRmDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to premium storage
foreach ($disk in $vmDisks)
{
    if ($disk.ManagedBy -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzureRmDiskUpdateConfig –AccountType $storageType
        Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
        -DiskName $disk.Name
    }
}

Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a>Converter um disco gerido de standard para premium e vice-versa

Para sua carga de trabalho de programador/teste, pode querer ter a mistura de discos standard e premium para reduzir o custo. Poderá fazê-lo com a atualização para o armazenamento premium, apenas os discos que exigem um melhor desempenho. O exemplo seguinte mostra como alternar de um único disco de uma VM do plano padrão para o armazenamento premium e vice-versa. Para utilizar discos geridos premium, a VM tem de utilizar um [tamanho da VM](sizes.md) que suporta o armazenamento premium. Este exemplo também muda para um tamanho que suporte o armazenamento premium.

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'Premium_LRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzureRmDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzureRmResource -ResourceId $disk.diskId

# Stop and deallocate the VM before changing the storage type
Stop-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name -Force

$vm = Get-AzureRmVM $vmResource.ResourceGroupName -Name $vmResource.ResourceName 

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzureRmVM -VM $vm -ResourceGroupName $rgName

# Update the storage type
$diskUpdateConfig = New-AzureRmDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="convert-a-managed-disk-from-standard-hdd-to-standard-ssd-and-vice-versa"></a>Converter um disco gerido standard HDD para SSD standard e vice-versa

O exemplo seguinte mostra como alternar de um único disco de uma VM do padrão HDD para SSD standard e vice-versa.

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between Standard_LRS and StandardSSD_LRS based on your scenario
$storageType = 'StandardSSD_LRS'

$disk = Get-AzureRmDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzureRmResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzureRmVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzureRmVM $vmResource.ResourceGroupName -Name $vmResource.ResourceName 

# Update the storage type
$diskUpdateConfig = New-AzureRmDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzureRmDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzureRmVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="next-steps"></a>Passos Seguintes

Ter uma cópia só de leitura de uma VM ao utilizar [instantâneos](snapshot-copy-managed-disk.md).

