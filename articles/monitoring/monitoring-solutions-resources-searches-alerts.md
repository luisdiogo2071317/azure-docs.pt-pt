---
title: Guardar pesquisas e os alertas em soluções de gestão | Microsoft Docs
description: As soluções de gestão incluem, geralmente, as pesquisas guardadas na análise de registos para analisar os dados recolhidos pela solução.  Estes podem também definir alertas para notificar o utilizador ou automaticamente tomar medidas em resposta a um problema crítico.  Este artigo descreve como definir a análise de registos guardar pesquisas e os alertas num modelo do Resource Manager pode ser incluídos em soluções de gestão.
services: monitoring
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/18/2018
ms.author: bwren, vinagara
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c29d6cb0da2e394912a2584b0d3c3cedf13f054c
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36304487"
---
# <a name="adding-log-analytics-saved-searches-and-alerts-to-management-solution-preview"></a>A adição de análise de registos guardar pesquisas e os alertas à solução de gestão (pré-visualização)

> [!NOTE]
> Esta documentação é de preliminar para criar soluções de gestão que estão atualmente em pré-visualização. Nenhum esquema descrita abaixo está sujeita a alterações.   


[As soluções de gestão](monitoring-solutions.md) normalmente incluirá a [pesquisas guardadas](../log-analytics/log-analytics-log-searches.md) na análise de registos para analisar os dados recolhidos pela solução.  Pode também definirem [alertas](../log-analytics/log-analytics-alerts.md) para notificar o utilizador ou automaticamente tomar medidas em resposta a um problema crítico.  Este artigo descreve como definir a análise de registos pesquisas guardadas e alertas num [modelo de gestão de recursos](../resource-manager-template-walkthrough.md) pelo que pode ser incluídos numa [soluções de gestão](monitoring-solutions-creating.md).

> [!NOTE]
> Os exemplos neste artigo utilizam parâmetros e variáveis que são necessários ou comuns para soluções de gestão e descrito em [estruturação e criação de uma solução de gestão no Azure](monitoring-solutions-creating.md)  

## <a name="prerequisites"></a>Pré-requisitos
Este artigo pressupõe que já está familiarizado com a [criar uma solução de gestão](monitoring-solutions-creating.md) e a estrutura de um [modelo do Resource Manager](../resource-group-authoring-templates.md) e ficheiro de solução.


## <a name="log-analytics-workspace"></a>Área de trabalho do Log Analytics
Todos os recursos na análise de registos estão contidos num [área de trabalho](../log-analytics/log-analytics-manage-access.md).  Conforme descrito em [área de trabalho de análise de registos e a conta de automatização](monitoring-solutions.md#log-analytics-workspace-and-automation-account), a área de trabalho não está incluída na solução de gestão, mas tem de existir antes da solução está instalada.  Se não estiver disponível, em seguida, a solução de instalação irá falhar.

O nome da área de trabalho é nome cada recurso de análise de registos.  Isto é feito na solução com o **área de trabalho** parâmetro como no seguinte exemplo de um recurso de SavedSearch.

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"

## <a name="log-analytics-api-version"></a>Versão de API de análise do registo
Todos os recursos de análise de registos definidos num modelo do Resource Manager tem uma propriedade **apiVersion** que define a versão da API, deve utilizar o recurso.   

A tabela seguinte apresenta a versão da API para o recurso utilizado neste exemplo.

| Tipo de recurso | Versão de API | Consulta |
|:---|:---|:---|
| savedSearches | 2017-03-15-preview | Evento &#124; onde EventLevelName = = "Erro"  |


## <a name="saved-searches"></a>Procuras Guardadas
Incluir [pesquisas guardadas](../log-analytics/log-analytics-log-searches.md) numa solução para permitir aos utilizadores consultar os dados recolhidos pela sua solução.  Guardar pesquisas são apresentados em **Favoritos** no portal do OMS e **pesquisas guardadas** no portal do Azure.  Uma pesquisa guardada também é necessária para cada alerta.   

[Análise de registos guardar pesquisa](../log-analytics/log-analytics-log-searches.md) recursos tem um tipo de `Microsoft.OperationalInsights/workspaces/savedSearches` e ter a seguinte estrutura.  Isto inclui os parâmetros e variáveis comuns para que possam copie e cole o fragmento de código no seu ficheiro de solução e alterar os nomes de parâmetro. 

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
        ],
        "tags": { },
        "properties": {
            "etag": "*",
            "query": "[variables('SavedSearch').Query]",
            "displayName": "[variables('SavedSearch').DisplayName]",
            "category": "[variables('SavedSearch').Category]"
        }
    }



Cada propriedade de uma procura guardada é descrita na seguinte tabela. 

| Propriedade | Descrição |
|:--- |:--- |
| categoria | A categoria para a pesquisa guardada.  As pesquisas guardadas na mesma solução frequentemente irão partilhar uma única categoria para são agrupados em conjunto na consola do. |
| DisplayName | Nome a apresentar para a pesquisa guardada no portal. |
| consulta | Consulta seja executada. |

> [!NOTE]
> Terá de utilizar os carateres de escape da consulta se este incluir carateres que poderão ser interpretados como JSON.  Por exemplo, se a consulta foi **tipo: AzureActivity OperationName:"Microsoft.Compute/virtualMachines/write"**, devem ser escrita no ficheiro de solução como **tipo: AzureActivity OperationName:\" Microsoft.Compute/virtualMachines/write\"**.

## <a name="alerts"></a>Alertas
[Alertas de registo do Azure](../monitoring-and-diagnostics/monitor-alerts-unified-log.md) são criados pelas regras de alertas do Azure que executar consultas de registo especificado em intervalos regulares.  Se os resultados da consulta correspondem aos critérios especificados, é criado um registo de alerta e uma ou mais ações são executadas utilizando [ação grupos](../monitoring-and-diagnostics/monitoring-action-groups.md).  

> [!NOTE]
> A partir de 14 de Maio de 2018, todos os alertas na área de trabalho serão iniciada automaticamente expandir no Azure. Um utilizador pode iniciar voluntariamente expandir alertas para o Azure antes de 14 de Maio de 2018. Para obter mais informações, consulte [expandir alertas no Azure da OMS](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Para os utilizadores que expandem alertas para o Azure, as ações são agora controladas em grupos de ação do Azure. Quando uma área de trabalho e as alertas são expandidas para o Azure, pode obter ou adicionar ações utilizando o [ação grupo - modelo do Azure Resource Manager](../monitoring-and-diagnostics/monitoring-create-action-group-with-resource-manager-template.md).

Regras de alerta numa solução de gestão são efetuadas cópias de segurança dos seguintes três recursos diferentes.

- **Pesquisa guardada.**  Define a pesquisa de registo que é executada.  Várias regras de alerta podem partilhar uma procura guardada único.
- **Agenda.**  Define a frequência a pesquisa de registo é executada.  Cada regra de alerta tem um e apenas um agendamento.
- **Ação de alerta.**  Cada regra de alerta tem um recurso do grupo de ação ou ação recursos (Legado) com um tipo de **alerta** que define os detalhes do alerta, tais como os critérios quando é criado um registo de alerta e a gravidade do alerta. [Grupo de ação](../monitoring-and-diagnostics/monitoring-action-groups.md) recursos podem ter uma lista de ações configuradas a tomar quando o alerta é desencadeado - por exemplo, a chamada de voz, SMS, e-mail, webhook ferramenta ITSM, runbook de automatização, aplicação lógica, etc.
 
O recurso de ação (Legado), opcionalmente, vai definir uma resposta de correio e o runbook.
- **Ação do Webhook (Legado).**  Se a regra de alerta chama um webhook, em seguida, necessita de um recurso de ação adicional com um tipo de **Webhook**.    

Guardar pesquisa recursos são descritos acima.  Os outros recursos são descritos abaixo.


### <a name="schedule-resource"></a>Recurso de agenda

Uma pesquisa guardada pode ter uma ou mais agendas com cada agenda que representa uma regra de alerta separada. A agenda define a frequência a pesquisa está a ser executada e o intervalo de tempo durante o qual os dados são obtidos.  Recursos de agendamento tem um tipo de `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` e ter a seguinte estrutura. Isto inclui os parâmetros e variáveis comuns para que possam copie e cole o fragmento de código no seu ficheiro de solução e alterar os nomes de parâmetro. 


    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name)]"
        ],
        "properties": {
            "etag": "*",
            "interval": "[variables('Schedule').Interval]",
            "queryTimeSpan": "[variables('Schedule').TimeSpan]",
            "enabled": "[variables('Schedule').Enabled]"
        }
    }



As propriedades de recursos de agenda são descritas na seguinte tabela.

| Nome do elemento | Necessário | Descrição |
|:--|:--|:--|
| enabled       | Sim | Especifica se o alerta é ativado quando é criado. |
| intervalo      | Sim | Frequência de consulta é executada em minutos. |
| queryTimeSpan | Sim | Período de tempo em minutos durante o qual a avaliar os resultados. |

O recurso de agenda deve dependem a pesquisa guardada para que é criado antes da agenda.

> [!NOTE]
> Nome da agenda tem de ser exclusivo numa área de trabalho especificada; duas agendas não podem ter o mesmo ID, mesmo que se encontrem associados com diferentes pesquisas guardadas. Também o nome de pesquisas guardadas todos os, agendas e criadas com a API de análise do registo de ações deve ser em minúsculas.


### <a name="actions"></a>Ações
Uma agenda pode ter várias ações. Uma ação pode definir uma ou mais processos para efetuar como enviar uma mensagem ou iniciar um runbook ou pode definir um limiar que determina quando os resultados de uma procura correspondem alguns critérios.  Algumas ações vai definir ambos para que os processos são efetuados quando é cumprido o limiar.

Ações podem ser definidas utilizando recursos [grupo de ação] ou ao recurso de ação.

> [!NOTE]
> A partir de 14 de Maio de 2018, todos os alertas na área de trabalho serão iniciada automaticamente expandir no Azure. Um utilizador pode iniciar voluntariamente expandir alertas para o Azure antes de 14 de Maio de 2018. Para obter mais informações, consulte [expandir alertas no Azure da OMS](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Para os utilizadores que expandem alertas para o Azure, as ações são agora controladas em grupos de ação do Azure. Quando uma área de trabalho e as alertas são expandidas para o Azure, pode obter ou adicionar ações utilizando o [ação grupo - modelo do Azure Resource Manager](../monitoring-and-diagnostics/monitoring-create-action-group-with-resource-manager-template.md).


Existem dois tipos de recursos da ação especificado pelo **tipo** propriedade.  Uma agenda requer um **alerta** ação, que define os detalhes da regra de alerta que ações são efetuadas quando é criado um alerta. Recursos da ação tem um tipo de `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.  

Ações de alerta tem a seguinte estrutura.  Isto inclui os parâmetros e variáveis comuns para que possam copie e cole o fragmento de código no seu ficheiro de solução e alterar os nomes de parâmetro. 


```
    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Alert').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
        ],
        "properties": {
            "etag": "*",
            "type": "Alert",
            "name": "[variables('Alert').Name]",
            "description": "[variables('Alert').Description]",
            "severity": "[variables('Alert').Severity]",
            "threshold": {
                "operator": "[variables('Alert').Threshold.Operator]",
                "value": "[variables('Alert').Threshold.Value]",
                "metricsTrigger": {
                    "triggerCondition": "[variables('Alert').Threshold.Trigger.Condition]",
                    "operator": "[variables('Alert').Trigger.Operator]",
                    "value": "[variables('Alert').Trigger.Value]"
                  },
              },
      "AzNsNotification": {
        "GroupIds": "[variables('MyAlert').AzNsNotification.GroupIds]",
        "CustomEmailSubject": "[variables('MyAlert').AzNsNotification.CustomEmailSubject]",
        "CustomWebhookPayload": "[variables('MyAlert').AzNsNotification.CustomWebhookPayload]"
        }
        }
    }
```

As propriedades de recursos da ação de alerta são descritas nas tabelas seguintes.

| Nome do elemento | Necessário | Descrição |
|:--|:--|:--|
| Tipo | Sim | Tipo de ação.  Este é **alerta** para ações de alerta. |
| Nome | Sim | Nome a apresentar para o alerta.  Este é o nome que é apresentado na consola para a regra de alerta. |
| Descrição | Não | Descrição opcional do alerta. |
| Gravidade | Sim | Gravidade do alerta registo entre os valores seguintes:<br><br> **Crítico**<br>**aviso**<br>**Informativo**


#### <a name="threshold"></a>Limiar
Esta secção é necessária.  Define as propriedades para o limiar de alerta.

| Nome do elemento | Necessário | Descrição |
|:--|:--|:--|
| Operador | Sim | Operador de comparação entre os valores seguintes:<br><br>**gt = maior<br>lt = inferior a** |
| Valor | Sim | O valor a comparar os resultados. |

##### <a name="metricstrigger"></a>MetricsTrigger
Esta secção é opcional.  Incluí-la para um alerta de métrica de medida.

> [!NOTE]
> Alertas de medida métrica estão atualmente em pré-visualização pública. 

| Nome do elemento | Necessário | Descrição |
|:--|:--|:--|
| TriggerCondition | Sim | Especifica se o limiar de número total de falhas ou falhas consecutivas entre os valores seguintes:<br><br>**Total<br>consecutivas** |
| Operador | Sim | Operador de comparação entre os valores seguintes:<br><br>**gt = maior<br>lt = inferior a** |
| Valor | Sim | Número de vezes que os critérios têm de ser cumpridos para acionar o alerta. |


#### <a name="throttling"></a>Limitação
Esta secção é opcional.  Inclua esta secção se pretende suprimir os alertas da mesma regra algum período de tempo depois de criar um alerta.

| Nome do elemento | Necessário | Descrição |
|:--|:--|:--|
| DurationInMinutes | Sim, se a limitação de elemento incluído | Número de minutos para suprimir alertas depois da mesma regra de alerta ser criado. |


#### <a name="azure-action-group"></a>Grupo de ação do Azure
Todos os alertas no Azure, utilize a ação grupo como o mecanismo predefinido para processamento de ações. Com o grupo de ação, pode especificar as suas ações uma vez e, em seguida, associar o grupo de ação para vários alertas - através do Azure. Sem a necessidade, repetidamente declarar as mesmas ações repetidas. Grupos de ação suportam várias ações - incluindo e-mail, SMS, chamada de voz, ITSM ligação, o Runbook de automatização, URI de Webhook e muito mais. 

Para o utilizador que tiver expandido o respetivas alertas no Azure - uma agenda agora deve ter os detalhes de ação grupo transmitidos juntamente com o limiar, para conseguir criar um alerta. Detalhes da mensagem de correio eletrónico, URLs de Webhook, detalhes de automatização de Runbook e outras ações, tem de ser definido no lado, um grupo de ação primeiro antes a criar um alerta; um pode criar [ação grupo Azure monitor](../monitoring-and-diagnostics/monitoring-action-groups.md) no Portal ou utilize [ação grupo - modelo do Resource](../monitoring-and-diagnostics/monitoring-create-action-group-with-resource-manager-template.md).

| Nome do elemento | Necessário | Descrição |
|:--|:--|:--|
| AzNsNotification | Sim | O ID de recurso do grupo de ação do Azure associada alerta para colocar as ações necessárias quando critérios de alerta são cumpridos. |
| CustomEmailSubject | Não | Linha de assunto personalizado do correio enviado para todos os endereços especificados no grupo de ação associada. |
| CustomWebhookPayload | Não | Payload personalizado sejam enviados para todos os pontos finais de webhook definidos num grupo de ação associada. O formato depende o webhook que está à espera e deve ser um JSON serializado válido. |


#### <a name="actions-for-oms-legacy"></a>Ações para OMS (legados)

Cada agenda tem um **alerta** ação.  Isto define os detalhes do alerta e, opcionalmente, as ações de notificação e remediação.  Uma notificação envia um e-mail para um ou vários endereços.  Uma remediação inicia um runbook na automatização do Azure para tentar corrigir o problema detetado.

> [!NOTE]
> A partir de 14 de Maio de 2018, todos os alertas na área de trabalho serão iniciada automaticamente expandir no Azure. Um utilizador pode iniciar voluntariamente expandir alertas para o Azure antes de 14 de Maio de 2018. Para obter mais informações, consulte [expandir alertas no Azure da OMS](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Para os utilizadores que expandem alertas para o Azure, as ações são agora controladas em grupos de ação do Azure. Quando uma área de trabalho e as alertas são expandidas para o Azure, pode obter ou adicionar ações utilizando o [ação grupo - modelo do Azure Resource Manager](../monitoring-and-diagnostics/monitoring-create-action-group-with-resource-manager-template.md).

##### <a name="emailnotification"></a>EmailNotification
 Esta secção é opcional incluí-la se pretender que o alerta para enviar correio para um ou mais destinatários.

| Nome do elemento | Necessário | Descrição |
|:--|:--|:--|
| Destinatários | Sim | Lista delimitada por vírgulas de endereços de e-mail para enviar notificações quando é criado um alerta, tal como no exemplo seguinte.<br><br>**[ "recipient1@contoso.com", "recipient2@contoso.com" ]** |
| Assunto | Sim | Linha de assunto de correio. |
| Anexo | Não | Anexos não são atualmente suportados.  Se este elemento está incluído, deve ser **nenhum**. |


##### <a name="remediation"></a>Remediação
Esta secção é opcional incluí-la se pretender que um runbook para começar em resposta ao alerta. |

| Nome do elemento | Necessário | Descrição |
|:--|:--|:--|
| RunbookName | Sim | Nome do runbook para iniciar. |
| WebhookUri | Sim | URI de webhook para o runbook. |
| Validade | Não | Data e hora de expiração a remediação. |

##### <a name="webhook-actions"></a>Ações de Webhook

As ações de Webhook iniciar um processo ao chamar um URL e, opcionalmente, fornecer um payload de envio. Estes são semelhantes às ações de remediação, exceto se destinam para webhooks que pode invocar processos que não sejam runbooks de automatização do Azure. Também fornecem a opção adicional de fornecer um payload a entregar o processo remoto.

Se o alerta irá chamar um webhook, em seguida, será necessário um recurso de ação com um tipo de **Webhook** para além de **alerta** recursos da ação.  

    {
      "name": "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Webhook').Name)]",
      "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions/",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
      ],
      "properties": {
        "etag": "*",
        "type": "[variables('Alert').Webhook.Type]",
        "name": "[variables('Alert').Webhook.Name]",
        "webhookUri": "[variables('Alert').Webhook.webhookUri]",
        "customPayload": "[variables('Alert').Webhook.CustomPayLoad]"
      }
    }

As propriedades de recursos de ação do Webhook são descritas nas tabelas seguintes.

| Nome do elemento | Necessário | Descrição |
|:--|:--|:--|
| tipo | Sim | Tipo de ação.  Este é **Webhook** para ações de webhook. |
| nome | Sim | Nome a apresentar para a ação.  Não é apresentado na consola do. |
| wehookUri | Sim | URI para o webhook. |
| CustomPayload | Não | Payload personalizado sejam enviados para o webhook. O formato depende do que o webhook está à espera. |


## <a name="sample"></a>Sample

Segue-se um exemplo de uma solução que inclui o que inclui os seguintes recursos:

- Pesquisa guardada
- Agenda
- Grupo de ação

Este exemplo utiliza [parâmetros de solução padrão]( monitoring-solutions-solution-file.md#parameters) variáveis que frequentemente deverá ser utilizadas numa solução, por oposição a valores de codificar nas definições de recursos.

```
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0",
        "parameters": {
          "workspaceName": {
            "type": "string",
            "metadata": {
              "Description": "Name of Log Analytics workspace"
            }
          },
          "workspaceregionId": {
            "type": "string",
            "metadata": {
              "Description": "Region of Log Analytics workspace"
            }
          },
          "actiongroup": {
            "type": "string",
            "metadata": {
              "Description": "List of action groups for alert actions separated by semicolon"
            }
          }
        },
        "variables": {
          "SolutionName": "MySolution",
          "SolutionVersion": "1.0",
          "SolutionPublisher": "Contoso",
          "ProductName": "SampleSolution",
    
          "LogAnalyticsApiVersion-Search": "2017-03-15-preview",
              "LogAnalyticsApiVersion-Solution": "2015-11-01-preview",

          "MySearch": {
            "displayName": "Error records by hour",
            "query": "MyRecord_CL | summarize AggregatedValue = avg(Rating_d) by Instance_s, bin(TimeGenerated, 60m)",
            "category": "Samples",
            "name": "Samples-Count of data"
          },
          "MyAlert": {
            "Name": "[toLower(concat('myalert-',uniqueString(resourceGroup().id, deployment().name)))]",
            "DisplayName": "My alert rule",
            "Description": "Sample alert.  Fires when 3 error records found over hour interval.",
            "Severity": "critical",
            "ThresholdOperator": "gt",
            "ThresholdValue": 3,
            "Schedule": {
              "Name": "[toLower(concat('myschedule-',uniqueString(resourceGroup().id, deployment().name)))]",
              "Interval": 15,
              "TimeSpan": 60
            },
            "MetricsTrigger": {
              "TriggerCondition": "Consecutive",
              "Operator": "gt",
              "Value": 3
            },
            "ThrottleMinutes": 60,
            "AzNsNotification": {
              "GroupIds": [
                "[parameters('actiongroup')]"
              ],
              "CustomEmailSubject": "Sample alert"
            }
          }
        },
        "resources": [
          {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
            "location": "[parameters('workspaceRegionId')]",
            "tags": { },
            "type": "Microsoft.OperationsManagement/solutions",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Solution')]",
            "dependsOn": [
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
            ],
            "properties": {
              "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
              "referencedResources": [
              ],
              "containedResources": [
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]"
              ]
            },
            "plan": {
              "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
              "Version": "[variables('SolutionVersion')]",
              "product": "[variables('ProductName')]",
              "publisher": "[variables('SolutionPublisher')]",
              "promotionCode": ""
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [ ],
            "tags": { },
            "properties": {
              "etag": "*",
              "query": "[variables('MySearch').query]",
              "displayName": "[variables('MySearch').displayName]",
              "category": "[variables('MySearch').category]"
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name)]"
            ],
            "properties": {
              "etag": "*",
              "interval": "[variables('MyAlert').Schedule.Interval]",
              "queryTimeSpan": "[variables('MyAlert').Schedule.TimeSpan]",
              "enabled": true
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/',  variables('MyAlert').Schedule.Name, '/',  variables('MyAlert').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion-Search')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/',  variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]"
            ],
            "properties": {
              "etag": "*",
              "Type": "Alert",
              "Name": "[variables('MyAlert').DisplayName]",
              "Description": "[variables('MyAlert').Description]",
              "Severity": "[variables('MyAlert').Severity]",
              "Threshold": {
                "Operator": "[variables('MyAlert').ThresholdOperator]",
                "Value": "[variables('MyAlert').ThresholdValue]",
                "MetricsTrigger": {
                  "TriggerCondition": "[variables('MyAlert').MetricsTrigger.TriggerCondition]",
                  "Operator": "[variables('MyAlert').MetricsTrigger.Operator]",
                  "Value": "[variables('MyAlert').MetricsTrigger.Value]"
                }
              },
              "Throttling": {
                "DurationInMinutes": "[variables('MyAlert').ThrottleMinutes]"
              },
            "AzNsNotification": {
              "GroupIds": "[variables('MyAlert').AzNsNotification.GroupIds]",
              "CustomEmailSubject": "[variables('MyAlert').AzNsNotification.CustomEmailSubject]"
            }             
            }
          }
        ]
    }
```

O ficheiro de parâmetros seguinte fornece valores de exemplos para esta solução.
```
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspacename": {
                "value": "myWorkspace"
            },
            "accountName": {
                "value": "myAccount"
            },
            "workspaceregionId": {
                "value": "East US"
            },
            "regionId": {
                "value": "East US 2"
            },
            "pricingTier": {
                "value": "Free"
            },
            "actiongroup": {
                "value": "/subscriptions/3b540246-808d-4331-99aa-917b808a9166/resourcegroups/myTestGroup/providers/microsoft.insights/actiongroups/sample"
            }
        }
    }
```

## <a name="next-steps"></a>Passos Seguintes
* [Adicionar vistas](monitoring-solutions-resources-views.md) à sua solução de gestão.
* [Adicionar runbooks de automatização e outros recursos](monitoring-solutions-resources-automation.md) à sua solução de gestão.

