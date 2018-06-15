---
title: Monitorização de recursos e aplicações do Azure | Microsoft Docs
description: Descrição geral dos serviços da Microsoft e funcionalidades que contribuem para uma estratégia completa de monitorização para os seus serviços do Azure e aplicações.
author: bwren
manager: carmonm
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: robb,bwren
ms.openlocfilehash: 00ec9364a900510aeadcb68b19b57be528fb9c50
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2018
ms.locfileid: "30267214"
---
# <a name="monitoring-azure-applications-and-resources"></a>Monitorização de aplicações do Azure e de recursos

A monitorização é o ato de recolha e análise de dados para determinar o desempenho, o estado de funcionamento e a disponibilidade da sua aplicação de negócio e os recursos que depende. Uma estratégia de monitorização efetiva ajuda-o a compreender a operação de detalhado dos componentes da aplicação. Também o ajuda a aumentar a disponibilidade, proativamente notificam dos problemas críticos, de modo a que o possa resolvê-los antes que comprometam.

Azure inclui vários serviços individualmente efetuar uma tarefa ou função específica no espaço de monitorização. Em conjunto, estes serviços fornecem uma solução abrangente para recolher, analisar e telemetria da sua aplicação e os recursos do Azure que suportem a funcionar. Pode também funcionar para monitorizar recursos críticos no local para fornecer uma híbrida com o ambiente de monitorização. Compreender as ferramentas e os dados que estão disponíveis é o primeiro passo na desenvolver uma estratégia completa de monitorização para a sua aplicação. 

O diagrama seguinte mostra uma vista conceptual dos componentes que funcionam em conjunto para fornecer monitorização de recursos do Azure. As secções seguintes descrevem estes componentes e fornecem ligações para informações técnicas detalhadas.

![Descrição geral da Monitorização](media/monitoring-overview/monitoring-products-overview.png)


## <a name="shared-capabilities"></a>Capacidades partilhadas
O principal e o serviço de monitorização profunda partilham a funcionalidade que fornece as seguintes capacidades. 

### <a name="alerts"></a>Alertas
[Alertas do Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md) proativamente notificá-lo de condições crítico e, potencialmente, tome as medidas corretivas. Regras de alertas podem utilizar dados de várias origens, incluindo as métricas e registos. Utilizarem [grupos ação](../monitoring-and-diagnostics/monitoring-action-groups.md), que contêm conjuntos exclusivos de destinatários e ações em resposta a um alerta. Com base nos seus requisitos, pode ter alertas iniciar ações externas utilizando webhooks e integrar com as ferramentas ITSM.

### <a name="dashboards"></a>Dashboards
Pode utilizar [dashboards do Azure](../azure-portal/azure-portal-dashboards.md) combinar diferentes tipos de dados para um painel único no [portal do Azure](https://portal.azure.com). Em seguida, pode partilhar o dashboard com outros utilizadores do Azure. 

Por exemplo, pode criar um dashboard que combina:
- Mosaicos que mostram um gráfico de métricas
- Uma tabela de registos de atividade
- Um gráfico de utilização do Application Insights
- O resultado de uma pesquisa de registo na análise de registos

Também pode exportar dados de análise de registos para [Power BI](https://docs.microsoft.com/power-bi/). Aqui, pode tirar partido de visualizações adicionais. Também pode verificar os dados disponíveis para outras pessoas dentro e fora da sua organização.

### <a name="metrics-explorer"></a>Explorador de Métricas
[Métricas](../monitoring-and-diagnostics/monitoring-overview-metrics.md) são valores de numérico gerados por um recurso do Azure para o ajudar a compreender a operação e o desempenho do recurso. Ao utilizar o Explorador de métricas, pode enviar ao Log Analytics métricas para análise com dados de outras origens.


## <a name="core-monitoring"></a>Monitorização dos componentes essenciais
Monitorização principal fornece fundamentais, necessário monitorização através de recursos do Azure. Estes serviços necessitam de configuração mínima e recolher telemetria de núcleos que utilizam os serviços de monitorização premium.    

### <a name="azure-monitor"></a>Azure Monitor
[Monitor do Azure](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) permite a monitorização dos componentes essenciais para os serviços do Azure, permitindo que a coleção de [métricas](../monitoring-and-diagnostics/monitoring-overview-metrics.md), [registos de atividade](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), e [os registos de diagnóstico](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md). Por exemplo, o registo de atividade indica quando os novos recursos são criados ou modificados. 

Métricas estão disponíveis que fornecer estatísticas de desempenho de recursos diferentes e mesmo sistema operativo no interior de uma máquina virtual. Pode ver estes dados com um dos exploradores no portal do Azure e criar alertas com base nestas métricas. Monitor do Azure fornece que as métricas mais rápidas pipeline (5 minutos para baixo para 1 minuto), pelo que deve utilizá-lo para notificações e alertas críticos de tempo. 

Também pode enviar estas métricas e registos de análise de registos do Azure para análise de tendências e detalhada ou criar regras de alerta adicionais para proativamente notificá-lo de problemas críticos como resultado que Analysis Services.  

### <a name="azure-advisor"></a>Azure Advisor
[Azure Advisor](../advisor/advisor-overview.md) monitoriza constantemente a telemetria de configuração e utilização de recursos. Em seguida, proporciona personalizadas recomendações com base nas melhores práticas. Seguir estas recomendações ajuda a melhorar o desempenho, a segurança e a disponibilidade dos recursos que suportam as suas aplicações.

### <a name="service-health"></a>Service Health
O estado de funcionamento da sua aplicação baseia-se nos serviços do Azure que depende. [Estado de funcionamento de serviço do Azure](../service-health/service-health-overview.md) identifica quaisquer problemas com os serviços do Azure que podem afetar a sua aplicação. Estado de funcionamento do serviço também ajuda-o a planear para manutenção agendada.

### <a name="activity-log"></a>Registo de Atividades
[Registo de atividade](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) fornecem dados sobre o funcionamento de um recurso do Azure. Estas informações incluem:
- Alterações de configuração para o recurso.
- Incidentes de estado de funcionamento do serviço.
- Recomendações no melhor utilização de recursos.
- Informações relacionadas com operações de dimensionamento automático. 

Pode ver os registos para um recurso específico na respetiva página no portal do Azure. Ou, pode ver os registos de vários recursos no Explorador de registo de atividade. 

Também pode enviar entradas de registo de atividade para análise de registos. Aqui, pode analisar os registos, utilizando os dados recolhidos por soluções de gestão, os agentes em máquinas virtuais e outras origens.

## <a name="deep-monitoring-services"></a>Serviços de monitorização profundas
Os seguintes serviços do Azure fornecem capacidades avançadas para recolher e analisar dados de monitorização a um nível mais aprofundado. Estes serviços tirar partido da monitorização principal e tirar partido das funcionalidades comuns no Azure. Fornecem análise poderosas com os dados recolhidos para lhe dar exclusivos insights para as suas aplicações e infraestrutura. Estes apresentam os dados no contexto de cenários que são direcionadas para diferentes público.

### <a name="deep-application-monitoring"></a>Monitorização de aplicações avançadas
#### <a name="application-insights"></a>Application Insights
Pode utilizar [Azure Application Insights](http://azure.microsoft.com/documentation/services/application-insights) para monitorizar a disponibilidade, desempenho e a utilização da sua aplicação, se de que está alojado na nuvem ou no local. 

Ao instrumentar a aplicação funcione com o Application Insights, pode obter conhecimentos aprofundados e implementar cenários de DevOps. Pode identificar rapidamente e diagnosticar erros sem aguardar por um utilizador comunicá-los. Com as informações que recolher, pode tornar-se informado opções de manutenção e melhorias da sua aplicação. 

O Application Insights tem um vasto conjunto ferramentas para interagir com os dados que recolhe. Application Insights armazena os respetivos dados num repositório comuns. Pode tirar partido da funcionalidade partilhada, tais como alertas, dashboards e uma análise detalhada com o idioma de consulta de análise de registos.

### <a name="deep-infrastructure-monitoring"></a>Monitorização de infraestrutura profunda
#### <a name="log-analytics"></a>Log Analytics
[Análise de registo](http://azure.microsoft.com/documentation/services/log-analytics) desempenha um papel de monitorização do Azure através da recolha de dados de uma variedade de recursos (incluindo as ferramentas de terceiros) num único repositório central. Aqui, pode analisar os dados ao utilizar uma linguagem de consulta poderosa. 

Application Insights e o Centro de segurança do Azure armazenam os seus dados na análise de registos de dados armazenam e utilizam o motor de análise. Dados também são recolhidos de Monitor do Azure, as soluções de gestão e agentes instalados em máquinas virtuais na nuvem ou no local. Esta funcionalidade partilhada ajuda-o a formar uma visão geral do seu ambiente.

#### <a name="management-solutions"></a>Soluções de gestão
[As soluções de gestão](../log-analytics/log-analytics-add-solutions.md) instaladores conjuntos de lógica que fornecem informações para uma determinada aplicação ou serviço. Análise de registos para armazenar e analisar os dados de monitorização recolher que dependem. 

Soluções de gestão são disponibilizadas pela Microsoft e parceiros para fornecer monitorização de vários serviços de terceiros e do Azure. Exemplos de monitorização de soluções:
* [Monitorização do contentor](../log-analytics/log-analytics-containers.md), que poderá ver e gerir os anfitriões de contentor.
* [Análise de SQL do Azure](../log-analytics/log-analytics-azure-sql.md), que recolhe e visualiza métricas de desempenho de bases de dados SQL do Azure.

Pode ver todas as soluções de gestão disponíveis no Portal do Azure, sob o *Monitor* ecrã. 

#### <a name="network-monitoring"></a>Monitorização de rede
Existem várias ferramentas que trabalham em conjunto para monitorizar diferentes aspetos da sua rede no Azure ou no local.  

[Observador de rede](../network-watcher/network-watcher-monitoring-overview.md) fornece baseada em cenários de monitorização e diagnóstico para cenários de rede diferente no Azure. Armazena dados no Azure métricas e diagnóstico para análise adicional. Funciona com as seguintes soluções para monitorização diferentes aspetos da sua rede. 

[Monitor de desempenho (NPM) de rede](https://blogs.msdn.microsoft.com/azuregov/2017/09/05/network-performance-monitor-general-availability/) é uma solução que monitoriza a conectividade entre nuvens públicas, centros de dados e ambientes no local de monitorização de rede baseado na nuvem.

[Monitor de ExpressRoute](https://azure.microsoft.com/blog/monitoring-of-azure-expressroute-in-preview/) é uma capacidade NPM que monitoriza a conetividade ponto a ponto e o desempenho através de circuitos ExpressRoute do Azure.

[Análise de DNS](../log-analytics/log-analytics-dns.md) é uma solução que fornece segurança, desempenho e operações relacionadas com as informações, com base nos seus servidores DNS.

[Monitor de ponto final de serviço](../networking/network-monitoring-overview.md) testa a acessibilidade de aplicações e Deteta congestionamentos de desempenho no local, operadora redes e centros de dados de nuvem/privadas.


#### <a name="service-map"></a>Mapa de Serviços
[Mapa de serviço](../operations-management-suite/operations-management-suite-service-map.md) fornece informações sobre o ambiente de IaaS ao analisar as máquinas virtuais com as respetivas dependências nos outros computadores e processos externos e processos diferentes. É integrado eventos, dados de desempenho e soluções de gestão na análise de registos. Em seguida, pode ver estes dados no contexto de cada computador e a respetiva relação para o resto do seu ambiente. 

Mapa de serviço é semelhante à [mapa de aplicação no Application Insights](../application-insights/app-insights-app-map.md). Concentra-se nos componentes de infraestrutura que suporta as suas aplicações.


## <a name="example-scenarios"></a>Cenários de exemplo
Seguem-se exemplos de alto nível que ilustram a forma como pode utilizar diferentes ferramentas de monitorização no Azure para diferentes cenários.

### <a name="monitoring-a-web-application"></a>Monitorizar uma aplicação web
Considere uma aplicação web implementada no Azure através do App Service do Azure, o Storage do Azure e uma base de dados do SQL Server. Iniciar acedendo [métricas](../monitoring-and-diagnostics/monitoring-overview-metrics.md) e [registos de atividade](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) para estes recursos no respetivos páginas no portal do Azure. Procurar informações críticas, tais como o número de pedidos à aplicação e tempo de resposta médio. Também identificar alterações de configuração.

É, em seguida, aceda ao Monitor no portal para ver métricas e registos de diferentes recursos em conjunto. Como determinar os parâmetros padrão para as métricas [criar regras de alertas](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md). Estas regras proativamente notificá-lo quando, por exemplo, o tempo de resposta médio aumenta para além de um limiar. Para obter uma vista rápida do desempenho diária da sua aplicação, crie um dashboard do Azure para mostrar gráficos de métricas que representam os KPIs críticos.

Para efetuar a monitorização mais aprofundada da sua aplicação, [configurá-lo para o Application Insights](../application-insights/quick-monitor-portal.md). Agora pode recolher dados adicionais que fornece mais aprofundadas sobre o funcionamento e desempenho da aplicação. Application Insights Deteta as relações entre componentes da aplicação subjacentes. Permite representação visual através de [o mapeamento de aplicações](../application-insights/app-insights-app-map.md) conjugados com [ponto-a-ponto rastreio](../application-insights/app-insights-transaction-diagnostics.md) para diagnosticar o componente exata, dependência ou exceção onde ocorreu um problema. 

Criar [testes de disponibilidade](../application-insights/app-insights-monitor-web-app-availability.md) proativamente teste a aplicação de regiões diferentes. Para ajudar os programadores [ativar o gerador de perfis](../application-insights/enable-profiler-compute.md) para controlar a pedidos e quaisquer exceções para baixo para uma linha de código específica. Para obter mais visibilidade serviços utilizados na sua aplicação, adicione o [solução de análise do SQL Server](../log-analytics/log-analytics-azure-sql.md) para recolher dados adicionais na análise de registos. 

Após algum tempo, optar por investigue a causa de raiz para períodos quando desempenho no site ficou abaixo de um limiar. Escrever uma consulta ao utilizar a análise de registos. Ajuda a correlacionar os dados de utilização e desempenho recolhidos pelo Application Insights com a configuração e dados de desempenho entre os recursos do Azure que suportam a sua aplicação.


### <a name="monitoring-virtual-machines"></a>Monitorizar máquinas virtuais
Tiver uma mistura de máquinas de virtuais Windows e Linux em execução no Azure. Utilize o Monitor do Azure para ver [registos de atividade](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) e [métricas de nível do anfitrião](../monitoring-and-diagnostics/monitoring-overview-metrics.md). Adicionar o [extensão de diagnóstico do Azure](../virtual-machines/linux/tutorial-monitoring.md#install-diagnostics-extension) às máquinas virtuais para recolher as métricas de sistema operativo convidado. Em seguida, criar [regras de alerta](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) para proativamente notificá-lo quando os limiares de se estender métricas básicas como a utilização do processador e memória.

Para recolher mais detalhes sobre as máquinas virtuais a executar uma aplicação de negócio, [criar uma área de trabalho de análise de registos e ativar a extensão VM](../log-analytics/log-analytics-quick-collect-azurevm.md) em cada máquina. Configurar o [coleção de origens de dados diferentes](../log-analytics/log-analytics-data-sources.md) para a sua aplicação e [criar vistas](../log-analytics/log-analytics-view-designer.md) para elaborar relatórios sobre o desempenho e operação diária. Pode, em seguida, [criar regras de alertas](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) para notificá-lo quando são recebidos eventos de erro específico. 

Para monitorizar continuamente o estado de funcionamento do agente instalado, adicione o [solução de gestão do Estado de funcionamento do agente](../operations-management-suite/oms-solution-agenthealth.md). Para obter mais informações sobre a aplicação [adicionar o agente de dependência](../operations-management-suite/operations-management-suite-service-map-configure.md) às máquinas virtuais para adicioná-los ao [mapa de serviço](../operations-management-suite/operations-management-suite-service-map.md). Mapa de serviço Deteta processos críticos e identifica as ligações entre máquinas com outros serviços. 

Após uma falha comunicada, utilize mapa de serviço para efetuar forenses para identificar as máquinas específicas que ocorreu o problema. Em seguida, crie um [consulta os dados de análise de registos](../log-analytics/log-analytics-log-search-new.md) para identificar o problema no futuro. E crie uma regra de alerta para proativamente notificá-lo quando a condição é detetada.



## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre:

* [Monitor do Azure](https://azure.microsoft.com/services/monitor/) para começar com métricas e alertas de monitorização principal.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) se estiver a tentar diagnosticar problemas na sua aplicação web do app Service.
* [Análise de registo](https://azure.microsoft.com/documentation/services/log-analytics/) para analisar os dados de monitorização recolhidos e registos.
