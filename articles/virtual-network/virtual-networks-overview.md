---
title: Rede Virtual do Azure | Microsoft Docs
description: Saiba mais sobre os conceitos e as funcionalidades da Rede Virtual do Azure.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: As someone with a basic network background that is new to Azure, I want to understand the capabilities of Azure Virtual Network, so that my Azure resources such as VMs, can securely communicate with each other, the internet, and my on-premises resources.
ms.assetid: 9633de4b-a867-4ddf-be3c-a332edf02e24
ms.service: virtual-network
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 3/23/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: 851c8c1eb13497355038ef4a8d5f1f9326c8c3bc
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33781185"
---
# <a name="what-is-azure-virtual-network"></a>O que é a Rede Virtual do Azure?

A Rede Virtual do Azure permite a muitos tipos de recursos do Azure, como as Máquinas Virtuais (VMs) do Azure, comunicarem entre si, a Internet e redes no local de forma segura. A Rede Virtual do Azure fornece as principais capacidades seguintes:

## <a name="isolation-and-segmentation"></a>Isolamento e segmentação

Pode implementar várias redes virtuais dentro de cada [subscrição](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) do Azure e [região](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region) do Azure. Cada uma das redes virtuais está isolada das restantes. Para cada rede virtual, pode:
- Especificar um espaço de endereços IP privado personalizado, utilizando endereços públicos e privados (RFC 1918). O Azure atribui aos recursos numa rede virtual um endereço IP privado a partir do espaço de endereços que atribuir.
- Segmentar a rede virtual numa ou mais sub-redes e atribuir uma parte do espaço de endereços da rede virtual a cada sub-rede.
- Utilizar a resolução de nomes fornecida pelo Azure ou especificar o seu servidor DNS para utilização pelos recursos numa rede virtual.

## <a name="communicate-with-the-internet"></a>Comunicar com a Internet

Todos os recursos numa rede virtual podem comunicar com a Internet à saída, por predefinição. Pode comunicar com um recurso à entrada, ao atribuir-lhe um endereço IP público. Para obter mais informações, veja [Endereços IP públicos](virtual-network-public-ip-address.md).

## <a name="communicate-between-azure-resources"></a>Comunicar entre os recursos do Azure

Os recursos do Azure comunicam de forma segura entre si, de uma das seguintes formas:

- **Através de uma rede virtual**: pode implementar VMs e vários outros tipos de recursos do Azure numa rede virtual, como Ambientes do Serviço de Aplicações do Azure, Serviço de Kubernetes do Azure (AKS) e Conjuntos de Dimensionamento de Máquinas Virtuais do Azure. Para ver uma lista completa dos recursos do Azure que pode implementar numa rede virtual, veja [Integração de serviço da rede virtual](virtual-network-for-azure-services.md). 
- **Através de um ponto final de serviço da rede virtual**: expanda o seu espaço de endereços privados da rede virtual e a identidade da sua rede virtual aos recursos do serviço do Azure, como Contas de Armazenamento do Azure e Bases de Dados SQL do Azure, através de uma ligação direta. Os pontos finais de serviço permitem-lhe obter os seus recursos críticos de serviço do Azure para apenas uma rede virtual. Para obter mais informações, veja [Descrição geral de pontos finais de serviço da rede virtual](virtual-network-service-endpoints-overview.md).
 
## <a name="communicate-with-on-premises-resources"></a>Comunicar com os recursos no local

Pode ligar os computadores e redes no local a uma rede virtual, com qualquer combinação das seguintes opções:

- **Rede privada virtual (VPN) ponto a site:** estabelecida entre uma rede virtual e um único computador na sua rede. Cada computador que pretende estabelecer conectividade com uma rede virtual tem de configurar a respetiva ligação. Este tipo de ligação é excelente se estiver a começar a utilizar o Azure ou para os programadores, uma vez que necessita de pouca ou nenhumas alterações à sua rede existente. A comunicação entre o computador e uma rede virtual é enviada através de um túnel encriptado através da Internet. Para obter mais informações, veja [VPN ponto a site](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#P2S).
- **Rede de VPNs:** estabelecida entre o dispositivo VPN no local e um Gateway de VPN do Azure que é implementado numa rede virtual. Este tipo de ligação permite a qualquer recurso no local, que esteja autorizado por si, a aceder a uma rede virtual. A comunicação entre o dispositivo VPN no local e um gateway de VPN do Azure é enviada por um túnel encriptado através da Internet. Para obter mais informações, veja [Rede de VPNs](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti).
- **Azure ExpressRoute:** estabelecida entre a rede e o Azure, através de um parceiro do ExpressRoute. Esta ligação é privada. O tráfego não é transmitido pela Internet. Para saber mais, veja [ExpressRoute](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#ExpressRoute).

## <a name="filter-network-traffic"></a>Filtre o tráfego de rede
Pode filtrar o tráfego de rede entre as sub-redes com uma ou ambas das seguintes opções:
- **Grupos de segurança de rede:** um grupo de segurança de rede pode conter várias regras de segurança de entrada e saída que permitem filtrar o tráfego de e para recursos por endereço IP, porta e protocolo de origem e de destino. Para obter mais informações, veja [Grupos de segurança de rede](security-overview.md#network-security-groups).
- **Aplicações virtuais de rede:** uma aplicação virtual de rede é uma VM que executa uma função de rede, como uma firewall, otimização de rede alargada ou outra função de rede. Para ver uma lista das aplicações virtuais de rede disponíveis que pode implementar numa rede virtual, veja [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances).

## <a name="route-network-traffic"></a>Encaminhe o tráfego de rede

O Azure encaminha tráfego entre sub-redes, redes virtuais ligadas, redes no local e na Internet, por predefinição. Pode implementar uma ou ambas das seguintes opções para substituir as rotas predefinidas que o Azure cria:
- **Tabelas de rotas:** pode criar tabelas de rotas personalizadas com rotas que controlam para onde o tráfego é encaminhado para cada sub-rede. Obtenha mais informações sobre [tabelas de rotas](virtual-networks-udr-overview.md#user-defined).
- **Rotas de protocolo BGP (Border Gateway Protocol):** se ligar a rede virtual à sua rede no local através de uma ligação de Gateway de VPN do Azure ou ExpressRoute, pode propagar as rotas de protocolo BGP no local para as suas redes virtuais. Saiba mais sobre como utilizar o protocolo BGP com o [Gateway de VPN do Azure](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) e o [ExpressRoute](../expressroute/expressroute-routing.md?toc=%2fazure%2fvirtual-network%2ftoc.json#dynamic-route-exchange).

## <a name="connect-virtual-networks"></a>Ligar redes virtuais

Pode ligar redes virtuais entre si, ativando recursos em qualquer rede virtual para comunicarem entre si, com o peering de rede virtual. As redes virtuais a que liga podem estar nas mesmas regiões ou em regiões diferentes do Azure. Para obter mais informações, veja [Peering de rede virtual](virtual-network-peering-overview.md).

## <a name="next-steps"></a>Passos seguintes

Tem agora uma descrição geral da Rede Virtual do Azure. Para começar a utilizar uma rede virtual, crie uma, implemente algumas VMs na mesma e comunique entre as VMs. Para saber como, veja o início rápido [Criar uma rede virtual](quick-create-portal.md).