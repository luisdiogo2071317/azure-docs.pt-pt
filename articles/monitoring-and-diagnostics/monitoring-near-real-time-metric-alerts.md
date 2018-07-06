---
title: Recursos suportados para alertas de métricas mais recente do Azure Monitor
description: Referência no suporte de métricas e registos para mais recente do Azure perto de alertas de métricas em tempo real.
author: snehithm
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/27/2018
ms.author: snmuvva
ms.component: alerts
ms.openlocfilehash: 01c0b5897ab47a2a5091646aed1977779cf0234c
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868034"
---
# <a name="supported-metrics-and-creation-methods-for-new-metric-alerts"></a>Métodos com métricas e criação de suporte para novos alertas de métricas
Agora suporta o Monitor do Azure um [novo tipo de alerta de métrica](monitoring-overview-unified-alerts.md) que apresenta vantagens consideráveis sobre o mais antigo [alertas de métricas clássicas](insights-alerts-portal.md). Os alertas mais antigos suportam uma [grande lista de métricas](monitoring-supported-metrics.md). Os alertas mais recente suportam um subconjunto (crescente) dessa lista maiores. Este artigo lista esse subconjunto. 

## <a name="portal-powershell-cli-rest-support"></a>Portal, PowerShell, CLI, REST suporte
Atualmente, pode criar alertas de métricas mais recente apenas no portal do Azure, [REST API](https://docs.microsoft.com/en-us/rest/api/monitor/metricalerts/createorupdate) ou [modelos do Resource Manager](monitoring-create-metric-alerts-with-templates.md). Suporte para configurar alertas mais recentes, com o PowerShell e a interface de linha de comandos (CLI 2.0 do Azure) do Azure estará disponível brevemente.

## <a name="metrics-and-dimensions-supported"></a>Métricas e dimensões suportadas
Alertas de métricas mais recente suportam alertas de métricas que usam dimensões. Pode usar as dimensões para filtrar a métrica para o nível certo. Podem ser exploradas e visualizadas a partir de todas as métricas suportadas, juntamente com dimensões aplicáveis [do Azure Monitor - Explorador de métricas (pré-visualização)](monitoring-metric-charts.md).

Eis a lista completa de origens de métrica de monitor do Azure suportadas pelos alertas mais recentes:

|Tipo de recurso  |Dimensões suportadas  | Métricas disponíveis|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | Sim        | [Gestão de API](monitoring-supported-metrics.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     Sim   | [Contas de automatização](monitoring-supported-metrics.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | N/A| [Contas do batch](monitoring-supported-metrics.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    N/A     |[Cache de Redis](monitoring-supported-metrics.md#microsoftcacheredis)|
|Microsoft.Compute/virtualMachines     |    N/A     | [Máquinas Virtuais](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   N/A      |[Conjuntos de dimensionamento de máquinas virtuais](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | Sim| [Grupos de contentores](monitoring-supported-metrics.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.DataFactory/datafactories| Sim| [V1 de fábricas de dados](monitoring-supported-metrics.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories     |   Sim     |[V2 de fábricas de dados](monitoring-supported-metrics.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   N/A      |[DB para MySQL](monitoring-supported-metrics.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    N/A     | [DB para o PostgreSQL](monitoring-supported-metrics.md#microsoftdbforpostgresqlservers)|
|Microsoft.EventHub/namespaces     |  Sim      |[Hubs de Eventos](monitoring-supported-metrics.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Não | [Cofres](monitoring-supported-metrics.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows     |     N/A    |[Aplicações Lógicas](monitoring-supported-metrics.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    N/A     | [Gateways de aplicação](monitoring-supported-metrics.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/dnsZones | N/A| [Zonas DNS](monitoring-supported-metrics.md#microsoftnetworkdnszones) |
|Network/loadbalancers (apenas para o SKU Standard)| Sim| [Balanceadores de carga](monitoring-supported-metrics.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/publicipaddresses     |  N/A       |[Endereços IP público](monitoring-supported-metrics.md#microsoftnetworkpublicipaddresses)|
|Microsoft.PowerBIDedicated/capacities | N/A | [Capacidades](monitoring-supported-metrics.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Search/searchServices     |   N/A      |[Serviços de pesquisa](monitoring-supported-metrics.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  Sim       |[Service Bus](monitoring-supported-metrics.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    Sim     | [Contas de armazenamento](monitoring-supported-metrics.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     Sim    | [Serviços de BLOBs](monitoring-supported-metrics.md#microsoftstoragestorageaccountsblobservices), [serviços de ficheiros](monitoring-supported-metrics.md#microsoftstoragestorageaccountsfileservices), [serviços de fila](monitoring-supported-metrics.md#microsoftstoragestorageaccountsqueueservices) e [serviços de tabela](monitoring-supported-metrics.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  N/A       | [Stream Analytics](monitoring-supported-metrics.md#microsoftstreamanalyticsstreamingjobs)|
|Microsoft.CognitiveServices/accounts     |    N/A     | [Serviços Cognitivos](monitoring-supported-metrics.md#microsoftcognitiveservicesaccounts)|
|Microsoft.OperationalInsights/workspaces (pré-visualização) | Sim|[Áreas de trabalho do log Analytics](#log-analytics-logs-as-metrics-for-alerting)|


## <a name="log-analytics-logs-as-metrics-for-alerting"></a>Registos de análise de registo como métricas para alertas 

Também pode utilizar alertas de métricas mais recente em populares registos do Log Analytics extraídos como métricas como parte das métricas de pré-visualização de registos.  
- [Contadores de desempenho](../log-analytics/log-analytics-data-sources-performance-counters.md) para máquinas Windows e Linux
- [Registos de heartbeat de agente de estado de funcionamento](../operations-management-suite/oms-solution-agenthealth.md)
- [Gestão de atualizações](../operations-management-suite/oms-solution-update-management.md) registos
 
> [!NOTE]
> Métrica específica e/ou de dimensão só será apresentada se os dados para o mesmo existem num período de escolhido. Estas métricas estão disponíveis para clientes com áreas de trabalho no E.U.A. leste, e.u.a. centro-oeste e Europa Ocidental que tiverem optado por à pré-visualização. Se gostaria de fazer parte desta pré-visualização, inscreva-se usando [a pesquisa](https://aka.ms/MetricLogPreview).

A seguinte lista de origens de métrica baseados no registo de Log Analytics é suportada:

Nome/Detalhes da métrica  |Dimensões suportadas  | Tipo de registo  |
|---------|---------|---------|
|Average_Avg. Disco seg/leitura     |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Windows      |
| Average_Avg. Disco seg/escritas     |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Windows      |
| Comprimento da fila de disco Average_Current   |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Windows      |
| Average_Disk leituras/seg    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Windows      |
| Average_Disk transferências/seg    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Windows      |
|   Average_ % de espaço livre    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Windows      |
| MBytes de Average_Available     |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Windows      |
| Average_ % comprometida Bytes em utilização    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Windows      |
| Average_Bytes recebidos/seg    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Windows      |
|  Average_Bytes enviados/seg    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Windows      |
|  Total de Average_Bytes/seg    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Windows      |
|  Average_ % tempo do processador    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Windows      |
|   Comprimento da fila Average_Processor    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Windows      |
|   Average_ % de Inodes livres   |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de espaço livre   |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de Inodes utilizados  |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de espaço utilizado   |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Disk Bytes lidos/seg    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Disk leituras/seg |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Disk transferências/seg |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Disk Zapsané Bajty/s   |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Disk escritas/seg    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Free Megabytes |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Logical Bytes de disco/seg |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de memória disponível |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de espaço de comutação disponível |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de memória utilizada  |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de espaço de comutação utilizado  |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    MBytes de memória Average_Available    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    MBytes de comutação Average_Available  |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Page leituras/seg |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Page escritas/seg    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Pages/seg  |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Espaço de comutação em MBytes Average_Used |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    MBytes de memória Average_Used |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Bytes de Average_Total transmitidos    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Bytes de Average_Total recebidos   |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Bytes de Average_Total    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Pacotes de Average_Total transmitidos  |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Pacotes de Average_Total recebidos |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Total Rx erros    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Total Tx erros    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Total colisões   |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Avg. Disco seg/leitura |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Avg. Disco seg/transferência |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Avg. Disco seg/escritas    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Physical Bytes de disco/seg    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Pct de tempo privilegiado    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Tempo de utilizador Average_Pct  |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Memória de Average_Used kBytes |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Virtual de memória compartilhada  |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de tempo de DPC |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de tempo inativo    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de tempo de interrupção   |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Tempo de espera de e/s de % de Average_ |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de tempo Nice    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de tempo privilegiado  |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % tempo do processador   |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % tempo de utilizador    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Memória física de Average_Free   |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Free espaço nos ficheiros de paginação |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Memória Virtual de Average_Free    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Processes  |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Size armazenados em ficheiros de paginação    |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Uptime |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Users  |     Sim - computador, ObjectName, InstanceName, CounterPath e SourceSystem    |   Contador de desempenho do Linux      |
|    Heartbeat  |     Sim - computador, OSType, versão e SourceComputerId    |   Registos de heartbeat |
|    Atualizar |     Sim - computador, produto, classificação, UpdateState, opcional & aprovado    |   Gestão de Atualizações |



## <a name="payload-schema"></a>Esquema do payload

A operação de POSTAGEM contém o JSON payload seguinte e o esquema para todos os perto de alertas de métricas mais recentes quando um configurado de forma apropriada [grupo de ação](monitoring-action-groups.md) é usado:

```json
{"schemaId":"AzureMonitorMetricAlert","data":
    {
    "version": "2.0",
    "status": "Activated",
    "context": {
    "timestamp": "2018-02-28T10:44:10.1714014Z",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/microsoft.insights/metricAlerts/StorageCheck",
    "name": "StorageCheck",
    "description": "",
    "conditionType": "SingleResourceMultipleMetricCriteria",
    "condition": {
      "windowSize": "PT5M",
      "allOf": [
        {
          "metricName": "Transactions",
          "dimensions": [
            {
              "name": "AccountResourceId",
              "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
            },
            {
              "name": "GeoType",
              "value": "Primary"
            }
          ],
          "operator": "GreaterThan",
          "threshold": "0",
          "timeAggregation": "PT5M",
          "metricValue": 1.0
        },
      ]
    },
    "subscriptionId": "00000000-0000-0000-0000-000000000000",
    "resourceGroupName": "Contoso",
    "resourceName": "diag500",
    "resourceType": "Microsoft.Storage/storageAccounts",
    "resourceId": "/subscriptions/1e3ff1c0-771a-4119-a03b-be82a51e232d/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500",
    "portalLink": "https://portal.azure.com/#resource//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
  },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre a nova [experiência de alertas](monitoring-overview-unified-alerts.md).
* Saiba mais sobre [alertas de registo no Azure](monitor-alerts-unified-log.md).
* Saiba mais sobre [alertas no Azure](monitoring-overview-alerts.md).
