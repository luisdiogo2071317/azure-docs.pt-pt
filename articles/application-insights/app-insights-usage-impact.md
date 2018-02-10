---
title: "Impacto de utilização do Azure Application Insights | Documentos da Microsoft"
description: "Analisar as propriedades de forma diferentes potencialmente afetar as taxas de conversão para partes das suas aplicações."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/25/2018
ms.author: mbullwin ; daviste
ms.openlocfilehash: d76db02647ce878343f60fc84cf063c5b7833438
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="impact-analysis-with-application-insights"></a>Análise de impacto com o Application Insights

Impacto analisa a forma como os tempos de carregamento e outras propriedades influenciam a taxas de conversão de várias partes da sua aplicação. Para colocá-la mais precisamente, Deteta como **qualquer dimensão** de um **página vista**, **evento personalizado**, ou **pedido** afeta a utilização de um diferentes **página vista** ou **evento personalizado**. 

![Ferramenta de impacto](./media/app-insights-usage-impact/0001-impact.png)

## <a name="still-not-sure-what-impact-does"></a>Ainda não se que impacto faz?

Uma forma errada de impacto é como a ferramenta ultimate para settling argumentos com alguém na sua equipa sobre como slowness em algum aspeto do seu site está a afetar se os utilizadores essência. Enquanto os utilizadores podem tolerar uma determinada quantidade de slowness, o impacto dá-lhe informações sobre como melhores balancear o Otimização e desempenho para maximizar a conversão de utilizador.

Mas analisar o desempenho é apenas um subconjunto das capacidades do impacto. Uma vez que o impacto suporta dimensões e eventos personalizados, responder a perguntas como como does eleição de browser do utilizador está correlacionado com taxas diferentes de conversão são apenas alguns cliques ausente.

![Conversão de captura de ecrã por browsers](./media/app-insights-usage-impact/0004-browsers.png)

> [!NOTE]
> O recurso do Application Insights tem de conter vistas de página ou eventos personalizados para utilizar a ferramenta de impacto. [Saiba como configurar a sua aplicação para recolher as vistas de página automaticamente com o Application Insights SDK de JavaScript](app-insights-javascript.md). Também é importante tenha em atenção que uma vez que estão a analisar correlação, tamanho de exemplo.
>
>

## <a name="is-page-load-time-impacting-how-many-people-convert-on-my-page"></a>É o tempo de carregamento de página afetar como muitas pessoas converter na minha página?

Para iniciar as respostas a questões com a ferramenta de impacto, escolha uma vista de página inicial, o evento personalizado ou a pedido.

![Ferramenta de impacto](./media/app-insights-usage-impact/0002-dropdown.png)

1. Selecione uma vista de página do **para a vista de página** pendente.
2. Deixe o **analisar como respetivo** pendente na seleção predefinida de **duração** (neste contexto **duração** é um alias para **tempo de carregamento de página**.)
3. Para o **tem impacto sobre a utilização de** lista pendente, selecione um evento personalizado. Este evento deve corresponder a um elemento de IU da vista de página que selecionou no passo 1.

![Captura de ecrã dos resultados](./media/app-insights-usage-impact/0003-results.png)

Nesta instância como **página de produto** tempo de carregamento aumenta a taxa de conversão para **produto compra clicado** ficar inativo. Com base na distribuição acima, uma duração de carregamento de página ideal de segundos 3.5 foi direcionada para alcançar uma taxa de conversão de % de 55 potenciais. Melhorias de desempenho adicionais para reduzir o tempo de carregamento abaixo segundos 3.5 atualmente está correlacionado com vantagens adicionais de conversão.

## <a name="what-if-im-tracking-page-views-or-load-times-in-custom-ways"></a>E se estou controlo vistas de página ou carregar vezes formas personalizado?

Impacto suporta propriedades de padrão e personalizadas e medidas. Utilize-se de que pretende. Em vez de duração, utilize filtros nos eventos primários e secundários para obter mais específicos.

## <a name="do-users-from-different-countries-or-regions-convert-at-different-rates"></a>Os utilizadores de diferentes países ou regiões convertidos com taxas diferentes?

1. Selecione uma vista de página do **para a vista de página** pendente.
2. Escolha "País ou região" em **analisar como respetivo** pendente
3. Para o **tem impacto sobre a utilização de** lista pendente, selecione um evento personalizado que corresponde a um elemento de IU da vista de página selecionou no passo 1.

Neste caso, os resultados já não se ajustam a um modelo do eixo x contínua forma que funcionavam no primeiro exemplo. Em vez disso, é apresentada uma visualização semelhante a um funil segmentado. Ordenar por **utilização** para ver a variação de conversão para o evento personalizado com base num país.


## <a name="how-does-the-impact-tool-calculate-these-conversion-rates"></a>Como a ferramenta de impacto calcular as taxas de conversão?

Os bastidores, a ferramenta de impacto depende [coeficiente de correlação Pearson] (https://en.wikipedia.org/wiki/Pearson_correlation_coefficient). Os resultados são calculados entre -1 e 1 por -1 que representam zero correlação e 1 que representa uma correlação positiva.

Como funciona a análise de impacto discrimina básica é o seguinte:

Permitir que _A_ = principal vista/personalizada eventos/pedido da página selecionar na primeira lista pendente. (**Para a vista de página**).

Permitir que _B_ = o evento de vista/personalizada secundário página selecionar (**tem impacto sobre a utilização de**).

Impacto observa uma amostra de todas as sessões dos utilizadores no intervalo de tempo selecionado. Para cada sessão, parece para cada ocorrência dos _A_.

As sessões são divididas em seguida, dois tipos diferentes de _subsessions_ baseadas nas duas condições:

- Um subsession convertida é composta por uma sessão de terminar com um _B_ eventos e abrange todos os _A_ eventos que ocorrem antes _B_.
- Um subsession não convertido ocorre quando todas as _A_do ocorrem sem um terminal _B_.

Como o impacto é basicamente calculado varia consoante se podemos estão analisar por métrica ou por dimensão. Para todas as métricas _A_num subsession são uma média. Enquanto que para dimensions o valor de cada _A_ contribui _1/N_ para o valor atribuído à _B_ onde _N_ é o número de _A_no subsession.

## <a name="next-steps"></a>Passos Seguintes

- Para ativar as experiências de utilização, começar a enviar [eventos personalizados](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) ou [vistas de página](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Se já enviar eventos personalizados ou vistas de página, explore as ferramentas de utilização para saber como os utilizadores utilizam o serviço.
    - [Funis](usage-funnels.md)
    - [Retenção](app-insights-usage-retention.md)
    - [Fluxos do Utilizador](app-insights-usage-flows.md)
    - [Livros](app-insights-usage-workbooks.md)
    - [Adicionar o contexto de utilizador](app-insights-usage-send-user-context.md)