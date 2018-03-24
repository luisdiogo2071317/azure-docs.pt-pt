---
title: Quase em tempo real alertas métricas no Monitor do Azure | Microsoft Docs
description: Saiba como utilizar quase em tempo real alertas métricas para monitorizar as métricas de recurso do Azure com uma frequência tão pequena como 1 minuto.
author: snehithm
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: snmuvva, vinagara
ms.custom: ''
ms.openlocfilehash: 15b9b0b69f3805b3e3af1d3973fd3a77bea62ab9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="use-the-newer-metric-alerts-for-azure-services-in-azure-portal"></a>Utilize os alertas de métricos mais recentes para serviços do Azure no portal do Azure
Monitor do Azure suporta um novo tipo de alerta chamado quase em tempo real alertas métricas. 

Quase em tempo real métrica alertas diferem das [clássicas alertas métricas](insights-alerts-portal.md) de algumas formas:

- **Melhorado latência**: quase em tempo real métrica alertas podem ser executados com uma frequência de cada um minuto. Alertas de métricas mais antigas sempre executam com uma frequência de 5 minutos.
- **Suporte para as métricas multidimensionais**: podem alertá nas métricas dimensional, permitindo-lhe monitorizar um segmento interessante da métrica.
- **Mais controlo sobre condições métricas**: pode definir regras de alerta rica em quase em tempo real alertas métricas. Os alertas suportam os valores máximo, mínimos, médios e totais de métricas de monitorização.
- **Combinar a monitorização de várias métricas**: quase em tempo real métrica alertas podem monitorizar as métricas de vários (atualmente, até duas métricas) com uma única regra. Um alerta é acionado se ambas as métricas infringir os respetivos limiares para o período de tempo especificado.
- **Sistema de notificação modulares**: quase em tempo real métrica utilizam alertas [grupos ação](monitoring-action-groups.md). Pode utilizar grupos de ação para criar modulares ações. Pode reutilizar os grupos de ação para múltiplas regras de alerta.
- **Métricas de registos**: de dados de registo populares entra [Log Analytics](../log-analytics/log-analytics-overview.md), métricas pode ser extraídas no Monitor do Azure e ser alertadas em quase em tempo real base.


## <a name="metrics-and-dimensions-supported"></a>Métricas e dimensões suportadas
Quase em tempo real métrica alertas suportam alertas com base nas métricas que utilizam dimensões. Pode utilizar dimensões para a métrica para o nível adequado de filtro. Podem ser explorou e visualizadas a partir de todas as métricas suportadas juntamente com dimensões aplicáveis [Azure Monitor - Explorador de métricas (pré-visualização)](monitoring-metric-charts.md).

Eis a lista completa das origens de métrica de monitor do Azure com base em que são suportados para quase em tempo real alertas métricas:

|Tipo de recurso  |Dimensões suportadas  | Métricas disponíveis|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | Sim        | [Gestão de API](monitoring-supported-metrics.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     Sim   | [Contas de automatização](monitoring-supported-metrics.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | N/A| [Contas do batch](monitoring-supported-metrics.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    N/A     |[Cache de Redis](monitoring-supported-metrics.md#microsoftcacheredis)|
|Microsoft.Compute/virtualMachines     |    N/A     | [Máquinas Virtuais](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   N/A      |[Conjuntos de dimensionamento de máquina virtual](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.DataFactory/factories     |   Sim     |[V2 de fábricas de dados](monitoring-supported-metrics.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   N/A      |[Base de dados MySQL](monitoring-supported-metrics.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    N/A     | [Base de dados PostgreSQL](monitoring-supported-metrics.md#microsoftdbforpostgresqlservers)|
|Microsoft.EventHub/namespaces     |  Sim      |[Hubs de Eventos](monitoring-supported-metrics.md#microsofteventhubnamespaces)|
|Microsoft.Logic/workflows     |     N/A    |[Aplicações Lógicas](monitoring-supported-metrics.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    N/A     | [Gateways de aplicação](monitoring-supported-metrics.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/publicipaddresses     |  N/A       |[Endereços de IP público](monitoring-supported-metrics.md#microsoftnetworkpublicipaddresses)|
|Microsoft.Search/searchServices     |   N/A      |[Serviços de pesquisa](monitoring-supported-metrics.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  Sim       |[Service Bus](monitoring-supported-metrics.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    Sim     | [Contas de armazenamento](monitoring-supported-metrics.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     Sim    | [Serviços do blob](monitoring-supported-metrics.md#microsoftstoragestorageaccountsblobservices), [serviços de ficheiros](monitoring-supported-metrics.md#microsoftstoragestorageaccountsfileservices), [fila serviços](monitoring-supported-metrics.md#microsoftstoragestorageaccountsqueueservices) e [tabela serviços](monitoring-supported-metrics.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  N/A       | [Stream Analytics](monitoring-supported-metrics.md#microsoftstreamanalyticsstreamingjobs)|
|Microsoft.CognitiveServices/accounts     |    N/A     | [Serviços Cognitivos](monitoring-supported-metrics.md#microsoftcognitiveservicesaccounts)|
|Microsoft.OperationalInsights/workspaces (pré-visualização) | Sim|[Áreas de trabalho de análise de registo](#support-for-oms-logs-as-metrics-for-alerting)|


## <a name="create-a-newer-metric-alert"></a>Criar um alerta de métrico mais recente
Atualmente, pode criar alertas métricas mais recentes apenas no portal do Azure ou REST API. Suporte para configurar quase em tempo real alertas métricas utilizando o PowerShell, a interface de linha de comandos do Azure (CLI do Azure) está disponível em breve.

Para saber como criar um alerta de métrico mais recente no portal do Azure, consulte [criar uma regra de alerta no portal do Azure](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal).

## <a name="manage-newer-metric-alerts"></a>Gerir alertas métricas mais recentes
Depois de criar um alerta de métrico near em tempo real, pode gerir o alerta, utilizando os passos descritos no [gerir os alertas no portal do Azure](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal).

## <a name="support-for-oms-logs-as-metrics-for-alerting"></a>Suporte para os registos do OMS como métricas de alertas

Também pode utilizar quase em tempo real alertas métricas populares registos do OMS extraídas como métricas como parte das métricas da pré-visualização de registos.  
- [Contadores de desempenho](../log-analytics/log-analytics-data-sources-performance-counters.md) para máquinas Windows e Linux
- [Registos de heartbeat para o estado de funcionamento do agente](../operations-management-suite/oms-solution-agenthealth.md)
- [Gestão de atualizações](../operations-management-suite/oms-solution-update-management.md) registos

Eis a lista completa de OMS baseado no registo métricas as origens que são suportados para quase em tempo real alertas métricas:

Nome da métricos/detalhes  |Dimensões suportadas  | Tipo de registo  |
|---------|---------|---------|
|Average_Avg. Disco seg/leitura     |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Windows      |
| Average_Avg. Disco seg/escrita     |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Windows      |
| Comprimento da fila de disco Average_Current   |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Windows      |
| Average_Disk leituras/seg    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Windows      |
| Average_Disk transferências/seg    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Windows      |
|   Average_ % de espaço livre    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Windows      |
| MBytes de Average_Available     |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Windows      |
| Bytes de % consolidada Average_ em utilização    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Windows      |
| Average_Bytes recebidos/seg    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Windows      |
|  Average_Bytes enviados/seg    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Windows      |
|  Total de Average_Bytes/seg    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Windows      |
|  Average_ % de tempo do processador    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Windows      |
|   Comprimento da fila Average_Processor    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Windows      |
|   Average_ % de Inodes livres   |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de espaço livre   |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de Inodes utilizados  |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de espaço utilizado   |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Disk Bytes lidos/seg    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Disk leituras/seg |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Disk transferências/seg |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Escrita de Average_Disk Bytes/seg   |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Disk escritas/seg    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Free Megabytes |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Logical Bytes de disco/seg |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de memória disponível |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de espaço de comutação disponível |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de memória utilizada  |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de espaço de comutação utilizado  |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    MBytes de memória Average_Available    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    MBytes de comutação Average_Available  |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Page leituras/seg |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Page escritas/seg    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Pages/sec  |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Espaço de comutação em MBytes Average_Used |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    MBytes de memória Average_Used |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Bytes de Average_Total transmitidos    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Total Bytes recebidos   |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Bytes de Average_Total    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Pacotes de Average_Total transmitidos  |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Pacotes de Average_Total recebidos |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Total Rx erros    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Total Tx erros    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Total colisões   |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Avg. Disco seg/leitura |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Avg. Disco seg/transferência |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Avg. Disco seg/escrita    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Physical Bytes de disco/seg    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Pct de tempo privilegiado    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Tempo de utilizador Average_Pct  |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Memória Average_Used kBytes |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Memória partilhada do Average_Virtual  |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de tempo de DPC |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de tempo inativo    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de tempo de interrupção   |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Tempo de espera de e/s % Average_ |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de tempo Nice    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de tempo privilegiado  |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % de tempo do processador   |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_ % tempo de utilizador    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Memória física Average_Free   |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Free espaço nos ficheiros de paginação |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Memória de Virtual Average_Free    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Processes  |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Size armazenadas em ficheiros de paginação    |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Uptime |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Average_Users  |     Sim - computador, ObjectName, InstanceName, CounterPath & SourceSystem    |   Contador de desempenho do Linux      |
|    Heartbeat  |     Sim - computador, OSType, versão e SourceComputerId    |   Registos de heartbeat |
|    Atualizar |     Sim - classificação do produto do computador, UpdateState, opcional & aprovado    |   Gestão de Atualizações |

> [!NOTE]
> Métrica específica e/ou dimensão só será apresentada se os dados para o mesmo existem num período escolhido. Estas métricas estão disponíveis para os clientes com áreas de trabalho nos EUA leste, Central EUA oeste e Europa Ocidental, que tem optada ativamente por participar para a pré-visualização. Se gostaria de fazer parte desta pré-visualização, inscreva-se utilizando [o inquérito](https://aka.ms/MetricLogPreview).


## <a name="payload-schema"></a>Esquema de payload

A operação POST contém as seguinte payload JSON e o esquema para todos os quase em tempo real métricas alertas quando adequadamente configurado [grupo ação](monitoring-action-groups.md) é utilizado:

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

* Saiba mais sobre as novas [alertas experiência](monitoring-overview-unified-alerts.md).
* Saiba mais sobre [registar alertas no Azure](monitor-alerts-unified-log.md).
* Saiba mais sobre [alertas no Azure](monitoring-overview-alerts.md).
