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
ms.openlocfilehash: 6e0d2b881e00acc6b1fccd3196187309fc36cba2
ms.sourcegitcommit: 8659efc885be98f42edae0b516ca576e38940c97
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/13/2018
ms.locfileid: "35568148"
---
#### <a name="expressroute-limits"></a>Limites do ExpressRoute
Os seguintes limites aplicam-se aos recursos do ExpressRoute por subscrição.

| Recurso | Limite Predefinido |
| --- | --- |
| Circuitos ExpressRoute por subscrição |10 |
| Circuitos ExpressRoute por região por subscrição para ARM |10 |
| Número máximo de rotas para o peering privado do Azure com o ExpressRoute standard |4,000 |
| Número máximo de rotas para o peering privado do Azure com o suplemento ExpressRoute premium |10,000 |
| Número máximo de rotas para o peering público do Azure com o ExpressRoute standard |200 |
| Número máximo de rotas para o peering público do Azure com o suplemento ExpressRoute premium |200 |
| Número máximo de rotas para peering da Microsoft do Azure com o ExpressRoute standard |200 |
| Número máximo de rotas para peering com o suplemento ExpressRoute premium da Microsoft Azure |200 |
| Número máximo de circuitos do ExpressRoute ligado à mesma rede virtual em diferentes localizações de peering |4 |
| Número de ligações de rede virtual permitidos por circuito do ExpressRoute |Consulte a tabela abaixo |

#### <a name="number-of-virtual-networks-per-expressroute-circuit"></a>Número de redes virtuais por circuito do ExpressRoute
| **Tamanho do circuito** | **Número de ligações VNet para standard** | **Número de ligações VNet com o suplemento Premium** |
| --- | --- | --- |
| 50 Mbps |10 |20 |
| 100 Mbps |10 |25 |
| 200 Mbps |10 |25 |
| 500 Mbps |10 |40 |
| 1 Gbps |10 |50 |
| 2 Gbps |10 |60 |
| 5 Gbps |10 |75 |
| 10 Gbps |10 |100 |

