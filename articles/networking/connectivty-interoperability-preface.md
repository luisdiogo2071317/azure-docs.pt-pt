---
title: 'Interoperabilidade do ExpressRoute, de VPN de Site a site e VNet Peering - configuração de teste: interoperabilidade de funcionalidades de conectividade de back-end do Azure | Documentos da Microsoft'
description: Esta página fornece uma configuração de teste que serve para analisar a interoperabilidade dos recursos do ExpressRoute, de VPN de Site a site e VNet Peering.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: e859a0a3ac35a9d9f2dab579b7609192e599f90f
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947353"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---test-setup"></a>Interoperabilidade do ExpressRoute, de VPN de Site a site e VNet peering - configuração de teste
Neste artigo, vamos identificar uma configuração de teste que podemos usar para analisar como as diferentes funcionalidades entre funcionam com os outros dois ao nível do plano de controlo e o plano de dados. Antes de discutir a configuração de teste, vamos rapidamente ver o que significam esses diferentes funcionalidades de rede do Azure.

ExpressRoute: Através do ExpressRoute peering privado pode ligar-se diretamente os espaços IP privados da sua rede no local para as implementações de VNet do Azure.  Com o ExpressRoute pode alcançar maior largura de banda e ligação privada. Existem muitos ExpressRoute ao meio ambiente parceiros que oferecem conectividade do ExpressRoute com um SLA. Para saber mais sobre o ExpressRoute e como configurá-lo, consulte [introdução ao ExpressRoute][ExpressRoute]

VPN de site a Site: Para ligar em segurança uma rede no local para o Azure pela Internet ou através do ExpressRoute, a opção de VPN Site a Site (S2S) está disponível. Para saber mais sobre como configurar a S2S VPN para ligar ao Azure, veja [configurar o Gateway de VPN][VPN]

O VNet peering: O VNet peering está disponível para estabelecer a conectividade entre redes virtuais (VNets). Para saber mais sobre VNet peering, veja [Tutorial sobre VNet Peering][VNet].

##<a name="test-setup"></a>Configuração de teste

O diagrama abaixo ilustra a configuração de teste.

[![1]][1]

A parte do centro da configuração do teste é a Vnet do Hub no 1 região do Azure. A Vnet do Hub está ligada a redes diferentes, da seguinte forma:

1.  Para falava Vnet através de Vnet peering. O Vnet spoke tem acesso remoto para ambos os gateways na Hub Vnet.
2.  A ramificação Vnet através de VPN de Site a Site. A conectividade utiliza eBGP para trocar as rotas.
3.  A 1 de localização de rede no local através do peering privado do ExpressRoute como o caminho principal e a conectividade VPN de Site a Site que o caminho de cópia de segurança. No restante deste documento, vamos referir-este circuito do ExpressRoute como ExpressRoute1. Por predefinição, os circuitos do ExpressRoute fornecem conectividade redundante para elevada disponibilidade. Em ExpressRoute1, com acesso do MSEE secundário a subinterface do roteador CE secundário está desativada. Isso é indicado com uma linha vermelha por cima da seta dupla-Linha no diagrama acima.
4.  A 2 de localização de rede no local através de outro peering privado do ExpressRoute. No restante deste documento, vamos referir-este circuito do ExpressRoute segundo como ExpressRoute2.
5.  ExpressRoute1 também se conecta a Vnet do Hub e a 1 de localização no local a uma Vnet remota em 2 de região do Azure.

## <a name="further-reading"></a>Leitura adicional

### <a name="using-expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>Utilizar a conectividade VPN de Site a Site e ExpressRoute em conjunto

#### <a name="site-to-site-vpn-over-expressroute"></a>VPN de site a Site através do ExpressRoute 

VPN site a Site pode ser configurada através do peering da Microsoft de ExpressRoute privada trocar dados entre a rede no local e as VNets do Azure com confidencialidade, anti-repetições, autenticidade e integridade. Para obter mais informações sobre como configurar IPSec de Site-Site VPN no modo de túnel através do peering da Microsoft do ExpressRoute, veja [VPN de Site a site através do peering de Microsoft ExpressRoute][S2S-Over-ExR]. 

A grande limitação de configuração de S2S VPN através do peering da Microsoft é a taxa de transferência. O débito através do túnel IPSec é limitado pela capacidade de GW de VPN. O débito de VPN GW é menor em comparação com a taxa de transferência do ExpressRoute. Em tais cenários, utilizar o túnel IPSec para proteger o tráfego elevado e um peering privado para o tráfego de todos os outros ajudaria a otimizar a utilização de largura de banda do ExpressRoute.

#### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>VPN de site a Site como um caminho de ativação pós-falha seguro para o ExpressRoute
ExpressRoute é oferecido como par de circuito redundantes para elevada disponibilidade. Pode configurar a conectividade do ExpressRoute com redundância geográfica em diferentes regiões do Azure. Também como Feito em nossa configuração de teste, dentro de uma determinada região do Azure, se pretender que um caminho de ativação pós-falha para a conectividade do ExpressRoute, pode fazer isso usando Site-Site VPN. Quando os mesmos prefixos são anunciados através do ExpressRoute e S2S VPN, o Azure prefere o ExpressRoute através de S2S VPN. Para evitar o encaminhamento assimétricas entre ExpressRoute e S2S VPN, no local a configuração de rede também deve reciprocate preferindo ExpressRoute através de conectividade de S2S VPN.

Para obter mais informações sobre como configurar ligações coexistentes do ExpressRoute e VPN Site a Site, consulte [coexistência Site a Site e ExpressRoute][ExR-S2S-CoEx].

### <a name="extending-backend-connectivity-to-spoke-vnets-and-branch-locations"></a>Estendendo a conectividade de back-end para spoke VNets e localizações de sucursais

#### <a name="spoke-vnet-connectivity-using-vnet-peering"></a>Conectividade de VNet com peering de VNet do spoke

Arquitetura de Vnet do hub-and-spoke é amplamente utilizada. O hub é uma rede virtual (VNet) no Azure que age como um ponto central de conectividade entre a VNets spoke e à sua rede no local. Os spokes são VNets que partilham com o hub e podem ser utilizados para isolar cargas de trabalho. O tráfego flui entre o datacenter no local e o hub através de uma ligação VPN ou ExpressRoute. Para obter mais informações sobre a arquitetura, consulte [arquitetura Hub-and-Spoke][Hub-n-Spoke]

Permite a VNet peering dentro de uma região VNets para utilizar gateways de VNet do hub (gateways VPN e ExpressRoute) para comunicar com redes remotas spoke.

#### <a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>Ramificar a conectividade de VNet através de VPN de Site a Site

Se pretender que o ramo Vnets (em regiões diferentes) e redes no local se comuniquem entre si através de uma vnet do hub, a solução nativa do Azure é a conectividade VPN de site a site através da VPN. Uma opção alternativa é usar uma NVA para encaminhamento no hub.

Para configurar gateways de VPN, veja [configurar o Gateway de VPN][VPN]. Para implementar a NVA altamente disponível, consulte [implementar NVA altamente disponível][Deploy-NVA].

## <a name="next-steps"></a>Passos Seguintes

Para os detalhes de configuração da topologia de teste, consulte [detalhes de configuração][Configuration].

Para análise de plano de controlo da configuração do teste e a compreender as vistas de VNet/VLAN diferente da topologia, consulte [análise de plano de controlo][Control-Analysis].

Para análise de plano de dados da configuração do teste e vistas de funcionalidades de monitorização de rede do Azure, veja [plano de dados de análise][Data-Analysis].

Para saber quantos circuitos do ExpressRoute pode ligar a um Gateway do ExpressRoute ou quantos Gateways do ExpressRoute pode ligar a um circuito do ExpressRoute ou para obter outros limites de dimensionamento do ExpressRoute, veja [FAQ do ExpressRoute][ExR-FAQ]



<!--Image References-->
[1]: ./media/backend-interoperability/TestSetup.png "a topologia de teste"

<!--Link References-->
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: https://docs.microsoft.com/azure/connectivty-interoperability-configuration
[Control-Analysis]:https://docs.microsoft.com/azure/connectivty-interoperability-control-plane
[Data-Analysis]: https://docs.microsoft.com/azure/connectivty-interoperability-data-plane
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha




