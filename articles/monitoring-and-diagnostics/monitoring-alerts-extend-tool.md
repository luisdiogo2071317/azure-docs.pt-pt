---
title: Como expandir (cópia) alertas a partir do portal do OMS no Azure | Microsoft Docs
description: Ferramentas e API através do qual alargar alertas da OMS para alertas do Azure, pode ser feito por clientes voluntariamente.
author: msvijayn
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/06/2018
ms.author: vinagara
ms.openlocfilehash: e5dc48aa5e3c614192ae140dc80b5d9845acc474
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="how-to-extend-copy-alerts-from-oms-into-azure"></a>Como expandir alertas (cópia) do OMS no Azure
A partir **14 de Maio de 2018**, todos os clientes através de alertas que são configurados no [Microsoft Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), será expandido para o Azure. Alertas que são expandidos para o Azure comportam-se da mesma forma que OMS. As capacidades de monitorização permaneçam intactas. Expandir alertas criados no OMS para o Azure fornece várias vantagens. Para obter mais informações sobre as vantagens e o processo de alargar alertas do OMS no Azure, consulte [expandir os alertas do OMS no Azure](monitoring-alerts-extend.md).

Os clientes que pretenda mover os alertas do OMS no Azure imediatamente, pode fazê-lo utilizando uma das opções indicadas.

## <a name="option-1---using-oms-portal"></a>Opção 1 - utilizando o portal do OMS
Para iniciar voluntariamente a expansão de alertas do portal do OMS no Azure, siga os passos listados abaixo.

1. Na página de descrição geral portal do OMS, vá a definições e, em seguida, a secção alertas. Clique no botão com a etiqueta "Expandir para Azure", como realçada na figura abaixo.

    ![Página Definições de alerta de portal do OMS com opção de expanda](./media/monitor-alerts-extend/ExtendInto.png)

2. Depois do botão é clicado um Assistente de 3-passo será apresentado, com o primeiro passo, fornecendo detalhes do processo. Prima seguinte, para continuar.

    ![Expandir alertas a partir do portal do OMS no Azure - passo 1](./media/monitor-alerts-extend/ExtendStep1.png)

3. No segundo passo, o sistema irá mostrar um resumo da alteração proposta, listagem adequada [ação grupos](monitoring-action-groups.md), para os alertas no portal do OMS. Se ações semelhantes são vistas através dos mais do que um alerta - o sistema irá propor associe todos eles grupo uma única ação.  Grupo de ação propostas siga a Convenção de nomenclatura: *WorkspaceName_AG_ #Number*. Para ser continuar, clique em seguinte.
Ecrã de exemplo abaixo.

    ![Expandir alertas a partir do portal do OMS no Azure - passo 2](./media/monitor-alerts-extend/ExtendStep2.png)


4. No último passo do assistente, pode colocar o portal do OMS para agendar a expandir a todos os alertas no Azure - através da criação de novos grupos de ação e associar os alertas, conforme mostrado no ecrã anterior. Para continuar escolha clique em Concluir e confirme na linha de comandos para iniciar o processo. Opcionalmente, os clientes também podem fornecer endereços de correio eletrónico para o qual gostaria de portal do OMS para enviar um relatório em a concluir o processamento.

    ![Expandir alertas a partir do portal do OMS no Azure - passo 3](./media/monitor-alerts-extend/ExtendStep3.png)

5. Depois do assistente estiver concluído, controlo regressará à página de definições de alerta e a opção "Expandir para Azure" será removida. Em segundo plano, o portal do OMS ação irá agendar alertas no Log Analytics ser expandido no Azure; Isto pode demorar algum tempo e quando a operação começa um breve período alertas de no portal do OMS não estarão disponível para serem modificadas. Será mostrado o estado atual através de faixa e se endereços de e-mail onde fornecida durante o passo 4, em seguida, será informado quando o processo em segundo plano expande com êxito todos os alertas no Azure. 

6. Alertas irão continuar a ser apresentado no portal do OMS, mesmo depois de serem obterem expandidos com êxito no Azure.

    ![Após expandir alertas no portal do OMS no Azure](./media/monitor-alerts-extend/PostExtendList.png)


## <a name="option-2---using-api"></a>Opção 2 - utilizando a API
Para os clientes que pretendem programaticamente, controlar ou automatizar o processo de alargar alertas no portal do OMS no Azure; Microsoft forneceu nova API AlertsVersion em análise de registos.

A API de AlertsVersion de análise do registo é RESTful e pode ser acedida através da API de REST do Azure Resource Manager. Neste documento, vai encontrar exemplos em que a API é acedida a partir de uma linha de comandos do PowerShell utilizando [ARMClient](https://github.com/projectkudu/ARMClient), uma ferramenta de linha de comandos de open source que simplifica a invocar a API do Azure Resource Manager. A utilização de ARMClient e do PowerShell é uma das muitas opções para aceder à API do. A API irá enviar os resultados no formato JSON, permitindo a utilização dos resultados de várias maneiras diferentes através de programação.

Ao utilizar GET na API, um pode obter no resultado o resumo da alteração proposta, como lista de adequado [ação grupos](monitoring-action-groups.md) para os alertas no portal do OMS, no JSON de formato. Se a ações semelhantes são vistas através dos mais do que um alerta - o sistema irá propor criar associe todos eles grupo uma única ação.  Grupo de ação propostas siga a Convenção de nomenclatura: *WorkspaceName_AG_ #Number*.

```
armclient GET  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

> [!NOTE]
> OBTER chamada à API do não irá resultar em alertas no portal do OMS obter expandido no Azure. Apenas fornecerá como resposta o resumo de propostas de alterações. Para confirmar que estas alterações ser feito para expandir os alertas no Azure, um pedido POST chamada tem de ser feito para a API.

Se a chamada GET API for bem-sucedida, juntamente com a resposta OK 200, uma lista JSON de alertas, juntamente com grupos de ação proposto seria fornecida. Resposta de exemplo abaixo:

```json
{
    "version": 1,
    "migrationSummary": {
        "alertsCount": 2,
        "actionGroupsCount": 2,
        "alerts": [
            {
                "alertName": "DemoAlert_1",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_1"
            },
            {
                "alertName": "DemoAlert_2",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_2"
            }
        ],
        "actionGroups": [
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                "actions": {
                    "emailIds": [
                        "JohnDoe@mail.com"
                    ],
                    "webhookActions": [
                        {
                            "name": "Webhook_1",
                            "serviceUri": "http://test.com"
                        }
                    ],
                    "itsmAction": {}
                }
            },
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                 "actions": {
                    "emailIds": [
                        "test1@mail.com",
                          "test2@mail.com"
                    ],
                    "webhookActions": [],
                    "itsmAction": {
                        "connectionId": "<Guid>",
                        "templateInfo":"{\"PayloadRevision\":0,\"WorkItemType\":\"Incident\",\"UseTemplate\":false,\"WorkItemData\":\"{\\\"contact_type\\\":\\\"email\\\",\\\"impact\\\":\\\"3\\\",\\\"urgency\\\":\\\"2\\\",\\\"category\\\":\\\"request\\\",\\\"subcategory\\\":\\\"password\\\"}\",\"CreateOneWIPerCI\":false}"
                    }
                }
            }
        ]
    }
}

```
No caso, não existem nenhum alerta na área de trabalho especificada, juntamente com a resposta OK 200 para a operação GET JSON seria:

```json
{
    "version": 1,
    "Message": "No Alerts found in the workspace for migration."
}
```

Se já tiver sido expandido todos os alertas na área de trabalho especificado, no Azure - a resposta a chamada GET seria:
```json
{
    "version": 2
}
```

Para iniciar o agendamento de expandir os alertas no portal do OMS no Azure, inicie um pedido POST para a API. Ao executar este comando/chamada confirma que o utilizador intenção, bem como aceitação tem os alertas no portal do OMS expandido para o Azure e efetuar as alterações, conforme indicado na resposta GET chamada à API. Opcionalmente, o utilizador pode fornecer uma lista de endereços de e-mail que OMS portal será correio um relatório, quando o processo em segundo plano agendada de expandir os alertas no portal do OMS no Azure for concluído com êxito.

```
$emailJSON = “{‘Recipients’: [‘a@b.com’, ‘b@a.com’]}”
armclient POST  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $emailJSON
```

> [!NOTE]
> Resultado de expandir o OMS portal alertas no Azure, pode variar entre o resumo fornecido pelo GET - na conta de qualquer alteração efetuada no sistema. Assim que a agendada, os alertas no portal do OMS será temporariamente indisponíveis para editar/modificação - enquanto podem ser criadas novos alertas. 

Se a publicação for bem-sucedido, deverá devolver a resposta 200 OK juntamente com:
```json
{
    "version": 2
}
```
Com a indicação de que os alertas foram expandidos no Azure, conforme indicado pelo versão 2. Esta versão é apenas para a verificar se alertas tem sido expandidas para o Azure e não tem o nenhum efeito em utilização com [API de pesquisa de análise do registo](../log-analytics/log-analytics-api-alerts.md). Assim que os alertas são expandidos no Azure com êxito, todos os endereços de correio eletrónico fornecidos durante a GET será enviado um relatório com detalhes sobre as alterações efetuadas.

E por fim, se todos os alertas na área de trabalho especificado, já está agendado para ser expandido para o Azure - a resposta a POST 403 Proibido. Para ver qualquer mensagem de erro ou determinar se se expandir o processo está bloqueado, o utilizador pode fazer uma mensagem de erro e chamada GET se qualquer será devolvido, juntamente com o resumo.

```json
{
    "version": 1,
    "message": "OMS was unable to extend your alerts into Azure, Error: The subscription is not registered to use the namespace 'microsoft.insights'. OMS will schedule extending your alerts, once remediation steps illustrated in the troubleshooting guide are done.",
    "recipients": [
       "john.doe@email.com",
       "jane.doe@email.com"
     ],
    "migrationSummary": {
        "alertsCount": 2,
        "actionGroupsCount": 2,
        "alerts": [
            {
                "alertName": "DemoAlert_1",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_1"
            },
            {
                "alertName": "DemoAlert_2",
                "alertId": " /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/savedSearches/<savedSearchId>/schedules/<scheduleId>/actions/<actionId>",
                "actionGroupName": "<workspaceName>_AG_2"
            }
        ],
        "actionGroups": [
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                "actions": {
                    "emailIds": [
                        "JohnDoe@mail.com"
                    ],
                    "webhookActions": [
                        {
                            "name": "Webhook_1",
                            "serviceUri": "http://test.com"
                        }
                    ],
                    "itsmAction": {}
                }
            },
            {
                "actionGroupName": "<workspaceName>_AG_1",
                "actionGroupResourceId": "/subscriptions/<subscriptionid>/resourceGroups/<resourceGroupName>/providers/microsoft.insights/actionGroups/<workspaceName>_AG_1",
                 "actions": {
                    "emailIds": [
                        "test1@mail.com",
                          "test2@mail.com"
                    ],
                    "webhookActions": [],
                    "itsmAction": {
                        "connectionId": "<Guid>",
                        "templateInfo":"{\"PayloadRevision\":0,\"WorkItemType\":\"Incident\",\"UseTemplate\":false,\"WorkItemData\":\"{\\\"contact_type\\\":\\\"email\\\",\\\"impact\\\":\\\"3\\\",\\\"urgency\\\":\\\"2\\\",\\\"category\\\":\\\"request\\\",\\\"subcategory\\\":\\\"password\\\"}\",\"CreateOneWIPerCI\":false}"
                    }
                }
            }
        ]
    }
}              

```

## <a name="troubleshooting"></a>Resolução de problemas 
Durante o processo de alargar alertas do OMS no Azure, pode haver problemas ocasionais que impede que o sistema de criação necessários [ação grupos](monitoring-action-groups.md). Nestes casos, uma mensagem de erro será apresentada no portal do OMS através de faixa na secção de alerta e, na chamada GET efetuada à API.

Listadas abaixo estão os passos de remediação para cada erro:
1. **Erro: A subscrição não está registada para utilizar o espaço de nomes 'insights'**: ![página Definições de alerta de portal do OMS com a mensagem de erro de registo](./media/monitor-alerts-extend/ErrorMissingRegistration.png)

    a. A subscrição associada à sua área de trabalho do OMS - não foi registada para utilizar a funcionalidade de monitorização do Azure (insights); devido a não é possível expandir alertas no Monitor de Azure & alertas que OMS.
    
    b. Para resolver, registe utilize insights (alertas & de monitor do Azure) na sua subscrição utilizando o Powershell, a CLI do Azure ou o portal do Azure. Para obter mais informações, veja o artigo no [resolver erros no registo do fornecedor de recursos](../azure-resource-manager/resource-manager-register-provider-errors.md)
    
    c. Depois de resolvido de acordo com os passos ilustrados no artigo, OMS irá expandir os alertas no Azure dentro da execução agendada o dia seguinte; sem a necessidade de qualquer ação ou foi iniciado.
2. **Erro: O bloqueio de âmbito está presente ao nível do grupo de recursos/subscrição para operações de escrita**: ![página Definições de alerta de portal do OMS com a mensagem de erro ScopeLock](./media/monitor-alerts-extend/ErrorScopeLock.png)

    a. Quando bloqueia âmbito estiver ativada, restringindo qualquer alteração nova na subscrição ou grupo de recursos que contém a área de trabalho de análise de registos (OMS); o sistema não consegue expandir alertas (cópia) no Azure e criar grupos de ação necessária.
    
    b. Para resolver, elimine o *ReadOnly* bloqueio na sua subscrição ou grupo de recursos que contém a área de trabalho; utilizando o portal do Azure, o Powershell, CLI do Azure ou API. Para obter mais informações, veja o artigo no [utilização de recursos do bloqueio](../azure-resource-manager/resource-group-lock-resources.md). 
    
    c. Depois de resolvido de acordo com os passos ilustrados no artigo, OMS irá expandir os alertas no Azure dentro da execução agendada o dia seguinte; sem a necessidade de qualquer ação ou foi iniciado.


## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre as novas [experiência de alertas do Azure](monitoring-overview-unified-alerts.md).
* Saiba mais sobre [registar alertas nos alertas do Azure](monitor-alerts-unified-log.md).
