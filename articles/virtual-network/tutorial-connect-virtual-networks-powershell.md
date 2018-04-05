---
title: Ligar redes virtuais com o peering de rede virtual - PowerShell | Microsoft Docs
description: Neste artigo, irá aprender a ligar redes virtuais com a rede virtual peering, com o Azure PowerShell.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to connect two virtual networks so that virtual machines in one virtual network can communicate with virtual machines in the other virtual network.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 445baa36f33cbe02b68bdb37406f842932089183
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-powershell"></a>Ligar redes virtuais com o peering de rede virtual com o PowerShell

Pode ligar redes virtuais entre si ao peering de rede virtual. Depois de redes virtuais em modo de peering, recursos em ambas as redes virtuais conseguem comunicar entre si, com a mesma latência e largura de banda, como se os recursos foram na mesma rede virtual. Neste artigo, saiba como:

> [!div class="checklist"]
> * Criar duas redes virtuais
> * Ligar duas redes virtuais com um peering de rede virtual
> * Implementar uma máquina virtual (VM) em cada rede virtual
> * Comunicar entre VMs

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este artigo requer o Azure PowerShell versão do módulo 5.4.1 ou posterior. Execute ` Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure. 

## <a name="create-virtual-networks"></a>Criar redes virtuais

Antes de criar uma rede virtual, tem de criar um grupo de recursos para a rede virtual e todos os outros recursos criados neste artigo. Criar um grupo de recursos com [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Crie uma rede virtual com [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). O exemplo seguinte cria uma rede virtual denominada *myVirtualNetwork1* com o prefixo de endereço *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork1 = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork1 `
  -AddressPrefix 10.0.0.0/16
```

Criar uma configuração de sub-rede com [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). O exemplo seguinte cria uma configuração de sub-rede com um prefixo de endereço 10.0.0.0/24:

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork1
```

A configuração de sub-rede de escrita para a rede virtual com [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), que cria a sub-rede:

```azurepowershell-interactive
$virtualNetwork1 | Set-AzureRmVirtualNetwork
```

Crie uma rede virtual com um prefixo de endereço 10.1.0.0/16 e uma sub-rede:

```azurepowershell-interactive
# Create the virtual network.
$virtualNetwork2 = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork2 `
  -AddressPrefix 10.1.0.0/16

# Create the subnet configuration.
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.1.0.0/24 `
  -VirtualNetwork $virtualNetwork2

# Write the subnet configuration to the virtual network.
$virtualNetwork2 | Set-AzureRmVirtualNetwork
```

## <a name="peer-virtual-networks"></a>Redes virtuais do elemento de rede

Criar um peering com [Add-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering). Os elementos de rede de exemplo seguintes *myVirtualNetwork1* para *myVirtualNetwork2*.

```azurepowershell-interactive
Add-AzureRmVirtualNetworkPeering `
  -Name myVirtualNetwork1-myVirtualNetwork2 `
  -VirtualNetwork $virtualNetwork1 `
  -RemoteVirtualNetworkId $virtualNetwork2.Id
```

No resultado devolvido após executa o comando anterior, verá que o **PeeringState** é *iniciado*. Os peering permanecem no *iniciado* estado até que crie o peering de *myVirtualNetwork2* para *myVirtualNetwork1*. Criar um peering do *myVirtualNetwork2* para *myVirtualNetwork1*. 

```azurepowershell-interactive
Add-AzureRmVirtualNetworkPeering `
  -Name myVirtualNetwork2-myVirtualNetwork1 `
  -VirtualNetwork $virtualNetwork2 `
  -RemoteVirtualNetworkId $virtualNetwork1.Id
```

No resultado devolvido após executa o comando anterior, verá que o **PeeringState** é *ligado*. Azure também alterar o estado do peering do *myVirtualNetwork1 myVirtualNetwork2* peering para *ligado*. Confirme que o estado do peering para o *myVirtualNetwork1 myVirtualNetwork2* peering alterado para *ligado* com [Get-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering).

```azurepowershell-interactive
Get-AzureRmVirtualNetworkPeering `
  -ResourceGroupName myResourceGroup `
  -VirtualNetworkName myVirtualNetwork1 `
  | Select PeeringState
```

Recursos de uma rede virtual não é possível comunicar com os recursos na outra rede virtual até que o **PeeringState** para peerings em ambas as redes virtuais é *ligado*. 

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Crie uma VM em cada rede virtual para que possa comunicar entre eles num passo posterior.

### <a name="create-the-first-vm"></a>Criar a primeira VM

Criar uma VM com [novo-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). O exemplo seguinte cria uma VM chamada *myVm1* no *myVirtualNetwork1* rede virtual. O `-AsJob` opção cria a VM em segundo plano, para poder continuar para o passo seguinte. Quando lhe for pedido, introduza o nome de utilizador e palavra-passe que pretende iniciar sessão na VM com o.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork1" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm1" `
  -AsJob
```

### <a name="create-the-second-vm"></a>Criar a VM segundo

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork2" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm2"
```

A VM demora alguns minutos a criar. Não continue passos posteriores até que o Azure cria a VM e devolve um resultado para o PowerShell.

## <a name="communicate-between-vms"></a>Comunicar entre VMs

Pode ligar ao endereço IP público de uma VM a partir da internet. Utilize [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) para devolver o endereço IP público de uma VM. O exemplo seguinte devolve o endereço IP público do *myVm1* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

Utilize o seguinte comando para criar uma sessão de ambiente de trabalho remota com o *myVm1* VM a partir do seu computador local. Substitua `<publicIpAddress>` com o endereço IP devolvido do comando anterior.

```
mstsc /v:<publicIpAddress>
```

Um ficheiro de protocolo de ambiente de trabalho remoto (RDP) é criado, transferido para o computador e aberto. Introduza o nome de utilizador e palavra-passe (poderá ter de selecionar **mais opções**, em seguida, **utilizar uma conta diferente**, para especificar as credenciais que introduziu quando criou a VM) e, em seguida, clique em **OK** . Poderá receber um aviso de certificado durante o processo de início de sessão. Clique em **Sim** ou **Continuar** para continuar com a ligação.

No *myVm1* VM, ativar o controlo de mensagem de protocolo ICMP (Internet) através do Windows firewall para consegue enviar pings para esta VM *myVm2* num passo posterior, com o PowerShell:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Apesar de ping é utilizado para comunicar entre VMs neste artigo, permitir ICMP através da Firewall do Windows para implementações de produção não é recomendada.

Para estabelecer ligação com o *myVm2* VM, introduza o seguinte comando numa linha de comandos *myVm1* VM:

```
mstsc /v:10.1.0.4
```

Uma vez que ativou o ping no *myVm1*, pode agora executar um ping-lo por endereço IP numa linha de comandos no *myVm2* VM:

```
ping 10.0.0.4
```

Receber quatro respostas. Desligar as sessões RDP para ambos *myVm1* e *myVm2*.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário utilizar [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o grupo de recursos e todos os recursos nele contidos.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Passos seguintes

Neste artigo, aprendeu a ligar duas redes na mesma região do Azure, com o peering de rede virtual. Também pode elemento redes virtuais em diferentes [regiões suportadas](virtual-network-manage-peering.md#cross-region) e na [diferentes subscrições do Azure](create-peering-different-subscriptions.md#powershell), bem como criar [hub- and -spoke designs de rede](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) com peering. Para obter mais informações sobre peering de rede virtual, consulte o artigo [descrição geral de peering da rede Virtual](virtual-network-peering-overview.md) e [gerir peerings de rede virtual](virtual-network-manage-peering.md).

Pode [ligar o seu próprio computador a uma rede virtual](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) através de uma VPN e interaja com os recursos numa rede virtual, ou em redes virtuais em modo de peering. Para obter scripts reutilizáveis concluir a muitas das tarefas abrangidas os artigos de rede virtual, consulte [script amostras](powershell-samples.md).
