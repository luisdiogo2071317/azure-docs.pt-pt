---
title: Criar um alerta de registo com um modelo do Resource Manager | Microsoft Docs
description: Saiba como criar o alerta de registo utilizando um modelo Azure Resource Manager e a API.
author: msvijayn
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
ms.date: 05/01/2018
ms.author: vinagara
ms.openlocfilehash: 4148c6d3f9d4407681159870b858c629c0b5d878
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="create-a-log-alert-with-a-resource-manager-template"></a>Criar um alerta de registo com um modelo do Resource Manager
Este artigo mostra como pode gerir [registar alertas](monitor-alerts-unified-log.md) através de programação à escala, no Azure com [modelo Azure Resource Manager](..//azure-resource-manager/resource-group-authoring-templates.md) através de [Azure Powershell](../azure-resource-manager/resource-group-template-deploy.md) e [CLI do azure](../azure-resource-manager/resource-group-template-deploy-cli.md). Atualmente alertas do Azure, suporta registar alertas em consultas de [Log Analytics do Azure](../log-analytics/log-analytics-tutorial-viewdata.md) e [Azure Application Insights](../application-insights/app-insights-analytics-tour.md).

## <a name="managing-log-alert-on-log-analytics"></a>Alerta de registo na análise de registos de gestão
Alerta de registo para [Log Analytics do Azure](../log-analytics/log-analytics-tutorial-viewdata.md) está integrado o [novos alertas do Azure experiência](monitoring-overview-unified-alerts.md); enquanto ainda executa desativar APIs de análise do registo e permanece compatibilidade com o esquema utilizada anteriormente para gerir [alertas no portal do OMS](..//log-analytics/log-analytics-alerts-creating.md).

> [!NOTE]
> A partir de 14 de Maio de 2018, todos os alertas na área de trabalho serão iniciada automaticamente expandir no Azure. Um utilizador pode iniciar voluntariamente expandir alertas para o Azure antes de 14 de Maio de 2018. Para obter mais informações, consulte [expandir alertas no Azure da OMS](monitoring-alerts-extend.md). 

### <a name="using-azure-resource-manager-template"></a>Utilizar o modelo Azure Resource Manager
Alertas de registo para análise de registos são criadas pelas regras de alerta que executam uma procura guardada num intervalo regular. Se os resultados de correspondência de consulta aos critérios especificados, é criado um registo de alerta e uma ou mais ações são executadas. 

Modelo de recursos para [pesquisa guardada de análise do registo](../log-analytics/log-analytics-log-searches.md) e [registar alertas de análise](../log-analytics/log-analytics-alerts.md) estão disponíveis na secção de análise de registos de documentação. Saiba mais sobre, [adicionar Log Analytics guardar pesquisas e os alertas](../operations-management-suite/operations-management-suite-solutions-resources-searches-alerts.md); que inclui exemplos ilustrativa, bem como detalhes de esquema.

### <a name="using-resource-template-via-apipowershell"></a>Utilizando o modelo de recursos através do API/Powershell
A API de REST de alerta de análise do registo é RESTful e pode ser acedida através da API de REST do Azure Resource Manager. A API, por conseguinte, pode ser acedida a partir de uma linha de comandos do PowerShell e irá enviar os resultados da pesquisa para o utilizador no formato JSON, permitindo-lhe utilizar os resultados de várias maneiras diferentes através de programação.

Saiba mais sobre [criar e gerir regras de alertas na análise de registos com a REST API](../log-analytics/log-analytics-api-alerts.md), incluindo exemplos de aceder à API a partir do Powershell.

## <a name="managing-log-alert-on-application-insights"></a>Gerir o alerta de registo no Application Insights
Alertas de registo para o Azure Application Insights foram introduzidas como parte dos novos alertas do Azure no Monitor do Azure. Por conseguinte, é executada em API de Monitor do Azure como [regras de consulta agendado](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/) grupo de operação REST.

### <a name="using-azure-resource-manager-template"></a>Utilizar o modelo Azure Resource Manager
Alerta de registo de recursos do Application Insights tem um tipo de `Microsoft.Insights/scheduledQueryRules/`. Para obter mais informações sobre este tipo de recurso, consulte [Azure Monitor - referência da API de regras de consulta agendado](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/).

Segue-se a estrutura de [criação de regras de consulta agendado](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/createorupdate) baseada em modelo de recursos, com o conjunto de dados de exemplo como variáveis.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",    
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
        "SeverityLevel": "4",
        },
     "alertTrigger":{
       "Operator":"GreaterThan",
       "Threshold":"1"
        },
      "actionGrp":{
       "ActionGroup": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/actiongroups/sampleAG",
      "Subject": "Customized Email Header",
      "Webhook": "{}"           
        }
  },
  "resources":[ {
     "name":"[variables('alertName')]",
     "type":"Microsoft.Insights/scheduledQueryRules",
     "apiVersion": "2018-04-16",
     "location": "[variables('alertLocation')]",
     "tags":{"hidden-link:[variables('alertTargetResource')]": "Resource"},
     "properties":{
        "description": "[variables('alertDescription')]",
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
> Campo de etiqueta com oculto ligação ao recurso de destino é obrigatório na utilização de [regras de consulta agendado ](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/) modelo de recurso ou chamada de API. 

O json de exemplo acima, podem ser guardado como (diga) sampleScheduledQueryRule.json para efeitos desta explicação passo a passo e pode ser implementado com [Gestor de recursos do Azure no portal do Azure](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).

### <a name="using-resource-template-via-clipowershell"></a>Utilizando o modelo de recursos através do CLI/Powershell
Monitor do Azure - API de regras de consulta agendada é uma REST API e totalmente compatível com a API de REST do Azure Resource Manager. Por conseguinte, pode ser utilizada através do Powershell utilizando o cmdlet do Gestor de recursos, bem como a CLI do Azure.

Ilustrado abaixo utilização através do cmdlet do PowerShell do Azure Resource Manager para o exemplo de modelo do Resource apresentada anteriormente (sampleScheduledQueryRule.json):
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile "D:\Azure\Templates\sampleScheduledQueryRule.json"
```
Ilustrado abaixo utilização através de comandos do Azure Resource Manager na CLI do Azure para o exemplo de modelo do Resource apresentada anteriormente (sampleScheduledQueryRule.json):

```azurecli
az group deployment create --resource-group myRG --template-file sampleScheduledQueryRule.json
```
Operação concluída com êxito, 201 vai ser devolvido para criação de regra de alerta do estado novo ou 200 vai ser devolvido se uma regra de alerta existente foi modificada.


## <a name="next-steps"></a>Passos Seguintes
* Compreender [ações de Webhook para alertas de registo](monitor-alerts-unified-log-webhook.md)
* Saiba mais sobre a nova [alertas do Azure](monitoring-overview-unified-alerts.md)
* Saiba mais sobre [Application Insights](../application-insights/app-insights-analytics.md)
* Saiba mais sobre [Log Analytics](../log-analytics/log-analytics-overview.md).   