---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/02/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4cbbb64489acf23c1248e35269e1441dd2a6878e
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39513842"
---
|**SKU**   | **S2S/VNet para VNet<br>Túneis** | **P2S<br>Ligações** | **Referência de<br>Débito de Agregação** |
|---       | ---                             | ---                    | ---                         |
|**VpnGw1**| Um máximo de 30*                         | Um máximo de 128\*\*             | 650 Mbps                    |
|**VpnGw2**| Um máximo de 30*                         | Um máximo de 128\*\*             | 1 Gbps                      |
|**VpnGw3**| Um máximo de 30*                         | Um máximo de 128\*\*             | 1,25 Gbps                   |
|**Básica** | Um máximo de 10                         | Um máximo de 128               | 100 Mbps                    | 

* (*) Utilize a [WAN Virtual](../articles/virtual-wan/virtual-wan-about.md) se precisar de mais de 30 túneis S2S VPN.

* (**) Entre em contacto com o suporte se forem precisas ligações adicionais. Isto aplica-se apenas ao IKEv2, o número de ligações de SSTP não pode ser aumentado.

* A Referência de Débito de Agregação baseia-se em medidas de vários túneis agregados através de um único gateway. Não é uma taxa de transferência garantida devido a condições de tráfego de Internet e aos comportamentos da sua aplicação.

* As informações sobre os preços estão disponíveis na página [Preços](https://azure.microsoft.com/pricing/details/vpn-gateway).

* Pode encontrar informações sobre SLA (contrato de nível de serviço) na página do [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/).

* Os VpnGw1, VpnGw2 e VpnGw3 são suportados para gateways de VPN com o modelo de implementação do gestor de recursos.