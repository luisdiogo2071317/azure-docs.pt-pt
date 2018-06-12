---
title: Expandir a alertas de Analytcs de registo para o Azure
description: Este artigo descreve as ferramentas e a API através do qual pode expandir a alertas de análise de registos para alertas do Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 21ba95a7b3efff177afe63d22da3f6ba9848ded2
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301036"
---
# <a name="extend-alerts-from-log-analytics-into-azure-alerts"></a>Expandir a alertas de análise de registos para alertas do Azure
A funcionalidade de alertas no Log Analytics do Azure está a ser substituída pelo alertas do Azure. Como parte desta transição, alertas que configurou originalmente no Log Analytics irão ser expandidos para o Azure. Se não pretender aguardar para ser automaticamente movido para o Azure, pode iniciar o processo de:

- Manualmente a partir do portal do Operations Management Suite. 
- Através de programação, utilizando a API de AlertsVersion.  

> [!NOTE]
> Microsoft irá expandir automaticamente alertas criados na análise de registos para alertas do Azure, a partir no 14 de Maio de 2018, uma série periódica até concluído. As agendas de Microsoft migrar os alertas para o Azure e, durante este transição alertas podem ser geridos do portal do Operations Management Suite e o portal do Azure. Este processo não é destrutivas ou interruptive.  

## <a name="option-1-initiate-from-the-operations-management-suite-portal"></a>Opção 1: Iniciar partir do portal do Operations Management Suite
Os passos seguintes descrevem como expandir alertas para a área de trabalho a partir do portal do Operations Management Suite.  

1. No portal do Azure, selecione **todos os serviços**. Na lista de recursos, escreva **Log Analytics**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **Log Analytics**.
2. No painel de subscrições de análise de registos, selecione uma área de trabalho e, em seguida, selecione o **Portal do OMS** mosaico.
![Painel de subscrição de captura de ecrã da análise de registos, com o mosaico do Portal do OMS realçado](./media/monitor-alerts-extend/azure-portal-01.png) 
3. Depois de são redirecionados para o portal do Operations Management Suite, selecione o **definições** ícone.
![Portal de captura de ecrã do Operations Management Suite, com ícone de definições realçado](./media/monitor-alerts-extend/oms-portal-settings-option.png) 
4. Do **definições** página, selecione **alertas**.  
5. Selecione **expandir no Azure**.
![Captura de ecrã do Operations Management Suite definições de alerta página do portal, com expanda no Azure realçado](./media/monitor-alerts-extend/ExtendInto.png)
6. É apresentado um Assistente de três passos no **alertas** painel. Descrição geral de leitura e selecione **seguinte**.
![Captura de ecrã do passo 1 do Assistente](./media/monitor-alerts-extend/ExtendStep1.png)  
7. O segundo passo, é apresentado um resumo das alterações propostas, listagem adequado [grupos ação](monitoring-action-groups.md) para os alertas. Se a ações semelhantes são vistas através dos mais do que um alerta, o assistente proposto associar um grupo de única ação todos eles.  A Convenção de nomenclatura é o seguinte: *WorkspaceName_AG_ #Number*. Para continuar, selecione **seguinte**.
![Captura de ecrã do passo 2 do Assistente](./media/monitor-alerts-extend/ExtendStep2.png)  
8. No último passo do assistente, selecione **concluir**e confirme quando lhe for pedido para iniciar o processo. Opcionalmente, pode fornecer um endereço de e-mail, para que são notificados quando o processo ser concluído e todos os alertas tem sido movidos com êxito para alertas do Azure.
![Captura de ecrã do passo 3 do Assistente](./media/monitor-alerts-extend/ExtendStep3.png)

Quando o assistente estiver concluído, no **definições de alerta** página, a opção de expandir alertas para o Azure é removida. Em segundo plano, os alertas são movidos para o Azure e isto pode demorar algum tempo. Durante a operação, não é possível efetuar alterações aos alertas do portal do Operations Management Suite. Pode ver o estado atual da faixa na parte superior do portal. Se forneceu anteriormente um endereço de e-mail, receberá um e-mail quando o processo foi concluído com êxito.  


Alertas continuam a ser apresentados no portal do Operations Management Suite, mesmo depois de estes são movidas com êxito no Azure.
![Página de definições de alerta portal de captura de ecrã do Operations Management Suite](./media/monitor-alerts-extend/PostExtendList.png)


## <a name="option-2-use-the-alertsversion-api"></a>Opção 2: Utilizar a API de AlertsVersion
Pode utilizar a API de AlertsVersion de análise do registo para expandir alertas de análise de registos para alertas do Azure a partir de qualquer cliente que possa chamar uma API REST. Pode aceder a API a partir do PowerShell utilizando [ARMClient](https://github.com/projectkudu/ARMClient), uma ferramenta de linha de comandos de open source. Pode enviar os resultados no JSON.  

Para utilizar a API, primeiro crie um pedido GET. Isto avalia e devolve um resumo das alterações propostas, antes de tentar expandir realmente no Azure através da utilização de um pedido POST. Os resultados listam os alertas e uma lista de propostas [grupos ação](monitoring-action-groups.md), no formato JSON. Se a ações semelhantes são vistas através dos mais do que um alerta, o serviço proposto todos eles associar um grupo de única ação. A Convenção de nomenclatura é o seguinte: *WorkspaceName_AG_ #Number*.

```
armclient GET  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Se o pedido GET for bem sucedido, um código de estado HTTP 200 é devolvido, juntamente com uma lista de alertas e proposto grupos de ação de dados JSON. Segue-se um exemplo de resposta:

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
Se a área de trabalho especificada não tem quaisquer regras de alerta definidas, os dados JSON devolve o seguinte:

```json
{
    "version": 1,
    "Message": "No Alerts found in the workspace for migration."
}
```

Se a todas as regras de alerta na área de trabalho especificada já tem sido expandidas para o Azure, a resposta ao pedido GET é:

```json
{
    "version": 2
}
```

Para iniciar a migrar os alertas para o Azure, inicie uma resposta POST. A resposta POST confirma que a sua intenção, bem como aceitação, ter alertas expandidas a partir da análise de registos para alertas do Azure. A atividade está agendada e os alertas são processados como indicado, com base nos resultados quando efetuar a resposta GET anteriormente. Opcionalmente, pode fornecer uma lista de endereços de e-mail para o qual Log Analytics envia um relatório quando o processo em segundo plano agendada de migrar os alertas for concluída com êxito. Pode utilizar o seguinte exemplo de pedido:

```
$emailJSON = “{‘Recipients’: [‘a@b.com’, ‘b@a.com’]}”
armclient POST  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $emailJSON
```

> [!NOTE]
> O resultado da migração alertas para alertas do Azure podem variar com base no resumo fornecido pelo GET resposta. Quando agendada, alertas na análise de registos estão temporariamente indisponíveis para serem modificadas no portal do Operations Management Suite. No entanto, pode criar novos alertas. 

Se o pedido POST for bem sucedido, devolve um Estado de HTTP 200 OK, juntamente com a seguinte resposta:

```json
{
    "version": 2
}
```

Esta resposta indica que os alertas foram expandidos com êxito para alertas do Azure. A propriedade de versão é apenas para a verificar se alertas tem sido expandidas para o Azure e não tem a nenhuma relação para o [API de pesquisa de análise do registo](../log-analytics/log-analytics-api-alerts.md). Quando os alertas são expandidos para o Azure com êxito, os endereços de correio eletrónico fornecido com a mensagem de pedido são enviados um relatório. Se todos os alertas na área de trabalho especificado já estão agendados para ser expandido, a resposta para o pedido POST é que a tentativa de foi proibida (um código de 403 Estado). Para ver qualquer mensagem de erro ou compreender se o processo está bloqueado, pode submeter um pedido GET. Se existir uma mensagem de erro, será devolvido, juntamente com as informações de resumo.

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


## <a name="option-3-use-a-custom-powershell-script"></a>Opção 3: Utilizar um script de PowerShell personalizado
 Se a Microsoft não tiver com êxito expandido os alertas do portal do Operations Management Suite para o Azure, pode efetuar manualmente, por isso, até 5 de Julho de 2018. As duas opções para a extensão manual são abordadas nas secções anteriores dois.

Após 5 de Julho de 2018, todos os alertas a partir do portal do Operations Management Suite são expandidos no Azure. Os utilizadores que não tenha de efetuar o [passos de remediação necessário sugeridos](#troubleshooting) serão associados os alertas em execução sem acionando ações ou notificações, devido à falta de [grupos ação](monitoring-action-groups.md). 

Para criar [grupos ação](monitoring-action-groups.md) para alertas manualmente na análise de registos, utilize o seguinte script de exemplo:
```PowerShell
########## Input Parameters Begin ###########


$subscriptionId = ""
$resourceGroup = ""
$workspaceName = "" 


########## Input Parameters End ###########

armclient login

try
{
    $workspace = armclient get /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/"$workspaceName"?api-version=2015-03-20 | ConvertFrom-Json
    $workspaceId = $workspace.properties.customerId
    $resourceLocation = $workspace.location
}
catch
{
    "Please enter valid input parameters i.e. Subscription Id, Resource Group and Workspace Name !!"
    exit
}

# Get Extend Summary of the Alerts
"`nGetting Extend Summary of Alerts for the workspace...`n"
try
{

    $value = armclient get /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/alertsversion?api-version=2017-04-26-preview

    "Extend preview summary"
    "=========================`n"

    $value

    $result = $value | ConvertFrom-Json
}
catch
{

    $ErrorMessage = $_.Exception.Message
    "Error occured while fetching/parsing Extend summary: $ErrorMessage"
    exit 
}

if ($result.version -eq 2)
{
    "`nThe alerts in this workspace have already been extended to Azure."
    exit
}

$in = Read-Host -Prompt "`nDo you want to continue extending the alerts to Azure? (Y/N)"

if ($in.ToLower() -ne "y")
{
    exit
} 


# Check for resource provider registration
try
{
    $val = armclient get subscriptions/$subscriptionId/providers/microsoft.insights/?api-version=2017-05-10 | ConvertFrom-Json
    if ($val.registrationState -eq "NotRegistered")
    {
        $val = armclient post subscriptions/$subscriptionId/providers/microsoft.insights/register/?api-version=2017-05-10
    }
}
catch
{
    "`nThe user does not have required access to register the resource provider. Please try with user having Contributor/Owner role in the subscription"
    exit
}

$actionGroupsMap = @{}
try
{
    "`nCreating new action groups for alerts extension...`n"
    foreach ($actionGroup in $result.migrationSummary.actionGroups)
    {
        $actionGroupName = $actionGroup.actionGroupName
        $actions = $actionGroup.actions
        if ($actionGroupsMap.ContainsKey($actionGroupName))
        {
            continue
        } 
        
        # Create action group payload
        $shortName = $actionGroupName.Substring($actionGroupName.LastIndexOf("AG_"))
        $properties = @{"groupShortName"= $shortName; "enabled" = $true}
        $emailReceivers = New-Object Object[] $actions.emailIds.Count
        $webhookReceivers = New-Object Object[] $actions.webhookActions.Count
        
        $count = 0
        foreach ($email in $actions.emailIds)
        {
            $emailReceivers[$count] = @{"name" = "Email$($count+1)"; "emailAddress" = "$email"}
            $count++
        }

        $count = 0
        foreach ($webhook in $actions.webhookActions)
        {
            $webhookReceivers[$count] = @{"name" = "$($webhook.name)"; "serviceUri" = "$($webhook.serviceUri)"}
            $count++
        }

        $itsmAction = $actions.itsmAction
        if ($itsmAction.connectionId -ne $null)
        {
            $val = @{
            "name" = "ITSM"
            "workspaceId" = "$subscriptionId|$workspaceId"
            "connectionId" = "$($itsmAction.connectionId)"
            "ticketConfiguration" = $itsmAction.templateInfo
            "region" = "$resourceLocation"
            }
            $properties["itsmReceivers"] = @($val)  
        }

        $properties["emailReceivers"] = @($emailReceivers)
        $properties["webhookReceivers"] = @($webhookReceivers)
        $armPayload = @{"properties" = $properties; "location" = "Global"} | ConvertTo-Json -Compress -Depth 4

    
        # Azure Resource Manager call to create action group
        $response = $armPayload | armclient put /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.insights/actionGroups/$actionGroupName/?api-version=2017-04-01

        "Created Action Group with name $actionGroupName" 
        $actionGroupsMap[$actionGroupName] = $actionGroup.actionGroupResourceId.ToLower()
        $index++
    }

    "`nSuccessfully created all action groups!!"
}
catch
{
    $ErrorMessage = $_.Exception.Message

    #Delete all action groups in case of failure
    "`nDeleting newly created action groups if any as some error happened..."
    
    foreach ($actionGroup in $actionGroupsMap.Keys)
    {
        $response = armclient delete /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.insights/actionGroups/$actionGroup/?api-version=2017-04-01      
    }

    "`nError: $ErrorMessage"
    "`nExiting..."
    exit
}

# Update all alerts configuration to the new version
"`nExtending OMS alerts to Azure...`n"

try
{
    $index = 1
    foreach ($alert in $result.migrationSummary.alerts)
    {
        $uri = $alert.alertId + "?api-version=2015-03-20"
        $config = armclient get $uri | ConvertFrom-Json
        $aznsNotification = @{
            "GroupIds" = @($actionGroupsMap[$alert.actionGroupName])
        }
        if ($alert.customWebhookPayload)
        {
            $aznsNotification.Add("CustomWebhookPayload", $alert.customWebhookPayload)
        }
        if ($alert.customEmailSubject)
        {
            $aznsNotification.Add("CustomEmailSubject", $alert.customEmailSubject)
        }      

        # Update alert version
        $config.properties.Version = 2

        $config.properties | Add-Member -MemberType NoteProperty -Name "AzNsNotification" -Value $aznsNotification
        $payload = $config | ConvertTo-Json -Depth 4
        $response = $payload | armclient put $uri
    
        "Extended alert with name $($alert.alertName)"
        $index++
    }
}
catch
{
    $ErrorMessage = $_.Exception.Message   
    if ($index -eq 1)
    {
        "`nDeleting all newly created action groups as no alerts got extended..."
        foreach ($actionGroup in $actionGroupsMap.Keys)
        {
            $response = armclient delete /subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.insights/actionGroups/$actionGroup/?api-version=2017-04-01      
        }
        "`nDeleted all action groups."  
    }
    
    "`nError: $ErrorMessage"
    "`nPlease resolve the issue and try extending again!!"
    "`nExiting..."
    exit
}

"`nSuccessfully extended all OMS alerts to Azure!!" 

# Update version of workspace to indicate extension
"`nUpdating alert version information in OMS workspace..." 

$response = armclient post "/subscriptions/$subscriptionId/resourceGroups/$resourceGroup/providers/Microsoft.OperationalInsights/workspaces/$workspaceName/alertsversion?api-version=2017-04-26-preview&iversion=2"

"`nExtension complete!!"
```


### <a name="about-the-custom-powershell-script"></a>Sobre o script do PowerShell personalizado 
Segue-se informações importantes sobre como utilizar o script:
- Um pré-requisito é a instalação do [ARMclient](https://github.com/projectkudu/ARMClient), uma ferramenta de linha de comandos de open source que simplifica a invocar a API do Azure Resource Manager.
- Para executar o script, tem de ter uma função de contribuinte ou proprietário na subscrição do Azure.
- Tem de fornecer os parâmetros seguintes:
    - $subscriptionId: o ID de subscrição do Azure associado à área de trabalho de análise de registos do Operations Management Suite.
    - $resourceGroup: o grupo de recursos do Azure para a área de trabalho de análise de registos do Operations Management Suite.
    - $workspaceName: o nome da área de trabalho de análise de registos do Operations Management Suite.

### <a name="output-of-the-custom-powershell-script"></a>Resultado do script do PowerShell personalizado
O script é verboso e produz os passos que é executada: 
- Apresenta o resumo, que contém as informações sobre os alertas de análise de registos do Operations Management Suite existentes na área de trabalho. O resumo também contém informações sobre os grupos de ação do Azure seja criado para as ações associadas. 
- Lhe for pedido para avançar com a extensão, ou de saída depois de visualizar o resumo.
- Se prosseguir com a extensão, são criados novos grupos de ação do Azure e todos os alertas existentes estão associados com os mesmos. 
- O script sair, apresentando a mensagem "Extensão concluída!" Em caso de eventuais falhas intermédias, o script apresenta erros subsequentes.

## <a name="troubleshooting"></a>Resolução de problemas 
Durante o processo de alargar a alertas, problemas podem impedir que o sistema de criar o necessário [grupos ação](monitoring-action-groups.md). Nestes casos, verá uma mensagem de erro da faixa no **alerta** secção do portal do Operations Management Suite, ou em GET chamada efectuada para a API.

> [!IMPORTANT]
> Se não executar os seguintes passos de remediação antes de 5 de Julho de 2018, alertas serão executada no Azure, mas não serão acionados qualquer notificação ou da ação. Para obter notificações de alertas, deve editar e adicionar manualmente [grupos ação](monitoring-action-groups.md), ou utilize precedente [script de PowerShell personalizado](#option-3---using-custom-powershell-script).

Eis os passos de remediação para cada erro:
- **Erro: O bloqueio de âmbito está presente ao nível do grupo de recursos/subscrição para operações de escrita**: ![captura de ecrã do Operations Management Suite page do portal definições de alerta, com a mensagem de erro de bloqueio de âmbito realçado](./media/monitor-alerts-extend/ErrorScopeLock.png)

    Quando o bloqueio de âmbito é ativado, a funcionalidade restringe qualquer nova alteração na subscrição ou grupo de recursos que contém a área de trabalho do Log Analytics (Operations Management Suite). O sistema não consegue expandir os alertas no Azure e criar grupos de ação necessária.
    
    Para resolver, elimine o *ReadOnly* bloqueio na sua subscrição ou grupo de recursos que contém a área de trabalho. Pode fazê-lo utilizando o portal do Azure, PowerShell, CLI do Azure ou a API. Para obter mais informações, consulte [utilização de recursos do bloqueio](../azure-resource-manager/resource-group-lock-resources.md). 
    
    Ao resolver o erro, utilizando os passos ilustrados no artigo, Operations Management Suite estende-se os alertas do Azure dentro da execução agendada o dia seguinte. Não precisa de efetuar qualquer ação adicional ou iniciar nada.

- **Erro: Política está presente ao nível do grupo de recursos/subscrição**: ![captura de ecrã do Operations Management Suite page do portal definições de alerta, com a mensagem de erro de política realçada](./media/monitor-alerts-extend/ErrorPolicy.png)

    Quando [política Azure](../azure-policy/azure-policy-introduction.md) é aplicada, restringe qualquer novo recurso de um subscrição ou grupo de recursos que contém a área de trabalho do Log Analytics (Operations Management Suite). O sistema não consegue expandir os alertas no Azure e criar grupos de ação necessária.
    
    Para resolver, edite a política que está a causar o *[RequestDisallowedByPolicy](../azure-resource-manager/resource-manager-policy-requestdisallowedbypolicy-error.md)* erro, o que impede a criação de novos recursos na sua subscrição ou grupo de recursos que contém a área de trabalho. Pode fazê-lo utilizando o portal do Azure, PowerShell, CLI do Azure ou a API. Pode auditar as ações para localizar a política apropriada que provoca a falha. Para obter mais informações, consulte [visualização de registos de atividade para auditar as ações](../azure-resource-manager/resource-group-audit.md). 
    
    Ao resolver o erro, utilizando os passos ilustrados no artigo, Operations Management Suite estende-se os alertas do Azure dentro da execução agendada o dia seguinte. Não precisa de efetuar qualquer ação adicional ou iniciar nada.


## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre as novas [experiência de alertas do Azure](monitoring-overview-unified-alerts.md).
* Saiba mais sobre [registar alertas nos alertas do Azure](monitor-alerts-unified-log.md).
