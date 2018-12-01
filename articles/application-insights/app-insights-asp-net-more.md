---
title: Aproveitar ao máximo o Azure Application Insights | Documentos da Microsoft
description: Após a introdução ao Application Insights, aqui está um resumo das funcionalidades, pode explorar.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 7ec10a2d-c669-448d-8d45-b486ee32c8db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/03/2017
ms.author: mbullwin
ms.openlocfilehash: 48232402ee091d6e41381f54cbfa3276cefc47ac
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52682236"
---
# <a name="more-telemetry-from-application-insights"></a>Mais telemetria do Application Insights
Depois de ter [adicionado o Application Insights ao seu código ASP.NET](app-insights-asp-net.md), existem algumas coisas que pode fazer para obter ainda mais telemetria. 

| Ação | O que obtém|
|---|---|
|(Servidores IIS) [Instalar o Monitor de estado](https://go.microsoft.com/fwlink/?LinkId=506648) em cada máquina do servidor.<br/>(As aplicações web do azure) No painel de controlo do Azure para a aplicação web, abra o painel do Application Insights.| [**Contadores de desempenho**](app-insights-performance-counters.md)<br/>[**Exceções** ](app-insights-asp-net-exceptions.md) - detalhadas rastreios de pilha<br/>[**Dependências**](app-insights-asp-net-dependencies.md)|
|[Adicione o fragmento do JavaScript às suas páginas web](app-insights-javascript.md)|[Página desempenho](app-insights-usage-overview.md), exceções do browser, desempenho de AJAX. Telemetria personalizada do lado do cliente.|
|[Criar testes web de disponibilidade](app-insights-monitor-web-app-availability.md)|Obtenha alertas se o seu site ficar indisponível|
|[Certifique-se de buildinfo.config](https://msdn.microsoft.com/library/dn449058.aspx) é gerado pelo MSBuild|[Criar anotações em gráficos de métricas](https://blogs.msdn.microsoft.com/visualstudioalm/2013/11/14/implementing-deployment-markers-in-application-insights/)
|[Escrever métricas e eventos personalizados](app-insights-api-custom-events-metrics.md)|Contagem de métricas e eventos de negócios, controlar a utilização detalhada e muito mais.|
|[Perfil de seu site em direto](https://aka.ms/AIProfilerPreview)|Tempos de função detalhadas da sua aplicação web em direto|






