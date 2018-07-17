---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 07/10/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8c3f727c6154a0364f151d22000d2684c361676a
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39037215"
---
### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpngateway"></a>Qual é a diferença entre um gateway de rede virtual do Azure (Gateway de VPN) e um vpngateway de WAN Virtual do Azure?

A WAN Virtual fornece conectividade Site a Site em grande escala e foi criada para débito, escalabilidade e facilidade de utilização. Os dispositivos de ramo CPE fazem o aprovisionamento automático e ligam numa WAN Virtual do Azure. Estes dispositivos estão disponíveis num ecossistema crescente de parceiros de SD-WAN e de VPN.

### <a name="which-device-providers-virtual-wan-partners-are-supported-at-launch-time"></a>Que fornecedores de dispositivos (parceiros de WAN Virtual) são suportados na altura do lançamento? 

De momento, a Citrix e a Riverbed suportam a experiência de WAN Virtual totalmente automatizada. Mais parceiros serão acrescentados nos próximos meses, incluindo: Nokia Nuage, Palo Alto e Checkpoint. Para obter mais informações, veja [Parceiros de WAN Virtual](https://aka.ms/virtualwan).

### <a name="am-i-required-to-use-a-preferred-partner-device"></a>Sou obrigado a utilizar um dispositivo de parceiro preferencial?

Não. Pode utilizar qualquer dispositivo compatível com VPN que cumpra os requisitos da Pré-visualização para suporte de IPsec de IKEv2.

### <a name="how-do-virtual-wan-partners-automate-connectivity-with-azure-virtual-wan"></a>Como é que os parceiros de WAN Virtual automatizam a conectividade com a WAN Virtual do Azure?

As soluções de conectividade definida pelo software gerem, normalmente, os respetivos dispositivos de ramo com um controlador ou um centro de aprovisionamento de dispositivos. O controlador pode utilizar APIs do Azure para automatizar a conectividade à WAN Virtual do Azure. Para obter mais informações sobre como isto funciona, veja [automatização de parceiros da WAN Virtual](../articles/virtual-wan/virtual-wan-configure-automation-providers.md).

### <a name="does-virtual-wan-change-any-existing-connectivity-features"></a>A WAN Virtual altera funcionalidades de conectividade existentes?   

As funcionalidades de conectividade do Azure existentes não sofrem quaisquer alterações.

### <a name="are-there-new-resource-manager-resources-available-for-virtual-wan"></a>Existem novos recursos do Resource Manager disponíveis para a WAN Virtual?
  
Sim, a WAN Virtual apresenta novos recursos do Resource Manager. Para obter mais informações, veja a [Descrição Geral](https://go.microsoft.com/fwlink/p/?LinkId=2004389).

### <a name="are-there-any-special-requirements-to-join-the-preview"></a>Existem requisitos especiais para aderir à Pré-visualização? 

Antes de poder configurar uma WAN Virtual do Azure, tem, primeiro, de inscrever a sua subscrição na Pré-visualização. Para se inscrever, envie um e-mail para <azurevirtualwan@microsoft.com> com o ID da subscrição. Receberá um e-mail assim que a sua subscrição tiver sido inscrita. Até receber a confirmação de que a sua subscrição foi inscrita, não poderá trabalhar com a WAN Virtual no portal.

Considerações:

* A Pré-visualização está limitada apenas às regiões públicas do Azure.
* Cada hub virtual suporta até 100 ligações. Cada ligação consiste de dois túneis que estão numa configuração ativo-ativo. Os túneis terminam num vpngateway de Hub Virtual do Azure.
* Considere utilizar esta Pré-visualização se:
  * Quiser implementar a largura de banda agregada inferior a 1 Gbps por hub virtual.
  * Tiver um dispositivo VPN que suporte a configuração baseada em rotas e conectividade IPsec de IKEv2.
  * Quiser explorar com SD-WAN e com dispositivos de ramo dos parceiros de lançamento (Riverbed e Citrix).<br>ou<br>Quiser configurar a conectividade ramo a ramo e ramo a Azure que inclua a gestão da configuração do seu dispositivo no local. (Auto-configuração)

### <a name="is-global-vnet-peering-supported-with-azure-virtual-wan"></a>O peering de VNet Global é suportado com a WAN Virtual do Azure? 

 Não.

### <a name="can-spoke-vnets-connected-to-a-virtual-hub-communicate-with-each-other"></a>As VNets spoke ligadas a um hub virtual podem comunicar entre si?

Sim. Pode fazer diretamente o peering de VNet entre spokes que estejam ligados a um hub virtual. Para obter mais informações, veja [Peering de Rede Virtual](../articles/virtual-network/virtual-network-peering-overview.md).

### <a name="can-i-deploy-and-use-my-favorite-network-virtual-appliance-in-an-nva-vnet-with-azure-virtual-wan"></a>Posso implementar e utilizar a minha aplicação virtual de rede favorita (numa VNet de NVA) com a WAN Virtual do Azure?

Sim, pode ligar a sua VNet de aplicação virtual de rede (NVA) favorita à WAN Virtual do Azure, mas precisará de capacidades de encaminhamento no hub que estará em Disponibilidade Geral. Todos os spokes ligados à VNet de NVA também têm de estar ligados ao hub virtual. 

### <a name="can-an-nva-vnet-have-a-virtual-network-gateway"></a>Uma VNet de NVA pode ter um gateway de rede virtual?

Não. A VNet de NVA não pode ter um gateway de rede virtual se estiver ligada ao hub virtual. 

### <a name="is-there-support-for-bgp"></a>Existe suporte para BGP?

Sim, o BGP é suportado. Para garantir que as rotas de uma VNet de NVA são anunciadas adequadamente, os spokes têm de desativar o BGP caso estejam ligados a uma VNet de NVA que, por sua vez, esteja ligada a um hub virtual. Além disso, ligue as VNets spoke ao hub virtual.

### <a name="can-i-direct-traffic-using-udr-in-the-virtual-hub"></a>Posso direcionar o tráfego com um UDR no hub virtual?

O UDR e a funcionalidade de encaminhamento estarão disponíveis aquando da Disponibilidade Geral.

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>Existem informações de licenciamento ou de preços da WAN Virtual?
 
Não existem custos adicionais durante a Pré-visualização. Os [preços de saída e de VPN do Azure](https://azure.microsoft.com/pricing/details/vpn-gateway/) atuais permanecem em vigor durante a Pré-visualização.