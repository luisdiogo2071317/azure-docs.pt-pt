---
title: incluir ficheiro
description: incluir ficheiro
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 06/12/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c681e5f68299c03685dd7722f8dc671e49026f78
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43305087"
---
#### <a name="expressroute-limits"></a>Limites do ExpressRoute
Os limites seguintes aplicam-se aos recursos do ExpressRoute por subscrição.

| Recurso | Limite Predefinido |
| --- | --- |
| Circuitos do ExpressRoute por subscrição |10 |
| Circuitos do ExpressRoute por região por subscrição (Azure Resource Manager) |10 |
| Número máximo de rotas para peering privado do Azure com o ExpressRoute standard |4,000 |
| Número máximo de rotas para peering privado do Azure com o suplemento ExpressRoute premium |10,000 |
| Número máximo de rotas para peering da Microsoft do Azure com o ExpressRoute standard |200 |
| Número máximo de rotas para o Azure peering da Microsoft com o suplemento ExpressRoute premium |200 |
| Número máximo de circuitos do ExpressRoute ligado à mesma rede virtual em diferentes localizações de peering |4 |
| Número de ligações de rede virtual permitidos por circuito do ExpressRoute |consulte a tabela abaixo |

#### <a name="number-of-virtual-networks-per-expressroute-circuit"></a>Número de redes virtuais por circuito do ExpressRoute
| **Tamanho do circuito** | **Número de ligações de VNet para o standard** | **Número de ligações VNet com o suplemento Premium** |
| --- | --- | --- |
| 50 Mbps |10 |20 |
| 100 Mbps |10 |25 |
| 200 Mbps |10 |25 |
| 500 Mbps |10 |40 |
| 1 Gbps |10 |50 |
| 2 Gbps |10 |60 |
| 5 Gbps |10 |75 |
| 10 Gbps |10 |100 |

