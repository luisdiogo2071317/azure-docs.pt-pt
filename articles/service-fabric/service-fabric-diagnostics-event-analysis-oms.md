---
title: Análise de eventos de recursos de infraestrutura de serviço do Azure com o Log Analytics | Documentos da Microsoft
description: Saiba mais sobre como visualizar e analisar eventos com o Log Analytics para monitorização e diagnóstico de clusters do Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: 332939710517e99aaa77642dc5e67256b476bd66
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52634580"
---
# <a name="event-analysis-and-visualization-with-log-analytics"></a>Análise de eventos e visualização com o Log Analytics
 Log Analytics recolhe e analisa a telemetria a partir de aplicações e serviços alojados na cloud e fornece ferramentas de análise para ajudar a maximizar a disponibilidade e desempenho. Este artigo descreve como executar consultas do Log Analytics para obter informações e resolver problemas relacionados com o que acontece no seu cluster. As seguintes perguntas comuns são abordadas:

* Como posso resolver eventos de estado de funcionamento?
* Como posso saber quando um nó ficar inativo?
* Como posso saber se os serviços do meu aplicativo tem iniciou ou parou?

## <a name="log-analytics-workspace"></a>Área de trabalho do Log Analytics

>[!NOTE] 
>Embora o armazenamento de diagnóstico está ativado por predefinição, o momento de criação de cluster, tem ainda de configurar a área de trabalho do Log Analytics para ler a partir do armazenamento do diagnóstico.

O log Analytics recolhe dados de recursos geridos, incluindo uma tabela de armazenamento do Azure ou um agente e mantém-lo num repositório central. Os dados, em seguida, podem ser utilizado para análise, alertas e visualização, ou ainda mais a exportação. O log Analytics oferece suporte a eventos, dados de desempenho ou outros dados personalizados. Confira [passos para configurar a extensão de diagnóstico para agregar eventos](service-fabric-diagnostics-event-aggregation-wad.md) e [passos para criar uma área de trabalho do Log Analytics para ler a partir de eventos no armazenamento](service-fabric-diagnostics-oms-setup.md) para se certificar de que os dados estão a ser encaminhados para o Log Analytics .

Depois de dados são recebidos pelo Log Analytics, o Azure tem várias *soluções de gestão* que são soluções previamente incluídas em pacotes ou dashboards operacionais para monitorizar os dados recebidos, personalizados para vários cenários. Estes incluem uma *análise do Service Fabric* solução e um *contentores* solução, que são as mais relevantes duas para diagnóstico e monitorização quando utilizar clusters do Service Fabric. Este artigo descreve como utilizar a solução de análise do Service Fabric, que é criada com a área de trabalho.

## <a name="access-the-service-fabric-analytics-solution"></a>Acesso a solução de análise do Service Fabric

1. No Portal do Azure, vá para o grupo de recursos em que criou a solução de análise do Service Fabric.

2. Selecione o recurso **ServiceFabric\<nameOfOMSWorkspace\>**.

2. No `Summary`, verá os mosaicos na forma de um gráfico para cada uma das soluções de ativadas, incluindo um para o Service Fabric. Clique nas **Service Fabric** graph (primeira imagem abaixo) para continuar para a solução de análise do Service Fabric (segunda imagem abaixo).

    ![Solução de Service Fabric](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_summary.PNG)

    ![Solução de Service Fabric](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_solution.PNG)

A imagem acima é a home page da solução de análise do Service Fabric. Esta é uma vista de instantâneo do que está acontecendo no seu cluster. Se ativou o diagnóstico durante a criação de cluster, pode ver eventos para 

* [Eventos de cluster do Service Fabric](service-fabric-diagnostics-event-generation-operational.md)
* [Eventos do modelo de programação de Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
* [Eventos do modelo de programação dos Reliable Services](service-fabric-reliable-services-diagnostics.md)

>[!NOTE]
>Além dos eventos do Service Fabric prontos a utilizar, mais detalhada de sistema de eventos pode ser recolhida pelo [atualizar a configuração da sua extensão de diagnóstico](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations).

### <a name="view-service-fabric-events-including-actions-on-nodes"></a>Ver eventos de recursos de infraestrutura de serviços incluindo ações em nós

1. Na página de análise do Service Fabric, clique no gráfico para **eventos de Service Fabric**.

    ![Canal operacional do Service Fabric solução](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events_selection.png)

2. Clique em **lista** para exibir os eventos numa lista. Uma vez aqui, verá todos os eventos de sistema que tenham sido recolhidos. Para referência, são de WADServiceFabricSystemEventsTable na conta de armazenamento do Azure e da mesma forma o reliable services e eventos de atores que verá são provenientes as respectivas tabelas.
    
    ![Canal operacional da consulta](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events.png)

Em alternativa pode clique na Lupa à esquerda e utilizar a linguagem de consulta de Kusto para encontrar o que está procurando. Por exemplo, para localizar todas as ações executadas em nós do cluster, pode utilizar a consulta seguinte. Os IDs de evento utilizados abaixo encontram-se no [referência de eventos de canal operacional](service-fabric-diagnostics-event-generation-operational.md).

```kusto
ServiceFabricOperationalEvent
| where EventId < 25627 and EventId > 25619 
```

Pode consultar em muitos mais campos, como os nós específicos (computador) o serviço de sistema (TaskName).

### <a name="view-service-fabric-reliable-service-and-actor-events"></a>Eventos de serviço fiável de recursos de infraestrutura do serviço de exibição e de Ator

1. Na página de análise do Service Fabric, clique no gráfico para **Reliable Services**.

    ![Serviços fiáveis do Service Fabric solução](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_services_events_selection.png)

2. Clique em **lista** para exibir os eventos numa lista. Aqui pode ver os eventos a partir de serviços fiáveis. Pode ver a diferentes eventos para quando o serviço runasync é iniciada e concluída que costuma acontecer sobre Implantações e atualizações. 

    ![Consulta Reliable Services](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_service_events.png)

Eventos de Reliable actor podem ser exibidos de maneira semelhante. Para configurar eventos mais detalhados dos reliable actors, terá de alterar o `scheduledTransferKeywordFilter` a configuração para a extensão de diagnóstico (mostrada abaixo). Detalhes sobre os valores para estes foram os [referência de eventos do reliable actors](service-fabric-reliable-actors-diagnostics.md#keywords).

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

A linguagem de consulta de Kusto é poderosa. Outra consulta valiosa, que pode executar é descobrir quais os nós estão a gerar a maioria dos eventos. A consulta na captura de ecrã abaixo mostra os eventos operacionais do Service Fabric agregados com o serviço específico e o nó.

![Eventos de consulta por nó](media/service-fabric-diagnostics-event-analysis-oms/oms_kusto_query.png)

## <a name="next-steps"></a>Passos Seguintes

* Para ativar a monitorização, ou seja, contadores de desempenho da infraestrutura, aceda à [adicionando o agente Log Analytics](service-fabric-diagnostics-oms-agent.md). O agente recolhe os contadores de desempenho e os adiciona à sua área de trabalho existente.
* Para os clusters no local, o Log Analytics oferece um Gateway (reencaminhar Proxy HTTP) que pode ser utilizado para enviar dados para o Log Analytics. Leia mais sobre isso em [ligar os computadores sem acesso à Internet ao Log Analytics com o gateway do Log Analytics](../azure-monitor/platform/gateway.md).
* Configurar [alertas automáticos](../log-analytics/log-analytics-alerts.md) para ajudar no diagnóstico e de deteção.
* Familiarizar-se com as funcionalidades de [registos de pesquisas e consultas](../log-analytics/log-analytics-log-searches.md) que são oferecidas como parte do Log Analytics.
* Obtenha uma visão geral mais detalhada do Log Analytics e o que ele oferece, leia [o que é o Log Analytics?](../operations-management-suite/operations-management-suite-overview.md).
