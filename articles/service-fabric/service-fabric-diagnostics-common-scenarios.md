---
title: Cenários comuns de diagnosticar de recursos de infraestrutura de serviço do Azure | Microsoft Docs
description: Saiba como resolver problemas de cenários comuns com o Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/16/2018
ms.author: srrengar
ms.openlocfilehash: bd7a7e0288ced0219a0600034b273d1acba6b09b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655741"
---
# <a name="diagnose-common-scenarios-with-service-fabric"></a>Diagnosticar cenários comuns com o Service Fabric

Este artigo ilustra cenários comuns de utilizadores tem encontrado na área de monitorização e diagnóstico com o Service Fabric. Os cenários apresentados abrangem todas as 3 camadas dos recursos de infraestrutura do serviço: aplicações, de Cluster e infraestrutura. Cada solução utiliza o Application Insights e análise de registos, ferramentas de monitorização do Azure, para concluir cada cenário. Os passos em cada solução de fornecer uma introdução aos utilizadores sobre como utilizar o Application Insights e análise de registos no contexto do Service Fabric.

## <a name="prerequisites-and-recommendations"></a>Pré-requisitos e recomendações

As soluções neste artigo, irão utilizar as seguintes ferramentas. Recomendamos que tenha este conjunto operacional e configurado:

* [Application Insights com o Service Fabric](service-fabric-tutorial-monitoring-aspnet.md)
* [Ativar o diagnóstico do Azure no seu cluster](service-fabric-diagnostics-event-aggregation-wad.md)
* [Configurar uma área de trabalho OMS da análise do registo](service-fabric-diagnostics-oms-setup.md)
* [Agente do OMS para controlar os contadores de desempenho](service-fabric-diagnostics-oms-agent.md)

## <a name="how-can-i-see-unhandled-exceptions-in-my-application"></a>Como ver exceções não processadas na minha aplicação?

1. Navegue para o recurso do Application Insights que a aplicação está configurada com.
2. Clique em *pesquisa* na parte superior esquerda. Em seguida, clicar em filtro no painel seguinte.

    ![Descrição geral de AI](media/service-fabric-diagnostics-common-scenarios/ai-search-filter.png)

3. Verá muitos tipos de eventos (rastreios de pedidos, eventos personalizados). Escolha "Exceção" como o filtro.

    ![Lista de filtros de AI](media/service-fabric-diagnostics-common-scenarios/ai-filter-list.png)

    Ao clicar uma exceção na lista, pode ver mais detalhes, incluindo o contexto de serviço, se estiver a utilizar o Application Insights SDK do serviço de recursos de infraestrutura.

    ![Exceção de AI](media/service-fabric-diagnostics-common-scenarios/ai-exception.png)

## <a name="how-do-i-view-which-http-calls-are-used-in-my-services"></a>Como ver qual HTTP chamadas são utilizadas nos meus serviços?

1. No mesmo recurso do Application Insights, pode filtrar por "pedidos" em vez de exceções e ver todos os pedidos efetuados
2. Se estiver a utilizar o Application Insights SDK do serviço de recursos de infraestrutura, pode ver uma representação visual dos seus serviços ligados para outro, e o número de foi concluída com êxito e pedidos falhados. No lado esquerdo, clique em "O mapeamento de aplicações"

    ![Painel de mapa de aplicação de AI](media/service-fabric-diagnostics-common-scenarios/app-map-blade.png) ![mapa de aplicação de AI](media/service-fabric-diagnostics-common-scenarios/app-map-new.png)

    Para obter mais informações sobre o mapa de aplicação, visite o [documentação de mapeamento de aplicações](../application-insights/app-insights-app-map.md)

## <a name="how-do-i-create-an-alert-when-a-node-goes-down"></a>Como criar um alerta quando um nó fica inativo

1. Eventos de nó são registados pelo cluster do Service Fabric. Navegue para o recurso de solução de análise de recursos de infraestrutura de serviço chamado **ServiceFabric(NameofResourceGroup)**
2. Clique no gráfico na parte inferior do painel intitulada "Resumo"

    ![Solução da OMS](media/service-fabric-diagnostics-common-scenarios/oms-solution-azure-portal.png)

3. Aqui tem muitos gráficos e apresentar várias métricas de mosaicos. Clique dos gráficos e é direcionado para a pesquisa de registo. Aqui pode consultar os contadores de desempenho ou eventos de cluster.
4. Introduza a seguinte consulta. Estes IDs de evento se encontram no [referência de eventos do nó](service-fabric-diagnostics-event-generation-operational.md#application-events)

    ```kusto
    ServiceFabricOperationalEvent
    | where EventId >= 25623 or EventId <= 25626
    ```

5. Clique em "Nova regra de alerta" na parte superior e agora sempre que um evento chegar com base nesta consulta, irá receber um alerta no seu método escolhido de comunicação.

    ![Novo alerta do OMS](media/service-fabric-diagnostics-common-scenarios/oms-create-alert.png)

## <a name="how-can-i-be-alerted-of-application-upgrade-rollbacks"></a>Como pode posso ser alertado de reverte de atualização de aplicação?

1. Na janela pesquisa registo mesma como antes de introduzir a seguinte consulta para atualização reverte. Estes IDs de evento encontram-se em [referência de eventos de aplicações](service-fabric-diagnostics-event-generation-operational.md#application-events)

    ```kusto
    ServiceFabricOperationalEvent
    | where EventId == 29623 or EventId == 29624
    ```

2. Clique em "Nova regra de alerta" na parte superior e agora sempre que um evento chegar com base nesta consulta, irá receber um alerta.

## <a name="how-do-i-see-container-metrics"></a>Como posso ver as métricas do contentor?

Na vista do mesma com todos os gráficos, irá ver alguns mosaicos para o desempenho dos seus contentores. Terá do agente do OMS e [solução de monitorização do contentor](service-fabric-diagnostics-oms-containers.md) para estes mosaicos preencher.

![Métricas de contentor do OMS](media/service-fabric-diagnostics-common-scenarios/containermetrics.png)

>[!NOTE]
>A telemetria do instrumento de **dentro** o contentor, terá de adicionar o [pacote nuget do Application Insights para contentores](https://github.com/Microsoft/ApplicationInsights-servicefabric#microsoftapplicationinsightsservicefabric--for-service-fabric-lift-and-shift-scenarios).

## <a name="how-can-i-monitor-performance-counters"></a>Como pode monitorizar os contadores de desempenho?

1. Depois de adicionar o agente do OMS para o cluster tem de adicionar os contadores de desempenho específicos que pretende controlar. Navegue para a página da área de trabalho OMS no portal da partir da página da solução que é o separador de área de trabalho no menu da esquerda.

    ![Separador de área de trabalho do OMS](media/service-fabric-diagnostics-common-scenarios/workspacetab.png)

2. Quando estiver na página da área de trabalho, clique em "Definições avançadas" no menu à esquerda do mesmo.

    ![As definições avançadas do OMS](media/service-fabric-diagnostics-common-scenarios/advancedsettingsoms.png)

3. Clique em dados > contadores de desempenho do Windows (dados > os contadores de desempenho do Linux em máquinas Linux) para iniciar a recolha de contadores específicos dos seus nós através do agente do OMS. Seguem-se exemplos de formato para os contadores adicionar

    * `.NET CLR Memory(<ProcessNameHere>)\\# Total committed Bytes`
    * `Processor(_Total)\\% Processor Time`
    * `Service Fabric Service(*)\\Average milliseconds per request`

    No início rápido, VotingData e VotingWeb são os nomes de processo utilizados, pelo que estes contadores de controlo teria aspeto

    * `.NET CLR Memory(VotingData)\\# Total committed Bytes`
    * `.NET CLR Memory(VotingWeb)\\# Total committed Bytes`

    ![Contadores de desempenho do OMS](media/service-fabric-diagnostics-common-scenarios/omsperfcounters.png)

4. Isto permite-lhe ver como a infraestrutura está a processar as cargas de trabalho e definir alertas relevantes com base na utilização de recursos. Por exemplo – poderá pretender definir um alerta se a utilização do processador total ficar acima 90% ou inferior a 5%. O nome do contador que utilizaria para isto é "% de tempo do processador." Pode fazê-lo através da criação de uma regra de alerta para a seguinte consulta:

    ```kusto
    Perf | where CounterName == "% Processor Time" and InstanceName == "_Total" | where CounterValue >= 90 or CounterValue <= 5.
    ```

## <a name="how-do-i-track-performance-of-my-reliable-services-and-actors"></a>Como controlar o desempenho da minha Reliable Services e Atores?

Para controlar o desempenho dos Reliable Services ou Atores nas suas aplicações, deve adicionar os contadores de Ator do Service Fabric, o método de Ator, o serviço e o método do serviço também. Pode adicionar estes contadores de forma semelhante, como o cenário acima, seguem-se exemplos fiáveis ator e do serviço de contadores de desempenho para adicionar do OMS

* `Service Fabric Service(*)\\Average milliseconds per request`
* `Service Fabric Service Method(*)\\Invocations/Sec`
* `Service Fabric Actor(*)\\Average milliseconds per request`
* `Service Fabric Actor Method(*)\\Invocations/Sec`

Verifique as ligações para a lista completa dos contadores de desempenho no fiável [serviços](service-fabric-reliable-serviceremoting-diagnostics.md) e [Actors](service-fabric-reliable-actors-diagnostics.md)

## <a name="next-steps"></a>Passos Seguintes

* [Configurar alertas no AI](../application-insights/app-insights-alerts.md) para ser notificado sobre as alterações no desempenho ou utilização
* [Smart deteção no Application Insights](../application-insights/app-insights-proactive-diagnostics.md) efetua uma análise proativa de telemetria a ser enviada para AI para o avisar de potenciais problemas de desempenho
* Saiba mais sobre a análise de registos do OMS [alertas](../log-analytics/log-analytics-alerts.md) para ajudar a deteção e de diagnóstico.
* Para os clusters no local, OMS oferece um Gateway (reencaminhar Proxy HTTP) que pode ser utilizado para enviar dados para OMS. Leia mais sobre o que no [ligar os computadores sem acesso à Internet ao OMS utilizando o Gateway do OMS](../log-analytics/log-analytics-oms-gateway.md)
* Obter familiarized com o [de registo de pesquisa e consultar](../log-analytics/log-analytics-log-searches.md) funcionalidades disponibilizadas como parte da análise de registos
* Obter uma descrição mais detalhada de análise de registos e o que oferece, leia [o que é a análise de registos?](../operations-management-suite/operations-management-suite-overview.md)
