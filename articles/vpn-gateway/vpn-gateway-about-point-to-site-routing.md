---
title: Acerca do Azure ponto a Site encaminhamento | Microsoft Docs
description: Este artigo ajuda-o a compreender a forma como se comporta a VPN ponto a Site encaminhamento.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: ''
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/16/2018
ms.author: anzaman
ms.openlocfilehash: d25709fb4abb1b8a35596c3dc246f7419a99419b
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="about-point-to-site-vpn-routing"></a>Sobre o encaminhamento de VPN de ponto a Site

Este artigo ajuda-o a compreender a forma como se comporta a VPN do Azure ponto a Site encaminhamento. Comportamento de encaminhamento P2S VPN está dependente do cliente do SO, o protocolo utilizado para a ligação VPN e como as redes virtuais (VNets) estão ligadas entre si.

Atualmente, o Azure suporta os dois protocolos para acesso remoto, IKEv2 e SSTP. IKEv2 é suportado em muitos sistemas operativos de cliente, incluindo Windows, Linux, MacOS, Android e iOS. SSTP só é suportada no Windows. Se fizer uma alteração à topologia da rede e que os clientes de VPN do Windows, o pacote de cliente VPN para clientes Windows tem de ser transferido e instalado novamente para que as alterações sejam aplicadas ao cliente.

> [!NOTE]
> Este artigo aplica-se apenas a IKEv2.
>

## <a name="diagrams"></a>Sobre os diagramas

Há uma série de diferentes diagramas neste artigo. Cada secção mostra uma topologia diferente ou uma configuração. Para efeitos deste artigo, Site a Site (S2S) e ligações VNet a VNet funcionarem da mesma forma, como são ambos os túneis IPsec. Todos os gateways VPN neste artigo são baseadas na rota.

## <a name="isolatedvnet"></a>Uma VNet isolada

A ligação de gateway VPN de ponto a Site neste exemplo é para uma VNet que está ligada ou não em modo de peering com qualquer outra rede virtual (VNet1). Neste exemplo, os clientes através de SSTP ou IKEv2 podem aceder a VNet1.

![isolada VNet encaminhamento](./media/vpn-gateway-about-point-to-site-routing/1.jpg "isolada de encaminhamento de VNet")

### <a name="address-space"></a>Espaço de endereços

* VNet1: 10.1.0.0/16

### <a name="routes-added"></a>Rotas adicionadas

* Rotas adicionadas para clientes do Windows: 10.1.0.0/16, 192.168.0.0/24

* Rotas adicionadas para clientes não-Windows: 10.1.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Clientes Windows podem aceder a VNet1

* Clientes do Windows não podem aceder a VNet1

## <a name="multipeered"></a>Várias VNets em modo de peering

Neste exemplo, a ligação de gateway VPN de ponto a Site é para a VNet1. VNet1 está em modo de peering com VNet2. VNet 2 está em modo de peering com VNet3. VNet1 está em modo de peering com VNet4. Não há nenhum peering direta entre VNet1 e VNet3. VNet1 tem "Permitir que o trânsito do gateway" e VNet2 tem "Utilizar remotos gateways" ativada.

Clientes que utilizam o Windows podem aceder diretamente as VNets, mas o cliente VPN tem de ser transferido novamente se as alterações efetuadas a topologia de rede ou de VNet peering. Clientes Windows não podem aceder diretamente as VNets. Acesso não é transitiva e está limitado a apenas diretamente as VNets.

![as VNets em modo de peering de vários](./media/vpn-gateway-about-point-to-site-routing/2.jpg "VNets em modo de peering de vários")

### <a name="address-space"></a>O espaço de endereços:

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* VNet4: 10.4.0.0/16

### <a name="routes-added"></a>Rotas adicionadas

* Rotas adicionadas para clientes do Windows: 10.1.0.0/16, 10.2.0.0/16, 10.4.0.0/16, 192.168.0.0/24

* Rotas adicionadas para clientes não-Windows: 10.1.0.0/16, 10.2.0.0/16, 10.4.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Clientes Windows podem aceder a VNet1, VNet2 e VNet4, mas o cliente VPN tem de ser transferido novamente para efetuar quaisquer alterações de topologia entre em vigor.

* Clientes Windows não podem aceder VNet1, VNet2 e VNet4

## <a name="multis2s"></a>Várias VNets ligadas através de uma VPN S2S

Neste exemplo, a ligação de gateway VPN de ponto a Site é para a VNet1. Está ligado VNet1 para a VNet2 utilizando uma ligação VPN de Site para Site. VNet2 está ligado a VNet3 utilizando uma ligação VPN de Site para Site. Não há nenhum peering direta ou a ligação VPN de Site a Site entre VNet1 e VNet3. Todas as ligações Site a Site não estão em execução BGP para o encaminhamento.

Clientes que utilizam, SO Windows ou outro suportado, só podem aceder a VNet1. Para aceder a VNets adicionais, tem de utilizar o BGP.

![várias VNets e S2S](./media/vpn-gateway-about-point-to-site-routing/3.jpg "várias VNets e S2S")

### <a name="address-space"></a>Espaço de endereços

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

### <a name="routes-added"></a>Rotas adicionadas

* Rotas adicionadas para clientes do Windows: 10.1.0.0/16, 192.168.0.0/24

* Rotas adicionadas para clientes não-Windows: 10.1.0.0/16, 10.2.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Clientes Windows só podem aceder a VNet1

* Clientes Windows não podem aceder apenas VNet1

## <a name="multis2sbgp"></a>Várias VNets ligadas através de uma VPN S2S (BGP)

Neste exemplo, a ligação de gateway VPN de ponto a Site é para a VNet1. Está ligado VNet1 para a VNet2 utilizando uma ligação VPN de Site para Site. VNet2 está ligado a VNet3 utilizando uma ligação VPN de Site para Site. Não há nenhum peering direta ou a ligação VPN de Site a Site entre VNet1 e VNet3. Todas as ligações Site a Site estão a executar BGP para o encaminhamento.

Clientes que utilizam, SO Windows ou outro suportados, podem aceder a todas as VNets que estão ligadas utilizando uma ligação VPN de Site a Site, mas rotas para VNets ligadas têm de ser adicionado manualmente para os clientes Windows.

![várias VNets e S2S (BGP)](./media/vpn-gateway-about-point-to-site-routing/4.jpg "várias VNets e S2S BGP")

### <a name="address-space"></a>Espaço de endereços

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

### <a name="routes-added"></a>Rotas adicionadas

* Rotas adicionadas para clientes do Windows: 10.1.0.0/16

* Rotas adicionadas para clientes não-Windows: 10.1.0.0/16, 10.2.0.0/16, 10.3.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Clientes Windows podem aceder a VNet1, VNet2 e VNet3, mas rotas para a VNet2 e VNet3 terão de ser adicionados manualmente.

* Clientes Windows não podem aceder VNet1, VNet2 e VNet3

## <a name="vnetbranch"></a>Uma VNet e o escritório de sucursal

Neste exemplo, a ligação de gateway VPN de ponto a Site é para a VNet1. VNet1 não está ligado / em modo de peering com outra rede virtual, mas está ligado a um site no local através de uma ligação de VPN de Site a Site não está a executar o BGP.

Clientes Windows podem aceder a VNet1 e o escritório de sucursal (Site1), mas as rotas para Site1 tem ser adicionadas manualmente para o cliente. Clientes Windows não podem aceder a VNet1, bem como o Site1 no local.

![encaminhamento com uma VNet e um escritório de sucursal](./media/vpn-gateway-about-point-to-site-routing/5.jpg "encaminhamento com uma VNet e um escritório de sucursal")

### <a name="address-space"></a>Espaço de endereços

* VNet1: 10.1.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Rotas adicionadas

* Rotas adicionadas para clientes do Windows: 10.1.0.0/16, 192.168.0.0/24

* Rotas adicionadas para clientes não-Windows: 10.1.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Clientes Windows podem aceder apenas VNet1

* Clientes Windows não podem aceder apenas VNet1

## <a name="vnetbranchbgp"></a>Uma VNet e o escritório de sucursal (BGP)

Neste exemplo, a ligação de gateway VPN de ponto a Site é para a VNet1. VNet1 não ligado ou não está em modo de peering com outra rede virtual, mas está ligado a um site no local (Site1) através de uma ligação de VPN de Site a Site que executa o BGP.

Clientes Windows podem aceder a VNet e o escritório de sucursal (Site1), mas as rotas para Site1 tem ser adicionadas manualmente para o cliente. Clientes do Windows não podem aceder a VNet, bem como o escritório de sucursal local.

![uma VNet e o escritório de sucursal (BGP)](./media/vpn-gateway-about-point-to-site-routing/6.jpg "uma VNet e o escritório de sucursal")

### <a name="address-space"></a>Espaço de endereços

* VNet1: 10.1.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Rotas adicionadas

* Rotas adicionadas para clientes do Windows: 10.1.0.0/16, 192.168.0.0/24

* Rotas adicionadas para clientes não-Windows: 10.1.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Clientes Windows podem aceder a VNet1 e Site1, mas rotas para Site1 terão de ser adicionados manualmente.

* Clientes Windows não podem aceder VNet1 e Site1.


## <a name="multivnets2sbranch"></a>Várias VNets ligadas através de S2S e o escritório de sucursal

Neste exemplo, a ligação de gateway VPN de ponto a Site é para a VNet1. Está ligado VNet1 para a VNet2 utilizando uma ligação VPN de Site para Site. VNet2 está ligado a VNet3 utilizando uma ligação VPN de Site para Site. Não há nenhum peering direta ou túnel VPN Site a Site entre as redes VNet1 e VNet3. VNet3 está ligado a uma sucursal (Site1) utilizando uma ligação VPN de Site para Site. Todas as ligações VPN não estão a executar o BGP.

Todos os clientes podem aceder apenas a VNet1.

![várias VNet S2S e ramo office](./media/vpn-gateway-about-point-to-site-routing/7.jpg "várias VNet S2S e ramo office")

### <a name="address-space"></a>Espaço de endereços

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Rotas adicionadas

* Os clientes de adicionadas rotas: 10.1.0.0/16, 192.168.0.0/24

* Rotas adicionadas para clientes não-Windows: 10.1.0.0/16 10.2.0.0/16, 10.3.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Os clientes Windows podem aceder apenas VNet1

* Clientes Windows não podem aceder apenas VNet1

## <a name="multivnets2sbranchbgp"></a>Várias VNets ligadas através de S2S e o escritório de sucursal (BGP)

Neste exemplo, a ligação de gateway VPN de ponto a Site é para a VNet1. Está ligado VNet1 para a VNet2 utilizando uma ligação VPN de Site para Site. VNet2 está ligado a VNet3 utilizando uma ligação VPN de Site para Site. Não há nenhum peering direta ou túnel VPN Site a Site entre as redes VNet1 e VNet3. VNet3 está ligado a uma sucursal (Site1) utilizando uma ligação VPN de Site para Site. Todas as ligações VPN não estão a executar o BGP. Todas as ligações VPN estão a executar o BGP.

Clientes que utilizam Windows podem aceder a VNets e sites que estão ligados através de uma ligação VPN de Site a Site, mas as rotas VNet2, VNet3 e Site1 tem ser adicionados manualmente para o cliente. Clientes Windows não podem aceder a VNets e sites que estão ligados utilizando uma ligação de VPN de Site para Site sem qualquer intervenção manual. O acesso é transitiva e os clientes podem aceder a recursos ligados todas as VNets e sites (no local).

![várias VNet S2S e ramo office](./media/vpn-gateway-about-point-to-site-routing/8.jpg "várias VNet S2S e ramo office")

### <a name="address-space"></a>Espaço de endereços

* VNet1: 10.1.0.0/16

* VNet2: 10.2.0.0/16

* VNet3: 10.3.0.0/16

* Site1: 10.101.0.0/16

### <a name="routes-added"></a>Rotas adicionadas

* Os clientes de adicionadas rotas: 10.1.0.0/16, 192.168.0.0/24

* Rotas adicionadas para clientes não-Windows: 10.1.0.0/16 10.2.0.0/16, 10.3.0.0/16, 10.101.0.0/16, 192.168.0.0/24

### <a name="access"></a>Access

* Os clientes Windows podem aceder a VNet1, VNet2, VNet3 e Site1, mas as rotas VNet2, VNet3 e Site1 tem ser adicionadas manualmente para o cliente.

* Clientes Windows não podem aceder a VNet1, Vnet2, VNet3 e Site1.

## <a name="next-steps"></a>Passos Seguintes

Consulte [criar uma VPN P2S no portal do Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md) para começar a criar a VPN P2S.
