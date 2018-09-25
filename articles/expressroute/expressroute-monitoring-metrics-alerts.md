---
title: A monitorização de ExpressRoute do Azure, métricas e alertas | Documentos da Microsoft
description: Esta página fornece informações sobre a monitorização de ExpressRoute
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: 73bacebe6edc5b1d1273a10a0db7397766c45b0c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971648"
---
# <a name="expressroute-monitoring-metrics-and-alerts"></a>Monitorização, métricas e alertas do ExpressRoute

 Este artigo ajuda-o a compreender a monitorização de ExpressRoute, métricas e alertas. Monitor do Azure é um meio único para todas as métricas, alertas, os registos de diagnóstico em todas as do Azure.

## <a name="circuit-metrics"></a>Métricas de circuito

Para navegar até **métricas**, clique na página de ExpressRoute para o circuito que pretende monitorizar. Sob **monitorização** pode exibir o **métricas**.

![métricas de circuito](./media/expressroute-monitoring-metrics-alerts/ermetricspeering.jpg)

## <a name="metrics-per-peering"></a>Métricas por peering

Pode ver as métricas para privado, público e peering em bits por segundo da Microsoft.

![métricas por peering](./media/expressroute-monitoring-metrics-alerts/erpeeringmetrics.jpg) 

## <a name="expressroute-gateway-connections-in-bitsseconds"></a>Ligações de gateway do ExpressRoute em bits/segundos

![ligações de gateway](./media/expressroute-monitoring-metrics-alerts/erconnections.jpg ) 

## <a name="alerts-for-expressroute-gateway-connections"></a>Alertas para as ligações de gateway do ExpressRoute

1. Para configurar alertas, navegue até **do Azure Monitor**, em seguida, clique em **alertas**.

  ![alerts](./media/expressroute-monitoring-metrics-alerts/eralertshowto.jpg)

2. Clique em **+ selecionar destino** e selecione o recurso de ligação de gateway do ExpressRoute.

  ![destino]( ./media/expressroute-monitoring-metrics-alerts/alerthowto2.jpg)
3. Defina os detalhes do alerta.

  ![grupo de ação](./media/expressroute-monitoring-metrics-alerts/alerthowto3.jpg)


4. Definir e adicionar o grupo de ação.

  ![Adicionar grupo de ação](./media/expressroute-monitoring-metrics-alerts/actiongroup.png)

## <a name="alerts-based-on-each-peering"></a>Alertas com base em cada peering

 ![o que](./media/expressroute-monitoring-metrics-alerts/basedpeering.jpg)

## <a name="configure-alerts-for-activity-logs-on-circuits"></a>Configurar alertas para os registos de atividades em circuitos

Na **critérios de alerta**, pode selecionar **registo de atividades** para o tipo de sinal e selecione o sinal.

  ![outro](./media/expressroute-monitoring-metrics-alerts/alertshowto6activitylog.jpg)

## <a name="next-steps"></a>Passos Seguintes
* Configurar a ligação do ExpressRoute.
  
  * [Criar e modificar um circuito](expressroute-howto-circuit-arm.md)
  * [Criar e modificar a configuração de peering](expressroute-howto-routing-arm.md)
  * [Ligar uma VNet a um circuito do ExpressRoute](expressroute-howto-linkvnet-arm.md)
