---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b698dd03473dd3cb708c47c6554869eebba48bf9
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51597642"
---
|**SKU**   | **S2S/VNet para VNet<br>Túneis** | **Ligações SSTP <br>P2S** | **Ligações IKEv2 <br>P2S** | **Referência de<br>Débito de Agregação** | **BGP** |
|---       | ---        | ---       | ---            | ---       | --- |
|**Básica** | Um máximo de 10    | Um máximo de 128  | Não suportado  | 100 Mbps  | Não suportado|
|**VpnGw1**| Um máximo de 30*   | Um máximo de 128  | Um máximo de 250       | 650 Mbps  | Suportado |
|**VpnGw2**| Um máximo de 30*   | Um máximo de 128  | Um máximo de 500       | 1 Gbps    | Suportado |
|**VpnGw3**| Um máximo de 30*   | Um máximo de 128  | Um máximo de 1000      | 1,25 Gbps | Suportado |


(*) Utilize a [WAN Virtual](../articles/virtual-wan/virtual-wan-about.md) se precisar de mais de 30 túneis S2S VPN.

* A Referência de Débito Agregado para um Gateway de VPN é uma combinação de S2S + P2S. **Se tiver muitas ligações P2S, isso pode afetar negativamente uma ligação S2S devido às limitações de débito.** A Referência de Débito de Agregação baseia-se em medidas de vários túneis agregados através de um único gateway. Não é uma taxa de transferência garantida devido a condições de tráfego de Internet e aos comportamentos da sua aplicação.

* Esses limites de ligação são separados. Por exemplo, pode ter 128 ligações SSTP e também 250 ligações IKEv2 num SKU VpnGw1.

* As informações sobre os preços estão disponíveis na página [Preços](https://azure.microsoft.com/pricing/details/vpn-gateway).

* Pode encontrar informações sobre SLA (contrato de nível de serviço) na página do [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/).

* Os VpnGw1, VpnGw2 e VpnGw3 são suportados para gateways de VPN com o modelo de implementação do gestor de recursos.
