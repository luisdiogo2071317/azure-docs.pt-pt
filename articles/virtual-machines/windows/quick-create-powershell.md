---
title: Guia de Início Rápido – Criar uma VM do Windows com o Azure PowerShell | Microsoft Docs
description: Neste guia de início rápido, irá aprender a utilizar o Azure PowerShell para criar uma máquina virtual do Windows
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 04/24/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: b199d031d4aaef4c1a707199f5692c86543e6360
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38767209"
---
# <a name="quickstart-create-a-windows-virtual-machine-in-azure-with-powershell"></a>Início Rápido: Criar uma máquina virtual do Windows no Azure com o PowerShell

O módulo do Azure PowerShell é utilizado para criar e gerir recursos do Azure a partir da linha de comandos do PowerShell ou em scripts. Este guia de início rápido mostra como utilizar o módulo do Azure PowerShell para implementar uma máquina virtual (VM) no Azure que executa o Windows Server 2016. Para ver a VM em ação, estabeleça o RDP para a VM e instale o servidor Web IIS.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer o módulo do Azure PowerShell versão 5.7.0 ou posterior. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Connect-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="create-resource-group"></a>Criar grupo de recursos

Crie um grupo de recursos do Azure com [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-virtual-machine"></a>Criar a máquina virtual

Crie uma VM com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Forneça nomes para cada um dos recursos e o cmdlet [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) criá-los-á caso ainda não existam.

Quando lhe for pedido, introduza um nome de utilizador e uma palavra-passe a utilizar como credenciais de início de sessão para a VM:

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 80,3389
```

## <a name="connect-to-virtual-machine"></a>Conectar à máquina virtual

Após a conclusão da implementação, estabeleça o RDP para a VM. Para ver a VM em ação, o servidor Web do IIS é instalado.

Para ver o endereço IP público da VM, utilize o cmdlet [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress):

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroup" | Select "IpAddress"
```

Utilize o seguinte comando para criar uma sessão de ambiente de trabalho remoto a partir do computador local. Substitua o endereço IP pelo endereço IP público da VM. Quando lhe for pedido, introduza as credenciais utilizadas quando a VM foi criada:

```powershell
mstsc /v:publicIpAddress
```

## <a name="install-web-server"></a>Instalar o servidor Web

Para ver a VM em ação, instale o servidor Web do IIS. Abra uma janela do PowerShell na VM e execute o comando seguinte:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Quando terminar, feche a ligação RDP à VM.

## <a name="view-the-web-server-in-action"></a>Ver o servidor Web em ação

Com o IIS instalado e a porta 80 aberta na VM a partir da Internet, utilize um browser à sua escolha para ver a página de boas-vindas do IIS predefinida. Utilize o endereço IP público da VM que obteve no passo anterior. O seguinte exemplo mostra o site do IIS predefinido:

![Site predefinido do IIS](./media/quick-create-powershell/default-iis-website.png)

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, pode utilizar o cmdlet [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o grupo de recursos, a VM e todos os recursos relacionados:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Neste guia de início rápido, implementou uma máquina virtual simples, abriu uma porta de rede para o tráfego Web e instalou um servidor Web básico. Para saber mais sobre as máquinas virtuais do Azure, continue com o tutorial para VMs do Windows.

> [!div class="nextstepaction"]
> [Tutoriais de máquinas virtuais do Windows do Azure](./tutorial-manage-vm.md)
