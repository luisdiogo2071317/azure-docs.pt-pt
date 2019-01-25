---
title: Analisar dados do Log Analytics no Azure Monitor | Documentos da Microsoft
description: Exigir uma pesquisa de registos para obter todos os dados do Log Analytics.  Este artigo descreve como novo registo de pesquisas são utilizadas no Log Analytics e fornece os conceitos que tem de compreender antes de criar um.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: bwren
ms.openlocfilehash: d3fc44456ac4f0df2bee35300c0f40728a40cb92
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54882252"
---
# <a name="analyze-log-analytics-data-in-azure-monitor"></a>Analisar dados do Log Analytics no Azure Monitor

Dados de registo recolhidos pelo Azure Monitor são armazenados numa área de trabalho do Log Analytics, que se baseia nos [Explorador de dados do Azure](/azure/data-explorer). Ele recolhe telemetria a partir de várias origens e utiliza a [linguagem do Explorador de dados de consulta](/azure/kusto/query) para recuperar e analisar dados.

> [!NOTE]
> O log Analytics anteriormente era tratado como seu próprio serviço no Azure. Ele agora é considerado uma parte do Azure Monitor e se concentra no armazenamento e análise de dados de registo com a sua linguagem de consulta. Funcionalidades que foram consideradas parte do Log Analytics, por exemplo, agentes Windows e Linux para a recolha de dados, modos de exibição para visualizar os dados existentes e alertas para ser notificado proativamente dos problemas, não foram alterados, mas agora são consideradas parte do Azure Monitor.



## <a name="log-queries"></a>Consultas de registo

Precisa de uma consulta de log para obter todos os dados do Log Analytics.  Esteja [análise de dados no portal](../log-query/portals.md), [configura uma regra do alerta](../platform/alerts-metric.md) para ser notificado de uma determinada condição ou ao obter dados com o [API do Log Analytics](https://dev.loganalytics.io/), irá utilizar uma consulta para especificar os dados que pretende.  Este artigo descreve como as consultas de registo são usadas no Log Analytics e fornece os conceitos que deve compreender antes de criar um.



## <a name="where-log-queries-are-used"></a>Em que são utilizadas consultas de registo

As diferentes formas que irá utilizar consultas do Log Analytics incluem o seguinte:

- **Portais.** Pode executar análises interativas de dados de registo no [portal do Azure](../log-query/portals.md).  Isto permite-lhe editar a consulta e analisar os resultados numa variedade de formatos e visualizações.  
- **Regras de alerta.** [Regras de alerta](../platform/alerts-overview.md) identificar proativamente os problemas dos dados na sua área de trabalho.  Cada regra de alerta baseia-se uma pesquisa de registos que está a ser executada automaticamente em intervalos regulares.  Os resultados são inspecionados para determinar se um alerta deve ser criado.
- **Dashboards.** Pode afixar os resultados de qualquer consulta numa [dashboard do Azure](../learn/tutorial-logs-dashboards.md) que permitem que visualize os dados de registos e de métricas em conjunto e, opcionalmente, partilhar com outros utilizadores do Azure. 
- **Modos de exibição.**  Pode criar visualizações de dados a serem incluídos nos dashboards do utilizador com [estruturador de vistas](../platform/view-designer.md).  Consultas de registo que fornecem os dados utilizados pelo [mosaicos](../platform/view-designer-tiles.md) e [partes de visualização](../platform/view-designer-parts.md) em cada vista.  
- **Exportar.**  Quando importa dados da área de trabalho do Log Analytics para o Excel ou [Power BI](../platform/powerbi.md), cria uma consulta de registo para definir os dados para exportar.
- **PowerShell.** Pode executar um script do PowerShell a partir de uma linha de comando ou um runbook da automatização do Azure que utiliza [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightssearchresults?view=azurermps-4.0.0) para recuperar dados do Log Analytics.  Este cmdlet requer uma consulta para determinar os dados a obter.
- **API do log Analytics.**  O [a API de pesquisa de registos do Log Analytics](../platform/alerts-overview.md) permite que qualquer cliente de REST API recuperar dados de registo de área de trabalho.  O pedido de API inclui uma consulta que é executada no Log Analytics para determinar os dados a obter.

![Pesquisas de registos](media/log-query-overview/queries-overview.png)

## <a name="write-a-query"></a>Escrever uma consulta
Utilizações de análise de registo [uma versão da linguagem de consulta Data Explorer](../log-query/get-started-queries.md) para recuperar e analisar dados de registo de diversas formas.  Geralmente começa com a consultas básicas e, em seguida, avançar para utilizar as funções mais avançadas, como os seus requisitos se tornam mais complexos.

A estrutura básica de uma consulta é uma tabela de origem, seguida de uma série de operadores, separados por um caráter de pipe `|`.  Em conjunto pode encadear vários operadores para refinar os dados e realizar funções avançadas.

Por exemplo, suponha que quisesse localizar os computadores de dez principais com a maioria dos eventos de erro ao longo do dia anterior.

```Kusto
Event
| where (EventLevelName == "Error")
| where (TimeGenerated > ago(1days))
| summarize ErrorCount = count() by Computer
| top 10 by ErrorCount desc
```

Ou talvez queira localizar os computadores que ainda não tinham um heartbeat no último dia.

```Kusto
Heartbeat
| where TimeGenerated > ago(7d)
| summarize max(TimeGenerated) by Computer
| where max_TimeGenerated < ago(1d)  
```

E quanto um gráfico de linhas com a utilização do processador para cada computador a partir da última semana?

```Kusto
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| where TimeGenerated  between (startofweek(ago(7d)) .. endofweek(ago(7d)) )
| summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5min)
| render timechart    
```

Pode ver nestes exemplos rápidos que, independentemente do tipo de dados que está trabalhando com a estrutura da consulta é semelhante.  Pode dividi-la em etapas distintas, onde os dados resultantes de um comando são enviados pelo pipeline para o próximo comando.

Também pode consultar dados em áreas de trabalho do Log Analytics na sua subscrição.

```Kusto
union Update, workspace("contoso-workspace").Update
| where TimeGenerated >= ago(1h)
| summarize dcount(Computer) by Classification 
```

## <a name="how-log-analytics-data-is-organized"></a>Como os dados do Log Analytics são organizados
Quando cria uma consulta, comece por determinar que tabelas contêm os dados que estava procurando. Diferentes tipos de dados estão divididos em tabelas dedicadas em cada [área de trabalho do Log Analytics](../learn/quick-create-workspace.md).  Documentação para origens de dados inclui o nome do tipo de dados que ele cria e uma descrição de cada uma de suas propriedades.  Muitas consultas só vão precisar dados a partir de uma única tabela, mas podem usar uma variedade de opções para incluir dados de várias tabelas.

Embora [Application Insights](../app/app-insights-overview.md) armazena dados de aplicação, tais como pedidos, exceções, rastreios e utilização do Log Analytics, estes dados são armazenados numa partição diferente do que os dados de registo. Utilizar a mesma linguagem de consulta para aceder a estes dados, mas tem de utilizar o [consola do Application Insights](../app/analytics.md) ou [API de REST do Application Insights](https://dev.applicationinsights.io/) para aceder ao mesmo. Pode usar [consultas entre recursos](../log-query/cross-workspace-query.md) para combinar dados do Application Insights com outros dados no Log Analytics.


![Tabelas](media/log-query-overview/queries-tables.png)







## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre o [pesquisas de registos de portais que utilizar para criar e editar](../log-query/portals.md).
- Veja uma [tutorial sobre como escrever consultas](../log-query/get-started-queries.md) usando a nova linguagem de consulta.
