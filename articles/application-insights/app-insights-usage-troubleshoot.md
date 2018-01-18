---
title: "Resolver problemas de análise de utilização no Azure Application Insights"
description: "Guia de resolução de problemas - análise da utilização de sites e aplicações com o Application Insights."
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/16/2018
ms.author: mbullwin
ms.openlocfilehash: cb5f3052301b23eb10cd6b84ab6fae98bcc7ea18
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="troubleshoot-usage-analytics-in-application-insights"></a>Resolver problemas de análise de utilização no Application Insights
Tem perguntas sobre o [as ferramentas de análise de utilização no Application Insights](app-insights-usage-overview.md): [eventos de utilizadores, sessões,](app-insights-usage-segmentation.md), [Funnels](usage-funnels.md), [utilizador flui](app-insights-usage-flows.md), [Retenção](app-insights-usage-retention.md), ou Cohorts? Seguem-se algumas respostas.

## <a name="counting-users"></a>Contagem de utilizadores
**A análise de utilização ferramentas mostram que a minha aplicação tem um utilizador/sessão, mas sei a minha aplicação tem vários utilizadores/sessões. Como pode corrigir estas contagens incorretas?**

Todos os eventos de telemetria no Application Insights tem um [ID de utilizador anónimo](application-insights-data-model-context.md) e um [ID de sessão](application-insights-data-model-context.md) como dois as respetivas propriedades padrão. Por predefinição, todas as ferramentas de análise de utilização contagem de utilizadores e sessões com base nestes IDs. Se estas propriedades padrão não estão a ser preenchidas com os IDs exclusivos para cada utilizador e a sessão da sua aplicação, verá uma contagem incorreta de utilizadores e sessões nas ferramentas de análise de utilização.

Se estiver a monitorizar uma aplicação web, a solução mais fácil consiste em Adicionar a [Application Insights SDK de JavaScript](app-insights-javascript.md) a sua aplicação e certifique-se o fragmento de script é carregado em cada página que pretende monitorizar. O SDK de JavaScript automaticamente gera o utilizador anónimo e IDs de sessão, em seguida, preenche os eventos de telemetria com estes IDs, que está a ser enviados da sua aplicação.

Se estiver a monitorizar um serviço web (sem interface de utilizador), [criar um inicializador de telemetria que povoa as propriedades de ID de ID e a sessão de utilizador anónimo](app-insights-usage-send-user-context.md) , de acordo com notions do seu serviço de utilizadores únicos e sessões.

Se a aplicação está a enviar [autenticado IDs de utilizador](app-insights-api-custom-events-metrics.md#authenticated-users), pode contar com base no utilizador autenticado IDs na ferramenta de utilizadores. Na lista pendente "Mostrar", selecione "Utilizadores autenticados."

As ferramentas de análise de utilização não suportam atualmente contagem de utilizadores ou sessões com base nas propriedades que não seja o ID de utilizador anónimo, o ID de utilizador autenticado ou o ID de sessão.

## <a name="naming-events"></a>Eventos de nomenclatura
**A minha aplicação tem milhares de vista de página diferente e os nomes do evento personalizado. É difícil de distingui-los e as ferramentas de análise de utilização, muitas vezes, deixar de responder. Como posso corrigir estes problemas de nomenclatura?**

Vista de página e os nomes de evento personalizado são utilizados em toda as ferramentas de análise de utilização. Eventos de nomenclatura bem é fundamental para obter o valor destas ferramentas. O objetivo é um equilíbrio entre ter nomes de poucos, demasiado genéricos ("botão clicado") e ter nomes demasiados, excessivamente específicos ("botão Editar clicou http://www.contoso.com/index").

Para efetuar quaisquer alterações para a vista de página e a aplicação está a enviar de nomes de evento personalizado, terá de alterar o código de origem e volte a implementar a aplicação. **Telemetria de todos os dados no Application Insights são armazenados nos últimos 90 dias e não podem ser eliminados**, por isso, as alterações efetuadas aos nomes do evento irão demorar 90 dias ao manifesto completamente. 90 dias depois de efetuar as alterações do nome, ambos os nomes de antigos e novos eventos serão apresentada na sua telemetria, por isso, ajuste consultas e comunicou dentro do seu equipas, em conformidade.

Se a aplicação está a enviar demasiados nomes de vista de página, verifique se estes nomes de vista de página são especificadas manualmente no código ou se estiver a ser enviados automaticamente pelo Application Insights JavaScript SDK:

* Se os nomes de vista de página manualmente especificados no código utilizando a [ `trackPageView` API](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md), altere o nome para ser menos específico. Evite prende comuns, como colocar o URL do nome de vista de página. Em alternativa, utilize o parâmetro de URL do `trackPageView` API. Mova outros detalhes do nome de vista da página para as propriedades personalizadas.

* Se o SDK de JavaScript do Application Insights está a enviar automaticamente os nomes de vista de página, pode alterar os títulos dos seus páginas ou mudar para enviar manualmente os nomes de vista de página. O SDK envia o [título](https://developer.mozilla.org/docs/Web/HTML/Element/title) de cada página que o nome de vista de página, por predefinição. Pode alterar os títulos de para ser mais gerais, mas ter mindful de SEO e outros impactos que ter esta alteração. Especificar manualmente a visualização de página nomes com o `trackPageView` API substitui os nomes de automaticamente recolhidos, pelo que poderia enviar nomes mais gerais na telemetria sem alterar os títulos de página.   

Se a aplicação está a enviar demasiados nomes de evento personalizado, altere o nome no código para ser menos específico. Novamente, evite colocar o URLs e outras por página ou informações dinâmicas os nomes do evento personalizado diretamente. Em vez disso, mover estes detalhes para as propriedades personalizadas do evento personalizado com o `trackEvent` API. Por exemplo, em vez de `appInsights.trackEvent("Edit button clicked on http://www.contoso.com/index")`, sugerimos que algo semelhante ao seguinte `appInsights.trackEvent("Edit button clicked", { "Source URL": "http://www.contoso.com/index" })`.

## <a name="next-steps"></a>Passos Seguintes

* [Descrição geral da análise de utilização](app-insights-usage-overview.md)

## <a name="get-help"></a>Obter ajuda
* [Stack Overflow](http://stackoverflow.com/questions/tagged/ms-application-insights)

