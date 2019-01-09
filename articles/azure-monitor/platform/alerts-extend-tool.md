---
title: Expandir os alertas do Log Analytics para o Azure
description: Este artigo descreve as ferramentas e a API através do qual pode expandir alertas do Log Analytics para alertas do Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/04/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: bdc3646116dfd5f16c0c039c4fb95d11c6593adf
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/09/2019
ms.locfileid: "54120998"
---
# <a name="extend-alerts-from-log-analytics-into-azure-alerts"></a>Expandir alertas do Log Analytics para alertas do Azure
A funcionalidade de alertas no Log Analytics do Azure está a ser substituída através de alertas do Azure. Como parte desta transição, os alertas que configurou originalmente no Log Analytics irão ser expandidas para o Azure. Se não quiser esperar que elas sejam movidas automaticamente para o Azure, pode iniciar o processo:

- Manualmente a partir do portal do Operations Management Suite. 
- Por meio de programação, utilizando a API AlertsVersion.  

> [!NOTE]
> Microsoft irá ampliar automaticamente os alertas criados em instâncias de cloud pública do Log Analytics para alertas do Azure a partir de 14 de Maio de 2018, uma série de recorrente até concluído. Se tiver quaisquer problemas ao criar [grupos de ação](../../azure-monitor/platform/action-groups.md), utilize [estes passos de remediação](alerts-extend-tool.md#troubleshooting) para obter os grupos de ação criados automaticamente. Pode utilizar estes passos até 5 de Julho de 2018. *Não aplicável para o Azure Government e os utilizadores de cloud soberanas do Log Analytics*. 

## <a name="option-1-initiate-from-the-operations-management-suite-portal"></a>Opção 1: Iniciar a partir do portal do Operations Management Suite
Os passos seguintes descrevem como ampliar alertas para a área de trabalho a partir do portal do Operations Management Suite.  

1. No portal do Azure, selecione **Todos os serviços**. Na lista de recursos, escreva **Log Analytics**. À medida que começa a escrever, a lista filtra com base na sua entrada. Selecione **Log Analytics**.
2. No painel de subscrições do Log Analytics, selecione uma área de trabalho e, em seguida, selecione o **Portal do OMS** mosaico.
![Painel de subscrição de captura de ecrã do Log Analytics, com o mosaico do Portal do OMS realçado](media/alerts-extend-tool/azure-portal-01.png) 
3. Depois, será redirecionado para o portal do Operations Management Suite, selecione o **definições** ícone.
![Portal de captura de ecrã do Operations Management Suite, com o ícone de definições realçado](media/alerts-extend-tool/oms-portal-settings-option.png) 
4. Partir do **configurações** página, selecione **alertas**.  
5. Selecione **ampliados para o Azure**.
![Captura de ecrã do Operations Management Suite definições de alerta página do portal, com expandir para Azure realçado](media/alerts-extend-tool/ExtendInto.png)
6. Um Assistente de três etapas é apresentado na **alertas** painel. Leia a visão geral e selecione **seguinte**.
![Captura de ecrã do passo 1 do Assistente](media/alerts-extend-tool/ExtendStep1.png)  
7. Na segunda etapa, é apresentado um resumo das alterações propostas, listagem apropriado [grupos de ação](../../azure-monitor/platform/action-groups.md) para os alertas. Se a ações semelhantes são vistas em mais do que um alerta, o assistente propõe associar um grupo de ação única todos eles.  A Convenção de nomenclatura é o seguinte: *WorkspaceName_AG_ #Number*. Para continuar, selecione **seguinte**.
![Captura de ecrã do passo 2 do Assistente](media/alerts-extend-tool/ExtendStep2.png)  
8. No último passo do assistente, selecione **concluir**e confirmar quando lhe for pedido para iniciar o processo. Opcionalmente, pode fornecer um endereço de e-mail, para que será notificado quando o processo for concluído e todos os alertas foram movidos com êxito para alertas do Azure.
![Captura de ecrã do passo 3 do Assistente](media/alerts-extend-tool/ExtendStep3.png)

Quando o assistente esteja concluído, sobre o **definições de alerta** página, a opção para expandir os alertas para o Azure é removida. Em segundo plano, os alertas são movidos para o Azure e isto pode demorar algum tempo. Durante a operação, pode fazer alterações aos alertas do portal do Operations Management Suite. Pode ver o estado atual na faixa na parte superior do portal de. Se um endereço de e-mail que indicou anteriormente, receberá um e-mail quando o processo foi concluída com êxito.  


Alertas continuam a ser listados no portal do Operations Management Suite, mesmo depois são movidas com êxito para o Azure.
![Página de definições de alerta de portal captura de ecrã do Operations Management Suite](media/alerts-extend-tool/PostExtendList.png)


## <a name="option-2-use-the-alertsversion-api"></a>Opção 2: Utilizar a API AlertsVersion
Pode utilizar a API AlertsVersion do Log Analytics para expandir alertas do Log Analytics para alertas do Azure de qualquer cliente que pode chamar uma API REST. Pode aceder à API do PowerShell, utilizando [ARMClient](https://github.com/projectkudu/ARMClient), uma ferramenta de linha de comandos de código-fonte aberto. O utilizador pode apresentar os resultados em JSON.  

Para utilizar a API, primeiro crie um pedido GET. Esta é avaliada e devolve um resumo das alterações propostas, antes de tentar, na verdade, expandir para o Azure através de um pedido POST. Os resultados listam os alertas e uma lista de propostas de [grupos de ação](../../azure-monitor/platform/action-groups.md), no formato JSON. Se a ações semelhantes são vistas em mais do que um alerta, o serviço propõe associar todos eles com um grupo de ação única. A Convenção de nomenclatura é o seguinte: *WorkspaceName_AG_ #Number*.

```
armclient GET  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview
```

Se a solicitação GET for bem-sucedida, código de estado HTTP 200 é retornado, juntamente com uma lista de alertas e proposto grupos de ação nos dados JSON. Segue-se uma resposta de exemplo:

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
                            "serviceUri": "https://test.com"
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
Se a área de trabalho especificada não tem quaisquer regras de alerta definidas, os dados JSON retorna o seguinte:

```json
{
    "version": 1,
    "Message": "No Alerts found in the workspace for migration."
}
```

Se todas as regras de alerta na área de trabalho especificada já tem sido expandidas para o Azure, é a resposta ao pedido GET:

```json
{
    "version": 2
}
```

Para iniciar a migrar os alertas para o Azure, inicie uma resposta de POSTAGEM. A resposta de POSTAGEM confirma que a sua intenção, bem como aceitação, ter alertas expandidas a partir do Log Analytics para alertas do Azure. A atividade for agendada e os alertas são processados como indicado, com base nos resultados quando efetuou anteriormente a resposta GET. Opcionalmente, pode fornecer uma lista de endereços de e-mail para o qual o Log Analytics envia um relatório quando o processo de plano de fundo agendada dos alertas de migração é concluída com êxito. Pode utilizar o seguinte exemplo de pedido:

```
$emailJSON = “{‘Recipients’: [‘a@b.com’, ‘b@a.com’]}”
armclient POST  /subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>/alertsversion?api-version=2017-04-26-preview $emailJSON
```

> [!NOTE]
> O resultado da migração alertas para alertas do Azure podem variar de acordo com o resumo fornecido pela resposta GET. Quando agendada, alertas no Log Analytics estão temporariamente indisponíveis para modificação no portal do Operations Management Suite. No entanto, pode criar novos alertas. 

Se o pedido POST for bem-sucedida, ela retorna um Estado de HTTP 200 OK, juntamente com a seguinte resposta:

```json
{
    "version": 2
}
```

Esta resposta indica que os alertas foram estendidos com êxito em alertas do Azure. A propriedade de versão é apenas para verificar se os alertas foram estendidas para o Azure e não tem relação com o [API de pesquisa do Log Analytics](../../azure-monitor/platform/api-alerts.md). Quando os alertas são expandidos para o Azure com êxito, qualquer endereços de e-mail fornecido com a mensagem de pedido são enviadas de um relatório. Se todos os alertas na área de trabalho especificada já estão agendados para serem expandidos, a resposta ao seu pedido POST é que a tentativa de foi proibida (um código de 403 status). Para ver qualquer mensagem de erro ou compreender se o processo está bloqueado, pode submeter um pedido GET. Se existir uma mensagem de erro, ele será retornado, juntamente com as informações de resumidas.

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
                            "serviceUri": "https://test.com"
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


## <a name="option-3-use-a-custom-powershell-script"></a>Opção 3: Utilizar um script personalizado do PowerShell
 Se a Microsoft não com êxito estendeu os alertas do portal do Operations Management Suite para o Azure, pode fazê-lo manualmente até 5 de Julho de 2018. As duas opções para a extensão manual são abordadas nas duas secções anteriores.

Depois de 5 de Julho de 2018, todos os alertas a partir do portal do Operations Management Suite são expandidos para o Azure. Os utilizadores que não demorou a [passos de remediação necessário sugeridos](#troubleshooting) serão associados seus alertas em execução sem disparando ações ou notificações, devido à falta de [grupos de ação](../../azure-monitor/platform/action-groups.md). 

Para criar [grupos de ação](../../azure-monitor/platform/action-groups.md) para os alertas manualmente no Log Analytics, utilize o seguinte script de exemplo:
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
    "Error occurred while fetching/parsing Extend summary: $ErrorMessage"
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


### <a name="about-the-custom-powershell-script"></a>Sobre o script de PowerShell personalizado 
Segue-se informações importantes sobre como utilizar o script:
- Um pré-requisito é a instalação do [ARMclient](https://github.com/projectkudu/ARMClient), uma ferramenta de linha de comandos de código-fonte aberto que simplifica a invocar a API do Azure Resource Manager.
- Para executar o script, tem de ter uma função de proprietário ou contribuinte na subscrição do Azure.
- Tem de fornecer os seguintes parâmetros:
    - $subscriptionId: O ID de subscrição do Azure associado à área de trabalho do Log Analytics do Operations Management Suite.
    - $resourceGroup: O grupo de recursos do Azure para a área de trabalho do Log Analytics do Operations Management Suite.
    - $workspaceName: O nome da área de trabalho do Log Analytics do Operations Management Suite.

### <a name="output-of-the-custom-powershell-script"></a>Saída do script do PowerShell personalizado
O script é detalhado e produz os passos, enquanto é executada: 
- Apresenta o resumo, que contém as informações sobre os alertas do Log Analytics do Operations Management Suite existentes na área de trabalho. O resumo também contém informações sobre os grupos de ação do Azure a ser criado para as ações associadas a eles. 
- Lhe for pedido para prosseguir com a extensão, ou se sair depois de visualizar o resumo.
- Se for adiante com a extensão, novos grupos de ação do Azure são criados e todos os alertas existentes são associados a elas. 
- O script é encerrado, apresentando a mensagem "Extensão concluída!" Em caso de quaisquer falhas de nível intermediárias, o script exibe erros subseqüentes.

## <a name="troubleshooting"></a>Resolução de problemas 
Durante o processo de expansão de alertas, problemas podem impedir que o sistema de criar as informações necessárias [grupos de ação](../../azure-monitor/platform/action-groups.md). Nesses casos, verá uma mensagem de erro num banner no **alerta** seção do portal do Operations Management Suite ou em GET chamar concluído para a API.

> [!IMPORTANT]
> Se a cloud pública do Azure com base em usuários do Log Analytics não efetue os seguintes passos de remediação antes de 5 de Julho de 2018, alertas serão executadas no Azure, mas não irão disparar a notificação ou qualquer ação. Para obter notificações de alertas, deve editar e adicionar manualmente [grupos de ação](../../azure-monitor/platform/action-groups.md), ou utilize anterior [script de PowerShell personalizado](#option-3---using-custom-powershell-script).

Eis os passos de remediação para cada erro:
- **Erro: Bloqueio de âmbito está presente ao nível do grupo de recursos/subscrição para operações de escrita**:   ![Captura de ecrã do Operations Management Suite page do portal definições de alerta, com a mensagem de erro de bloqueio de âmbito realçado](media/alerts-extend-tool/ErrorScopeLock.png)

    Quando o bloqueio de âmbito é ativado, a funcionalidade restringe alterações ao novo a subscrição ou grupo de recursos que contém a área de trabalho do Log Analytics (Operations Management Suite). O sistema é não é possível expandir os alertas para o Azure e criar grupos de ação necessário.
    
    Para resolver, elimine o *só de leitura* bloqueio na sua subscrição ou grupo de recursos que contém a área de trabalho. Pode fazê-lo ao utilizar o portal do Azure, PowerShell, CLI do Azure ou a API. Para obter mais informações, consulte [utilização de recursos de bloqueio](../../azure-resource-manager/resource-group-lock-resources.md). 
    
    Ao resolver o erro, utilizando as etapas ilustradas no artigo, o Operations Management Suite estende os alertas para o Azure em execução agendada do dia seguinte. Não precisa de fazer mais nada ou iniciar qualquer coisa.

- **Erro: Política está presente ao nível do grupo de recursos/subscrição**:   ![Captura de ecrã do Operations Management Suite page do portal definições de alerta, com a mensagem de erro de política realçada](media/alerts-extend-tool/ErrorPolicy.png)

    Quando [do Azure Policy](../../governance/policy/overview.md) é aplicada, restringe qualquer novo recurso num subscrição ou grupo de recursos que contém a área de trabalho do Log Analytics (Operations Management Suite). O sistema é não é possível expandir os alertas para o Azure e criar grupos de ação necessário.
    
    Para resolver, edite a política que está a causar o *[RequestDisallowedByPolicy](../../azure-resource-manager/resource-manager-policy-requestdisallowedbypolicy-error.md)* erro, o que impede a criação de novos recursos na sua subscrição ou grupo de recursos que contém a área de trabalho. Pode fazê-lo ao utilizar o portal do Azure, PowerShell, CLI do Azure ou a API. É possível auditar as ações para localizar a política adequada, que está causando a falha. Para obter mais informações, consulte [visualizar registos de atividades para auditar as ações](../../azure-resource-manager/resource-group-audit.md). 
    
    Ao resolver o erro, utilizando as etapas ilustradas no artigo, o Operations Management Suite estende os alertas para o Azure em execução agendada do dia seguinte. Não precisa de fazer mais nada ou iniciar qualquer coisa.


## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre a nova [experiência de alertas do Azure](../../azure-monitor/platform/alerts-overview.md).
* Saiba mais sobre [alertas de registo nos alertas do Azure](alerts-unified-log.md).
