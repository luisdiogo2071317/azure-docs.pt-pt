---
title: Métricas e alertas no Gestor de tráfego do Azure | Documentos da Microsoft
description: Este artigo descreve as métricas disponíveis para o Gestor de tráfego no Azure.
services: traffic-manager
documentationcenter: ''
author: KumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/11/2018
ms.author: kumud
ms.openlocfilehash: 2be5718dd95eff837c5a016b5254be0992eb8f9a
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50138383"
---
# <a name="traffic-manager-metrics-and-alerts"></a>O Gestor de tráfego métricas e alertas

Gestor de tráfego oferece-lhe que inclui vários métodos de encaminhamento e opções de monitorização do ponto final de balanceamento de carga com base no DNS. Este artigo descreve as métricas e alertas associados que estão disponíveis para clientes. 

## <a name="metrics-available-in-traffic-manager"></a>Métricas disponíveis no Gestor de tráfego 

O Gestor de tráfego fornece as métricas seguintes numa base por perfil que pode ser consumida pelos clientes para compreender a utilização do Gestor de tráfego e o estado dos seus pontos finais sob esse perfil.  

### <a name="queries-by-endpoint-returned"></a>Consultas por ponto de extremidade devolvido
Uso [esta métrica](../monitoring-and-diagnostics/monitoring-supported-metrics.md) para ver o número de consultas que foram processados por um perfil do Gestor de tráfego num determinado período. Também pode ver as mesmas informações numa granularidade de nível de ponto final que ajudam a compreender como o número de vezes que um ponto final foi devolvido nas respostas de consulta do Gestor de tráfego.

No exemplo abaixo, a figura 1 exibe todas as respostas de consulta que foram devolvidas pelo perfil do Gestor de tráfego. 

  
![Métricas de Gestor de tráfego – visualização agregada de todas as consultas](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-queries-aggregate-view.png)

*Figura 1: Agregar vista com todas as consultas*
  
Figura 2 apresenta as mesmas informações, no entanto, está dividida pelos pontos de extremidade. Como resultado, pode ver o volume de respostas de consulta em que foi devolvido um ponto de extremidade específico.

![Métricas de Gestor de tráfego – dividir o modo de exibição do volume de consulta por ponto final](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-query-volume-per-endpoint.png)

*Figura 2: O modo de divisão mostrado por ponto final devolvida um volume consulta*

## <a name="endpoint-status-by-endpoint"></a>Estado do ponto final pelo ponto final
Uso [esta métrica](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftnetworktrafficmanagerprofiles) para compreender o estado de funcionamento dos pontos finais no perfil. Demora dois valores:
 - utilizar **1** , se o ponto final está ativo.
 - utilizar **0** se o ponto final está inativo.

Esta métrica pode ser apresentada como um valor agregado que representa o estado de todas as métricas (figura 3), em alternativa, este pode ser dividido (consulte a figura 4) para mostrar o estado dos pontos de extremidade específicos. No caso anterior, se o nível de agregação está selecionado como **média**, o valor de esta métrica é a média aritmética do Estado de todos os pontos finais. Por exemplo, se um perfil tem dois pontos de extremidade e apenas uma está em bom estada, esta métrica terá um valor de **0,50** conforme mostrado na figura 3. 


![Métricas de Gestor de tráfego – visualização composta do Estado do ponto final](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-composite-view.png)

*Figura 3: Exibição composta de métrica de estado do ponto de extremidade – agregação "Média" selecionada*


![Métricas de Gestor de tráfego – dividir a vista de estado do ponto final](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-split-view.png)

*Figura 4: O modo de divisão de métricas de estado do ponto final*

Pode consumir estas métricas através de [serviço do Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md)do portal, [REST API](https://docs.microsoft.com/rest/api/monitor/), [da CLI do Azure](https://docs.microsoft.com/cli/azure/monitor), e [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights), ou a seção de métricas de experiência do portal do Gestor de tráfego.

## <a name="alerts-on-traffic-manager-metrics"></a>Alertas em métricas de Gestor de tráfego
Além de processamento e exibição de métricas do Gestor de tráfego, o Azure Monitor permite aos clientes configurar e receber alertas associados a estas métricas. Pode escolher o que as condições que devem ser atendidas nestas métricas para um alerta a ocorrer, a frequência com que essas condições necessitam a monitorizar e como os alertas devem ser enviados para si. Para obter mais informações, consulte [documentação de alertas do Azure Monitor](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [serviço do Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md)
- Saiba como [crie um novo gráfico através do Azure Monitor](../monitoring-and-diagnostics/monitoring-metric-charts.md#how-do-i-create-a-new-chart)
