---
title: Modelo de dados de telemetria de Insights de aplicação do Azure - eventos de telemetria | Microsoft Docs
description: Modelo de dados do Application Insights para telemetria de eventos
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: mbullwin; sergkanz
ms.openlocfilehash: 4f6b5c35b65b4aff2dbe8dafbb2eb07d75c2382a
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="event-telemetry-application-insights-data-model"></a>Telemetria de evento: modelo de dados do Application Insights

Pode criar itens de telemetria de eventos (no [Application Insights](app-insights-overview.md)) para representar um evento que ocorreram na sua aplicação. Normalmente, é uma interação do utilizador, tais como o botão clique ou ordenar a finalização da compra. Também pode ser um evento de ciclo de vida de aplicação, como inicialização ou a configuração de atualização. 

Ponto de vista semântico, eventos podem ou não podem ser correlacionados com pedidos. No entanto, se utilizados corretamente, a telemetria de eventos é mais importante do que os pedidos ou rastreios. Eventos representam a telemetria de negócio e deve ser um assunto para separar, menos agressiva [amostragem](app-insights-api-filtering-sampling.md).

## <a name="name"></a>Nome

Nome do evento. Para permitir que o agrupamento correto e métricas úteis, restringir a aplicação para que gera um pequeno número de nomes do evento separado. Por exemplo, não utilize um nome separado para cada instância gerado um evento.

Comprimento máximo: 512 carateres

## <a name="custom-properties"></a>Propriedades personalizadas

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Medidas personalizadas

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Passos Seguintes

- Consulte [modelo de dados](application-insights-data-model.md) para o modelo de tipos e os dados do Application Insights.
- [Escrever telemetria de evento personalizado](app-insights-api-custom-events-metrics.md#trackevent)
- Veja [plataformas](app-insights-platforms.md) suportado pelo Application Insights.
