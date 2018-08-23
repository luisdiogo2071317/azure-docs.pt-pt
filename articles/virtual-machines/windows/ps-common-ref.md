---
title: Comandos comuns do PowerShell para máquinas de virtuais do Azure | Documentos da Microsoft
description: Comandos comuns do PowerShell para começar a criar e gerir as suas VMs do Windows no Azure.
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
ms.openlocfilehash: cb9f03ab87079ba33135840e3e7599d2e8cc95e9
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/11/2018
ms.locfileid: "42060216"
---
# <a name="common-powershell-commands-for-creating-and-managing-azure-virtual-machines"></a>Comandos comuns do PowerShell para criar e gerir máquinas virtuais do Azure

Este artigo aborda alguns dos comandos do Azure PowerShell que pode utilizar para criar e gerir máquinas virtuais na sua subscrição do Azure.  Para obter mais ajuda com parâmetros de linha de comandos específicos e opções, pode utilizar o **Get-Help** *comando*.

Veja [How to install and configure Azure PowerShell (Como instalar e configurar o Azure PowerShell)](/powershell/azure/overview) para obter informações sobre como instalar a versão mais recente do Azure PowerShell, selecionar a subscrição que quer utilizar e iniciar sessão na sua conta do Azure.

Estas variáveis podem ser útil para, se executar mais de um dos comandos neste artigo:

- $location - a localização da máquina virtual. Pode usar [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) para localizar um [região geográfica](https://azure.microsoft.com/regions/) que funciona melhor para si.
- $myResourceGroup - o nome do grupo de recursos que contém a máquina virtual.
- $myVM - o nome da máquina virtual.

## <a name="create-a-vm---simplified"></a>Criar uma VM - simplificada

| Tarefa | Comando |
| ---- | ------- |
| Criar uma VM simple | [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) -nome $myVM <BR></BR><BR></BR> New-AzureRMVM tem um conjunto de *simplificada* parâmetros, onde é tudo o que é necessário um nome único. O valor-nome será utilizado como o nome de todos os recursos necessários para criar uma nova VM. É possível especificar mais, mas isso é tudo o que é necessário.|
| Criar uma VM a partir de uma imagem personalizada | New-AzureRmVm - ResourceGroupName $myResourceGroup-nome $myVM ImageName "myImage"-localização $location  <BR></BR><BR></BR>Tem de já ter criado seu próprio [imagem gerida](capture-image-resource.md). Pode utilizar uma imagem para tornar vários, VMs idênticas. |



## <a name="create-a-vm-configuration"></a>Criar uma configuração de VM

| Tarefa | Comando |
| ---- | ------- |
| Criar uma configuração de VM |$vm = [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig) - VMName $myVM - VMSize "Standard_D1_v1"<BR></BR><BR></BR>A configuração da VM é usada para definir ou atualizar as definições para a VM. A configuração é inicializada com o nome da VM e a respetiva [tamanho](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Adicionar definições de configuração |$vm = [set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) - VM $vm-Windows - ComputerName $myVM-$cred - ProvisionVMAgent de credenciais - EnableAutoUpdate<BR></BR><BR></BR>Definições de sistema operativo, incluindo [credenciais](https://technet.microsoft.com/library/hh849815.aspx) são adicionadas para o objeto de configuração que criou anteriormente com New-AzureRmVMConfig. |
| Adicionar uma interface de rede |$vm = [Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/Add-AzureRmVMNetworkInterface) -VM $vm -Id $nic.Id<BR></BR><BR></BR>Uma VM tem de ter uma [interface de rede](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) para comunicar-se numa rede virtual. Também pode utilizar [Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.compute/add-azurermvmnetworkinterface) para recuperar um objeto de interface de rede existente. |
| Especifique uma imagem de plataforma |$vm = [set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmsourceimage) - VM $vm - PublisherName "publisher_name"-"publisher_offer" da oferta - Skus "product_sku"-versão "mais recente"<BR></BR><BR></BR>[Informações de imagem](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) é adicionado ao objeto de configuração que criou anteriormente com New-AzureRmVMConfig. O objeto devolvido deste comando só é utilizado ao definir o disco do SO a utilizar uma imagem de plataforma. |
| Criar uma VM |[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) -ResourceGroupName $myResourceGroup -Location $location -VM $vm<BR></BR><BR></BR>Todos os recursos são criados num [grupo de recursos](../../azure-resource-manager/powershell-azure-resource-manager.md). Antes de executar este comando, execute New-AzureRmVMConfig, conjunto AzureRmVMOperatingSystem, Set-AzureRmVMSourceImage, Add-AzureRmVMNetworkInterface e Set-AzureRmVMOSDisk. |
| Atualizar uma VM |[Update-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvm) -ResourceGroupName $myResourceGroup -VM $vm<BR></BR><BR></BR>Obter a configuração atual do VM usando Get-AzureRmVM, alterar definições de configuração do objeto de VM e, em seguida, execute este comando. |

## <a name="get-information-about-vms"></a>Obter informações sobre VMs

| Tarefa | Comando |
| ---- | ------- |
| Lista de VMs numa subscrição |[Get-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvm) |
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
* Veja os passos básicos para a criação de uma máquina virtual no [criar uma VM do Windows com o Resource Manager e PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

