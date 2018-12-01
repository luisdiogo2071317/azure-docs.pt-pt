---
title: Modelo de dados de telemetria de informações de aplicação do Azure - telemetria de rastreio | Documentos da Microsoft
description: Modelo de dados do Application Insights para a telemetria de rastreio
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
ms.openlocfilehash: f87b2d1bd105a9a9b8eabb3f2f6686c36f71e6df
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2018
ms.locfileid: "52721736"
---
# <a name="trace-telemetry-application-insights-data-model"></a>Telemetria de rastreio: modelo de dados do Application Insights

Telemetria de rastreio (no [Application Insights](app-insights-overview.md)) representa `printf` declarações de rastreio são pesquisados por texto de estilo. `Log4Net`, `NLog`, e outras entradas de ficheiro de registo baseados em texto são convertidas em instâncias deste tipo. O rastreio não tem medidas como uma extensibilidade.

## <a name="message"></a>Mensagem

Mensagem de rastreio.

Comprimento máximo: 32 768 carateres

## <a name="severity-level"></a>Nível de gravidade

Nível de gravidade de rastreio. O valor pode ser `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="custom-properties"></a>Propriedades personalizadas

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Passos Seguintes

- [Explore os logs de rastreamento do .NET no Application Insights](app-insights-asp-net-trace-logs.md).
- [Explore Java registos de rastreio no Application Insights](app-insights-java-trace-logs.md).
- Ver [modelo de dados](application-insights-data-model.md) para o modelo de tipos e dados do Application Insights.
- [Escrever telemetria de rastreio personalizada](app-insights-api-custom-events-metrics.md#tracktrace)
- Confira [plataformas](app-insights-platforms.md) suportada pelo Application Insights.
