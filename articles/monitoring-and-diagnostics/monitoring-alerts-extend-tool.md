---
title: Como expandir os alertas de Analytcs de registo para o Azure | Microsoft Docs
description: Este artigo descreve as ferramentas e a API através do qual pode expandir a alertas de análise de registos para alertas do Azure.
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
ms.date: 06/04/2018
ms.author: vinagara
ms.openlocfilehash: 0dce6e6772b4efea90df2e095ac0041641d99061
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763532"
---
# <a name="how-to-extend-alerts-from-log-analytics-into-azure-alerts"></a>Como expandir os alertas de análise de registos para alertas do Azure
Alertas na análise de registos está a ser substituído por alertas do Azure e como parte desta transição, alertas que configurou no Log Analytics irão ser expandidos para o Azure.  Se não pretender aguardar para ser automaticamente movido para o Azure, pode iniciar o processo de uma das opções seguintes:

1. Manualmente a partir do portal do OMS 
2. Através de programação utilizando a API de AlertsVersion  

> [!NOTE]
> Microsoft automaticamente irá expandir alertas criados na análise de registos para alertas do Azure ao iniciar no **14 de Maio de 2018** numa abordagem faseada até concluído. A partir deste dia reencaminhar, Microsoft irá começar ao agendar a migrar os alertas para o Azure e, durante desta transição, alertas podem ser geridos a partir do portal do OMS e o portal do Azure. Este processo é nondestructive e não interruptive.  

## <a name="option-1---initiate-from-the-oms-portal"></a>Opção 1 - iniciar a partir do Portal do OMS
Os passos seguintes descrevem como expandir alertas para a área de trabalho a partir do portal do OMS.  

1. No portal do Azure, clique em **All services** (Todos os serviços). Na lista de recursos, escreva **Log Analytics**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **Log Analytics**.
2. No painel de subscrições de análise de registos, selecione uma área de trabalho e, em seguida, selecione o **Portal do OMS** mosaico.<br><br> ![Botão Pesquisa de Registos](./media/monitor-alerts-extend/azure-portal-01.png)<br><br> 
3. Depois de são redirecionados para o portal do OMS, clique no mosaico de definições no lado superior direito da página.<br><br> ![Opção de definições do portal do OMS](./media/monitor-alerts-extend/oms-portal-settings-option.png)<br><br> 
4. Do **definições** página, selecione **alertas**.  
5. Clique no botão **expanda no Azure**.<br><br> ![Página Definições de alerta de portal do OMS com opção de expanda](./media/monitor-alerts-extend/ExtendInto.png)
6. Um assistente é apresentado no painel, com a primeira das três passos para fornecer uma descrição geral do processo.  Clique em **seguinte** para continuar.<br><br> ![Expandir a alertas de análise de registos para o Azure - passo 1](./media/monitor-alerts-extend/ExtendStep1.png)  
7. No segundo passo, é apresentado um resumo das alterações propostas, listagem adequado [ação grupos](monitoring-action-groups.md) para os alertas. Se a ações semelhantes são vistas através dos mais do que um alerta, o serviço será propor associe todos eles grupo uma única ação.  Grupo de ação propostas siga a Convenção de nomenclatura: *WorkspaceName_AG_ #Number*. Para continuar, clique em **seguinte**.<br><br> ![expandir a alertas de análise de registos para o Azure - passo 2](./media/monitor-alerts-extend/ExtendStep2.png)  
8. No último passo do assistente, clique em **concluir** e confirme quando lhe for pedido para iniciar o processo.  Opcionalmente, pode fornecer um endereços de e-mail, de modo a que são notificados quando o processo ser concluído e todos os alertas tem sido movidos com êxito para alertas do Azure.<br><br> ![Expandir a alertas de análise de registos para o Azure - passo 3](./media/monitor-alerts-extend/ExtendStep3.png)

Assim que o assistente estiver concluído, repare que no **definições de alerta** página que a opção de expandir alertas para o Azure é removida.  Em segundo plano, os alertas são movidos para o Azure e isto pode demorar algum tempo.  Durante a operação, não poderá efetuar alterações ao alertas a partir do portal do OMS.  Será apresentado o estado atual da faixa na parte superior do portal e, se forneceu anteriormente um endereço de e-mail irá receber um e-mail quando o processo é concluído com êxito.  


Alertas continuam a ser apresentados no portal do OMS, mesmo depois de estes são movidas com êxito no Azure.<br><br> ![Depois de mover alertas na análise de registos para o Azure](./media/monitor-alerts-extend/PostExtendList.png)


## <a name="option-2---using-the-alertsversion-api"></a>Opção 2 - utilizando o AlertsVersion API
Pode utilizar a API de AlertsVersion de análise do registo para expandir alertas de análise de registos para alertas do Azure a partir de qualquer cliente que possa chamar uma API REST. Pode aceder a partir do PowerShell utilizando [ARMClient](https://github.com/projectkudu/ARMClient), uma ferramenta de linha de comandos de open source que simplifica a invocar a API do Azure Resource Manager. A utilização de ARMClient e do PowerShell é uma das muitas opções para aceder à API do.  Utilizando a API irá enviar os resultados no JSON.  

Para utilizar a API, primeiro crie um pedido GET, que irá avaliar e devolver um resumo das alterações propostas antes de tentar expandir realmente no Azure através de um pedido POST. Os resultados da listam os alertas e lista proposta de [grupos ação](monitoring-action-groups.md) no formato JSON.  Se a ações semelhantes são vistas através dos mais do que um alerta, o serviço será propor todos eles associar um grupo de única ação.  Grupos de ação propostos siga a Convenção de nomenclatura: *WorkspaceName_AG_ #Number*.

```
armclient GET  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Se o pedido GET for bem-sucedida, é devolvido um código de estado HTTP 200 juntamente com uma lista de alertas e grupos de ação proposto nos dados JSON. Segue-se um exemplo de resposta:

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
Se a área de trabalho especificada não tem quaisquer regras de alerta definidas, juntamente com o estado de HTTP 200 OK código para a operação de obter os dados JSON devolve:

```json
{
    "version": 1,
    "Message": "No Alerts found in the workspace for migration."
}
```

Se todas as regras de alerta na área de trabalho especificada já tiverem sido expandidas para o Azure - se a resposta ao pedido GET:

```json
{
    "version": 2
}
```

Para iniciar a migrar os alertas para o Azure, inicie uma resposta POST. A resposta POST confirma que a sua intenção, bem como aceitação ter alertas expandidas a partir da análise de registos ao Auzre alertas.  A atividade está agendada e os alertas são processados como indicado com base nos resultados quando efetuar a resposta GET anteriormente.  Opcionalmente, pode fornecer uma lista de endereços de e-mail para o qual de análise de registos serão correio um relatório quando o processo em segundo plano agendada de migrar os alertas for concluída com êxito.  Este procedimento é efetuado utilizando o seguinte exemplo de pedido:

```
$emailJSON = “{‘Recipients’: [‘a@b.com’, ‘b@a.com’]}”
armclient POST  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $emailJSON
```

> [!NOTE]
> Resultado do migrar alertas para alertas do Azure podem variar consoante o resumo fornecido pelo GET resposta.  Assim que a agendada, alertas na análise de registos serão temporariamente indisponíveis para editar/modificação no portal do OMS.  No entanto, podem ser criadas novos alertas. 

Se o pedido POST for bem sucedido, devolve um Estado de HTTP 200 OK juntamente com a seguinte resposta:

```json
{
    "version": 2
}
```

Esta resposta indica que os alertas foram expandidos com êxito para alertas do Azure. A propriedade de versão é apenas para a verificar se alertas tem sido expandidas para o Azure e não tem a nenhuma relação para o [API de pesquisa de análise do registo](../log-analytics/log-analytics-api-alerts.md). Assim que os alertas são expandidos para o Azure com êxito, os endereços de e-mail fornecido com a mensagem de pedido são enviados um relatório com detalhes sobre as alterações efetuadas.  Se todos os alertas na área de trabalho especificado já estão agendados para ser expandido, a resposta para o pedido POST é um significado de código de 403 Estado que a tentativa de foi proibida. Para ver qualquer mensagem de erro ou compreender que se o processo é bloqueado, pode submeter um pedido GET e uma mensagem de erro se existir, será devolvido, juntamente com as informações de resumo.

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


## <a name="option-3---using-custom-powershell-script"></a>Opção 3 - utilizando o script de PowerShell personalizado
 Depois 14 de Maio de 2018 - se Microsoft não tiver com êxito expandido os alertas do portal do OMS no Azure; em seguida, até **5 de Julho de 2018** -utilizador manualmente pode fazer o mesmo através de [Option1 - através da GUI](#option-1---initiate-from-the-oms-portal) ou [opção 2 - através da API](#option-2---using-the-alertsversion-api).

Depois de **5 de Julho de 2018** -todos os alertas a partir do portal do OMS irão ser expandidos para o Azure. Os utilizadores que não tenha de efetuar o [passos de remediação necessário sugeridos](#troubleshooting), serão associados os alertas em execução sem acionando ações ou notificações devido à falta de [ação grupos](monitoring-action-groups.md). 

Para criar manualmente [ação grupos](monitoring-action-groups.md) para alertas na análise de registos, os utilizadores podem utilizar o script de exemplo abaixo.
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

    
        # ARM call to create action group
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


**Utilizar o script do PowerShell personalizado** 
- Pré-requisitos é a instalação do [ARMclient](https://github.com/projectkudu/ARMClient), uma ferramenta de linha de comandos de open source que simplifica a invocar a API do Azure Resource Manager
- Utilizador a executar o script que referida tem de ter função de contribuinte ou proprietário na subscrição do Azure
- Seguem-se os parâmetros fornecidos para o script:
    - $subscriptionId: o ID de subscrição do Azure associada com a área de trabalho do OMS/LA
    - $resourceGroup: o grupo de recursos do Azure onde reside a área de trabalho do OMS/LA
    - $workspaceName: o nome da área de trabalho OMS/LA

**Resultado do script do PowerShell personalizado** o script é verboso e será saída os passos conforme ser executada. 
- Apresentará o resumo, que contém as informações sobre os alertas existentes do OMS/LA na área de trabalho e os grupos de ação do Azure seja criado para as ações associadas. 
- Será solicitado ao utilizador para avançar com a extensão ou de saída depois de visualizar o resumo.
- Se o utilizador solicitar em avançar com a extensão, serão criados novos grupos de ação do Azure e todos os alertas existentes serão associados com os mesmos. 
- No final, o script sair, apresentando a mensagem "extensão concluir!." Em caso de eventuais falhas intermédias, serão apresentados erros subsequentes.

## <a name="troubleshooting"></a>Resolução de problemas 
Durante o processo de alargar alertas do OMS no Azure, pode haver ocasional problema que impede que o sistema de criação necessários [ação grupos](monitoring-action-groups.md). Nestes casos, uma mensagem de erro será apresentada no portal do OMS através de faixa na secção de alerta e, na chamada GET efetuada à API.

> [!WARNING]
> Se o utilizador não siga os passos de remediação de precribed fornecidos em baixo, antes **5 de Julho de 2018** -, em seguida, os alertas serão executadas no Azure, mas sem acionando qualquer notificação ou da ação. Para obter notificações de alertas, os utilizadores tem manualmente de editar e adicionar [ação grupos](monitoring-action-groups.md) ou utilize o [script de PowerShell personalizado](#option-3---using-custom-powershell-script) fornecida acima.

Listadas abaixo estão os passos de remediação para cada erro:
1. **Erro: O bloqueio de âmbito está presente ao nível do grupo de recursos/subscrição para operações de escrita**: ![página Definições de alerta de portal do OMS com a mensagem de erro ScopeLock](./media/monitor-alerts-extend/ErrorScopeLock.png)

    a. Quando bloqueia âmbito estiver ativada, restringindo qualquer alteração nova na subscrição ou grupo de recursos que contém a área de trabalho de análise de registos (OMS); o sistema não consegue expandir alertas (cópia) no Azure e criar grupos de ação necessária.
    
    b. Para resolver, elimine o *ReadOnly* bloqueio na sua subscrição ou grupo de recursos que contém a área de trabalho; utilizando o portal do Azure, o Powershell, CLI do Azure ou API. Para obter mais informações, veja o artigo no [utilização de recursos do bloqueio](../azure-resource-manager/resource-group-lock-resources.md). 
    
    c. Depois de resolvido de acordo com os passos ilustrados no artigo, OMS irá expandir os alertas no Azure dentro da execução agendada o dia seguinte; sem a necessidade de qualquer ação ou foi iniciado.

2. **Erro: Política está presente ao nível do grupo de recursos/subscrição**: ![página Definições de alerta de portal do OMS com a mensagem de erro de política](./media/monitor-alerts-extend/ErrorPolicy.png)

    a. Quando [política Azure](../azure-policy/azure-policy-introduction.md) é aplicada, restringindo a qualquer recurso novo na subscrição ou grupo de recursos que contém a área de trabalho de análise de registos (OMS); o sistema não consegue expandir alertas (cópia) no Azure e criar grupos de ação necessária.
    
    b. Para resolver, edite a política a causar o *[RequestDisallowedByPolicy](../azure-resource-manager/resource-manager-policy-requestdisallowedbypolicy-error.md)* erro, o que impede a criação de novos recursos na sua subscrição ou grupo de recursos que contém a área de trabalho. Através do portal do Azure, Powershell, a CLI do Azure ou a API; pode auditar as ações para localizar a política apropriada que provoca a falha. Para obter mais informações, veja o artigo no [visualização de registos de atividade para auditar as ações](../azure-resource-manager/resource-group-audit.md). 
    
    c. Depois de resolvido de acordo com os passos ilustrados no artigo, OMS irá expandir os alertas no Azure dentro da execução agendada o dia seguinte; sem a necessidade de qualquer ação ou foi iniciado.


## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre as novas [experiência de alertas do Azure](monitoring-overview-unified-alerts.md).
* Saiba mais sobre [registar alertas nos alertas do Azure](monitor-alerts-unified-log.md).
