---
title: Comandos do PowerShell comuns para máquinas virtuais do Azure | Microsoft Docs
description: Comandos do PowerShell comuns para ajudar a começar a criar e gerir as VMs do Windows no Azure.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ba3839a2-f3d5-4e19-a5de-95bfb1c0e61e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/01/2018
ms.author: cynthn
ms.openlocfilehash: ff861c21250a042191651ab4a4cffbf3928e4f26
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738569"
---
# <a name="common-powershell-commands-for-creating-and-managing-azure-virtual-machines"></a>Comandos do PowerShell comuns para criar e gerir máquinas virtuais do Azure

Este artigo aborda alguns dos comandos do Azure PowerShell que pode utilizar para criar e gerir máquinas virtuais na sua subscrição do Azure.  Para obter mais ajuda com comutadores de linha de comandos específicos e as opções, pode utilizar o **Get-Help** *comando*.

Veja [How to install and configure Azure PowerShell (Como instalar e configurar o Azure PowerShell)](/powershell/azure/overview) para obter informações sobre como instalar a versão mais recente do Azure PowerShell, selecionar a subscrição que quer utilizar e iniciar sessão na sua conta do Azure.

Estas variáveis poderão ser útil se executar mais do que um dos comandos neste artigo:

- $location - a localização da máquina virtual. Pode utilizar [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) para localizar um [região geográfica](https://azure.microsoft.com/regions/) que funciona para si.
- $myResourceGroup - o nome do grupo de recursos que contém a máquina virtual.
- $myVM - o nome da máquina virtual.

## <a name="create-a-vm---simplified"></a>Criar uma VM - simplificada

| Tarefa | Comando |
| ---- | ------- |
| Criar uma VM simple | [Novo-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) -nome $myVM <BR></BR><BR></BR> Novo-AzureRMVM tem um conjunto de *simplificada* parâmetros, onde é tudo o que é necessário um nome único. O valor de - nome será utilizado como o nome para todos os recursos necessários para criar uma nova VM. Pode especificar mais, mas isto é tudo o que é necessário.|
| Criar uma VM a partir de uma imagem personalizada | Novo-AzureRmVm - ResourceGroupName $myResourceGroup-nome $myVM ImageName "myImage"-localização $location  <BR></BR><BR></BR>Tem de já criou os seus próprios [imagem gerida](capture-image-resource.md). Pode utilizar uma imagem para tornar vários, VMs idênticas. |



## <a name="create-a-vm-configuration"></a>Criar uma configuração de VM

| Tarefa | Comando |
| ---- | ------- |
| Criar uma configuração de VM |$vm = [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig) - VMName $myVM - VMSize "Standard_D1_v1"<BR></BR><BR></BR>A configuração de VM é utilizada para definir ou atualizar as definições para a VM. A configuração é inicializada com o nome da VM e a respetiva [tamanho](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Adicionar as definições de configuração |$vm = [conjunto AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) - VM $vm-Windows - ComputerName $myVM-Credential $cred - ProvisionVMAgent - EnableAutoUpdate<BR></BR><BR></BR>Definições do sistema operativo, incluindo [credenciais](https://technet.microsoft.com/library/hh849815.aspx) são adicionadas para o objeto de configuração que criou anteriormente com AzureRmVMConfig de novo. |
| Adicione uma interface de rede |$vm = [Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/Add-AzureRmVMNetworkInterface) -VM $vm -Id $nic.Id<BR></BR><BR></BR>Uma VM tem de ter um [interface de rede](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) comunicar numa rede virtual. Também pode utilizar [Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) para obter um objeto de interface de rede existente. |
| Especifique uma imagem de plataforma |$vm = [conjunto AzureRmVMSourceImage](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage) - VM $vm - PublisherName "publisher_name"-oferecem "publisher_offer" - Skus "product_sku"-versão "mais recente"<BR></BR><BR></BR>[Informações de imagem](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) é adicionada ao objeto de configuração que criou anteriormente com AzureRmVMConfig de novo. O objeto devolvido deste comando só é utilizado quando definir o disco do SO a utilizar uma imagem de plataforma. |
| Criar uma VM |[New-AzureRmVM]() -ResourceGroupName $myResourceGroup -Location $location -VM $vm<BR></BR><BR></BR>Todos os recursos são criados num [grupo de recursos](../../azure-resource-manager/powershell-azure-resource-manager.md). Antes de executar este comando, execute New-AzureRmVMConfig, AzureRmVMOperatingSystem conjunto, AzureRmVMSourceImage conjunto, AzureRmVMNetworkInterface adicionar e AzureRmVMOSDisk de conjunto. |
| Atualizar uma VM |[Update-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvm) -ResourceGroupName $myResourceGroup -VM $vm<BR></BR><BR></BR>Obter a configuração atual do VM utilizando o Get-AzureRmVM, alterar definições de configuração do objeto de VM e, em seguida, execute este comando. |

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


## <a name="next-steps"></a>Passos Seguintes
* Consulte os passos básicos para criar uma máquina virtual no [criar uma VM do Windows utilizando o Gestor de recursos e o PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

