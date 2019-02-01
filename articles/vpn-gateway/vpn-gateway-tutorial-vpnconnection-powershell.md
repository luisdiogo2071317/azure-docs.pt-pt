---
title: Criar e Gerir ligações VPN S2S do Azure com o PowerShell | Microsoft Docs
description: Tutorial - Criar e Gerir ligações VPN S2S com o módulo Azure PowerShell
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 05/08/2018
ms.author: yushwang
ms.custom: mvc
ms.openlocfilehash: 0c71062bded65f8aa7c259c0678ee6675e2dab38
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55509478"
---
# <a name="create-and-manage-s2s-vpn-connections-with-the-azure-powershell-module"></a>Criar e Gerir ligações VPN S2S com o módulo Azure PowerShell

As ligações VPN S2S do Azure fornecem conectividade segura em vários locais entre as instalações do cliente e o Azure. Este tutorial explica os ciclos de vida das ligações VPN S2S IPsec, como criar e gerir uma ligação VPN S2S. Saiba como:

> [!div class="checklist"]
> * Criar uma ligação VPN S2S
> * Atualizar a propriedade de ligação: chave pré-partilhada, BGP, política IPsec/IKE
> * Adicionar mais ligações VPN
> * Eliminar uma ligação VPN

O diagrama seguinte mostra a topologia para este tutorial:

![Diagrama de ligação de rede de VPNs](./media/vpn-gateway-tutorial-vpnconnection-powershell/site-to-site-diagram.png)

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer a versão 5.3 ou posterior do módulo Azure PowerShell. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/azurerm/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="requirements"></a>Requisitos

Conclua o primeiro tutorial: "[Gateway de VPN de criar com o Azure PowerShell](vpn-gateway-tutorial-create-gateway-powershell.md)" para criar os seguintes recursos:

1. Grupo de recursos (TestRG1), rede virtual (VNet1) e GatewaySubnet
2. Gateway de VPN (VNet1GW)

Os valores dos parâmetros de rede virtual são listados abaixo. Tenha em atenção os valores adicionais para o gateway de rede local para representar a sua rede no local. Altere os valores com base na configuração do seu ambiente e da sua rede.

```azurepowershell-interactive
# Virtual network
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "East US"
$VNet1Prefix = "10.1.0.0/16"
$VNet1ASN    = 65010
$Gw1         = "VNet1GW"

# On-premises network
$LNG1        = "VPNsite1"
$LNGprefix1  = "10.101.0.0/24"
$LNGprefix2  = "10.101.1.0/24"
$LNGIP1      = "YourDevicePublicIP"

# Optional - on-premises BGP properties
$LNGASN1     = 65011
$BGPPeerIP1  = "10.101.1.254"

# Connection
$Connection1 = "VNet1ToSite1"
```

O fluxo de trabalho para criar uma ligação VPN S2S é simples:

1. Criar um gateway de rede local para representar a sua rede no local
2. Criar uma ligação entre o gateway de VPN do Azure e o gateway de rede local

## <a name="create-a-local-network-gateway"></a>Criar um gateway de rede local

Um gateway de rede local representa a sua rede no local. Pode especificar as propriedades da sua rede no local no gateway de rede local, incluindo:

* Endereço IP público do seu dispositivo VPN
* Espaço de endereços no local
* (Opcional) Atributos BGP (endereço IP do elemento de rede BGP e número de AS)

Crie um gateway de rede local com o comando [New-AzureRmLocalNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermlocalnetworkgateway?view=azurermps-6.8.1).

```azurepowershell-interactive
New-AzureRmLocalNetworkGateway -Name $LNG1 -ResourceGroupName $RG1 `
  -Location 'East US' -GatewayIpAddress $LNGIP1 -AddressPrefix $LNGprefix1,$LNGprefix2
```

## <a name="create-a-s2s-vpn-connection"></a>Criar uma ligação VPN S2S

Em seguida, crie uma ligação de Rede de VPNs entre o gateway de rede virtual e o dispositivo VPN com o comando [New-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworkgatewayconnection?view=azurermps-6.8.1). Repare que o “-ConnectionType” relativo a Rede de VPNs é *IPsec*.

```azurepowershell-interactive
$vng1 = Get-AzureRmVirtualNetworkGateway -Name $GW1  -ResourceGroupName $RG1
$lng1 = Get-AzureRmLocalNetworkGateway   -Name $LNG1 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection1 -ResourceGroupName $RG1 `
  -Location $Location1 -VirtualNetworkGateway1 $vng1 -LocalNetworkGateway2 $lng1 `
  -ConnectionType IPsec -SharedKey "Azure@!b2C3"
```

Adicione a propriedade "**-EnableBGP $True**" opcional para ativar o BGP para a ligação se estiver a utilizar o BGP. Está desativado por predefinição.

## <a name="update-the-vpn-connection-pre-shared-key-bgp-and-ipsecike-policy"></a>Atualizar a chave pré-partilhada, o BGP e a política IPsec/IKE da ligação VPN

### <a name="view-and-update-your-pre-shared-key"></a>Ver e atualizar a chave pré-partilhada

A ligação VPN S2S do Azure utiliza uma chave pré-partilhada (segredo) para fazer a autenticação entre o dispositivo VPN no local e o gateway de VPN do Azure. Pode ver e atualizar a chave pré-partilhada para uma ligação com os comandos [Get-AzureRmVirtualNetworkGatewayConnectionSharedKey](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworkgatewayconnectionsharedkey?view=azurermps-6.8.1) e [Set-AzureRmVirtualNetworkGatewayConnectionSharedKey](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgatewayconnectionsharedkey?view=azurermps-6.8.1).

> [!IMPORTANT]
> A chave pré-partilhada é uma cadeia de **carateres ASCII imprimíveis** com um máximo de 128 carateres.

Este comando mostra a chave pré-partilhada para a ligação:

```azurepowershell-interactive
Get-AzureRmVirtualNetworkGatewayConnectionSharedKey `
  -Name $Connection1 -ResourceGroupName $RG1
```

O resultado será "**Azure@!b2C3**", seguindo o exemplo acima. Utilize o comando abaixo para alterar o valor da chave pré-partilhada para "**Azure@!_b2=C3**":

```azurepowershell-interactive
Set-AzureRmVirtualNetworkGatewayConnectionSharedKey `
  -Name $Connection1 -ResourceGroupName $RG1 `
  -Value "Azure@!_b2=C3"
```

### <a name="enable-bgp-on-vpn-connection"></a>Ativar o BGP na ligação VPN

O gateway de VPN do Azure suporta o protocolo de encaminhamento dinâmico BGP. Pode ativar o BGP em cada ligação individual, dependendo se está a utilizar o BGP nos seus dispositivos e redes no local. Especifique as seguintes propriedades do BGP antes de ativar o BGP na ligação:

* ASN (Número de Sistema Autónomo) da VPN do Azure
* ASN do gateway de rede local no local
* Endereço IP do elemento de rede BGP do gateway de rede local no local

Se não tiver configurado as propriedades do BGP, utilize os seguintes comandos para adicionar essas propriedades para o seu gateway VPN e gateway de rede local: [Set-AzureRmVirtualNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgateway?view=azurermps-6.8.1) e [conjunto AzureRmLocalNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermlocalnetworkgateway?view=azurermps-6.8.1).

```azurepowershell-interactive
$vng1 = Get-AzureRmVirtualNetworkGateway -Name $GW1  -ResourceGroupName $RG1
Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $vng1 -Asn $VNet1ASN

$lng1 = Get-AzureRmLocalNetworkGateway   -Name $LNG1 -ResourceGroupName $RG1
Set-AzureRmLocalNetworkGateway -LocalNetworkGateway $lng1 `
  -Asn $LNGASN1 -BgpPeeringAddress $BGPPeerIP1
```

Ative o BGP com o comando [Set-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgatewayconnection?view=azurermps-6.8.1).

```azurepowershell-interactive
$connection = Get-AzureRmVirtualNetworkGatewayConnection `
  -Name $Connection1 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection `
  -EnableBGP $True
```

Pode desativar o BGP, alterando o valor da propriedade "-EnableBGP" para **$False**. Consulte o artigo [BGP nos gateways de VPN do Azure](vpn-gateway-bgp-resource-manager-ps.md) para obter uma explicação mais detalhada do BGP nos gateways de VPN do Azure.

### <a name="apply-a-custom-ipsecike-policy-on-the-connection"></a>Aplicar uma política IPsec/IKE personalizada na ligação

Pode aplicar uma política IPsec/IKE opcional para especificar a combinação exata de algoritmos criptográficos IPsec/IKE e forças de chave na ligação, em vez de utilizar as [propostas predefinidas](vpn-gateway-about-vpn-devices.md#ipsec). O script de exemplo seguinte cria uma política IPsec/IKE diferente com os seguintes parâmetros e algoritmos:

* IKEv2: AES256, SHA256, DHGroup14
* IPsec: Aes128, SHA1, PFS14, 14 400 de duração do SA segundos & 102,400,000 KB

```azurepowershell-interactive
$connection = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection1 `
                -ResourceGroupName $RG1
$newpolicy  = New-AzureRmIpsecPolicy `
                -IkeEncryption AES256 -IkeIntegrity SHA256 -DhGroup DHGroup14 `
                -IpsecEncryption AES128 -IpsecIntegrity SHA1 -PfsGroup PFS2048 `
                -SALifeTimeSeconds 14400 -SADataSizeKilobytes 102400000

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection `
  -IpsecPolicies $newpolicy
```

Consulte o artigo [Política IPsec/IKE para ligações S2S ou VNet a VNet](vpn-gateway-ipsecikepolicy-rm-powershell.md) para ver uma lista completa de algoritmos e instruções.

## <a name="add-another-s2s-vpn-connection"></a>Adicionar outra ligação VPN S2S

Para adicionar outra ligação VPN S2S ao mesmo gateway de VPN, crie outro gateway de rede local e crie uma nova ligação entre o novo gateway de rede local e o gateway de VPN. Siga o exemplo neste artigo.

```azurepowershell-interactive
# On-premises network
$LNG2        = "VPNsite2"
$Location2   = "West US"
$LNGprefix21 = "10.102.0.0/24"
$LNGprefix22 = "10.102.1.0/24"
$LNGIP2      = "YourDevicePublicIP"
$Connection2 = "VNet1ToSite2"

New-AzureRmLocalNetworkGateway -Name $LNG2 -ResourceGroupName $RG1 `
  -Location $Location2 -GatewayIpAddress $LNGIP2 -AddressPrefix $LNGprefix21,$LNGprefix22

$vng1 = Get-AzureRmVirtualNetworkGateway -Name $GW1  -ResourceGroupName $RG1
$lng2 = Get-AzureRmLocalNetworkGateway   -Name $LNG2 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection2 -ResourceGroupName $RG1 `
  -Location $Location1 -VirtualNetworkGateway1 $vng1 -LocalNetworkGateway2 $lng2 `
  -ConnectionType IPsec -SharedKey "AzureA1%b2_C3+"
```

Agora, existem duas ligações VPN S2S para o seu gateway de VPN do Azure.

![Ligações VPN Multilocal](./media/vpn-gateway-tutorial-vpnconnection-powershell/multisite-connections.png)

## <a name="delete-a-s2s-vpn-connection"></a>Eliminar uma ligação VPN S2S

Elimine uma ligação VPN S2S com o comando [Remove-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermvirtualnetworkgatewayconnection?view=azurermps-6.8.1).

```azurepowershell-interactive
Remove-AzureRmVirtualNetworkGatewayConnection -Name $Connection2 -ResourceGroupName $RG1
```

Elimine o gateway de rede local se já não precisar dele. Não é possível eliminar um gateway de rede local se existirem outras ligações associadas ao mesmo.

```azurepowershell-interactive
Remove-AzureRmVirtualNetworkGatewayConnection -Name $LNG2 -ResourceGroupName $RG1
```

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu a criar e gerir ligações VPN S2S, como, por exemplo, a:

> [!div class="checklist"]
> * Criar uma ligação VPN S2S
> * Atualizar a propriedade de ligação: chave pré-partilhada, BGP, política IPsec/IKE
> * Adicionar mais ligações VPN
> * Eliminar uma ligação VPN

Avance para os tutoriais seguintes para saber mais sobre ligações S2S, VNet a VNet e P2S.

> [!div class="nextstepaction"]
> * [Criar ligações VNet a VNet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [Criar ligações P2S](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
