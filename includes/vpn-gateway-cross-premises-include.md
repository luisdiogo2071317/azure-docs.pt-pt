---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/16/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 55ac7e055c972a9b18ef374ac8498b418c5d56af
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34307588"
---
|  | **Ponto a Site** | **Site a Site** | **ExpressRoute** |
| --- | --- | --- | --- |
| **Serviços Suportados pelo Azure** |Cloud Services e Virtual Machines |Cloud Services e Virtual Machines |[Lista de serviços](../articles/expressroute/expressroute-faqs.md#supported-services) |
| **Larguras de banda típicas** |Com base no SKU de gateway |Normalmente, < 1 Gbps agregado |50 Mbps, 100 Mbps, 200 Mbps, 500 Mbps, 1 Gbps, 2 Gbps, 5 Gbps, 10 Gbps |
| **Protocolos Suportados** |Secure Sockets Tunneling Protocol (SSTP) e IPsec |IPsec |Ligação direta por VLANs, tecnologias VPN do NSP (MPLS, VPLS,...) |
| **Encaminhamento** |RouteBased (dinâmico) |Suportamos o roteamento PolicyBased (encaminhamento estático) e RouteBased (VPN de encaminhamento dinâmico) |BGP |
| **Resiliência de ligação** |ativa-passiva |ativa-passiva ou ativa-ativa |ativa-ativa |
| **Caso de utilização típica** |Criar protótipos, cenários de desenvolvimento/teste/laboratório para serviços em nuvem e máquinas virtuais |Cenários de desenvolvimento/teste/laboratório e cargas de trabalho de produção de pequena escala para serviços em nuvem e máquinas virtuais |Acesso a todos os serviços do Azure (lista validada), Cargas de trabalho de classe empresarial fundamentais, Cópia de segurança, Macrodados, Azure como um site de DR |
| **SLA** |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |
| **Preços** |[Preços](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Preços](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[Preços](https://azure.microsoft.com/pricing/details/expressroute/) |
| **Documentação Técnica** |[Documentação do Gateway de VPN (VPN Gateway documentation)](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[Documentação do Gateway de VPN (VPN Gateway documentation)](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[Documentação do ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/) |
| **FAQ** |[FAQ do Gateway de VPN](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[FAQ do Gateway de VPN](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[FAQ do ExpressRoute](../articles/expressroute/expressroute-faqs.md) |
