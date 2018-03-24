---
title: Utilizar a API de REST de alerta de análise de registo do OMS
description: A API de REST de alerta de análise do registo permite-lhe criar e gerir alertas na análise de registos que faz parte do Operations Management Suite (OMS).  Este artigo fornece detalhes sobre a API e vários exemplos para efetuar operações diferentes.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 628ad256-7181-4a0d-9e68-4ed60c0f3f04
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ed0ac6e2041ef503470f7317a5736deecd1d2b8f
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="create-and-manage-alert-rules-in-log-analytics-with-rest-api"></a>Criar e gerir regras de alertas na análise de registos com a REST API
A API de REST de alerta de análise do registo permite-lhe criar e gerir alertas no Operations Management Suite (OMS).  Este artigo fornece detalhes sobre a API e vários exemplos para efetuar operações diferentes.

A API de REST de pesquisa de análise do registo é RESTful e pode ser acedida através da API de REST do Azure Resource Manager. Neste documento, vai encontrar exemplos em que a API é acedida a partir de uma linha de comandos do PowerShell utilizando [ARMClient](https://github.com/projectkudu/ARMClient), uma ferramenta de linha de comandos de open source que simplifica a invocar a API do Azure Resource Manager. A utilização de ARMClient e do PowerShell é uma das muitas opções para aceder à API de pesquisa de análise do registo. Com estas ferramentas, pode utilizar o Gestor de recursos API RESTful do Azure para efetuar chamadas a áreas de trabalho do OMS e executar comandos de pesquisa dentro delas. A API irá enviar os resultados da pesquisa para si no formato JSON, permitindo-lhe utilizar os resultados da pesquisa de várias maneiras diferentes através de programação.

## <a name="prerequisites"></a>Pré-requisitos
Atualmente, os alertas só podem ser criados com uma procura guardada na análise de registos.  Pode consultar o [API de REST do registo de pesquisa](log-analytics-log-search-api.md) para obter mais informações.

## <a name="schedules"></a>Agendas
Uma pesquisa guardada pode ter uma ou mais agendas. A agenda define a frequência a pesquisa está a executar e o intervalo de tempo durante o qual é identificado os critérios.
As agendas de tem as propriedades na tabela seguinte.

| Propriedade | Descrição |
|:--- |:--- |
| Intervalo |Frequência de pesquisa é executada. Medido em minutos. |
| QueryTimeSpan |O intervalo de tempo durante o qual é avaliada os critérios. Tem de ser igual ou superior ao intervalo. Medido em minutos. |
| Versão |A versão de API que está a ser utilizada.  Atualmente, este deve ser sempre definido para 1. |

Por exemplo, considere uma consulta de eventos com um intervalo de 15 minutos e um Timespan de 30 minutos. Neste caso, a consulta deverá ser executada a cada 15 minutos e, seria possível acionar a um alerta se os critérios continuaram a resolver para verdadeiro por um intervalo de 30 minutos.

### <a name="retrieving-schedules"></a>A obter agendas
Utilize o método Get para obter todas as agendas de uma procura guardada.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules?api-version=2015-03-20

Utilize o método Get com um ID de agenda para obter uma determinada agenda para uma pesquisa guardada.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20

Segue-se uma resposta de amostra de uma agenda.

```json
{
    "value": [{
        "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/MyWorkspace/savedSearches/0f0f4853-17f8-4ed1-9a03-8e888b0d16ec/schedules/a17b53ef-bd70-4ca4-9ead-83b00f2024a8",
        "etag": "W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\"",
        "properties": {
            "Interval": 15,
            "QueryTimeSpan": 15
        }
    }]
}
```

### <a name="creating-a-schedule"></a>Criar uma agenda
Utilize o método Put com um ID exclusivo de agendamento para criar uma nova agenda.  Tenha em atenção que as duas agendas não podem ter o mesmo ID mesmo que se encontrem associados com diferentes das procuras guardadas.  Quando criar uma agenda na consola do OMS, um GUID é criado para o ID de agenda.

> [!NOTE]
> O nome para pesquisas guardadas todos, agendas e criadas com a API de análise do registo de ações tem de ser em minúsculas.

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson

### <a name="editing-a-schedule"></a>Editar uma agenda
Utilize o método Put com um ID de agenda existente para a mesma pesquisa guardada para modificar essa agenda.  O corpo do pedido tem de incluir o etag da agenda.

      $scheduleJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A49.8074679Z'\""','properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' } }"
      armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/mynewschedule?api-version=2015-03-20 $scheduleJson


### <a name="deleting-schedules"></a>Agendas de eliminação
Utilize o método Delete com um ID de agenda para eliminar uma agenda.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}?api-version=2015-03-20


## <a name="actions"></a>Ações
Uma agenda pode ter várias ações. Uma ação pode definir uma ou mais processos para efetuar como enviar uma mensagem ou iniciar um runbook ou pode definir um limiar que determina quando os resultados de uma procura correspondem alguns critérios.  Algumas ações vai definir ambos para que os processos são efetuados quando é cumprido o limiar.

Todas as ações de ter as propriedades na tabela seguinte.  Diferentes tipos de alertas têm diferentes propriedades adicionais, que são descritas abaixo.

| Propriedade | Descrição |
|:--- |:--- |
| Tipo |Tipo de ação.  Atualmente, os valores possíveis são alerta e o Webhook. |
| Nome |Nome a apresentar para o alerta. |
| Versão |A versão de API que está a ser utilizada.  Atualmente, este deve ser sempre definido para 1. |

### <a name="retrieving-actions"></a>A obter ações

> [!NOTE]
> A partir do dia 23 de Abril de 2018, todos os alertas na área de trabalho irão ser automaticamente expandidos para o Azure. Um utilizador pode iniciar voluntariamente expandir alertas para o Azure antes de 23 de Abril de 2018. Para obter mais informações, consulte [expandir alertas no Azure da OMS](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Para os utilizadores que expandem alertas para o Azure, as ações são agora controladas em grupos de ação do Azure. Quando uma área de trabalho e as alertas são expandidas para o Azure, pode obter ou adicionar ações utilizando o [ação grupo API](https://docs.microsoft.com/en-us/rest/api/monitor/actiongroups).

Utilize o método Get para obter todas as ações para uma agenda.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search  ID}/schedules/{Schedule ID}/actions?api-version=2015-03-20

Utilize o método Get com o ID de ação para obter uma ação específica para uma agenda.

    armclient get /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/actions/{Action ID}?api-version=2015-03-20

### <a name="creating-or-editing-actions"></a>Criação ou edição de ações
Utilize o método Put com um ID de ação que é exclusivo para a agenda para criar uma ação de novo.  Quando cria uma ação na consola do OMS, um GUID é para o ID de ação.

> [!NOTE]
> O nome para pesquisas guardadas todos, agendas e criadas com a API de análise do registo de ações tem de ser em minúsculas.

Utilize o método Put com um ID de ação existente para a mesma pesquisa guardada para modificar essa agenda.  O corpo do pedido tem de incluir o etag da agenda.

O formato do pedido para criar uma nova ação varia consoante o tipo de ação para que estes exemplos são fornecidos nas secções abaixo.

### <a name="deleting-actions"></a>Eliminar ações

> [!NOTE]
> A partir do dia 23 de Abril de 2018, todos os alertas na área de trabalho irão ser automaticamente expandidos para o Azure. Um utilizador pode iniciar voluntariamente expandir alertas para o Azure antes de 23 de Abril de 2018. Para obter mais informações, consulte [expandir alertas no Azure da OMS](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Para os utilizadores que expandem alertas para o Azure, as ações são agora controladas em grupos de ação do Azure. Quando uma área de trabalho e as alertas são expandidas para o Azure, pode obter ou adicionar ações utilizando o [ação grupo API](https://docs.microsoft.com/en-us/rest/api/monitor/actiongroups).

Utilize o método Delete com o ID de ação para eliminar uma ação.

    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Subscription ID}/schedules/{Schedule ID}/Actions/{Action ID}?api-version=2015-03-20

### <a name="alert-actions"></a>Ações de alerta
Uma agenda deve ter um e apenas uma ação de alerta.  Ações de alerta de ter um ou mais das secções na seguinte tabela.  Cada um é descrita mais detalhadamente abaixo.

| Section | Descrição | Utilização |
|:--- |:--- |:--- |
| Limiar |Critérios de quando é executada a ação.| Necessário para cada alerta, antes ou depois de estes são expandidas para o Azure. |
| Gravidade |Etiqueta utilizada para classificar o alerta quando é acionada.| Necessário para cada alerta, antes ou depois de estes são expandidas para o Azure. |
| Grupos de Ação |IDs de ActionGroup do Azure onde estão especificadas as ações necessárias, como - correio electrónico, SMSs, chamadas de voz, Webhooks, Runbooks de automatização, ITSM conectores, etc.| Necessário depois de alertas são expandidos para o Azure|
| Personalizar Ações|Modificar a saída padrão para ações selecionadas ActionGroup| Opcional para cada alerta, pode ser utilizado depois de alertas são expandidos para o Azure. |
| EmailNotification |Envie correio para vários destinatários. | Não ser obrigatório, se os alertas são expandidos para o Azure|
| Remediação |Inicie um runbook na automatização do Azure para tentar corrigir o problema identificado. |Não ser obrigatório, se os alertas são expandidos para o Azure|
| Ações de Webhook | Enviar dados de alertas, para o serviço pretendido como JSON |Não ser obrigatório, se os alertas são expandidos para o Azure|

> [!NOTE]
> A partir do dia 23 de Abril de 2018, todos os alertas na área de trabalho irão ser automaticamente expandidos para o Azure. Um utilizador pode iniciar voluntariamente expandir alertas para o Azure antes de 23 de Abril de 2018. Para obter mais informações, consulte [expandir alertas no Azure da OMS](../monitoring-and-diagnostics/monitoring-alerts-extend.md).

#### <a name="thresholds"></a>Limiares
Uma ação de alerta deve ter um e apenas um limiar.  Quando os resultados de uma procura guardada coincidir com o limiar de uma ação associada essa procura, quaisquer outros processos nessa ação são executados.  Uma ação também pode conter apenas um limiar para que possa ser utilizado com as ações de outros tipos que não contenham limiares.

Limiares de ter as propriedades na tabela seguinte.

| Propriedade | Descrição |
|:--- |:--- |
| Operador |Operador de comparação de limiar. <br> gt = maior que <br> lt = inferior a |
| Valor |Valor para o limiar. |

Por exemplo, considere uma consulta de eventos com um intervalo de 15 minutos, Timespan de 30 minutos e um limiar de maior que 10. Neste caso, a consulta deverá ser executada a cada 15 minutos e seria possível acionar um alerta se devolveu 10 eventos que foram criados ao longo de um intervalo de 30 minutos.

Segue-se uma resposta de amostra de uma ação com apenas um limiar.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Version": 1
    }

Utilize o método Put com um ID exclusivo de ação para criar uma nova ação de limiar para uma agenda.  

    $thresholdJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

Utilize o método Put com um ID de ação existente para modificar uma ação de limiar para uma agenda.  O corpo do pedido tem de incluir o etag da ação.

    $thresholdJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdJson

#### <a name="severity"></a>Gravidade
Análise de registos permite-lhe classificar os alertas em categorias, para permitir a triagem e gestão mais fácil. É a gravidade do alerta definida: informativo, aviso e crítico. Estes estão mapeadas para a dimensão de normalizado gravidade dos alertas do Azure como:

|Nível de gravidade de análise do registo  |Nível de gravidade de alertas do Azure  |
|---------|---------|
|Crítico |Gravidade 0|
|aviso |Gravidade 1|
|Informativo | Gravidade 2|

Segue-se uma resposta de amostra de uma ação com apenas um limiar e a gravidade. 

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My threshold action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Severity": "critical",
        "Version": 1    }

Utilize o método Put com um ID exclusivo de ação para criar uma nova ação para uma agenda com gravidade.  

    $thresholdWithSevJson = "{'properties': { 'Name': 'My Threshold', 'Version':'1','Severity': 'critical', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdWithSevJson

Utilize o método Put com um ID de ação existente para modificar uma ação de gravidade para uma agenda.  O corpo do pedido tem de incluir o etag da ação.

    $thresholdWithSevJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'My Threshold', 'Version':'1','Severity': 'critical', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdWithSevJson

#### <a name="action-groups"></a>Grupos de Ação
Todos os alertas no Azure, utilize a ação grupo como o mecanismo predefinido para processamento de ações. Com o grupo de ação, pode especificar as suas ações uma vez e, em seguida, associar o grupo de ação para vários alertas - através do Azure. Sem a necessidade, repetidamente declarar as mesmas ações repetidas. Grupos de ação suportam várias ações - incluindo e-mail, SMS, chamada de voz, ITSM ligação, o Runbook de automatização, URI de Webhook e muito mais. 

Para o utilizador que tiver expandido o respetivas alertas no Azure - uma agenda agora deve ter os detalhes de ação grupo transmitidos juntamente com o limiar, para conseguir criar um alerta. Detalhes da mensagem de correio eletrónico, URLs de Webhook, detalhes de automatização de Runbook e outras ações, tem de ser definido no lado, um grupo de ação primeiro antes a criar um alerta; um pode criar [ação grupo Azure monitor](../monitoring-and-diagnostics/monitoring-action-groups.md) no Portal ou utilize [ação grupo API](https://docs.microsoft.com/en-us/rest/api/monitor/actiongroups).

Para adicionar uma associação de grupo de ação para um alerta, especifique do Azure Resource Manager um ID exclusivo do grupo de ação na definição do alerta. É fornecida uma ilustração de exemplo abaixo:

     "etag": "W/\"datetime'2017-12-13T10%3A52%3A21.1697364Z'\"",
      "properties": {
        "Type": "Alert",
        "Name": "test-alert",
        "Description": "I need to put a descriptio here",
        "Threshold": {
          "Operator": "gt",
          "Value": 12
        },
        "AzNsNotification": {
          "GroupIds": [
            "/subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup"
          ]
        },
        "Severity": "critical",
        "Version": 1
      },

Utilize o método Put com um ID exclusivo de ação para associar o grupo de ação já existente para uma agenda.  Segue-se uma ilustração de exemplo de utilização.

    $AzNsJson = "{'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup']} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

Utilize o método Put com um ID de ação existente para modificar um grupo de ação para uma agenda.  O corpo do pedido tem de incluir o etag da ação.

    $AzNsJson = "{'etag': 'datetime'2017-12-13T10%3A52%3A21.1697364Z'\"', properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup']} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

#### <a name="customize-actions"></a>Personalizar Ações
Por ações predefinidas, siga o modelo padrão e o formato para notificações. Mas o utilizador pode personalizar algumas ações, mesmo são controladas por grupos de ação. Atualmente, a personalização for possível para o assunto do E-Mail e o Payload de Webhook.

##### <a name="customize-e-mail-subject-for-action-group"></a>Personalizar o assunto da mensagem de correio eletrónico para o grupo de ação
Por predefinição, é o assunto do correio eletrónico para alertas: notificação de alerta <AlertName> para <WorkspaceName>. Mas isto pode ser personalizado, para que possa palavras específicas ou etiquetas - para que possa recorrer facilmente regras de filtro na pasta a receber. Os detalhes de cabeçalho de e-mail de personalizar necessário enviar juntamente com os detalhes de ActionGroup, tal como no exemplo abaixo.

     "etag": "W/\"datetime'2017-12-13T10%3A52%3A21.1697364Z'\"",
      "properties": {
        "Type": "Alert",
        "Name": "test-alert",
        "Description": "I need to put a descriptio here",
        "Threshold": {
          "Operator": "gt",
          "Value": 12
        },
        "AzNsNotification": {
          "GroupIds": [
            "/subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup"
          ]
          "CustomEmailSubject": "Azure Alert fired"
        },
        "Severity": "critical",
        "Version": 1
      },

Utilize o método Put com um ID exclusivo de ação para associar o grupo de ação já existente com a personalização de uma agenda.  Segue-se uma ilustração de exemplo de utilização.

    $AzNsJson = "{'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup'], 'CustomEmailSubject': 'Azure Alert fired'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

Utilize o método Put com um ID de ação existente para modificar um grupo de ação para uma agenda.  O corpo do pedido tem de incluir o etag da ação.

    $AzNsJson = "{'etag': 'datetime'2017-12-13T10%3A52%3A21.1697364Z'\"', properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup']}, 'CustomEmailSubject': 'Azure Alert fired' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

##### <a name="customize-webhook-payload-for-action-group"></a>Personalizar o Payload de Webhook para o grupo de ação
Por predefinição, o webhook enviado através do grupo de ação de análise de registos tem uma estrutura fixa. Mas um pode personalizar o payload JSON utilizando específicas variáveis suportadas, para satisfazer os requisitos do ponto final webhook. Para obter mais informações, consulte [ação do Webhook para regras de alerta de registo](../monitoring-and-diagnostics/monitor-alerts-unified-log-webhook.md). 

Os detalhes de webhook personalizar tem de enviar, juntamente com os detalhes de ActionGroup e irá ser aplicada a todos os Webhook URI especificado no interior do grupo de ação; tal como no exemplo abaixo.

     "etag": "W/\"datetime'2017-12-13T10%3A52%3A21.1697364Z'\"",
      "properties": {
        "Type": "Alert",
        "Name": "test-alert",
        "Description": "I need to put a descriptio here",
        "Threshold": {
          "Operator": "gt",
          "Value": 12
        },
        "AzNsNotification": {
          "GroupIds": [
            "/subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup"
          ]
          "CustomWebhookPayload": "{\"field1\":\"value1\",\"field2\":\"value2\"}",
          "CustomEmailSubject": "Azure Alert fired"
        },
        "Severity": "critical",
        "Version": 1
      },

Utilize o método Put com um ID exclusivo de ação para associar o grupo de ação já existente com a personalização de uma agenda.  Segue-se uma ilustração de exemplo de utilização.

    $AzNsJson = "{'properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup'], 'CustomEmailSubject': 'Azure Alert fired','CustomWebhookPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

Utilize o método Put com um ID de ação existente para modificar um grupo de ação para uma agenda.  O corpo do pedido tem de incluir o etag da ação.

    $AzNsJson = "{'etag': 'datetime'2017-12-13T10%3A52%3A21.1697364Z'\"', properties': { 'Name': 'test-alert', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 12 },'Severity': 'critical', 'AzNsNotification': {'GroupIds': ['subscriptions/1234a45-123d-4321-12aa-123b12a5678/resourcegroups/my-resource-group/providers/microsoft.insights/actiongroups/test-actiongroup']}, 'CustomEmailSubject': 'Azure Alert fired','CustomWebhookPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}' }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myAzNsaction?api-version=2015-03-20 $AzNsJson

#### <a name="email-notification"></a>Notificação por E-mail
Notificações por e-mail enviam correio para um ou mais destinatários.  Incluem as propriedades na tabela seguinte.

> [!NOTE]
> A partir do dia 23 de Abril de 2018, todos os alertas na área de trabalho irão ser automaticamente expandidos para o Azure. Um utilizador pode iniciar voluntariamente expandir alertas para o Azure antes de 23 de Abril de 2018. Para obter mais informações, consulte [expandir alertas no Azure da OMS](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Para os utilizadores que expandem alertas para o Azure, as ações como notificação por correio electrónico agora são controladas em grupos de ação do Azure. Quando uma área de trabalho e as alertas são expandidas para o Azure, pode obter ou adicionar ações utilizando o [ação grupo API](https://docs.microsoft.com/en-us/rest/api/monitor/actiongroups).
   

| Propriedade | Descrição |
|:--- |:--- |
| Destinatários |Lista de endereços de correio. |
| Assunto |O assunto do correio. |
| Anexo |Anexos não são atualmente suportados, pelo que este terão sempre um valor de "None". |

Segue-se uma resposta de amostra de uma ação de notificação de correio eletrónico com um limiar.  

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My email action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "EmailNotification": {
            "Recipients": [
                "recipient1@contoso.com",
                "recipient2@contoso.com"
            ],
            "Subject": "This is the subject",
            "Attachment": "None"
        },
        "Version": 1
    }

Utilize o método Put com um ID exclusivo de ação para criar uma nova ação de correio eletrónico para uma agenda.  O exemplo seguinte cria uma notificação por e-mail com um limiar para que o correio eletrónico é enviado quando os resultados da pesquisa guardada excedem o limiar.

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

Utilize o método Put com um ID de ação existente para modificar uma ação de correio eletrónico para uma agenda.  O corpo do pedido tem de incluir o etag da ação.

    $emailJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myemailaction?api-version=2015-03-20 $emailJson

#### <a name="remediation-actions"></a>Ações de remediação
Remediações iniciar um runbook na automatização do Azure que tentam corrigir o problema identificado pelo alerta.  Tem de criar um webhook para o runbook utilizado na ação de remediação e, em seguida, especifique o URI na propriedade WebhookUri.  Ao criar esta ação utilizando a consola do OMS, é criado automaticamente um novo webhook para o runbook.

> [!NOTE]
> A partir do dia 23 de Abril de 2018, todos os alertas na área de trabalho irão ser automaticamente expandidos para o Azure. Um utilizador pode iniciar voluntariamente expandir alertas para o Azure antes de 23 de Abril de 2018. Para obter mais informações, consulte [expandir alertas no Azure da OMS](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Para os utilizadores que expandem alertas para o Azure, as ações como remediação utilizando o runbook agora são controladas em grupos de ação do Azure. Quando uma área de trabalho e as alertas são expandidas para o Azure, pode obter ou adicionar ações utilizando o [ação grupo API](https://docs.microsoft.com/en-us/rest/api/monitor/actiongroups).

Remediações incluem as propriedades na tabela seguinte.

| Propriedade | Descrição |
|:--- |:--- |
| RunbookName |Nome do runbook. Isto deve corresponder ao runbook publicado na conta de automatização configurado na solução de automatização na sua área de trabalho do OMS. |
| WebhookUri |URI do webhook. |
| Validade |A data de expiração e hora do webhook.  Se o webhook não tiver uma expiração, em seguida, isto pode ser qualquer data futura válida. |

Segue-se uma resposta de amostra de uma ação de remediação com um limiar.

    "etag": "W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"",
    "properties": {
        "Type": "Alert",
        "Name": "My remediation action",
        "Threshold": {
            "Operator": "gt",
            "Value": 10
        },
        "Remediation": {
            "RunbookName": "My-Runbook",
            "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d",
            "Expiry": "2018-02-25T18:27:20"
            },
        "Version": 1
    }

Utilize o método Put com um ID exclusivo de ação para criar uma nova ação de remediação para uma agenda.  O exemplo seguinte cria uma remediação com um limiar para que o runbook é iniciado quando os resultados da pesquisa guardada excedem o limiar.

    $remediateJson = "{'properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

Utilize o método Put com um ID de ação existente para modificar uma ação de remediação para uma agenda.  O corpo do pedido tem de incluir o etag da ação.

    $remediateJson = "{'etag': 'W/\"datetime'2016-02-25T20%3A54%3A20.1302566Z'\"','properties': { 'Type':'Alert', 'Name': 'My Remediation Action', 'Version':'1', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'Remediation': {'RunbookName': 'My-Runbook', 'WebhookUri':'https://s1events.azure-automation.net/webhooks?token=4jCibOjO3w4W2Cfg%2b2NkjLYdafnusaG6i8tnP8h%2fNNg%3d', 'Expiry':'2018-02-25T18:27:20Z'} }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/myremediationaction?api-version=2015-03-20 $remediateJson

#### <a name="example"></a>Exemplo
Segue-se um exemplo completo para criar um novo alerta de e-mail.  Esta ação cria uma nova agenda juntamente com uma ação que contém um limiar e e-mail.

    $subscriptionId = "3d56705e-5b26-5bcc-9368-dbc8d2fafbfc"
    $resourceGroup  = "MyResourceGroup"    
    $workspaceName    = "MyWorkspace"
    $searchId       = "MySearch"
    $scheduleId     = "MySchedule"
    $thresholdId    = "MyThreshold"
    $actionId       = "MyEmailAction"

    $scheduleJson = "{'properties': { 'Interval': 15, 'QueryTimeSpan':15, 'Active':'true' }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/?api-version=2015-03-20 $scheduleJson

    $emailJson = "{'properties': { 'Name': 'MyEmailAction', 'Version':'1', 'Severity':'Warning', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 }, 'EmailNotification': {'Recipients': ['recipient1@contoso.com', 'recipient2@contoso.com'], 'Subject':'This is the subject', 'Attachment':'None'} }"
    armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/savedSearches/$searchId/schedules/$scheduleId/actions/$actionId/?api-version=2015-03-20 $emailJson

#### <a name="webhook-actions"></a>Ações de Webhook
As ações de Webhook iniciar um processo ao chamar um URL e, opcionalmente, fornecer um payload de envio.  Estes são semelhantes às ações de remediação, exceto se destinam para webhooks que pode invocar processos que não sejam runbooks de automatização do Azure.  Também fornecem a opção adicional de fornecer um payload a entregar o processo remoto.

> [!NOTE]
> A partir do dia 23 de Abril de 2018, todos os alertas na área de trabalho irão ser automaticamente expandidos para o Azure. Um utilizador pode iniciar voluntariamente expandir alertas para o Azure antes de 23 de Abril de 2018. Para obter mais informações, consulte [expandir alertas no Azure da OMS](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Para os utilizadores que expandem alertas para o Azure, as ações como Webhook agora são controladas em grupos de ação do Azure. Quando uma área de trabalho e as alertas são expandidas para o Azure, pode obter ou adicionar ações utilizando o [ação grupo API](https://docs.microsoft.com/en-us/rest/api/monitor/actiongroups).


As ações de Webhook não dispõe de um limiar, mas em vez disso, devem ser adicionadas a uma agenda que tem uma ação com um limiar de alerta.  

Segue-se uma resposta de exemplo para a ação de webhook e uma ação de alerta associada com um limiar.

    {
        "__metadata": {},
        "value": [
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/72884702-acf9-4653-bb67-f42436b342b4",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"",
                "properties": {
                    "Type": "Webhook",
                    "Name": "My Webhook Action",
                    "WebhookUri": "https://oaaswebhookdf.cloudapp.net/webhooks?token=VfkYTIlpk%2fc%2bJBP",
                    "CustomPayload": "{\"fielld1\":\"value1\",\"field2\":\"value2\"}",
                    "Version": 1
                }
            },
            {
                "id": "subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/bwren/savedSearches/2d1b30fb-7f48-4de5-9614-79ee244b52de/schedules/b80f5621-7217-4007-b32d-165d14377093/Actions/90a27cf8-71b7-4df2-b04f-54ed01f1e4b6",
                "etag": "W/\"datetime'2016-02-26T20%3A25%3A00.565204Z'\"",
                "properties": {
                    "Type": "Alert",
                    "Name": "Threshold for my webhook action",
                    "Threshold": {
                        "Operator": "gt",
                        "Value": 10
                    },
                    "Version": 1
                }
            }
        ]
    }

##### <a name="create-or-edit-a-webhook-action"></a>Criar ou editar uma ação do webhook
Utilize o método Put com um ID exclusivo de ação para criar uma nova ação de webhook para uma agenda.  O exemplo seguinte cria uma ação de Webhook e uma ação de alerta com um limiar para que o webhook será acionado quando os resultados da pesquisa guardada excedem o limiar.

    $thresholdAction = "{'properties': { 'Name': 'My Threshold', 'Version':'1', 'Type':'Alert', 'Threshold': { 'Operator': 'gt', 'Value': 10 } }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mythreshold?api-version=2015-03-20 $thresholdAction

    $webhookAction = "{'properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction

Utilize o método Put com um ID de ação existente para modificar uma ação do webhook para uma agenda.  O corpo do pedido tem de incluir o etag da ação.

    $webhookAction = "{'etag': 'W/\"datetime'2016-02-26T20%3A25%3A00.6862124Z'\"','properties': {'Type': 'Webhook', 'Name': 'My Webhook", 'WebhookUri': 'https://oaaswebhookdf.cloudapp.net/webhooks?token=VrkYTKlhk%2fc%2bKBP', 'CustomPayload': '{\"field1\":\"value1\",\"field2\":\"value2\"}', 'Version': 1 }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Search ID}/schedules/{Schedule ID}/actions/mywebhookaction?api-version=2015-03-20 $webhookAction


## <a name="next-steps"></a>Passos Seguintes
* Utilize o [API REST para efetuar pesquisas de registo](log-analytics-log-search-api.md) na análise de registos.
* Saiba mais sobre [registar alertas nos alertas do azure](../monitoring-and-diagnostics/monitor-alerts-unified-log.md)

