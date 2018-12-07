---
title: Recursos suportados para alertas de métricas no Azure Monitor
description: Referência no suporte de métricas e registos para alertas de métricas no Azure Monitor
author: snehithm
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/29/2018
ms.author: snmuvva
ms.component: alerts
ms.openlocfilehash: 02412f2ac4ff6ce0394ad46e03ae4c89f924624f
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2018
ms.locfileid: "53016406"
---
# <a name="supported-resources-for-metric-alerts-in-azure-monitor"></a>Recursos suportados para alertas de métricas no Azure Monitor

Agora suporta o Monitor do Azure um [novo tipo de alerta de métrica](monitoring-overview-alerts.md) que apresenta vantagens consideráveis sobre o mais antigo [alertas de métricas clássicas](monitoring-overview-alerts-classic.md). As métricas estão disponíveis para [grande lista de serviços do Azure](monitoring-supported-metrics.md). Os alertas mais recente suportam um subconjunto (crescente) dos tipos de recursos. Este artigo lista esse subconjunto.


Também pode utilizar alertas de métricas mais recente em populares registos do Log Analytics extraídos como métricas. Para obter mais informações, veja [alertas de métrica para os registos](monitoring-metric-alerts-logs.md).

## <a name="portal-powershell-cli-rest-support"></a>Portal, PowerShell, CLI, REST suporte
Atualmente, pode criar alertas de métricas mais recente apenas no portal do Azure, [REST API](https://docs.microsoft.com/rest/api/monitor/metricalerts/), ou [modelos do Resource Manager](monitoring-create-metric-alerts-with-templates.md). Suporte para configurar alertas mais recentes com as versões do PowerShell e CLI do Azure 2.0 e versões posteriores estará disponível brevemente.

## <a name="metrics-and-dimensions-supported"></a>Métricas e dimensões suportadas
Alertas de métricas mais recente suportam alertas de métricas que usam dimensões. Pode usar as dimensões para filtrar a métrica para o nível certo. Podem ser exploradas e visualizadas a partir de todas as métricas suportadas, juntamente com dimensões aplicáveis [do Azure Monitor - Explorador de métricas](monitoring-metric-charts.md).

Eis a lista completa de origens de métrica de monitor do Azure suportadas pelos alertas mais recentes:

|Tipo de recurso  |Dimensões suportadas  | Métricas disponíveis|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | Sim        | [Gestão de API](monitoring-supported-metrics.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     Sim   | [Contas de automatização](monitoring-supported-metrics.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | N/A| [Contas do batch](monitoring-supported-metrics.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    N/A     |[Cache de Redis do Azure](monitoring-supported-metrics.md#microsoftcacheredis)|
|Microsoft.CognitiveServices/accounts     |    N/A     | [Serviços Cognitivos](monitoring-supported-metrics.md#microsoftcognitiveservicesaccounts)|
|Microsoft.Compute/virtualMachines     |    N/A     | [Máquinas Virtuais](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   N/A      |[Conjuntos de dimensionamento de máquinas virtuais](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | Sim| [Grupos de contentores](monitoring-supported-metrics.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.ContainerService/managedClusters | Sim | [Clusters geridos](monitoring-supported-metrics.md#microsoftcontainerservicemanagedclusters)|
|Microsoft.DataFactory/datafactories| Sim| [V1 de fábricas de dados](monitoring-supported-metrics.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories     |   Sim     |[V2 de fábricas de dados](monitoring-supported-metrics.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   N/A      |[DB para MySQL](monitoring-supported-metrics.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    N/A     | [DB para o PostgreSQL](monitoring-supported-metrics.md#microsoftdbforpostgresqlservers)|
|Microsoft.EventHub/namespaces     |  Sim      |[Hubs de Eventos](monitoring-supported-metrics.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Não | [cofres](monitoring-supported-metrics.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows     |     N/A    |[Aplicações Lógicas](monitoring-supported-metrics.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    N/A     | [Gateways de aplicação](monitoring-supported-metrics.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/expressRouteCircuits | N/A |  [Express Route circuitos](monitoring-supported-metrics.md#microsoftnetworkexpressroutecircuits) |
|Microsoft.Network/dnsZones | N/A| [Zonas DNS](monitoring-supported-metrics.md#microsoftnetworkdnszones) |
|Network/loadbalancers (apenas para o SKU Standard)| Sim| [Balanceadores de carga](monitoring-supported-metrics.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/publicipaddresses     |  N/A       |[Endereços IP públicos](monitoring-supported-metrics.md#microsoftnetworkpublicipaddresses)|
|Microsoft.PowerBIDedicated/capacities | N/A | [Capacidades](monitoring-supported-metrics.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Network/trafficManagerProfiles | Sim | [Perfis do Gestor de tráfego](monitoring-supported-metrics.md#microsoftnetworktrafficmanagerprofiles) |
|Microsoft.Search/searchServices     |   N/A      |[Serviços de pesquisa](monitoring-supported-metrics.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  Sim       |[Service Bus](monitoring-supported-metrics.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    Sim     | [Contas de armazenamento](monitoring-supported-metrics.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     Sim    | [Serviços de BLOBs](monitoring-supported-metrics.md#microsoftstoragestorageaccountsblobservices), [serviços de ficheiros](monitoring-supported-metrics.md#microsoftstoragestorageaccountsfileservices), [serviços de fila](monitoring-supported-metrics.md#microsoftstoragestorageaccountsqueueservices) e [serviços de tabela](monitoring-supported-metrics.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  N/A       | [Stream Analytics](monitoring-supported-metrics.md#microsoftstreamanalyticsstreamingjobs)|
| Microsoft.Web/serverfarms | Sim | [Planos de serviço de aplicações](monitoring-supported-metrics.md#microsoftwebserverfarms)  |
| Microsoft.Web/sites | Sim | [Serviços aplicacionais](monitoring-supported-metrics.md#microsoftwebsites-excluding-functions) e [funções](monitoring-supported-metrics.md#microsoftwebsites-functions)|
| Microsoft.Web/sites/slots | Sim | [Ranhuras de serviço de aplicações](monitoring-supported-metrics.md#microsoftwebsitesslots)|
|Microsoft.OperationalInsights/workspaces| Sim|[Áreas de trabalho do log Analytics](monitoring-supported-metrics.md#microsoftoperationalinsightsworkspaces)|



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

* Saiba mais sobre a nova [experiência de alertas](monitoring-overview-alerts.md).
* Saiba mais sobre [alertas de registo no Azure](monitor-alerts-unified-log.md).
* Saiba mais sobre [alertas no Azure](monitoring-overview-alerts.md).
