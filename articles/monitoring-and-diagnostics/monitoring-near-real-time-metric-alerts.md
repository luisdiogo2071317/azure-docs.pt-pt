---
title: "Quase em tempo real alertas métricas no Monitor do Azure | Microsoft Docs"
description: "Saiba como utilizar quase em tempo real alertas métricas para monitorizar as métricas de recurso do Azure com uma frequência tão pequena como 1 minuto."
author: snehithm
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: snmuvva, vinagara
ms.custom: 
ms.openlocfilehash: 88995b1f3350fe485e28efccc93779ae0a42eb97
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/12/2018
---
# <a name="near-real-time-metric-alerts-preview"></a>Quase em tempo real alertas métricas (pré-visualização)
Monitor do Azure suporta um novo tipo de alerta chamado quase em tempo real alertas métricas (pré-visualização). Esta funcionalidade está atualmente em pré-visualização pública.

Quase em tempo real métrica alertas diferem dos alertas de métricas regulares de algumas formas:

- **Melhorado latência**: quase em tempo real métrica alertas podem monitorizar as alterações nos valores métricas com uma frequência tão pequena como um minuto.
- **Mais controlo sobre condições métricas**: pode definir regras de alerta rica em quase em tempo real alertas métricas. Os alertas suportam os valores máximo, mínimos, médios e totais de métricas de monitorização.
- **Métricas de registos**: de dados de registo populares entra [Log Analytics](../log-analytics/log-analytics-overview.md), métricas pode ser extraídas no Monitor do Azure e ser alertadas em quase em tempo real base
- **Combinar a monitorização de várias métricas**: quase em tempo real métrica alertas podem monitorizar as métricas de vários (atualmente, até duas métricas) com uma única regra. Um alerta é acionado se ambas as métricas infringir os respetivos limiares para o período de tempo especificado.
- **Sistema de notificação modulares**: quase em tempo real métrica utilizam alertas [grupos ação](monitoring-action-groups.md). Pode utilizar grupos de ação para criar modulares ações. Pode reutilizar os grupos de ação para múltiplas regras de alerta.

> [!NOTE]
> O alerta de métrico near em tempo real está atualmente em pré-visualização pública. E métricas de funcionalidades de registos estão presentes no *limitado* pré-visualização pública. A experiência de utilizador e a funcionalidade está sujeita a alterações.
>

## <a name="metrics-and-dimensions-supported"></a>Métricas e dimensões suportadas
Quase em tempo real métrica alertas suportam alertas com base nas métricas que utilizam dimensões. Pode utilizar dimensões para a métrica para o nível adequado de filtro. Podem ser explorou e visualizadas a partir de todas as métricas suportadas juntamente com dimensões aplicáveis [Azure Monitor - Explorador de métricas (pré-visualização)](monitoring-metric-charts.md).

Eis a lista completa das origens de métrica de monitor do Azure com base em que são suportados para quase em tempo real alertas métricas:

|Nome da métricos/detalhes  |Dimensões suportadas  |
|---------|---------|
|Microsoft.ApiManagement/service     | Sim        |
|Microsoft.Automation/automationAccounts     |     N/A    |
|Microsoft.Automation/automationAccounts     |   N/A      |
|Microsoft.Cache/Redis     |    N/A     |
|Microsoft.Compute/virtualMachines     |    N/A     |
|Microsoft.Compute/virtualMachineScaleSets     |   N/A      |
|Microsoft.DataFactory/factories     |   N/A      |
|Microsoft.DBforMySQL/servers     |   N/A      |
|Microsoft.DBforPostgreSQL/servers     |    N/A     |
|Microsoft.EventHub/namespaces     |   N/A      |
|Microsoft.Logic/workflows     |     N/A    |
|Microsoft.Network/applicationGateways     |    N/A     |
|Microsoft.Network/publicipaddresses     |  N/A       |
|Microsoft.Search/searchServices     |   N/A      |
|Microsoft.ServiceBus/namespaces     |  N/A       |
|Microsoft.Storage/storageAccounts     |    Sim     |
|Microsoft.Storage/storageAccounts/services     |     Sim    |
|Microsoft.StreamAnalytics/streamingjobs     |  N/A       |
|Microsoft.CognitiveServices/accounts     |    N/A     |


Métricas de registos, suporta atualmente os seguintes registos OMS populares:
- [Contadores de desempenho](../log-analytics/log-analytics-data-sources-performance-counters.md) para máquinas Windows e Linux
- Registos de heartbeat de máquinas
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
> Métrica específica e/ou dimensão só será apresentada se os dados para o mesmo existem num período escolhido

## <a name="create-a-near-real-time-metric-alert"></a>Criar um alerta de métrico near em tempo real
Atualmente, pode criar quase em tempo real alertas métricas apenas no portal do Azure. Suporte para configurar quase em tempo real alertas métricas utilizando o PowerShell, a interface de linha de comandos do Azure (CLI do Azure) e as APIs REST da Azure Monitor estará disponível brevemente.

A experiência para criar um alerta de métrico near em tempo real foi movido para o novo **alertas (pré-visualização)** página. Mesmo que os alertas atuais página apresenta **alerta adicionar quase em tempo real métrica**, são redirecionados para o **alertas (pré-visualização)** página.

Para saber como criar um alerta de métrico em tempo real near, consulte [criar uma regra de alerta no portal do Azure](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal).

## <a name="manage-near-real-time-metric-alerts"></a>Gerir quase em tempo real alertas métricas
Depois de criar um alerta de métrico near em tempo real, pode gerir o alerta, utilizando os passos descritos no [gerir os alertas no portal do Azure](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal).

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

* Saiba mais sobre as novas [alertas experiência (pré-visualização)](monitoring-overview-unified-alerts.md).
* Saiba mais sobre [registar alertas nos alertas do Azure (pré-visualização)](monitor-alerts-unified-log.md).
* Saiba mais sobre [alertas no Azure](monitoring-overview-alerts.md).
