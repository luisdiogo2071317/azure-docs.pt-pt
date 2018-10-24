---
title: 'Interoperabilidade do ExpressRoute, de VPN de Site a site e VNet Peering - análise do plano de dados: interoperabilidade de funcionalidades de conectividade de back-end do Azure | Documentos da Microsoft'
description: Esta página fornece a análise de plano de dados da configuração do teste que seja criada para analisar a interoperabilidade dos recursos do ExpressRoute, de VPN de Site a site e VNet Peering.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: c9f3824b1e0f44338696ba3c2e434d60eee3af8b
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947344"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---data-plane-analysis"></a>Interoperabilidade do ExpressRoute, de VPN de Site a site e VNet Peering - análise do plano de dados

Neste artigo, vamos percorrer a análise de plano de dados da configuração do teste. Para rever a configuração de teste, consulte a [configuração de teste][Setup]. Para rever os detalhes da configuração do programa de configuração de teste, consulte [configuração do programa de configuração de teste][Configuration]. Para rever a análise de plano de controlo da configuração do teste, consulte [análise de plano de controlo][Control-Analysis].

Análise de plano de dados examina o caminho percorrido por pacotes percorrendo o caminho entre uma rede local (LAN/VNet) para outra dentro de uma topologia. O caminho de dados entre duas redes locais pode não ser necessariamente simétrico. Portanto, neste artigo vamos analisar caminho de encaminhamento de uma rede local para outro em separado do caminho inverso.

##<a name="data-path-from-hub-vnet"></a>Caminho de dados a partir de VNet do Hub

###<a name="path-to-spoke-vnet"></a>Caminho para falava VNet

O VNet peering emula a funcionalidade de ponte de rede entre as duas VNets em modo de peering. O traceroute saída de uma VNet do Hub para uma VM na spoke VNet esteja listada abaixo:

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1     2 ms     1 ms     1 ms  10.11.30.4

    Trace complete.

O clip de ecrã seguinte é a exibição de gráfico de ligação de VNet do Hub e spoke VNet apresentado por observador de rede do Azure:


[![1]][1]

###<a name="path-to-branch-vnet"></a>Caminho para o ramo VNet

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1     1 ms     1 ms     1 ms  10.10.30.142
      2     *        *        *     Request timed out.
      3     2 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

O traceroute acima, o primeiro salto é o GW de VPN da VNet do Hub. O segundo salto é o GW de VPN da VNet ramo, cujo endereço IP não tenha sido anunciado dentro da VNet do Hub. O terceiro salto é a VM na VNet ramo.

O clip de ecrã seguinte é a vista de gráfico de ligação de VNet do Hub e a VNet de ramo apresentado por observador de rede do Azure:

[![2]][2]

Para a mesma ligação, o clip de ecrã seguinte é a exibição de grade apresentada por observador de rede do Azure:

[![3]][3]

###<a name="path-to-on-premises-location-1"></a>Caminho para 1 localização no local

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1     2 ms     2 ms     2 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     2 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

O traceroute acima, o primeiro salto é o ponto de extremidade de túnel de GW do ExpressRoute para MSEE. O segundo e o terceiro salto são respectivamente CE router e IPs de LAN de 1 de localização no local, estes endereços IP não são anunciados dentro da VNet do Hub. O quarto salto é na localização no local-1.


###<a name="path-to-on-premises-location-2"></a>Caminho para 2 de localização no local

    C:\Users\rb>tracert 10.1.31.10

    Tracing route to 10.1.31.10 over a maximum of 30 hops

      1    76 ms    75 ms    75 ms  10.10.30.134
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4    75 ms    75 ms    75 ms  10.1.31.10

    Trace complete.

O traceroute acima, o primeiro salto é o ponto de extremidade de túnel de GW do ExpressRoute para MSEE. O segundo e o terceiro salto são respectivamente CE router e IPs de LAN de 2 de localização no local, estes endereços IP não são anunciados dentro da VNet do Hub. O quarto salto é a VM a localização no local-2.

###<a name="path-to-remote-vnet"></a>Caminho para a VNet remota

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1     2 ms     2 ms     2 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3    69 ms    68 ms    69 ms  10.17.30.4

    Trace complete.

O traceroute acima, o primeiro salto é o ponto de extremidade de túnel de GW do ExpressRoute para MSEE. O segundo salto é a VNet remota GW IP. O intervalo IP do segundo salto não será anunciado dentro da VNet do Hub. O terceiro salto é a VM na VNet remota.

##<a name="data-path-from-spoke-vnet"></a>Caminho de dados a partir de Spoke VNet

Lembre-se de que a VNet Spoke partilham a vista de rede da VNet do Hub. Através de VNet peering, o VNet spoke utiliza a conectividade de gateway remoto de VNet do hub como se eles estão ligados diretamente para o VNet spoke.

###<a name="path-to-hub-vnet"></a>Caminho para a VNet do Hub

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.10.30.4

    Trace complete.

###<a name="path-to-branch-vnet"></a>Caminho para o ramo VNet

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1     1 ms    <1 ms    <1 ms  10.10.30.142
      2     *        *        *     Request timed out.
      3     3 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

O traceroute acima, o primeiro salto é o GW de VPN da VNet do Hub. O segundo salto é o GW de VPN da VNet ramo, cujo endereço IP não tenha sido anunciado dentro da VNet Hub/Spoke. O terceiro salto é a VM na VNet ramo.

###<a name="path-to-on-premises-location-1"></a>Caminho para 1 localização no local

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    24 ms     2 ms     3 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     3 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

O traceroute acima, o primeiro salto é o ponto final de túnel de ExpressRoute GW a VNet Hub para MSEE. O segundo e o terceiro salto são respectivamente CE router e IPs de LAN de 1 de localização no local, estes endereços IP não são anunciados dentro da VNet Hub/Spoke. O quarto salto é na localização no local-1.

###<a name="path-to-on-premises-location-2"></a>Caminho para 2 de localização no local

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    24 ms     2 ms     3 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     3 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

O traceroute acima, o primeiro salto é o ponto final de túnel de ExpressRoute GW a VNet Hub para MSEE. O segundo e o terceiro salto são respectivamente CE router e IPs de LAN de 2 de localização no local, estes endereços IP não são anunciados nas VNets Hub/Spoke. O quarto salto é a VM a localização no local-2.

###<a name="path-to-remote-vnet"></a>Caminho para a VNet remota

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1     2 ms     1 ms     1 ms  10.10.30.133
      2     *        *        *     Request timed out.
      3    71 ms    70 ms    70 ms  10.17.30.4

    Trace complete.

O traceroute acima, o primeiro salto é o ponto final de túnel de ExpressRoute GW a VNet Hub para MSEE. O segundo salto é a VNet remota GW IP. O intervalo IP do segundo salto não será anunciado dentro da VNet Hub/Spoke. O terceiro salto é a VM na VNet remota.

##<a name="data-path-from-branch-vnet"></a>Caminho de dados de ramo de VNet

###<a name="path-to-hub-vnet"></a>Caminho para a VNet do Hub

    C:\Windows\system32>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     4 ms     3 ms     3 ms  10.10.30.4

    Trace complete.

O traceroute acima, o primeiro salto é o GW de VPN da VNet ramo. O segundo salto é o GW de VPN da VNet do Hub, cujo endereço IP não tenha sido anunciado dentro da VNet remota. O terceiro salto é a VM na Hub VNet.

###<a name="path-to-spoke-vnet"></a>Caminho para falava VNet

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1     1 ms    <1 ms     1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     4 ms     3 ms     2 ms  10.11.30.4

    Trace complete.

O traceroute acima, o primeiro salto é o GW de VPN da VNet ramo. O segundo salto é o GW de VPN da VNet do Hub, cujos endereços IP não estão a ser anunciados dentro da VNet remota, e o terceiro salto é a VM na Spoke VNet.

###<a name="path-to-on-premises-location-1"></a>Caminho para 1 localização no local

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1     1 ms    <1 ms    <1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     3 ms     2 ms     2 ms  10.2.30.125
      4     *        *        *     Request timed out.
      5     3 ms     3 ms     3 ms  10.2.30.10

    Trace complete.

O traceroute acima, o primeiro salto é o GW de VPN da VNet ramo. O segundo salto é o GW de VPN da VNet do Hub, cujo endereço IP não tenha sido anunciado dentro da VNet remota. O terceiro salto é o ponto de terminação de túnel VPN no router CE primário. O quarto salto é um endereço IP interno do endereço de IP de LAN de 1 de localização no local que não tenha sido anunciado fora do router do CE. O quinto salto é o destino VM sobre a localização no local-1.

###<a name="path-to-on-premises-location-2-and-remote-vnet"></a>Caminho para a VNet no local de 2 de local e remoto

Conforme discutimos antes na análise de plano de controlo, o ramo VNet tem sem visibilidade para 2 de localização no local ou a VNet remota pela configuração de rede. Os seguintes resultados de ping confirmar o fato. 

    C:\Users\rb>ping 10.1.31.10

    Pinging 10.1.31.10 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.1.31.10:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

    C:\Users\rb>ping 10.17.30.4

    Pinging 10.17.30.4 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.17.30.4:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

##<a name="data-path-from-on-premises-location-1"></a>Caminho de dados do On-Premises localização-1

###<a name="path-to-hub-vnet"></a>Caminho para a VNet do Hub

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5     2 ms     2 ms     2 ms  10.10.30.4

    Trace complete.

No traceroute acima, os dois primeiros saltos fazem parte da rede no local. O terceiro salto é a interface MSEE primária voltada para o roteador CE. O quarto salto é ExpressRoute G/W de VNet do hub, cujo intervalo IP não está a ser anunciado à rede no local. O quinto salto é a VM de destino.

O observador de rede do Azure fornece apenas visão centrada em do Azure. Por conseguinte, para a exibição centrados em locais usamos Monitor de desempenho de rede (NPM) do Azure. NPM fornece agentes que podem ser instalados servidores na rede fora do Azure e fazer a análise de caminho de dados.

O clip de ecrã seguinte é a vista de topologia de conectividade de VMS de 1 de localização no local para a VM na VNet através do ExpressRoute do hub.

[![4]][4]

Lembre-se, a configuração de teste utiliza o Site-to-Site VPN como conectividade de cópia de segurança para o ExpressRoute entre 1 de localização no local e a VNet do Hub. Para testar o back-datapath, vamos induza uma falha de ligação de ExpressRoute entre o roteador CE da principal de 1 de localização no local e do MSEE correspondente ao desligar a interface de CE com acesso do MSEE.

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3     3 ms     2 ms     3 ms  10.10.30.4

    Trace complete.

O clip de ecrã seguinte é a vista de topologia de conectividade de VMS de 1 de localização no local para a VM na VNet do hub através de conectividade VPN de Site a Site quando a conectividade do ExpressRoute está inativa.

[![5]][5]

###<a name="path-to-spoke-vnet"></a>Caminho para falava VNet

Diga-nos trazer de volta a conectividade de principal do ExpressRoute para fazer a análise de datapath em direção à Spoke VNet.

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5     3 ms     2 ms     2 ms  10.11.30.4

    Trace complete.

Vamos abra a conectividade do ExpressRoute-1 primária para o resto da análise datapath.

###<a name="path-to-branch-vnet"></a>Caminho para o ramo VNet

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3     3 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

###<a name="path-to-on-premises-location-2"></a>Caminho para 2 de localização no local

Conforme discutimos antes na análise de plano de controle, a localização no local-1 tem sem visibilidade para localização no local-2, por que a configuração de rede. Os seguintes resultados de ping confirmar o fato. 

    C:\Users\rb>ping 10.1.31.10
    
    Pinging 10.1.31.10 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.1.31.10:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

###<a name="path-to-remote-vnet"></a>Caminho para a VNet remota

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2     2 ms     5 ms     7 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5    69 ms    70 ms    69 ms  10.17.30.4

    Trace complete.

##<a name="data-path-from-on-premises-location-2"></a>Caminho de dados do On-Premises localização-2

###<a name="path-to-hub-vnet"></a>Caminho para a VNet do Hub

    C:\Windows\system32>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.1.31.3
      2    <1 ms    <1 ms    <1 ms  192.168.31.4
      3    <1 ms    <1 ms    <1 ms  192.168.31.22
      4     *        *        *     Request timed out.
      5    75 ms    74 ms    74 ms  10.10.30.4

    Trace complete.

###<a name="path-to-spoke-vnet"></a>Caminho para falava VNet

    C:\Windows\system32>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops
      1    <1 ms    <1 ms     1 ms  10.1.31.3
      2    <1 ms    <1 ms    <1 ms  192.168.31.0
      3    <1 ms    <1 ms    <1 ms  192.168.31.18
      4     *        *        *     Request timed out.
      5    75 ms    74 ms    74 ms  10.11.30.4

    Trace complete.

###<a name="path-to-branch-vnet-on-premises-location-1-and-remote-vnet"></a>Caminho para a qual ramificar VNet, no local a VNet de 1 de local e remoto

Conforme discutimos antes na análise de plano de controle, a localização no local-1 tem sem visibilidade para o ramo VNet, no local localização-1 e a VNet remota pela configuração de rede. 

##<a name="data-path-from-remote-vnet"></a>Caminho de dados a partir de VNet remota

###<a name="path-to-hub-vnet"></a>Caminho para a VNet do Hub

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    65 ms    65 ms    65 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3    69 ms    68 ms    68 ms  10.10.30.4

    Trace complete.

###<a name="path-to-spoke-vnet"></a>Caminho para falava VNet

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1    67 ms    67 ms    67 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3    71 ms    69 ms    69 ms  10.11.30.4

    Trace complete.

### <a name="path-to-branch-vnet-and-on-premises-location-2"></a>Caminho para a VNet de ramo e 2 de localização no local

Conforme discutimos antes na análise de plano de controle, a VNet remota tem sem visibilidade para o ramo VNet e a localização no local-2, pela configuração de rede. 


### <a name="path-to-on-premises-location-1"></a>Caminho para 1 localização no local

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    67 ms    67 ms    67 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4    69 ms    69 ms    69 ms  10.2.30.10

    Trace complete.


## <a name="further-reading"></a>Leitura adicional

### <a name="using-expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>Utilizar a conectividade VPN de Site a Site e ExpressRoute em conjunto

####<a name="site-to-site-vpn-over-expressroute"></a>VPN de site a Site através do ExpressRoute

VPN site a Site pode ser configurada através do peering da Microsoft de ExpressRoute privada trocar dados entre a rede no local e as VNets do Azure com confidencialidade, anti-repetições, autenticidade e integridade. Para obter mais informações sobre como configurar IPSec de Site-Site VPN no modo de túnel através do peering da Microsoft do ExpressRoute, veja [VPN de Site a site através do peering de Microsoft ExpressRoute][S2S-Over-ExR]. 

A grande limitação de configuração de S2S VPN através do peering da Microsoft é a taxa de transferência. O débito através do túnel IPSec é limitado pela capacidade de GW de VPN. O débito de VPN GW é menor em comparação com a taxa de transferência do ExpressRoute. Em tais cenários, utilizar o túnel IPSec para proteger o tráfego elevado e um peering privado para o tráfego de todos os outros ajudaria a otimizar a utilização de largura de banda do ExpressRoute.

#### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>VPN de site a Site como um caminho de ativação pós-falha seguro para o ExpressRoute
ExpressRoute é oferecido como par de circuito redundantes para elevada disponibilidade. Pode configurar a conectividade do ExpressRoute com redundância geográfica em diferentes regiões do Azure. Também como Feito em nossa configuração de teste, dentro de uma determinada região do Azure, se pretender que um caminho de ativação pós-falha para a conectividade do ExpressRoute, pode fazer isso usando Site-Site VPN. Quando os mesmos prefixos são anunciados através do ExpressRoute e S2S VPN, o Azure prefere o ExpressRoute através de S2S VPN. Para evitar o encaminhamento assimétricas entre ExpressRoute e S2S VPN, no local a configuração de rede também deve reciprocate preferindo ExpressRoute através de conectividade de S2S VPN.

Para obter mais informações sobre como configurar ligações coexistentes do ExpressRoute e VPN Site a Site, consulte [coexistência Site a Site e ExpressRoute][ExR-S2S-CoEx].

### <a name="extending-backend-connectivity-to-spoke-vnets-and-branch-locations"></a>Estendendo a conectividade de back-end para falava VNets e o ramo de localizações

#### <a name="spoke-vnet-connectivity-using-vnet-peering"></a>Conectividade de VNet com peering de VNet do spoke

Arquitetura de Vnet do hub-and-spoke é amplamente utilizada. O hub é uma rede virtual (VNet) no Azure que age como um ponto central de conectividade entre a VNets spoke e à sua rede no local. Os spokes são VNets que partilham com o hub e podem ser utilizados para isolar cargas de trabalho. O tráfego flui entre o datacenter no local e o hub através de uma ligação VPN ou ExpressRoute. Para obter mais detalhes sobre a arquitetura, consulte [arquitetura Hub-and-Spoke][Hub-n-Spoke]

Permite a VNet peering dentro de uma região VNets para utilizar gateways de VNet do hub (gateways VPN e ExpressRoute) para comunicar com redes remotas spoke.

#### <a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>Ramificar a conectividade de VNet através de VPN de Site a Site

Se pretender que o ramo Vnets (em regiões diferentes) e redes no local se comuniquem entre si através de uma vnet do hub, a solução nativa do Azure é a conectividade VPN de site a site através da VPN. Uma opção alternativa é usar uma NVA para encaminhamento no hub.

Para configurar gateways de VPN, veja [configurar o Gateway de VPN][VPN]. Para implementar a NVA altamente disponível, consulte [implementar NVA altamente disponível][Deploy-NVA].

## <a name="next-steps"></a>Passos Seguintes

Para saber quantos circuitos do ExpressRoute pode ligar a um Gateway do ExpressRoute ou quantos Gateways do ExpressRoute pode ligar a um circuito do ExpressRoute ou para obter outros limites de dimensionamento do ExpressRoute, veja [FAQ do ExpressRoute][ExR-FAQ]


<!--Image References-->
[1]: ./media/backend-interoperability/HubVM-SpkVM.jpg "vista do observador de rede de conectividade entre a VNet do Hub e Spoke VNet"
[2]: ./media/backend-interoperability/HubVM-BranchVM.jpg "vista do observador de rede de conectividade da VNet do Hub para o ramo VNet"
[3]: ./media/backend-interoperability/HubVM-BranchVM-Grid.jpg "exibição de grade do observador de rede de conectividade da VNet do Hub para o ramo VNet"
[4]: ./media/backend-interoperability/Loc1-HubVM.jpg "exibição de Monitor de desempenho de rede de conectividade a partir de 1 de localização de VM para a VNet do Hub através do ExpressRoute 1"
[5]: ./media/backend-interoperability/Loc1-HubVM-S2S.jpg "exibição de Monitor de desempenho de rede de conectividade a partir de 1 de localização de VM para a VNet do Hub através de S2S VPN"

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




