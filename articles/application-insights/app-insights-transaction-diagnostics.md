---
title: "Diagnóstico de transação de informações de aplicação do Azure | Microsoft Docs"
description: "Diagnóstico de transação ponto-a-ponto do Application Insights"
services: application-insights
documentationcenter: .net
author: SoubhagyaDash
manager: victormu
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/19/2018
ms.author: sdash
ms.openlocfilehash: b090699cf90c74af8480b811901b6e3078b007b3
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="unified-cross-component-transaction-diagnostics"></a>Diagnóstico de unificada de entre componentes de transação

*Esta experiência está atualmente em pré-visualização e substitui os painéis de diagnósticos existentes para pedidos do lado do servidor, dependências e exceções.*

A pré-visualização introduz uma nova experiência de diagnóstico unificada automaticamente está correlacionada com a telemetria do lado do servidor de todos os componentes do Application Insights monitorizado numa única vista. É irrelevante se tiver vários recursos com chaves de instrumentação separada; Application Insights Deteta a relação subjacente e permite-lhe diagnosticar facilmente o componente da aplicação, dependência ou exceção que causou um slowdown transação ou falha.

## <a name="what-does-component-mean-in-the-context-of-application-insights"></a>O que significa componente no contexto do Application Insights?

Componentes são independentemente implementáveis partes da aplicação distribuída/micro-serviços. As equipas de operações e os programadores têm visibilidade de nível de código ou o acesso a telemetria gerada por estes componentes da aplicação. 

* Componentes são diferentes das "observadas" dependências externas, como SQL, etc. EventHub que a sua organização/equipa pode não ter acesso ao (código ou telemetria).
* Componentes de executam em qualquer número de instâncias do servidor/função/contentor.
* Componentes podem ser separadas chaves de instrumentação do Application Insights (mesmo se subscrições diferentes) ou diferentes funções de relatório para uma única chave de instrumentação do Application Insights. A nova experiência mostra os detalhes em todos os componentes, independentemente da forma como estas tiverem sido configuradas.

> [!Tip]
> Para obter os melhores resultados, certifique-se de que todos os componentes são equipados com os SDKs estáveis mais recentes do Application Insights. Se existirem diferentes recursos do Application Insights, certifique-se de que tem os direitos adequados para ver os respetivos telemetria.

## <a name="enable-and-access"></a>Ativar e acesso
Ativar "Unified detalhes: diagnósticos de transação de E2E" do [lista pré-visualizações](app-insights-previews.md)

![Ativar a pré-visualização](media/app-insights-e2eTxn-diagnostics/previews.png)

Esta pré-visualização está atualmente disponível para pedidos do lado do servidor, dependências e exceções. Pode aceder a nova experiência de **resultados da pesquisa**, **desempenho**, ou **falha** triagem experiências. A pré-visualização substitui os painéis detalhes clássico correspondente. 

![Amostras de desempenho](media/app-insights-e2eTxn-diagnostics/performanceSamplesClickThrough.png)

## <a name="transaction-diagnostics-experience"></a>Experiência de diagnóstico de transação 
Esta vista tem três partes de chaves: um gráfico de transação entre componentes, uma lista de sequência de tempo de toda a telemetria de uma operação do componente específico e o painel de detalhes de qualquer item de telemetria selecionado no lado esquerdo.

![Partes de chaves](media/app-insights-e2eTxn-diagnostics/3partsCrossComponent.png)

### <a name="1-cross-component-transaction-chart"></a>[1] gráfico de transação de Cross-componente

Este gráfico fornece uma linha cronológica com barras horizontais durante a pedidos e dependências de componentes. Quaisquer exceções que são recolhidas são também marcadas na linha cronológica.

* A linha superior este gráfico representa o ponto de entrada, o pedido de entrada para o primeiro componente chamado nesta transação. A duração é o total de tempo decorrido para a transação concluir.
* As chamadas para dependências externas são linhas não expansível simples, com os ícones que representa o tipo de dependência.
* Chamadas para outros componentes são expansíveis linhas. Cada linha corresponde a uma operação específica invocada no componente.
* Por predefinição, o pedido, dependência ou exceção que inicialmente selecionou é apresentada no gráfico.
* Selecione qualquer linha para ver os respectivos detalhes à direita. Clique em "rastreios de gerador de perfis abrir" ou "depuração Abrir instantâneo" diagnóstico de nível de código nos painéis de detalhe correspondente.

> [!NOTE]
As chamadas de outros componentes têm duas linhas: uma linha representa a chamada de saída (dependência) componente do autor da chamada do e a outra linha corresponde do pedido de entrada, o componente chamado. O ícone à esquerda e distinto estilo das barras de duração irão ajudar a diferenciar entre eles.

### <a name="2-time-sequenced-telemetry-of-the-selected-component-operation"></a>[2] tempo-sequenciado telemetria da operação de componentes selecionado

Qualquer linha selecionada no gráfico de transação entre componentes está relacionado com uma operação invocada num componente em particular. Esta operação do componente selecionado é refletida no título da secção na parte inferior. Abra esta secção para ver uma sequência de tempo simples de todas as telemetrias relacionadas com essa operação específica. Pode selecionar qualquer item de telemetria nesta lista para ver detalhes correspondentes à direita.

![Sequência de tempo de toda a telemetria](media/app-insights-e2eTxn-diagnostics/allTelemetryDrawerOpened.png)

### <a name="3-details-pane"></a>[3] painel de detalhes de

Este painel mostra os detalhes dos itens selecionados de qualquer uma das duas secções no lado esquerdo. "Mostrar tudo" apresenta uma lista de todos os atributos padrão que são recolhidos. Atributos personalizados em separado estão listados abaixo conjunto padrão.

![Detalhe de exceção](media/app-insights-e2eTxn-diagnostics/exceptiondetail.png)

## <a name="profiler-and-snapshot-debugger"></a>Depurador do gerador de perfis e instantâneos

[O gerador de perfis do Application Insights](app-insights-profiler.md) ou [depurador de instantâneo](app-insights-snapshot-debugger.md) ajudar com o nível de código de diagnóstico de problemas de desempenho e falha. Com esta experiência, pode ver os rastreios de gerador de perfis ou instantâneos de qualquer componente com um único clique.

![Integração do depurador](media/app-insights-e2eTxn-diagnostics/debugSnapshot.png)

## <a name="faq"></a>FAQ

*Vejo um único componente no gráfico e outros utilizadores que apenas que mostra como dependências externas sem quaisquer detalhes do que aconteceu dentro desses componentes.*

Razões possíveis:

* Os outros componentes são equipados com o Application Insights?
* Se utilizar o estável mais recente de Application Insights SDK?
* Se estes componentes são recursos do Application Insights separados, tem acesso necessário à sua telemetria?

Se tiver acesso e os componentes são equipados com os SDKs de informações de aplicação mais recente, indique através do canal de comentários direito superior.

*Tenho apenas dependências externas. Deve importantes sobre esta pré-visualização?*

Sim. A nova experiência unifica toda a telemetria do lado do servidor relacionada numa única vista. Painéis de detalhe mais antigos serão substituídos por esta experiência no futuro, por isso, experimente e dê-nos seus comentários. Eis o aspeto de uma transação de componente único:

![Experiência de componente único](media/app-insights-e2eTxn-diagnostics/singleComponent.png)

*Consulte o artigo linhas duplicadas para as dependências. É esta esperada?*

Neste momento, estamos são a mostrar a chamada de dependência de saída separados do pedido de entrada. Normalmente, as chamadas de dois parecem idênticas com apenas o valor de duração existente que está a ser diferente devido a rede percurso de ida e volta. O ícone à esquerda e distinto estilo das barras de duração irão ajudar a diferenciar entre eles. Esta apresentação dos dados é confuso? Dê-nos seus comentários!

*E sobre o relógio skews em instâncias de diferentes componentes?*

Linhas cronológicas são ajustadas para relógio skews no gráfico de transação. Pode ver os carimbos exatos no painel de detalhes, ou utilizando a análise.

*Por que motivo está a nova experiência em falta na maior parte das consultas itens relacionados?*

Esta ação é propositada. Todos os itens relacionados, todos os componentes, já estão disponíveis no lado esquerdo (superior e inferior secções). A nova experiência tem dois itens relacionados que não abrange o lado esquerdo: toda a telemetria de cinco minutos antes e depois deste evento e a linha de tempo do utilizador.

*Por que razão a nova experiência não tem a funcionalidade que posso loved em painéis de mais antigos?*

Envie-nos comentários! Queremos resolver os problemas antes desta experiência estiver geralmente disponível, em que momento as vistas mais antigas vão ser preteridas. Por agora, pode desativar a pré-visualização para regressar ao painéis de mais antigos.

## <a name="known-issues"></a>Problemas Conhecidos

* Os exemplos de falha de ligação de mapeamento de aplicações para os painéis de detalhe mais antigos.
* O insights autocluster baseada nos resultados da pesquisa uma ligação para os painéis de detalhe mais antigos.
* Integração do item de trabalho não está disponível na nova experiência.