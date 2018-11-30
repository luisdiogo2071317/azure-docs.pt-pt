---
title: Descrição geral dos registos de Firewall do Azure
description: Este artigo é uma visão geral dos registos de diagnóstico do Firewall do Azure.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: 0698f1dbc491781089ef94eec32f2a427fd3cca4
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422393"
---
# <a name="azure-firewall-logs"></a>Registos de Firewall do Azure

Pode monitorizar os registos do Azure Firewall com registos de firewall. Também pode utilizar os registos de atividades para auditar operações nos recursos do Azure Firewall.

Pode aceder a alguns destes registos através do portal. Os registos podem ser enviados para o [Log Analytics](../azure-monitor/insights/azure-networking-analytics.md), o Armazenamento e os Hubs de Eventos, e analisados no Log Analytics ou através de ferramentas diferentes, como o Excel e o Power BI.

## <a name="diagnostic-logs"></a>Registos de diagnósticos

 Os seguintes registos de diagnóstico estão disponíveis para o Azure Firewall:

* **Registo de regra de Aplicação**

   O registo de regra de Aplicação é guardado numa conta de armazenamento, transmitido em fluxo aos Hubs de eventos e/ou enviado para o Log Analytics, apenas se o tiver ativado para cada Azure Firewall. Cada nova ligação que corresponde a uma das suas regras de aplicação configuradas resulta num registo da ligação aceite/negada. Os dados são registados no formato JSON, conforme mostrado no exemplo seguinte:

   ```
   Category: application rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
   {
    "category": "AzureFirewallApplicationRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallApplicationRuleLog",
    "properties": {
        "msg": "HTTPS request from 10.1.0.5:55640 to mydestination.com:443. Action: Allow. Rule Collection: collection1000. Rule: rule1002"
    }
   }
   ```

* **Registo de regra de Rede**

   O registo de regra de Rede é guardado numa conta de armazenamento, transmitido em fluxo aos Hubs de eventos e/ou enviado para o Log Analytics, apenas se o tiver ativado para cada Azure Firewall. Cada nova ligação que corresponde a uma das suas regras de rede configuradas resulta num registo da ligação aceite/negada. Os dados são registados no formato JSON, conforme mostrado no exemplo seguinte:

   ```
   Category: network rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
  {
    "category": "AzureFirewallNetworkRule",
    "time": "2018-06-14T23:44:11.0590400Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallNetworkRuleLog",
    "properties": {
        "msg": "TCP request from 111.35.136.173:12518 to 13.78.143.217:2323. Action: Deny"
    }
   }

   ```

Tem três opções para armazenar os registos:

* **Conta de armazenamento**: as contas de armazenamento são ideais para os registos quando estes são armazenados durante um período mais longo e revistos quando necessário.
* **Hubs de eventos**: os Hubs de eventos são uma excelente opção para integrar com outras informações de segurança e ferramentas de gestão de eventos (SEIM) para obter alertas sobre os seus recursos.
* **Log Analytics**: o Log Analytics é ideal para monitorização geral em tempo real da sua aplicação ou para observar tendências.

## <a name="activity-logs"></a>Registos de atividade

   As entradas de registos de atividades são recolhidas por predefinição e pode visualizá-las no portal do Azure.

   Pode utilizar os [registos de atividades do Azure](../azure-resource-manager/resource-group-audit.md) (anteriormente conhecidos como registos operacionais e registos de auditoria) para ver todas as operações que são submetidas para a sua subscrição do Azure.


## <a name="next-steps"></a>Passos Seguintes

Para saber como monitorizar as métricas e registos de Firewall do Azure, veja [Tutorial: registos de Firewall de Azure Monitor](tutorial-diagnostics.md).