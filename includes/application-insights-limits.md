---
title: incluir ficheiro
description: incluir ficheiro
services: application-insights
author: mrbullwinkle
ms.service: application-insights
ms.topic: include
ms.date: 06/21/2018
ms.author: mbullwin
ms.custom: include file
ms.openlocfilehash: 0dbc0834d5f5b87a39ae33d296af847cf4e368e3
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2019
ms.locfileid: "53977468"
---
Existem alguns limites no número de métricas e eventos por aplicação (ou seja, por chave de instrumentação). Os limites dependem do [plano de preços](https://azure.microsoft.com/pricing/details/application-insights/) que escolher.

| Recurso | Limite predefinido | Nota
| --- | --- | --- |
| Total de dados por dia | 100 GB | Pode reduzir os dados ao definir um limite. Se precisar de mais dados, pode aumentar o limite no portal, 1000 GB. Para as capacidades mais de 1000 GB, envie um email para AIDataCap@microsoft.com.
| Limitação | 32 K de eventos/segundo | O limite é mediso ao longo de um minuto.
| Retenção de dados | 90 dias | Este recurso é para [Pesquisa](../articles/azure-monitor/app/diagnostic-search.md), [Análise](../articles/azure-monitor/app/analytics.md) e [Explorador de métricas](../articles/application-insights/app-insights-metrics-explorer.md).
| [Teste de disponibilidade de vários passos](../articles/azure-monitor/app/monitor-web-app-availability.md#multi-step-web-tests) com retenção de resultados detalhados | 90 dias | Este recurso fornece resultados detalhados de cada passo.
| Tamanho máximo do evento | 64 K | 
| Comprimento do nome da propriedade e da métrica | 150 | Ver [escreva esquemas](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Comprimento da cadeia de valor da propriedade | 8,192 | Ver [escreva esquemas](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Comprimento da mensagem de exceção e de rastreio | 10 MIL | Ver [escreva esquemas](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| [Testes de disponibilidade](../articles/azure-monitor/app/monitor-web-app-availability.md) com contagem por aplicação | 100 |
| [Profiler](../articles/application-insights/app-insights-profiler.md) retenção de dados | 5 dias |
| [Profiler](../articles/application-insights/app-insights-profiler.md) dados enviados por dia | 10 GB |

Para mais informações, consulte [About pricing and quotas in Application Insights (Acerca de preços e quotas no Application Insights)](../articles/application-insights/app-insights-pricing.md).