---
title: "Monitorização de recursos e aplicações do Azure | Microsoft Docs"
description: "Descrição geral da diferentes serviços da Microsoft e a funcionalidade que contribuem para uma estratégia completa de monitorização para os seus serviços do Azure e aplicações."
author: robb
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: robb,bwren
ms.openlocfilehash: 505e92b5fc63f570bc4d0f8899ae977b93850356
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="monitoring-azure-applications-and-resources"></a>Monitorização de aplicações do Azure e de recursos

A monitorização é o ato de recolha e análise de dados para determinar o desempenho, o estado de funcionamento e a disponibilidade da sua aplicação de negócio e os recursos depende. Uma estratégia de monitorização efetiva irão ajudá-lo a compreender a operação de detalhado de diferentes componentes da aplicação e para aumentar o tempo de atividade por proativamente notificam dos problemas críticos, de modo a que o possa resolvê-los antes que comprometam.

Azure inclui vários serviços individualmente efetuar uma tarefa ou função específica no espaço de monitorização e em conjunto fornecem uma solução abrangente para recolher, analisar e a funcionar telemetria da sua aplicação e os recursos do Azure subjacentes suporte.  Pode também funcionar para monitorizar recursos críticos no local para fornecer uma híbrida com o ambiente de monitorização.   Compreender as ferramentas e os dados que estão disponíveis é o primeiro passo na desenvolver uma estratégia completa de monitorização para a sua aplicação. 

O diagrama seguinte mostra uma vista conceptual de diferentes componentes que funcionam em conjunto para fornecer monitorização de recursos do Azure.  Cada um destes é descrita nas secções seguintes com ligações para informações técnicas detalhadas.

![Descrição geral da Monitorização](media/monitoring-overview/overview.png)

## <a name="basic-monitoring"></a>Monitorização Básica
Monitorização básica fornece monitorização necessário fundamentais através de recursos do Azure.  Estes serviços necessitam de configuração mínima e recolher telemetria de núcleos que é utilizada pelo premium monitorização dos serviços.    

### <a name="azure-monitor"></a>Azure Monitor
[Monitor do Azure](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) permite a monitorização básica para o serviço do Azure, permitindo que a coleção de [métricas](../monitoring-and-diagnostics/monitoring-overview-metrics.md), [registos de atividade](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), e [registos de diagnóstico](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).  Por exemplo, o registo de atividade serão indicam quando os novos recursos são criados ou modificados.  Métricas estão disponíveis que fornecer estatísticas de desempenho de recursos diferentes e mesmo sistema operativo no interior de uma máquina virtual.  Pode ver estes dados com um dos exploradores no portal do Azure, envia-as à análise de registos de análise de tendências e detalhada ou criar regras de alerta para proativamente notificá-lo de problemas críticos.

### <a name="service-health"></a>Service Health
O estado de funcionamento da sua aplicação baseia-se nos serviços do Azure que depende.  [Estado de funcionamento de serviço do Azure](../service-health/service-health-overview.md) identifica quaisquer problemas com os serviços do Azure que poderão ter impacto na sua aplicação e também o ajudará a planear para qualquer agenda de manutenção.

### <a name="azure-advisor"></a>Azure Advisor
[Azure Advisor](../advisor/advisor-overview.md) monitoriza constantemente a telemetria de configuração e utilização de recursos para fornecer personalizadas recomendações com base nas melhores práticas.  Os seguintes estas recomendações ajuda a melhorar o desempenho, a segurança e a disponibilidade dos recursos que suportam as suas aplicações.


## <a name="premium-monitoring-services"></a>Serviços de monitorização de Premium
Os seguintes serviços do Azure fornecem capacidades avançadas para recolher e analisar dados de monitorização.  Estes tirar partido da monitorização e tire partido comuns funcionalidade básica no Azure e fornecem análise poderosas com os dados recolhidos para lhe dar insights exclusivos para as suas aplicações e infraestrutura.  Estes apresentam os dados dentro do contexto de determinados cenários direcionadas para diferentes público.

### <a name="application-insights"></a>Application Insights
[Application Insights](http://azure.microsoft.com/documentation/services/application-insights) permite-lhe para monitor de disponibilidade, desempenho e a utilização da sua aplicação se de que está alojado na nuvem ou no local.  Por instrumentar a aplicação funcione com o Application Insights, pode obter conhecimentos aprofundados, permitindo-lhe identificar e diagnosticar erros sem aguardar por um utilizador comunicá-las rapidamente. Com as informações que recolher, pode tornar-se informado opções de manutenção e melhorias da sua aplicação.  Para além de ferramentas extensas para interagir com os dados que recolhe, o Application Insights armazena os respetivos dados num repositório comuns para tirar partido das funcionalidades partilhada como alertas, dashboards e uma análise detalhada com o idioma de consulta de análise de registos.

### <a name="log-analytics"></a>Log Analytics
[Análise de registo](http://azure.microsoft.com/documentation/services/log-analytics) desempenha um papel central na monitorização do Azure através da recolha de dados de uma variedade de recursos para um repositório único, onde pode ser analisado com um idioma de consulta poderosa.  Application Insights e o Centro de segurança do Azure armazenam os seus dados na análise de registos de dados armazenam e tirar partido do respetivo motor de análise.  Isto combinados com dados recolhidos a partir de Monitor do Azure, as soluções de gestão e agentes instalados em máquinas virtuais na nuvem ou no local permitem-lhe formar uma visão geral de todo o seu ambiente. 


### <a name="service-map"></a>Mapa de Serviços
[Mapa de serviço](../operations-management-suite/operations-management-suite-service-map.md) fornece informações sobre o ambiente de IaaS ao analisar as máquinas virtuais com as respetivas dependências nos outros computadores e processos externos e processos diferentes.  É integrado eventos, dados de desempenho e soluções de gestão na análise de registos para que possa visualizar estes dados no contexto de cada computador e a respetiva relação para o resto do seu ambiente.  Mapa de serviço é semelhante a [mapa de aplicação no Application Insights](../application-insights/app-insights-app-map.md) mas centra-se nos componentes de infraestrutura que suporta as suas aplicações.

### <a name="network-watcher"></a>Observador de Rede
[Observador de rede](../network-watcher/network-watcher-monitoring-overview.md) fornece baseada em cenários de monitorização e diagnóstico para cenários de rede diferente no Azure.  Armazena os dados no Azure métricas e diagnóstico para mais análises e funciona com a seguinte soluções para monitorização diferentes aspetos da sua rede de monitorização de rede:
* [Monitor de desempenho (NPM) de rede](https://blogs.msdn.microsoft.com/azuregov/2017/09/05/network-performance-monitor-general-availability/) - uma solução que monitoriza a conectividade entre nuvens públicas, centros de dados e ambientes no local de monitorização de rede de baseada na nuvem
* [Monitor de ExpressRoute](https://azure.microsoft.com/en-in/blog/monitoring-of-azure-expressroute-in-preview/) -capacidade de NPM um que monitoriza a conetividade ponto a ponto e o desempenho através de circuitos do ExpressRoute.
* Análise de tráfego - uma solução baseada na nuvem, que fornece visibilidade para a atividade de utilizador e a aplicação na sua rede de nuvem.
* [Análise de DNS](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-dns) -fornece informações relacionadas com operações, desempenho e segurança, com base nos seus servidores DNS.

### <a name="management-solutions"></a>Soluções de gestão
[As soluções de gestão](../log-analytics/log-analytics-add-solutions.md) instaladores conjuntos de lógica que fornecem informações para uma determinada aplicação ou serviço.  Análise de registos para armazenar e analisar os dados de monitorização que recolher que dependem.  Soluções de gestão são disponibilizadas pela Microsoft e os parceiros que fornecem monitorização para vários serviços do Azure e de terceiros. Exemplo de soluções de monitorização incluem [monitorização do contentor](../log-analytics/log-analytics-containers.md) que poderá ver e gerir os anfitriões de contentor e [análise do Azure SQL](../log-analytics/log-analytics-azure-sql.md) que recolhe e visualiza métricas de desempenho para o SQL Server Bases de dados do Azure.


## <a name="shared-functionality"></a>Funcionalidade partilhada
As seguintes ferramentas do Azure fornecem funcionalidades críticas para o premium monitorização dos serviços.  Estes são partilhados por vários serviços, permitindo-lhe tirar partido das funcionalidades e configurações comuns em vários serviços.

### <a name="alerts"></a>Alertas
[Alertas do Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md) proativamente notificá-lo de condições crítico e, potencialmente, tome as medidas corretivas.  Regras de alertas podem tirar partido dos dados de várias origens, incluindo as métricas e registos. Utilizarem [ação grupos](../monitoring-and-diagnostics/monitoring-action-groups.md) que contêm conjuntos exclusivos de destinatários e ações em resposta a um alerta.  Com base nos seus requisitos, pode ter alertas iniciar ações externas utilizando webhooks e integrar com as ferramentas ITSM.

### <a name="dashboards"></a>Dashboards
[Dashboards do Azure](../azure-portal/azure-portal-dashboards.md) permitem combinar diferentes tipos de dados para um painel único no portal do Azure e partilha com outros utilizadores do Azure.  Por exemplo, pode criar um dashboard que combina os mosaicos que mostra um gráfico de métricas, uma tabela de registos de atividade, um gráfico de utilização do Application Insights e o resultado de uma pesquisa de registo na análise de registos.

Também pode exportar dados de análise de registos para [Power BI](https://docs.microsoft.com/power-bi/) para tirar partido das visualizações adicionais e também para disponibilizar dados para outras pessoas dentro e fora da sua organização.

### <a name="metrics-explorer"></a>Explorador de métricas
[Métricas](../monitoring-and-diagnostics/monitoring-overview-metrics.md) são valores de numérico gerados pelos recursos do Azure que o ajudam a compreender a operação e o desempenho do recurso. Pode enviar métricas para análise de registos para análise com dados de outras origens.



### <a name="activity-logs"></a>Registos de Atividade
[Registos de atividade](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) fornecem dados sobre o funcionamento dos recursos do Azure.  Isto inclui essas informações como alterações de configuração para o recurso, incidentes de estado de funcionamento do serviço, recomendações melhor saber como utilizar os recursos e informações relacionadas com operações de dimensionamento automático.  Pode ver os registos para um recurso específico na respetiva página nos registos do Azure de portal ou de vista a partir de vários recursos no Explorador de registo de atividade.  Também pode enviar registos de atividade para análise de registos, pelo que pode ser analisados com dados recolhidos por soluções de gestão, os agentes em máquinas virtuais e outras origens.


## <a name="example-scenarios"></a>Cenários de exemplo
Seguem-se exemplos de nível elevados que ilustram a forma como iria tirar partido diferentes ferramentas de monitorização no Azure para diferentes cenários.

### <a name="monitoring-a-web-application"></a>Monitorizar uma aplicação web
Considere uma aplicação web implementada no Azure através de serviços aplicacionais, o Storage do Azure e uma base de dados do SQL Server.  Foi possível iniciar acedendo [métricas](../monitoring-and-diagnostics/monitoring-overview-metrics.md) e [registos de atividade](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) para cada um destes recursos individuais no respetivas páginas no portal do Azure.  Isto inclui informações críticas, tais como o número de pedidos à aplicação e tempo de resposta médio para além de identificar alterações de configuração.

Em seguida, foi possível ir para o Monitor no portal para ver métricas e registos de diferentes recursos em conjunto.  Como determinar os parâmetros padrão para as métricas [criar regras de alertas](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) para proativamente notificá-lo quando, por exemplo, o tempo de resposta médio aumenta para além de um limiar.  Para obter uma vista rápida do desempenho diária da sua aplicação, crie um dashboard do Azure para mostrar gráficos de métricas que representa os KPIs críticos.

Para efetuar a monitorização mais aprofundada da sua aplicação, [configurá-lo para o Application Insights](../application-insights/quick-monitor-portal.md).  Agora pode recolher dados adicionais fornecer mais aprofundadas sobre o funcionamento e desempenho da aplicação.  Application Insights Deteta as relações entre componentes da sua aplicação, permitindo representação visual através de subjacentes a [o mapeamento de aplicações](../application-insights/app-insights-app-map.md) conjugados com [ponto-a-ponto rastreio](../application-insights/app-insights-transaction-diagnostics.md) para diagnosticar o componente exata, dependência ou exceção onde ocorreu um problema.  Criar [testes de disponibilidade](../application-insights/app-insights-monitor-web-app-availability.md) proativamente teste a aplicação de regiões diferentes.  Para ajudar os programadores [ativar o gerador de perfis](../application-insights/enable-profiler-compute.md) para controlar a pedidos e quaisquer exceções para baixo para uma linha de código específica.  

Para obter mais visibilidade serviços utilizados na sua aplicação, adicione o [solução de análise do SQL Server](../log-analytics/log-analytics-azure-sql.md) recolher dados adicionais para análise de registos. Após algum tempo, optar por investigue a causa de raiz para períodos de tempo quando desempenho no site ficou abaixo do limiar.  Escrever uma consulta com a análise de registos para correlacionar os dados de utilização e desempenho recolhidos pelo Application Insights com a configuração e o desempenho dados entre os recursos do Azure que suporta a sua aplicação.


### <a name="monitoring-virtual-machines"></a>Monitorizar máquinas virtuais
Tiver uma mistura de máquinas de virtuais Windows e Linux em execução do Azure.  Utilize o Monitor do Azure para ver [registos de atividade](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) e [alojar as métricas de nível](../monitoring-and-diagnostics/monitoring-overview-metrics.md) e, em seguida, adicione o [extensão de diagnóstico do Azure](../virtual-machines/linux/tutorial-monitoring.md#install-diagnostics-extension) às máquinas virtuais para recolher a métrica sistema de operativo convidado.  Em seguida, criar [regras de alerta](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) proativamente notificar quando básico métricas essas utilização do processador e memória cruzam limiares.

Para recolher mais detalhes sobre as máquinas virtuais a executar uma aplicação de negócio, [criar uma área de trabalho de análise de registos e ativar a extensão VM](../log-analytics/log-analytics-quick-collect-azurevm.md) em cada máquina.  Configurar [coleção de origens de dados diferentes](../log-analytics/log-analytics-data-sources.md) para a sua aplicação e [criar vistas](../log-analytics/log-analytics-view-designer.md) para elaborar relatórios sobre o desempenho e operação diária.  Pode, em seguida, [criar regras de alertas](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) para notificá-lo quando são recebidos eventos de erro específico.  Para poder monitorizar continuamente o estado de funcionamento do agente instalado, de adicionar o [solução de gestão do Estado de funcionamento do agente](../operations-management-suite/oms-solution-agenthealth.md).

Para obter mais informações sobre a aplicação [adicionar o agente de dependência](../operations-management-suite/operations-management-suite-service-map-configure.md) às máquinas virtuais para adicioná-los ao [mapa de serviço](../operations-management-suite/operations-management-suite-service-map.md).  -Deteta processos críticos e identifica as ligações entre máquinas com outros serviços.  Após uma falha comunicada, utilize mapa de serviço para efetuar forenses para identificar as máquinas específicas que ocorreu o problema.  Em seguida, crie um [consulta os dados de análise de registos](../log-analytics/log-analytics-log-search-new.md) para identificar o problema no futuro e criar uma regra de alerta para proativamente notificá-lo quando a condição foi detetada.



## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre

* [Monitor do Azure um vídeo de Ignite 2016](https://myignite.microsoft.com/videos/4977)
* [Introdução ao Monitor do Azure](monitoring-get-started.md)
* [Diagnóstico do Azure](../azure-diagnostics.md) se está a tentar diagnosticar problemas no seu serviço de nuvem, a Máquina Virtual, a Máquina Virtual Dimensionar aplicação de recursos de infraestrutura definida ou serviço.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) se estiver a tentar diagnóstico problemas na sua aplicação de serviço Web da aplicação.
* [Análise de registo](https://azure.microsoft.com/documentation/services/log-analytics/) para analisar os dados de monitorização recolhidos.
