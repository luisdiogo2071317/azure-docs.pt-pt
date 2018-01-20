---
title: Invocar um webhook no alertas de registo de atividade do Azure | Microsoft Docs
description: "Eventos de registo de atividade de rotas para outros serviços para ações personalizadas. Por exemplo enviar SMS, registo de erros ou notificar a equipa através do serviço de mensagens/chat."
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
ms.openlocfilehash: 08467aed4e1601b32598fc42515d9c38b601a9d4
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="call-a-webhook-on-azure-activity-log-alerts"></a>Invocar um webhook no alertas de registo de atividade do Azure
Webhooks permitem-lhe uma notificação de alerta do Azure de rotas para outros sistemas de ações de pós-processamento ou personalizados. Pode utilizar um webhook num alerta para o encaminhar para os serviços que enviar SMS, registo de erros, notificar a equipa através de serviços de chat/mensagens ou fazer qualquer número de outras ações. Este artigo descreve como definir um webhook ser chamado quando um desencadeado de alerta de registo de atividade do Azure. Também mostra que aspeto o payload para o POST de HTTP para um webhook. Para obter informações sobre a configuração e o esquema para um alerta de métrica do Azure, [consulte em vez disso, esta página](insights-webhooks-alerts.md). Também pode configurar um alerta de registo de atividade para enviar correio eletrónico quando ativado.

> [!NOTE]
> Esta funcionalidade está atualmente em pré-visualização e será removida, a determinada altura no futuro.
>
>

Pode configurar um registo de atividade alertas utilizando o [Cmdlets do PowerShell do Azure](insights-powershell-samples.md#create-metric-alerts), [CLI de várias plataformas](insights-cli-samples.md#work-with-alerts), ou [API REST da Azure Monitor](https://msdn.microsoft.com/library/azure/dn933805.aspx). Atualmente, não é possível definir uma cópia de segurança no portal do Azure.

## <a name="authenticating-the-webhook"></a>Autenticar o webhook
O webhook pode autenticar utilizando um dos seguintes métodos:

1. **Autorização baseada em tokens** -o webhook URI é guardado com um ID de token, por exemplo,`https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2. **Autorização básica** -o webhook URI é guardado com um nome de utilizador e palavra-passe, por exemplo,`https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Esquema de payload
A operação POST contém a seguinte payload JSON e o esquema para todos os alertas com base no registo de atividade. Este esquema é semelhante às utilizadas pelo alertas com base em métrica.

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
| status |Utilizado para os alertas de métricas. Sempre definido como "ativado" para os alertas de registo de atividade. |
| context |Contexto do evento. |
| activityLog | As propriedades de registo do evento.|
| Autorização |As propriedades RBAC do evento. Estes incluem, normalmente, "action", "função" e "âmbito." |
| action | Ação capturada pelo alerta. |
| Âmbito | Âmbito do alerta (revertidos recursos).|
| canais | Operação |
| afirmações | Uma coleção de informações é está relacionada com as afirmações. |
| chamador |GUID ou nome de utilizador do utilizador que executou a operação, afirmação UPN ou afirmação SPN com base na disponibilidade. Pode ser nulo para determinadas chamadas de sistema. |
| correlationId |Normalmente, um GUID no formato de cadeia. Eventos com correlationId pertencerem à mesma ação superior e, normalmente, partilham um correlationId. |
| descrição |Descrição do alerta conforme definido durante a criação do alerta. |
| eventSource |Nome do serviço do Azure ou infraestrutura que gerou o evento. |
| eventTimestamp |Hora que do evento ocorreu. |
| eventDataId |Identificador exclusivo para o evento. |
| nível |Um dos seguintes valores: "Críticas", "Error", "Aviso", "Informativa" e "Verboso". |
| operationName |Nome da operação. |
| operationId |Normalmente, um GUID partilhado entre os eventos que correspondem a única operação. |
| resourceId |ID de recurso do recurso afetado. |
| resourceGroupName |Nome do grupo de recursos para o recurso afectado |
| resourceProviderName |O fornecedor de recursos do recurso afetado. |
| status |Cadeia. Estado da operação. Os valores comuns incluem: "Iniciado", "Em curso", "Foi concluída com êxito", "Falha", "Ativo", "Resolvido". |
| subStatus |Normalmente inclui o código de estado HTTP da chamada REST correspondente. Também poderão incluir outras cadeias que descrevem um subestado. Os valores de subestado comuns incluem: OK (código de estado de HTTP: 200), criado (código de estado de HTTP: 201), aceite (código de estado de HTTP: 202), não conteúdo (código de estado HTTP: 204), pedido incorreto (código de estado de HTTP: 400), não encontrado (código de estado de HTTP: 404), conflito (código de estado HTTP: 409), erro de servidor interno (código de estado HTTP: 500), serviço indisponível (código de estado HTTP: 503), tempo limite do Gateway (código de estado HTTP: 504) |
| subscriptionId |ID da subscrição do Azure. |
| submissionTimestamp |Tempo no qual o evento foi gerado pelo serviço do Azure que processou o pedido. |
| resourceType | O tipo de recurso que gerou o evento.|
| propriedades |Conjunto de `<Key, Value>` pares (ou seja, `Dictionary<String, String>`) que inclui detalhes sobre o evento. |

## <a name="next-steps"></a>Passos Seguintes
* [Saiba mais sobre o registo de atividade](monitoring-overview-activity-logs.md)
* [Executar scripts de automatização do Azure (Runbooks) nos alertas do Azure](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Utilize a aplicação lógica para enviar um SMS através do Twilio a partir de um alerta do Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Neste exemplo é para alertas métricas, mas pode ser modificado para trabalhar com um alerta de registo de atividade.
* [Utilize a aplicação lógica para enviar uma mensagem Slack a partir de um alerta do Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Neste exemplo é para alertas métricas, mas pode ser modificado para trabalhar com um alerta de registo de atividade.
* [Utilize a aplicação lógica para enviar uma mensagem para uma fila do Azure a partir de um alerta do Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Neste exemplo é para alertas métricas, mas pode ser modificado para trabalhar com um alerta de registo de atividade.
