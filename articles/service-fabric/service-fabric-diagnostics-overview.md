---
title: Monitorização de infraestrutura de serviço do Azure e a descrição geral de diagnóstico | Microsoft Docs
description: Saiba mais sobre a monitorização e diagnóstico para clusters, aplicações e serviços do Azure Service Fabric.
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
ms.date: 04/03/2018
ms.author: dekapur;srrengar
ms.openlocfilehash: 03fa2862bbce39ac9ee6b7da02bd93b02b05f216
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Monitorização e diagnóstico para o Azure Service Fabric

Este artigo fornece uma descrição geral da monitorização e diagnóstico para o Azure Service Fabric. Monitorização e diagnóstico é cruciais para desenvolver, testar e implementar cargas de trabalho em qualquer ambiente de nuvem. Monitorização permite-lhe controlar a forma como as aplicações são utilizadas, a utilização de recursos e o estado de funcionamento geral do cluster. Pode utilizar estas informações para diagnosticar e corrigir quaisquer problemas e evitar problemas aconteça no futuro. 

## <a name="application-monitoring"></a>Monitorização de aplicações
Monitorização de aplicações controla a forma como as funcionalidades e os componentes da aplicação estão a ser utilizados. Pretende monitorizar as aplicações para tornar problemas se de que os utilizadores são detetados de impacto. Monitorização de aplicações pode ser útil nos seguintes cenários:
* determinar o carregamento da aplicação e o utilizador tráfego - precisa de aumentar os seus serviços para satisfazer os pedidos de utilizador ou o endereço um estrangulamento potencial na sua aplicação?
* identificar problemas com comunicação de serviço e gestão remota do cluster
* Perceber a que os utilizadores estão a fazer com a sua aplicação - recolher telemetria nas aplicações que pode ajudar a guia funcionalidade futuras desenvolvimento e um melhor diagnóstico erros de aplicação
* Monitorização que está a acontecer no interior dos contentores em execução

Service Fabric suporta muitas opções para instrumentar código da aplicação com a telemetria e rastreios adequados. Recomendamos que utilize o Application Insights (AI). Integração do AI com o Service Fabric inclui ferramentas experiências para Visual Studio e o Azure portal, bem como métricas específicas do Service Fabric, fornecer uma experiência de registo de out of box abrangente. Apesar de muitas registos são criados e recolhidos por si com AI automaticamente, recomendamos que adicione mais registo personalizado às suas aplicações para criar uma experiência mais rica de diagnóstico. Ver mais informações sobre começar a utilizar com o Application Insights com o Service Fabric no [análise de eventos com o Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md).

![Detalhes de rastreio de IA](./media/service-fabric-tutorial-monitoring-aspnet/trace-details.png)

## <a name="platform-cluster-monitoring"></a>Monitorização de plataforma (Cluster)
Monitorização do seu cluster do Service Fabric é fundamental para se certificar de que a plataforma e todas as cargas de trabalho estão a ser executado conforme pretendido. Um dos objetivos de Service Fabric é manter as aplicações sejam resilientes a falhas de hardware. Este objetivo é conseguido através da capacidade dos serviços do sistema a plataforma para detetar a infraestrutura e os problemas rapidamente as cargas de trabalho de ativação pós-falha para outros nós do cluster. Mas neste caso específica, e se os serviços de sistema próprios têm problemas? Ou, em caso de tentativa de mover uma carga de trabalho, são violou as regras para o posicionamento dos serviços? O cluster de monitorização permite-lhe para se manter informado sobre a atividade a decorrer no seu cluster, o que ajuda a diagnosticar problemas e corrigi-los de forma eficaz. Alguns pontos chaves que pretende procurar são:
* Service Fabric está a comportar da forma esperada, em termos de balanceamento de trabalho em todo o cluster e colocar as aplicações? 
* As ações do utilizador são executadas no seu cluster confirmados e executado num como esperado? Este aspeto é especialmente relevante quando o dimensionamento de um cluster.
* Está Service Fabric a processar os dados e a comunicação de serviço do serviço dentro do cluster corretamente?

O Service Fabric fornece um conjunto abrangente de eventos a Box, através dos canais operacional e o serviço de mensagens & dados. No Windows, estes são sob a forma de um único fornecedor ETW com um conjunto de relevantes `logLevelKeywordFilters` utilizado para escolher entre diferentes canais. No Linux, todos os eventos de plataforma passem por LTTng e são colocados uma tabela, a partir de onde podem ser filtradas conforme necessário. 

Estes canais contenham organizados, estruturados eventos que podem ser utilizados para melhor compreender o estado do cluster. Diagnóstico está ativado por predefinição o do momento de criação do cluster, que criar uma tabela de armazenamento do Azure onde dos eventos estes canais são enviados para a consulta no futuro. Pode ler mais sobre como monitorizar o seu cluster em [geração de registo e evento nível no plataforma](service-fabric-diagnostics-event-generation-infra.md).

Para recolher os registos e eventos gerados pelo seu cluster, em geral, recomendamos que utilize o [extensão de diagnóstico do Azure](service-fabric-diagnostics-event-aggregation-wad.md). Isto integra-se bem solução específica do OMS a recursos de infraestrutura de serviço de análise do registo, análise de recursos de infraestrutura de serviço, que fornece um dashboard personalizado para monitorização de clusters de Service Fabric e permite-lhe consultar eventos do cluster e configurar alertas. Saiba mais sobre no [análise de eventos com o OMS](service-fabric-diagnostics-event-analysis-oms.md). 

 ![Solução SF do OMS](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-solution.png)

## <a name="performance-monitoring"></a>Monitorização de desempenho
Monitorizar a infraestrutura subjacente é uma parte de chave de compreender o estado do seu cluster e a utilização de recursos. Medir o desempenho do sistema depende de vários fatores, cada um dos quais é medida através de um indicadores de desempenho (KPIs) de chave. KPIs relevantes do Service Fabric podem ser mapeadas para as métricas que podem ser recolhidas a partir de nós do cluster, como os contadores de desempenho.
Estes KPIs podem ajudar a:
* Compreender a utilização de recursos e a carga - com o objetivo dimensionar o seu cluster ou a otimizar os processos de serviço.
* Problemas de infraestrutura - de prever muitos problemas são precedidos por alterações repentino (remoções) no desempenho, pelo que pode utilizar KPIs, tais como a utilização de e/s e CPU de rede para prever e diagnosticar problemas de infraestrutura.

Uma lista de contadores de desempenho que devem ser recolhidas ao nível da infraestrutura pode ser encontrada em [métricas de desempenho](service-fabric-diagnostics-event-generation-perf.md). 

O Service Fabric fornece um conjunto de contadores de desempenho para os modelos de programação Reliable Services e Atores. Se estiver a utilizar qualquer um destes modelos, estes contadores de desempenho podem fornecer KPIs que ajudam a garantir que os atores são girar e reduzir verticalmente corretamente ou que os pedidos de serviço fiável estão a ser processados suficientemente rápida. Para obter mais informações, consulte [monitorização para a gestão remota de serviço fiável](service-fabric-reliable-serviceremoting-diagnostics.md#performance-counters) e [monitorização do desempenho dos Reliable Actors](service-fabric-reliable-actors-diagnostics.md#performance-counters). Além disto, o Application Insights tem também um conjunto de métricas de desempenho irá recolher, se configurado com a sua aplicação.

Utilize o [agente do OMS](service-fabric-diagnostics-oms-agent.md) para recolher os contadores de desempenho adequadas e ver estes KPIs na análise de registos do OMS.

![Gráfico de descrição geral de diagnóstico](media/service-fabric-diagnostics-overview/diagnostics-overview.png)

## <a name="health-monitoring"></a>Monitorização do estado de funcionamento
A plataforma de Service Fabric inclui um modelo de estado de funcionamento, que fornece o estado de funcionamento extensível relatórios para o estado de entidades num cluster. Cada nó, da aplicação, serviço, partição, réplica ou instância, tem um Estado de funcionamento continuamente atualizável. O estado de funcionamento pode ser "OK", "Aviso" ou "Error". O estado de funcionamento é alterado através de relatórios de estado de funcionamento que são emitidos para cada entidade, com base nos problemas no cluster. O estado de funcionamento das suas entidades pode ser verificado em qualquer altura no Explorador de recursos de infraestrutura de serviço (SFX), conforme mostrado abaixo, ou pode ser consultado através de API de estado de funcionamento as plataformas. Também pode personalizar os relatórios de estado de funcionamento e modificar o estado de funcionamento de uma entidade adicionando os seus próprios relatórios de estado de funcionamento ou utilizando a API de estado de funcionamento. Obter mais detalhes sobre o modelo de estado de funcionamento podem ser encontrados em [introdução à monitorização de estado de funcionamento do Service Fabric](service-fabric-health-introduction.md).

![Dashboard de estado de funcionamento SFX](media/service-fabric-diagnostics-overview/sfx-healthstatus.png)

Para além de ver relatórios de estado de funcionamento mais recentes no SFX, cada relatório também está disponível como um evento. Eventos de estado de funcionamento podem ser recolhidos através do canal operacional (consulte [agregação de evento com o Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations)) e armazenadas na análise de registos para alertas e consultar no futuro. Isto ajuda a detetar problemas que possam afetar a disponibilidade de aplicações, pelo que recomendamos que configure alertas para cenários de falha adequado (alertas personalizados através de Log Analytics).

## <a name="other-logging-solutions"></a>Outras soluções de registo

Embora as soluções de dois, recomendamos, [OMS](service-fabric-diagnostics-event-analysis-oms.md) e [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) criada na integração com o Service Fabric, muitos eventos são escritos através de fornecedores de etw em são extensíveis com outros soluções de registo. Também deve consultar para o [pilha elástico](https://www.elastic.co/products) (especialmente se estiver a considerar a executar um cluster num ambiente offline), [Splunk](https://www.splunk.com/), [Dynatrace](https://www.dynatrace.com/), ou qualquer outro plataforma da sua preferência. 

Os pontos de chaves para qualquer plataforma que escolher devem incluir como confortável são com a interface de utilizador e as opções de consultas, a capacidade de visualizar dados e criar dashboards facilmente legíveis e as ferramentas adicionais fornecem para melhorar a monitorização, tais como automatizada alertas.

## <a name="next-steps"></a>Passos Seguintes

* Para começar a trabalhar com instrumentação as suas aplicações, consulte [geração de registo e evento nível no aplicação](service-fabric-diagnostics-event-generation-app.md).
* Saiba mais sobre a monitorização da plataforma e os eventos de Service Fabric fornece para si no [geração de registo e evento nível no plataforma](service-fabric-diagnostics-event-generation-infra.md).
* Aceda os passos para configurar AI para a sua aplicação com [monitorizar e diagnosticar uma aplicação ASP.NET Core no Service Fabric](service-fabric-tutorial-monitoring-aspnet.md).
* Saiba como configurar a análise de registos do OMS para monitorização contentores- [monitorização e diagnóstico para o Windows contentores no Azure Service Fabric](service-fabric-tutorial-monitoring-wincontainers.md).
* Saiba mais sobre as recomendações gerais de monitorização de recursos do Azure - [melhores práticas - monitorização e diagnóstico](https://docs.microsoft.com/azure/architecture/best-practices/monitoring). 
