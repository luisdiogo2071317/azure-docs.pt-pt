---
title: Comandos do PowerShell comuns para máquinas virtuais do Azure | Microsoft Docs
description: Comandos do PowerShell comuns para ajudar a começar a criar e gerir as VMs do Windows no Azure.
services: virtual-machines-windows
documentationcenter: ''
author: davidmu1
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ba3839a2-f3d5-4e19-a5de-95bfb1c0e61e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: f84473e73a32da43cc6cc80b21deb49ab4f3ceb9
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="common-powershell-commands-for-creating-and-managing-azure-virtual-machines"></a>Comandos do PowerShell comuns para criar e gerir máquinas virtuais do Azure

Este artigo aborda alguns dos comandos do Azure PowerShell que pode utilizar para criar e gerir máquinas virtuais na sua subscrição do Azure.  Para obter mais ajuda com comutadores de linha de comandos específicos e as opções, pode utilizar **Get-Help** *comando*.

Veja [How to install and configure Azure PowerShell (Como instalar e configurar o Azure PowerShell)](/powershell/azure/overview) para obter informações sobre como instalar a versão mais recente do Azure PowerShell, selecionar a subscrição que quer utilizar e iniciar sessão na sua conta do Azure.

Estas variáveis poderão ser útil se executar mais do que um dos comandos neste artigo:

- $location - a localização da máquina virtual. Pode utilizar [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) para localizar um [região geográfica](https://azure.microsoft.com/regions/) que funciona para si.
- $myResourceGroup - o nome do grupo de recursos que contém a máquina virtual.
- $myVM - o nome da máquina virtual.

## <a name="create-a-vm"></a>Criar uma VM

| Tarefa | Comando |
| ---- | ------- |
| Criar uma configuração de VM |$vm = [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig) -VMName $myVM -VMSize "Standard_D1_v1"<BR></BR><BR></BR>A configuração de VM é utilizada para definir ou atualizar as definições para a VM. A configuração é inicializada com o nome da VM e a respetiva [tamanho](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Adicionar as definições de configuração |$vm = [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) -VM $vm -Windows -ComputerName $myVM -Credential $cred -ProvisionVMAgent -EnableAutoUpdate<BR></BR><BR></BR>Definições do sistema operativo, incluindo [credenciais](https://technet.microsoft.com/library/hh849815.aspx) são adicionadas para o objeto de configuração que criou anteriormente com AzureRmVMConfig de novo. |
| Adicione uma interface de rede |$vm = [Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/Add-AzureRmVMNetworkInterface) -VM $vm -Id $nic.Id<BR></BR><BR></BR>Uma VM tem de ter um [interface de rede](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) comunicar numa rede virtual. Também pode utilizar [Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) para obter um objeto de interface de rede existente. |
| Especifique uma imagem de plataforma |$vm = [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage) -VM $vm -PublisherName "publisher_name" -Offer "publisher_offer" -Skus "product_sku" -Version "latest"<BR></BR><BR></BR>[Informações de imagem](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) é adicionada ao objeto de configuração que criou anteriormente com AzureRmVMConfig de novo. O objeto devolvido deste comando só é utilizado quando definir o disco do SO a utilizar uma imagem de plataforma. |
| Definir o disco do SO a utilizar uma imagem de plataforma |$vm = [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmosdisk) -VM $vm -Name "myOSDisk" -VhdUri "http://mystore1.blob.core.windows.net/vhds/myOSDisk.vhd" -CreateOption FromImage<BR></BR><BR></BR>O nome do disco do sistema operativo e a respetiva localização no [armazenamento](../../storage/common/storage-powershell-guide-full.md) é adicionada ao objeto de configuração que criou anteriormente. |
| Definir o disco do SO a utilizar uma imagem generalizada |$vm = Set-AzureRmVMOSDisk -VM $vm -Name "myOSDisk" -SourceImageUri "https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk.{guid}.vhd" -VhdUri "https://mystore1.blob.core.windows.net/vhds/disk_name.vhd" -CreateOption FromImage -Windows<BR></BR><BR></BR>O nome do disco do sistema operativo, a localização da imagem de origem e localização do disco no [armazenamento](../../storage/common/storage-powershell-guide-full.md) é adicionada ao objeto de configuração. |
| Definir o disco do SO a utilizar uma imagem especializada |$vm = Set-AzureRmVMOSDisk -VM $vm -Name "myOSDisk" -VhdUri "http://mystore1.blob.core.windows.net/vhds/" -CreateOption Attach -Windows |
| Criar uma VM |[New-AzureRmVM]() -ResourceGroupName $myResourceGroup -Location $location -VM $vm<BR></BR><BR></BR>Todos os recursos são criados num [grupo de recursos](../../azure-resource-manager/powershell-azure-resource-manager.md). Antes de executar este comando, execute New-AzureRmVMConfig, AzureRmVMOperatingSystem conjunto, AzureRmVMSourceImage conjunto, AzureRmVMNetworkInterface adicionar e AzureRmVMOSDisk de conjunto. |

## <a name="get-information-about-vms"></a>Obter informações sobre as VMs

| Tarefa | Comando |
| ---- | ------- |
| Lista de VMs de uma subscrição |[Get-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvm) |
| Lista de VMs num grupo de recursos |Get-AzureRmVM -ResourceGroupName $myResourceGroup<BR></BR><BR></BR>Para obter uma lista de grupos de recursos na sua subscrição, utilize [Get-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermresourcegroup). |
| Obter informações sobre uma VM |Get-AzureRmVM -ResourceGroupName $myResourceGroup -Name $myVM |

## <a name="manage-vms"></a>Gerir VMs
| Tarefa | Comando |
| --- | --- |
| Iniciar uma VM |[Start-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/start-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Parar uma VM |[Stop-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/stop-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Reiniciar uma VM em execução |[Restart-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/restart-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Eliminar uma VM |[Remove-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Generalize uma VM |[Set-AzureRmVm](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM -Generalized<BR></BR><BR></BR>Execute este comando antes de executar AzureRmVMImage guardar. |
| Capturar uma VM |[Save-AzureRmVMImage](https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvmimage) -ResourceGroupName $myResourceGroup -VMName $myVM -DestinationContainerName "myImageContainer" -VHDNamePrefix "myImagePrefix" -Path "C:\filepath\filename.json"<BR></BR><BR></BR>Uma máquina virtual tem de ser [preparado, encerre e generalizado](prepare-for-upload-vhd-image.md) para ser utilizado para criar uma imagem. Antes de executar este comando, execute Set-AzureRmVm. |
| Atualizar uma VM |[Update-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvm) -ResourceGroupName $myResourceGroup -VM $vm<BR></BR><BR></BR>Obter a configuração atual do VM utilizando o Get-AzureRmVM, alterar definições de configuração do objeto de VM e, em seguida, execute este comando. |
| Adicionar um disco de dados a uma VM |[AzureRmVMDataDisk adicionar](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmdatadisk) - VM $vm-Name "myDataDisk" - VhdUri "https://mystore1.blob.core.windows.net/vhds/myDataDisk.vhd" - LUN #-colocação em cache ReadWrite - DiskSizeinGB # - CreateOption vazio<BR></BR><BR></BR>Utilize Get-AzureRmVM para obter o objeto da VM. Especifique o número LUN e o tamanho do disco. Execute atualização-AzureRmVM para aplicar as alterações de configuração para a VM. O disco que adiciona não está inicializado. |
| Remover um disco de dados de uma VM |[Remove-AzureRmVMDataDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvmdatadisk) -VM $vm -Name "myDataDisk"<BR></BR><BR></BR>Utilize Get-AzureRmVM para obter o objeto da VM. Execute atualização-AzureRmVM para aplicar as alterações de configuração para a VM. |
| Adicionar uma extensão para uma VM |[Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) -ResourceGroupName $myResourceGroup -Location $location -VMName $myVM -Name "extensionName" -Publisher "publisherName" -Type "extensionType" -TypeHandlerVersion "#.#" -Settings $Settings -ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Execute este comando com apropriados [informações de configuração](template-description.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#extensions) para a extensão que pretende instalar. |
| Remover uma extensão de VM |[Remove-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvmextension) -ResourceGroupName $myResourceGroup -Name "extensionName" -VMName $myVM |

## <a name="next-steps"></a>Passos Seguintes
* Consulte os passos básicos para criar uma máquina virtual no [criar uma VM do Windows utilizando o Gestor de recursos e o PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

