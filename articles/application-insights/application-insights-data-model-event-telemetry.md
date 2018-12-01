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
ms.openlocfilehash: 5883a8eb4a550c61251fa20331dbd433328c5d59
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2018
ms.locfileid: "52724048"
---
# <a name="event-telemetry-application-insights-data-model"></a>Telemetria de evento: modelo de dados do Application Insights

Pode criar itens de telemetria de evento (no [Application Insights](app-insights-overview.md)) para representar um evento que ocorreu em seu aplicativo. Normalmente, é uma interação do usuário, como o clique de botão ou solicitar o Check-out. Também pode ser um evento de ciclo de vida do aplicativo, como atualização de configuração ou de inicialização. 

Semanticamente, eventos podem ou não podem ser correlacionados com pedidos. No entanto, se usados corretamente, é mais importante do que os pedidos ou rastreios de telemetria dos eventos. Eventos representam a telemetria de negócios e deve ser um assunto para separar, menos agressivos [amostragem](app-insights-api-filtering-sampling.md).

## <a name="name"></a>Nome

Nome do evento. Para permitir que o agrupamento correto e de métricas úteis, restringir o seu aplicativo para que ele gera um pequeno número de nomes de eventos separado. Por exemplo, não utilize um nome separado para cada instância gerado de um evento.

Comprimento máximo: 512 carateres

## <a name="custom-properties"></a>Propriedades personalizadas

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Medições personalizadas

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Passos Seguintes

- Ver [modelo de dados](application-insights-data-model.md) para o modelo de tipos e dados do Application Insights.
- [Escrever telemetria de evento personalizado](app-insights-api-custom-events-metrics.md#trackevent)
- Confira [plataformas](app-insights-platforms.md) suportada pelo Application Insights.
