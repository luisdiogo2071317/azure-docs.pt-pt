---
title: "Quase em tempo real alertas métricas no Monitor do Azure | Microsoft Docs"
description: "Quase em tempo real métrica alertas permitem-lhe monitorizar métricas de recurso do Azure com uma frequência de 1 min."
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
ms.openlocfilehash: d3e88a98e0ba93a630d131c25ca4dd5cb16f1b1a
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="near-real-time-metric-alerts-preview"></a>Quase em tempo real alertas métricas (pré-visualização)
Monitor do Azure suporta agora um novo tipo de alertas métricas chamado quase em tempo real métrica alertas (pré-visualização). Esta funcionalidade está atualmente em pré-visualização pública.
Estes alertas diferem dos alertas de métricas regulares em algumas formas

- **Melhorado latência** -quase em tempo real métrica alertas podem monitorizar as alterações nos valores métricas logo 1 min.
- **Mais controlo sobre condições métricas** -quase em tempo real métrica alertas, permitir que os utilizadores definir regras de alertas mais rica. Os alertas agora suportam os valores máximo, mínimos, médios e totais das métricas de monitorização.
- **Combinar a monitorização de várias métricas** -quase em tempo real métrica alertas podem monitorizar várias métricas (atualmente duas) com uma única regra. Alerta obtém acionado se ambas as métricas infringir os respetivos limiares para o período de tempo especificado.
- **Sistema de notificação modulares** - quase em tempo real alertas de métrica de utilização [grupos ação](monitoring-action-groups.md). Esta funcionalidade permite aos utilizadores criar ações de forma modular e reutilize-as para várias regras de alertas.

> [!NOTE]
> Quase em tempo real alertas métricas funcionalidade está atualmente em pré-visualização pública. A experiência de utilizador e a funcionalidade está sujeita a alterações.
>

## <a name="what-resources-can-i-create-near-real-time-metric-alerts-for"></a>Os recursos que podem criar quase em tempo real alertas métricas para?
Obter uma lista completa dos tipos de recursos que são suportadas pelo quase em tempo real alertas métricas:

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

## <a name="near-real-time-metric-alerts-on-metrics-with-dimensions"></a>Alertas de métrica em tempo real near nas métricas com dimensões
Quase em tempo real métrica alertas suporta alertas nas métricas com dimensões. As dimensões são uma forma para filtrar a métrica para o nível adequado. Quase em tempo real métrica alertas nas métricas com dimensões são suportadas para os seguintes tipos de recursos

* Microsoft.ApiManagement/service
* Storage/storageaccounts (apenas suportada para contas do storage em regiões E.U.A.)
* Microsoft.Storage/storageAccounts/services (apenas suportada para contas do storage em regiões E.U.A.)


## <a name="create-a-near-real-time-metric-alert"></a>Criar um alerta de métrico Near em tempo real
Atualmente, quase em tempo real métrica alertas só podem ser criados através do portal do Azure. Suporte para configurar quase em tempo real alertas métricas através do PowerShell, a interface de linha de comandos (CLI) e API REST da Azure Monitor estará disponível brevemente.

A experiência de alerta criar para quase em tempo real métrica alerta ter sido movida para o novo **Alerts(Preview)** experiência. Apesar de, os alertas atuais página mostra **alerta adicionar quase em tempo real métrica**, são redirecionados para a nova experiência.

Pode criar um alerta em tempo real métrico near utilizando os passos descritos [aqui](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal).

## <a name="managing-near-real-time-metric-alerts"></a>Gerir quase em tempo real alertas métricas
Assim que tiver criado uma **alerta quase em tempo real métrica**, podem ser gerido utilizando os passos descritos [aqui](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal).

## <a name="payload-schema"></a>Esquema de payload

A operação POST contém a seguinte payload JSON e o esquema para todos os quase em tempo real alertas métricas.

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

* [Saiba mais sobre a nova experiência de alertas (pré-visualização)](monitoring-overview-unified-alerts.md)
* [Saiba mais sobre alertas de registo nos alertas do Azure (pré-visualização)](monitor-alerts-unified-log.md)
* [Saiba mais sobre alertas no Azure](monitoring-overview-alerts.md)