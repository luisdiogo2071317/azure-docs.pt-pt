---
title: Anexar um disco de dados a uma VM do Windows no Azure com o PowerShell | Documentos da Microsoft
description: Como anexar o disco de dados nova ou existente a uma VM do Windows com o PowerShell com o modelo de implementação do Resource Manager.
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
ms.date: 10/11/2017
ms.author: cynthn
ms.openlocfilehash: 384203134d1588053f91b66d32e9b0bf1ec69306
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38680920"
---
# <a name="attach-a-data-disk-to-a-windows-vm-using-powershell"></a>Anexar um disco de dados a uma VM do Windows com o PowerShell

Este artigo mostra-lhe como anexar discos de novos e existentes para uma máquina de virtual do Windows com o PowerShell. 

Antes de fazer isso, consulte estas dicas:
* O tamanho da máquina virtual controla quantos discos de dados, pode anexar. Para obter detalhes, consulte [tamanhos de máquinas virtuais](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Para utilizar o armazenamento Premium, terá um armazenamento Premium ativado o tamanho da VM, como a máquina virtual de série DS ou série GS. Para obter detalhes, consulte [o armazenamento Premium: armazenamento de elevado desempenho para cargas de trabalho de Máquina Virtual de Azure](premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial precisará da versão 6.0.0 ou posterior do módulo do Azure PowerShell. Executar ` Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Connect-AzureRmAccount` para criar uma ligação com o Azure.


## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a>Adicionar um disco de dados vazia para uma máquina virtual

Este exemplo mostra como adicionar um disco de dados vazia para uma máquina virtual existente.

### <a name="using-managed-disks"></a>Utilizar discos geridos

```azurepowershell-interactive
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128
$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

### <a name="using-managed-disks-in-an-availability-zone"></a>Utilizar discos geridos numa zona de disponibilidade
Para criar um disco numa zona de disponibilidade, utilize [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig) com o `-Zone` parâmetro. O exemplo seguinte cria um disco na zona *1*.


```powershell
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US 2' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128 -Zone 1
$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```


### <a name="initialize-the-disk"></a>Inicialize o disco

Depois de adicionar um disco vazio, terá de inicializá-la. Para inicializar o disco, pode iniciar sessão a uma VM e utilizar a gestão de discos. Se ativou o WinRM e um certificado na VM quando o criou, pode utilizar o PowerShell remoto para inicializar o disco. Também pode utilizar uma extensão de script personalizado: 

```azurepowershell-interactive
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```
        
O ficheiro de script pode conter algo semelhante a este código para inicializar os discos:

```azurepowershell-interactive
    $disks = Get-Disk | Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { [char]$_ }
    $count = 0
    $labels = "data1","data2"

    foreach ($disk in $disks) {
        $driveLetter = $letters[$count].ToString()
        $disk | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] -Confirm:$false -Force
    $count++
    }
```


## <a name="attach-an-existing-data-disk-to-a-vm"></a>Anexar um disco de dados existente a uma VM

Pode anexar um disco gerido existente a uma VM como um disco de dados. 

```azurepowershell-interactive
$rgName = "myResourceGroup"
$vmName = "myVM"
$location = "East US" 
$dataDiskName = "myDisk"
$disk = Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $dataDiskName 

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzureRmVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

## <a name="next-steps"></a>Passos Seguintes

Criar uma [instantâneo](snapshot-copy-managed-disk.md).
