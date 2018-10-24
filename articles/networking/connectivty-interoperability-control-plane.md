---
title: 'Interoperabilidade do ExpressRoute, de VPN de Site a site e VNet Peering - análise do plano de controlo: interoperabilidade de funcionalidades de conectividade de back-end do Azure | Documentos da Microsoft'
description: Esta página fornece a análise de plano de controlo da configuração do teste criada para analisar a interoperabilidade dos recursos do ExpressRoute, de VPN de Site a site e VNet Peering.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: ee887da18b5666e61bc25365791b2e7dffb925e0
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947359"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---control-plane-analysis"></a>Interoperabilidade do ExpressRoute, de VPN de Site a site e VNet Peering - análise do plano de controlo

Neste artigo, vamos percorrer a análise de plano de controlo da configuração do teste. Se quiser rever a configuração de teste, consulte a [configuração de teste][Setup]. Para rever os detalhes da configuração do programa de configuração de teste, consulte [configuração do programa de configuração de teste][Configuration].

Análise de plano de controlo, essencialmente, examina rotas trocadas entre redes dentro de uma topologia. Controlar a ajuda de análise de plano diferentes como ver a topologia de rede.

##<a name="hub-and-spoke-vnet-perspective"></a>Ponto de vista do hub- and -Spoke VNet

O diagrama seguinte ilustra a rede da VNet do Hub e a perspetiva de VNet Spoke (realçado em azul). O diagrama também mostra o número de sistema autónomo (ASN) de rede diferente e rotas trocadas entre várias redes. 

[![1]][1]

Tenha em atenção que o gateway do ExpressRoute do ASN de VNet é diferente do ASN de Routers Enterprise Edge Microsoft (MSEEs). Gateway do ExpressRoute utiliza um ASN privado (65515) e MSEEs utilizam pública ASN (12076) globalmente. Quando configurar o ExpressRoute de peering, porque MSEE é o mesmo nível, vai utilizar 12076 como o elemento de rede ASN. No lado do Azure, MSEE estabelece eBGP peering com GW do ExpressRoute. O peering de eBGP dupla que o MSEE estabelece para cada peering de ExpressRoute é transparente ao nível do plano de controlo. Por conseguinte, quando é visualizada uma tabela de rota do ExpressRoute, verá a VNet ExpressRoute GW ASN para prefixos da VNet. Uma captura de ecrã de tabela de rota de ExpressRoute do exemplo é mostrada abaixo: 

[![5]][5]

No Azure, o ASN só é significativo de uma perspectiva de peering. Por predefinição, o ASN de GW do ExpressRoute e VPN GW é 65515.

##<a name="on-premises-location-1-and-remote-vnet-perspective-via-expressroute-1"></a>Perspetiva de 1 de localização no local e remoto VNet através do ExpressRoute-1

1 localização no local e remoto VNet estão ambos ligado à VNet do Hub através do ExpressRoute 1 e, portanto, eles compartilham o mesmo ponto de vista da topologia, como mostra o diagrama abaixo.

[![2]][2]

##<a name="on-premises-location-1-and-branch-vnet-perspective-via-site-to-site-vpn"></a>Perspetiva de 1 de localização no local e ramo VNet através de VPN de Site a Site

1 de localização no local e o ramo VNet estão ambos ligados a GW de VPN da VNet do Hub através de ligações de VPN de Site a Site e, portanto, eles compartilham o mesmo ponto de vista da topologia, conforme mostrado no diagrama abaixo.

[![3]][3]

##<a name="on-premises-location-2-perspective"></a>Ponto de vista de 2 de localização no local

2 de localização no local está ligado à VNet do Hub através do peering privado do ExpressRoute 2. 

[![4]][4]

## <a name="further-reading"></a>Leitura adicional

### <a name="using-expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>Utilizar a conectividade VPN de Site a Site e ExpressRoute em conjunto

####  <a name="site-to-site-vpn-over-expressroute"></a>VPN de site a Site através do ExpressRoute

VPN site a Site pode ser configurada através do peering da Microsoft de ExpressRoute privada trocar dados entre a rede no local e as VNets do Azure com confidencialidade, anti-repetições, autenticidade e integridade. Para obter mais informações sobre como configurar VPN de IPSec de Site a Site no modo de túnel através do peering da Microsoft do ExpressRoute, veja [VPN de Site a site através do peering de Microsoft ExpressRoute][S2S-Over-ExR]. 

A grande limitação de configuração de S2S VPN através do peering da Microsoft é a taxa de transferência. O débito através do túnel IPSec é limitado pela capacidade de GW de VPN. O débito de VPN GW é menor em comparação com a taxa de transferência do ExpressRoute. Em tais cenários, utilizar o túnel IPSec para proteger o tráfego elevado e um peering privado para o tráfego de todos os outros ajudaria a otimizar a utilização de largura de banda do ExpressRoute.

#### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>VPN de site a Site como um caminho de ativação pós-falha seguro para o ExpressRoute
ExpressRoute é oferecido como par de circuito redundantes para elevada disponibilidade. Pode configurar a conectividade do ExpressRoute com redundância geográfica em diferentes regiões do Azure. Também como Feito em nossa configuração de teste, dentro de uma determinada região do Azure, se pretender que um caminho de ativação pós-falha para a conectividade do ExpressRoute, pode fazer isso usando Site-Site VPN. Quando os mesmos prefixos são anunciados através do ExpressRoute e S2S VPN, o Azure prefere o ExpressRoute através de S2S VPN. Para evitar o encaminhamento assimétricas entre ExpressRoute e S2S VPN, no local a configuração de rede também deve reciprocate preferindo ExpressRoute através de conectividade de S2S VPN.

Para obter mais informações sobre como configurar ligações coexistentes do ExpressRoute e VPN Site a Site, consulte [coexistência Site a Site e ExpressRoute][ExR-S2S-CoEx].

### <a name="extending-backend-connectivity-to-spoke-vnets-and-branch-locations"></a>Estendendo a conectividade de back-end para spoke VNets e localizações de sucursais

#### <a name="spoke-vnet-connectivity-using-vnet-peering"></a>Conectividade de VNet com peering de VNet do spoke

Arquitetura de Vnet do hub-and-spoke é amplamente utilizada. O hub é uma rede virtual (VNet) no Azure que age como um ponto central de conectividade entre a VNets spoke e à sua rede no local. Os spokes são VNets que partilham com o hub e podem ser utilizados para isolar cargas de trabalho. O tráfego flui entre o datacenter no local e o hub através de uma ligação VPN ou ExpressRoute. Para obter mais detalhes sobre a arquitetura, consulte [arquitetura Hub-and-Spoke][Hub-n-Spoke]

Permite a VNet peering dentro de uma região VNets para utilizar gateways de VNet do hub (gateways VPN e ExpressRoute) para comunicar com redes remotas spoke.

#### <a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>Ramificar a conectividade de VNet através de VPN de Site a Site

Se pretender que o ramo Vnets (em regiões diferentes) e redes no local se comuniquem entre si através de uma vnet do hub, a solução nativa do Azure é a conectividade VPN de site a site através da VPN. Uma opção alternativa é usar uma NVA para encaminhamento no hub.

Para configurar gateways de VPN, veja [configurar o Gateway de VPN][VPN]. Para implementar a NVA altamente disponível, consulte [implementar NVA altamente disponível][Deploy-NVA].

## <a name="next-steps"></a>Passos Seguintes

Para análise de plano de dados da configuração do teste e vistas de funcionalidades de monitorização de rede do Azure, veja [plano de dados de análise][Data-Analysis].

Para saber quantos circuitos do ExpressRoute pode ligar a um Gateway do ExpressRoute ou quantos Gateways do ExpressRoute pode ligar a um circuito do ExpressRoute ou para obter outros limites de dimensionamento do ExpressRoute, veja [FAQ do ExpressRoute][ExR-FAQ]


<!--Image References-->
[1]: ./media/backend-interoperability/HubView.png "hub e Spoke VNet perspectiva da topologia"
[2]: ./media/backend-interoperability/Loc1ExRView.png "localização-1 e a perspetiva de VNet remota através do ExpressRoute 1 da topologia"
[3]: ./media/backend-interoperability/Loc1VPNView.png "localização-1 e o ponto de vista do ramo VNet através de S2S VPN da topologia"
[4]: ./media/backend-interoperability/Loc2View.png "2 de localização de ponto de vista da topologia"
[5]: ./media/backend-interoperability/ExR1-RouteTable.png "RouteTable 1 do ExpressRoute"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-config
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-configuration
[Control-Analysis]:https://docs.microsoft.com/azure/networking/connectivty-interoperability-control-plane
[Data-Analysis]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-data-plane
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[VNet-Config]: https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering




