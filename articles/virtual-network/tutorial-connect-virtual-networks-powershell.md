---
title: Ligar redes virtuais com o peering de rede virtual - PowerShell | Microsoft Docs
description: Saiba como ligar redes virtuais com o peering de rede virtual.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: 
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/06/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: c7b3fa2b566ab02e7fb4a03055db83f1545895e8
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-powershell"></a>Ligar redes virtuais com o peering de rede virtual com o PowerShell

Pode ligar redes virtuais entre si ao peering de rede virtual. Depois de redes virtuais em modo de peering, recursos em ambas as redes virtuais conseguem comunicar entre si, com a mesma latência e largura de banda, como se os recursos foram na mesma rede virtual. Este artigo abrange a criação e o peering de duas redes virtuais. Saiba como:

> [!div class="checklist"]
> * Criar duas redes virtuais
> * Criar um peering entre redes virtuais
> * Peering de teste

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este artigo requer o Azure PowerShell versão do módulo 3,6 ou posterior. Executar ` Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure. 

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

O prefixo de endereço para o *myVirtualNetwork2* rede virtual não se sobreponha a com o prefixo de endereço do *myVirtualNetwork1* rede virtual. Não é possível elemento redes virtuais com sobreposição de prefixos de endereço.

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

Peerings estão entre duas redes virtuais, mas não estão transitivas. Sim, por exemplo, se pretendesse também a ponto *myVirtualNetwork2* para *myVirtualNetwork3*, terá de criar um peering entre redes virtuais adicionais *myVirtualNetwork2* e *myVirtualNetwork3*. Apesar de *myVirtualNetwork1* está em modo de peering com *myVirtualNetwork2*, recursos *myVirtualNetwork1* só foi possível aceder a recursos em  *myVirtualNetwork3* se *myVirtualNetwork1* foi também em modo de peering com *myVirtualNetwork3*. 

Antes de peering redes virtuais de produção, recomenda-se que exaustivamente familiarizar-se com o [descrição geral do peering](virtual-network-peering-overview.md), [gerir peering](virtual-network-manage-peering.md), e [limites de rede virtual ](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Embora este artigo ilustra um peering entre duas redes virtuais na mesma subscrição e localização, também pode elemento redes virtuais no [regiões diferentes](#register) e [diferentes subscrições do Azure](create-peering-different-subscriptions.md#powershell). Também pode criar [hub- and -spoke designs de rede](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) com peering.

## <a name="test-peering"></a>Peering de teste

Para testar a comunicação de rede entre máquinas virtuais nas redes virtuais diferentes através de um peering, implementar uma máquina virtual para cada sub-rede e, em seguida, comunicar entre as máquinas virtuais. 

### <a name="create-virtual-machines"></a>Criar máquinas virtuais

Crie uma máquina virtual em cada rede virtual, de modo pode validar a comunicação entre elas num passo posterior.

Criar uma máquina virtual com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). O exemplo seguinte cria uma máquina virtual denominada *myVm1* no *myVirtualNetwork1* rede virtual. O `-AsJob` opção cria a máquina virtual em segundo plano, para poder continuar para o passo seguinte. Quando lhe for pedido, introduza o nome de utilizador e palavra-passe que pretende iniciar sessão na máquina virtual com o.

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

Azure atribui automaticamente 10.0.0.4 como o endereço IP privado da máquina virtual, porque 10.0.0.4 é o primeiro endereço IP disponível no *Subnet1* de *myVirtualNetwork1*. 

Criar uma máquina virtual no *myVirtualNetwork2* rede virtual.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork2" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm2"
```

A máquina virtual demora alguns minutos a criar. Apesar de não na saída devolvida, Azure atribuído 10.1.0.4 como o endereço IP privado da máquina virtual, porque 10.1.0.4 é o primeiro endereço IP disponível no *Subnet1* de *myVirtualNetwork2*. 

Não continue passos posteriores até que o Azure cria a máquina virtual e devolve um resultado para o PowerShell.

### <a name="test-virtual-machine-communication"></a>Comunicação de máquina virtual de teste

Pode ligar ao endereço IP público de uma máquina virtual a partir da Internet. Utilize [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) para devolver o endereço IP público de uma máquina virtual. O exemplo seguinte devolve o endereço IP público do *myVm1* máquina virtual:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

Utilize o seguinte comando para criar uma sessão de ambiente de trabalho remota com o *myVm1* máquina virtual do seu computador local. Substitua `<publicIpAddress>` com o endereço IP devolvido do comando anterior.

```
mstsc /v:<publicIpAddress>
```

Um ficheiro de protocolo de ambiente de trabalho remoto (RDP) é criado, transferido para o computador e aberto. Introduza o nome de utilizador e palavra-passe (poderá ter de selecionar **mais opções**, em seguida, **utilizar uma conta diferente**, para especificar as credenciais que introduziu quando criou a máquina virtual) e, em seguida, clique em  **OK**. Poderá receber um aviso de certificado durante o processo de início de sessão. Clique em **Sim** ou **Continuar** para continuar com a ligação.

Numa linha de comandos, ativar ping através da firewall do Windows, pelo que pode enviar ping a esta máquina virtual de *myVm2* num passo posterior.

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

Apesar de ping é utilizado para fins de teste neste artigo, permitir ICMP através da Firewall do Windows para implementações de produção não é recomendada.

Para estabelecer ligação com o *myVm2* máquina virtual, introduza o seguinte comando numa linha de comandos *myVm1* máquina virtual:

```
mstsc /v:10.1.0.4
```

Uma vez que ativou o ping no *myVm1*, pode agora executar um ping-lo por endereço IP numa linha de comandos no *myVm2* máquina virtual:

```
ping 10.0.0.4
```

Receber quatro respostas. Se executar o ping pelo nome da máquina virtual (*myVm1*), em vez do endereço IP, ping falhar, porque *myVm1* é um nome de anfitrião desconhecido. Resolução de nomes predefinido do Azure funciona entre as máquinas virtuais na mesma rede virtual, mas não entre máquinas virtuais nas redes virtuais diferentes. Para resolver nomes através de redes virtuais, terá [implementar o seu próprio servidor DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md) ou utilize [domínios privados do DNS do Azure](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Desligar as sessões RDP para ambos *myVm1* e *myVm2*.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário utilizar [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o grupo de recursos e todos os recursos nele contidos.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

**<a name="register"></a>Registar-se para a pré-visualização de peering de rede virtual global**

O peering de redes virtuais na mesma região encontra-se em disponibilidade geral. Peering redes virtuais em diferentes regiões está atualmente em pré-visualização. Consulte [atualizações da rede Virtual](https://azure.microsoft.com/updates/?product=virtual-network) de regiões disponíveis. A ponto redes virtuais em regiões, primeiro tem de registar para a pré-visualização, concluindo os seguintes passos (dentro da subscrição cada rede virtual que pretende elemento está no):

1. Registe a subscrição que cada rede virtual que pretende ponto destina-se na pré-visualização, introduzindo os comandos seguintes:

    ```powershell-interactive
    Register-AzureRmProviderFeature `
      -FeatureName AllowGlobalVnetPeering `
      -ProviderNamespace Microsoft.Network
    
    Register-AzureRmResourceProvider `
      -ProviderNamespace Microsoft.Network
    ```
2. Confirme que são registadas para a pré-visualização, introduzindo o seguinte comando:

    ```powershell-interactive    
    Get-AzureRmProviderFeature `
      -FeatureName AllowGlobalVnetPeering `
      -ProviderNamespace Microsoft.Network
    ```

    Se tentar elemento redes virtuais em diferentes regiões antes do **RegistrationState** recebe depois de introduzir o comando anterior é de saída **registada** para ambas as subscrições, falha de peering. .

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu a ligar duas redes com o peering de rede virtual. Pode [ligar o seu próprio computador a uma rede virtual](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) através de uma VPN e interaja com os recursos numa rede virtual, ou em redes virtuais em modo de peering.

Continue a amostras de script para scripts reutilizáveis concluir a muitas das tarefas abrangidas os artigos de rede virtual.

> [!div class="nextstepaction"]
> [Exemplos de script de rede virtual](../networking/powershell-samples.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
