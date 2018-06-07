---
title: Obter dados de conformidade na política do Azure
description: Azure avaliações de política e efeitos determinam a conformidade. Saiba como obter os detalhes de conformidade.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/24/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: d36ecb18811901fb781e151c06badc0697c2d769
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655397"
---
# <a name="getting-compliance-data"></a>Obter dados de conformidade

Uma das vantagens maiores de política do Azure é a conhecimentos aprofundados e controlos fornece sobre recursos de uma subscrição ou [grupo de gestão](../azure-resource-manager/management-groups-overview.md) das subscrições. Este controlo pode ser executado muitas formas diferentes, tais como impedir os recursos que está a ser criados na localização incorreta, impor a utilização de tag comum e consistente, ou auditorias recursos existentes para apropriado configurações e definições. Em todos os casos, os dados forem gerados pelo política para ativar a compreender o estado de compatibilidade do seu ambiente.

Existem várias formas de aceder às informações de compatibilidade geradas pela sua política e atribuições de iniciativa:

- Utilizar o [Portal do Azure](#portal)
- Através de [linha de comandos](#command_line) scripting

Antes de consultar os métodos para elaborar relatórios sobre compatibilidade, vamos ver quando são actualizadas informações de compatibilidade e a frequência e a eventos que activam um ciclo de avaliação.

## <a name="evaluation-triggers"></a>Acionadores de avaliação

Os resultados de um ciclo de avaliação foi concluída são refletidos no `Microsoft.PolicyInsights` fornecedor de recursos através de `PolicyStates` e `PolicyEvents` operações. Para obter mais informações sobre as opções e capacidades da API de REST de informações de política, consulte [política Insights](/rest/api/policy-insights/).

Avaliações de políticas atribuídas e iniciativas ocorrem em resultado de vários eventos:

- Uma política ou iniciativa recentemente é atribuída a um âmbito. Quando isto ocorre, demora cerca de 30 minutos para que a atribuição a aplicar ao âmbito definido. Assim que for aplicada, o ciclo de avaliação começa para recursos dentro desse âmbito contra a política atribuída recentemente ou iniciativa e consoante os efeitos utilizados pela política ou iniciativa, recursos marcados como compatíveis ou incompatíveis. Uma política de grandes dimensões ou iniciativa avaliada em comparação com um âmbito de grandes dimensões de recursos pode demorar de tempo, pelo que não existe nenhum expectativas predefinida dos quando ciclo de avaliação irão concluir. Depois de terminar, os resultados de compatibilidade atualizados estão disponíveis no portal e SDKs.
- Uma política ou iniciativa já atribuído a um âmbito é atualizada. O ciclo de avaliação e a temporização das atualizações para este cenário é igual para uma nova atribuição a um âmbito.
- Um recurso é implementado para um âmbito com uma atribuição através do Gestor de recursos, REST, CLI do Azure ou do Azure PowerShell. Neste cenário, o evento efeito (acrescentar de auditoria, negar, a implementar) e informações de estado de conformidade torna-se disponíveis no portal e SDKs cerca de 15 minutos mais tarde.
- Ciclo de avaliação de compatibilidade padrão. Uma vez a cada 24 horas, atribuições são reavaliadas automaticamente. Uma política de grandes dimensões ou iniciativa avaliada em comparação com um âmbito de grandes dimensões de recursos pode demorar de tempo, pelo que não existe nenhum expectativas predefinida dos quando ciclo de avaliação irão concluir. Depois de terminar, os resultados de compatibilidade atualizados estão disponíveis no portal e SDKs.

## <a name="how-compliance-works"></a>Como funciona a conformidade

Numa atribuição, um recurso não é compatível caso não cumpra política ou iniciativa regras. A tabela seguinte mostra como outra política efeitos trabalham com a avaliação de condição para o estado de conformidade resultante:

| Estado do recurso | Efeito | Avaliação da política | Estado de compatibilidade |
| --- | --- | --- | --- |
| Existe | Negar, Auditar, Acrescentar\*, DeployIfNotExist\*, AuditIfNotExist\* | Verdadeiro | Em Não Conformidade |
| Existe | Negar, Auditar, Acrescentar\*, DeployIfNotExist\*, AuditIfNotExist\* | Falso | Compatível |
| Novo | Audit, AuditIfNotExist\* | Verdadeiro | Em Não Conformidade |
| Novo | Audit, AuditIfNotExist\* | Falso | Compatível |

\* Os efeitos de Append, DeployIfNotExist e AuditIfNotExist requerem que a declaração IF seja TRUE.
Os efeitos também necessitam que a condição de existência seja FALSE para estarem em não conformidade. Quando for TRUE, a condição IF aciona a avaliação da condição de existência dos recursos relacionados.

Para compreender melhor como os recursos são sinalizados como não conformes, vamos utilizar o exemplo de atribuição de política criado acima.

Por exemplo, suponha que tem um grupo de recursos – ContsoRG, com algumas contas de armazenamento (realçadas a vermelho) que estão expostas a redes públicas.

![Contas do Storage expostas a redes públicas](media/policy-insights/resource-group01.png)

Neste exemplo, tem de ser wary de riscos de segurança. Agora que criou a atribuição de política, esta é avaliada para todas as contas de armazenamento no grupo de recursos de ContosoRG. Auditorias das três contas de armazenamento não conformes, por conseguinte, alterar os respetivos Estados para **incompatíveis.**

![Auditar as contas de armazenamento não conformes](media/policy-insights/resource-group03.png)

## <a name="portal"></a>Portal

O portal do Azure showcases uma experiência de gráfica de visualizar e compreender o estado de conformidade no seu ambiente. No **política** página, o **descrição geral** opção fornece detalhes de âmbitos disponíveis sobre a compatibilidade das políticas e iniciativas. Para além do Estado de compatibilidade e a contagem por atribuição, contém um gráfico que mostra a compatibilidade nos últimos sete dias. O **conformidade** página contém muita desta mesma informação (exceto o gráfico), mas fornece filtrar e ordenar as opções adicionais.

![Página de política de conformidade](media/policy-compliance/compliance-page.png)

Como uma política ou iniciativa pode ser atribuída a âmbitos diferentes, tenha em atenção na tabela o âmbito de cada atribuição e o tipo de definição de que foi atribuído a esse âmbito. O número de políticas não compatíveis e incompatíveis recursos para cada atribuição também é fornecido. Clicar numa política ou iniciativa na tabela fornece uma mais aprofundada a compatibilidade para essa atribuição específica.

![Detalhes de conformidade da política](media/policy-compliance/compliance-details.png)

Enquanto a lista de recursos no **recursos não compatível** separador reflete o estado de avaliação dos recursos existentes para a atribuição atual, eventos (acrescentar de auditoria, negar, a implementar) por pedido para criar um recurso são apresentado no **eventos** separador.

![Eventos de conformidade da política](media/policy-compliance/compliance-events.png)

Faça duplo clique na linha do evento que pretende recolher mais detalhes sobre e selecione **Mostrar registos de atividade**. A página de registo de atividade é aberto e é previamente filtrada da procura que mostra os detalhes para a atribuição e os eventos. O registo de atividade fornece contexto adicional e informações sobre esses eventos.

![Registo de atividades de conformidade de política](media/policy-compliance/compliance-activitylog.png)

## <a name="command-line"></a>Linha de Comandos

As mesmas informações que está disponíveis no portal do podem ser obtidas utilizando a API REST diretamente (incluindo com [ARMClient](https://github.com/projectkudu/ARMClient)) ou o Azure PowerShell com a API REST. Para obter detalhes completos na REST API, consulte o [política Insights](/rest/api/policy-insights/) referência. As páginas de referência da REST API têm um verde 'Tente-' botão em cada operação que permite-lhe para experimentá-lo diretamente no browser.

Para utilizar os exemplos seguintes no Azure PowerShell, construir um token de autenticação com este código de exemplo. Em seguida, substitua o $restUri cadeia pretendida nos exemplos para obter um objeto JSON que, em seguida, pode ser analisado.

```azurepowershell-interactive
# Login first with Connect-AzureRmAccount if not using Cloud Shell

$azContext = Get-AzureRmContext
$azProfile = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile
$profileClient = New-Object -TypeName Microsoft.Azure.Commands.ResourceManager.Common.RMProfileClient -ArgumentList ($azProfile)
$token = $profileClient.AcquireAccessToken($azContext.Subscription.TenantId)
$authHeader = @{
    'Content-Type'='application/json'
    'Authorization'='Bearer ' + $token.AccessToken
}

# Define the REST API to communicate with
# Use double quotes for $restUri as some endpoints take strings passed in single quotes
$restUri = "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/summarize?api-version=2018-04-04"

# Invoke the REST API
$response = Invoke-RestMethod -Uri $restUri -Method POST -Headers $authHeader

# View the response object (as JSON)
$response
```

### <a name="summarize-results"></a>Resumir os resultados

Utilizando a API REST, resumo pode ser efetuado por grupo de gestão, a subscrição, grupo de recursos, recursos, iniciativa, política, atribuição de nível de subscrição ou atribuição ao nível do grupo de recursos. Eis um exemplo de resumo ao nível da subscrição utilizando a política de conhecimentos aprofundados [resumir para subscrição](/rest/api/policy-insights/policystates/summarizeforsubscription):

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/summarize?api-version=2018-04-04
```

A saída resume a subscrição. O resultado de exemplo abaixo, a compatibilidade resumida são em **value.results.nonCompliantResources** e **value.results.nonCompliantPolicies**. Este pedido fornece mais detalhes, incluindo a cada atribuição que composta pelos números de não conformidade e as informações de definição para cada atribuição. Cada objeto de política na hierarquia fornece um **queryResultsUri** que podem ser utilizados para obter detalhes adicionais nesse nível.

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#summary",
    "@odata.count": 1,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#summary/$entity",
        "results": {
            "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false",
            "nonCompliantResources": 15,
            "nonCompliantPolicies": 1
        },
        "policyAssignments": [{
            "policyAssignmentId": "/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77",
            "policySetDefinitionId": "",
            "results": {
                "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77'",
                "nonCompliantResources": 15,
                "nonCompliantPolicies": 1
            },
            "policyDefinitions": [{
                "policyDefinitionReferenceId": "",
                "policyDefinitionId": "/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "effect": "deny",
                "results": {
                    "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'",
                    "nonCompliantResources": 15
                }
            }]
        }]
    }]
}
```

### <a name="query-for-resources"></a>Consulta de recursos

Utilizando o exemplo de acima, **value.policyAssignments.policyDefinitions.results.queryResultsUri** fornecido-nos com um Uri de exemplo para obter todos os recursos de não conformidade para uma definição de política específica. Observar o **$filter** valor, IsCompliant é igual (eq) como FALSO, PolicyAssignmentId está especificado para a definição de política e, em seguida, PolicyDefinitionId próprio.
O motivo, incluindo o PolicyAssignmentId no filtro é porque o PolicyDefinitionId foi existe na política, vários ou iniciativa atribuições com uma variedade de âmbitos. Especificando o PolicyAssignmentId e o PolicyDefinitionId, iremos pode ser explícitos nos resultados de que está a procurar. Anteriormente, utilizámos **mais recente** para o PolicyStates (a única permitido valor para **policyStatesSummaryResource** no operador de resumir para subscrição), que define automaticamente um  **de** e **para** janela de tempo das últimas 24 horas.

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'
```

A resposta de exemplo abaixo tenha sido recortada para mostrar apenas um único recurso de não conformidade de uma forma abreviada (tenha em atenção que @odata.count é, na verdade, 15 e corresponde à contagem de recursos não conformes do exemplo acima). A resposta de detalhado fornece várias peças de dados sobre o recurso, a política (ou a iniciativa) e a atribuição. Tenha em atenção que também pode ver os parâmetros de atribuição foram transmitidos para a definição de política.

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
    "@odata.count": 15,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
        "timestamp": "2018-05-19T04:41:09Z",
        "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/rg-tags/providers/Microsoft.Compute/virtualMachines/linux",
        "policyAssignmentId": "/subscriptions/{subscriptionId}/resourceGroups/rg-tags/providers/Microsoft.Authorization/policyAssignments/37ce239ae4304622914f0c77",
        "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
        "effectiveParameters": "",
        "isCompliant": false,
        "subscriptionId": "{subscriptionId}",
        "resourceType": "/Microsoft.Compute/virtualMachines",
        "resourceLocation": "westus2",
        "resourceGroup": "RG-Tags",
        "resourceTags": "tbd",
        "policyAssignmentName": "37ce239ae4304622914f0c77",
        "policyAssignmentOwner": "tbd",
        "policyAssignmentParameters": "{\"tagName\":{\"value\":\"costCenter\"},\"tagValue\":{\"value\":\"Contoso-Test\"}}",
        "policyAssignmentScope": "/subscriptions/{subscriptionId}/resourceGroups/RG-Tags",
        "policyDefinitionName": "1e30110a-5ceb-460c-a204-c1c3969c6d62",
        "policyDefinitionAction": "deny",
        "policyDefinitionCategory": "tbd",
        "policySetDefinitionId": "",
        "policySetDefinitionName": "",
        "policySetDefinitionOwner": "",
        "policySetDefinitionCategory": "",
        "policySetDefinitionParameters": "",
        "managementGroupIds": "",
        "policyDefinitionReferenceId": ""
    }]
}
```

### <a name="view-events"></a>Ver eventos

Quando um recurso é criado ou atualizado, é gerado um resultado de avaliação da política. Os resultados são denominados _eventos política_. Utilize o Uri seguinte para ver eventos de política recentes associados à subscrição.

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/default/queryResults?api-version=2018-04-04
```

Os resultados assemelham-se ao seguinte exemplo:

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default",
    "@odata.count": 1,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default/$entity",
        "NumAuditEvents": 16
    }]
}
```

Para obter mais informações sobre consulta de eventos de política, consulte o [política eventos](/rest/api/policy-insights/policyevents) artigo de referência.

### <a name="azure-powershell-preview"></a>O Azure PowerShell (pré-visualização)

O módulo Azure PowerShell para a política ainda não é final, mas está atualmente disponível na galeria do PowerShell como um [versão de pré-visualização](https://www.powershellgallery.com/packages/AzureRM.PolicyInsights).
Se PowerShellGet é, pelo menos, versão 1.6.0 (necessário para suportar a versão de pré-lançamento itens), pode transferir a versão de pré-visualização utilizando `Install-Module` (Certifique-se de que a versão mais recente [Azure PowerShell](/powershell/azure/install-azurerm-ps) instalado):

```powershell
# Download preview from PowerShell Gallery via PowerShellGet
Install-Module -Name AzureRM.PolicyInsights -AllowPrerelease

# Import the downloaded module
Import-Module AzureRM.PolicyInsights

# Login with Connect-AzureRmAccount if not using Cloud Shell
Connect-AzureRmAccount
```

O módulo de pré-visualização tem três cmdlets:

- `Get-AzureRmPolicyStateSummary`
- `Get-AzureRmPolicyState`
- `Get-AzureRmPolicyEvent`

Exemplo: Ao obter o estado de resumo para a política mais superior atribuída com o maior número de recursos incompatíveis.

```powershell
PS > Get-AzureRmPolicyStateSummary -Top 1

NonCompliantResources : 15
NonCompliantPolicies  : 1
PolicyAssignments     : {/subscriptions/{subscriptionId}/resourcegroups/RG-Tags/providers/micros
                        oft.authorization/policyassignments/37ce239ae4304622914f0c77}
```

Exemplo: Obter o registo de estado para mais recentemente avaliada recursos (a predefinição é por timestamp por ordem descendente).

```powershell
PS > Get-AzureRmPolicyState -Top 1

Timestamp                  : 5/22/2018 3:47:34 PM
ResourceId                 : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Network/networkInterfaces/linux316
PolicyAssignmentId         : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Authorization/policyAssignments/37ce239ae4304622914f0c77
PolicyDefinitionId         : /providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62
IsCompliant                : False
SubscriptionId             : {subscriptionId}
ResourceType               : /Microsoft.Network/networkInterfaces
ResourceLocation           : westus2
ResourceGroup              : RG-Tags
ResourceTags               : tbd
PolicyAssignmentName       : 37ce239ae4304622914f0c77
PolicyAssignmentOwner      : tbd
PolicyAssignmentParameters : {"tagName":{"value":"costCenter"},"tagValue":{"value":"Contoso-Test"}}
PolicyAssignmentScope      : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags
PolicyDefinitionName       : 1e30110a-5ceb-460c-a204-c1c3969c6d62
PolicyDefinitionAction     : deny
PolicyDefinitionCategory   : tbd
```

Exemplo: Obter os detalhes para todos os recursos de rede virtual incompatíveis.

```powershell
PS > Get-AzureRmPolicyState -Filter "ResourceType eq '/Microsoft.Network/virtualNetworks'"

Timestamp                  : 5/22/2018 4:02:20 PM
ResourceId                 : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Network/virtualNetworks/RG-Tags-vnet
PolicyAssignmentId         : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Authorization/policyAssignments/37ce239ae4304622914f0c77
PolicyDefinitionId         : /providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62
IsCompliant                : False
SubscriptionId             : {subscriptionId}
ResourceType               : /Microsoft.Network/virtualNetworks
ResourceLocation           : westus2
ResourceGroup              : RG-Tags
ResourceTags               : tbd
PolicyAssignmentName       : 37ce239ae4304622914f0c77
PolicyAssignmentOwner      : tbd
PolicyAssignmentParameters : {"tagName":{"value":"costCenter"},"tagValue":{"value":"Contoso-Test"}}
PolicyAssignmentScope      : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags
PolicyDefinitionName       : 1e30110a-5ceb-460c-a204-c1c3969c6d62
PolicyDefinitionAction     : deny
PolicyDefinitionCategory   : tbd
```

Exemplo: Obter eventos relacionados com os recursos de rede virtual não compatível que ocorreram após uma data específica.

```powershell
PS > Get-AzureRmPolicyEvent -Filter "ResourceType eq '/Microsoft.Network/virtualNetworks'" -From '2018-05-19'

Timestamp                  : 5/19/2018 5:18:53 AM
ResourceId                 : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Network/virtualNetworks/RG-Tags-vnet
PolicyAssignmentId         : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Authorization/policyAssignments/37ce239ae4304622914f0c77
PolicyDefinitionId         : /providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62
IsCompliant                : False
SubscriptionId             : {subscriptionId}
ResourceType               : /Microsoft.Network/virtualNetworks
ResourceLocation           : eastus
ResourceGroup              : RG-Tags
ResourceTags               : tbd
PolicyAssignmentName       : 37ce239ae4304622914f0c77
PolicyAssignmentOwner      : tbd
PolicyAssignmentParameters : {"tagName":{"value":"costCenter"},"tagValue":{"value":"Contoso-Test"}}
PolicyAssignmentScope      : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags
PolicyDefinitionName       : 1e30110a-5ceb-460c-a204-c1c3969c6d62
PolicyDefinitionAction     : deny
PolicyDefinitionCategory   : tbd
TenantId                   : {tenantId}
PrincipalOid               : {principalOid}
```

O **PrincipalOid** campo pode ser utilizado para obter um utilizador específico com o cmdlet do PowerShell do Azure `Get-AzureRmADUser`. Substitua **{principalOid}** com a resposta obter do exemplo anterior.

```powershell
PS > (Get-AzureRmADUser -ObjectId {principalOid}).DisplayName
Trent Baker
```

## <a name="log-analytics"></a>Log Analytics

Se tiver um [Log Analytics](../log-analytics/log-analytics-overview.md) área de trabalho com o `AzureActivity` solução associado à sua subscrição, em seguida, pode também ver resultados de não conformidade de ciclo de avaliação através de consultas de Kusto simples e o `AzureActivity` tabela. Com detalhes de não conformidade na análise de registos, isto também significa que alertas podem ser configurados para deteção de não conformidade num recurso específico, grupo de recursos ou mesmo um limiar de itens não conformes, tais como mais de 10 nas últimas 24 horas.

![Conformidade com a política através da análise do registo](media/policy-compliance/compliance-loganalytics.png)

## <a name="next-steps"></a>Passos Seguintes

- Reveja o [estrutura de definição de política](policy-definition.md).
- Reveja [compreender os efeitos de política](policy-effects.md).
- Revisão que um grupo de gestão está [organizar os recursos com grupos de gestão do Azure](../azure-resource-manager/management-groups-overview.md)