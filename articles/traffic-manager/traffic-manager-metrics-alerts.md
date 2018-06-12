---
title: Métricas e alertas no Traffic Manager do Azure | Microsoft Docs
description: Este artigo descreve as métricas disponíveis para o Gestor de tráfego no Azure.
services: traffic-manager
documentationcenter: ''
author: KumudD
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/11/2018
ms.author: kumud
ms.openlocfilehash: 424782be2d814df6d598591198b5005fb494d3da
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35304205"
---
# <a name="traffic-manager-metrics-and-alerts"></a>Métricas de Gestor de tráfego e alertas

Gestor de tráfego fornece-lhe que inclui vários métodos de encaminhamento e as opções de monitorização de ponto final de balanceamento de carga baseado no DNS. Este artigo descreve as métricas e alertas associados que estão disponíveis para os clientes. 

## <a name="metrics-available-in-traffic-manager"></a>Métricas disponíveis no Gestor de tráfego 

Gestor de tráfego fornece as métricas seguintes numa base por perfil que pode ser utilizada por clientes para compreender a respetiva utilização do Gestor de tráfego e o estado dos respetivos pontos finais sob esse perfil.  

### <a name="queries-by-endpoint-returned"></a>Consultas pelo ponto final devolvido
Utilize [esta métrica](../monitoring-and-diagnostics/monitoring-supported-metrics.md) para ver o número de consultas que foram processados por um perfil de Gestor de tráfego através de um período de tempo especificado. Também pode ver as mesmas informações granularidade endpoint nível que ajuda a compreender a forma como os demasiadas vezes um ponto final foi devolvido nas respostas de consulta do Gestor de tráfego.

No exemplo abaixo, a figura 1 mostra todas as respostas de consulta que foram devolvidas pelo perfil do Gestor de tráfego. 

  
![Gestor de tráfego métricas - vista agregado de todas as consultas](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-queries-aggregate-view.png)

*Figura 1: Agregar vista com todas as consultas*
  
Figura 2 mostra as mesmas informações, no entanto, é dividida por pontos finais. Como resultado, pode ver o volume de respostas de consulta em que foi devolvido um ponto final específico.

![Métricas de Gestor de tráfego - dividir a vista do volume de consulta por ponto final](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-query-volume-per-endpoint.png)

*Figura 2: Dividir vista com o volume de consulta mostrado por ponto final devolvido*

## <a name="endpoint-status-by-endpoint"></a>Estado de ponto final pelo ponto final
Utilize [esta métrica](../monitoring-and-diagnostics/monitoring-supported-metrics.md) para compreender o estado de funcionamento dos pontos finais no perfil. Demora dois valores:
 - utilizar **1** , se o ponto final está a funcionar.
 - utilizar **0** se o ponto final está inativo.

Esta métrica pode ser apresentada como um valor de agregação que representa o estado de todas as métricas (figura 3), em alternativa, pode ser dividido (consulte a figura 4) para mostrar o estado dos pontos finais específicos. No caso de anterior, se o nível de agregação é selecionado como **médio**, o valor de esta métrica é a média aritmética do Estado de todos os pontos finais. Por exemplo, se um perfil tem dois pontos finais e apenas um é bom estado de funcionamento, esta métrica terá um valor de **0.50** conforme mostrado na figura 3. 


![Gestor de tráfego métricas - composta vista de estado de ponto final](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-composite-view.png)

*Figura 3: Vista composta de métrica de estado de ponto final – agregação "Médio" selecionada*


![Métricas de Gestor de tráfego - dividir a vista de estado de ponto final](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-split-view.png)

*Figura 4: Ver de divisão de métricas de estado do ponto final*

Pode consumir estas métricas através de [serviço Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md)do portal, [REST API](https://docs.microsoft.com/rest/api/monitor/), [CLI do Azure](https://docs.microsoft.com/cli/azure/monitor), e [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights), ou através da secção de métricas de experiência do portal do Gestor de tráfego.

## <a name="alerts-on-traffic-manager-metrics"></a>Alertas nas métricas do Gestor de tráfego
Além de processamento e apresentar as métricas do Gestor de tráfego, o Monitor do Azure permite aos clientes configurar e receber alertas associados a estas métricas. Pode escolher o condições precisam de ser cumpridos nestas métricas para um alerta a ocorrer, frequência dessas condições têm de ser monitorizados e como os alertas devem ser enviados para si. Para obter mais informações, consulte [documentação de alertas do Monitor de Azure](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [serviço de monitorização do Azure](../monitoring-and-diagnostics/monitoring-supported-metrics.md)
- Saiba como [criar um novo gráfico utilizando o Monitor do Azure](../monitoring-and-diagnostics/monitoring-metric-charts.md#how-do-i-create-a-new-chart)