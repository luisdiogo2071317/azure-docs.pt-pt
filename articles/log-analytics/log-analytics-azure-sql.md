---
title: Solução de análise de SQL do Azure no Log Analytics | Microsoft Docs
description: Solução de análise de SQL do Azure ajuda-o a gerir as bases de dados SQL do Azure
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/03/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: f57a47677f752a644975a25fa746d78bced5d766
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133030"
---
# <a name="monitor-azure-sql-databases-using-azure-sql-analytics-preview"></a>Monitorizar bases de dados do SQL do Azure através da análise de SQL do Azure (pré-visualização)

![Símbolo de análise de SQL do Azure](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Análise de SQL do Azure é uma solução para monitorização do desempenho de bases de dados do Azure SQL na escala em vários conjuntos elásticos e subscrições de monitorização de nuvem. Este recolhe e visualiza importantes métricas de desempenho de SQL Database do Azure com intelligence incorporado para resolução de problemas na parte superior do desempenho. 

Ao utilizar as métricas que recolher com a solução, pode criar regras personalizadas de monitorização e alertas. A solução ajuda-o a identificar problemas em cada camada de pilha de aplicação. Utiliza a métrica do diagnóstico do Azure, juntamente com as vistas de análise de registos para apresentar dados sobre todas as suas bases de dados SQL do Azure e conjuntos elásticos numa única área de trabalho do Log Analytics. Análise de registos ajuda-o a recolher, correlacionar e visualizar dados estruturados e não estruturados.

Atualmente, esta solução de pré-visualização suporta até 150.000 bases de dados de SQL do Azure e 5000 conjuntos elásticos SQL por área de trabalho.

Para obter uma descrição prática utilizando a solução de análise de SQL do Azure e para cenários de utilização normal, veja o vídeo incorporado:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-Intelligent-Insights-for-Improving-Azure-SQL-Database-Performance/player]
>

## <a name="connected-sources"></a>Origens ligadas

Análise de SQL do Azure é uma solução suporte de transmissão em fluxo de telemetria de diagnóstico para bases de dados do Azure SQL e conjuntos elásticos de monitorização de nuvem. Como utilizar os agentes para ligar ao serviço análise de registos, a solução não suporta a conectividade com o Windows, Linux ou recursos do SCOM, consulte a tabela de compatibilidade abaixo.

| Origem Ligada | Suporte | Descrição |
| --- | --- | --- |
| **[Diagnóstico do Azure](log-analytics-azure-storage.md)** | **Sim** | Dados de métricas e registo do Azure são enviados para análise de registos diretamente pelo Azure. |
| [Conta de armazenamento do Azure](log-analytics-azure-storage.md) | Não | Análise de registos não ler os dados a partir de uma conta de armazenamento. |
| [Agentes do Windows](log-analytics-windows-agent.md) | Não | Agentes diretos do Windows não são utilizados pela solução. |
| [Agentes do Linux](log-analytics-linux-agents.md) | Não | Direcionar os agentes não são utilizados pela solução de Linux. |
| [Grupo de gestão do SCOM](log-analytics-om-agents.md) | Não | Uma ligação direta do agente do SCOM para análise de registos não é utilizada pela solução. |

## <a name="configuration"></a>Configuração

Execute os seguintes passos para adicionar a solução de análise de SQL do Azure a sua área de trabalho.

1. Adicionar a solução de análise de SQL do Azure a sua área de trabalho de [do Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview).
2. No portal do Azure, clique em **+ criar um recurso**, em seguida, procure **análise do Azure SQL**.  
    ![Monitorização + Gestão](./media/log-analytics-azure-sql/monitoring-management.png)
3. Selecione **análise de SQL do Azure (pré-visualização)** da lista
4. No **análise de SQL do Azure (pré-visualização)** área, clique em **criar**.  
    ![Criar](./media/log-analytics-azure-sql/portal-create.png)
5. No **criar nova solução** área, crie uma nova ou selecione uma área de trabalho existente que pretende adicionar a solução para e, em seguida, clique em **criar**.  
    ![Adicione a área de trabalho](./media/log-analytics-azure-sql/add-to-workspace.png)

### <a name="configure-azure-sql-databases-and-elastic-pools-to-stream-diagnostics-telemetry"></a>Configurar bases de dados do Azure SQL e conjuntos elásticos a telemetria de diagnóstico de fluxo

Assim que tiver criado a solução de análise de SQL do Azure na sua área de trabalho, para monitorizar o desempenho de bases de dados do Azure SQL e/ou conjuntos elásticos, é necessário para **configurar cada** da SQL Database do Azure e pretende de recursos de agrupamento elástico para monitorizar a transmitir a telemetria de diagnóstico para a solução.

- Ativar o diagnóstico do Azure para as bases de dados SQL do Azure e os conjuntos elásticos e [configurá-los para enviar os seus dados para análise de registos](../sql-database/sql-database-metrics-diag-logging.md).

### <a name="to-configure-multiple-azure-subscriptions"></a>Para configurar várias subscrições do Azure

Para suportar várias subscrições, utilize o script do PowerShell do [registo de métricas de recurso de ativar o Azure com o PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/). Forneça o ID de recurso da área de trabalho como um parâmetro ao executar o script para enviar dados de diagnóstico de recursos de uma subscrição do Azure para uma área de trabalho outra subscrição do Azure.

**Exemplo**

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-the-solution"></a>Utilizar a solução

Quando adiciona a solução para a sua área de trabalho, o mosaico de análise de SQL do Azure é adicionado à sua área de trabalho e é apresentado na descrição geral. O mosaico mostra o número de bases de dados SQL do Azure e conjuntos elásticos SQL do Azure que está ligada a solução.

![Mosaico de análise de SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a>Visualizar dados de análise de SQL do Azure

Clique em de **análise do Azure SQL** mosaico para abrir o dashboard de análise de SQL do Azure. O dashboard inclui a descrição geral de todas as bases de dados que são monitorizados através de diferentes perspetivas. Para diferentes perspetivas funcione, tem de ativar métricas adequadas ou registos nos seus recursos do SQL Server para transmissão em fluxo a área de trabalho do Log Analytics do Azure.

![Descrição geral da análise SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-overview.png)

Selecionar qualquer uma dos mosaicos, abre-se de um relatório de desagregação em perspetiva específica. Depois de perspetiva estiver selecionada, é aberto o relatório de desagregação.

![Tempos limite de análise SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-metrics.png)

Cada perspetiva fornece resumos na subscrição, o servidor, o conjunto elástico e o nível de base de dados. Além disso, cada perspetiva mostra uma perspetiva específico para o relatório no lado direito. Selecionar a subscrição, o servidor, o conjunto ou base de dados da lista continua a desagregação.

| Perspetiva | Descrição |
| --- | --- |
| Recurso por tipo | Perspetiva contagens de todos os recursos monitorizados. Desagregar fornece o resumo das métricas DTU e GB. |
| Informações | Fornece hierárquica desagregar para Insights inteligente. Saiba mais sobre as informações inteligentes. |
| Erros | Fornece desagregar hierárquica erros de SQL que ocorreram nas bases de dados. |
| Tempos limite | Fornece hierárquica desagregar para tempos limite SQL que ocorreram nas bases de dados. |
| Bloqueios | Fornece hierárquica desagregar para blockings SQL que ocorreram nas bases de dados. |
| Esperas de base de dados | Fornece hierárquica desagregar para estatísticas de espera SQL no nível da base de dados. Inclui resumos de tempo de espera total e o tempo de espera por tipo de espera. |
| Duração de consulta | Fornece hierárquica desagregar para as estatísticas de execução da consulta como a utilização da CPU, duração de consulta, utilização de registo e/s e utilização de dados e/s. |
| Esperas de consulta | Fornece hierárquica desagregar para as estatísticas de espera de consulta por categoria de espera. |

### <a name="intelligent-insights-report"></a>Relatório de Insights inteligente

Base de dados SQL do Azure [Insights inteligente](../sql-database/sql-database-intelligent-insights.md) permite-lhe determinar o que acontece com o desempenho de base de dados. Todas as informações inteligente recolhidos podem ser visualizadas e acedidas através de perspetiva Insights.

![Informações de análise SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pool-and-database-reports"></a>Relatórios de conjunto elástico e base de dados

Conjuntos elásticos e bases de dados têm os seus próprios relatórios específicos que mostram todos os dados recolhidos para o recurso no tempo especificado.

![Base de dados de análise SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-database.png)

![Agrupamento elástico de análise SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Relatórios de consulta

Através da duração de consulta e a consulta aguarda perspetivas, pode correlacionar o desempenho de qualquer consulta através do relatório de consulta. Este relatório compara o desempenho de consulta em bases de dados diferentes e torna mais fácil identificar bases de dados que executar a consulta selecionada bem versus aqueles que são lenta.

![Consultas de análises SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-queries.png)

### <a name="analyze-data-and-create-alerts"></a>Analisar dados e criar alertas

Pode facilmente [criam alertas](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) com os dados provenientes de recursos de SQL Database do Azure. Seguem-se algumas útil [pesquisa registo](log-analytics-log-searches.md) utilizam consultas que pode utilizar com um alerta de registo:



*DTU elevado na base de dados SQL do Azure*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL" and ResourceId contains "/DATABASES/" and MetricName=="dtu_consumption_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

*Elevada DTU num agrupamento elástico de bases de dados SQL do Azure*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL" and ResourceId contains "/ELASTICPOOLS/" and MetricName=="dtu_consumption_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```



## <a name="next-steps"></a>Passos Seguintes

- Utilize [pesquisas de registo](log-analytics-log-searches.md) na análise de registos para ver os dados detalhados de SQL do Azure.
- [Criar os seus próprios dashboards](log-analytics-dashboards.md) mostrar os dados de SQL do Azure.
- [Criar alertas](log-analytics-alerts.md) quando ocorrem eventos específicos de SQL do Azure.
