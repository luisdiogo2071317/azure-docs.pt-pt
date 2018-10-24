---
title: 'Interoperabilidade do ExpressRoute, de VPN de Site a site e VNet Peering - detalhes de configuração: interoperabilidade de funcionalidades de conectividade de back-end do Azure | Documentos da Microsoft'
description: Esta página fornece os detalhes de configuração da configuração do teste que serve para analisar a interoperabilidade dos recursos do ExpressRoute, de VPN de Site a site e VNet Peering.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: d94900b764331c6fff0e0384e6edbebc88ac938b
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947341"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---test-configuration-details"></a>Interoperabilidade do ExpressRoute, de VPN de Site a site e VNet Peering - detalhes de configuração de teste

Neste artigo, vamos percorrer os detalhes de configuração da configuração do teste. Para rever a configuração de teste, consulte a [configuração de teste][Setup]. 

##<a name="spoke-vnet-connectivity-using-vnet-peering"></a>Conectividade de VNet com peering de VNet do spoke

Azure portal captura de ecrã seguinte mostra os detalhes do peering de VNet de spoke VNet. Para obter orientações passo a passo configurar o VNet peering entre duas redes virtuais, consulte [gerir o VNet Peering][VNet-Config]. Se pretender que a VNet Spoke para utilizar os gateways ligados à vnet do Hub, precisa verificar *utilizar gateways remotos*.

[![1]][1]

Azure portal captura de ecrã seguinte mostra os detalhes do peering de VNet de VNet do hub. Se pretender que a VNet do Hub para permitir que a usar seus gateways de VNet Spoke, precisa verificar *utilizar gateways remotos*.

[![2]][2]

##<a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>Ramificar a conectividade de VNet através de VPN de Site a Site

Conectividade VPN de site a Site entre o Hub e o ramo VNets está configurada com os gateways VPN. Por predefinição, os gateways do ExpressRoute e VPN estão configurados com o valor privado de ASN de 65515. Gateway de VPN permite-lhe alterar o valor ASN. A configuração de teste, conforme mostrado no Azure portal captura de ecrã seguinte, o valor ASN do gateway de VPN de VNet ramo é alterado para 65516 para ativar o encaminhamento de eBGP entre o Hub e o ramo VNets.


[![3]][3]


##<a name="location-1-on-premises-connectivity-using-expressroute-and-site-to-site-vpn"></a>Conectividade de 1 de localização no local com o ExpressRoute e VPN Site a Site

###<a name="expressroute1-configuration-details"></a>Detalhes de configuração de ExpressRoute1

A seguinte captura de ecrã portal mostra a configuração de circuito do ExpressRoute de 1 de região do Azure para Routers de CE 1 localização no local.

[![4]][4]

A seguinte captura de ecrã portal mostra a configuração da ligação entre o circuito de ExpressRoute1 e a VNet do Hub.

[![5]][5]

A configuração seguinte está a listagem do roteador CE primário (ASR1000 de Cisco routers são utilizados como routers CE na configuração de teste) relacionados com a configuração para conectividade de peering privada do ExpressRoute. Quando o Site-Site VPN e o circuito do ExpressRoute são configuradas em paralelo para ligar uma rede no local ao Azure; Azure prefere o circuito do ExpressRoute por predefinição. Para evitar o encaminhamento assimétricas, rede no local também deve prefere o ExpressRoute através de VPN de Site a Site para as rotas recebidas tanto através do ExpressRoute e VPN Site a Site. Isso é feito na seguinte configuração com o atributo de preferência local do BGP. 

    interface TenGigabitEthernet0/0/0.300
     description Customer 30 private peering to Azure
     encapsulation dot1Q 30 second-dot1q 300
     ip vrf forwarding 30
     ip address 192.168.30.17 255.255.255.252
    !
    interface TenGigabitEthernet1/0/0.30
     description Customer 30 to south bound LAN switch
     encapsulation dot1Q 30
     ip vrf forwarding 30
     ip address 192.168.30.0 255.255.255.254
     ip ospf network point-to-point
    !
    router ospf 30 vrf 30
     router-id 10.2.30.253
     redistribute bgp 65021 subnets route-map BGP2OSPF
     network 192.168.30.0 0.0.0.1 area 0.0.0.0
    default-information originate always
     default-metric 10
    !
    router bgp 65021
     !
     address-family ipv4 vrf 30
      network 10.2.30.0 mask 255.255.255.128
      neighbor 192.168.30.18 remote-as 12076
      neighbor 192.168.30.18 activate
      neighbor 192.168.30.18 next-hop-self
      neighbor 192.168.30.18 soft-reconfiguration inbound
      neighbor 192.168.30.18 route-map prefer-ER-over-VPN in
      neighbor 192.168.30.18 prefix-list Cust30_to_Private out
     exit-address-family
    !
    route-map prefer-ER-over-VPN permit 10
     set local-preference 200
    !
    ip prefix-list Cust30_to_Private seq 10 permit 10.2.30.0/25
    !

###<a name="site-to-site-vpn-configuration-details"></a>Detalhes de configuração de VPN de site a Site

Segue-se da lista da configuração de roteador principal do CE relacionados com a conectividade VPN de Site a Site:

    crypto ikev2 proposal Cust30-azure-proposal
     encryption aes-cbc-256 aes-cbc-128 3des
     integrity sha1
     group 2
    !
    crypto ikev2 policy Cust30-azure-policy
     match address local 66.198.12.106
     proposal Cust30-azure-proposal
    !
    crypto ikev2 keyring Cust30-azure-keyring
     peer azure
      address 52.168.162.84
      pre-shared-key local IamSecure123
      pre-shared-key remote IamSecure123
    !
    crypto ikev2 profile Cust30-azure-profile
     match identity remote address 52.168.162.84 255.255.255.255
     identity local address 66.198.12.106
     authentication local pre-share
     authentication remote pre-share
     keyring local Cust30-azure-keyring
    !
    crypto ipsec transform-set Cust30-azure-ipsec-proposal-set esp-aes 256 esp-sha-hmac
     mode tunnel
    !
    crypto ipsec profile Cust30-azure-ipsec-profile
     set transform-set Cust30-azure-ipsec-proposal-set
     set ikev2-profile Cust30-azure-profile
    !
    interface Loopback30
     ip address 66.198.12.106 255.255.255.255
    !
    interface Tunnel30
     ip vrf forwarding 30
     ip address 10.2.30.125 255.255.255.255
     tunnel source Loopback30
     tunnel mode ipsec ipv4
     tunnel destination 52.168.162.84
     tunnel protection ipsec profile Cust30-azure-ipsec-profile
    !
    router bgp 65021
     !
     address-family ipv4 vrf 30
      network 10.2.30.0 mask 255.255.255.128
      neighbor 10.10.30.254 remote-as 65515
      neighbor 10.10.30.254 ebgp-multihop 5
      neighbor 10.10.30.254 update-source Tunnel30
      neighbor 10.10.30.254 activate
      neighbor 10.10.30.254 soft-reconfiguration inbound
     exit-address-family
    !
    ip route vrf 30 10.10.30.254 255.255.255.255 Tunnel30

##<a name="location-2-on-premises-connectivity-using-expressroute"></a>Localização 2 conectividade no local através do ExpressRoute

Um circuito do ExpressRoute segunda, na proximidade mais de perto a 2 de localização no local, liga-se a 2 de localização no local à VNet do hub. A seguinte captura de ecrã portal mostra a segunda configuração de ExpressRoute.

[![6]][6]

A seguinte captura de ecrã portal mostra a configuração da ligação entre o segundo circuito do ExpressRoute e a VNet do Hub.

[![7]][7]

O ExpressRoute1 liga-se a Vnet do Hub e a 1 de localização no local a uma Vnet remota numa região diferente do Azure.

[![8]][8]

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

Para análise de plano de controlo da configuração do teste e a compreender as vistas de VNet/VLAN diferente da topologia, consulte [análise de plano de controlo][Control-Analysis].

Para análise de plano de dados da configuração do teste e vistas de funcionalidades de monitorização de rede do Azure, veja [plano de dados de análise][Data-Analysis].

Para saber quantos circuitos do ExpressRoute pode ligar a um Gateway do ExpressRoute ou quantos Gateways do ExpressRoute pode ligar a um circuito do ExpressRoute ou para obter outros limites de dimensionamento do ExpressRoute, veja [FAQ do ExpressRoute][ExR-FAQ]


<!--Image References-->
[1]: ./media/backend-interoperability/SpokeVNet_peering.png "falava VNet peering de VNet"
[2]: ./media/backend-interoperability/HubVNet-peering.png "da VNet do hub o VNet peering"
[3]: ./media/backend-interoperability/BranchVNet-VPNGW.png "configuração de GW de VPN de VNet do ramo"
[4]: ./media/backend-interoperability/ExR1.png "ExpressRoute1 configuração"
[5]: ./media/backend-interoperability/ExR1-Hub-Connection.png "configuração da ligação de ExpressRoute1 para Hub VNet ExR GW"
[6]: ./media/backend-interoperability/ExR2.png "ExpressRoute2 configuração"
[7]: ./media/backend-interoperability/ExR2-Hub-Connection.png "configuração da ligação de ExpressRoute2 para Hub VNet ExR GW"
[8]: ./media/backend-interoperability/ExR2-Remote-Connection.png "configuração da ligação de ExpressRoute2 para remoto GW de ExR de VNet"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
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




