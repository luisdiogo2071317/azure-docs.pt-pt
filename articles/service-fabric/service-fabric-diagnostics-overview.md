---
title: Azure Service Fabric, monitorização e diagnóstico descrição-geral | Documentos da Microsoft
description: Saiba mais sobre monitorização e diagnóstico para clusters, aplicações e serviços do Azure Service Fabric.
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
ms.date: 04/25/2018
ms.author: srrengar
ms.openlocfilehash: a607f560b5e74071f5ee15d03e615138f25a3aef
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/18/2018
ms.locfileid: "49406811"
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Monitorização e diagnóstico para o Azure Service Fabric

Este artigo fornece uma descrição geral da monitorização e diagnóstico para o Azure Service Fabric. Monitorização e diagnóstico é fundamental para desenvolver, testar e implementar cargas de trabalho em qualquer ambiente de cloud. Monitorização permite-lhe controlar a utilização de seus aplicativos, a utilização de recursos e o estado de funcionamento global do seu cluster. Pode utilizar estas informações para diagnosticar e corrigir quaisquer problemas e impedir que os problemas que ocorrem no futuro. 

## <a name="application-monitoring"></a>Monitorização de aplicações
Monitorização de aplicações controla como os recursos e componentes da sua aplicação estão sendo usados. Pretende monitorizar as aplicações para tornar-se de que problemas esse impacto que os utilizadores são detetados. Monitorização de seus aplicativos pode ser útil nos seguintes cenários:
* Determinar a carga da aplicação e o utilizador de tráfego – precisa de dimensionar os seus serviços para atender às demandas de usuário ou abordar um possível afunilamento em seu aplicativo?
* Identificação de problemas com a comunicação de serviço e a comunicação remota em seu cluster
* Descobrir o que os usuários estão fazendo com a sua aplicação – recolher a telemetria em seus aplicativos pode ajudar a desenvolver de funcionalidades futuras do guia e um melhor diagnóstico para erros de aplicação
* Monitorizar o que está acontecendo nos contentores em execução

O Service Fabric suporta muitas opções para instrumentar o código da aplicação com a telemetria e rastreios adequados. Recomendamos que utilize o Application Insights (IA). Integração de IA com o Service Fabric inclui ferramentas experiências para o portal do Azure e do Visual Studio, bem como métricas específicas do Service Fabric, para proporcionar uma experiência de registo de out-of-the-box abrangente. Embora muitos registos são automaticamente criados e recolhidos para com IA, recomendamos que adicionar mais registo personalizado para as aplicações para criar uma experiência mais rica de diagnóstico. Ver mais informações sobre como começar com o Application Insights com o Service Fabric em [análise de eventos com o Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md).

## <a name="platform-cluster-monitoring"></a>Monitorização de plataforma (Cluster)
Monitorização do seu cluster do Service Fabric é fundamental para garantir que a plataforma e todas as cargas de trabalho estão a funcionar como pretendido. Um dos objetivos do Service Fabric é manter as aplicações resilientes a falhas de hardware. Essa meta é alcançada através de capacidade dos serviços do sistema da plataforma para detetar problemas de infraestrutura e, rapidamente, com cargas de trabalho de ativação pós-falha para outros nós do cluster. Mas, nesse caso específico, e se os serviços do sistema se tem problemas? Ou, em caso de tentativa de mover uma carga de trabalho, são violou regras para o posicionamento de serviços? Monitorizar o cluster permite que mantenha-se informado sobre a atividade que ocorrem no seu cluster, o que ajuda a diagnosticar problemas e corrigi-los com eficiência. Algumas coisas chave que pretende procurar por são:
* É o Service Fabric se comportando como esperado, em termos de balanceamento de trabalho em todo o cluster e colocar seus aplicativos? 
* Ações do usuário são executadas no seu cluster reconhecidas e executadas em conforme o esperado? Isto é especialmente relevante quando dimensionar um cluster.
* Service Fabric trata os seus dados e a comunicação de serviços dentro do cluster corretamente?

O Service Fabric fornece um conjunto abrangente de eventos de imediato. Estes [eventos do Service Fabric](service-fabric-diagnostics-events.md) podem ser acedidos através das APIs de EventStore ou o canal operacional (canal de eventos exposto pela plataforma). 
* EventStore - EventStore (disponível no Windows em versões 6.2 e versões posteriores, Linux continua em curso a partir da data da última deste artigo atualização), expõe esses eventos por meio de um conjunto de APIs (acessíveis por meio de pontos finais REST ou por meio da biblioteca de cliente). Saiba mais sobre o EventStore no [descrição geral do EventStore](service-fabric-diagnostics-eventstore.md).
* Eventos de recursos de infraestrutura do serviço do Service Fabric eventos canais - no Windows, estão disponíveis a partir de um fornecedor ETW único com um conjunto de relevantes `logLevelKeywordFilters` usado para selecionar entre canais operacional e de mensagens & de dados - esta é a maneira na qual vamos separar de saída Serviço de eventos de recursos de infraestrutura sejam filtrados no conforme necessário. No Linux, os eventos do Service Fabric passar pela LTTng e são colocados numa tabela de armazenamento, de onde eles podem ser filtrados conforme necessário. Estes canais contenham organizados, estruturados eventos que podem ser utilizados para compreender melhor o estado do cluster. Diagnósticos estão ativados por predefinição no momento da criação do cluster, que criar uma tabela de armazenamento do Azure onde os eventos desses canais são enviados para consultar no futuro. 

Recomendamos utilizar o EventStore para análise rápida e para ter uma idéia de instantâneo de funcionamento do seu cluster e, se as coisas estão acontecendo como esperado. Para recolher os registos e os eventos gerados pelo seu cluster, em geral, recomendamos que utilize o [extensão de diagnóstico do Azure](service-fabric-diagnostics-event-aggregation-wad.md). Isso se integra bem com a análise do Service Fabric, Service Fabric solução específica do Log Analytics, que fornece um dashboard personalizado para monitorizar clusters do Service Fabric e permite-lhe consultar eventos do seu cluster e configurar alertas. Saiba mais sobre isso em [análise de eventos com o Log Analytics](service-fabric-diagnostics-event-analysis-oms.md). 

 Pode ler mais sobre a monitorização do seu cluster em [geração de eventos e de registo ao nível no plataforma](service-fabric-diagnostics-event-generation-infra.md).

## <a name="performance-monitoring"></a>Monitorização de desempenho
Monitorizar a sua infraestrutura subjacente é uma parte fundamental do Noções básicas sobre o estado do seu cluster e a utilização de recursos. Avaliando o desempenho do sistema depende de vários fatores, cada um dos quais normalmente é medida por meio de um indicadores chave de desempenho (KPIs). KPIs relevantes do Service Fabric podem ser mapeados para as métricas que podem ser coletadas a partir de nós do cluster, como contadores de desempenho.
Estes KPIs podem ajudar com:
* Noções básicas sobre a utilização de recursos e carregar - com o objetivo de dimensionamento do seu cluster ou otimizar seus processos de serviço.
* Prever problemas de infraestrutura - muitos problemas são precedidos por alterações repentinas (cai) no desempenho, pelo que pode utilizar os KPIs, como a utilização de CPU e e/s de rede para prever e diagnosticar problemas de infraestrutura.

Uma lista de contadores de desempenho que devem ser recolhidos no nível de infraestrutura pode ser encontrada em [métricas de desempenho](service-fabric-diagnostics-event-generation-perf.md). 

O Service Fabric fornece um conjunto de contadores de desempenho para os modelos de programação Reliable Services e Atores. Se estiver a utilizar qualquer um desses modelos, estes contadores de desempenho podem fornecer KPIs que ajudam a garantir que seus atores são girando corretamente e reduzir verticalmente, ou que os pedidos de serviço fiável estão a ser processados rápido o bastante. Para obter mais informações, consulte [monitorização para Reliable Service Remoting](service-fabric-reliable-serviceremoting-diagnostics.md#performance-counters) e [monitorização de desempenho para Reliable Actors](service-fabric-reliable-actors-diagnostics.md#performance-counters). Além disso, o Application Insights tem também um conjunto de métricas de desempenho irá recolher, se configurado com a sua aplicação.

Utilize o [agente do Log Analytics](service-fabric-diagnostics-oms-agent.md) para recolher os contadores de desempenho adequado e ver estes KPIs no Azure Log Analytics.

![Gráfico de descrição geral de diagnóstico](media/service-fabric-diagnostics-overview/diagnostics-overview.png)

## <a name="health-monitoring"></a>Monitorização do estado de funcionamento
A plataforma do Service Fabric inclui um modelo de estado de funcionamento, que fornece o estado de funcionamento extensível relatórios para o estado de entidades num cluster. Cada nó, aplicações, serviço, partição, réplica ou instância, tem um Estado de funcionamento continuamente atualizável. O estado de funcionamento pode ser "OK", "Aviso" ou "Erro". O estado de funcionamento é alterado por meio de relatórios de estado de funcionamento que são emitidos para cada entidade, com base em problemas no cluster. O estado de funcionamento das suas entidades pode ser verificado em qualquer altura no Service Fabric Explorer (SFX), conforme mostrado abaixo, ou pode ser consultado através da API de estado de funcionamento das plataformas. Também pode personalizar os relatórios de estado de funcionamento e modificar o estado de funcionamento de uma entidade ao adicionar seus próprios relatórios de estado de funcionamento ou com a API de estado de funcionamento. Obter mais detalhes sobre o modelo de estado de funcionamento podem ser encontrados em [introdução à monitorização de estado de funcionamento do Service Fabric](service-fabric-health-introduction.md).

![Dashboard de estado de funcionamento SFX](media/service-fabric-diagnostics-overview/sfx-healthstatus.png)

Além de ver relatórios de estado de funcionamento mais recente no SFX, cada relatório também está disponível como um evento. Eventos de estado de funcionamento podem ser coletados por meio do canal operacional (consulte [agregação de eventos com o diagnóstico do Azure](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations)) e armazenados no Log Analytics para alertas e consultas no futuro. Isto ajuda a detetar problemas que possam afetar a disponibilidade de aplicações, pelo que recomendamos que configure alertas para cenários de falha adequadas (alertas personalizados com o Log Analytics).

## <a name="other-logging-solutions"></a>Outras soluções de registo

Embora as duas soluções recomendamos, [do Azure Log Analytics](service-fabric-diagnostics-event-analysis-oms.md) e [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) criaram na integração com o Service Fabric, muitos eventos são escritos por meio de provedores do etw e são extensível com outras soluções de registo. Também deve examinar a [Elastic Stack](https://www.elastic.co/products) (especialmente se estiver a considerar a executar um cluster num ambiente offline), [Splunk](https://www.splunk.com/), [Dynatrace](https://www.dynatrace.com/), ou qualquer outro plataforma de sua preferência. 

Os principais pontos para qualquer plataforma que escolher devem incluir como confortável estiver com a interface do usuário e a consulta opções, a capacidade de visualizar dados e criar dashboards facilmente legíveis e as ferramentas adicionais fornecem para melhorar a monitorização, Por exemplo, alertas automáticos.

## <a name="next-steps"></a>Passos Seguintes

* Para começar a trabalhar com instrumentar seus aplicativos, consulte [geração de eventos e de registo ao nível no aplicativo](service-fabric-diagnostics-event-generation-app.md).
* Saiba mais sobre a monitorização da plataforma e os eventos que o Service Fabric fornece para na [geração de eventos e de registo ao nível no plataforma](service-fabric-diagnostics-event-generation-infra.md).
* Avance para os passos para configurar a ia para a sua aplicação com [monitorizar e diagnosticar uma aplicação ASP.NET Core no Service Fabric](service-fabric-tutorial-monitoring-aspnet.md).
* Saiba como configurar o Azure Log Analytics para monitorizar contentores- [monitorização e diagnósticos para contentores do Windows no Azure Service Fabric](service-fabric-tutorial-monitoring-wincontainers.md).
* Saiba mais sobre as recomendações gerais da monitorização para recursos do Azure - [práticas recomendadas – a monitorização e diagnóstico](https://docs.microsoft.com/azure/architecture/best-practices/monitoring). 
