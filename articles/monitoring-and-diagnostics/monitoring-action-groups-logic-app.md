---
title: Como a acionar ações complexas com alertas de monitorização do Azure e os grupos de ação
description: Saiba como criar uma ação de aplicação lógica para processar os alertas de monitorização do Azure.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/30/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: 14e562234152d2f1f2f2d2b57b34cd5724d3dd14
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753098"
---
# <a name="create-a-logic-app-action"></a>Criar uma ação de aplicação lógica

Este artigo mostra como configurar e acionar uma aplicação lógica para criar uma conversação no Microsoft Teams quando um alerta é acionado.

## <a name="overview"></a>Descrição geral
Quando é acionado um alerta de Monitor do Azure, aquele invoca um [grupo ação](monitoring-action-groups.md). Grupos de ação permitem-lhe acionar uma ou mais ações para notificar os utilizadores sobre um alerta e também resolvê-lo.

O processo geral é:

-   Crie a aplicação lógica para o respetivo tipo de alerta.

-   Importe o esquema para o respetivo tipo de alerta para a aplicação lógica.

-   Defina o comportamento da aplicação lógica.

-   Copie o ponto final de HTTP da aplicação lógica para um grupo de ação do Azure.

O processo é semelhante, se pretender que a aplicação lógica para efetuar uma ação diferente.

## <a name="create-an-activity-log-alert-administrative"></a>Criar um alerta de registo de atividade: administrativas

1.  No portal do Azure, selecione **crie um recurso** no canto superior esquerdo.

2.  Procure e selecione **aplicação lógica**, em seguida, selecione **criar**.

3.  Dê a sua aplicação lógica um **nome**, escolha um **grupo de recursos**, e assim sucessivamente.

    ![Criar uma aplicação lógica](media/monitoring-action-groups/create-logic-app-dialog.png "criar uma aplicação lógica")

4.  Selecione **criar** para criar a aplicação lógica. Uma mensagem de pop-up indica que a aplicação lógica é criada. Selecione **iniciar recursos** para abrir o **Designer de aplicações lógicas**.

5.  Selecione o acionador: **quando HTTP de pedido é recebido**.

    ![Acionadores da aplicação lógica](media/monitoring-action-groups/logic-app-triggers.png "acionadores da aplicação lógica")

6.  Selecione **editar** para alterar o acionador de pedido HTTP.

    ![Acionadores de pedido HTTP](media/monitoring-action-groups/http-request-trigger-shape.png "acionadores de pedido HTTP")

7.  Selecione **Utilizar o payload de exemplo para gerar esquema**.

    ![Utilize um payload de exemplo](media/monitoring-action-groups/use-sample-payload-button.png "utilizar um payload de exemplo")

8.  Copie e cole o esquema de exemplo seguinte na caixa de diálogo:

    ```json
        {
            "schemaId": "Microsoft.Insights/activityLogs",
            "data": {
                "status": "Activated",
                "context": {
                "activityLog": {
                    "authorization": {
                    "action": "microsoft.insights/activityLogAlerts/write",
                    "scope": "/subscriptions/…"
                    },
                    "channels": "Operation",
                    "claims": "…",
                    "caller": "logicappdemo@contoso.com",
                    "correlationId": "91ad2bac-1afa-4932-a2ce-2f8efd6765a3",
                    "description": "",
                    "eventSource": "Administrative",
                    "eventTimestamp": "2018-04-03T22:33:11.762469+00:00",
                    "eventDataId": "ec74c4a2-d7ae-48c3-a4d0-2684a1611ca0",
                    "level": "Informational",
                    "operationName": "microsoft.insights/activityLogAlerts/write",
                    "operationId": "61f59fc8-1442-4c74-9f5f-937392a9723c",
                    "resourceId": "/subscriptions/…",
                    "resourceGroupName": "LOGICAPP-DEMO",
                    "resourceProviderName": "microsoft.insights",
                    "status": "Succeeded",
                    "subStatus": "",
                    "subscriptionId": "…",
                    "submissionTimestamp": "2018-04-03T22:33:36.1068742+00:00",
                    "resourceType": "microsoft.insights/activityLogAlerts"
                }
                },
                "properties": {}
            }
        }
    ```

9. O **Designer de aplicação lógica** apresenta uma janela de pop-up para relembrá-o de que o pedido enviado para a aplicação lógica tem de definir o **Content-Type** cabeçalho para **application/json**. Feche a janela de pop-up. O alerta de Monitor de Azure define o cabeçalho.

    ![Defina o cabeçalho Content-Type](media/monitoring-action-groups/content-type-header.png "defina o cabeçalho de tipo de conteúdo")

10. Selecione **+** **novo passo** e, em seguida, escolha **adicionar uma ação**.

    ![Adicionar uma ação](media/monitoring-action-groups/add-action.png "adicionar uma ação")

11. Procure e seleccione o conector do Microsoft Teams. Escolha o **Teams da Microsoft – mensagem Post** ação.

    ![Ações de Microsoft Teams](media/monitoring-action-groups/microsoft-teams-actions.png "ações Teams da Microsoft")

12. Configure a ação de Teams da Microsoft. O **Designer de aplicações lógicas** pede-lhe para autenticar a sua conta Office 365. Escolha o **equipa Id** e **Id do canal** para enviar a mensagem.

13. Configurar a mensagem utilizando uma combinação de texto estático e as referências a \<campos\> no conteúdo dinâmico. Copie e cole o seguinte texto para o **mensagem** campo:

    ```text
      Activity Log Alert: <eventSource>
      operationName: <operationName>
      status: <status>
      resourceId: <resourceId>
    ```

    Em seguida, procure e substitua o \<campos\> com etiquetas de conteúdo dinâmicas com o mesmo nome.

    > [!NOTE]
    > Existem dois campos dinâmicos que são nomeados **estado**. Adicione ambos estes campos para a mensagem. Utilize o campo que está a ser o **activityLog** matriz de propriedades e elimine o outro campo. Coloque o cursor sobre o **estado** campo para ver a referência de campo completamente qualificado, conforme mostrado na captura de ecrã seguinte:

    ![Ação de Teams da Microsoft: publicar uma mensagem](media/monitoring-action-groups/teams-action-post-message.png "ação Teams da Microsoft: publicar uma mensagem")

14. Na parte superior do **Designer de aplicações lógicas**, selecione **guardar** para guardar a sua aplicação lógica.

15. Abra o grupo de ação existente e adicionar uma ação para fazer referência a aplicação lógica. Se não tiver um grupo existente de ação, consulte [criar e gerir grupos de ação no portal do Azure](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-action-groups) para criar um. Não se esqueça de guardar as alterações.

    ![Atualizar o grupo de ação](media/monitoring-action-groups/update-action-group.png "atualizar o grupo de ação")

Da próxima vez que um alerta invoca o grupo de ação, a sua aplicação lógica é chamada.

## <a name="create-a-service-health-alert"></a>Criar um alerta de estado de funcionamento do serviço

Entradas de estado de funcionamento do serviço do Azure fazem parte do registo de atividade. O processo para criar o alerta é semelhante à [criar um alerta de registo de atividade](#create-an-activity-log-alert-administrative), mas com algumas alterações:

- Os passos 1 a 7 são os mesmos.
- Para o passo 8, utilize o esquema de exemplo seguintes para o acionador de pedido HTTP:

    ```json
    {
        "schemaId": "Microsoft.Insights/activityLogs",
        "data": {
            "status": "Activated",
            "context": {
                "activityLog": {
                    "channels": "Admin",
                    "correlationId": "e416ed3c-8874-4ec8-bc6b-54e3c92a24d4",
                    "description": "…",
                    "eventSource": "ServiceHealth",
                    "eventTimestamp": "2018-04-03T22:44:43.7467716+00:00",
                    "eventDataId": "9ce152f5-d435-ee31-2dce-104228486a6d",
                    "level": "Informational",
                    "operationName": "Microsoft.ServiceHealth/incident/action",
                    "operationId": "e416ed3c-8874-4ec8-bc6b-54e3c92a24d4",
                    "properties": {
                        "title": "…",
                        "service": "…",
                        "region": "Global",
                        "communication": "…",
                        "incidentType": "Incident",
                        "trackingId": "…",
                        "impactStartTime": "2018-03-22T21:40:00.0000000Z",
                        "impactMitigationTime": "2018-03-22T21:41:00.0000000Z",
                        "impactedServices": "[{"ImpactedRegions"}]",
                        "defaultLanguageTitle": "…",
                        "defaultLanguageContent": "…",
                        "stage": "Active",
                        "communicationId": "11000001466525",
                        "version": "0.1.1"
                    },
                    "status": "Active",
                    "subscriptionId": "…",
                    "submissionTimestamp": "2018-04-03T22:44:50.8013523+00:00"
                }
            },
            "properties": {}
        }
    }
    ```

-  Os passos 9 e 10 são os mesmos.
-  Para obter os passos 11 através de 14, utilize o seguinte processo:

   1. Selecione **+** **novo passo** e, em seguida, escolha **adicionar uma condição**. Defina as condições seguintes para se certificar de que a aplicação lógica executa apenas quando os dados de entrada corresponde a estes valores:
       - `schemaId == Microsoft.Insights/activityLogs`
       - `eventSource == ServiceHealth`
       - `version == 0.1.1`

      !["Condição do payload de estado de funcionamento do serviço"](media/monitoring-action-groups/service-health-payload-condition.png "condição do payload de estado de funcionamento do serviço")

   1. No **se for verdadeiro** condição, siga as instruções nos passos 11 através de 13 no [criar um alerta de registo de atividade](#create-an-activity-log-alert-administrative) para adicionar a ação de Teams da Microsoft.

   1. Defina a mensagem utilizando uma combinação de HTML e de conteúdo dinâmico. Copie e cole o seguinte conteúdo para o **mensagem** campo. Substitua o `[incidentType]`, `[trackingID]`, `[title]`, e `[communication]` campos com etiquetas de conteúdo dinâmicos com o mesmo nome:

       ```html
       <p>
       <b>Alert Type:</b>&nbsp;<strong>[incidentType]</strong>&nbsp;
       <strong>Tracking ID:</strong>&nbsp;[trackingId]&nbsp;
       <strong>Title:</strong>&nbsp;[title]</p>
       <p>
       <a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details, log in to the Azure Service Health dashboard.</a>
       </p>
       <p>[communication]</p>
       ```

       !["Action de post do Estado de funcionamento do serviço condição verdadeira"](media/monitoring-action-groups/service-health-true-condition-post-action.png "ação de mensagem de estado de funcionamento do serviço condição verdadeira")

   1. Para o **se for FALSO** condição, forneça uma mensagem úteis:

       ```html
       <p><strong>Service Health Alert</strong></p>
       <p><b>Unrecognized alert schema</b></p>
       <p><a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details, log in to the Azure Service Health dashboard.\</a></p>
       ```

       !["Action de post de condição falso de estado de funcionamento do serviço"](media/monitoring-action-groups/service-health-false-condition-post-action.png "ação de post do Estado de funcionamento do serviço condição falsa")

- Passo 15 é o mesmo. Siga as instruções para guardar a sua aplicação lógica e atualizar o grupo de ação.

## <a name="create-a-metric-alert"></a>Criar um alerta de métrico

O processo para criar um alerta de métrico é semelhante à [criar um alerta de registo de atividade](#create-an-activity-log-alert-administrative), mas com algumas alterações:

- Os passos 1 a 7 são os mesmos.
- Para o passo 8, utilize o esquema de exemplo seguintes para o acionador de pedido HTTP:

    ```json
    {
    "schemaId": "AzureMonitorMetricAlert",
    "data": {
        "version": "2.0",
        "status": "Activated",
        "context": {
        "timestamp": "2018-04-09T19:00:07.7461615Z",
        "id": "…",
        "name": "TEST-VM CPU Utilization",
        "description": "",
        "conditionType": "SingleResourceMultipleMetricCriteria",
        "condition": {
            "windowSize": "PT15M",
            "allOf": [
                {
                    "metricName": "Percentage CPU",
                    "dimensions": [
                    {
                        "name": "ResourceId",
                        "value": "d92fc5cb-06cf-4309-8c9a-538eea6a17a6"
                    }
                ],
                "operator": "GreaterThan",
                "threshold": "5",
                "timeAggregation": "PT15M",
                "metricValue": 1.0
            }
            ]
        },
        "subscriptionId": "…",
        "resourceGroupName": "TEST",
        "resourceName": "test-vm",
        "resourceType": "Microsoft.Compute/virtualMachines",
        "resourceId": "…",
        "portalLink": "…"
        },
        "properties": {}
    }
    }
    ```

- Os passos 9 e 10 são os mesmos.
- Para obter os passos 11 através de 14, utilize o seguinte processo:

   1. Selecione **+** **novo passo** e, em seguida, escolha **adicionar uma condição**. Defina as condições seguintes para se certificar de que a aplicação lógica executa apenas quando os dados de entrada corresponde a estes valores:
       - `schemaId == AzureMonitorMetricAlert`
       - `version == 2.0`

       !["Condição métrica payload alerta"](media/monitoring-action-groups/metric-alert-payload-condition.png "condição métrica payload de alerta")

   1. No **se for verdadeiro** condição, adicione um **para cada** ciclo e a ação de Teams da Microsoft. Defina a mensagem utilizando uma combinação de HTML e de conteúdo dinâmico.

       !["Action de post métrica condição verdadeira alerta"](media/monitoring-action-groups/metric-alert-true-condition-post-action.png "ação de post métrica condição verdadeira alerta")

   1. No **se for FALSO** condição, definir uma ação de Teams da Microsoft para comunicar que o alerta métrico não corresponde às expectativas da aplicação lógica. Inclua JSON payload. Repare como fazer referência a `triggerBody` conteúdo dinâmico a `json()` expressão.

       !["Action de post métrica condição falso alerta"](media/monitoring-action-groups/metric-alert-false-condition-post-action.png "ação post de condição de Falso alerta métrica")

- Passo 15 é o mesmo. Siga as instruções para guardar a sua aplicação lógica e atualizar o grupo de ação.

## <a name="next-steps"></a>Passos Seguintes
* Obter um [descrição geral dos alertas de registo de atividade do Azure](monitoring-overview-alerts.md) e saber como receber alertas.  
* Saiba como [configurar alertas quando é devolvida uma notificação de estado de funcionamento de serviço de Azure](monitoring-activity-log-alerts-on-service-notifications.md).
* Saiba mais sobre [grupos ação](monitoring-action-groups.md).