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
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: 82c02c0212fd79d8847d374022b6ac8f862f042a
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/22/2018
ms.locfileid: "52291118"
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Monitorização e diagnóstico para o Azure Service Fabric

Este artigo fornece uma descrição geral da monitorização e diagnóstico para o Azure Service Fabric. Monitorização e diagnóstico é fundamental para desenvolver, testar e implementar cargas de trabalho em qualquer ambiente de cloud. Por exemplo, pode controlar como os seus aplicativos são usados, as ações executadas pela plataforma do Service Fabric, a utilização de recursos com contadores de desempenho e o estado de funcionamento global do seu cluster. Pode usar essas informações para diagnosticar e corrigir problemas e impedir que ocorra no futuro. As próximas seções explicam resumidamente cada área de monitorização a considerar para cargas de trabalho de produção do Service Fabric. 

## <a name="application-monitoring"></a>Monitorização de aplicações
Monitorização de aplicações controla como os recursos e componentes da sua aplicação estão sendo usados. Pretende monitorizar as aplicações para tornar-se de que problemas esse impacto que os utilizadores são detetados. É a responsabilidade de monitorização de aplicações em utilizadores desenvolvendo um aplicativo e seus serviços, uma vez que é exclusivo para a lógica de negócios da sua aplicação. Monitorização de seus aplicativos pode ser útil nos seguintes cenários:
* Quanto tráfego é com meu aplicativo? -Precisa dimensionar os seus serviços para atender às demandas de utilizador ou endereço de um possível afunilamento em seu aplicativo?
* São minhas chamadas de serviço para serviço com êxito e controlado?
* Quais ações são executadas pelos utilizadores da minha aplicação? -Recolher a telemetria pode ajudá-lo desenvolver recursos futuros e um melhor diagnóstico para erros de aplicações
* Meu aplicativo está a lançar exceções sem tratamento? 
* O que está acontecendo dentro os serviços em execução dentro de meu contentores?

A grande novidade sobre a monitorização de aplicações é que os desenvolvedores podem usar qualquer estrutura que gostariam, uma vez que residem dentro do contexto da sua aplicação e as ferramentas! Pode saber mais sobre a solução do Azure para a monitorização de aplicações com o Azure Monitor - Application Insights no [análise de eventos com o Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md).
Além disso, temos um tutorial sobre como [defina esta opção para aplicativos .NET](service-fabric-tutorial-monitoring-aspnet.md). Este tutorial aplica-se sobre como instalar as ferramentas corretas, um exemplo de escrever telemetria personalizada em seu aplicativo e exibição do application diagnostics e a telemetria no portal do Azure. 


## <a name="platform-cluster-monitoring"></a>Monitorização de plataforma (Cluster)
Um utilizador estiver no controle sobre quais telemetria provém de seu aplicativo, uma vez que um utilizador escreve o código em si, mas o que sobre os diagnósticos de plataforma do Service Fabric? Um dos objetivos do Service Fabric é manter as aplicações resilientes a falhas de hardware. Essa meta é alcançada através de capacidade dos serviços do sistema da plataforma para detetar problemas de infraestrutura e, rapidamente, com cargas de trabalho de ativação pós-falha para outros nós do cluster. Mas, nesse caso específico, e se os serviços do sistema se tem problemas? Ou, em caso de tentar implementar ou mover uma carga de trabalho, são violou regras para o posicionamento de serviços? O Service Fabric fornece diagnósticos para estes e muito mais para se certificar de que é informado sobre a atividade que ocorrem no seu cluster. Alguns cenários de exemplo para monitorização de clusters incluem:

* É o Service Fabric se comportando como esperado, em termos de balanceamento de trabalho em todo o cluster e colocar seus aplicativos? 
* Ações do usuário são executadas no seu cluster reconhecidas e executadas em conforme o esperado? Por exemplo, Dimensionamento, ativação pós-falha, as implementações
* É o Service Fabric manter o controle de quais os nós fazem parte do cluster e informando-me quando existe um problema em um?

O diagnóstico fornecido está na forma de um conjunto abrangente de eventos prontos a utilizar. Estes [eventos do Service Fabric](service-fabric-diagnostics-events.md) ilustrar ações feitas pela plataforma num diferentes entidades como nós, aplicações, serviços, partições etc. O último cenário acima, se um nó for desativado, a plataforma seria emitir um `NodeDown` eventos e pode ser notificado imediatamente por sua ferramenta de monitoramento de escolha. Outros exemplos comuns incluem `ApplicationUpgradeRollbackStarted` ou `PartitionReconfigured` durante uma ativação pós-falha. **Os mesmos eventos estão disponíveis nos clusters do Windows e Linux.**

Os eventos são enviados através de canais padrão no Windows e Linux e podem ser lidos por qualquer ferramenta de monitoramento que suporta estes. A solução do Azure Monitor é o Log Analytics. Fique à vontade ler mais sobre o nosso [integração do Log Analytics](service-fabric-diagnostics-event-analysis-oms.md) que inclui um dashboard personalizado do operacional para o cluster e algumas consultas de exemplo a partir da qual pode criar alertas. Mais conceitos de monitorização de cluster estão disponíveis em [geração de eventos e de registo ao nível no plataforma](service-fabric-diagnostics-event-generation-infra.md).

### <a name="health-monitoring"></a>Monitorização do estado de funcionamento
A plataforma do Service Fabric inclui um modelo de estado de funcionamento, que fornece o estado de funcionamento extensível relatórios para o estado de entidades num cluster. Cada nó, aplicações, serviço, partição, réplica ou instância, tem um Estado de funcionamento continuamente atualizável. O estado de funcionamento pode ser "OK", "Aviso" ou "Erro". Considere eventos do Service Fabric como verbos feitos pelo cluster para várias entidades e estado de funcionamento como um adjetivo para cada entidade. Sempre que faz a transição do Estado de funcionamento de uma entidade específica, um evento também será emitido. Desta forma, que pode configurar alertas para eventos de estado de funcionamento e de consultas na sua ferramenta de monitoramento de escolha, tal como qualquer outro evento. 

Além disso, podemos até mesmo permitir que os utilizadores substituir o estado de funcionamento de entidades. Se a sua aplicação está a passar por uma atualização e tiver testes de validação falhar, pode escrever para o estado de funcionamento do Service Fabric com a API de estado de funcionamento para indicar a sua aplicação já não está em bom estada e recursos de infraestrutura do serviço será automaticamente reverter a atualização! Para saber mais sobre o modelo de estado de funcionamento, confira o [introdução à monitorização de estado de funcionamento do Service Fabric](service-fabric-health-introduction.md)

![Dashboard de estado de funcionamento SFX](media/service-fabric-diagnostics-overview/sfx-healthstatus.png)


### <a name="watchdogs"></a>Watchdogs
Em geral, um watchdog é um serviço separado que pode ver o estado de funcionamento e de carga em serviços, pontos finais de ping e estado de funcionamento do relatório para qualquer coisa no cluster. Isso pode ajudar a evitar erros que não seriam detetados com base na vista de um único serviço. Watchdogs também são um bom lugar para código de anfitrião que executa as ações corretivas sem interação do utilizador (por exemplo, a limpeza dos ficheiros de registo no armazenamento em determinados intervalos de tempo). Pode encontrar uma implementação de serviços de watchdog de exemplo [aqui](https://github.com/Azure-Samples/service-fabric-watchdog-service).

## <a name="infrastructure-performance-monitoring"></a>Monitoramento da infraestrutura (desempenho)
Agora que já abordamos os diagnósticos no seu aplicativo e a plataforma, como sabemos que o hardware esteja funcionando como esperado? Monitorizar a sua infraestrutura subjacente é uma parte fundamental do Noções básicas sobre o estado do seu cluster e a utilização de recursos. Avaliando o desempenho do sistema depende de muitos fatores que podem ser subjetivas, dependendo de suas cargas de trabalho. Esses fatores geralmente são avaliados através de contadores de desempenho. Estes contadores de desempenho podem vir de várias origens, incluindo o sistema operativo, o .NET framework ou própria plataforma do Service Fabric. Alguns cenários em que seriam úteis são

* Estou eu utilizando o meu hardware com eficiência? Que pretende utilizar o seu hardware em 90% da CPU ou de 10% da CPU. Isso é útil quando dimensionar o cluster, ou a otimização de processos da sua aplicação.
* Posso prever problemas de infraestrutura proativamente? -Muitos problemas são precedidos por alterações repentinas (cai) no desempenho, pelo que pode utilizar contadores de desempenho, tais como a utilização de e/s e CPU para prever e diagnosticar os problemas de forma proativa de rede.

Uma lista de contadores de desempenho que devem ser recolhidos no nível de infraestrutura pode ser encontrada em [métricas de desempenho](service-fabric-diagnostics-event-generation-perf.md). 

Alsp do Service Fabric fornece um conjunto de contadores de desempenho para os modelos de programação Reliable Services e Atores. Se estiver a utilizar qualquer um desses modelos, estes contadores de desempenho podem informações para garantir que seus atores são girando corretamente e reduzir verticalmente, de que os pedidos de serviço fiável estão a ser processados rápido o bastante. Para obter mais informações, consulte [monitorização para Reliable Service Remoting](service-fabric-reliable-serviceremoting-diagnostics.md#performance-counters) e [monitorização de desempenho para Reliable Actors](service-fabric-reliable-actors-diagnostics.md#performance-counters). 

A solução do Azure Monitor para recolher estas é o Log Analytics como monitorização ao nível da plataforma. Deve utilizar o [agente do Log Analytics](service-fabric-diagnostics-oms-agent.md) para recolher os contadores de desempenho adequado e visualizá-los no Log Analytics.

## <a name="recommended-setup"></a>Configuração recomendada
Agora que estamos já Passei de cada área de cenários de monitorização e de exemplo, aqui está um resumo do Azure, ferramentas de monitorização e configurar necessários para monitorizar todas as áreas de acima. 

* Monitorização de aplicações com [Application Insights](service-fabric-tutorial-monitoring-aspnet.md)
* Monitorização de clusters com [agente de diagnóstico](service-fabric-diagnostics-event-aggregation-wad.md) e [do Log Analytics](service-fabric-diagnostics-oms-setup.md)
* Monitorização da infraestrutura com [do Log Analytics](service-fabric-diagnostics-oms-agent.md)

Também pode usar e modificar o modelo de ARM de exemplo localizado [aqui](service-fabric-diagnostics-oms-setup.md#deploy-log-analytics-with-azure-resource-manager) para automatizar a implantação de todos os recursos necessários e agentes. 

## <a name="other-logging-solutions"></a>Outras soluções de registo

Embora as duas soluções recomendamos, [do Azure Log Analytics](service-fabric-diagnostics-event-analysis-oms.md) e [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) criaram na integração com o Service Fabric, muitos eventos são escritos por meio de provedores do ETW e são extensível com outras soluções de registo. Também deve examinar a [Elastic Stack](https://www.elastic.co/products) (especialmente se estiver a considerar a executar um cluster num ambiente offline), [Dynatrace](https://www.dynatrace.com/), ou qualquer outra plataforma de sua preferência. Temos uma lista de parceiros integrados disponíveis [aqui](service-fabric-diagnostics-partners.md).

Os principais pontos para qualquer plataforma que escolher devem incluir como confortável estiver com a interface do usuário, recursos de consulta, as visualizações personalizadas e dashboards disponíveis e as ferramentas adicionais fornecem para aprimorar sua experiência de monitorização. 

## <a name="next-steps"></a>Passos Seguintes

* Para começar a trabalhar com instrumentar seus aplicativos, consulte [geração de eventos e de registo ao nível no aplicativo](service-fabric-diagnostics-event-generation-app.md).
* Avance para os passos para configurar o Application Insights para a sua aplicação com [monitorizar e diagnosticar uma aplicação ASP.NET Core no Service Fabric](service-fabric-tutorial-monitoring-aspnet.md).
* Saiba mais sobre a monitorização da plataforma e os eventos que o Service Fabric fornece para na [geração de eventos e de registo ao nível no plataforma](service-fabric-diagnostics-event-generation-infra.md).
* Configurar a integração do Log Analytics com o Service Fabric em [configurar o Log Analytics para um cluster](service-fabric-diagnostics-oms-setup.md)
* Saiba como configurar o Log Analytics para monitorizar contentores- [monitorização e diagnósticos para contentores do Windows no Azure Service Fabric](service-fabric-tutorial-monitoring-wincontainers.md).
* Veja os problemas de diagnóstico de exemplo e soluções com o Service Fabric no [cenários comuns de diagnóstico](service-fabric-diagnostics-common-scenarios.md)
* Check-out de outros produtos de diagnóstico que se integram com o Service Fabric no [parceiros de diagnóstico do Service Fabric](service-fabric-diagnostics-partners.md)
* Saiba mais sobre as recomendações gerais da monitorização para recursos do Azure - [práticas recomendadas – a monitorização e diagnóstico](https://docs.microsoft.com/azure/architecture/best-practices/monitoring). 