---
title: Criar a VM do Windows utilizando simplificada New-AzureRMVM cmdlet na Shell de nuvem do Azure | Microsoft Docs
description: "Saiba mais rapidamente criar máquinas virtuais do Windows com o cmdlet New-AzureRMVM simplificado na Shell de nuvem do Azure."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: 94eb6232cf59d502a9d70545785c3788398f4d27
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/15/2017
---
# <a name="create-a-windows-virtual-machine-with-the-simplified-new-azurermvm-cmdlet-in-cloud-shell"></a>Criar uma máquina virtual do Windows com o cmdlet New-AzureRMVM simplificado na Shell de nuvem 

O [New-AzureRMVM](/powershell/module/azurerm.resources/new-azurermvm) cmdlet foi adicionado um conjunto de parâmetros para criar uma nova VM com o PowerShell simplificado. Este tópico mostra como utilizar o PowerShell na Shell de nuvem do Azure, com a versão mais recente do cmdlet New-AzureVM pré-instalado, para criar uma nova VM. Utilizaremos um conjunto de parâmetros simplificada que cria automaticamente todos os recursos necessários com predefinições inteligentes. 

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.


[!INCLUDE [cloud-shell-powershell](../../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer o Azure PowerShell versão do módulo 5.1.1 ou posterior. Executar ` Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="create-the-vm"></a>Crie a VM

Pode utilizar o [New-AzureRMVM](/powershell/module/azurerm.resources/new-azurermvm) cmdlet para criar uma VM com predefinições inteligentes, que incluem a utilização da imagem do Windows Server 2016 Datacenter do Azure Marketplace. Pode utilizar o novo-AzureRMVM com apenas o **-nome** parâmetro e irão utilizar esse valor para todos os nomes de recursos. Neste exemplo, vamos definir o parâmetro **-Name** como *myVM*. 

Certifique-se de que o **PowerShell** está selecionado no Cloud Shell e escreva:

```azurepowershell-interactive
New-AzureRMVm -Name myVM
```

É-lhe pedido para criar um nome de utilizador e uma palavra-passe para a VM, que serão utilizados quando ligar à VM mais adiante neste tópico. A palavra-passe tem de ter entre 12 e 123 carateres e cumprir três dos quatro requisitos de complexidade que se seguem: um caráter em letra minúscula, um caráter em letra maiúscula, um número e um caráter especial.

Basta um minuto para criar a VM e os recursos associados. Quando tudo estiver terminado, poderá ver todos os recursos criados com o cmdlet [Find-AzureRmResource](/powershell/module/azurerm.resources/find-azurermresource).

```azurepowershell-interactive
Find-AzureRmResource `
    -ResourceGroupNameEquals myVMResourceGroup | Format-Table Name
```

## <a name="connect-to-the-vm"></a>Ligar à VM

Depois de a implementação estar concluída, crie uma ligação de ambiente de trabalho remoto à máquina virtual.

Utilize o comando [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) para devolver o endereço IP público da máquina virtual. Tome nota deste Endereço IP.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
    -ResourceGroupName myVMResourceGroup | Select IpAddress
```

No seu computador local, abra uma linha de comandos e utilize o **mstsc** comando para iniciar uma sessão de ambiente de trabalho remoto com a nova VM. Substitua &lt;publicIPAddress&gt; pelo endereço IP da máquina virtual. Quando lhe for pedido, introduza o nome de utilizador e a palavra-passe que atribuiu à VM quando esta foi criada.

```
mstsc /v:<publicIpAddress>
```
## <a name="specify-different-resource-names"></a>Especifique nomes de recursos diferente

Pode também fornecer mais descritivos nomes para os recursos e ainda-los criado automaticamente. Eis um exemplo em que podemos ter com o nome vários recursos para a nova VM, incluindo um novo grupo de recursos.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 3389
```

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, pode utilizar o comando [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o Grupo de Recursos, a VM e todos os recursos relacionados.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myVM
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Neste tópico, implementou uma máquina virtual simples com o cmdlet New-AzVM e, em seguida, ligou-a via RDP. Para saber mais sobre as máquinas virtuais do Azure, continue com o tutorial para VMs do Windows.

> [!div class="nextstepaction"]
> [Tutoriais de máquinas virtuais do Windows do Azure](./tutorial-manage-vm.md)
