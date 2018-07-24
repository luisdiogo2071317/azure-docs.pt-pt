---
title: Solução de análise de SQL do Azure no Log Analytics | Documentos da Microsoft
description: Solução de análise de SQL do Azure ajuda-o a gerir as suas bases de dados SQL do Azure
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
ms.openlocfilehash: 440e16416b8567178c61c3d6ce2155e0e331521c
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39216330"
---
# <a name="monitor-azure-sql-databases-using-azure-sql-analytics-preview"></a>Monitorizar as bases de dados de SQL do Azure através da análise de SQL do Azure (pré-visualização)

![Símbolo de análise de SQL do Azure](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Análise de SQL do Azure é uma solução de monitorização para monitorização do desempenho de bases de dados do Azure SQL em escala em vários conjuntos elásticos e as subscrições de nuvem. Ele recolhe e visualiza métricas de desempenho de base de dados do Azure SQL importantes com a inteligência incorporada para resolução de problemas na parte superior de desempenho. 

Ao utilizar as métricas que recolhe com a solução, pode criar regras personalizadas de monitorização e alertas. A solução ajuda-o a identificar problemas em cada camada de sua pilha de aplicativos. Ele usa métricas de diagnóstico do Azure, juntamente com os modos de exibição do Log Analytics para apresentar dados sobre todas as suas bases de dados SQL do Azure e conjuntos elásticos um único espaço de trabalho do Log Analytics. O log Analytics ajuda-o a recolher, correlacionar e visualizar dados estruturados e não estruturados.

Atualmente, esta solução de pré-visualização suporta um máximo de 150 000 bases de dados do Azure SQL e 5000 conjuntos elásticos SQL por área de trabalho.

Para obter uma descrição geral prática sobre como utilizar a solução de análise de SQL do Azure e para cenários de uso típico, veja o vídeo incorporado:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-Intelligent-Insights-for-Improving-Azure-SQL-Database-Performance/player]
>

## <a name="connected-sources"></a>Origens ligadas

Análise de SQL do Azure é uma cloud de monitorização a solução suporte de transmissão em fluxo de telemetria de diagnóstico para conjuntos elásticos e bases de dados do Azure SQL. Como não utilizar agentes para ligar ao serviço Log Analytics, a solução não suporta a conectividade com o Windows, Linux ou de recursos do SCOM, veja a tabela de compatibilidade abaixo.

| Origem Ligada | Suporte | Descrição |
| --- | --- | --- |
| **[Diagnóstico do Azure](log-analytics-azure-storage.md)** | **Sim** | Dados de registo e métricas do Azure são enviados para o Log Analytics diretamente pelo Azure. |
| [Conta de armazenamento do Azure](log-analytics-azure-storage.md) | Não | O log Analytics não ler os dados de uma conta de armazenamento. |
| [Agentes do Windows](log-analytics-windows-agent.md) | Não | Agentes diretos do Windows não são utilizados pela solução. |
| [Agentes do Linux](log-analytics-linux-agents.md) | Não | Agentes diretos do Linux não são utilizados pela solução. |
| [Grupo de gestão do SCOM](log-analytics-om-agents.md) | Não | Uma ligação direta do agente do SCOM para o Log Analytics não é utilizada pela solução. |

## <a name="configuration"></a>Configuração

Execute os seguintes passos para adicionar a solução de análise de SQL do Azure à sua área de trabalho.

1. Adicionar a solução de análise de SQL do Azure à sua área de trabalho de [do Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview).
2. No portal do Azure, clique em **+ criar um recurso**, em seguida, procure **análise de SQL do Azure**.  
    ![Monitorização + Gestão](./media/log-analytics-azure-sql/monitoring-management.png)
3. Selecione **análise de SQL do Azure (pré-visualização)** da lista
4. Na **análise de SQL do Azure (pré-visualização)** área, clique em **criar**.  
    ![Criar](./media/log-analytics-azure-sql/portal-create.png)
5. Na **criar nova solução** área, crie um novo ou selecione uma área de trabalho existente que pretende adicionar a solução para e, em seguida, clique em **criar**.  
    ![Adicionar área de trabalho](./media/log-analytics-azure-sql/add-to-workspace.png)

### <a name="configure-azure-sql-databases-and-elastic-pools-to-stream-diagnostics-telemetry"></a>Configurar conjuntos elásticos e bases de dados do Azure SQL para a telemetria de diagnóstico do stream

Depois de criar a solução de análise de SQL do Azure na sua área de trabalho, para monitorizar o desempenho de bases de dados do Azure SQL e/ou conjuntos elásticos, precisará **configure cada** de base de dados do Azure SQL e o recurso de conjunto elástico desejar para monitorizar a respetiva telemetria de diagnóstico para a solução do stream.

- Ativar o diagnóstico do Azure para as suas bases de dados SQL do Azure e conjuntos elásticos e [configurá-las para enviar os seus dados para o Log Analytics](../sql-database/sql-database-metrics-diag-logging.md).

### <a name="to-configure-multiple-azure-subscriptions"></a>Para configurar várias subscrições do Azure

Para oferecer suporte a várias subscrições, utilize o script do PowerShell da [registo de métricas de recurso de ativar o Azure com o PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/). Forneça o ID de recurso da área de trabalho como um parâmetro ao executar o script para enviar dados de diagnóstico a partir dos recursos numa subscrição do Azure para uma área de trabalho outra subscrição do Azure.

**Exemplo**

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-the-solution"></a>Utilizar a solução

Quando adiciona a solução para a área de trabalho, o mosaico de análise de SQL do Azure é adicionado à sua área de trabalho e é apresentado na descrição geral. O mosaico mostra o número de bases de dados SQL do Azure e conjuntos elásticos SQL do Azure e que a solução está ligada ao.

![Mosaico de análise de SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a>Ver os dados de análise de SQL do Azure

Clique nas **a análise de SQL do Azure** mosaico para abrir o dashboard de análise de SQL do Azure. O dashboard inclui a descrição geral de todas as bases de dados que são monitorizados através de diferentes perspectivas. Para diferentes perspectivas para funcionar, tem de ativar métricas adequadas ou registos nos seus recursos do SQL para ser transmitido à área de trabalho do Log Analytics do Azure.

![Descrição geral da análise SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-overview.png)

Selecionar qualquer um dos mosaicos, abre-se de um relatório de desagregação para o ponto de vista específico. Depois do ponto de vista é selecionado, é aberto o relatório de desagregação.

![Tempos limite de análise SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-metrics.png)

Cada uma delas fornece resumos na subscrição, o servidor, o conjunto elástico e o nível base de dados. Além disso, cada uma delas mostra um ponto de vista específicos para o relatório no lado direito. Selecionar a subscrição, servidor, agrupamento ou base de dados da lista continua a desagregação.

| Ponto de vista | Descrição |
| --- | --- |
| Recurso por tipo | Ponto de vista que conte de todos os recursos monitorizados. Desagregar fornece o resumo das métricas DTU e GB. |
| Informações | Fornece a desagregação hierárquica em informações inteligentes. Saiba mais sobre informações inteligentes. |
| Erros | Fornece a desagregação hierárquica em erros de SQL que ocorreram nas bases de dados. |
| Tempos limite | Fornece a desagregação hierárquica em tempos limite SQL que ocorreram nas bases de dados. |
| Bloqueios | Fornece a desagregação hierárquica no blockings SQL que ocorreram nas bases de dados. |
| Esperas de base de dados | Fornece a desagregação hierárquica para estatísticas de espera SQL no nível da base de dados. Inclui resumos de tempo de espera total e o tempo de espera por tipo de espera. |
| Duração de consulta | Fornece a desagregação hierárquica para as estatísticas de execução de consulta, como a duração de consulta, a utilização da CPU, utilização e/s de dados, utilização e/s de registo. |
| Esperas de consulta | Fornece a desagregação hierárquica para as estatísticas de espera de consulta por categoria de espera. |

### <a name="intelligent-insights-report"></a>Relatório de informações inteligente

Base de dados SQL do Azure [informações inteligentes](../sql-database/sql-database-intelligent-insights.md) permite-lhe determinar o que acontece com o desempenho da sua base de dados. Todas as informações inteligentes recolhidos podem ser visualizadas e acessadas por meio da perspectiva de informações.

![Informações de análise SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pool-and-database-reports"></a>Conjunto elástico e da base de dados de relatórios

Conjuntos elásticos e bases de dados tem seus próprios relatórios específicos que mostrar todos os dados que são recolhidos para o recurso no tempo especificado.

![Base de dados de análise SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-database.png)

![Conjunto elástico de análise SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Relatórios de consulta

Através da duração de consulta e perspectivas de esperas de consulta, pode correlacionar o desempenho de qualquer consulta através do relatório de consulta. Este relatório compara o desempenho das consultas entre bases de dados diferentes e torna mais fácil identificar bases de dados que executam a consulta selecionada bem em comparação com aqueles que forem lentas.

![Consultas de análise SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-queries.png)

### <a name="analyze-data-and-create-alerts"></a>Analisar dados e criar alertas

Pode facilmente [criar alertas](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) com os dados provenientes de recursos de base de dados do Azure SQL. Aqui estão algumas útil [pesquisa de registos](log-analytics-log-searches.md) consultas que pode utilizar com um alerta de registo:



*DTU elevada na base de dados SQL do Azure*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL" and ResourceId contains "/DATABASES/" and MetricName=="dtu_consumption_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

*DTU elevada num conjunto elástico de base de dados SQL do Azure*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL" and ResourceId contains "/ELASTICPOOLS/" and MetricName=="dtu_consumption_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```



## <a name="next-steps"></a>Passos Seguintes

- Uso [pesquisas de registos](log-analytics-log-searches.md) no Log Analytics para ver os dados de SQL do Azure detalhados.
- [Criar seus próprios dashboards](log-analytics-dashboards.md) a mostrar dados de SQL do Azure.
- [Criar alertas](log-analytics-alerts.md) quando ocorrem eventos específicos do SQL do Azure.
