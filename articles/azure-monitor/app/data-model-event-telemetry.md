---
title: Modelo de dados de telemetria de informações de aplicação do Azure - telemetria de eventos | Documentos da Microsoft
description: Modelo de dados do Application Insights para a telemetria de evento
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: 7e69fb74226aaaad13b0a5410e0a46a152d73533
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/28/2018
ms.locfileid: "53812340"
---
# <a name="event-telemetry-application-insights-data-model"></a>Telemetria de evento: Modelo de dados do Application Insights

Pode criar itens de telemetria de evento (no [Application Insights](../../application-insights/app-insights-overview.md)) para representar um evento que ocorreu em seu aplicativo. Normalmente, é uma interação do usuário, como o clique de botão ou solicitar o Check-out. Também pode ser um evento de ciclo de vida do aplicativo, como atualização de configuração ou de inicialização. 

Semanticamente, eventos podem ou não podem ser correlacionados com pedidos. No entanto, se usados corretamente, é mais importante do que os pedidos ou rastreios de telemetria dos eventos. Eventos representam a telemetria de negócios e deve ser um assunto para separar, menos agressivos [amostragem](../../azure-monitor/app/api-filtering-sampling.md).

## <a name="name"></a>Nome

Nome do evento. Para permitir que o agrupamento correto e de métricas úteis, restringir o seu aplicativo para que ele gera um pequeno número de nomes de eventos separado. Por exemplo, não utilize um nome separado para cada instância gerado de um evento.

Comprimento máximo: 512 carateres

## <a name="custom-properties"></a>Propriedades personalizadas

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Medições personalizadas

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Passos Seguintes

- Ver [modelo de dados](data-model.md) para o modelo de tipos e dados do Application Insights.
- [Escrever telemetria de evento personalizado](../../azure-monitor/app/api-custom-events-metrics.md#trackevent)
- Confira [plataformas](../../application-insights/app-insights-platforms.md) suportada pelo Application Insights.
