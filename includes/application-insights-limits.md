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
ms.openlocfilehash: 90de751f416ca611f3c674232c224199ad7af717
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36310172"
---
Existem alguns limites no número de métricas e eventos por aplicação (ou seja, por chave de instrumentação). Os limites dependem do [plano de preços](https://azure.microsoft.com/pricing/details/application-insights/) que escolher.

| Recurso | Limite predefinido | Nota
| --- | --- | --- |
| Total de dados por dia | 100 GB | Pode reduzir os dados ao definir um limite. Se precisar de mais dados, pode aumentar o limite no portal, 1000 GB. Para as capacidades maiores do que 1000 GB, envie um e-mail para AIDataCap@microsoft.com.
| Limitação | Eventos de 32 K/segundo | O limite é mediso ao longo de um minuto.
| Retenção de dados | 90 dias | Este recurso é para [Pesquisa](../articles/application-insights/app-insights-diagnostic-search.md), [Análise](../articles/application-insights/app-insights-analytics.md) e [Explorador de métricas](../articles/application-insights/app-insights-metrics-explorer.md).
| [Teste de disponibilidade de vários passos](../articles/application-insights/app-insights-monitor-web-app-availability.md#multi-step-web-tests) com retenção de resultados detalhados | 90 dias | Este recurso fornece resultados detalhados de cada passo.
| Tamanho máximo do evento | 64 K | 
| Comprimento do nome da propriedade e da métrica | 150 | Consulte [escreva esquemas](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Comprimento da cadeia de valor da propriedade | 8,192 | Consulte [escreva esquemas](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Comprimento da mensagem de exceção e de rastreio | 10 MIL | Consulte [escreva esquemas](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| [Testes de disponibilidade](../articles/application-insights/app-insights-monitor-web-app-availability.md) com contagem por aplicação | 100 |
| [O gerador de perfis](../articles/application-insights/app-insights-profiler.md) retenção de dados | 5 dias |
| [O gerador de perfis](../articles/application-insights/app-insights-profiler.md) dados enviados por dia | 10 GB |

Para mais informações, consulte [About pricing and quotas in Application Insights (Acerca de preços e quotas no Application Insights)](../articles/application-insights/app-insights-pricing.md).