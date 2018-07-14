---
title: Recolher a monitorização dos dados no Azure | Documentos da Microsoft
description: Descrição geral dos dados de monitorização que são recolhidos a partir de aplicações e serviços no Azure e as ferramentas usado para analisá-lo.
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
ms.openlocfilehash: d3ebd512f8244de74c009ac8a2936ed8e817dad9
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991511"
---
# <a name="collecting-monitoring-data-in-azure"></a>Recolher dados de monitorização no Azure
Este artigo fornece uma visão geral dos dados de monitorização que são recolhidos a partir de aplicações e serviços no Azure e as ferramentas utilizadas para analisá-lo. 

## <a name="types-of-monitoring-data"></a>Tipos de dados de monitorização
Todos os dados de monitorização adapta-se um dos dois tipos fundamentais, as métricas ou registos. Cada tipo tem características distintas e é mais adequado para cenários específicos, conforme descrito abaixo.

### <a name="metrics"></a>Métricas
As métricas são valores numéricos que descrevem algum aspeto de um sistema num momento específico no tempo. Eles incluem dados distintos, incluindo o valor em si, o tempo que foi recolhido o valor, o tipo de medição que representa o valor e o recurso em particular que o valor está associado. Métricas são recolhidas em intervalos regulares, independentemente do valor é alterado. Por exemplo, pode recolher a utilização do processador de uma máquina virtual a cada minuto ou o número de utilizadores com sessão iniciado sua aplicação a cada 10 minutos.

As métricas são leves e com capacidade de oferecer suporte a cenários em tempo real em tempo quase. Eles são particularmente úteis para alertar, uma vez que as métricas podem ser convertidas com frequência e um alerta pode ser acionado rapidamente com a lógica relativamente simples. Por exemplo, pode acionar um alerta quando uma métrica excede um valor de limiar ou acionar um alerta quando a diferença entre o valor de duas métricas atinge um determinado valor.

Métricas individuais normalmente fornecem pouco insight por conta própria. Eles fornecem um valor único sem qualquer contexto que não seja a comparação com um limiar simple. Eles são valiosos porém quando combinada com outras métricas para identificar padrões e tendências ou quando combinado com os registos que fornecem o contexto em torno de valores específicos. Por exemplo, um determinado número de utilizadores na sua aplicação num determinado momento pode informar a pouco sobre o estado de funcionamento da aplicação. Uma queda repentina sessão dos utilizadores, embora indicado por vários valores da mesma métrica pode indicar um problema. Excessivas exceções emitidas pela aplicação e indicado por uma métrica separada podem identificar um problema de aplicativo que faz com que a lista. Eventos criados pelo aplicativo em particular a identificação de falha de componentes da aplicação podem ajudar a identificar a causa raiz.

Os alertas com base em registos não são como estando a responder como os alertas com base em métricas, mas podem incluir uma lógica mais complexa. Pode criar um alerta com base nos resultados de qualquer consulta que executa análises complexas em dados de várias origens.

### <a name="logs"></a>Registos
Os registos contêm diferentes tipos de dados organizados em registos com diferentes conjuntos de propriedades de cada tipo. Os registos podem conter valores numéricos, como as métricas, mas normalmente contêm dados de texto com descrições detalhadas. Eles ainda mais diferem entre as métricas em que elas variam em sua estrutura e, muitas vezes não são recolhidos em intervalos regulares.

Um tipo comum de entrada de log é um evento. Os eventos são recolhidos esporadicamente, à medida que são criados por uma aplicação ou serviço e, normalmente, incluem informações suficientes para fornecer contexto completo por conta própria.  Por exemplo, um evento pode indicar que um recurso em particular foi criado ou modificado, um novo anfitrião iniciadas em resposta ao aumento no tráfego ou foi detetado um erro num aplicativo.

Os registos são particularmente úteis para combinar dados de uma variedade de origens para análises complexas e tendências ao longo do tempo. Como o formato dos dados pode variar, os aplicativos podem criar registos personalizados usando a estrutura que eles precisam. Métricas ainda podem ser replicadas nos registos para combiná-los com outros dados de monitorização para fins de tendência e outras análises de dados.


## <a name="monitoring-tools-in-azure"></a>Ferramentas de monitorização no Azure
A monitorização dos dados no Azure é recolhida e analisados através de várias origens que são descritas nas secções seguintes.

### <a name="azure-metrics"></a>Métricas do Azure
Métricas de recursos do Azure e aplicações são recolhidas em métricas do Azure. Dados métricos são integrados às páginas no portal do Azure para recursos do Azure específicos, como máquinas virtuais, que incluem gráficos de métricas de tais como a utilização de CPU e da rede para a máquina selecionada. Também podem ser analisados com as [Explorador de métricas](../monitoring-and-diagnostics/monitoring-metric-charts.md) que pode gráfico os valores de várias métricas ao longo do tempo.  Pode ver os gráficos interativamente ou afixá-los a um dashboard para visualizá-los com outras visualizações. Também pode obter métricas com o [do Azure de monitorização de REST API](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

Pode obter detalhes sobre os dados de métrica que são recolhidos por diferentes tipos de recursos do Azure ao [origens de dados no Azure de monitorização](monitoring-data-sources.md). 

![Explorador de Métricas](media/monitoring-data-collection/metrics-explorer.png)


### <a name="azure-activity-log"></a>Registo de atividades do Azure 
O [registo de atividades do Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) armazena registos sobre a configuração e o estado de funcionamento dos serviços do Azure. Pode utilizar o Explorador de registo de atividade para ver estes registos no portal do Azure, mas elas são comumente [copiados para o Log Analytics](../log-analytics/log-analytics-activity.md) que fosse analisado com outros dados de registo.

Pode utilizar o Explorador de registo de atividade para ver o registo de atividade filtrado de acordo com a determinados critérios.  A maioria dos recursos terão também uma opção no respetivo menu no portal do Azure, que exibe a atividade de Log Explorer filtrada para esse recurso o registo de atividades. Também pode obter registos de atividades com o [do Azure de monitorização de REST API](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

![Explorador de registo de atividade](media/monitoring-data-collection/activity-log-explorer.png)


### <a name="log-analytics"></a>Log Analytics
O log Analytics disponibiliza uma plataforma de dados comuns para gerir no Azure. É o serviço principal utilizado para armazenamento e análise de registos no Azure, recolher dados de uma variedade de origens, incluindo os agentes nas máquinas virtuais, soluções de gestão e vários recursos do Azure. Dados de outras origens, incluindo métricas e registo de atividades podem ser copiados para o Log Analytics para criar um repositório central completado dos dados de monitorização.

Log Analytics tem uma linguagem de consulta avançada para analisar os dados que coleta.  Pode usar [portais de pesquisa de registos](../log-analytics/log-analytics-log-search-portals.md) para interativamente escrever e testar consultas e analisar os resultados. Também pode [criar vistas](../log-analytics/log-analytics-view-designer.md) para visualizar os resultados de pesquisas de registos ou cole os resultados de uma consulta diretamente a um dashboard do Azure.  Soluções de gestão incluem vistas e pesquisas de registos no Log Analytics para analisar os dados que recolher. Outros serviços como o Application Insights armazenam dados no Log Analytics e fornecem ferramentas adicionais para análise.  

![Registos](media/monitoring-data-collection/logs.png)

### <a name="application-insights"></a>Application Insights
Application Insights recolhe telemetria para aplicações web instalado numa variedade de plataformas. Ele armazena os respetivos dados de métricas do Azure e o Log Analytics e fornece um amplo conjunto de ferramentas avançadas, sobre as ferramentas existentes para analisar estes dados, para analisar e visualizar os seus dados. Isto permite-lhe tirar partido de um conjunto comum de serviços, como alertas, pesquisas de registos e dashboards que utiliza para outros de monitorização.


![App Insights](media/monitoring-data-collection/app-insights.png)

### <a name="service-map"></a>Mapa de Serviços
Mapa de serviço fornece uma representação visual de máquinas virtuais com seus processos e as dependências. Armazena a maioria desses dados no Log Analytics para que pode analisá-los com outros dados de gestão. A consola do mapa de serviço também obtém dados do Log Analytics para apresentá-los no contexto da máquina virtual a ser analisado.

![Mapa de Serviços](media/monitoring-data-collection/service-map.png)


## <a name="transferring-monitoring-data"></a>Transferência de dados de monitorização

### <a name="metrics-to-logs"></a>Métricas para os registos
Métricas também podem ser replicadas para o Log Analytics para realizar análises complexas com outros tipos de dados com a sua linguagem de consulta avançada. Também pode manter os dados de registo por períodos mais longos do que de métricas, que permite efetuar tendências ao longo do tempo. Quando as métricas ou outros dados de desempenho são armazenados no Log Analytics, que dados age como um registo. Utilize métricas para suportar perto de análise em tempo real e os alertas ao utilizar registos para fins de tendência e análise com outros dados.

Pode obter orientações para recolher métricas de recursos do Azure ao [registos do serviço de recolha do Azure e as métricas de utilização do Log Analytics](../log-analytics/log-analytics-azure-storage.md). Obtenha orientações para recolher métricas de recursos de recursos de PaaS do Azure no [configurar a recolha de métricas de recursos de PaaS do Azure com o Log Analytics](../log-analytics/log-analytics-collect-azurepass-posh.md).

### <a name="logs-to-metrics"></a>Registos para métricas
Conforme descrito acima, as métricas estão mais responsivas do que os registos que lhe permite criar alertas com menor latência e a um custo menor. O log Analytics recolhe uma quantidade significativa de dados numéricos que seriam adequado para as métricas, mas não estão armazenados em métricas do Azure. Um exemplo comum é que os dados de desempenho coletados por agentes e soluções de gestão. Alguns destes valores podem ser copiados as métricas do Azure onde eles estão disponíveis para alertas e análise com o Explorador de métricas.

A explicação desta funcionalidade está disponível em [mais rapidamente os alertas de métrica para os registos de agora em pré-visualização pública limitada](https://azure.microsoft.com/blog/faster-metric-alerts-for-logs-now-in-limited-public-preview/). A lista de valores de suporte está disponível em [suportada métricas e criação de métodos para novos alertas de métricas](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md).

### <a name="event-hub"></a>Hub de Eventos
Além de usar as ferramentas no Azure para analisar dados de monitorização, pode querer reencaminhar-o para uma ferramenta externa desse tipo de informações de segurança e produtos de gestão (SIEM) de eventos. Isto é geralmente feito utilizando [Hub de eventos do Azure](https://docs.microsoft.com/azure/event-hubs/). 

Pode obter orientações para os diferentes tipos de dados em [Azure Stream a monitorização dos dados para um hub de eventos para consumo por uma ferramenta externa](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre o [monitorização dos dados disponíveis](monitoring-data-sources.md) diferentes recursos no Azure. 