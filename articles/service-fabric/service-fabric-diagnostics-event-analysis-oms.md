---
title: Análise de eventos de recursos de infraestrutura de serviço do Azure com a análise de registos | Microsoft Docs
description: Saiba mais sobre como visualizar e analisar eventos utilizando a análise de registos para monitorização e diagnóstico de clusters de Service Fabric do Azure.
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
ms.date: 04/16/2018
ms.author: dekapur; srrengar
ms.openlocfilehash: 8efbc1d400f1d32e6aee2c1e2d78847bea786940
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="event-analysis-and-visualization-with-log-analytics"></a>Análise de eventos e visualização de análise de registos

Análise de registos, também conhecido como OMS (Operations Management Suite), é uma coleção de serviços de gestão que ajudam na monitorização e diagnóstico para aplicações e serviços alojados na nuvem. Este artigo descreve como executar consultas na análise de registos para obter informações e resolver problemas relacionados com o que acontece no seu cluster. As seguintes perguntas comuns são resolvidas:

* Como posso resolver problemas eventos de estado de funcionamento?
* Como saber quando um nó fica inativo?
* Como saber se os serviços da minha aplicação tem iniciado ou parado?

## <a name="log-analytics-workspace"></a>Área de trabalho do Log Analytics

Análise de registos recolhe dados a partir dos recursos geridos, incluindo uma tabela de armazenamento do Azure ou um agente e mantém-lo num repositório central. Os dados, em seguida, podem ser utilizado para análise, alertas e visualização ou ainda mais a exportar. Análise de registos suporta eventos, dados de desempenho ou quaisquer outros dados personalizados. Veja [passos para configurar a extensão de diagnóstico para agregar eventos](service-fabric-diagnostics-event-aggregation-wad.md) e [passos para criar uma área de trabalho de análise de registos para ler a partir de eventos no armazenamento](service-fabric-diagnostics-oms-setup.md) certificar-se de dados é que fluem para análise de registos .

Depois de dados são recebidos pelo análise de registos, o Azure tem vários *soluções de gestão* que são prepackaged soluções para monitorizar os dados de entrada, personalizados para vários cenários. Estes incluem uma *análise de recursos de infraestrutura de serviço* solução e um *contentores* solução, o que são os dois mais relevante para diagnóstico e monitorização quando utilizar clusters de Service Fabric. Este artigo descreve como utilizar a solução de análise de recursos de infraestrutura de serviço, que é criada com a área de trabalho.

## <a name="access-the-service-fabric-analytics-solution"></a>A solução de análise de recursos de infraestrutura do serviço de acesso

1. Vá para o grupo de recursos no qual criou a solução de análise de recursos de infraestrutura de serviço. Selecione o recurso **ServiceFabric\<nameOfOMSWorkspace\>**  e aceda à página de descrição geral.

2. Na página Descrição geral, clique na ligação perto da parte superior para aceder ao portal do OMS

    ![Link do Portal do OMS](media/service-fabric-diagnostics-event-analysis-oms/oms-portal-link.png)

3. Agora está no portal do OMS e pode ver as soluções que tiver ativado. Clique no gráfico intitulado Service Fabric (primeira imagem abaixo) para obter direcionado para a solução de Service Fabric (segunda imagem abaixo)

    ![Solução SF do OMS](media/service-fabric-diagnostics-event-analysis-oms/oms-workspace-all-solutions.png)

    ![Solução SF do OMS](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics-new.png)

A imagem acima é a home page da solução de análise de recursos de infraestrutura de serviço. Esta é uma vista de instantâneo do que acontece no seu cluster. Se tiver ativado o diagnóstico após a criação do cluster, pode ver eventos 

* [Canal operacional](service-fabric-diagnostics-event-generation-operational.md): operações de nível mais elevadas que efetua a plataforma do Service Fabric (coleção de serviços do sistema).
* [Eventos do modelo de programação de Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
* [Serviços fiáveis de eventos do modelo de programação](service-fabric-reliable-services-diagnostics.md)

>[!NOTE]
>Para além do canal operacional, mais detalhados de eventos de sistema podem ser recolhidos pelo [atualizar a configuração de extensão de diagnóstico](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations)

### <a name="view-operational-events-including-actions-on-nodes"></a>Ver eventos operacionais, incluindo ações em nós

1. Na página de análise de recursos de infraestrutura de serviço no portal do OMS, clique no gráfico para o canal operacional

    ![Canal operacional do OMS SF solução](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics-new-operational.png)

2. Clique em tabela para ver os eventos numa lista. Uma vez aqui, verá todos os eventos de sistema que tenham sido recolhidos. Para referência, estes são de WADServiceFabricSystemEventsTable na conta do Storage do Azure e da mesma forma as reliable services e eventos de atores que consulte junto são destas tabelas correspondentes.
    
    ![Canal de operacional de consulta do OMS](media/service-fabric-diagnostics-event-analysis-oms/oms-query-operational-channel.png)

Em alternativa, pode clique a Lupa à esquerda e utilize o idioma de consulta Kusto encontrar aquilo que procura. Por exemplo, para localizar todas as ações executadas em nós do cluster, pode utilizar a seguinte consulta. Os IDs de evento utilizados abaixo encontram-se no [referência de eventos operacionais de canal](service-fabric-diagnostics-event-generation-operational.md)

```kusto
ServiceFabricOperationalEvent
| where EventId < 25627 and EventId > 25619 
```

Pode consultar o muitos mais campos, tais como os nós específicos (computador) o serviço de sistema (TaskName).

### <a name="view-service-fabric-reliable-service-and-actor-events"></a>Eventos de serviço fiável de recursos de infraestrutura do serviço de vista e Ator

1. Na página de análise de recursos de infraestrutura de serviço no portal do OMS, clique o gráfico para Reliable Services

    ![OMS SF solução Reliable Services](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics-reliable-services.png)

2. Clique em tabela para ver os eventos numa lista. Aqui pode ver eventos a partir dos serviços fiáveis. Pode ver eventos de diferentes para quando o runasync com serviço é iniciada e concluída que normalmente acontece em implementações e atualizações. 

    ![Consultar o OMS Reliable Services](media/service-fabric-diagnostics-event-analysis-oms/oms-query-reliable-services.png)

Eventos de ator fiável podem ser visualizados de forma semelhante. Para configurar mais detalhado de eventos dos reliable actors, terá de alterar o `scheduledTransferKeywordFilter` na configuração para a extensão de diagnóstico (mostrada abaixo). Obter detalhes sobre os valores para estes estão no [referência de eventos de reliable actors](service-fabric-reliable-actors-diagnostics.md#keywords)

```json
"EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
```

O idioma de consulta Kusto é poderoso. É outra consulta importante, que pode executar saber quais os nós estão a gerar a maior parte dos eventos. A consulta na captura de ecrã abaixo mostra os eventos de serviços fiável agregado com o serviço específico e o nó

![Eventos de consulta do OMS por nó](media/service-fabric-diagnostics-event-analysis-oms/oms-query-events-per-node.png)

## <a name="next-steps"></a>Passos Seguintes

* Para ativar a monitorização, ou seja, os contadores de desempenho da infraestrutura, aceda a [adicionar o agente do OMS](service-fabric-diagnostics-oms-agent.md). O agente recolhe os contadores de desempenho e adiciona-o à sua área de trabalho existente.
* Para os clusters no local, OMS oferece um Gateway (reencaminhar Proxy HTTP) que pode ser utilizado para enviar dados para OMS. Leia mais sobre o que no [ligar os computadores sem acesso à Internet ao OMS utilizando o Gateway do OMS](../log-analytics/log-analytics-oms-gateway.md)
* Configurar OMS configurar [automatizada alertas](../log-analytics/log-analytics-alerts.md) para ajudar a deteção e diagnóstico
* Obter familiarized com o [de registo de pesquisa e consultar](../log-analytics/log-analytics-log-searches.md) funcionalidades disponibilizadas como parte da análise de registos
* Obter uma descrição mais detalhada de análise de registos e o que oferece, leia [o que é a análise de registos?](../operations-management-suite/operations-management-suite-overview.md)
