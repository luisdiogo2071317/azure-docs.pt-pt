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
ms.date: 12/06/2017
ms.author: snmuvva
ms.custom: 
ms.openlocfilehash: 6370f4596e6b20962c6de0dbcbd5f86c3b7777cc
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2018
---
# <a name="near-real-time-metric-alerts-preview"></a>Quase em tempo real alertas métricas (pré-visualização)
Monitor do Azure suporta um novo tipo de alerta chamado quase em tempo real alertas métricas (pré-visualização). Esta funcionalidade está atualmente em pré-visualização pública.

Quase em tempo real métrica alertas diferem dos alertas de métricas regulares de algumas formas:

- **Melhorado latência**: quase em tempo real métrica alertas podem monitorizar as alterações nos valores métricas com uma frequência tão pequena como 1 minuto.
- **Mais controlo sobre condições métricas**: pode definir regras de alerta rica em quase em tempo real alertas métricas. Os alertas suportam os valores máximo, mínimos, médios e totais de métricas de monitorização.
- **Combinar a monitorização de várias métricas**: quase em tempo real métrica alertas podem monitorizar as métricas de vários (atualmente, até duas métricas) com uma única regra. Um alerta é acionado se ambas as métricas infringir os respetivos limiares para o período de tempo especificado.
- **Sistema de notificação modulares**: quase em tempo real métrica utilizam alertas [grupos ação](monitoring-action-groups.md). Pode utilizar grupos de ação para criar modulares ações. Pode reutilizar os grupos de ação para múltiplas regras de alerta.

> [!NOTE]
> A próxima funcionalidade alerta métrica em tempo real está atualmente em pré-visualização pública. A experiência de utilizador e a funcionalidade está sujeita a alterações.
>

## <a name="resources-you-can-use-with-near-real-time-metric-alerts"></a>Pode utilizar com quase em tempo real alertas de métricas de recursos
Eis a lista completa dos tipos de recursos que são suportados para quase em tempo real alertas métricas:

* Microsoft.ApiManagement/service
* Microsoft.Automation/automationAccounts
* Microsoft.Batch/batchAccounts
* Microsoft.Cache/Redis
* Microsoft.Compute/virtualMachines
* Microsoft.Compute/virtualMachineScaleSets
* Microsoft.DataFactory/factories
* Microsoft.DBforMySQL/servers
* Microsoft.DBforPostgreSQL/servers
* Microsoft.EventHub/namespaces
* Microsoft.Logic/workflows
* Microsoft.Network/applicationGateways
* Microsoft.Network/publicipaddresses
* Microsoft.Search/searchServices
* Microsoft.ServiceBus/namespaces
* Microsoft.Storage/storageAccounts
* Microsoft.Storage/storageAccounts/services
* Microsoft.StreamAnalytics/streamingjobs
* Microsoft.CognitiveServices/accounts

## <a name="near-real-time-metric-alerts-for-metrics-that-use-dimensions"></a>Quase em tempo real alertas métricas com base nas métricas que utilizam dimensões
Quase em tempo real métrica alertas suportam alertas com base nas métricas que utilizam dimensões. Pode utilizar dimensões para a métrica para o nível adequado de filtro. Quase em tempo real métrica alertas com base nas métricas que utilizam dimensões são suportadas para os seguintes tipos de recursos:

* Microsoft.ApiManagement/service
* Storage/storageaccounts (suportado apenas para contas do storage em regiões E.U.A.)
* Microsoft.Storage/storageAccounts/services (suportado apenas para contas do storage em regiões E.U.A.)

## <a name="create-a-near-real-time-metric-alert"></a>Criar um alerta de métrico near em tempo real
Atualmente, pode criar quase em tempo real alertas métricas apenas no portal do Azure. Suporte para configurar quase em tempo real alertas métricas utilizando o PowerShell, a interface de linha de comandos do Azure (CLI do Azure) e as APIs REST da Azure Monitor estará disponível brevemente.

A experiência para criar um alerta de métrico near em tempo real foi movido para o novo **alertas (pré-visualização)** página. Mesmo que os alertas atuais página apresenta **alerta adicionar quase em tempo real métrica**, são redirecionados para o **alertas (pré-visualização)** página.

Para saber como criar um alerta de métrico em tempo real near, consulte [criar uma regra de alerta no portal do Azure](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal).

## <a name="manage-near-real-time-metric-alerts"></a>Gerir quase em tempo real alertas métricas
Depois de criar um alerta de métrico near em tempo real, pode gerir o alerta, utilizando os passos descritos no [gerir os alertas no portal do Azure](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal).

## <a name="payload-schema"></a>Esquema de payload

A operação POST contém as seguinte payload JSON e o esquema para todos os quase em tempo real alertas métricas:

```json
{
    "WebhookName": "Alert1510875839452",
    "RequestBody": {
        "status": "Activated",
        "context": {
            "condition": {
                "metricName": "Percentage CPU",
                "metricUnit": "Percent",
                "metricValue": "17.7654545454545",
                "threshold": "1",
                "windowSize": "10",
                "timeAggregation": "Average",
                "operator": "GreaterThan"
            },
            "resourceName": "ContosoVM1",
            "resourceType": "microsoft.compute/virtualmachines",
            "resourceRegion": "westus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/automationtest/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
            "timestamp": "2017-11-16T23:54:03.9517451Z",
            "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/microsoft.insights/alertrules/VMMetricAlert1",
            "name": "VMMetricAlert1",
            "description": "A metric alert for the VM Win2012R2",
            "conditionType": "Metric",
            "subscriptionId": "00000000-0000-0000-0000-000000000000",
            "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
            "resourceGroupName": "ContosoVM"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
    },
    "RequestHeader": {
        "Connection": "Keep-Alive",
        "Host": "s1events.azure-automation.net",
        "User-Agent": "azure-insights/0.9",
        "x-ms-request-id": "00000000-0000-0000-0000-000000000000"
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre as novas [alertas experiência (pré-visualização)](monitoring-overview-unified-alerts.md).
* Saiba mais sobre [registar alertas nos alertas do Azure (pré-visualização)](monitor-alerts-unified-log.md).
* Saiba mais sobre [alertas no Azure](monitoring-overview-alerts.md).