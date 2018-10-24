---
title: Registo de diagnóstico e de métricas de base de dados SQL do Azure | Documentos da Microsoft
description: Saiba mais sobre como configurar a base de dados do Azure SQL para armazenar a utilização de recursos, conectividade e estatísticas de execução de consulta.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: v-daljep
ms.reviewer: carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: 775883d575a87758f563bd8dae8e5a726cd8ed36
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49959082"
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Métricas de base de dados SQL do Azure e o registo de diagnósticos 

Base de dados SQL do Azure, conjuntos elásticos, instância gerida e bases de dados em podem de instância gerida emitir os registos de diagnóstico e métricas para monitorizar o desempenho mais fácil. Pode configurar uma base de dados para utilização de recursos de fluxo, os operadores de sessões e conectividade a um dos recursos do Azure seguintes:

* **Análise de SQL do Azure**: utilizada como a solução com relatórios, alertas e mitigação de monitorização do desempenho inteligente integrada da base de dados do Azure.
* **Os Hubs de eventos do Azure**: utilizado para integrar a telemetria de base de dados SQL na sua solução personalizada monitorizção ou em pipelines.
* **O armazenamento do Azure**: utilizado para arquivar grandes quantidades de telemetria a uma fração do preço.

    ![Arquitetura](./media/sql-database-metrics-diag-logging/architecture.png)

Compreender as métricas e registo categorias que são suportadas por vários serviços do Azure, considere a leitura:

* [Descrição geral das métricas no Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
* [Descrição geral dos registos de diagnóstico do Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) 

 Pode ativar e gerir a telemetria de métricas e diagnósticos fazendo logon num banco de dados utilizando um dos seguintes métodos:

- Portal do Azure
- PowerShell
- CLI do Azure
- API de REST do Azure Monitor 
- Modelo Azure Resource Manager

Quando ativar as métricas e registo de diagnósticos, tem de especificar o destino de recursos do Azure onde serão recolhidos os dados selecionados. As opções disponíveis incluem:

- Análise de SQL
- Hubs de Eventos
- Armazenamento 

Pode aprovisionar um novo recurso do Azure ou selecione um recurso existente. Depois de selecionar um recurso, usando a opção de definições de diagnóstico, tem de especificar os dados a recolher. 

## <a name="enable-logging-for-elastic-pools-or-managed-instance"></a>Ativar o registo para conjuntos elásticos, ou à instância gerida

Conjuntos elásticos e as instâncias geridas como contentores de base de dados tem sua própria telemetria de diagnóstico que não está ativada por predefinição. Tenha em atenção de que esta telemetria está separada de telemetria de diagnóstico de base de dados. Isso é por isso que a transmissão em fluxo de telemetria de diagnóstico para conjuntos elásticos e a instância gerida precisa de ser configurado, além disso, com a configuração de telemetria de diagnóstico de base de dados, como explicado mais abaixo. 

### <a name="configure-streaming-of-diagnostics-telemetry-for-elastic-pools"></a>Configurar a transmissão em fluxo de telemetria de diagnóstico para conjuntos elásticos

A telemetria de diagnóstico seguinte está disponível para a recolha para elastic pools de recursos:

| Recurso | Monitorização de telemetria |
| :------------------- | ------------------- |
| **Conjunto elástico** | [Todas as métricas](sql-database-metrics-diag-logging.md#all-metrics) contém a percentagem de eDTU/CPU, limite de eDTU/da CPU, físico dados lidos percentagem, log escrever percentagem, percentagem de sessões, percentagem de funções de trabalho, armazenamento, percentagem de armazenamento, limite de armazenamento e a percentagem de armazenamento do XTP. |

Para ativar a transmissão em fluxo de telemetria de diagnóstico **recurso de conjunto elástico**, siga estes passos:

- Ir para o recurso de conjunto elástico no portal do Azure
- Selecione **as definições de diagnóstico**
- Selecione **ativar os diagnósticos** se não existem definições anteriores existe ou selecione **Editar definição** para editar uma definição anterior
- Escreva o nome para a definição – para sua própria referência
- Selecione para o recurso a transmitir dados de diagnóstico do conjunto elástico: **arquivo para a conta de armazenamento**, **Stream para um hub de eventos**, ou **enviar para o Log Analytics**
- No caso do Log Analytics é selecionado, selecione **configurar** e criar uma nova área de trabalho ao selecionar **+ criar nova área de trabalho**, ou selecione uma área de trabalho existente
- Selecione a caixa de verificação para a telemetria de diagnóstico do conjunto elástico **AllMetrics**
- Clicar em **Guardar**

Repita os passos acima para cada conjunto elástico que pretende monitorizar.

### <a name="configure-streaming-of-diagnostics-telemetry-for-managed-instance"></a>Configurar a transmissão em fluxo de telemetria de diagnóstico para a instância gerida

A telemetria de diagnóstico seguinte está disponível para a recolha para o recurso de instância gerida:

| Recurso | Monitorização de telemetria |
| :------------------- | ------------------- |
| **Instância gerida** | [ResourceUsageStats](sql-database-metrics-diag-logging.md#resource-usage-stats) contém a contagem de vCores, percentagem de CPU média, pedidos de e/s, bytes lidos/escritos, reservado espaço de armazenamento, o espaço de armazenamento utilizado. |

Para ativar a transmissão em fluxo de telemetria de diagnóstico **recurso de instância gerida**, siga estes passos:

- Ir para o recurso de instância gerida no portal do Azure
- Selecione **as definições de diagnóstico**
- Selecione **ativar os diagnósticos** se não existem definições anteriores existe ou selecione **Editar definição** para editar uma definição anterior
- Escreva o nome para a definição – para sua própria referência
- Selecione para o recurso a transmitir dados de diagnóstico do conjunto elástico: **arquivo para a conta de armazenamento**, **Stream para um hub de eventos**, ou **enviar para o Log Analytics**
- No caso do Log Analytics é selecionado, criar ou utilizar uma área de trabalho existente
- Selecione a caixa de verificação, por exemplo telemetria de diagnóstico **ResourceUsageStats**
- Clicar em **Guardar**

Repita os passos acima para cada instância gerida que pretende monitorizar.

## <a name="enable-logging-for-azure-sql-database-or-databases-in-managed-instance"></a>Ativar o registo de base de dados do Azure SQL ou bases de dados na instância gerida

Métricas e diagnósticos de registo na base de dados SQL e bases de dados na instância gerida não está ativada por predefinição.

A telemetria de diagnóstico seguinte está disponível para a recolha para bases de dados do Azure SQL e bases de dados na instância gerida:

| Telemetria para bases de dados de monitorização | Suporte de base de dados SQL do Azure | Base de dados no suporte de instância gerida |
| :------------------- | ------------------- | ------------------- |
| [Todas as métricas](sql-database-metrics-diag-logging.md#all-metrics): limitar a percentagem DTU contém/da CPU, DTU/CPU, percentagem, percentagem de escrita de registo, de leitura de dados físicos com êxito/com falhas/bloqueado por ligações de firewall, percentagem de sessões, percentagem de funções de trabalho, armazenamento, percentagem de armazenamento, e Percentagem de armazenamento do XTP. | Sim | Não |
| [QueryStoreRuntimeStatistics](sql-database-metrics-diag-logging.md#query-store-runtime-statistics): contém informações sobre as estatísticas de tempo de execução de consulta, como, por exemplo são a utilização da CPU e estatísticas de duração de consulta. | Sim | Sim |
| [QueryStoreWaitStatistics](sql-database-metrics-diag-logging.md#query-store-wait-statistics): contém informações sobre as estatísticas de espera de consulta, que indica o que as suas consultas aguardado, tais como CPU, o registo e o bloqueio. | Sim | Sim |
| [Erros](sql-database-metrics-diag-logging.md#errors-dataset): contém informações sobre erros do SQL que tenham acontecido nesta base de dados. | Sim | Não |
| [DatabaseWaitStatistics](sql-database-metrics-diag-logging.md#database-wait-statistics-dataset): contém informações sobre quanto um banco de dados de tempo gasto aguardando tipos diferentes de espera. | Sim | Não |
| [Tempos limite](sql-database-metrics-diag-logging.md#time-outs-dataset): contém informações sobre tempos limite que ocorreu numa base de dados. | Sim | Não |
| [Blocos](sql-database-metrics-diag-logging.md#blockings-dataset): contém informações sobre o bloqueio de eventos que ocorreram numa base de dados. | Sim | Não |
| [SQLInsights](sql-database-metrics-diag-logging.md#intelligent-insights-dataset): contém informações inteligentes sobre o desempenho. [Saiba mais sobre informações inteligentes](sql-database-intelligent-insights.md). | Sim | Sim |

### <a name="azure-portal"></a>Portal do Azure

Transmissão em fluxo de telemetria de diagnóstico para a base de dados do Azure SQL e bases de dados na instância gerida para destinos de armazenamento do Azure, os hubs de eventos, ou o Log Analytics é configurado através do menu de definições de diagnóstico para cada uma das bases de dados no portal do Azure.

### <a name="configure-streaming-of-diagnostics-telemetry-for-azure-sql-database"></a>Configurar a transmissão em fluxo de telemetria de diagnóstico para a base de dados do Azure SQL

Para ativar a transmissão em fluxo de telemetria de diagnóstico **base de dados do Azure SQL**, siga estes passos:

- Aceda ao seu recurso de base de dados do Azure SQL
- Selecione **as definições de diagnóstico**
- Selecione **ativar os diagnósticos** se não existem definições anteriores existe ou selecione **Editar definição** para editar uma definição anterior
- Até três (3) paralelas ligações a telemetria de diagnóstico de fluxo pode ser criado. Para configurar várias paralela de transmissão em fluxo de dados de diagnóstico para vários recursos, selecione **+ Adicionar definição de diagnóstico** para criar uma configuração adicional.

   ![Ativar no portal do Azure](./media/sql-database-metrics-diag-logging/enable-portal.png)

- Escreva o nome para a definição – para sua própria referência
- Selecione para o recurso a transmitir dados de diagnóstico da base de dados: **arquivo para a conta de armazenamento**, **Stream para um hub de eventos**, ou **enviar para o Log Analytics**
- Para a experiência de monitorização padrão, selecione as caixas de verificação para a telemetria de registo de diagnóstico de base de dados: **SQLInsights**, **AutomaticTuning**, **QueryStoreRuntimeStatistics** , **QueryStoreWaitStatistics**, **erros**, **DatabaseWaitStatistics**, **tempos limite**, **blocos** , **Deadlocks**. Este telemetria é o evento com base e fornece o padrão de experiência de monitorização.
- Experiência de monitorização avançada, selecione a caixa de verificação **AllMetrics**. Esta é uma telemetria com base de 1 minuto para a telemetria de diagnóstico de base de dados, conforme descrito acima. 

   ![Definições de diagnósticos](./media/sql-database-metrics-diag-logging/diagnostics-portal.png)

Repita os passos acima para cada base de dados de SQL do Azure que pretende monitorizar.

> [!NOTE]
> Não é possível ativar o log de auditoria de definições de diagnóstico de base de dados, mesmo que a opção é mostrada. Para ativar a transmissão de registos de auditoria, consulte [configurar a auditoria da base de dados](sql-database-auditing.md#subheading-2)
>

### <a name="configure-streaming-of-diagnostics-telemetry-for-databases-in-managed-instance"></a>Configurar a transmissão em fluxo de telemetria de diagnóstico para bases de dados na instância gerida

Para ativar a transmissão em fluxo de telemetria de diagnóstico **bases de dados na instância gerida**, siga estes passos:

- Aceda à sua base de dados na instância gerida
- Selecione **as definições de diagnóstico**
- Selecione **ativar os diagnósticos** se não existem definições anteriores existe ou selecione **Editar definição** para editar uma definição anterior
- Até três (3) paralelas ligações a telemetria de diagnóstico de fluxo pode ser criado. Para configurar várias paralela de transmissão em fluxo de dados de diagnóstico para vários recursos, selecione **+ Adicionar definição de diagnóstico** para criar uma configuração adicional.
- Escreva o nome para a definição – para sua própria referência
- Selecione para o recurso a transmitir dados de diagnóstico da base de dados: **arquivo para a conta de armazenamento**, **Stream para um hub de eventos**, ou **enviar para o Log Analytics**
- Selecione as caixas de verificação para a telemetria de diagnóstico de base de dados: **SQLInsights**, **QueryStoreRuntimeStatistics**, **QueryStoreWaitStatistics** e **erros**

   ![Definições de diagnósticos](./media/sql-database-metrics-diag-logging/diagnostics-portal-mi.png)

Repita os passos acima para cada base de dados na instância gerida que pretende monitorizar.

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

### <a name="to-configure-multiple-azure-subscriptions"></a>Para configurar várias subscrições do Azure

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

Saiba mais sobre como [alterar as definições de diagnóstico, utilizando a API de REST do Azure Monitor](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings). 

### <a name="resource-manager-template"></a>Modelo do Resource Manager

Saiba mais sobre como [ative as definições de diagnóstico durante a criação de recursos com um modelo do Resource Manager](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md). 

## <a name="stream-into-log-analytics"></a>Stream para o Log Analytics 

Registos de diagnóstico e métricas de base de dados SQL podem ser transmitidos para o Log Analytics, utilizando o incorporado **enviar para o Log Analytics** opção no portal. Também pode ativar o Log Analytics utilizando uma definição de diagnóstico através de cmdlets do PowerShell, a CLI do Azure ou a API de REST do Azure Monitor.

### <a name="installation-overview"></a>Visão geral da instalação

A monitorização de uma frota de base de dados SQL é simples com o Log Analytics. São necessários três passos:

1. Crie um recurso do Log Analytics.

2. Configure bases de dados aos registos de diagnóstico e métricas de registo para o recurso do Log Analytics que criou.

3. Instalar o **a análise de SQL do Azure** solução no Azure Marketplace.

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

## <a name="data-retention-policy-and-pricing"></a>Política de retenção de dados e preços

Se selecionar os Hubs de eventos ou uma conta de armazenamento, pode especificar uma política de retenção. Esta política elimina os dados que é mais antigos que um período de tempo selecionado. Se especificar o Log Analytics, a política de retenção depende do escalão de preço selecionado. Aplica-se em consumo de telemetria de diagnóstico acima as unidades gratuitas de ingestão de dados alocados a cada mês. As unidades gratuitas de ingestão de dados fornecido ativar gratuita monitorização de vários bancos de dados por mês. Tenha em atenção que as bases de dados mais ativos com cargas de trabalho mais pesadas serão ingestão de dados mais versus bases de dados inativos. Para obter mais informações, consulte [preços do Log Analytics](https://azure.microsoft.com/pricing/details/monitor/). 

Se estiver a utilizar a análise de SQL do Azure, pode monitorizar facilmente o seu consumo de ingestão de dados na solução, selecione a área de trabalho do OMS no menu de navegação de análise de SQL do Azure e, em seguida, selecionar a utilização e custos estimados.

## <a name="metrics-and-logs-available"></a>Métricas e registos disponíveis

Pode encontrar o conteúdo de telemetria de monitoramento detalhado de métricas e registos disponíveis para a base de dados do Azure SQL, os conjuntos elásticos, instância gerida e bases de dados na instância gerida para sua **análise personalizada** e **aplicação desenvolvimento** usando [idioma de análise de SQL](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries).

## <a name="all-metrics"></a>Todas as métricas

### <a name="all-metrics-for-elastic-pools"></a>Todas as métricas para conjuntos elásticos

|**Recurso**|**Métricas**|
|---|---|
|Conjunto elástico|percentagem de eDTU, eDTU utilizado, o limite de eDTU, percentagem de CPU, percentagem de leitura de dados físicos, log escrever percentagem, percentagem de sessões, percentagem de funções de trabalho, armazenamento, percentagem de armazenamento, limite de armazenamento, percentagem de armazenamento do XTP |

### <a name="all-metrics-for-azure-sql-database"></a>Todas as métricas para a base de dados do Azure SQL

|**Recurso**|**Métricas**|
|---|---|
|Base de Dados SQL do Azure|Percentagem de DTU, DTU utilizado, o limite DTU, percentagem de CPU, percentagem de leitura de dados físicos, de registo de escrita em percentagem, com êxito/com falhas/bloqueado por ligações de firewall, percentagem de sessões, percentagem de funções de trabalho, armazenamento, percentagem de armazenamento, percentagem de armazenamento do XTP, e deadlocks |

## <a name="logs"></a>Registos

### <a name="logs-for-managed-instance"></a>Registos para a instância gerida

### <a name="resource-usage-stats"></a>Estatísticas de utilização de recursos

|Propriedade|Descrição|
|---|---|
|TenantId|O ID de inquilino.|
|SourceSystem|Sempre: Azure|
|TimeGenerated [UTC]|Carimbo de hora quando o registo foi registado.|
|Tipo|Sempre: AzureDiagnostics|
|ResourceProvider|Nome do fornecedor de recursos. Always: MICROSOFT.SQL|
|Categoria|Nome da categoria. Sempre: ResourceUsageStats|
|Recurso|Nome do recurso.|
|ResourceType|Nome do tipo de recurso. Sempre: MANAGEDINSTANCES|
|SubscriptionId|GUID que pertence a base de dados de subscrição.|
|ResourceGroup|Nome do grupo de recursos que a base de dados pertence.|
|LogicalServerName_s|Nome da instância gerida.|
|ResourceId|URI do recurso.|
|SKU_s|Produto de instância SKU gerido|
|virtual_core_count_s|Numver de vCores disponíveis|
|avg_cpu_percent_s|Percentagem de CPU média|
|reserved_storage_mb_s|Capacidade de armazenamento reservada na instância gerida|
|storage_space_used_mb_s|Armazenamento utilizado na instância gerida|
|io_requests_s|Contagem IOPS|
|io_bytes_read_s|Bytes IOPS de leitura|
|io_bytes_written_s|Bytes IOPS escritos|

### <a name="logs-for-azure-sql-database-and-managed-instance-database"></a>Registos para a base de dados do Azure SQL e a instância gerida da base de dados

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

### <a name="automatic-tuning-dataset"></a>Conjunto de dados de otimização automática

|Propriedade|Descrição|
|---|---|
|TenantId|O ID de inquilino.|
|SourceSystem|Sempre: Azure|
|TimeGenerated [UTC]|Carimbo de hora quando o registo foi registado.|
|Tipo|Sempre: AzureDiagnostics|
|ResourceProvider|Nome do fornecedor de recursos. Always: MICROSOFT.SQL|
|Categoria|Nome da categoria. Sempre: AutomaticTuning|
|Recurso|Nome do recurso.|
|ResourceType|Nome do tipo de recurso. Sempre: Servidores/bases de dados|
|SubscriptionId|GUID que pertence a base de dados de subscrição.|
|ResourceGroup|Nome do grupo de recursos que a base de dados pertence.|
|LogicalServerName_s|Nome do servidor que a base de dados pertence.|
|LogicalDatabaseName_s|Nome da base de dados.|
|ElasticPoolName_s|Nome do conjunto elástico que pertence a base de dados, se aplicável.|
|DatabaseName_s|Nome da base de dados.|
|ResourceId|URI do recurso.|
|RecommendationHash_s|Valor hash exclusivo de recomendação de otimização automática.|
|OptionName_s|Operação de otimização automática.|
|Schema_s|Esquema de banco de dados.|
|Table_s|Tabela afetada.|
|IndexName_s|Nome do índice.|
|IndexColumns_s|Nome da coluna.|
|IncludedColumns_s|Colunas incluídas.|
|EstimatedImpact_s|Estima o impacto da recomendação de otimização automática JSON.|
|Event_s|Tipo de evento de otimização automática.|
|Timestamp_t|Última atualização timestamp.|

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
