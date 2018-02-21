---
title: Invocar um webhook num alerta de registo de atividade do Azure | Microsoft Docs
description: "Saiba como encaminhar eventos de registo de atividade para outros serviços para ações personalizadas. Por exemplo, pode enviar mensagens SMS, registo de erros ou notificar a equipa através de um chat ou o serviço de mensagens."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 64d333d1-7f37-4a00-9d16-dda6e69a113b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: johnkem
ms.openlocfilehash: 9872c30d123f0a7443e28dc58ee0d4e16572a390
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2018
---
# <a name="call-a-webhook-on-an-azure-activity-log-alert"></a>Invocar um webhook num alerta de registo de atividade do Azure
Pode utilizar webhooks para encaminhar uma notificação de alerta do Azure para outros sistemas de pós-processamento ou de ações personalizadas. Pode utilizar um webhook num alerta para o encaminhar para os serviços que enviam mensagens de SMS, para o registo de erros, notificar a equipa através de chat ou serviços de mensagens ou para várias outras ações. Também pode configurar um alerta de registo de atividade para enviar correio eletrónico quando um alerta é ativado.

Este artigo descreve como definir um webhook ser chamado quando um desencadeado de alerta de registo de atividade do Azure. Também mostra que aspeto o payload para o POST de HTTP para um webhook. Para obter informações sobre a configuração e o esquema de um alerta de métrica do Azure, consulte [configurar um webhook num alerta métrico Azure](insights-webhooks-alerts.md). 

> [!NOTE]
> Atualmente, a funcionalidade que suporta a chamada de um webhook num alerta de registo de atividade do Azure está em pré-visualização.
>
>

Pode configurar um alerta de registo de atividade, utilizando [cmdlets Azure PowerShell](insights-powershell-samples.md#create-metric-alerts), um [CLI de várias plataformas](insights-cli-samples.md#work-with-alerts), ou [as APIs REST da Azure Monitor](https://msdn.microsoft.com/library/azure/dn933805.aspx). Atualmente, não é possível utilizar o portal do Azure para configurar um alerta de registo de atividade.

## <a name="authenticate-the-webhook"></a>Autenticar o webhook
O webhook pode autenticar utilizando um dos seguintes métodos:

* **Autorização baseada em tokens**. Guardar o webhook URI está com um ID de token. Por exemplo: `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
* **Autorização básica**. O webhook URI é guardado com um nome de utilizador e palavra-passe. Por exemplo: `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Esquema de payload
A operação POST contém a seguinte payload JSON e o esquema para todos os alertas com base no registo de atividade. Este esquema é semelhante à que utilizou para alertas com base em métrica.

```json
{
    "WebhookName": "Alert1515526229589",
    "RequestBody": {
        "schemaId": "Microsoft.Insights/activityLogs",
        "data": {
            "status": "Activated",
            "context": {
                "activityLog": {
                    "authorization": {
                        "action": "Microsoft.Compute/virtualMachines/deallocate/action",
                        "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/Microsoft.Compute/virtualMachines/ContosoVM1"
                    },
                    "channels": "Operation",
                    "claims": {
                        "aud": "https://management.core.windows.net/",
                        "iss": "https://sts.windows.net/00000000-0000-0000-0000-000000000000/",
                        "iat": "1234567890",
                        "nbf": "1234567890",
                        "exp": "1234567890",
                        "aio": "Y2NgYBD8ZLlhu27JU6WZsXemMIvVAAA=",
                        "appid": "00000000-0000-0000-0000-000000000000",
                        "appidacr": "2",
                        "e_exp": "262800",
                        "http://schemas.microsoft.com/identity/claims/identityprovider": "https://sts.windows.net/00000000-0000-0000-0000-000000000000/",
                        "http://schemas.microsoft.com/identity/claims/objectidentifier": "00000000-0000-0000-0000-000000000000",
                        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "00000000-0000-0000-0000-000000000000",
                        "http://schemas.microsoft.com/identity/claims/tenantid": "00000000-0000-0000-0000-000000000000",
                        "uti": "XnCk46TrDkOQXwo49Y8fAA",
                        "ver": "1.0"
                    },
                    "caller": "00000000-0000-0000-0000-000000000000",
                    "correlationId": "00000000-0000-0000-0000-000000000000",
                    "description": "",
                    "eventSource": "Administrative",
                    "eventTimestamp": "2018-01-09T20:11:25.8410967+00:00",
                    "eventDataId": "00000000-0000-0000-0000-000000000000",
                    "level": "Informational",
                    "operationName": "Microsoft.Compute/virtualMachines/deallocate/action",
                    "operationId": "00000000-0000-0000-0000-000000000000",
                    "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
                    "resourceGroupName": "ContosoVM",
                    "resourceProviderName": "Microsoft.Compute",
                    "status": "Succeeded",
                    "subStatus": "",
                    "subscriptionId": "00000000-0000-0000-0000-000000000000",
                    "submissionTimestamp": "2018-01-09T20:11:40.2986126+00:00",
                    "resourceType": "Microsoft.Compute/virtualMachines"
                }
            },
            "properties": {}
        }
    },
    "RequestHeader": {
        "Expect": "100-continue",
        "Host": "s1events.azure-automation.net",
        "User-Agent": "IcMBroadcaster/1.0",
        "X-CorrelationContext": "RkkKACgAAAACAAAAEADlBbM7x86VTrHdQ2JlmlxoAQAQALwazYvJ/INPskb8S5QzgDk=",
        "x-ms-request-id": "00000000-0000-0000-0000-000000000000"
    }
}
```

| Nome do elemento | Descrição |
| --- | --- |
| status |Utilizado para os alertas de métricas. Existência de alertas de registo de atividade, sempre definido como ativado.|
| context |O contexto do evento. |
| activityLog | As propriedades de registo do evento.|
| Autorização |As propriedades de controlo de acesso baseado em funções (RBAC) do evento. Estas propriedades incluem, normalmente, **ação**, **função**, e **âmbito**. |
| action | A ação capturada pelo alerta. |
| Âmbito | O âmbito do alerta (ou seja, o recurso).|
| canais | A operação. |
| afirmações | Uma coleção de informações que está relacionada com as afirmações. |
| chamador |O GUID ou nome de utilizador do utilizador que executou a operação, a afirmação UPN ou a afirmação SPN com base na disponibilidade. Pode ser um valor nulo para determinadas chamadas de sistema. |
| correlationId |Normalmente, um GUID no formato de cadeia. Os eventos com **correlationId** pertencer à mesma ação de maior. Normalmente, têm o mesmo **correlationId** valor. |
| descrição |A descrição do alerta que foi definida quando o alerta foi criado. |
| eventSource |O nome do serviço do Azure ou infraestrutura que gerou o evento. |
| eventTimestamp |A hora em que o evento ocorreu. |
| eventDataId |O identificador exclusivo do evento. |
| nível |Um dos seguintes valores: crítico, erro, aviso, informativo ou verboso. |
| operationName |O nome da operação. |
| operationId |Normalmente, um GUID que é partilhado entre os eventos. O GUID, normalmente, corresponde a uma única operação. |
| resourceId |O ID de recurso do recurso afetado. |
| resourceGroupName |O nome do grupo de recursos para o recurso afetado. |
| resourceProviderName |O fornecedor de recursos do recurso afetado. |
| status |Um valor de cadeia que indica o estado da operação. Os valores comuns incluem iniciado, em curso, com êxito, falha, Active Directory e resolvido. |
| subStatus |Normalmente inclui o código de estado HTTP da chamada REST correspondente. Também poderão incluir outras cadeias que descrevem um subestado. Valores de subestado comuns incluem OK (código de estado de HTTP: 200), criado (código de estado de HTTP: 201), aceite (código de estado de HTTP: 202), não conteúdo (código de estado de HTTP: 204), pedido incorreto (código de estado de HTTP: 400), não foi encontrado (código de estado HTTP: 404), conflito (código de estado de HTTP: 409), erro interno do servidor (código de estado de HTTP: 500), serviço indisponível (código de estado HTTP: 503) e tempo limite do Gateway (código de estado HTTP : 504). |
| subscriptionId |O ID de subscrição do Azure. |
| submissionTimestamp |A hora em que o evento foi gerado pelo serviço do Azure que processou o pedido. |
| resourceType | O tipo de recurso que gerou o evento.|
| propriedades |Um conjunto de pares chave/valor que contém os detalhes sobre o evento. Por exemplo, `Dictionary<String, String>`. |

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre o [registo de atividade](monitoring-overview-activity-logs.md).
* Saiba como [executar scripts de automatização do Azure (runbooks) nos alertas do Azure](http://go.microsoft.com/fwlink/?LinkId=627081).
* Saiba como [utilizar uma aplicação lógica para enviar uma mensagem SMS através do Twilio a partir de um alerta do Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Neste exemplo é para alertas métricas, mas pode modificá-la para trabalhar com um alerta de registo de atividade.
* Saiba como [utilizar uma aplicação lógica para enviar uma mensagem Slack a partir de um alerta do Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Neste exemplo é para alertas métricas, mas pode modificá-la para trabalhar com um alerta de registo de atividade.
* Saiba como [utilizar uma aplicação lógica para enviar uma mensagem para uma fila do Azure a partir de um alerta do Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Neste exemplo é para alertas métricas, mas pode modificá-la para trabalhar com um alerta de registo de atividade.
