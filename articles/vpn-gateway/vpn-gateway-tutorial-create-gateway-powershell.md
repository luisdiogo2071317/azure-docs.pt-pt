---
title: Criar e Gerir o gateway de VPN do Azure com o PowerShell | Microsoft Docs
description: Tutorial - Criar e Gerir o gateway de VPN com o módulo do Azure PowerShell
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: tutorial
ms.date: 02/11/2019
ms.author: yushwang
ms.custom: mvc
ms.openlocfilehash: afe71953e9917ccf274742124d59cb790f15521b
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/19/2019
ms.locfileid: "56414138"
---
# <a name="tutorial-create-and-manage-a-vpn-gateway-using-powershell"></a>Tutorial: Criar e gerir um gateway de VPN com o PowerShell

Os gateways de VPN do Azure fornecem conectividade em vários locais entre as instalações do cliente e o Azure. Este tutorial abrange itens básicos da implementação do gateway de VPN do Azure, tais como criar e gerir um gateway de VPN. Saiba como:

> [!div class="checklist"]
> * Criar um gateway de VPN
> * Ver o endereço IP público
> * Redimensionar um gateway de VPN
> * Repor um Gateway de VPN

O diagrama seguinte mostra a rede virtual e o gateway de VPN criados como parte deste tutorial.

![VNet e gateway de VPN](./media/vpn-gateway-tutorial-create-gateway-powershell/vnet1-gateway.png)

### <a name="azure-cloud-shell-and-azure-powershell"></a>Azure Cloud Shell e Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [working with cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

## <a name="common-network-parameter-values"></a>Valores de parâmetros de rede comuns

Altere os valores abaixo, com base na sua configuração de rede e de ambiente, em seguida, copiar e colar para definir as variáveis para este tutorial. Se a sua sessão do Cloud Shell exceder o tempo limite, ou precisar de utilizar uma janela do PowerShell diferente, copie e cole as variáveis à sua sessão de novo e continue o tutorial.

```azurepowershell-interactive
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "East US"
$FESubnet1   = "FrontEnd"
$BESubnet1   = "Backend"
$GwSubnet1   = "GatewaySubnet"
$VNet1Prefix = "10.1.0.0/16"
$FEPrefix1   = "10.1.0.0/24"
$BEPrefix1   = "10.1.1.0/24"
$GwPrefix1   = "10.1.255.0/27"
$VNet1ASN    = 65010
$DNS1        = "8.8.8.8"
$Gw1         = "VNet1GW"
$GwIP1       = "VNet1GWIP"
$GwIPConf1   = "gwipconf1"
```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Criar um grupo de recursos com o [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) comando. Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. Primeiro, tem de ser criado um grupo de recursos. No exemplo seguinte, é criado um grupo de recursos com o nome *TestRG1* na região *E.U.A. Leste*:

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

O gateway de VPN do Azure fornece conectividade em vários locais e a funcionalidade de servidor VPN P2S para a sua rede virtual. Adicione o gateway de VPN a uma rede virtual existente ou crie uma nova rede virtual e o gateway. Este exemplo cria uma nova rede virtual com três sub-redes: GatewaySubnet, front-end e back-end usando [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) e [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork):

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$gwsub1 = New-AzVirtualNetworkSubnetConfig -Name $GWSubnet1 -AddressPrefix $GwPrefix1
$vnet   = New-AzVirtualNetwork `
            -Name $VNet1 `
            -ResourceGroupName $RG1 `
            -Location $Location1 `
            -AddressPrefix $VNet1Prefix `
            -Subnet $fesub1,$besub1,$gwsub1
```

## <a name="request-a-public-ip-address-for-the-vpn-gateway"></a>Pedir um endereço IP público para o gateway de VPN

Os gateways de VPN do Azure comunicam com os dispositivos VPN no local através da Internet para efetuar a negociação de IKE (Internet Key Exchange) e estabelecer túneis IPsec. Criar e atribuir um endereço IP público ao gateway de VPN, conforme mostrado no exemplo abaixo, com [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) e [New-AzVirtualNetworkGatewayIpConfig](/powershell/module/az.network/new-azvirtualnetworkgatewayipconfig):

> [!IMPORTANT]
> Atualmente, só pode utilizar um endereço IP público dinâmico para o gateway. O endereço IP estático não é suportado nos gateways de VPN do Azure.

```azurepowershell-interactive
$gwpip    = New-AzPublicIpAddress -Name $GwIP1 -ResourceGroupName $RG1 `
              -Location $Location1 -AllocationMethod Dynamic
$subnet   = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' `
              -VirtualNetwork $vnet
$gwipconf = New-AzVirtualNetworkGatewayIpConfig -Name $GwIPConf1 `
              -Subnet $subnet -PublicIpAddress $gwpip
```

## <a name="create-a-vpn-gateway"></a>Criar um gateway de VPN

Um gateway de VPN pode demorar 45 minutos ou mais a ser criado. Depois de concluída a criação do gateway, pode criar uma ligação entre a rede virtual e outra VNet. Em alternativa, pode criar uma ligação entre a rede virtual e uma localização no local. Criar um gateway de VPN com o [New-AzVirtualNetworkGateway](/powershell/module/az.network/New-azVirtualNetworkGateway) cmdlet.

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name $Gw1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
```

Valores de parâmetros chave:
* GatewayType: Uso **Vpn** para ligações de VNet a VNet e de site a site
* VpnType: Uso **RouteBased** para interagir com dispositivos de intervalo de VPN maiores e mais funcionalidades de encaminhamento
* GatewaySku: **VpnGw1** é o padrão; alterá-lo para VpnGw2 ou VpnGw3, se precisar de taxas de transferência maior ou mais ligações. Para obter mais informações, veja [SKUs de gateway](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

Se estiver a utilizar o TryIt, a sessão pode tempo limite. Que está OK. O gateway ainda irá criar.

Depois de concluída a criação do gateway, pode criar uma ligação entre a rede virtual e outra VNet ou criar uma ligação entre a rede virtual e uma localização no local. Também pode configurar uma ligação P2S para a sua VNet a partir de um computador cliente.

## <a name="view-the-gateway-public-ip-address"></a>Ver o endereço IP público do gateway

Se souber o nome do endereço IP público, utilize [Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=azurermps-6.8.1) para mostrar o endereço IP público atribuído ao gateway.

Se a sua sessão excedeu o tempo limite, copie os parâmetros comuns de rede desde o início deste tutorial para a nova sessão e continuar, em seguida, continuar.

```azurepowershell-interactive
$myGwIp = Get-AzPublicIpAddress -Name $GwIP1 -ResourceGroup $RG1
$myGwIp.IpAddress
```

## <a name="resize-a-gateway"></a>Redimensionar um gateway

Pode alterar a SKU do gateway de VPN após a criação do gateway. SKUs de gateway diferentes suportam especificações diferentes, tais como débitos, número de ligações, etc. O exemplo seguinte utiliza [redimensionamento AzVirtualNetworkGateway](/powershell/module/az.network/Resize-azVirtualNetworkGateway) para redimensionar o gateway de VpnGw1 VpnGw2. Para obter mais informações, veja [SKUs de gateway](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

```azurepowershell-interactive
$gateway = Get-AzVirtualNetworkGateway -Name $Gw1 -ResourceGroup $RG1
Resize-AzVirtualNetworkGateway -GatewaySku VpnGw2 -VirtualNetworkGateway $gateway
```

O redimensionamento de um gateway de VPN também demora cerca de 30 a 45 minutos, embora esta operação **não** vá interromper ou remover as ligações e configurações existentes.

## <a name="reset-a-gateway"></a>Repor um gateway

Como parte dos passos de resolução de problemas, pode repor o gateway de VPN do Azure para obrigar o gateway VPN a reiniciar as configurações de túnel IPsec/IKE. Uso [reposição-AzVirtualNetworkGateway](/powershell/module/az.network/Reset-azVirtualNetworkGateway) para repor o gateway.

```azurepowershell-interactive
$gateway = Get-AzVirtualNetworkGateway -Name $Gw1 -ResourceGroup $RG1
Reset-AzVirtualNetworkGateway -VirtualNetworkGateway $gateway
```

Para obter mais informações, veja [Repor um gateway de VPN](vpn-gateway-resetgw-classic.md).

## <a name="clean-up-resources"></a>Limpar recursos

Se avançar para o [próximo tutorial](vpn-gateway-tutorial-vpnconnection-powershell.md), vai querer manter estes recursos porque eles são o pré-requisito.

No entanto, se o gateway faz parte de um protótipo, teste ou implementação de uma prova de conceito, pode utilizar o [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) de comando para remover o grupo de recursos, o gateway VPN, e todos os recursos relacionados.

```azurepowershell-interactive
Remove-AzResourceGroup -Name $RG1
```

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu sobre a criação e gestão básicas do gateway de VPN, como, por exemplo:

> [!div class="checklist"]
> * Criar um gateway de VPN
> * Ver o endereço IP público
> * Redimensionar um gateway de VPN
> * Repor um Gateway de VPN

Avance para os tutoriais seguintes para saber mais sobre ligações S2S, VNet a VNet e P2S.

> [!div class="nextstepaction"]
> * [Criar ligações S2S](vpn-gateway-tutorial-vpnconnection-powershell.md)
> * [Criar ligações VNet a VNet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [Criar ligações P2S](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
