---
title: Criar VM a partir de uma imagem gerida no Azure | Microsoft Docs
description: Crie uma máquina virtual do Windows a partir de uma imagem gerida generalizada com o Azure PowerShell ou o portal do Azure, o modelo de implementação Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: cynthn
ms.openlocfilehash: 1d543bd9590664e74cff70cf55e8f7bd42f2c6f0
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-vm-from-a-managed-image"></a>Criar uma VM a partir de uma imagem gerida

Pode criar várias VMs a partir de uma imagem VM gerida utilizando o PowerShell ou o portal do Azure. Uma imagem VM gerida contém as informações necessárias para criar uma VM, incluindo os SO e discos de dados. Os VHDs que compõem a imagem, incluindo os discos de SO e discos de dados, são armazenadas como discos geridos. 

Tem de ter já [criada uma imagem VM gerida](capture-image-resource.md) a utilizar para criar a nova VM. 

## <a name="use-the-portal"></a>Utilizar o portal

1. Abra o [Portal do Azure](https://portal.azure.com).
2. No menu do lado esquerdo, selecione **Todos os recursos**. Pode ordenar os recursos por **tipo** encontrar facilmente as imagens.
3. Selecione a imagem que pretende utilizar na lista. A imagem **descrição geral** é aberta a página.
4. Clique em **+ criar VM** no menu.
5. Introduza as informações da máquina virtual. O nome de utilizador e palavra-passe introduzidos aqui são utilizados para iniciar sessão na máquina virtual. Quando terminar, clique em **OK**. Pode criar a nova VM num grupo Resrouce existente ou escolha **criar nova** para criar um novo grupo de recursos para armazenar a VM.
6. Selecione um tamanho para a VM. Para ver mais tamanhos, selecione **Visualizar todos** ou altere o filtro **Tipo de disco suportado**. 
7. Em **definições**, efetue as alterações necessárias e clique em **OK**. 
8. Na página Resumo, deverá ver o nome de imagem listado para **imagem privada**. Clique em **Ok** para iniciar a implementação da máquina virtual.


## <a name="use-powershell"></a>Utilizar o PowerShell

Pode utilizar o PowerShell para criar uma VM a partir de uma imagem utilizando o parâmetro simplificado definido para o [New-AzureRmVm](/powershell/module/azurerm.compute/new-azurermvm) cmdlet. A imagem tem de estar no mesmo grupo de recursos onde pretende criar a VM.

Este exemplo requer a versão do módulo 5.6.0 de AzureRM ou posterior. Executar ` Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps).

O parâmetro simplificado definido para o novo-AzureRmVm apenas requer que forneça um nome, o nome de grupo e a imagem de recurso para criar uma VM a partir de uma imagem, mas irá utilizar o valor da **-nome** parâmetro como o nome de todos os recursos que as TI cria automaticamente. Neste exemplo, iremos fornecer nomes mais detalhados para cada recurso, mas permitir que o cmdlet criá-los automaticamente. Também pode criar recursos, como a rede virtual, antecedência e transmitir o nome para o cmdlet. Irá utilizar os recursos existentes se pode encontrá-los pelo respetivo nome.

O exemplo seguinte cria uma VM chamada *myVMFromImage*, além de *myResourceGroup* grupo de recursos, a imagem com o nome do *myImage*. 


```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVMfromImage" `
    -ImageName "myImage" `
    -Location "East US" `
    -VirtualNetworkName "myImageVnet" `
    -SubnetName "myImageSubnet" `
    -SecurityGroupName "myImageNSG" `
    -PublicIpAddressName "myImagePIP" `
    -OpenPorts 3389
```



## <a name="next-steps"></a>Passos Seguintes
Para gerir a sua nova máquina virtual com o Azure PowerShell, consulte [criar e gerir VMs do Windows com o módulo Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

