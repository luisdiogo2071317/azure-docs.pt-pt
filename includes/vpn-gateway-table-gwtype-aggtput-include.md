---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/29/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 03af5efcd4a37203a82db503f8bc602b33de734d
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227426"
---
|**SKU**   | **S2S/VNet para VNet<br>Túneis** | **Ligações SSTP <br>P2S** | **Ligações IKEv2 <br>P2S** | **Referência de<br>Débito de Agregação** |
|---       | ---        | ---       | ---            | ---       |
|**VpnGw1**| Um máximo de 30*   | Um máximo de 128  | Um máximo de 250       | 650 Mbps  |
|**VpnGw2**| Um máximo de 30*   | Um máximo de 128  | Um máximo de 500       | 1 Gbps    |
|**VpnGw3**| Um máximo de 30*   | Um máximo de 128  | Um máximo de 1000      | 1,25 Gbps |
|**Básica** | Um máximo de 10    | Um máximo de 128  | Não suportado  | 100 Mbps  | 

(*) Utilize a [WAN Virtual](../articles/virtual-wan/virtual-wan-about.md) se precisar de mais de 30 túneis S2S VPN.

* A Referência de Débito Agregado para um Gateway de VPN é uma combinação de S2S + P2S. **Se tiver muitas ligações P2S, isso pode afetar negativamente uma ligação S2S devido às limitações de débito.** A Referência de Débito de Agregação baseia-se em medidas de vários túneis agregados através de um único gateway. Não é uma taxa de transferência garantida devido a condições de tráfego de Internet e aos comportamentos da sua aplicação.

* Esses limites de ligação são separados. Por exemplo, pode ter 128 ligações SSTP e também 250 ligações IKEv2 num SKU VpnGw1.

* As informações sobre os preços estão disponíveis na página [Preços](https://azure.microsoft.com/pricing/details/vpn-gateway).

* Pode encontrar informações sobre SLA (contrato de nível de serviço) na página do [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/).

* Os VpnGw1, VpnGw2 e VpnGw3 são suportados para gateways de VPN com o modelo de implementação do gestor de recursos.
