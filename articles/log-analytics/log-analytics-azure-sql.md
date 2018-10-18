---
title: Solução de análise de SQL do Azure no Log Analytics | Documentos da Microsoft
description: Solução de análise de SQL do Azure ajuda-o a gerir as suas bases de dados SQL do Azure
services: log-analytics
documentationcenter: ''
author: danimir
manager: carmonm
ms.reviewer: carlrab
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/03/2018
ms.author: v-daljep
ms.component: ''
ms.openlocfilehash: bc82e030742bb5ea9621e972fcce2435cdda61ea
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49353463"
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview"></a>Monitorizar a base de dados do SQL do Azure através da análise de SQL do Azure (pré-visualização)

![Símbolo de análise de SQL do Azure](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Análise de SQL do Azure é uma solução de monitorização para monitorização do desempenho de bases de dados SQL do Azure, conjuntos elásticos e instâncias geridas em escala e em várias subscrições de nuvem. Ele recolhe e visualiza métricas de desempenho de base de dados do Azure SQL importantes com a inteligência incorporada para resolução de problemas de desempenho.

Ao utilizar as métricas que recolhe com a solução, pode criar regras personalizadas de monitorização e alertas. A solução ajuda-o a identificar problemas em cada camada de sua pilha de aplicativos. Ele usa métricas de diagnóstico do Azure, juntamente com os modos de exibição do Log Analytics para apresentar dados sobre todos os seus SQL do Azure bases de dados, conjuntos elásticos e bases de dados de instâncias geridas numa única área de trabalho do Log Analytics. O log Analytics ajuda-o a recolher, correlacionar e visualizar dados estruturados e não estruturados.

Para obter uma descrição geral prática sobre como utilizar a solução de análise de SQL do Azure e para cenários de uso típico, veja o vídeo incorporado:


> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-Intelligent-Insights-for-Improving-Azure-SQL-Database-Performance/player]
>

Embora a solução é gratuita, consumo de telemetria de diagnóstico acima as unidades gratuitas de ingestão de dados alocados a cada mês aplica-se, consulte [preços do Log Analytics](https://azure.microsoft.com/en-us/pricing/details/monitor). As unidades gratuitas de ingestão de dados fornecido ativar gratuita monitorização de vários bancos de dados por mês. Tenha em atenção que as bases de dados mais ativos com cargas de trabalho mais pesadas serão ingestão de dados mais versus bases de dados inativos. Pode monitorizar facilmente o seu consumo de ingestão de dados na solução, selecione a área de trabalho do OMS no menu de navegação de análise de SQL do Azure e, em seguida, selecionar a utilização e custos estimados.

## <a name="connected-sources"></a>Origens ligadas

Análise de SQL do Azure é uma cloud apenas monitorização solução suporte de transmissão em fluxo de telemetria de diagnóstico para a SQL Database do Azure, conjuntos elásticos e bases de dados de instância gerida.

Como a solução não utiliza agentes para ligar ao serviço Log Analytics, a solução não suportar a monitorização do SQL Server alojado no local ou em VMs, veja a tabela de compatibilidade abaixo.

| Origem Ligada | Suportadas | Descrição |
| --- | --- | --- |
| **[Diagnóstico do Azure](log-analytics-azure-storage.md)** | **Sim** | Dados de registo e métricas do Azure são enviados para o Log Analytics diretamente pelo Azure. |
| [Conta de armazenamento do Azure](log-analytics-azure-storage.md) | Não | O log Analytics não ler os dados de uma conta de armazenamento. |
| [Agentes do Windows](log-analytics-windows-agent.md) | Não | Agentes diretos do Windows não são usados pela solução. |
| [Agentes do Linux](log-analytics-linux-agents.md) | Não | Agentes diretos do Linux não são usados pela solução. |
| [Grupo de gestão do SCOM](log-analytics-om-agents.md) | Não | Uma ligação direta do agente do SCOM para o Log Analytics não é utilizada pela solução. |

## <a name="configuration"></a>Configuração

Execute os seguintes passos para adicionar a solução de análise de SQL do Azure ao dashboard do Azure.

1. Adicionar a solução de análise de SQL do Azure à sua área de trabalho de [do Azure marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview).
2. No portal do Azure, clique em **+ criar um recurso**, em seguida, procure **análise de SQL do Azure**.  
    ![Monitorização + Gestão](./media/log-analytics-azure-sql/monitoring-management.png)
3. Selecione **análise de SQL do Azure (pré-visualização)** da lista
4. Na **análise de SQL do Azure (pré-visualização)** área, clique em **criar**.  
    ![Criar](./media/log-analytics-azure-sql/portal-create.png)
5. Na **criar nova solução** área, crie um novo ou selecione uma área de trabalho existente que pretende adicionar a solução para e, em seguida, clique em **criar**.

    ![Adicionar área de trabalho](./media/log-analytics-azure-sql/add-to-workspace.png)

### <a name="configure-azure-sql-databases-elastic-pools-and-managed-instances-to-stream-diagnostics-telemetry"></a>Configurar bases de dados do Azure SQL, os conjuntos elásticos e instâncias geridas para a telemetria de diagnóstico do stream

Assim que tiver criado a solução de análise de SQL do Azure na sua área de trabalho, para monitorizar o desempenho de bases de dados SQL do Azure, bases de dados de instância gerida e conjuntos elásticos, precisará **configure cada** destes recursos que pretende Monitorize a respetiva telemetria de diagnóstico para a solução do stream.

- Ativar os diagnósticos do Azure para SQL Database do Azure, bases de dados de instância gerida e para os conjuntos elásticos [transmitir a telemetria de diagnóstico para análise de SQL do Azure](../sql-database/sql-database-metrics-diag-logging.md).

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

Quando adiciona a solução para a área de trabalho, o mosaico de análise de SQL do Azure é adicionado à sua área de trabalho e é apresentado na descrição geral. O mosaico mostra o número de bases de dados SQL do Azure, conjuntos elásticos, instâncias geridas e as bases de dados em instâncias de geridas que a solução está a receber a telemetria de diagnóstico de.

![Mosaico de análise de SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

A solução fornece duas vistas separadas – uma para monitorização de bases de dados do Azure SQL e conjuntos elásticos e a outra exibição para monitorizar a instância gerida e bases de dados em instâncias geridas.

Para ver o dashboard de monitorização de análise de SQL do Azure para conjuntos elásticos e bases de dados do Azure SQL, clique na parte superior do mosaico. Para ver o dashboard de monitorização para a instância gerida e bases de dados na instância gerida a análise de SQL do Azure, clique na parte inferior do mosaico.

### <a name="viewing-azure-sql-analytics-data"></a>Ver os dados de análise de SQL do Azure

O dashboard inclui a descrição geral de todas as bases de dados que são monitorizados através de diferentes perspectivas. Para diferentes perspectivas para funcionar, tem de ativar métricas adequadas ou registos nos seus recursos do SQL para ser transmitido à área de trabalho do Log Analytics do Azure.

Tenha em atenção que se algumas métricas ou registos não são transmitidos para o Azure Log Analytics, os mosaicos na solução não estarão preenchidos com informações de monitorização.

### <a name="azure-sql-database-and-elastic-pool-view"></a>Base de dados SQL do Azure e o modo de exibição do conjunto elástico

Depois da análise de SQL do Azure do mosaico da base de dados do Azure SQL e conjuntos elásticos é selecionado, é apresentado o dashboard de monitorização.

![Descrição geral da análise SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-overview.png)

Selecionar qualquer um dos mosaicos, abre-se de um relatório de desagregação para o ponto de vista específico. Depois do ponto de vista é selecionado, é aberto o relatório de desagregação.

![Tempos limite de análise SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-metrics.png)

Cada ponto de vista nesta vista fornece resumos na subscrição, o servidor, o conjunto elástico e o nível base de dados. Além disso, cada uma delas mostra um ponto de vista específicos para o relatório no lado direito. Selecionar a subscrição, servidor, agrupamento ou base de dados da lista continua a desagregação.

### <a name="managed-instance-and-databases-in-managed-instance-view"></a>A instância gerida e bases de dados na vista de instância gerida

Depois da análise de SQL do Azure do mosaico para instâncias geridas e bases de dados da instância gerida está selecionada, é apresentado o dashboard de monitorização.

![Descrição geral da análise SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-overview-mi.png)

Selecionar qualquer um dos mosaicos, abre-se de um relatório de desagregação para o ponto de vista específico. Depois do ponto de vista é selecionado, é aberto o relatório de desagregação.

Selecionar vista de instância gerida, mostra detalhes sobre a utilização de instância gerida, contém as bases de dados e telemetria sobre as consultas executadas em toda a instância.

![Tempos limite de análise SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-metrics-mi.png)

### <a name="perspectives"></a>Perspetivas

O abaixo tabela descreve as perspetivas suportadas para duas versões do dashboard, um para os conjuntos de base de dados e elástico de SQL do Azure e a outra para a instância gerida.

| Ponto de vista | Descrição | Suporte de agrupamentos de base de dados SQL e elástico | Suporte a instância gerenciados |
| --- | ------- | ----- | ----- |
| Recurso por tipo | Ponto de vista que conte de todos os recursos monitorizados. | Sim | Sim | 
| Informações | Fornece a desagregação hierárquica em informações inteligentes sobre o desempenho. | Sim | Sim |
| Erros | Fornece a desagregação hierárquica em erros de SQL que ocorreram nas bases de dados. | Sim | Sim |
| Tempos limite | Fornece a desagregação hierárquica em tempos limite SQL que ocorreram nas bases de dados. | Sim | Não |
| Bloqueios | Fornece a desagregação hierárquica no blockings SQL que ocorreram nas bases de dados. | Sim | Não |
| Esperas de base de dados | Fornece a desagregação hierárquica para estatísticas de espera SQL no nível da base de dados. Inclui resumos de tempo de espera total e o tempo de espera por tipo de espera. |Sim | Sim |
| Duração da consulta | Fornece a desagregação hierárquica para as estatísticas de execução de consulta, como a duração de consulta, a utilização da CPU, utilização e/s de dados, utilização e/s de registo. | Sim | Sim |
| Esperas de consulta | Fornece a desagregação hierárquica para as estatísticas de espera de consulta por categoria de espera. | Sim | Sim |

### <a name="intelligent-insights-report"></a>Relatório de informações inteligente

Base de dados SQL do Azure [informações inteligentes](../sql-database/sql-database-intelligent-insights.md) permite-lhe determinar o que está acontecendo com desempenho de bases de dados de bases de dados do Azure SQL e a instância gerida. Todas as informações inteligentes recolhidos podem ser visualizadas e acessadas por meio da perspectiva de informações.

![Informações de análise SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pool-and-database-reports"></a>Conjunto elástico e os relatórios de dados

Conjuntos elásticos e bases de dados SQL tem seus próprios relatórios específicos que mostram todos os dados que são recolhidos para o recurso no tempo especificado.

![Base de dados de análise SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-database.png)

![Conjunto elástico de SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Relatórios de consulta

Através da duração de consulta e perspectivas de esperas de consulta, pode correlacionar o desempenho de qualquer consulta através do relatório de consulta. Este relatório compara o desempenho das consultas entre bases de dados diferentes e torna mais fácil identificar bases de dados que executam a consulta selecionada bem em comparação com aqueles que forem lentas.

![Consultas de análise SQL do Azure](./media/log-analytics-azure-sql/azure-sql-sol-queries.png)

### <a name="analyze-data-and-create-alerts"></a>Analisar dados e criar alertas

### <a name="creating-alerts-for-azure-sql-database"></a>Criar alertas para a base de dados do Azure SQL

Pode facilmente [criar alertas](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) com os dados provenientes de recursos de base de dados do Azure SQL. Aqui estão algumas útil [pesquisa de registos](log-analytics-log-searches.md) consultas que pode utilizar com um alerta de registo:

*Elevada da CPU na base de dados SQL do Azure*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL"
| where ResourceId contains "/DATABASES/"
| where MetricName=="cpu_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

> [!NOTE]
> - Pré-requisito de configurar este alerta é esse métricas de diagnóstico de fluxo (opção de "Todas as métricas") de bases de dados monitorizadas à solução.
> - Substitua o cpu_percent de valor MetricName dtu_consumption_percent para obter resultados DTU elevados em vez disso.

*Elevada da CPU em conjuntos elásticos da base de dados do Azure SQL*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL"
| where ResourceId contains "/ELASTICPOOLS/"
| where MetricName=="cpu_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

> [!NOTE]
> - Pré-requisito de configurar este alerta é esse métricas de diagnóstico de fluxo (opção de "Todas as métricas") de bases de dados monitorizadas à solução.
> - Substitua o cpu_percent de valor MetricName dtu_consumption_percent para obter resultados DTU elevados em vez disso.

*Armazenamento de base de dados SQL do Azure em média superior a 95% na última 1 hora*

```
let time_range = 1h;
let storage_threshold = 95;
AzureMetrics
| where ResourceId contains "/DATABASES/"
| where MetricName == "storage_percent"
| summarize max_storage = max(Average) by ResourceId, bin(TimeGenerated, time_range)
| where max_storage > storage_threshold
| distinct ResourceId
```

> [!NOTE]
> - Pré-requisito de configurar este alerta é esse métricas de diagnóstico de fluxo (opção de "Todas as métricas") de bases de dados monitorizadas à solução.
> - Esta consulta requer uma regra de alerta para ser configurado para acionar um alerta quando existem resultados (> 0 resultados) da consulta, que indica se a condição existe no algumas bases de dados. O resultado é uma lista de recursos de base de dados que estão acima storage_threshold dentro time_range definido.
> - O resultado é uma lista de recursos de base de dados que estão acima storage_threshold dentro time_range definido.

*Alerta sobre informações inteligentes*

```
let alert_run_interval = 1h;
let insights_string = "hitting its CPU limits";
AzureDiagnostics
| where Category == "SQLInsights" and status_s == "Active" 
| where TimeGenerated > ago(alert_run_interval)
| where rootCauseAnalysis_s contains insights_string
| distinct ResourceId
```

> [!NOTE]
> - Pré-requisito de configurar este alerta é o log de diagnóstico que bases de dados monitorizadas stream SQLInsights à solução.
> - Esta consulta requer uma regra de alerta para ser configurado para ser executado com a mesma frequência de alert_run_interval para evitar resultados duplicados. A regra deve ser configurada para disparar o alerta quando existem resultados (> 0 resultados) da consulta.
> - Personalize o alert_run_interval para especificar o intervalo de tempo para verificar se a condição ocorreu em bases de dados configuradas para o registo de SQLInsights de fluxo para a solução.
> - Personalize o insights_string para capturar a saída do texto de análise de causa de raiz de informações. Este é o mesmo texto apresentado na IU da solução que pode utilizar o insights existente. Em alternativa, pode utilizar a consulta abaixo para ver o texto de todas as informações geradas na sua subscrição. Utilize a saída da consulta para coletar as cadeias de caracteres distintas para configurar alertas nas informações.

```
AzureDiagnostics
| where Category == "SQLInsights" and status_s == "Active" 
| distinct rootCauseAnalysis_s
```

### <a name="creating-alerts-for-managed-instance"></a>Criar alertas para a instância gerida

* Armazenamento de instância gerido é superior a 90%

```
let storage_percentage_treshold = 90;
AzureDiagnostics
| where Category =="ResourceUsageStats"
| summarize (TimeGenerated, calculated_storage_percentage) = arg_max(TimeGenerated, todouble(storage_space_used_mb_s) *100 / todouble (reserved_storage_mb_s))
   by ResourceId
| where calculated_storage_percentage > storage_percentage_treshold
```

> [!NOTE]
> - Pré-requisito de configurar este alerta é que a instância gerida monitorizado tem a transmissão em fluxo do registo de ResourceUsageStats ativado para a solução de.
> - Esta consulta requer uma regra de alerta para ser configurado para acionar um alerta quando existem resultados (> 0 resultados) da consulta, que indica que a condição existe na instância gerida. O resultado é o consumo de percentagem de armazenamento na instância gerida.

## <a name="next-steps"></a>Passos Seguintes

- Uso [pesquisas de registos](log-analytics-log-searches.md) no Log Analytics para ver os dados de SQL do Azure detalhados.
- [Criar seus próprios dashboards](log-analytics-dashboards.md) a mostrar dados de SQL do Azure.
- [Criar alertas](log-analytics-alerts.md) quando ocorrem eventos específicos do SQL do Azure.
