---
title: Monitorizar o estado de funcionamento da sua aplicação e a utilização com o Application Insights
description: Introdução ao Application Insights. Analise a utilização, disponibilidade e desempenho dos seus no local ou aplicações do Microsoft Azure.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 40650472-e860-4c1b-a589-9956245df307
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: sdash
ms.openlocfilehash: 02421492528e44ed6a913443a7793235170d4881
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="monitor-performance-in-web-applications"></a>Monitorizar o desempenho nas aplicações Web


Certifique-se de que a aplicação está a executar corretamente e descobrir rapidamente sobre eventuais falhas. [Application Insights] [ start] conte quaisquer problemas de desempenho e exceções e ajudar a localizar e diagnosticar as causas raiz.

Application Insights podem monitorizar aplicações web de Java e ASP.NET e serviços, serviços do WCF. Podem ser alojado no local, em máquinas virtuais, ou como Web sites do Microsoft Azure. 

No lado do cliente, o Application Insights pode demorar a telemetria de páginas web e uma ampla variedade de dispositivos, incluindo iOS, Android e aplicações da loja Windows.

## <a name="setup"></a>Configurar a monitorização de desempenho
Se ainda não adicionou ainda Application Insights ao seu projeto (ou seja, se não tem Applicationinsights), escolha uma das seguintes formas para começar a utilizar:

* [Aplicações web do ASP.NET](app-insights-asp-net.md)
  * [Adicionar a monitorização de exceções](app-insights-asp-net-exceptions.md)
  * [Adicionar a monitorização de dependência](app-insights-monitor-performance-live-website-now.md)
* [Aplicações web J2EE](app-insights-java-get-started.md)
  * [Adicionar a monitorização de dependência](app-insights-java-agent.md)

## <a name="view"></a>Explorar métricas de desempenho
No [portal do Azure](https://portal.azure.com), navegue para o recurso do Application Insights que configurou para a sua aplicação. O painel de descrição geral apresenta dados de desempenho básico:

Clique em qualquer gráfico para ver mais detalhes e para ver os resultados para um período mais longo. Por exemplo, clique no mosaico de pedidos e, em seguida, selecione um intervalo de tempo:

![Clicar para obter mais dados e selecione um intervalo de tempo](./media/app-insights-web-monitor-performance/appinsights-48metrics.png)

Clique num gráfico para escolher as métricas apresenta, ou adicione um novo gráfico e selecionar a métricas:

![Clique num gráfico para escolher as métricas](./media/app-insights-web-monitor-performance/appinsights-61perfchoices.png)

> [!NOTE]
> **Desmarque todas as métricas** para ver a seleção completa que está disponível. As métricas enquadram-se grupos; Quando qualquer membro de um grupo é selecionado, são apresentados apenas os outros membros desse grupo.

## <a name="metrics"></a>O que faz-média de todos os? Os mosaicos de desempenho e relatórios
Existem várias métricas de desempenho, que pode obter. Vamos começar com as que são apresentados por predefinição no painel de aplicação.

### <a name="requests"></a>Pedidos
O número de pedidos HTTP recebidos no período de tempo especificado. Compare-as com os resultados em outros relatórios para ver como a aplicação se comporta como a carga varia.

Pedidos de HTTP incluem todos os pedidos GET ou POST de páginas, dados e imagens.

Clique no mosaico para obter contagens para os URLs específicos.

### <a name="average-response-time"></a>Tempo de resposta médio
Mede o tempo entre a um pedido web introduzir a sua aplicação e a resposta a ser devolvido.

Os pontos de mostram uma mudança médio. Se existirem muitos pedidos, poderão existir alguns desvio da média sem um horário de pico óbvios ou dip no gráfico.

Procure picos de atividade invulgares. Em geral, esperar que um tempo de resposta a aumentar com um aumento súbito do pedidos. Se o aumento súbito desproporcionado, pode atingir um limite de recursos, tais como CPU ou a capacidade de um serviço que utiliza a sua aplicação.

Clique no mosaico para obter vezes para URLs específicos.

![](./media/app-insights-web-monitor-performance/appinsights-42reqs.png)

### <a name="slowest-requests"></a>Pedidos mais lentos
![](./media/app-insights-web-monitor-performance/appinsights-44slowest.png)

Mostra quais os pedidos poderão ter de otimização de desempenho.

### <a name="failed-requests"></a>Pedidos falhados
![](./media/app-insights-web-monitor-performance/appinsights-46failed.png)

Uma contagem de pedidos que emitiu exceções não identificadas.

Clique no mosaico para ver os detalhes de falhas específicas e selecione um pedido individual para ver os detalhes. 

Apenas uma amostra representativa de falhas é mantida para inspecção individuais.

### <a name="other-metrics"></a>Outras métricas
Para ver o que definir outras métricas pode apresentar, clique num gráfico e, em seguida, desmarque todas as métricas para ver o completo disponível. Clique (i) para ver a definição de cada métrica.

![Anular seleção de todas as métricas para ver o conjunto completo](./media/app-insights-web-monitor-performance/appinsights-62allchoices.png)

Selecionar qualquer métrica desativa a outras pessoas que não pode aparecer no mesmo gráfico.

## <a name="set-alerts"></a>Definir alertas
Para ser notificado por correio eletrónico dos valores invulgares de qualquer métrica, adicione um alerta. Pode escolher a enviar o e-mail para os administradores de conta ou para endereços de e-mail específico.

![](./media/app-insights-web-monitor-performance/appinsights-413setMetricAlert.png)

Defina o recurso antes das outras propriedades. Não escolha os recursos de teste Web, se pretender definir alertas nas métricas de desempenho ou utilização.

Tenha o cuidado de tenha em atenção as unidades nas quais está a pedido para introduzir o valor de limiar.

*Não consigo ver o botão Adicionar alerta.* -Este é um grupo de conta para o qual tem acesso só de leitura? Contacte o administrador da conta.

## <a name="diagnosis"></a>Diagnosticar problemas
Eis algumas sugestões para localizar e diagnosticar problemas de desempenho:

* Configurar [testes web] [ availability] para ser alertado se o web site ficar inativo ou responder lentamente ou incorretamente. 
* Compare a contagem de pedidos com outras métricas para ver se falhas ou resposta lenta relacionadas com a carregar.
* [Inserir, procure as instruções de rastreio] [ diagnostic] no código para ajudar a identificar problemas.
* Monitorizar a aplicação Web na operação com [métricas em fluxo em direto][livestream].
* Capturar o estado da aplicação .net com [instantâneo depurador][snapshot].

## <a name="find-and-fix-performance-bottlenecks-with-performance-investigation-experience"></a>Localize e corrija os congestionamentos de desempenho com a experiência de investigação de desempenho

Pode utilizar a experiência de investigação de desempenho para rever as operações de desempenho lentas na sua aplicação Web. Pode selecionar uma operação lenta específica e utilizar rapidamente [gerador de perfis](app-insights-profiler.md) raiz fazer com que as operações lentas para baixo até o código. Utilizar a distribuição da duração novo apresentada para a operação selecionada, pode rapidamente rapidamente apenas como incorreta é a experiência para os seus clientes. Pode ver o número das suas interações do utilizador foram afetado para cada operação lenta. No exemplo seguinte, foi decidido demorar um olhar mais a experiência de operação de clientes/detalhes obter. A distribuição da duração, é possível ver que existem três picos. Pico de pedidos mais à esquerda é cerca de 400 ms e representa excelente experiência reativa. Pico de pedidos média é em torno 1.2 s e representa uma experiência mediocre. Por fim, o 3.6 s temos outro pico pequeno pedidos que representa a experiência de percentil 99th, que é provavelmente, causar os nossos clientes deixe dissatisfied. Essa experiência é dez vezes mais lenta do que o excelente experiência para a mesma operação. 

![Picos de duração três clientes/detalhes GET](./media/app-insights-web-monitor-performance/PerformanceTriageViewZoomedDistribution.png)

Para obter um melhor sentido das experiências de utilizador para esta operação, mas pode selecionar um intervalo de tempo maior. Iremos pode, em seguida, também reduzir no tempo numa janela de tempo específico em que a operação foi lenta. No exemplo seguinte, iremos mudar da predefinição 24 horas, intervalo de tempo para 7 dias intervalo de tempo e, em seguida, ampliado para o intervalo de tempo 9:47 para 12:47 entre Tue a 12 e qua a 13. A distribuição da duração e o número de rastreios de gerador de perfis de exemplo e foram atualizadas no lado direito.

![Obtenha os clientes/detalhes intervalo três picos de duração dentro de 7 dias com uma janela de tempo](./media/app-insights-web-monitor-performance/PerformanceTriageView7DaysZoomedTrend.png)

Para restringir nas experiências lentas, iremos junto aplicar zoom no durações que se inserem entre 95th e o percentil 99th. Estes representam a % de 4 de interações do utilizador que foram lentas.

![Obtenha os clientes/detalhes intervalo três picos de duração dentro de 7 dias com uma janela de tempo](./media/app-insights-web-monitor-performance/PerformanceTriageView7DaysZoomedTrendZoomed95th99th.png)

Podemos agora a vista de olhos amostras representativos, clicando no botão exemplos ou os rastreios de gerador de perfis representativa, clicando no botão de rastreios de gerador de perfis. Neste exemplo, existem quatro rastreios que tenham sido recolhidos para obter os clientes/detalhes na duração de janela e o intervalo de tempo de interesse.

Por vezes, o problema não será possível no seu código, mas em vez numa dependência chama o seu código. Pode mudar para o separador de dependências na vista de triagem de desempenho para investigar essas dependências lentas. Por predefinição a vista de desempenho é médias tendências, mas que realmente pretende observar o percentil 95th (ou 99th, caso esteja a monitorizar um serviço madura). No exemplo seguinte Concentramo-na dependência lenta de BLOB do Azure, onde chamamos PUT fabrikamaccount. O bom ocorrer no cluster cerca de 40 ms, enquanto as chamadas lentas para a dependência mesma três vezes inferior, clustering cerca de 120 ms. Não tem muitas destas chamadas para adicionar a cópia de segurança para fazer com que a respetiva operação abrandar intensas em termos. Pode explorar representativos exemplos e rastreios de gerador de perfis, tal como pode fazer com o separador de operações.

![Obtenha os clientes/detalhes intervalo três picos de duração dentro de 7 dias com uma janela de tempo](./media/app-insights-web-monitor-performance/SlowDependencies95thTrend.png)

A experiência de investigação de desempenho mostra relevantes insights ao longo do lado do conjunto de exemplo que escolheu para se focarem em. É a melhor forma de ver todas as informações disponíveis para mudar para um intervalo de tempo de 30 dias e, em seguida, selecione geral para ver informações em todas as operações para o último mês.

![Obtenha os clientes/detalhes intervalo três picos de duração dentro de 7 dias com uma janela de tempo](./media/app-insights-web-monitor-performance/Performance30DayOveralllnsights.png)


## <a name="next"></a>Passos seguintes
[Testes Web] [ availability] -tenham pedidos web enviados para a aplicação em intervalos regulares de todo o mundo.

[Capturar e procurar rastreios de diagnóstico] [ diagnostic] - inserir chamadas de rastreio e pouco úteis percorrer os resultados para identificar problemas.

[Controlo de utilização] [ usage] -saber como as pessoas utilizam a sua aplicação.

[Resolução de problemas] [ qna] - e as perguntas e respostas



<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[greenbrown]: app-insights-asp-net.md
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
[usage]: app-insights-web-track-usage.md
[livestream]: app-insights-live-stream.md
[snapshot]: app-insights-snapshot-debugger.md



