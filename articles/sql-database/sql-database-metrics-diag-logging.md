---
title: Registo de diagnóstico e de métricas de base de dados SQL do Azure | Documentos da Microsoft
description: Saiba mais sobre como configurar a base de dados do Azure SQL para armazenar a utilização de recursos, conectividade e estatísticas de execução de consulta.
services: sql-database
documentationcenter: ''
author: Danimir
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: v-daljep
ms.reviewer: carlrab
ms.openlocfilehash: fbeda6a74be11668f16d477696ea00653b73baa6
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284831"
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Métricas de base de dados SQL do Azure e o registo de diagnósticos 
Base de dados SQL do Azure pode emitir métricas e diagnósticos registos para uma monitorização mais fácil. Pode configurar a Base de Dados SQL para armazenar a utilização de recursos, funções de trabalho e sessões e a conectividade a um dos recursos do Azure seguintes:

* **O armazenamento do Azure**: utilizado para arquivar grandes quantidades de telemetria a um preço baixo.
* **Os Hubs de eventos do Azure**: utilizado para integrar a telemetria de base de dados SQL na sua solução personalizada monitorizção ou em pipelines.
* **O Azure Log Analytics**: utilizado para uma solução de monitorização de out-of-the-box com relatórios, alertas e mitigação. Esta é uma funcionalidade do [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md)

    ![Arquitetura](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="enable-logging"></a>Ativar registo

Métricas e diagnósticos de registo não está ativada por predefinição. Pode ativar e gerir métricas e diagnósticos de registo utilizando um dos seguintes métodos:

- Portal do Azure
- PowerShell
- CLI do Azure
- API de REST do Azure Monitor 
- Modelo Azure Resource Manager

Quando ativar as métricas e registo de diagnósticos, tem de especificar o recurso do Azure em que são recolhidos os dados selecionados. As opções disponíveis incluem:

- Log Analytics
- Hubs de Eventos
- Armazenamento 

Pode aprovisionar um novo recurso do Azure ou selecione um recurso existente. Depois de selecionar o recurso de armazenamento, tem de especificar os dados a recolher. As opções disponíveis incluem:

- [Todas as métricas](sql-database-metrics-diag-logging.md#all-metrics): percentagem de DTU contém, limite DTU, percentagem de CPU, percentagem de leitura de dados físicos, de registo de escrita em percentagem, com êxito/com falhas/bloqueado por ligações de firewall, percentagem de sessões, percentagem de funções de trabalho, armazenamento, percentagem de armazenamento e a percentagem de armazenamento do XTP.
- [QueryStoreRuntimeStatistics](sql-database-metrics-diag-logging.md#query-store-runtime-statistics): contém informações sobre as estatísticas de tempo de execução de consulta, por exemplo, a duração de consulta e de utilização da CPU.
- [QueryStoreWaitStatistics](sql-database-metrics-diag-logging.md#query-store-wait-statistics): contém informações sobre as estatísticas de espera de consulta, que indica o que as suas consultas aguardado, tais como CPU, o registo e o bloqueio.
- [Erros](sql-database-metrics-diag-logging.md#errors-dataset): contém informações sobre erros do SQL que tenham acontecido nesta base de dados.
- [DatabaseWaitStatistics](sql-database-metrics-diag-logging.md#database-wait-statistics-dataset): contém informações sobre quanto um banco de dados de tempo gasto aguardando tipos diferentes de espera.
- [Tempos limite](sql-database-metrics-diag-logging.md#time-outs-dataset): contém informações sobre tempos limite que ocorreu numa base de dados.
- [Blocos](sql-database-metrics-diag-logging.md#blockings-dataset): contém informações sobre o bloqueio de eventos que ocorreram numa base de dados.
- [SQLInsights](sql-database-metrics-diag-logging.md#intelligent-insights-dataset): contém informações inteligentes. [Saiba mais sobre informações inteligentes](sql-database-intelligent-insights.md).
- **Auditoria** / **SQLSecurityAuditEvents**: não está disponível.

Se selecionar os Hubs de eventos ou uma conta de armazenamento, pode especificar uma política de retenção. Esta política elimina os dados que é mais antigos que um período de tempo selecionado. Se especificar o Log Analytics, a política de retenção depende do escalão de preço selecionado. Para obter mais informações, consulte [preços do Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/). 

Para saber como ativar o registo e compreender as categorias de métricas e registos que são suportadas por vários serviços do Azure, recomendamos que leia: 

* [Descrição geral das métricas no Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
* [Descrição geral dos registos de diagnóstico do Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) 

### <a name="azure-portal"></a>Portal do Azure

1. Para ativar a recolha de registos de métricas e diagnósticos no portal, aceda à sua base de dados SQL ou a página do conjunto elástico e, em seguida, selecione **as definições de diagnóstico**.

   ![Ativar no portal do Azure](./media/sql-database-metrics-diag-logging/enable-portal.png)

2. Criar um novo ou editar as definições de diagnóstico existentes ao selecionar o destino e a telemetria.

   ![Definições de diagnóstico](./media/sql-database-metrics-diag-logging/diagnostics-portal.png)

### <a name="powershell"></a>PowerShell

Para ativar as métricas e diagnósticos de registro com o PowerShell, utilize os seguintes comandos:

- Para ativar o armazenamento de registos de diagnóstico numa conta de armazenamento, utilize este comando:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   O ID de conta de armazenamento é o ID de recurso para a conta de armazenamento onde pretende enviar os registos.

- Para ativar a transmissão em fluxo de registos de diagnóstico para um hub de eventos, use este comando:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   O ID de regra de Azure Service Bus é uma cadeia de caracteres com este formato:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- Para ativar o envio de registos de diagnóstico para uma área de trabalho do Log Analytics, use este comando:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- Pode obter o ID de recurso da sua área de trabalho do Log Analytics, utilizando o seguinte comando:

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

Pode combinar estes parâmetros para ativar várias opções de saída.

### <a name="to-configure-multiple-azure-resources"></a>Para configurar vários recursos do Azure

Para oferecer suporte a várias subscrições, utilize o script do PowerShell da [registo de métricas de recurso de ativar o Azure com o PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/).

Forneça o ID de recurso da área de trabalho &lt;$WSID&gt; como um parâmetro ao executar o script (Enable-AzureRMDiagnostics.ps1) para enviar dados de diagnóstico de vários recursos para a área de trabalho. Para obter o ID de área de trabalho &lt;$WSID&gt; para que gostaria de enviar dados de diagnóstico, substitua &lt;subID&gt; com o ID de subscrição, substituir &lt;RG_NAME&gt; com o nome do grupo de recursos, e substitua &lt;WS_NAME&gt; com o nome de área de trabalho no seguinte script.

- Para configurar vários recursos do Azure, utilize os seguintes comandos:

    ```powershell
    PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/<RG_NAME>/providers/microsoft.operationalinsights/workspaces/<WS_NAME>"
    PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
    ```

### <a name="azure-cli"></a>CLI do Azure

Para ativar as métricas e diagnósticos de registro com a CLI do Azure, utilize os seguintes comandos:

- Para ativar o armazenamento de registos de diagnóstico numa conta de armazenamento, utilize este comando:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   O ID de conta de armazenamento é o ID de recurso para a conta de armazenamento onde pretende enviar os registos.

- Para ativar a transmissão em fluxo de registos de diagnóstico para um hub de eventos, use este comando:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   O ID da regra do Service Bus é uma cadeia de caracteres com este formato:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Para ativar o envio de registos de diagnóstico para uma área de trabalho do Log Analytics, use este comando:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

Pode combinar estes parâmetros para ativar várias opções de saída.

### <a name="rest-api"></a>API REST

Saiba mais sobre como [alterar as definições de diagnóstico, utilizando a API de REST do Azure Monitor](https://docs.microsoft.com/en-us/rest/api/monitor/diagnosticsettings). 

### <a name="resource-manager-template"></a>Modelo do Resource Manager

Saiba mais sobre como [ative as definições de diagnóstico durante a criação de recursos com um modelo do Resource Manager](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md). 

## <a name="stream-into-log-analytics"></a>Stream para o Log Analytics 
Registos de diagnóstico e métricas de base de dados SQL podem ser transmitidos para o Log Analytics, utilizando o incorporado **enviar para o Log Analytics** opção no portal. Também pode ativar o Log Analytics utilizando uma definição de diagnóstico através de cmdlets do PowerShell, a CLI do Azure ou a API de REST do Azure Monitor.

### <a name="installation-overview"></a>Visão geral da instalação

A monitorização de uma frota de base de dados SQL é simples com o Log Analytics. São necessários três passos:

1. Crie um recurso do Log Analytics.

2. Configure bases de dados aos registos de diagnóstico e métricas de registo para o recurso do Log Analytics que criou.

3. Instalar o **a análise de SQL do Azure** solução a partir da galeria no Log Analytics.

### <a name="create-a-log-analytics-resource"></a>Criar um recurso do Log Analytics

1. Selecione **criar um recurso** no menu à esquerda.

2. Selecione **monitorização + gestão**.

3. Selecione **Log Analytics**.

4. Preencha o formulário do Log Analytics com as informações adicionais, que é necessários: nome de área de trabalho, subscrição, grupo de recursos, localização e escalão de preço.

   ![Log Analytics](./media/sql-database-metrics-diag-logging/log-analytics.png)

### <a name="configure-databases-to-record-metrics-and-diagnostics-logs"></a>Configurar bases de dados aos registos de diagnóstico e métricas de registo

A forma mais fácil de configurar suas métricas de registos de bases de dados de onde é através do portal do Azure. No portal, aceda ao seu recurso de base de dados SQL e selecione **as definições de diagnóstico**. 

### <a name="install-the-sql-analytics-solution-from-the-gallery"></a>Instalar a solução de análise de SQL a partir da Galeria

1. Depois de criar o recurso do Log Analytics e seus dados estão a ser encaminhados para ele, instale a solução de análise de SQL. Na home page, no menu lateral, selecione **Galeria de soluções**. Na galeria, selecione o **a análise de SQL do Azure** solução e selecione **Add**.

   ![Solução de monitorização](./media/sql-database-metrics-diag-logging/monitoring-solution.png)

2. Na sua home page, o **a análise de SQL do Azure** mosaico é apresentado. Selecione este mosaico para abrir o dashboard de análise de SQL.

### <a name="use-the-sql-analytics-solution"></a>Utilizar a solução de análise de SQL

Análise de SQL é um dashboard hierárquico que lhe permite mover através da hierarquia de recursos de base de dados SQL. Para saber como utilizar a solução de análise de SQL, veja [monitorizar a base de dados de SQL com a solução de análise de SQL](../log-analytics/log-analytics-azure-sql.md).

## <a name="stream-into-event-hubs"></a>Transmitir em fluxo para os Hubs de Eventos

Registos de diagnóstico e métricas de base de dados SQL podem serão transmitidos para os Hubs de eventos utilizando o incorporado **Stream para um hub de eventos** opção no portal. Também pode ativar o ID da regra do Service Bus utilizando uma definição de diagnóstico através de cmdlets do PowerShell, a CLI do Azure ou a API de REST do Azure Monitor. 

### <a name="what-to-do-with-metrics-and-diagnostics-logs-in-event-hubs"></a>O que fazer com métricas e diagnósticos registos nos Hubs de eventos
Depois dos dados selecionados são transmitidos em Hubs de eventos, está um passo mais perto para ativar cenários de monitorização avançada. Os Event Hubs atuam como a porta de entrada para um pipeline de eventos. Depois de dados são recolhidos para um hub de eventos, podem ser transformado e armazenado através de qualquer fornecedor de análise em tempo real ou adaptadores de criação de batches/armazenamento. Os Hubs de eventos desassocia a produção de um fluxo de eventos do consumo desses eventos. Dessa forma, os consumidores de eventos podem acessar os eventos no seu próprio agendamento. Para obter mais informações sobre os Hubs de eventos, consulte:

- [Quais são os Hubs de eventos do Azure?](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Introdução ao Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)


Aqui estão algumas formas que pode utilizar a capacidade de transmissão em fluxo:

* **Ver o estado de funcionamento do serviço através da transmissão em fluxo de dados de caminhos recorrentes para o Power BI**. Ao utilizar os Hubs de eventos, o Stream Analytics e o Power BI, pode facilmente transformar os dados de métricas e diagnósticos para perto de informações em tempo real nos seus serviços do Azure. Para uma descrição geral de como configurar um hub de eventos, o processamento de dados com o Stream Analytics e utilize o Power BI como resultado, consulte [Stream Analytics e o Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md).

* **Stream registos para fluxos de telemetria e de registo de terceiros**. Ao utilizar os Hubs de eventos de transmissão em fluxo, pode obter os registos de métricas e diagnósticos em diferentes soluções de análise de monitorização e registo de terceiros. 

* **Criar uma plataforma de Registro em log e telemetria personalizada**. Se já tiver uma plataforma de telemetria personalizados internos ou estão pensando em criar uma, altamente dimensionável de publicação-subscrição natureza dos Hubs de eventos permite ingerir forma flexível os registos de diagnóstico. Ver [guia de Dan Rosanova para utilizar os Hubs de eventos numa plataforma de telemetria de escala global](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="stream-into-storage"></a>Stream para o armazenamento

Registos de diagnóstico e métricas de base de dados SQL podem ser armazenados no armazenamento, utilizando o incorporado **arquivo para uma conta de armazenamento** opção no portal. Também pode ativar o armazenamento utilizando uma definição de diagnóstico através de cmdlets do PowerShell, a CLI do Azure ou a API de REST do Azure Monitor.

### <a name="schema-of-metrics-and-diagnostics-logs-in-the-storage-account"></a>Esquema de métricas e diagnósticos registos na conta de armazenamento

Depois de configurar a coleção de registos de diagnóstico e métricas, é criado um contentor de armazenamento na conta de armazenamento que selecionou quando as primeiras linhas de dados estão disponíveis. A estrutura destes blobs é:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ databases/{database_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```
    
Ou, mais simples:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Por exemplo, pode ser um nome de blob para todas as métricas:

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Se desejar gravar os dados do conjunto elástico, o nome do blob é um pouco diferente:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

### <a name="download-metrics-and-logs-from-storage"></a>Transferir os registos e métricas do armazenamento

Saiba como [transferir registos de diagnóstico e métricas do armazenamento](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-the-sample-application).

## <a name="metrics-and-logs-available"></a>Métricas e registos disponíveis

### <a name="all-metrics"></a>Todas as métricas

|**Recurso**|**Métricas**|
|---|---|
|Base de Dados|Percentagem de DTU, DTU utilizado, o limite DTU, percentagem de CPU, percentagem de leitura de dados físicos, de registo de escrita em percentagem, com êxito/com falhas/bloqueado por ligações de firewall, percentagem de sessões, percentagem de funções de trabalho, armazenamento, percentagem de armazenamento, percentagem de armazenamento do XTP, e deadlocks |
|Conjunto elástico|percentagem de eDTU, eDTU utilizado, o limite de eDTU, percentagem de CPU, percentagem de leitura de dados físicos, log escrever percentagem, percentagem de sessões, percentagem de funções de trabalho, armazenamento, percentagem de armazenamento, limite de armazenamento, percentagem de armazenamento do XTP |
|||

### <a name="query-store-runtime-statistics"></a>Estatísticas de tempo de execução de consulta Store

|Propriedade|Descrição|
|---|---|
|TenantId|O ID de inquilino.|
|SourceSystem|Sempre: Azure|
|TimeGenerated [UTC]|Carimbo de hora quando o registo foi registado.|
|Tipo|Sempre: AzureDiagnostics|
|ResourceProvider|Nome do fornecedor de recursos. Always: MICROSOFT.SQL|
|Categoria|Nome da categoria. Sempre: QueryStoreRuntimeStatistics|
|OperationName|Nome da operação. Sempre: QueryStoreRuntimeStatisticsEvent|
|Recurso|Nome do recurso.|
|ResourceType|Nome do tipo de recurso. Sempre: Servidores/bases de dados|
|SubscriptionId|GUID que pertence a base de dados de subscrição.|
|ResourceGroup|Nome do grupo de recursos que a base de dados pertence.|
|LogicalServerName_s|Nome do servidor que a base de dados pertence.|
|ElasticPoolName_s|Nome do conjunto elástico que pertence a base de dados, se aplicável.|
|DatabaseName_s|Nome da base de dados.|
|ResourceId|URI do recurso.|
|query_hash_s|Hash de consulta.|
|query_plan_hash_s|Hash de plano de consulta.|
|statement_sql_handle_s|Identificador de instrução sql.|
|interval_start_time_d|Inicie datetimeoffset do intervalo de número de tiques de 1900-1-1.|
|interval_end_time_d|Datetimeoffset de fim do intervalo no número de tiques de 1900-1-1.|
|logical_io_writes_d|Número total de escritas de e/s lógicas.|
|max_logical_io_writes_d|Número máx. de e/s lógica escreve por execução.|
|physical_io_reads_d|Número total de leituras de e/s físicas.|
|max_physical_io_reads_d|Número máx. de e/s lógica lê por execução.|
|logical_io_reads_d|Número total de leituras lógicas de e/s.|
|max_logical_io_reads_d|Número máx. de e/s lógica lê por execução.|
|execution_type_d|Tipo de execução.|
|count_executions_d|Número de execuções da consulta.|
|cpu_time_d|Tempo total de CPU consumido pela consulta em microssegundos.|
|max_cpu_time_d|Consumidor de tempo de CPU máxima por uma única execução em microssegundos.|
|dop_d|Soma dos graus de paralelismo.|
|max_dop_d|Grau máximo de paralelismo utilizado para execução única.|
|rowcount_d|Número total de linhas devolvidas.|
|max_rowcount_d|Número máximo de linhas retornadas na execução única.|
|query_max_used_memory_d|Quantidade total de memória utilizada em KB.|
|max_query_max_used_memory_d|Quantidade de máx. de memória utilizada por uma única execução em KB.|
|duration_d|Tempo de execução total em microssegundos.|
|max_duration_d|Tempo de execução máxima de uma única execução.|
|num_physical_io_reads_d|Número total de leituras físicas.|
|max_num_physical_io_reads_d|Número máximo de leituras físicos por execução.|
|log_bytes_used_d|Quantidade total de bytes de registo utilizados.|
|max_log_bytes_used_d|Quantidade de máximo de bytes de registo utilizados por execução.|
|query_id_d|ID da consulta no Query Store.|
|plan_id_d|ID do plano de consulta Store.|

Saiba mais sobre [dados de estatísticas de tempo de execução de consulta Store](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql).

### <a name="query-store-wait-statistics"></a>Estatísticas de espera de Store de consulta

|Propriedade|Descrição|
|---|---|
|TenantId|O ID de inquilino.|
|SourceSystem|Sempre: Azure|
|TimeGenerated [UTC]|Carimbo de hora quando o registo foi registado.|
|Tipo|Sempre: AzureDiagnostics|
|ResourceProvider|Nome do fornecedor de recursos. Always: MICROSOFT.SQL|
|Categoria|Nome da categoria. Sempre: QueryStoreWaitStatistics|
|OperationName|Nome da operação. Sempre: QueryStoreWaitStatisticsEvent|
|Recurso|Nome do recurso|
|ResourceType|Nome do tipo de recurso. Sempre: Servidores/bases de dados|
|SubscriptionId|GUID que pertence a base de dados de subscrição.|
|ResourceGroup|Nome do grupo de recursos que a base de dados pertence.|
|LogicalServerName_s|Nome do servidor que a base de dados pertence.|
|ElasticPoolName_s|Nome do conjunto elástico que pertence a base de dados, se aplicável.|
|DatabaseName_s|Nome da base de dados.|
|ResourceId|URI do recurso.|
|wait_category_s|Categoria da espera.|
|is_parameterizable_s|A consulta é parametrizável.|
|statement_type_s|Tipo de instrução.|
|statement_key_hash_s|Hash de chave de instrução.|
|exec_type_d|Tipo de execução.|
|total_query_wait_time_ms_d|Tempo de espera total da consulta na categoria específica de espera.|
|max_query_wait_time_ms_d|Tempo de espera máximo da consulta em execução individual na categoria específica de espera.|
|query_param_type_d|0|
|query_hash_s|Consultar o hash no Query Store.|
|query_plan_hash_s|Hash de plano de consulta no Query Store.|
|statement_sql_handle_s|Identificador de instrução de consulta Store.|
|interval_start_time_d|Inicie datetimeoffset do intervalo de número de tiques de 1900-1-1.|
|interval_end_time_d|Datetimeoffset de fim do intervalo no número de tiques de 1900-1-1.|
|count_executions_d|Contagem de execuções da consulta.|
|query_id_d|ID da consulta no Query Store.|
|plan_id_d|ID do plano de consulta Store.|

Saiba mais sobre [dados de estatísticas de espera de consulta Store](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql).

### <a name="errors-dataset"></a>Conjunto de dados de erros

|Propriedade|Descrição|
|---|---|
|TenantId|O ID de inquilino.|
|SourceSystem|Sempre: Azure|
|TimeGenerated [UTC]|Carimbo de hora quando o registo foi registado.|
|Tipo|Sempre: AzureDiagnostics|
|ResourceProvider|Nome do fornecedor de recursos. Always: MICROSOFT.SQL|
|Categoria|Nome da categoria. Sempre: erros|
|OperationName|Nome da operação. Sempre: ErrorEvent|
|Recurso|Nome do recurso|
|ResourceType|Nome do tipo de recurso. Sempre: Servidores/bases de dados|
|SubscriptionId|GUID que pertence a base de dados de subscrição.|
|ResourceGroup|Nome do grupo de recursos que a base de dados pertence.|
|LogicalServerName_s|Nome do servidor que a base de dados pertence.|
|ElasticPoolName_s|Nome do conjunto elástico que pertence a base de dados, se aplicável.|
|DatabaseName_s|Nome da base de dados.|
|ResourceId|URI do recurso.|
|Mensagem|Mensagem de erro em texto simples.|
|user_defined_b|É o bit de definidas pelo utilizador do erro.|
|error_number_d|Código de erro.|
|Gravidade|Gravidade do erro.|
|state_d|Estado do erro.|
|query_hash_s|Hash de consulta da consulta com falhas, se disponível.|
|query_plan_hash_s|Hash de plano de consulta da consulta com falhas, se disponível.|

Saiba mais sobre [mensagens de erro do SQL Server](https://msdn.microsoft.com/library/cc645603.aspx).

### <a name="database-wait-statistics-dataset"></a>Conjunto de dados de estatísticas de espera de base de dados

|Propriedade|Descrição|
|---|---|
|TenantId|O ID de inquilino.|
|SourceSystem|Sempre: Azure|
|TimeGenerated [UTC]|Carimbo de hora quando o registo foi registado.|
|Tipo|Sempre: AzureDiagnostics|
|ResourceProvider|Nome do fornecedor de recursos. Always: MICROSOFT.SQL|
|Categoria|Nome da categoria. Sempre: DatabaseWaitStatistics|
|OperationName|Nome da operação. Sempre: DatabaseWaitStatisticsEvent|
|Recurso|Nome do recurso|
|ResourceType|Nome do tipo de recurso. Sempre: Servidores/bases de dados|
|SubscriptionId|GUID que pertence a base de dados de subscrição.|
|ResourceGroup|Nome do grupo de recursos que a base de dados pertence.|
|LogicalServerName_s|Nome do servidor que a base de dados pertence.|
|ElasticPoolName_s|Nome do conjunto elástico que pertence a base de dados, se aplicável.|
|DatabaseName_s|Nome da base de dados.|
|ResourceId|URI do recurso.|
|wait_type_s|Nome do tipo de espera.|
|start_utc_date_t [UTC]|Hora de início do período de medida.|
|end_utc_date_t [UTC]|Hora de fim do período de medida.|
|delta_max_wait_time_ms_d|Tempo por execução de espera máximo|
|delta_signal_wait_time_ms_d|Tempo de espera de sinal total.|
|delta_wait_time_ms_d|Tempo de espera total no período.|
|delta_waiting_tasks_count_d|Número de tarefas de espera.|

Saiba mais sobre [estatísticas de espera de base de dados](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql).

### <a name="time-outs-dataset"></a>Conjunto de dados de tempos limite

|Propriedade|Descrição|
|---|---|
|TenantId|O ID de inquilino.|
|SourceSystem|Sempre: Azure|
|TimeGenerated [UTC]|Carimbo de hora quando o registo foi registado.|
|Tipo|Sempre: AzureDiagnostics|
|ResourceProvider|Nome do fornecedor de recursos. Always: MICROSOFT.SQL|
|Categoria|Nome da categoria. Sempre: tempos limite|
|OperationName|Nome da operação. Sempre: TimeoutEvent|
|Recurso|Nome do recurso|
|ResourceType|Nome do tipo de recurso. Sempre: Servidores/bases de dados|
|SubscriptionId|GUID que pertence a base de dados de subscrição.|
|ResourceGroup|Nome do grupo de recursos que a base de dados pertence.|
|LogicalServerName_s|Nome do servidor que a base de dados pertence.|
|ElasticPoolName_s|Nome do conjunto elástico que pertence a base de dados, se aplicável.|
|DatabaseName_s|Nome da base de dados.|
|ResourceId|URI do recurso.|
|error_state_d|Código de estado de erro.|
|query_hash_s|Consultar o hash, se disponível.|
|query_plan_hash_s|Consultar o hash do plano, se disponível.|

### <a name="blockings-dataset"></a>Blockings conjunto de dados

|Propriedade|Descrição|
|---|---|
|TenantId|O ID de inquilino.|
|SourceSystem|Sempre: Azure|
|TimeGenerated [UTC]|Carimbo de hora quando o registo foi registado.|
|Tipo|Sempre: AzureDiagnostics|
|ResourceProvider|Nome do fornecedor de recursos. Always: MICROSOFT.SQL|
|Categoria|Nome da categoria. Sempre: blocos|
|OperationName|Nome da operação. Sempre: BlockEvent|
|Recurso|Nome do recurso|
|ResourceType|Nome do tipo de recurso. Sempre: Servidores/bases de dados|
|SubscriptionId|GUID que pertence a base de dados de subscrição.|
|ResourceGroup|Nome do grupo de recursos que a base de dados pertence.|
|LogicalServerName_s|Nome do servidor que a base de dados pertence.|
|ElasticPoolName_s|Nome do conjunto elástico que pertence a base de dados, se aplicável.|
|DatabaseName_s|Nome da base de dados.|
|ResourceId|URI do recurso.|
|lock_mode_s|Modo de bloqueio usado pela consulta.|
|resource_owner_type_s|Proprietário do bloqueio.|
|blocked_process_filtered_s|Bloqueado o processo de relatório XML.|
|duration_d|Duração do bloqueio em microssegundos.|

### <a name="deadlocks-dataset"></a>Conjunto de dados de deadlocks

|Propriedade|Descrição|
|---|---|
|TenantId|O ID de inquilino.|
|SourceSystem|Sempre: Azure|
|TimeGenerated [UTC] |Carimbo de hora quando o registo foi registado.|
|Tipo|Sempre: AzureDiagnostics|
|ResourceProvider|Nome do fornecedor de recursos. Always: MICROSOFT.SQL|
|Categoria|Nome da categoria. Sempre: Deadlocks|
|OperationName|Nome da operação. Sempre: DeadlockEvent|
|Recurso|Nome do recurso.|
|ResourceType|Nome do tipo de recurso. Sempre: Servidores/bases de dados|
|SubscriptionId|GUID que pertence a base de dados de subscrição.|
|ResourceGroup|Nome do grupo de recursos que a base de dados pertence.|
|LogicalServerName_s|Nome do servidor que a base de dados pertence.|
|ElasticPoolName_s|Nome do conjunto elástico que pertence a base de dados, se aplicável.|
|DatabaseName_s|Nome da base de dados. |
|ResourceId|URI do recurso.|
|deadlock_xml_s|Relatório de deadlock XML.|

### <a name="intelligent-insights-dataset"></a>Conjunto de dados de informações inteligente
Saiba mais sobre o [formato de registo de informações inteligentes](sql-database-intelligent-insights-use-diagnostics-log.md).

## <a name="next-steps"></a>Passos Seguintes

Para saber como ativar o registo e compreender as categorias de métricas e registo suportadas por vários serviços do Azure, leia:

 * [Descrição geral das métricas no Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
 * [Descrição geral dos registos de diagnóstico do Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)

Para saber mais sobre os Hubs de eventos, leia:

* [O que é o Event Hubs do Azure?](../event-hubs/event-hubs-what-is-event-hubs.md)
* [Introdução ao Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

Para saber mais sobre o armazenamento, veja como [transferir registos de diagnóstico e métricas do armazenamento](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-the-sample-application).
