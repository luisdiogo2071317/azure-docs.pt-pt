---
title: Criar um alerta de registo com um modelo do Resource Manager
description: Saiba como criar o alerta de registo utilizando um modelo Azure Resource Manager e API.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 6ec07d02e61d50aa588d75ba7337eb9237e11207
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/18/2018
ms.locfileid: "49402902"
---
# <a name="create-a-log-alert-with-a-resource-manager-template"></a>Criar um alerta de registo com um modelo do Resource Manager
Este artigo mostra como pode gerir [alertas de registo](monitor-alerts-unified-log.md) através de programação em escala, no Azure com [modelo Azure Resource Manager](..//azure-resource-manager/resource-group-authoring-templates.md) via [Azure Powershell](../azure-resource-manager/resource-group-template-deploy.md) e [Da CLI do azure](../azure-resource-manager/resource-group-template-deploy-cli.md). Atualmente alertas do Azure suporta alertas de registo em consultas a partir [do Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) e [Azure Application Insights](../application-insights/app-insights-analytics-tour.md).

## <a name="managing-log-alert-on-log-analytics"></a>Gerir o alerta de registo no Log Analytics
Alerta de registo para [do Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) é integrado a [experiência de novos alertas do Azure](monitoring-overview-unified-alerts.md); enquanto ainda é executado fora APIs de análise de registo e permanece compatibile com esquema utilizada anteriormente.

> [!NOTE]
> A partir de 14 de Maio de 2018, começaram a todos os alertas numa área de trabalho estender automaticamente para o Azure. Para obter mais informações, consulte [expandir alertas para o Azure](monitoring-alerts-extend.md). 

### <a name="using-azure-resource-manager-template"></a>Usando o modelo Azure Resource Manager
Alertas de registo para o Log Analytics são criadas por regras de alerta que executam uma procura guardada num intervalo regular. Se os resultados da correspondência de consulta especificados critérios, é criado um registo de alerta e uma ou mais ações são executadas. 

Modelo de recursos de pesquisa do Log analytics guardada e Log analytics alertsare disponível na seção do Log Analytics da documentação. Para saber mais, veja [adicionando o Log Analytics guardar pesquisas e alertas](../operations-management-suite/operations-management-suite-solutions-resources-searches-alerts.md); que inclui exemplos ilustrativos, bem como detalhes de esquema.

### <a name="using-resource-template-via-apipowershell"></a>Com o modelo de recursos através do API/Powershell
A API de REST alerta do Log Analytics é RESTful e podem ser acessada através da API de REST do Azure Resource Manager. A API, portanto, pode ser acessada a partir de uma linha de comando do PowerShell e irá enviar os resultados da pesquisa para si no formato JSON, permitindo que use os resultados de várias formas diferentes através de programação.

Saiba mais sobre [criar e gerir regras de alerta no Log Analytics com a REST API](../log-analytics/log-analytics-api-alerts.md), incluindo exemplos de aceder à API a partir do Powershell.

## <a name="managing-log-alert-on-application-insights"></a>Gerir o alerta de registo no Application Insights
Alertas de registo para o Azure Application Insights foram introduzidas como parte dos novos alertas do Azure em Azure Monitor. Por isso ele seja executado na API do Azure Monitor como [regras de consulta agendada](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) grupo de operação de REST.

### <a name="using-azure-resource-manager-template"></a>Usando o modelo Azure Resource Manager
Alerta de registo de recursos do Application Insights tem um tipo de `Microsoft.Insights/scheduledQueryRules/`. Para obter mais informações sobre este tipo de recurso, consulte [do Azure Monitor - referência da API de regras de consulta agendada](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/).

Segue-se a estrutura para [criação de regras de consulta agendada](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate) com base em modelo de recursos, com o conjunto de dados de exemplo como variáveis.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0", 
    "parameters": {      
    },   
    "variables": {
    "alertLocation": "southcentralus",
    "alertName": "samplelogalert",
    "alertTag": "hidden-link:/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
    "alertDesription": "Sample log search alert",
    "alertStatus": "true",
    "alertSource":{
        "Query":"requests",
        "SourceId": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
        "Type":"ResultCount"
         },
     "alertSchedule":{
         "Frequency": 15,
         "Time": 60
         },
     "alertActions":{
         "SeverityLevel": "4"
         },
      "alertTrigger":{
        "Operator":"GreaterThan",
        "Threshold":"1"
         },
       "actionGrp":{
        "ActionGroup": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/actiongroups/sampleAG",
        "Subject": "Customized Email Header",
        "Webhook": "{ \"alertname\":\"#alertrulename\", \"IncludeSearchResults\":true }"           
         }
  },
  "resources":[ {
    "name":"[variables('alertName')]",
    "type":"Microsoft.Insights/scheduledQueryRules",
    "apiVersion": "2018-04-16",
    "location": "[variables('alertLocation')]",
    "tags":{"[variables('alertTag')]": "Resource"},
    "properties":{
       "description": "[variables('alertDesription')]",
       "enabled": "[variables('alertStatus')]",
       "source": {
           "query": "[variables('alertSource').Query]",
           "dataSourceId": "[variables('alertSource').SourceId]",
           "queryType":"[variables('alertSource').Type]"
       },
      "schedule":{
           "frequencyInMinutes": "[variables('alertSchedule').Frequency]",
           "timeWindowInMinutes": "[variables('alertSchedule').Time]"    
       },
      "action":{
           "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.AlertingAction",
           "severity":"[variables('alertActions').SeverityLevel]",
           "aznsAction":{
               "actionGroup":"[array(variables('actionGrp').ActionGroup)]",
               "emailSubject":"[variables('actionGrp').Subject]",
               "customWebhookPayload":"[variables('actionGrp').Webhook]"
           },
       "trigger":{
               "thresholdOperator":"[variables('alertTrigger').Operator]",
               "threshold":"[variables('alertTrigger').Threshold]"
           }
       }
     }
   }
 ]
}
```
> [!IMPORTANT]
> Campo de etiqueta com ligação de oculto para o recurso de destino é obrigatório na utilização de [regras de consulta agendada ](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) modelo de chamada ou recurso de API. 

O json de exemplo acima podem ser salvas como (Digamos) sampleScheduledQueryRule.json com o objetivo deste passo a passo e podem ser implementado com [Gestor de recursos do Azure no portal do Azure](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).

### <a name="using-resource-template-via-clipowershell"></a>Com o modelo de recursos através do CLI/Powershell
O Azure Monitor - API de regras de consulta agendada é uma REST API e totalmente compatível com a API de REST do Azure Resource Manager. Por conseguinte, pode ser utilizada através do Powershell com o cmdlet do Gestor de recursos, bem como a CLI do Azure.

Ilustrado abaixo utilização através do cmdlet do PowerShell do Azure Resource Manager para o exemplo de modelo do Resource mostrado anteriormente (sampleScheduledQueryRule.json):
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile "D:\Azure\Templates\sampleScheduledQueryRule.json"
```
Ilustrado abaixo utilização através do comando do Azure Resource Manager na CLI do Azure para o exemplo de modelo do Resource mostrado anteriormente (sampleScheduledQueryRule.json):

```azurecli
az group deployment create --resource-group myRG --template-file sampleScheduledQueryRule.json
```
Numa operação concluída com êxito, será devolvido 201 à criação de regra de alerta do estado novo ou 200 vai ser devolvido se uma regra de alerta existente foi modificada.


## <a name="next-steps"></a>Passos Seguintes
* Compreender [ações de Webhook para alertas de registo](monitor-alerts-unified-log-webhook.md)
* Saiba mais sobre o novo [alertas do Azure](monitoring-overview-unified-alerts.md)
* Saiba mais sobre [Application Insights](../application-insights/app-insights-analytics.md)
* Saiba mais sobre [do Log Analytics](../log-analytics/log-analytics-overview.md).   
