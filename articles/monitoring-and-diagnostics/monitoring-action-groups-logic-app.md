---
title: Como a acionar ações complexas com alertas de monitorização do Azure e os grupos de ações
description: Saiba como criar uma ação de aplicação lógica para o processo de alertas de monitorização do Azure.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/30/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: eafb2bcf0175190748c9dd020051cbebfcaee1fd
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263889"
---
# <a name="create-a-logic-app-action"></a>Criar uma ação de aplicação lógica
## <a name="overview"></a>Descrição geral ##
Quando é acionado um alerta de Monitor do Azure, aquele invoca um [grupo ação](monitoring-action-groups.md). Grupos de ação permitem-lhe acionar uma ou mais ações para notificar os utilizadores do alerta e até mesmo resolvê-lo.

Este artigo mostra como configurar e acionar uma aplicação lógica para criar uma conversação no Microsoft Teams quando um alerta é acionado.

O processo geral é:

-   Criar a aplicação lógica para o respetivo tipo de alerta

-   Importar o esquema para o respetivo tipo de alerta para a aplicação lógica

-   Definir o comportamento da aplicação lógica

-   Copie o ponto final de HTTP da aplicação lógica para um grupo de ação do Azure

O processo é semelhante, se pretender que a aplicação lógica para efetuar uma ação diferente.

## <a name="create-an-activity-log-alert--administrative"></a>Criar um alerta de registo de atividade – administração

1.  Clique no botão **Criar um recurso**, no canto superior esquerdo do portal do Azure.

2.  Pesquise e selecione **Aplicação Lógica**. Clique no botão **Criar**.

3.  Dê um nome a sua aplicação lógica, escolha um grupo de recursos, etc.

    ![Caixa de diálogo de aplicação de lógica de criar](media/monitoring-action-groups/create-logic-app-dialog.png "caixa de diálogo de aplicação de lógica de criar")

4.  Clique no botão Criar para criar a aplicação lógica. Será apresentado um pop-up quando a aplicação lógica tiver sido criada. Clique no botão iniciar recursos para abrir o designer de aplicações lógicas.

5.  Selecione o acionador **quando HTTP de pedido é recebido**.

    ![Acionadores da aplicação lógica](media/monitoring-action-groups/logic-app-triggers.png "Acionadores da aplicação lógica")

6.  Selecione **editar** para alterar o acionador de pedido HTTP

    ![Forma de Acionador de pedido HTTP](media/monitoring-action-groups/http-request-trigger-shape.png "forma de Acionador de pedido HTTP")

7.  Selecione **Utilizar o payload de exemplo para gerar esquema**.

    ![Botão de payload de exemplo de utilização](media/monitoring-action-groups/use-sample-payload-button.png "botão de payload de exemplo de utilização")

8.  Copie e cole o esquema de exemplo seguinte na caixa de diálogo.

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

9. O Designer de aplicação de lógica irá apresentar uma nota reminding esse pedido enviado para a aplicação lógica tem como o cabeçalho de tipo de conteúdo application/json. Avançar e dispensar a caixa de diálogo. O alerta de Monitor do Azure irá efetuar este procedimento corretamente.

    ![Cabeçalho Content-Type](media/monitoring-action-groups/content-type-header.png "cabeçalho Content-Type")

10. Selecione **+ novo passo** e, em seguida, escolha **adicionar uma ação**.

    ![Adicionar ação](media/monitoring-action-groups/add-action.png "adicionar ação")

11. Procure e seleccione o conector do Microsoft Teams. Escolha o **Teams da Microsoft – mensagem Post** ação.

    ![Ações de Microsoft Teams](media/monitoring-action-groups/microsoft-teams-actions.png "ações Teams da Microsoft")

12. Configure a ação de Teams da Microsoft. O Designer de aplicações lógicas pedirá para autenticar a sua conta Office 365. Escolha o **equipa Id** e **Id do canal** para enviar a mensagem.

13. Configurar o **mensagem** utilizando uma combinação de texto estático e as referências a \<campos\> no conteúdo dinâmico. Corte e cole o seguinte texto no campo mensagem.

    ```text
      Activity Log Alert: <eventSource>
      operationName: <operationName>
      status: <status>
      resourceId: <resourceId>
    ```

    Em seguida, procure e substitua o \<campos\> com etiquetas de conteúdo dinâmicas com o mesmo nome.

    **[NOTA!]**  Existem dois campos dinâmicos com o nome **estado**. Adicione ambos os campos de estado para a mensagem. Utilize um na matriz de propriedades de activityLog e elimine o outro. Se pairar o rato sobre o **estado** campo, verá a referência de campo completamente qualificado, conforme mostrado na captura de ecrã

    ![Ação - mensagem Post de equipas](media/monitoring-action-groups/teams-action-post-message.png "equipas ação - mensagem Post")

14. Guardar a sua aplicação lógica clicando no botão Guardar na parte superior do Designer

15. Clique em forma de pedido de HTTP para fazer com que a expandir. Copie o URL de POST de HTTP.

    ![URL DO HTTP POST](media/monitoring-action-groups/http-post-url.png "HTTP POST URL")

16. Abra o grupo de ação existente e adicionar uma ação para fazer referência a aplicação lógica. Se não tiver um grupo existente de ação, consulte <https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-action-groups> para criar um. Não se esqueça de guardar as alterações.

    ![Grupo de atualização de ação](media/monitoring-action-groups/update-action-group.png "grupo de atualização de ação")

Da próxima vez que um alerta invoca o grupo de ação, a sua aplicação lógica é chamada.

## <a name="create-a-service-health-alert"></a>Criar um alerta de estado de funcionamento do serviço

As entradas de estado de funcionamento de serviço fazem parte do registo de atividade, para que o processo é semelhante com as seguintes alterações

1.  Os passos 1 a 7 são os mesmos.
2.  Utilize o esquema de exemplo seguintes para o acionador de HTTP no passo 8.

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

3.  Passos 9 10 são os mesmos.
4.  A partir do passo 11 no utilize o procedimento abaixo indicado.
5.  Clique em de **+ novo passo** botão e escolha **adicionar uma condição**. Definir as condições seguintes para se certificar de que a aplicação lógica executa apenas quando os dados de entrada corresponde a estes valores
    - schemaId = = Microsoft.Insights/activityLogs
    - eventSource = = ServiceHealth
    - versão = = 0.1.1

    !["Serviço de condição do payload de estado de funcionamento"](media/monitoring-action-groups/service-health-payload-condition.png "condição do payload de estado de funcionamento do serviço")

6. No **se for verdadeiro** condição, adicione a ação de Microsoft Teams utilizando os passos 11 – 13 do exemplo anterior.

7. Defina a mensagem utilizando uma combinação de HTML e [conteúdo dinâmico]. Copie e cole o conteúdo abaixo no campo mensagem. Substitua o [incidentType], [trackingID], [título] e [comunicação] campos com etiquetas de conteúdo dinâmicos com o mesmo nome

    ```html
    <p>
    <b>Alert Type:</b>&nbsp;<strong>[incidentType]</strong>&nbsp;
    <strong>Tracking ID:</strong>&nbsp;[trackingId]&nbsp;
    <strong>Title:</strong>&nbsp;[title]</p>
    <p>
    <a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details log into the Azure Service Health dashboard</a>
    </p>
    <p>[communication]</p>
    ```

    !["Serviço ação do Estado de funcionamento condição verdadeira post"](media/monitoring-action-groups/service-health-true-condition-post-action.png "ação de post de condição verdadeira de estado de funcionamento do serviço")

8. Para o **se for FALSO** condição forneça uma mensagem útil

    ```html
    <p><strong>Service Health Alert</strong></p>
    <p><b>Unrecognized alert schema</b></p>
    <p><a href="https://ms.portal.azure.com/#blade/Microsoft_Azure_Health/AzureHealthBrowseBlade/serviceIssues">For details log into the Azure Service Health dashboard\</a></p>
    ```

    !["Action de post de condição falso de estado de funcionamento do serviço"](media/monitoring-action-groups/service-health-false-condition-post-action.png "ação de post do Estado de funcionamento do serviço condição falsa")

9.  Siga os passos 15 – 16 do exemplo anterior para guardar a sua aplicação lógica e atualizar o grupo de ação

## <a name="metric-alert"></a>Alerta de Métrica

1.  Os passos 1 a 7 são os mesmos que o primeiro exemplo
2.  Utilize o esquema de exemplo seguintes para o acionador de HTTP no passo 8.

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

3.  Passos 9 10 são os mesmos.
4.  A partir do passo 11 no utilize o procedimento abaixo indicado.
5.  Clique em de **+ novo passo** botão e escolha **adicionar uma condição**. Definir as condições seguintes para se certificar de que a aplicação lógica executa apenas quando os dados de entrada corresponde a estes valores

    - schemaId = = AzureMonitorMetricAlert
    - versão = = 2.0

    !["Condição métrica payload alerta"](media/monitoring-action-groups/metric-alert-payload-condition.png "condição métrica payload de alerta")

6.  No **se for verdadeiro** condição iremos adicionar um **para cada** forma e a ação de Teams da Microsoft e definir a mensagem utilizando uma combinação de HTML e [conteúdo dinâmico]

    !["Action de post métrica condição verdadeira alerta"](media/monitoring-action-groups/metric-alert-true-condition-post-action.png "ação de post métrica condição verdadeira alerta")

7.  No **se for FALSO** formam, definir uma ação de Teams da Microsoft que comunica que o alerta de métrica não correspondem às expectativas da aplicação de lógica e incluem o payload JSON. Tenha em atenção a como podemos referenciar o conteúdo dinâmico triggerBody na expressão json().

    !["Action de post métrica condição falso alerta"](media/monitoring-action-groups/metric-alert-false-condition-post-action.png "ação post de condição de Falso alerta métrica")

8.  Siga os passos 15 – 16 do primeiro exemplo, para guardar a sua aplicação lógica e atualizar o grupo de ação

## <a name="next-steps"></a>Passos Seguintes ##
* Obter um [descrição geral dos alertas de registo de atividade](monitoring-overview-alerts.md)e saber como receber alertas.  
* Saiba como [configurar alertas sempre que uma notificação de estado de funcionamento do serviço é publicada](monitoring-activity-log-alerts-on-service-notifications.md).
* Saiba mais sobre [grupos de ação](monitoring-action-groups.md)