---
title: Monitorizar aplicações - serviço de aplicações do Azure | Documentos da Microsoft
description: Saiba como monitorizar aplicações no serviço de aplicações do Azure com o portal do Azure.
services: app-service
documentationcenter: ''
author: btardif
manager: erikre
editor: ''
ms.assetid: d273da4e-07de-48e0-b99d-4020d84a425e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: byvinyal
ms.custom: seodec18
ms.openlocfilehash: d9f25b7a10b7a50663198120a895220b02818d7b
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2019
ms.locfileid: "53994951"
---
# <a name="monitor-apps-in-azure-app-service"></a>Monitorizar aplicações no serviço de aplicações do Azure
[Serviço de aplicações do Azure](https://go.microsoft.com/fwlink/?LinkId=529714) fornece a funcionalidade interna de monitorização para aplicações web, móveis back-ends e aplicações API no [portal do Azure](https://portal.azure.com).

No portal do Azure, pode rever *quotas* e *métricas* para uma aplicação, reveja o plano do serviço de aplicações e configurar automaticamente a *alertas* e *dimensionamento* que se baseiam nas métricas.

## <a name="understand-quotas"></a>Compreender as quotas

Aplicações que estão alojadas no serviço de aplicações estão sujeitas a determinados limites para os recursos podem utilizar. Os limites são definidos pelo plano de serviço de aplicações que está associada a aplicação.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Se a aplicação estiver alojada num *gratuito* ou *partilhado* plano, os limites nos recursos que pode utilizar a aplicação são definidos por quotas.

Se a aplicação estiver alojada num *básica*, *padrão*, ou *Premium* plano, os limites nos recursos que eles podem usar são definidos pelo *tamanho* ( Pequeno, médio, grande) e *contagem de instâncias* (1, 2, 3 e assim por diante) do plano do serviço de aplicações.

As quotas para aplicações gratuito ou partilhado são:

| Quota | Descrição |
| --- | --- |
| **CPU (curto)** | A quantidade de CPU permitida para esta aplicação num intervalo de 5 minutos. Esta quota repõe a cada cinco minutos. |
| **CPU (dia)** | A quantidade total de CPU permitida para esta aplicação num dia. Esta quota repõe a cada 24 horas à meia-noite UTC. |
| **Memória** | A quantidade total de memória permitida para esta aplicação. |
| **Largura de banda** | A quantidade total de largura de banda de saída permitida para esta aplicação num dia. Esta quota repõe a cada 24 horas à meia-noite UTC. |
| **Sistema de ficheiros** | A quantidade total de armazenamento permitida. |

A quota apenas aplicável a aplicações que estão alojadas numa *básica*, *padrão*, e *Premium* planos é o sistema de ficheiros.

Para obter mais informações sobre as quotas específicas, limites e recursos disponíveis para as várias SKUs de serviço de aplicação, consulte [limites de serviço de subscrição do Azure](../azure-subscription-service-limits.md#app-service-limits).

### <a name="quota-enforcement"></a>Imposição de quota

Se uma aplicação exceder os *CPU (curto)*, *CPU (dia)*, ou *largura de banda* quota, a aplicação está parado até que repõe a quota. Durante este período, todos os pedidos recebidos resultam num erro HTTP 403.

![mensagem de erro 403][http403]

Se for excedida a quota de memória da aplicação, o aplicativo é reiniciado.

Se for excedida a quota do sistema de ficheiros, qualquer escrever operação falhar. Falhas de operação incluem quaisquer escritas nos registos de escrita.

Pode aumentar ou remover quotas a partir da sua aplicação ao atualizar o seu plano do serviço de aplicações.

## <a name="understand-metrics"></a>Compreender as métricas

Métricas fornecem informações sobre a aplicação ou o serviço de aplicações de comportamento do plano.

Para uma aplicação, as métricas disponíveis são:

| Métrica | Descrição |
| --- | --- |
| **Tempo médio de resposta** | O tempo médio despendido para a aplicação atender a solicitações, em milissegundos. |
| **Conjunto de trabalho de memória média** | A quantidade média de memória utilizada pela aplicação, no mebibytes (MiB). |
| **Tempo de CPU** | A quantidade de CPU consumida pela aplicação, em segundos. Para obter mais informações sobre esta métrica, consulte [percentagem de CPU do vs de tempo de CPU](#cpu-time-vs-cpu-percentage). |
| **Dados em** | A quantidade de largura de banda de entrada consumida pela aplicação, no MiB. |
| **Saída de dados** | A quantidade de largura de banda de saída consumida pela aplicação, no MiB. |
| **2xx de HTTP** | Contagem de pedidos, resultando num código de estado HTTP ≥ 200 mas < 300. |
| **Http 3xx** | Contagem de pedidos, resultando num código de estado HTTP ≥ 300 mas < 400. |
| **HTTP 401** | Contagem de pedidos, resultando em código de estado HTTP 401. |
| **HTTP 403** | Contagem de pedidos, resultando em código de estado HTTP 403. |
| **HTTP 404** | Contagem de pedidos, resultando em código de estado de HTTP 404. |
| **HTTP 406** | Contagem de pedidos, resultando em código de estado HTTP 406. |
| **Http 4xx** | Contagem de pedidos, resultando num código de estado HTTP ≥ 400 mas < 500. |
| **Erros de servidor HTTP** | Contagem de pedidos, resultando num código de estado HTTP ≥ 500 mas < 600. |
| **Conjunto de trabalho de memória** | A quantidade atual de memória utilizada pela aplicação, no MiB. |
| **Pedidos** | O número total de pedidos, independentemente do seu código de estado HTTP resultante. |

Para um plano de serviço de aplicações, as métricas disponíveis são:

> [!NOTE]
> Métricas do plano de serviço de aplicações estão disponíveis apenas para planos *básica*, *padrão*, e *Premium* escalões.
> 

| Métrica | Descrição |
| --- | --- |
| **Percentagem de CPU** | A CPU média utilizada em todas as instâncias do plano. |
| **Percentagem de memória** | Média de memória utilizada em todas as instâncias do plano. |
| **Dados em** | A média entrada largura de banda utilizada em todas as instâncias do plano. |
| **Saída de dados** | A média de largura de banda utilizada em todas as instâncias do plano de saída. |
| **Comprimento da fila de disco** | O número médio de leitura e pedidos que foram colocados em fila de escrita no armazenamento. Um comprimento de fila de disco elevados é uma indicação de uma aplicação que pode ser mais lento devido a e/s de disco excessiva. |
| **Comprimento da fila de HTTP** | O número médio de pedidos HTTP que tinha sentar-se na fila antes de a ser concluído. Um comprimento de fila de HTTP alto ou crescente é um sintoma de um plano com muita carga. |

### <a name="cpu-time-vs-cpu-percentage"></a>Percentagem de CPU do vs de tempo de CPU
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Existem duas métricas que refletem a utilização da CPU:

**Tempo de CPU**: Útil para as aplicações alojadas no gratuito ou partilhado planos, como uma das suas quotas é definida em minutos de CPU utilizados pela aplicação.

**Percentagem de CPU**: Útil para aplicações alojadas nos planos básico, Standard e Premium, uma vez que podem ser aumentados horizontalmente. Percentagem de CPU é uma boa indicação de que o uso geral em todas as instâncias.

## <a name="metrics-granularity-and-retention-policy"></a>Política de retenção e a granularidade de métricas
As métricas para uma aplicação e o plano do serviço de aplicações são registadas e agregadas pelo serviço, com as políticas de retenção e nas duas granularidades seguintes:

* **Minuto** métricas de granularidade são retidas durante 30 horas.
* **Hora** métricas de granularidade são retidas durante 30 dias.
* **Dia** métricas de granularidade são retidas durante 30 dias.

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Monitorização de quotas e as métricas no portal do Azure
Para rever o estado dos vários quotas e métricas que afetam uma aplicação, vá para o [portal do Azure](https://portal.azure.com).

![Gráfico de quotas no portal do Azure][quotas]

Para encontrar as quotas, selecione **configurações** > **Quotas**. O gráfico, pode rever: 
1. O nome da quota.
1. O intervalo de reposição.
1. O limite atual.
1. O valor atual.

![Gráfico de métricas no portal do Azure][metrics] pode aceder a métricas diretamente a partir de **recurso** página. Para personalizar o gráfico: 
1. Selecione o gráfico.
1. Selecione **editar gráfico**.
1. Editar a **intervalo de tempo**.
1. Editar a **tipo de gráfico**.
1. Edite as métricas que pretende apresentar.  

Para saber mais sobre métricas, veja [monitorizar as métricas de serviço](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Alertas e o dimensionamento automático
As métricas para uma aplicação ou um plano do serviço de aplicações podem ser conectadas a alertas. Para obter mais informações, consulte [Receber notificações de alerta](../monitoring-and-diagnostics/insights-alerts-portal.md).

Aplicações de serviço de aplicações alojadas nesse autoscale de suporte de planos básico, Standard ou Premium do serviço de aplicações. Com o dimensionamento automático, pode configurar regras que monitoram as métricas de plano de serviço de aplicações. Regras podem aumentar ou diminuir a contagem de instâncias, que pode fornecer recursos adicionais conforme necessário. As regras também podem ajudar a poupar dinheiro quando a aplicação é aprovisionada excessiva. 

Para obter mais informações sobre o dimensionamento automático, consulte [como dimensionar](../monitoring-and-diagnostics/insights-how-to-scale.md) e [melhores práticas para o dimensionamento automático do Azure Monitor](../azure-monitor/platform/autoscale-best-practices.md).

[fzilla]:https://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:https://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
