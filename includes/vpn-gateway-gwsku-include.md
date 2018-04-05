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
ms.openlocfilehash: 05dc8ae48a9164e4f7118d378ab0eb7c30a4249e
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
Quando cria um gateway de rede virtual, tem de especificar o SKU de gateway que pretende utilizar. Selecione o SKU que atende as suas necessidades com base nos tipos de cargas de trabalho, taxas de transferência, funcionalidades e SLA.

###  <a name="benchmark"></a>SKUs de Gateway por túnel, ligação e débito

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

###  <a name="feature"></a>SKUs de gateway por conjunto de funcionalidades

O novo gateway VPN SKUs simplificam os conjuntos de funcionalidades oferecidos nos gateways:

| **SKU**| **Funcionalidades**|
| ---    | ---         |
|**Básico** (*)   | **VPN baseado na rota**: 10 túneis com P2S; sem autenticação RADIUS para P2S; nenhum IKEv2 para P2S<br>**VPN baseada em políticas**: (IKEv1): 1 túnel; nenhum P2S|
| **VpnGw1, VpnGw2, and VpnGw3** | **VPN baseado na rota**: até 30 túneis (*), P2S, o BGP, a política de IPsec/IKE ativo-ativo, personalizada, coexistência ExpressRoute/VPN |
|        |             |

( * ) Pode configurar "PolicyBasedTrafficSelectors" para se ligar a um gateway de VPN baseada em rota (VpnGw1, VpnGw2, VpnGw3) para vários dispositivos com firewall baseada na política no local. Consulte [Gateways de ligação VPN para vários dispositivos VPN com base na política local com o PowerShell](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) para obter detalhes.

(**) O SKU básico é considerado um SKU de legado. O SKU básico tem algumas limitações de funcionalidade. Não é possível redimensionar um gateway que utiliza um SKU básico para um dos SKUs de gateway nova, em vez disso, tem de alterar para um novo SKU, que envolve a eliminar e recriar o gateway de VPN.

###  <a name="workloads"></a>SKUs de gateway - vs de produção. Cargas de trabalho Dev-Test

Devido às diferenças no SLAs e conjuntos de funcionalidades, recomendamos os seguintes SKUs de produção vs programador teste:

| **Carga de trabalho**                       | **SKU**               |
| ---                                | ---                    |
| **Produção, cargas de trabalho críticas** | VpnGw1, VpnGw2, VpnGw3 |
| **Dev-test ou prova de conceito**   | Básico (*)                 |
|                                    |                        |

(**) O SKU básico é considerado um SKU de legado e tem limitações de funcionalidade. Certifique-se de que a funcionalidade que precisa é suportada antes de utilizar o SKU básico.

Se estiver a utilizar os SKUs antigos (Legado), as recomendações de SKU de produção são padrão e HighPerformance. Para informações e instruções para SKUs antigos, consulte [SKUs de Gateway (Legado)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).