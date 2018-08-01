---
title: Recolhe a monitorização dos dados no Azure | Documentos da Microsoft
description: Descrição geral dos dados de monitorização que são recolhidos a partir de aplicações e serviços no Azure e as ferramentas utilizadas para analisá-lo.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2018
ms.author: bwren
ms.openlocfilehash: 35580d71aa2592fa94f42cfdbad3c192acc303c5
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39363990"
---
# <a name="collect-monitoring-data-in-azure"></a>Recolher dados de monitorização no Azure
Este artigo fornece uma visão geral dos dados de monitorização que são recolhidos a partir de aplicações e serviços no Azure. Também descreve as ferramentas que pode usar para analisar os dados. 

## <a name="types-of-monitoring-data"></a>Tipos de dados de monitorização
Todos os dados de monitorização adapta-se um dos dois tipos fundamentais, as métricas ou registos. Cada tipo tem características distintas e é mais adequado para cenários específicos.

### <a name="metrics"></a>Métricas
As métricas são valores numéricos que descrevem algum aspeto de um sistema num determinado momento. Eles incluem:

* Dados distintos, incluindo o valor em si.
* O tempo que o valor foi recolhido.
* O tipo de medida, que representa o valor.
* O recurso de que o valor está associado. 

Métricas são recolhidas em intervalos regulares, independentemente do valor é alterado. Por exemplo, poderá recolher utilização do processador de uma máquina virtual a cada minuto ou o número de utilizadores com sessão iniciada sua aplicação a cada 10 minutos.

As métricas são leves e com capacidade de oferecer suporte a cenários em tempo real em tempo quase. Eles são úteis para alertar porque as métricas podem ser convertidas com frequência, e um alerta pode ser acionado rapidamente com a lógica relativamente simples. Por exemplo, pode acionar um alerta quando uma métrica excede um valor de limiar. Ou pode acionar um alerta quando a diferença entre duas métricas atinge um determinado valor.

Métricas individuais normalmente fornecem pouco insight por conta própria. Eles fornecem um valor único sem qualquer contexto que não seja a comparação com um limiar simple. Eles são valiosos quando combinada com outras métricas para identificar padrões e tendências, ou quando combinado com os registos que fornecem o contexto em torno de valores específicos. 

Por exemplo, um determinado número de utilizadores na sua aplicação num determinado momento pode eventualmente indicar pouco sobre o estado de funcionamento da aplicação. Mas uma queda repentina de usuários, indicado por vários valores da mesma métrica, pode indicar um problema. Excessivas exceções emitidas pela aplicação e indicado por uma métrica separada, podem identificar um problema de aplicativo que está a causar o menu. Eventos que o aplicativo cria para identificar falhas em seus componentes podem ajudar a identificar a causa raiz.

Os alertas com base em registos não são como estando a responder como os alertas com base em métricas, mas podem incluir uma lógica mais complexa. Pode criar um alerta com base nos resultados de qualquer consulta que executa análises complexas em dados de várias origens.

### <a name="logs"></a>Registos
Os registos contêm diferentes tipos de dados organizados em registos com diferentes conjuntos de propriedades de cada tipo. Registos de podem conter valores numéricos, como as métricas, mas normalmente contêm dados de texto com descrições detalhadas. Eles ainda mais diferem entre as métricas em que elas variam em sua estrutura e, muitas vezes não são recolhidos em intervalos regulares.

Um tipo comum de entrada de log é um evento. Eventos são recolhidos esporadicamente. Eles são criados por uma aplicação ou serviço e, normalmente, incluem informações suficientes para fornecer contexto completo por conta própria. Por exemplo, um evento pode indicar que um recurso em particular foi criado ou modificado, um novo anfitrião iniciado em resposta ao aumento de tráfego, ou foi detetado um erro num aplicativo.

Os registos são especialmente úteis para combinar dados de uma variedade de origens, para análises complexas e tendências ao longo do tempo. Como o formato dos dados pode variar, os aplicativos podem criar registos personalizados usando a estrutura que eles precisam. Métricas ainda podem ser replicadas nos registos para combiná-los com outros dados de monitorização para fins de tendência e outras análises de dados.


## <a name="monitoring-tools-in-azure"></a>Ferramentas de monitorização no Azure
A monitorização dos dados no Azure é recolhida e analisada através das seguintes origens.

### <a name="azure-monitor"></a>Azure Monitor
São recolhidas métricas de recursos do Azure e aplicações no Azure Monitor. Dados métricos são integrados às páginas no portal do Azure para recursos do Azure. Para máquinas virtuais, os gráficos de tais métricas de utilização da CPU e da rede são apresentados para a máquina selecionada. 

Também pode analisar os dados utilizando [Explorador de métricas](../monitoring-and-diagnostics/monitoring-metric-charts.md), que mostra graficamente os valores de várias métricas ao longo do tempo. Pode ver os gráficos interativamente ou afixá-los a um dashboard para visualizá-los com outras visualizações. Também pode obter métricas utilizando o [do Azure de monitorização de REST API](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

Para obter mais informações sobre os dados de métrica que recolher diferentes tipos de recursos do Azure, consulte [origens de dados no Azure de monitorização](monitoring-data-sources.md). 

![Explorador de Métricas](media/monitoring-data-collection/metrics-explorer.png)


### <a name="activity-log"></a>Registo de Atividades 
O [registo de atividades do Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) armazena registos sobre a configuração e o estado de funcionamento dos serviços do Azure. Pode utilizar o Explorador de registo de atividade para ver estes registos no portal do Azure, mas elas são comumente [copiados para o Azure Log Analytics](../log-analytics/log-analytics-activity.md) que fosse analisado com outros dados de registo.

Pode utilizar o Explorador de registo de atividade para ver o registo de atividade filtrado de acordo com a determinados critérios. A maioria dos recursos também tem um **registo de atividades** opção no menu no portal do Azure. Ele exibe a atividade de Log Explorer filtrado para esse recurso. Também pode obter registos de Atividades utilizando o [API do REST de monitorização do Azure](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

![Explorador de registo de atividade](media/monitoring-data-collection/activity-log-explorer.png)


### <a name="log-analytics"></a>Log Analytics
O log Analytics disponibiliza uma plataforma de dados comuns para gerir no Azure. É o principal serviço que é utilizado para armazenamento e análise de registos no Azure. Recolhe dados de uma variedade de origens, incluindo os agentes nas máquinas virtuais, soluções de gestão e recursos do Azure. Pode copiar dados de outras origens, incluindo métricas e registo de atividades, para criar um repositório central completado dos dados de monitorização.

Log Analytics tem uma linguagem de consulta avançada para analisar os dados que recolhe. Pode usar [portais de pesquisa de registos](../log-analytics/log-analytics-log-search-portals.md) para interativamente escrever e testar consultas e analisar os resultados. Também pode [criar vistas](../log-analytics/log-analytics-view-designer.md) para visualizar os resultados de pesquisas de registos ou cole os resultados de uma consulta diretamente a um dashboard do Azure.  

Soluções de gestão incluem vistas e pesquisas de registos no Log Analytics para analisar os dados que eles recolhem. Outros serviços, como o Azure Application Insights, armazenam dados no Log Analytics e fornecem ferramentas adicionais para análise.  

![Registos](media/monitoring-data-collection/logs.png)

### <a name="application-insights"></a>Application Insights
Application Insights recolhe telemetria para aplicações web instalado numa variedade de plataformas. Ele armazena os respetivos dados no Azure Monitor e o Log Analytics. E fornece um amplo conjunto de ferramentas para analisar e visualizar os seus dados. Estas capacidades permitem-lhe utilizar um conjunto comum de serviços, como alertas, pesquisas de registos e dashboards que utiliza para outros de monitorização.


![Application Insights](media/monitoring-data-collection/app-insights.png)

### <a name="service-map"></a>Mapa de Serviços
Mapa de serviço fornece uma representação visual de máquinas virtuais com seus processos e as dependências. Armazena a maioria desses dados no Log Analytics para que pode analisá-los com outros dados de gestão. A consola do mapa de serviço também obtém dados do Log Analytics para apresentá-los no contexto da máquina virtual que está a ser analisado.

![Mapa de Serviços](media/monitoring-data-collection/service-map.png)


## <a name="transferring-monitoring-data"></a>Transferência de dados de monitorização

### <a name="metrics-to-logs"></a>Métricas para os registos
Pode replicar as métricas no Log Analytics para executar análises complexas com outros tipos de dados ao utilizar a sua linguagem de consulta avançada. Também pode manter os dados de registo por períodos mais longos do que de métricas, que lhe permitem realizar tendências ao longo do tempo. Quando as métricas ou outros dados de desempenho são armazenados no Log Analytics, que dados age como um registo. Utilize métricas para suportar perto de análise em tempo real e os alertas ao utilizar registos para fins de tendência e análise com outros dados.

Pode obter orientações para recolher métricas de recursos do Azure ao [registos do serviço de recolha do Azure e as métricas de utilização do Log Analytics](../log-analytics/log-analytics-azure-storage.md). Obtenha orientações para recolher métricas de recursos de recursos de PaaS do Azure no [configurar a recolha de métricas de recursos de PaaS do Azure com o Log Analytics](../log-analytics/log-analytics-collect-azurepass-posh.md).

### <a name="logs-to-metrics"></a>Registos para métricas
Conforme descrito anteriormente, as métricas estão mais responsivas do que os registos, para que possa criar alertas com menor latência e a um custo menor. O log Analytics recolhe uma quantidade significativa de dados numéricos que seriam adequado para as métricas, mas não estão armazenados no Azure Monitor. 

Um exemplo comum é que os dados de desempenho coletados por agentes e soluções de gestão. Alguns destes valores podem ser copiados para o Azure Monitor, onde eles estão disponíveis para alertas e análise com o Explorador de métricas.

A explicação desta funcionalidade está disponível em [mais rapidamente os alertas de métrica para os registos de agora em pré-visualização pública limitada](https://azure.microsoft.com/blog/faster-metric-alerts-for-logs-now-in-limited-public-preview/). A lista de valores de suporte está disponível em [suportada métricas e criação de métodos para novos alertas de métricas](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md).

### <a name="event-hubs"></a>Hubs de Eventos
Além de usar as ferramentas no Azure para analisar dados de monitorização, pode querer reencaminhá-lo para uma ferramenta externa, como um produto de management (SIEM) de eventos e informações de segurança. Este encaminhamento é geralmente feito através de [os Hubs de eventos do Azure](https://docs.microsoft.com/azure/event-hubs/). 

Pode obter orientações para os diferentes tipos de dados em [Azure Stream a monitorização dos dados para um hub de eventos para consumo por uma ferramenta externa](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre o [monitorização dos dados disponíveis](monitoring-data-sources.md) diferentes recursos no Azure. 