---
title: 'Planear e conceber para ligações entre locais: Gateway de VPN do Azure | Documentos da Microsoft'
description: Saiba mais sobre o planeamento de Gateway de VPN e de design para em vários locais, híbridas e ligações de VNet a VNet
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 07/27/2017
ms.author: yushwang
ms.openlocfilehash: 7802061ba09a30ca34ed3804ace846118c5edb9b
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56235375"
---
# <a name="planning-and-design-for-vpn-gateway"></a>Planear e conceber para o Gateway de VPN

Planear e estruturar seu em vários locais e configurações de VNet a VNet podem ser simples ou complexo, dependendo das suas necessidades de rede. Este artigo orienta-o por meio de considerações de planejamento e design básicas.

## <a name="planning"></a>Planejamento

### <a name="compare"></a>Opções de conectividade em vários locais

Se pretender ligar em segurança os seus sites no local a uma rede virtual, tem três formas diferentes de fazer isso: Site a Site, ponto a Site e ExpressRoute. Compare as ligações em vários locais diferentes que estão disponíveis. A opção que escolher pode depender de diversas considerações, tais como:

* Que tipo de débito requer a sua solução?
* Pretende comunicar através da Internet pública via VPN segura ou através de uma ligação privada?
* Tem um endereço IP público disponível para utilização?
* Está a planear utilizar um dispositivo VPN? Se assim for, é compatível?
* Está a ligar apenas alguns computadores ou pretende uma ligação persistente para o seu site?
* Que tipo de gateway de VPN é necessário para a solução que pretende criar?
* SKU de gateway que deve usar?

### <a name="planningtable"></a>Tabela de planeamento

A tabela seguinte pode ajudá-lo a decidir a melhor opção de conectividade para a sua solução.

[!INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]

### <a name="gwsku"></a>SKUs de Gateway

[!INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

### <a name="wf"></a>Workflow

A lista seguinte descreve o fluxo de trabalho comuns de conectividade de cloud:

1. Conceber e planear a topologia de conectividade e liste os espaços de endereço para todas as redes que pretende ligar.
2. Crie uma rede virtual do Azure. 
3. Crie um gateway VPN para a rede virtual.
4. Criar e configurar conexões com redes no local ou outras redes virtuais (conforme necessário).
5. Criar e configurar uma ligação ponto a Site para o seu gateway de VPN do Azure (conforme necessário).

## <a name="design"></a>Design
### <a name="topologies"></a>Topologias da ligação

Começar por observar os diagramas na [sobre o Gateway de VPN](vpn-gateway-about-vpngateways.md) artigo. O artigo contém diagramas básicos, os modelos de implementação para cada topologia e as ferramentas de implantação disponíveis que pode utilizar para implementar a sua configuração.

### <a name="designbasics"></a>Noções básicas de design

As secções seguintes abordam as noções básicas de gateway VPN. 

#### <a name="servicelimits"></a>Limites de serviços de rede

Percorra as tabelas para ver [limites dos serviços de rede](../azure-subscription-service-limits.md#networking-limits). Dos limites listados podem afetar o seu design.

#### <a name="subnets"></a>Sobre sub-redes

Quando estiver a criar ligações, tem de considerar os intervalos de sub-rede. Não pode ter a sobreposição de intervalos de endereços da sub-rede. Uma sub-rede sobreposta é quando uma rede virtual ou localização no local contém o mesmo espaço de endereços que contém a localização de outra. Isso significa que precisa seus engenheiros de rede para as suas redes no local para extrair um intervalo que pode utilizar para o IP do Azure/sub-redes de espaço de endereçamento. Precisa de espaço de endereço que não está a ser utilizado na rede no local.

Evitar sub-redes sobrepostas também é importante quando estiver a trabalhar com ligações de VNet a VNet. Se sobreponham as sub-redes e um endereço IP existe de envio e de destino VNets, as ligações de VNet a VNet falhar. Azure não é possível encaminhar os dados para a outra VNet, porque o endereço de destino é parte da VNet em modo de envio.

Gateways de VPN requerem numa sub-rede específica denominada uma sub-rede de gateway. Para funcionarem corretamente, todas as sub-redes do gateway têm de ter o nome GatewaySubnet. Certifique-se de que não atribua um nome à sub-rede do gateway um nome diferente e não implemente VMs ou qualquer outra coisa na sub-rede de gateway. Ver [sub-redes de Gateway](vpn-gateway-about-vpn-gateway-settings.md#gwsub).

#### <a name="local"></a>Sobre os gateways de rede local

O gateway de rede local refere-se normalmente à sua localização no local. No modelo de implementação clássica, o gateway de rede local é referido como um Site de rede Local. Quando configurar um gateway de rede local, atribua um nome, especifique o endereço IP público do dispositivo VPN no local e, especificar os prefixos de endereços que estão na localização no local. O Azure analisa os prefixos de endereço de destino para tráfego de rede, consulta a configuração especificada para o gateway de rede local e encaminha os pacotes em conformidade. Pode modificar os prefixos de endereço, conforme necessário. Para obter mais informações, consulte [gateways de rede Local](vpn-gateway-about-vpn-gateway-settings.md#lng).

#### <a name="gwtype"></a>Sobre os tipos de gateway

Selecionar o tipo de gateway correto para a sua topologia é fundamental. Se selecionar o tipo errado, o gateway não funcionará corretamente. O tipo de gateway especifica o modo como o gateway em si se liga e é uma configuração obrigatória no modelo de implementação Resource Manager.

Os tipos de gateway são:

* Vpn
* ExpressRoute

#### <a name="connectiontype"></a>Sobre os tipos de ligação

Cada configuração exige um tipo específico de ligação. Os tipos de ligação são:

* IPsec
* Vnet2Vnet
* ExpressRoute
* VPNClient

#### <a name="vpntype"></a>Sobre os tipos de VPN

Cada configuração exige um tipo específico de VPN. Se está a combinar duas configurações, por exemplo, a criar uma ligação Site a Site e uma ligação Ponto a Site para a mesma VNet, tem de utilizar um tipo de VPN que cumpra ambos os requisitos de ligação.

[!INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

As tabelas seguintes mostram o tipo de VPN conforme ela é mapeada para cada configuração de ligação. Certifique-se de que o tipo VPN para o seu gateway corresponde da configuração que pretende criar. 

[!INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)]

### <a name="devices"></a>Dispositivos VPN para ligações Site a Site

Para configurar uma ligação Site a Site, independentemente do modelo de implementação, terá dos seguintes itens:

* Um dispositivo VPN que é compatível com gateways de VPN do Azure
* Um endereço IPv4 IP destinado ao público que não está protegido por um NAT

Tem de ter experiência de configuração do dispositivo VPN, ou ter alguém que pode configurar o dispositivo para.

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

### <a name="forcedtunnel"></a>Considere o encaminhamento de túnel forçado

Na maioria das configurações, pode configurar o túnel forçado. O túnel forçado permite-lhe redirecionamento ou "forçar" todo o tráfego vinculado à Internet de volta para a sua localização no local através de um túnel VPN de Site a Site para inspeção e auditoria. Este é um requisito de críticas de segurança de TI de empresas a maioria das políticas. 

Sem o túnel forçado, tráfego da Internet das suas VMs no Azure atravessará sempre da infraestrutura de rede do Azure diretamente saída à Internet, sem a opção para que possa inspecionar ou o tráfego de auditoria. Acesso não autorizado da Internet pode potencialmente conduzir a divulgação de informações ou outros tipos de falhas de segurança.

Uma ligação de túnel forçada pode ser configurada em ambos os modelos de implementação e utilizando ferramentas diferentes. Para obter mais informações, consulte [configurar imposição do túnel](vpn-gateway-forced-tunneling-rm.md).

**Diagrama de encapsulamento de forçado**

![Diagrama de encapsulamento de forçado o Gateway de VPN do Azure](./media/vpn-gateway-plan-design/forced-tunneling-diagram.png)

## <a name="next-steps"></a>Passos Seguintes

Consulte a [FAQ do Gateway de VPN](vpn-gateway-vpn-faq.md) e [sobre o Gateway de VPN](vpn-gateway-about-vpngateways.md) artigos para obter mais informações para ajudá-lo com seu design.

Para obter mais informações sobre as definições do gateway específico, consulte [sobre o VPN Gateway Settings](vpn-gateway-about-vpn-gateway-settings.md).
