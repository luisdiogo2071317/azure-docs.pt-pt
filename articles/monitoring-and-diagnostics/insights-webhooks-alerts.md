---
title: "Configurar webhooks em alertas métricas do Azure | Microsoft Docs"
description: "Saiba como reroute alertas do Azure para sistemas de outros, não do Azure."
author: johnkemnetz
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 8b3ae540-1d19-4f3d-a635-376042f8a5bb
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: johnkem
ms.openlocfilehash: 049803e7701c68559103d9b1fa5dfacf820d0548
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2018
---
# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Configurar um webhook num alerta métrico do Azure
Pode utilizar webhooks para encaminhar uma notificação de alerta do Azure para outros sistemas de ações de pós-processamento ou personalizados. Pode utilizar um webhook num alerta para o encaminhar para os serviços que enviam mensagens de SMS, para o registo de erros, notificar a equipa através de chat ou serviços de mensagens ou para várias outras ações. 

Este artigo descreve como definir um webhook num alerta métrico do Azure. Mostra também aspeto o payload para o POST de HTTP para um webhook. Para obter informações sobre a configuração e o esquema de uma atividade do Azure alerta de registo (alerta em eventos), consulte [invocar um webhook num alerta de registo de atividade do Azure](insights-auditlog-to-webhook-email.md).

Alertas do Azure utilizam HTTP POST para enviar o conteúdo de alerta no formato JSON para um webhook URI que fornecem ao criar o alerta. O esquema está definido neste artigo. O URI tem de ser um ponto final HTTP ou HTTPS válido. Azure mensagens uma entrada por pedido, quando um alerta é ativado.

## <a name="configure-webhooks-via-the-azure-portal"></a>Configurar webhooks através do portal do Azure
Para adicionar ou atualizar o webhook URI, no [portal do Azure](https://portal.azure.com/), aceda a **Create/Update alertas**.

![Adicionar um painel de regra de alerta](./media/insights-webhooks-alerts/Alertwebhook.png)

Também pode configurar um alerta para publicar um webhook URI utilizando [cmdlets Azure PowerShell](insights-powershell-samples.md#create-metric-alerts), um [CLI de várias plataformas](insights-cli-samples.md#work-with-alerts), ou [as APIs REST da Azure Monitor](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticate-the-webhook"></a>Autenticar o webhook
O webhook pode autenticar através da utilização de autorização baseada em tokens. Guardar o webhook URI está com um ID de token. Por exemplo: `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>Esquema de payload
A operação POST contém as seguinte payload JSON e o esquema para todos os alertas com base em métrica:

```JSON
{
    "WebhookName": "Alert1515515157799",
    "RequestBody": {
        "status": "Activated",
        "context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                "metricName": "Requests",
                "metricUnit": "Count",
                "metricValue": "10",
                "threshold": "10",
                "windowSize": "15",
                "timeAggregation": "Average",
                "operator": "GreaterThanOrEqual"
            },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
        },
        "properties": {
            "key1": "value1",
            "key2": "value2"
        }
    }
}
```


| Campo | Obrigatório | Fixo conjunto de valores | Notas |
|:--- |:--- |:--- |:--- |
| status |S |Ativado, resolvido |O estado para o alerta com base nas condições definido. |
| context |S | |O contexto do alerta. |
| carimbo de data/hora |S | |A hora em que o alerta foi acionado. |
| ID |S | |Cada regra de alerta tem um ID exclusivo. |
| nome |S | |O nome do alerta. |
| descrição |S | |Uma descrição do alerta. |
| conditionType |S |Métrica do evento |São suportados dois tipos de alertas: métricas e eventos. Alertas métricas baseiam-se numa condição de métrica. Alertas de eventos são baseadas num evento no registo de atividade. Utilize este valor para verificar se o alerta é baseado numa métrica ou um evento. |
| condição |S | |Os campos específicos para verificar se baseiam o **conditionType** valor. |
| metricName |Existência de alertas métricas | |O nome da métrica que define o que monitoriza a regra. |
| metricUnit |Existência de alertas métricas |Bytes, BytesPerSecond, Count, CountPerSecond, Percent, Seconds |A unidade permitida na métrica. Consulte [valores permitidos](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx). |
| metricValue |Existência de alertas métricas | |O valor real da métrica que causou o alerta. |
| Limiar |Existência de alertas métricas | |O valor de limiar em que o alerta está ativado. |
| windowSize |Existência de alertas métricas | |O período de tempo que é utilizado para monitorizar a atividade de alerta com base no limiar. O valor tem de estar entre 5 minutos e 1 dia. O valor deve estar no formato de duração ISO 8601. |
| timeAggregation |Existência de alertas métricas |Média, Last, máximo, mínimo, None, Total |Como devem ser combinados os dados que são recolhidos ao longo do tempo. O valor predefinido é média. Consulte [valores permitidos](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx). |
| operador |Existência de alertas métricas | |O operador que é utilizado para comparar os dados métricos atuais para o limiar definido. |
| subscriptionId |S | |O ID de subscrição do Azure. |
| resourceGroupName |S | |O nome do grupo de recursos para o recurso afetado. |
| resourceName |S | |O nome de recurso do recurso afetado. |
| resourceType |S | |O tipo de recurso do recurso afetado. |
| resourceId |S | |O ID de recurso do recurso afetado. |
| resourceRegion |S | |A região ou a localização do recurso afetado. |
| portalLink |S | |Uma ligação direta para a página de resumo do portal de recursos. |
| propriedades |N |Opcional |Um conjunto de pares chave/valor que contém os detalhes sobre o evento. Por exemplo, `Dictionary<String, String>`. O campo de propriedades é opcional. Uma IU personalizada ou um fluxo de trabalho de baseado na aplicação lógica, os utilizadores podem introduzir pares chave/valor que podem ser transmitidas através do payload. Uma maneira alternativa para passar as propriedades personalizadas para o webhook é através de webhook URI propriamente dito (como parâmetros de consulta). |

> [!NOTE]
> Pode definir o **propriedades** campo utilizando apenas [as APIs REST da Azure Monitor](https://msdn.microsoft.com/library/azure/dn933805.aspx).
>
>

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre alertas do Azure e webhooks as vídeo [alertas de integrar o Azure com PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080).
* Saiba como [executar scripts de automatização do Azure (runbooks) nos alertas do Azure](http://go.microsoft.com/fwlink/?LinkId=627081).
* Saiba como [utilizar uma aplicação lógica para enviar uma mensagem SMS através do Twilio a partir de um alerta do Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app).
* Saiba como [utilizar uma aplicação lógica para enviar uma mensagem Slack a partir de um alerta do Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app).
* Saiba como [utilizar uma aplicação lógica para enviar uma mensagem para uma fila do Azure a partir de um alerta do Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app).
