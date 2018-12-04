---
title: Descrição geral de Monitor do Azure | Documentos da Microsoft
description: Descrição geral dos serviços e das funcionalidades da Microsoft que contribuem para uma estratégia completa de monitorização para os seus serviços e aplicações do Azure.
author: bwren
manager: carmonm
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/26/2018
ms.author: bwren
ms.openlocfilehash: a3e0125a66c4b8bbda6e0b7087fe5e0e387d43e4
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52847393"
---
# <a name="azure-monitor-overview"></a>Descrição geral do Azure Monitor

O Azure Monitor maximiza a disponibilidade e desempenho das suas aplicações, fornecendo uma solução abrangente para recolher, analisar e atuar na telemetria a partir da nuvem e ambientes no local. Ajuda a compreender o desempenho das suas aplicações e identifica proativamente os problemas que as afetam e os recursos de que dependem.

> [!VIDEO https://www.youtube.com/embed/_hGff5bVtkM]

## <a name="overview"></a>Descrição geral
O diagrama seguinte fornece uma visão geral do Azure Monitor. No centro do diagrama são arquivos de dados de métricas e registos que são os dois tipos fundamentais de utilização de dados pelo Azure Monitor. No lado esquerdo são os [monitorizados de origens que recolhem telemetria a partir de diferentes recursos](../azure-monitor/platform/data-sources.md) e preencher o [arquivos de dados](../azure-monitor/platform/data-collection.md). À direita são as diferentes funções do Azure Monitor executa com estes dados recolhidos, tais como análise, alertas e transmissão em fluxo a sistemas externos.


![Descrição geral do Azure Monitor](media/overview/overview.png)


## <a name="monitoring-data-platform"></a>Plataforma de dados de monitorização
Todos os dados recolhidos pelo Monitor do Azure se encaixa em um dos dois tipos fundamentais, [métricas e registos](../azure-monitor/platform/data-collection.md). [Métricas](../azure-monitor/platform/data-collection.md#metrics) são valores numéricos que descrevem algum aspeto de um sistema num momento específico no tempo. Elas são simples e capaz de oferecer suporte a cenários em tempo real em tempo quase. [Registos](../azure-monitor/platform/data-collection.md#logs) contêm diferentes tipos de dados organizados em registos com diferentes conjuntos de propriedades de cada tipo. Telemetria, como eventos e rastreios são armazenadas como registos além do mais dados de desempenho para que ele possível combinar tudo para análise.

Para muitos recursos do Azure, verá os dados recolhidos pelo direito de Monitor do Azure na sua página de descrição geral no portal do Azure. Dê uma olhada em qualquer máquina virtual por exemplo, e verá vários gráficos de métricas de desempenho a apresentar. Clique em qualquer um dos gráficos para abrir os dados no [Explorador de métricas](../monitoring-and-diagnostics/monitoring-metric-charts.md) no portal do Azure, que permite-lhe os valores de várias métricas do gráfico ao longo do tempo.  Pode ver os gráficos interativamente ou afixá-los a um dashboard para visualizá-los com outras visualizações.

![Métricas](media/overview/metrics.png)

Dados de registo recolhidos pelo Monitor do Azure são armazenados no Log Analytics, que inclui um [linguagem de consulta avançada](../azure-monitor/log-query/log-query-overview.md) rapidamente obter, consolidar e analisar os dados recolhidos.  Pode criar e testar consultas através do [página do Log Analytics](../log-analytics/log-analytics-log-search-portals.md) no portal do Azure e, em seguida, em qualquer um, analisar diretamente os dados através destas ferramentas ou guardar consultas para utilização com [visualizações](visualizations.md) ou [ regras de alerta](../monitoring-and-diagnostics/monitoring-overview-alerts.md).

A linguagem de consulta do Log Analytics é adequada para consultas de registo simples, mas também inclui funcionalidades avançadas, como agregações, associações e análises inteligentes. Pode aprender rapidamente a linguagem de consulta através de [várias lições](../log-analytics/query-language/get-started-queries.md) que estão disponíveis.  É fornecida orientação específica para os utilizadores que já estejam familiarizados com [SQL](../log-analytics/query-language/sql-cheatsheet.md) e [Splunk](../log-analytics/query-language/splunk-cheatsheet.md).

![Registos](media/overview/logs.png)

## <a name="what-data-does-azure-monitor-collect"></a>Os dados que recolher o Azure Monitor?
O Azure Monitor pode recolher dados de várias origens. Pode considerar os dados para as suas aplicações em camadas que vão desde a sua aplicação, qualquer sistema operacional e depende, para baixo para a própria plataforma de serviços de monitorização. Monitor do Azure recolhe dados de cada um dos seguintes escalões:

- **Dados de monitorização de aplicações**: dados sobre o desempenho e a funcionalidade do código escreveu, independentemente de sua plataforma.
- **Dados de monitorização de SO de convidado**: dados sobre o sistema operativo no qual a sua aplicação está em execução. Isso poderia estar em execução no Azure, noutra cloud ou no local. 
- **Dados de monitorização de recursos do Azure**: dados sobre o funcionamento de um recurso do Azure.
- **Subscrição do Azure, os dados de monitorização**: dados sobre a operação e a gestão de uma subscrição do Azure, bem como dados sobre o estado de funcionamento e a operação do Azure em si. 
- **Inquilino do Azure, os dados de monitorização**: dados sobre o funcionamento dos serviços do Azure ao nível do inquilino, como o Azure Active Directory.

Assim que criar uma subscrição do Azure e comece a adicionar recursos, tais como máquinas virtuais e aplicações web, o Azure Monitor inicia a recolha de dados.  [Registos de atividades](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) gravar quando os recursos são criados ou modificados. [Métricas](../monitoring-and-diagnostics/monitoring-overview-metrics.md) informá-lo como o recurso está sendo executada e os recursos que ele está a consumir. 

Expandir os dados que está a recolher sobre o funcionamento real dos recursos por [ativar os diagnósticos](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) e [adicionando um agente](../azure-monitor/platform/agent-windows.md) para recursos de computação. Esta ação irá recolher telemetria para o funcionamento interno de recurso e permitem-lhe configurar diferente [origens de dados](../azure-monitor/platform/agent-data-sources.md) para recolher registos e métricas de sistema de operativo de convidado do Windows e Linux. 

[Adicionar um pacote de instrumentação à sua aplicação](../application-insights/app-insights-azure-web-apps.md), para ativar o Application Insights para recolher as informações detalhadas sobre a sua aplicação, incluindo exceções, pedidos de aplicações e vistas de página. Verificar a disponibilidade da sua aplicação ao configurar uma [teste de disponibilidade](../application-insights/app-insights-monitor-web-app-availability.md) para simular o tráfego de utilizador.

### <a name="custom-sources"></a>Origens personalizadas
O Azure Monitor pode recolher dados de registo de qualquer cliente REST utilizando o [API do Recoletor de dados](../log-analytics/log-analytics-data-collector-api.md). Isto permite-lhe criar cenários de monitorização personalizados e expande a monitorização para recursos que não expõem a telemetria por meio de outras origens.



## <a name="insights"></a>Informações
Dados de monitorização só é útil se ele pode aumentar a visibilidade sobre o funcionamento do seu ambiente informático. Monitor do Azure inclui vários recursos e ferramentas que fornecem informações importantes para seus aplicativos e outros recursos que eles dependem. [Soluções de monitorização](../azure-monitor/insights/solutions.md) e funcionalidades como [Application Insights](../application-insights/app-insights-overview.md) e informações de contentor fornecem informações mais detalhadas sobre diferentes aspetos da sua aplicação e os serviços específicos do Azure. 

### <a name="application-insights"></a>Application Insights
[O Application Insights](../application-insights/app-insights-overview.md) monitoriza a disponibilidade, desempenho e utilização das suas aplicações web, se estão hospedados na nuvem ou no local. Ela aproveita a plataforma de análise de dados avançada no Log Analytics para lhe fornecer informações mais detalhadas sobre as operações de seu aplicativo e diagnosticar erros sem esperar por um utilizador para comunicá-las. Application Insights inclui pontos de ligação a uma variedade de ferramentas de desenvolvimento e integra-se com o Visual Studio para oferecer suporte a seus processos de DevOps.

![App Insights](media/overview/app-insights.png)

### <a name="azure-monitor-for-containers"></a>Azure Monitor para contentores
Monitor do Azure para contentores é uma funcionalidade concebida para monitorizar o desempenho de cargas de trabalho de contentor implementadas em clusters do Kubernetes alojados no Azure Kubernetes Service (AKS) geridos. Ele fornece visibilidade de desempenho por memória de coleta e métricas de processador de controladores, nós e contentores que estão disponíveis no Kubernetes por meio da API de métricas. Os registos do contentor também são recolhidos.  Depois de ativar a monitorização de clusters de Kubernetes, estas métricas e registos são automaticamente recolhidos para por meio de uma versão em contentores do agente do Log Analytics para Linux e armazenados no Log Analytics.

![Estado de funcionamento do contentor](media/overview/container-insights.png)

### <a name="azure-monitor-for-vms"></a>Azure Monitor para VMs
Informações de VM de Monitor do Azure monitoriza máquinas de virtuais do Azure (VM) em escala, ao analisar o desempenho e estado de funcionamento do Windows e VMs do Linux, incluindo os respetivos diferentes processos e interconectadas dependências em outros recursos e processos externos. A solução inclui suporte para monitorização do desempenho e dependências de aplicações para as VMs alojadas no local ou de outro fornecedor de cloud.  


![VM Insights](media/overview/vm-insights.png)

### <a name="monitoring-solutions"></a>Soluções de monitorização
[Soluções de monitorização](../azure-monitor/insights/solutions.md) no Azure Monitor são empacotados conjuntos de lógica que fornecem informações para uma determinada aplicação ou serviço. Recolher dados no Log Analytics, juntamente com outros dados de monitorização, usando [consultas](../azure-monitor/log-query/log-query-overview.md) para análise e [vistas](../azure-monitor/platform/view-designer.md) para visualização. Soluções de monitorização são [disponibilizados pela Microsoft](../azure-monitor/insights/solutions-inventory.md) e parceiros para fornecer monitorização para vários serviços do Azure e outras aplicações.

![Soluções de monitorização](media/overview/solutions-overview.png)

## <a name="responding-to-critical-situations"></a>Responder a situações críticas
Além de lhe permitir analisar dados de monitorização interativamente, uma solução de monitorização eficaz tem de ser capaz de responder proativamente a condições críticas identificadas nos dados que recolhe. Isso pode ser enviar um email ou de texto para um administrador responsável para investigar um problema. Ou também pode iniciar um processo automatizado que tenta corrigir uma condição de erro.


### <a name="alerts"></a>Alertas
[Alertas no Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-alerts.md) notificado proativamente das condições críticas e potencialmente tentar tomar medidas corretivas. Regras de alerta com base em métricas fornecem quase em tempo real de alerta com base em valores numéricos, enquanto as regras com base em registos permitem para lógica complexa, em todos os dados de várias origens.

Regras em utilização do Azure Monitor de alerta [grupos de ação](../monitoring-and-diagnostics/monitoring-action-groups.md), que contêm conjuntos de exclusivos de destinatários e ações que podem ser partilhadas entre várias regras. Com base nos seus requisitos, grupos de ação podem executar ações como através de webhooks ter alertas iniciar ações externas ou integrar com as suas ferramentas ITSM.

![Alertas](media/overview/alerts.png)

### <a name="autoscale"></a>Dimensionamento Automático
Dimensionamento automático permite que tenha a quantidade certa de recursos em execução para processar a carga na sua aplicação. Permite-lhe criar regras que utilizem métricas recolhidas pelo Azure Monitor para determinar quando deve adicionar automaticamente os recursos para processar os aumentos de carga e também poupar dinheiro ao remover os recursos que estão parados inativo. Especifique um número mínimo e máximo de instâncias e a lógica para quando aumentar ou diminuir recursos.

![Dimensionamento Automático](media/overview/autoscale.png)

## <a name="visualizing-monitoring-data"></a>Visualizar dados de monitorização
[Visualizações](visualizations.md) como gráficos e tabelas são ferramentas eficazes para resumir os dados de monitorização e apresentá-los para audiências diferentes. O Azure Monitor tem suas próprias funcionalidades para visualizar dados de monitorização e tira partido de outros serviços do Azure para publicá-la para audiências diferentes.

### <a name="dashboards"></a>Dashboards
[Dashboards do Azure](../azure-portal/azure-portal-dashboards.md) permitem-lhe combinar diferentes tipos de dados, incluindo métricas e registos, num único painel no [portal do Azure](https://portal.azure.com). Opcionalmente, pode partilhar o dashboard com outros utilizadores do Azure. Elementos em todo o Azure Monitor podem ser adicionados a um dashboard do Azure, além da saída de qualquer gráfico de consulta ou métricas de registo. Por exemplo, pode criar um dashboard que combina os mosaicos que mostram um gráfico de métricas, uma tabela de registos de atividade, um gráfico de utilização do Application Insights e o resultado de uma consulta do Log Analytics.

![Dashboard](media/overview/dashboard.png)

### <a name="views"></a>Vistas
[Modos de exibição no Azure Monitor](../azure-monitor/platform/view-designer.md) apresentam visualmente os dados de registo no Log Analytics.  Cada vista inclui um único mosaico Desagrega até uma combinação de visualizações, como barras e linhas de gráficos, além de listas que resumem os dados críticos.  Soluções de monitorização incluem vistas que resumem os dados para uma aplicação específica e pode criar suas próprias vistas para apresentar dados de uma pesquisa de registos do Log Analytics. Como outros elementos no Azure Monitor, os modos de exibição podem ser adicionados para dashboards do Azure.

![Vista do Log Analytics](media/overview/view.png)

### <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com) é um serviço de análise de negócio que fornece visualizações interativas numa variedade de origens de dados e é um meio eficaz de disponibilizar dados para outras pessoas dentro e fora da sua organização. Pode configurar o Power BI para [importar automaticamente os dados de registo do Azure Monitor](../log-analytics/log-analytics-powerbi.md) para aproveitar essas visualizações adicionais.


![Power BI](media/overview/power-bi.png)


## <a name="integrate-and-export-data"></a>Integrar e exportar dados
Muitas vezes, terá que o requisito para integrar o Azure Monitor com outros sistemas e para criar soluções personalizadas que utilizam os seus dados de monitorização. Outros serviços do Azure funcionam com o Azure Monitor para fornecer essa integração.

### <a name="event-hub"></a>Hub de Eventos
[Os Hubs de eventos do Azure](https://docs.microsoft.com/azure/event-hubs) é um serviço de ingestão plataforma e o evento de transmissão em fluxo que pode transformar e armazenar dados em qualquer fornecedor de análise em tempo real ou adaptadores de criação de batches/armazenamento. Utilizar os Hubs de eventos para [transmitir dados de registo do Azure Monitor](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md) para SIEM e ferramentas de monitorização de parceiros.

> [!VIDEO https://www.youtube.com/embed/SPHxCgbcvSw]

### <a name="logic-apps"></a>Aplicações Lógicas
[O Logic Apps](https://azure.microsoft.com/services/logic-apps) é um serviço que permite-lhe automatizar tarefas e processos empresariais com fluxos de trabalho que se integram com serviços e sistemas diferentes. Estão disponíveis atividades que de leitura e escrita de métricas e registos no Azure Monitor, que permite-lhe criar fluxos de trabalho de integração com uma variedade de outros sistemas.

![Aplicação Lógica](platform/media/collect-activity-logs-subscriptions/log-analytics-logic-apps-activity-log-overview.png)

### <a name="api"></a>API
Várias APIs estão disponíveis para ler e escrever métricas e registos de e para o Azure Monitor, além de aceder a alertas gerados pelo. Também pode configurar e obter alertas. Isso fornece essencialmente ilimitadas possibilidades para criar soluções personalizadas que se integram com o Azure Monitor.

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre:

* [Métricas e registos](../azure-monitor/platform/data-collection.md) para os dados recolhidos pelo Monitor do Azure.
* [Origens de dados](../azure-monitor/platform/data-sources.md) para como os diferentes componentes da sua aplicação enviam telemetria.
* [Log Analytics](../azure-monitor/log-query/log-query-overview.md) para analisar os dados recolhidos.
