---
title: Iniciar expandir os alertas do OMS no Azure | Microsoft Docs
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
ms.date: 03/16/2018
ms.author: vinagara
ms.openlocfilehash: 9361c2a0a4854f463eb2d679c3884f84f6858997
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="initiate-extending-alerts-from-oms-into-azure"></a>Iniciar expandir alertas do OMS no Azure
A partir **23 de Abril de 2018**, todos os clientes através de alertas que são configurados no [Microsoft Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), será expandido para o Azure. Alertas que são expandidos para o Azure comportam-se da mesma forma que OMS. As capacidades de monitorização permaneçam intactas. Expandir alertas criados no OMS para o Azure fornece várias vantagens. Para obter mais informações sobre as vantagens e o processo de alargar alertas do OMS no Azure, consulte [expandir os alertas do OMS no Azure](monitoring-alerts-extend.md).

Os clientes que pretenda mover os alertas do OMS no Azure imediatamente, pode fazê-lo utilizando uma das opções indicadas.

## <a name="option-1---using-oms-portal"></a>Opção 1 - utilizando o portal do OMS
Para iniciar voluntariamente a expansão de alertas do OMS no Azure, siga os passos listados abaixo.

1. Na página Descrição geral do Operations Management Suite (OMS), aceda a definições e, em seguida, a secção alertas. Clique no botão com a etiqueta "Expandir para Azure", como realçada na figura abaixo.

    ![Página de definições de alerta do OMS com opção de expanda](./media/monitor-alerts-extend/ExtendInto.png)

2. Depois do botão é clicado um Assistente de 3-passo será apresentado, com o primeiro passo, fornecendo detalhes do processo. Prima seguinte, para continuar.

    ![Expanda os alertas do OMS no Azure - passo 1](./media/monitor-alerts-extend/ExtendStep1.png)

3. No segundo passo, o sistema irá mostrar um resumo da alteração proposta, listagem adequada [ação grupos](monitoring-action-groups.md), para os alertas no OMS. Se ações semelhantes são vistas através dos mais do que um alerta - o sistema irá propor associe todos eles grupo uma única ação.  Grupo de ação propostas siga a Convenção de nomenclatura: *WorkspaceName_AG_ #Number*. Para ser continuar, clique em seguinte.
Ecrã de exemplo abaixo.

    ![Expanda os alertas do OMS no Azure - passo 2](./media/monitor-alerts-extend/ExtendStep2.png)

    > [!NOTE]
    > Se a edição de alerta opção mostrada acima, é utilizada a; o utilizador não poderá regressar ao assistente. E será necessário para reiniciar o processo de alargar alertas do OMS no Azure, do passo 1. Também a lista mostra alteração proposta resumo, o resultado real pode variar com base nas alterações são efetuadas em paralelo.

4. No último passo do assistente, pode pedir ao OMS para agendar a expandir a todos os alertas no Azure - através da criação de novos grupos de ação e associar os alertas, conforme mostrado no ecrã anterior. Para continuar escolher "Ter OMS automaticamente todos os alertas na sua área de trabalho para o Azure", em seguida, clique em Concluir e confirme na linha de comandos para iniciar o processo. Em alternativa, os clientes podem utilizar uma nova API de análise do registo - para acionar manualmente expandir os alertas, escolhendo a opção alternativa. 

    ![Expanda os alertas do OMS no Azure - passo 3](./media/monitor-alerts-extend/ExtendStep3.png)

5. Depois do assistente estiver concluído, controlo regressará à página de definições de alerta e a opção "Expandir para Azure" será removida. Em segundo plano, OMS ação irá agendar alertas no OMS expandido no Azure; Isto pode demorar algum tempo e quando a operação começa um breve período alertas de no OMS não estarão disponível para serem modificadas. Depois de concluído o processo em segundo plano, mensagens de correio eletrónico serão enviadas para todos os utilizadores com função de administrador ou de contribuinte; com os detalhes dos grupos de ação criados e respetivas alertas foram associados com. 

6. Alertas irão continuar a ser listados na OMS, mesmo depois de serem obterem expandidos no Azure.

    ![Após expandir alertas no OMS no Azure](./media/monitor-alerts-extend/PostExtendList.png)


## <a name="option-2---using-api"></a>Opção 2 - utilizando a API
Para os clientes que pretendem programaticamente, controlar ou automatizar o processo de alargar alertas no OMS no Azure; Microsoft forneceu nova API AlertsVersion em análise de registos.

A API de AlertsVersion de análise do registo é RESTful e pode ser acedida através da API de REST do Azure Resource Manager. Neste documento, vai encontrar exemplos em que a API é acedida a partir de uma linha de comandos do PowerShell utilizando [ARMClient](https://github.com/projectkudu/ARMClient), uma ferramenta de linha de comandos de open source que simplifica a invocar a API do Azure Resource Manager. A utilização de ARMClient e do PowerShell é uma das muitas opções para aceder à API do. A API irá enviar os resultados no formato JSON, permitindo a utilização dos resultados de várias maneiras diferentes através de programação.

Ao utilizar GET na API, um pode obter no resultado o resumo da alteração proposta, como lista de adequado [ação grupos](monitoring-action-groups.md) para os alertas na OMS, no JSON de formato. Se a ações semelhantes são vistas através dos mais do que um alerta - o sistema irá propor criar associe todos eles grupo uma única ação.  Grupo de ação propostas siga a Convenção de nomenclatura: *WorkspaceName_AG_ #Number*.

```
armclient GET  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

> [!NOTE]
> OBTER chamada à API do não irá resultar em alertas no OMS obter expandido no Azure. Apenas fornecerá como resposta o resumo de propostas de alterações. Para confirmar que estas alterações ser feito para expandir os alertas no Azure, um pedido POST chamada tem de ser feito para a API.

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

Para iniciar o agendamento de expandir os alertas no OMS no Azure, inicie um pedido POST para a API. Ao executar este comando/chamada confirma que o utilizador intenção, bem como aceitação tem os alertas no OMS expandido para o Azure e efetuar as alterações, conforme indicado na resposta GET chamada à API.

```
armclient POST  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

> [!NOTE]
> Resultado de expandir OMS alertas no Azure, pode variar entre o resumo fornecido pelo GET - na conta de qualquer alteração efetuada no sistema. Assim que a agendada, os alertas no OMS será temporariamente indisponíveis para editar/modificação - enquanto podem ser criadas novos alertas. 

Se a publicação for bem-sucedido, deverá devolver a resposta 200 OK juntamente com:
```json
{
    "version": 2
}
```
Com a indicação de que os alertas foram expandidos no Azure, conforme indicado pelo versão 2. Esta versão é apenas para a verificar se alertas tem sido expandidas para o Azure e não tem o nenhum efeito em utilização com [API de pesquisa de análise do registo](../log-analytics/log-analytics-api-alerts.md). Assim que os alertas são expandidos no Azure com êxito, todos os utilizadores associados a funções de administrador e contribuinte na área de trabalho irão receber um e-mail com detalhes sobre as alterações efetuadas.


E por fim, se todos os alertas na área de trabalho especificado, já ter sido agendado para ser expandido para o Azure - a resposta a POST 403 Proibido.


## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre as novas [experiência de alertas do Azure](monitoring-overview-unified-alerts.md).
* Saiba mais sobre [registar alertas nos alertas do Azure](monitor-alerts-unified-log.md).
