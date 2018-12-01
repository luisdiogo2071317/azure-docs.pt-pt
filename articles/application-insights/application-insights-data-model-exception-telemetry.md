---
title: Modelo de dados de telemetria de informações de aplicação do Azure - telemetria de exceção | Documentos da Microsoft
description: Modelo de dados do Application Insights para a telemetria de exceção
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
ms.openlocfilehash: 672eb1100731d636ae7c34f16d371c4f93b5716c
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2018
ms.locfileid: "52721821"
---
# <a name="exception-telemetry-application-insights-data-model"></a>Telemetria de exceção: modelo de dados do Application Insights

Na [Application Insights](app-insights-overview.md), uma instância de exceção representa uma exceção de processados ou não processada que ocorreu durante a execução da aplicação monitorizada.

## <a name="problem-id"></a>ID do Problema

Identificador de onde a exceção foi acionada no código. Utilizado para exceções de agrupamento. Normalmente, uma combinação de tipo de exceção e uma função a partir da pilha de chamadas.

Comprimento máximo: 1024 carateres

## <a name="severity-level"></a>Nível de gravidade

Nível de gravidade de rastreio. O valor pode ser `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="exception-details"></a>Detalhes da exceção

(Para ser estendido)

## <a name="custom-properties"></a>Propriedades personalizadas

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Medições personalizadas

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Passos Seguintes

- Ver [modelo de dados](application-insights-data-model.md) para o modelo de tipos e dados do Application Insights.
- Saiba como [diagnosticar exceções nas aplicações web com o Application Insights](app-insights-asp-net-exceptions.md).
- Confira [plataformas](app-insights-platforms.md) suportada pelo Application Insights.
