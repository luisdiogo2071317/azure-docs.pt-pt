---
title: "Monitorização de infraestrutura de serviço do Azure e a descrição geral de diagnóstico | Microsoft Docs"
description: "Saiba mais sobre a monitorização e diagnóstico para clusters, aplicações e serviços do Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2018
ms.author: dekapur
ms.openlocfilehash: 43a45a31efffcd623e6381049876c3607663ec4f
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2018
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Monitorização e diagnóstico para o Azure Service Fabric

Este artigo fornece uma descrição geral de como configurar a monitorização e diagnóstico para as suas aplicações em execução em clusters de Service Fabric. Monitorização e diagnóstico é cruciais para desenvolver, testar e implementar cargas de trabalho em qualquer ambiente de nuvem. Monitorização permite-lhe controlar a forma como as aplicações são utilizadas, a utilização de recursos e o estado de funcionamento geral do cluster. Pode utilizar estas informações para diagnosticar e corrigir quaisquer problemas no cluster e para ajudar a evitar problemas aconteça no futuro. 

Os objetivos principais da monitorização e diagnóstico são:
* Detetar e diagnosticar problemas de infraestrutura
* Detetar problemas com a sua aplicação
* Compreender o consumo de recursos
* Monitorizar o desempenho de aplicações, serviço e infraestrutura

Num cluster de Service Fabric, os dados de monitorização e diagnóstico provém de três níveis: aplicação, a plataforma (cluster) e a infraestrutura. 
* O [nível aplicação](service-fabric-diagnostics-event-generation-app.md) inclui dados sobre o desempenho das suas aplicações e qualquer registo personalizado adicional que tenha adicionado. Dados de monitorização de aplicações devem terminar a cópia de segurança no Application Insights (AI) da aplicação em si. Podem ter a através do SDK de AI, EventFlow ou outro pipeline à sua escolha.
* O [nível de plataforma](service-fabric-diagnostics-event-generation-infra.md) inclui eventos de ações que está a ser executadas no seu cluster, relacionadas com a gestão do cluster e as aplicações em execução no mesmo. Dados de monitorização de plataforma devem ser enviados para análise de registos do OMS, com a solução de análise de recursos de infraestrutura de serviço para o ajudar a visualizar os eventos de entrada. Estes dados são normalmente enviados para uma conta de armazenamento através da extensão do Windows ou Linux do diagnóstico do Azure, de onde é acedida através da análise de registos do OMS. 
* O nível de infraestrutura centra-se nos [monitorização do desempenho](service-fabric-diagnostics-event-generation-perf.md), observar as métricas-chave e os contadores de desempenho para determinar a utilização de recursos e a carga. Monitorização de desempenho pode ser conseguida através da utilização de um agente - Recomendamos a utilização do agente do OMS (Microsoft Monitoring), para que os dados de desempenho termina no mesmo local como os eventos de plataforma que permite uma melhor experiência de diagnóstico que lhe correlacionar alterações através de um cluster. 

![Gráfico de descrição geral de diagnóstico](media/service-fabric-diagnostics-overview/diagnostics-overview.png)

## <a name="monitoring-scenarios"></a>Cenários de monitorização

Esta secção descreve cenários-chave para um cluster do Service Fabric - monitorização de aplicações, de cluster, desempenho e a monitorização de estado de funcionamento. Para cada cenário, é abordada a abordagem de intenção e geral para monitorização. Obter mais detalhes sobre estas e outras recomendações gerais de monitorização de recursos do Azure podem ser encontrados em [melhores práticas - monitorização e diagnóstico](https://docs.microsoft.com/azure/architecture/best-practices/monitoring). 

Estes cenários também aproximadamente ligado estão mapeados para os três níveis de dados de monitorização e diagnóstico, conforme mencionado acima, ou seja, para cada cenário a serem processados corretamente no cluster, deve ter dados de monitorização e diagnóstico disponíveis no nível correspondente . O cenário de monitorização de estado de funcionamento é uma exceção, uma vez que abranger o cluster e tudo em execução no mesmo.

## <a name="application-monitoring"></a>Monitorização de aplicações
Monitorização de aplicações controla a forma como as funcionalidades e os componentes de uma aplicação que criou, estão a ser utilizados. Pretende monitorizar as aplicações para tornar problemas se de que os utilizadores são detetados de impacto. Monitorizar as aplicações pode ser úteis:
* determinar o carregamento da aplicação e o utilizador tráfego - precisa de aumentar os seus serviços para satisfazer os pedidos de utilizador ou o endereço um estrangulamento potencial na sua aplicação?
* identificar problemas com comunicação de serviço e gestão remota do cluster
* perceber a que os utilizadores estão a fazer com a sua aplicação - instrumentação as aplicações que pode ajudar a guia funcionalidade futuras desenvolvimento e um melhor diagnóstico erros de aplicação

Para monitorizar ao nível da aplicação no Service Fabric, recomendamos que utilize o Application Insights (AI). Integração do AI com o Service Fabric inclui ferramentas experiências do Visual Studio e o portal do Azure e o conhecimento do contexto de serviço do Service Fabric e gestão remota no dashboard de AI e o mapeamento de aplicações, originando um registo de out of box abrangente experiência. Apesar de muitas registos são criados e recolhidos para, quando utilizar AI automaticamente, recomendamos que adicione mais registo personalizado para as suas aplicações e ter que aparecem no AI juntamente com o que é fornecido para criar uma experiência mais rica de diagnóstico para lidar com problemas no futuro. Ver mais informações sobre como utilizar AI com o Service Fabric no [análise de eventos com o Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md). 

Para começar a instrumentação código para monitorizar as aplicações, aceda a [geração de registo e evento nível no aplicação](service-fabric-diagnostics-event-generation-app.md). 

### <a name="monitoring-application-availability"></a>Monitorização de disponibilidade de aplicações
É possível que todo o seu cluster parece estar em execução conforme esperado e está a comunicar como bom estado de funcionamento, mas as suas aplicações parecem unaccessible ou inacessível. Apesar de não existirem muitos casos, isto iria acontecer, é importante saber quando isto acontece conseguir resolver logo que possível. Monitorização de disponibilidade amplamente está preocupada com a disponibilidade de componentes do sistema para compreender o geral "tempo de atividade" do sistema de controlo. Num cluster, iremos focar-se na disponibilidade a partir da perspetiva da aplicação - funciona a plataforma para garantir que esse ciclo de vida de aplicação cenários de gestão não causar período de indisponibilidade. Nonetheless, vários problemas do cluster podem resultar em com impacto na disponibilidade da aplicação e nestes casos, como um proprietário da aplicação, normalmente, pretende saber de imediato. Recomendamos que juntamente com todas as outras aplicações, implemente um Watchdog no seu cluster. O objetivo de um watchdog do seria Verifique os pontos finais adequados para a sua aplicação num intervalo de tempo definido e devolver relatórios que estão acessíveis. Também pode fazer isto, utilizando um serviço externo que efetua o ping o ponto final e devolve um relatório no intervalo de tempo especificado.

## <a name="cluster-monitoring"></a>Monitorização de cluster
Monitorização do seu cluster do Service Fabric é fundamental para se certificar de que a plataforma e todas as cargas de trabalho em execução no mesmo estão a ser executado conforme pretendido. Um dos objetivos de Service Fabric é manter as aplicações em execução através de falhas de hardware. Isto é conseguido através da capacidade dos serviços do sistema a plataforma para detetar a infraestrutura e os problemas rapidamente as cargas de trabalho de ativação pós-falha para outros nós do cluster. Mas neste caso específica, e se os serviços de sistema próprios têm problemas? Ou, em caso de tentativa de mover uma carga de trabalho, são violou as regras para o posicionamento dos serviços? O cluster de monitorização permite-lhe para se manter informado sobre a atividade a decorrer no seu cluster, o que ajuda a diagnosticar problemas e corrigi-los de forma eficaz. Alguns pontos chaves que pretende procurar são:
* Service Fabric está a comportar da forma esperada, em termos de balanceamento de trabalho em todo o cluster e colocar as aplicações? 
* Estão a ser executadas ações para modificar a configuração do cluster que está a ser confirmada e atuou como esperado? Este aspeto é especialmente relevante quando o dimensionamento de um cluster.
* Está Service Fabric a processar os dados e a comunicação de serviço do serviço dentro do cluster corretamente?

O Service Fabric fornece um conjunto abrangente de eventos a Box, através dos canais operacional e o serviço de mensagens & dados. No Windows, estes são sob a forma de um único fornecedor ETW com um conjunto de relevantes `logLevelKeywordFilters` utilizado para escolher entre diferentes canais. No Linux, todos os eventos de plataforma passem por LTTng e são colocados uma tabela, a partir de onde podem ser filtradas conforme necessário. 

Estes canais contenham organizados, estruturados eventos que podem ser utilizados para melhor compreender o estado do cluster. "O diagnóstico" está ativado por predefinição no momento da criação do cluster, que configurar, com uma tabela de armazenamento do Azure onde dos eventos estes canais são enviados para a consulta no futuro. Pode ler mais sobre como monitorizar o seu cluster em [geração de registo e evento nível no plataforma](service-fabric-diagnostics-event-generation-infra.md). Recomendamos que utilize a análise de registos do OMS para monitorizar o seu cluster. Análise de registos do OMS oferece uma solução específica do Service Fabric, análise de recursos de infraestrutura de serviço, que fornece um dashboard personalizado para monitorização de clusters de Service Fabric e permite-lhe consultar eventos do cluster e configurar alertas. Saiba mais sobre no [análise de eventos com o OMS](service-fabric-diagnostics-event-analysis-oms.md). 

### <a name="watchdogs"></a>Watchdogs
Geralmente, um watchdog do é um serviço separado que pode ver o estado de funcionamento e carregar através de serviços, pontos finais de ping e estado de funcionamento do relatório para tudo no cluster. Isto pode ajudar a evitar erros não seriam possível detetar com base na vista de um único serviço. Watchdogs também são um bom local para o código de anfitrião que executa toma ações sem interação do utilizador (por exemplo, da limpeza dos ficheiros de registo no armazenamento em determinados intervalos de tempo). Pode encontrar uma implementação de serviço do exemplo watchdog [aqui](https://github.com/Azure-Samples/service-fabric-watchdog-service).

## <a name="performance-monitoring"></a>Monitorização de desempenho
Monitorizar a infraestrutura subjacente é uma parte de chave de compreender o estado do seu cluster e a utilização de recursos. Medir o desempenho do sistema depende de vários fatores, cada um dos quais é medida através de um indicadores de desempenho (KPIs) de chave. KPIs relevantes do Service Fabric podem ser mapeadas para as métricas que podem ser recolhidas a partir de nós do cluster, como os contadores de desempenho.
Estes KPIs podem ajudar a:
* Compreender a utilização de recursos e carga - com o objetivo dimensionar o seu cluster ou a otimizar os processos de serviço
* problemas de infraestrutura - de prever muitos problemas são precedidos por alterações repentino (remoções) no desempenho, pelo que pode utilizar KPIs, tais como a utilização de e/s e CPU, de rede para prever e diagnosticar problemas de infraestrutura

Uma lista de contadores de desempenho que devem ser recolhidas ao nível da infraestrutura pode ser encontrada em [métricas de desempenho](service-fabric-diagnostics-event-generation-perf.md). 

Para a monitorização de desempenho de nível de aplicação, o Service Fabric fornece um conjunto de contadores de desempenho para os modelos de programação Reliable Services e Atores. Se estiver a utilizar qualquer um destes modelos, estes contadores de desempenho podem fornecer KPIs que ajudam a garantir que os atores são girar e reduzir verticalmente corretamente ou que os pedidos de serviço fiável estão a ser processados suficientemente rápida. Consulte [monitorização para a gestão remota de serviço fiável](service-fabric-reliable-serviceremoting-diagnostics.md#performance-counters) e [monitorização do desempenho dos Reliable Actors](service-fabric-reliable-actors-diagnostics.md#performance-counters) para obter mais informações sobre estes. Além disto, o Application Insights tem também um conjunto de métricas de desempenho irá recolher, se configurado com a sua aplicação.

Utilize o agente do OMS para recolher os contadores de desempenho adequada e ver estes KPIs na análise de registos do OMS. Também pode utilizar a extensão de agente de diagnóstico do Azure (ou qualquer outro agente semelhante) para recolher e armazenar as métricas para análise. 

## <a name="health-monitoring"></a>Monitorização do estado de funcionamento
A plataforma de Service Fabric inclui um modelo de estado de funcionamento, que fornece o estado de funcionamento extensível relatórios para o estado de entidades num cluster. Cada nó, da aplicação, serviço, partição, réplica ou instância, tem um Estado de funcionamento continuamente atualizável. O estado de funcionamento pode ser "OK", "Aviso" ou "Error". O estado de funcionamento é alterado através de relatórios de estado de funcionamento que são emitidos para cada entidade, com base nos problemas no cluster. O estado de funcionamento das suas entidades pode ser verificado em qualquer altura no Explorador de recursos de infraestrutura de serviço (SFX), conforme mostrado abaixo, ou pode ser consultado através de API de estado de funcionamento as plataformas. Também pode personalizar os relatórios de estado de funcionamento e modificar o estado de funcionamento de uma entidade adicionando os seus próprios relatórios de estado de funcionamento ou utilizando a API de estado de funcionamento. Obter mais detalhes sobre o modelo de estado de funcionamento podem ser encontrados em [introdução à monitorização de estado de funcionamento do Service Fabric](service-fabric-health-introduction.md).

![Dashboard de estado de funcionamento SFX](media/service-fabric-diagnostics-overview/sfx-healthstatus.png)

Para além de ver relatórios de estado de funcionamento mais recentes no SFX, cada relatório também está disponível como um evento. Eventos de estado de funcionamento podem ser recolhidos através do canal operacional (consulte [agregação de evento com o Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md#collect-health-and-load-events)) e armazenadas na análise de registos do OMS para alertas e consultar no futuro. Isto ajuda a detetar problemas que possam afetar a disponibilidade de aplicações, pelo que recomendamos que configure alertas para cenários de falha adequado (alertas personalizados através do OMS).

## <a name="monitoring-workflow"></a>Fluxo de trabalho de monitorização 

O fluxo de trabalho geral de monitorização e diagnóstico consiste em três passos:

1. **Geração de eventos**: Isto inclui eventos (registos de rastreios, eventos personalizados) ao nível da infraestrutura, a plataforma (cluster) e a aplicação
2. **Agregação de evento**: nesta fase é efetivamente o pipeline para os eventos acabar numa ferramenta onde poder analisá-las, que inclui a extensão de diagnóstico do Azure ou EventFlow
3. **Análise**: eventos tem de estar acessível numa ferramenta, para permitir a análise - visualização, consultar, alertas, etc.

São vários produtos disponíveis que abrangem estes três áreas e estiver livre para escolher as diferentes tecnologias para cada. É importante certificar-se de que as peças vários funcionam em conjunto para fornecer uma solução de monitorização ponto a ponto para o cluster.

## <a name="event-generation"></a>Geração de eventos

É o primeiro passo no fluxo de trabalho de monitorização e diagnóstico para configurar a criação e a geração de dados de eventos e registo. Estes eventos, os registos e os contadores de desempenho são emitidos em todos os três níveis: o nível de aplicação (qualquer instrumentação adicionado às aplicações e serviços implementados em cluster), a plataforma (eventos emitidos do cluster com base na operação de Service Fabric), e o nível de infraestrutura (métricas de desempenho de cada nó). Dados de diagnóstico que são recolhidos em cada um destes níveis são personalizáveis, apesar de Service Fabric ativar algumas instrumentação de predefinição. 

Leia mais sobre [eventos ao nível da plataforma](service-fabric-diagnostics-event-generation-infra.md) e [eventos ao nível da aplicação](service-fabric-diagnostics-event-generation-app.md) para compreender o que é fornecido e como adicionar mais instrumentação. A decisão principal, que é necessário efetuar aqui está como pretende instrumente a sua aplicação. Para aplicações de .NET, recomendamos que utilize ILogger, mas também pode explorar EventSource, Serilog e outras bibliotecas semelhantes. Para Java, recomendamos que utilize Log4j. Para além destes, existem várias outras opções disponíveis que podem ser utilizadas com base na natureza da aplicação. À vontade explorar que seria ser melhor para o caso de utilização específicos, ou escolha uma que é mais confortável com a utilizar. 

Depois de tomar uma decisão no fornecedor de registo que pretende utilizar, tem de certificar-se de que os registos estão a ser agregada e armazenados corretamente.

## <a name="event-aggregation"></a>Agregação de eventos

Para recolher os registos e eventos gerados pelas suas aplicações e o cluster, em geral, recomendamos que utilize o [extensão de diagnóstico do Azure](service-fabric-diagnostics-event-aggregation-wad.md) (mais semelhantes a recolha de registos com base no agente) ou [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) (no processo de registo coleção). Se estiver a utilizar o Application Insights e estiver a desenvolver no .NET ou Java, é recomendado, em seguida, utilizar o Application Insights SDK em vez de EventFlow.

Embora seja possível obter uma tarefa semelhante terminada com apenas um deles, na maioria das situações, combinando o agente de extensão de diagnóstico do Azure com um pipeline no processo de recolha (AI SDK ou EventFlow) leva a um fluxo de trabalho mais fiável abrangente, a monitorização . O agente de extensão de diagnóstico do Azure será o caminho para os eventos de nível de plataforma, ao utilizar o SDK de AI ou EventFlow (no processo de recolha) para os registos de nível de aplicação. 

No caso de que pretende utilizar apenas um destes, eis alguns pontos chaves a lembrar.
* Recolher registos de aplicações através da extensão de diagnóstico do Azure é uma boa opção para serviços do Service Fabric se o conjunto de origens de registo e destinos não são alterados frequentemente e não existe um mapeamento entre as origens e os respetivos destinos simples. O motivo para isto está a configurar o Azure Diagnostics acontece na camada do Gestor de recursos, para que efetuar alterações significativas a configuração requer a atualização do cluster completo. Isto significa que, muitas vezes, termina a ser menos eficientes do que vai com o SDK de AI ou EventFlow.
* Utilizar EventFlow permite-lhe ter serviços enviar registos diretamente a uma plataforma de análise e visualização de e/ou ao armazenamento. Outras bibliotecas (ILogger Serilog, etc.) também podem ser utilizadas para a mesma finalidade, mas EventFlow tem a vantagem de ter sido concebidas especificamente para recolha de registos no processo e para suportar os serviços do Service Fabric. Isto tende a ter várias vantagens em termos de Facilidade de implementação e configuração e oferece uma maior flexibilidade para suportar a outro registo bibliotecas e ferramentas de análise. 

## <a name="event-analysis"></a>Análise de eventos

Existem várias plataformas excelente que existem no mercado quando se trata de análise e visualização de dados de monitorização e diagnóstico. Os dois que recomendamos são [OMS](service-fabric-diagnostics-event-analysis-oms.md) e [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) devido a sua integração com o Service Fabric. Também deve consultar para o [pilha elástico](https://www.elastic.co/products) (especialmente se estiver a considerar a executar um cluster num ambiente offline), [Splunk](https://www.splunk.com/), ou qualquer outra plataforma da sua preferência. 

Os pontos de chaves para qualquer plataforma que escolher devem incluir como confortável são com a interface de utilizador e as opções de consultas, a capacidade de visualizar dados e criar dashboards facilmente legíveis e as ferramentas adicionais fornecem para melhorar a monitorização, tais como automatizada alertas.

Para além da plataforma que escolher, quando configurar um cluster do Service Fabric como um recurso do Azure, também ter acesso a out of box monitorização de desempenho do Azure para as máquinas.

### <a name="azure-monitor"></a>Azure Monitor

Pode utilizar [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md) monitorizar muitos dos recursos do Azure no qual baseia-se um cluster do Service Fabric. Um conjunto de métricas para o [conjunto de dimensionamento da máquina virtual](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets) e individuais [máquinas virtuais](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesetsvirtualmachines) são recolhidos automaticamente e apresentado no portal do Azure. Para ver as informações recolhidas, no portal do Azure, selecione o grupo de recursos que contém o cluster do Service Fabric. Em seguida, selecione o conjunto de dimensionamento de máquina virtual que pretende visualizar. No **monitorização** secção (na navegação esquerda), selecione **métricas** para ver um gráfico dos valores.

![Vista do portal do Azure das informações recolhidas métricas](media/service-fabric-diagnostics-overview/azure-monitoring-metrics.png)

Para personalizar os gráficos, siga as instruções em [métricas no Microsoft Azure](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md). Também pode criar alertas com base nestas métricas, conforme descrito em [criar alertas no Monitor do Azure para serviços do Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md). 

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre a monitorização da plataforma e os eventos de Service Fabric fornece para si no [geração de registo e evento nível no plataforma](service-fabric-diagnostics-event-generation-infra.md)
* Para começar a trabalhar com instrumentação as suas aplicações, consulte [geração de registo e evento nível no aplicação](service-fabric-diagnostics-event-generation-app.md)
* Seguir o tutorial [monitorizar e diagnosticar uma aplicação ASP.NET Core no Service Fabric](service-fabric-tutorial-monitoring-aspnet.md)

