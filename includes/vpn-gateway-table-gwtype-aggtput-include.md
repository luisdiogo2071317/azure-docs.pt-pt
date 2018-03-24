---
title: incluir ficheiro
description: incluir ficheiro
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c9457e51858d4a073d8baffdd435c8100d95d566
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
|**SKU**   | **S2S/VNet para VNet<br>Túneis** | **P2S<br>Ligações** | **Referência de<br>Débito de Agregação** |
|---       | ---                             | ---                    | ---                         |
|**VpnGw1**| Um máximo de 30                         | Um máximo de 128               | 650 Mbps                    |
|**VpnGw2**| Um máximo de 30                         | Um máximo de 128               | 1 Gbps                      |
|**VpnGw3**| Um máximo de 30                         | Um máximo de 128               | 1,25 Gbps                   |
|**Básica** | Um máximo de 10                         | Um máximo de 128               | 100 Mbps                    | 
|          |                                 |                        |                             | 

- A Referência de Débito de Agregação baseia-se em medidas de vários túneis agregados através de um único gateway. Não é uma taxa de transferência garantida devido a condições de tráfego de Internet e aos comportamentos da sua aplicação.

- As informações sobre os preços estão disponíveis na página [Preços](https://azure.microsoft.com/pricing/details/vpn-gateway).

- Pode encontrar informações sobre SLA (contrato de nível de serviço) na página do [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/).

- VpnGw1, VpnGw2 e VpnGw3 são suportadas para gateways de VPN utilizando apenas o modelo de implementação do Resource Manager.