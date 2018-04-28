---
title: Gerir o controlo de acesso baseado em funções (RBAC) com o Azure PowerShell | Microsoft Docs
description: Como gerir o RBAC com o Azure PowerShell, incluindo listagem funções, atribuir funções e eliminar atribuições de funções.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/17/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.openlocfilehash: 9fe7e6d12c3a831b006107ae53ca37cd22e34d4c
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="manage-role-based-access-control-with-azure-powershell"></a>Gerir o controlo de acesso baseado em funções com o Azure PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](role-assignments-powershell.md)
> * [CLI do Azure](role-assignments-cli.md)
> * [API REST](role-assignments-rest.md)

Com o controlo de acesso baseado em funções (RBAC), definem o acesso de utilizadores, grupos e principais de serviço através da atribuição de funções num determinado âmbito. Este artigo descreve como gerir o acesso com o Azure PowerShell.

## <a name="prerequisites"></a>Pré-requisitos

Antes de poder utilizar o PowerShell para gerir o RBAC, precisa de um dos seguintes:

* [PowerShell na Shell de nuvem do Azure](/azure/cloud-shell/overview)
* [O Azure PowerShell 5.1.0 ou posterior](/powershell/azure/install-azurerm-ps)

## <a name="list-roles"></a>Lista de funções

### <a name="list-all-available-roles"></a>Lista todas as funções disponíveis

Para funções RBAC de lista que estão disponíveis para atribuição e inspecionar as operações para que possam conceder acesso, utilizam [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

```azurepowershell
Get-AzureRmRoleDefinition | FT Name, Description
```

```Example
AcrImageSigner                                    acr image signer
AcrQuarantineReader                               acr quarantine data reader
AcrQuarantineWriter                               acr quarantine data writer
API Management Service Contributor                Can manage service and the APIs
API Management Service Operator Role              Can manage service but not the APIs
API Management Service Reader Role                Read-only access to service and APIs
Application Insights Component Contributor        Can manage Application Insights components
Application Insights Snapshot Debugger            Gives user permission to use Application Insights Snapshot Debugge...
Automation Job Operator                           Create and Manage Jobs using Automation Runbooks.
Automation Operator                               Automation Operators are able to start, stop, suspend, and resume ...
...
```

### <a name="list-a-specific-role"></a>Lista de uma função específica

Para listar uma função específica, utilize [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

```azurepowershell
Get-AzureRmRoleDefinition <role name>
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Contributor"

Name             : Contributor
Id               : b24988ac-6180-42a0-ab88-20f7382dd24c
IsCustom         : False
Description      : Lets you manage everything except access to resources.
Actions          : {*}
NotActions       : {Microsoft.Authorization/*/Delete, Microsoft.Authorization/*/Write,
                   Microsoft.Authorization/elevateAccess/Action}
AssignableScopes : {/}
```

### <a name="list-a-specific-role-in-json-format"></a>Lista de uma função específica no formato JSON

Para listar uma função específica no formato JSON, utilize [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

```azurepowershell
Get-AzureRmRoleDefinition <role name> | ConvertTo-Json
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Contributor" | ConvertTo-Json

{
    "Name":  "Contributor",
    "Id":  "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "IsCustom":  false,
    "Description":  "Lets you manage everything except access to resources.",
    "Actions":  [
                    "*"
                ],
    "NotActions":  [
                       "Microsoft.Authorization/*/Delete",
                       "Microsoft.Authorization/*/Write",
                       "Microsoft.Authorization/elevateAccess/Action"
                   ],
    "AssignableScopes":  [
                             "/"
                         ]
}
```

### <a name="list-actions-of-a-role"></a>Lista de ações de uma função

Para listar as ações para uma função específica, utilize [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition).

```azurepowershell
Get-AzureRmRoleDefinition <role name> | FL Actions, NotActions
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Contributor" | FL Actions, NotActions

Actions    : {*}
NotActions : {Microsoft.Authorization/*/Delete, Microsoft.Authorization/*/Write,
             Microsoft.Authorization/elevateAccess/Action}
```

```azurepowershell
(Get-AzureRmRoleDefinition <role name>).Actions
```

```Example
PS C:\> (Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions

Microsoft.Authorization/*/read
Microsoft.Compute/availabilitySets/*
Microsoft.Compute/locations/*
Microsoft.Compute/virtualMachines/*
Microsoft.Compute/virtualMachineScaleSets/*
Microsoft.DevTestLab/schedules/*
Microsoft.Insights/alertRules/*
Microsoft.Network/applicationGateways/backendAddressPools/join/action
Microsoft.Network/loadBalancers/backendAddressPools/join/action
...
```

## <a name="see-who-has-access"></a>Ver quem tem acesso

Para listar atribuições de acesso do RBAC, utilize [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

### <a name="list-role-assignments-at-a-specific-scope"></a>Listar atribuições de função a um âmbito específico

Pode ver todas as atribuições de função para uma subscrição especificada, o grupo de recursos ou o recurso. Por exemplo, para ver as todas as Active Directory atribuições de para um grupo de recursos, utilize [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

```azurepowershell
Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>
```

```Example
PS C:\> Get-AzureRmRoleAssignment -ResourceGroupName pharma-sales-projectforecast | FL DisplayName, RoleDefinitionName, Scope

DisplayName        : Alain Charon
RoleDefinitionName : Backup Operator
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast

DisplayName        : Isabella Simonsen
RoleDefinitionName : BizTalk Contributor
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast

DisplayName        : Alain Charon
RoleDefinitionName : Virtual Machine Contributor
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast
```

### <a name="list-roles-assigned-to-a-user"></a>Funções de lista atribuídas a um utilizador

Para listar todas as funções que estão atribuídas a um utilizador especificado, utilize [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

```azurepowershell
Get-AzureRmRoleAssignment -SignInName <user email>
```

```Example
PS C:\> Get-AzureRmRoleAssignment -SignInName isabella@example.com | FL DisplayName, RoleDefinitionName, Scope

DisplayName        : Isabella Simonsen
RoleDefinitionName : BizTalk Contributor
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast
```

Para listar todas as funções que estão atribuídas a um utilizador especificado e as funções que estão atribuídas a grupos a que o utilizador pertence, utilize [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment).

```azurepowershell
Get-AzureRmRoleAssignment -SignInName <user email> -ExpandPrincipalGroups
```

```Example
Get-AzureRmRoleAssignment -SignInName isabella@example.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>Listar atribuições de funções de administrador e coadmin do serviço clássico

Para listar atribuições de acesso para o administrador de subscrição clássica e coadministradores, utilize [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment):

```azurepowershell
Get-AzureRmRoleAssignment -IncludeClassicAdministrators
```

## <a name="grant-access"></a>Conceder acesso

### <a name="search-for-object-ids"></a>Pesquisa de IDs de objeto

Para atribuir uma função, tem de identificar o objeto (utilizador, grupo ou aplicação) e o âmbito.

Se não souber o ID de subscrição, pode encontrá-lo no **subscrições** painel no portal do Azure ou pode utilizar [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription).

Para obter o ID de objeto para um grupo do Azure AD, utilize [Get-AzureRmADGroup](/powershell/module/azurerm.resources/get-azurermadgroup):

```azurepowershell
Get-AzureRmADGroup -SearchString <group name in quotes>
```

Para obter o ID de objeto para uma aplicação ou principal de serviço do Azure AD, utilize [Get-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/get-azurermadserviceprincipal):

```azurepowershell
Get-AzureRmADServicePrincipal -SearchString <service name in quotes>
```

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Atribuir uma função a uma aplicação no âmbito de subscrição

Para conceder acesso a uma aplicação no âmbito de subscrição, utilize [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment):

```azurepowershell
New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>
```

```Example
PS C:\> New-AzureRmRoleAssignment -ObjectId 77777777-7777-7777-7777-777777777777 -RoleDefinitionName "Reader" -Scope /subscriptions/00000000-0000-0000-0000-000000000000

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/66666666-6666-6666-6666-666666666666
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
DisplayName        : MyApp1
SignInName         :
RoleDefinitionName : Reader
RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
ObjectId           : 77777777-7777-7777-7777-777777777777
ObjectType         : ServicePrincipal
CanDelegate        : False
```

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Atribuir uma função a um utilizador no âmbito do grupo de recursos

Para conceder acesso a um utilizador no âmbito do grupo de recursos, utilize [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment):

```azurepowershell
New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>
```

```Example
PS C:\> New-AzureRmRoleAssignment -SignInName alain@example.com -RoleDefinitionName "Virtual Machine Contributor" -ResourceGroupName pharma-sales-projectforecast


RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast/pr
                     oviders/Microsoft.Authorization/roleAssignments/55555555-5555-5555-5555-555555555555
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast
DisplayName        : Alain Charon
SignInName         : alain@example.com
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : 44444444-4444-4444-4444-444444444444
ObjectType         : User
CanDelegate        : False
```

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Atribuir uma função a um grupo no âmbito do recurso

Para conceder acesso a um grupo no âmbito de recursos, utilize [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment):

```azurepowershell
New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>
```

```Example
PS C:\> Get-AzureRmADGroup -SearchString "Pharma"

SecurityEnabled DisplayName         Id                                   Type
--------------- -----------         --                                   ----
           True Pharma Sales Admins aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa Group

PS C:\> New-AzureRmRoleAssignment -ObjectId aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa -RoleDefinitionName "Virtual Machine Contributor" -ResourceName RobertVirtualNetwork -ResourceType Microsoft.Network/virtualNetworks -ResourceGroupName RobertVirtualNetworkResourceGroup

RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyVirtualNetworkResourceGroup
                     /providers/Microsoft.Network/virtualNetworks/RobertVirtualNetwork/providers/Microsoft.Authorizat
                     ion/roleAssignments/bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/MyVirtualNetworkResourceGroup
                     /providers/Microsoft.Network/virtualNetworks/RobertVirtualNetwork
DisplayName        : Pharma Sales Admins
SignInName         :
RoleDefinitionName : Virtual Machine Contributor
RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
ObjectId           : aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa
ObjectType         : Group
CanDelegate        : False
```

## <a name="remove-access"></a>Remover o acesso

Para remover o acesso dos utilizadores, grupos e aplicações, utilize [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment):

```azurepowershell
Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>
```

```Example
PS C:\> Remove-AzureRmRoleAssignment -SignInName alain@example.com -RoleDefinitionName "Virtual Machine Contributor" -ResourceGroupName pharma-sales-projectforecast
```

## <a name="list-custom-roles"></a>Lista de funções personalizadas

Para listar as funções que estão disponíveis para atribuição a um âmbito, utilize o [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) comando.

O exemplo seguinte apresenta uma lista de todas as funções que estão disponíveis para atribuição na subscrição selecionada.

```azurepowershell
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

```Example
Name                                              IsCustom
----                                              --------
Virtual Machine Operator                              True
AcrImageSigner                                       False
AcrQuarantineReader                                  False
AcrQuarantineWriter                                  False
API Management Service Contributor                   False
...
```

O exemplo seguinte lista apenas as funções personalizadas que estão disponíveis para atribuição na subscrição selecionada.

```azurepowershell
Get-AzureRmRoleDefinition | ? {$_.IsCustom -eq $true} | FT Name, IsCustom
```

```Example
Name                     IsCustom
----                     --------
Virtual Machine Operator     True
```

Se a subscrição selecionada não se encontra no `AssignableScopes` da função, a função personalizada não será apresentada.

## <a name="create-a-custom-role"></a>Criar uma função personalizada

Para criar uma função personalizada, utilize o [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) comando. Existem dois métodos structuring a função, utilizando `PSRoleDefinition` objeto ou um modelo JSON. 

### <a name="get-operations-for-a-resource-provider"></a>Obter operações para um fornecedor de recursos

Quando criar funções personalizadas, é importante saber de todas as operações de possíveis dos fornecedores de recursos.
Pode ver a lista de [operações de fornecedor de recursos](resource-provider-operations.md) ou pode utilizar o [Get-AzureRMProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) comando para obter estas informações.
Por exemplo, se pretende verificar todas as operações disponíveis para as máquinas virtuais, utilize este comando:

```azurepowershell
Get-AzureRMProviderOperation <operation> | FT OperationName, Operation, Description -AutoSize
```

```Example
PS C:\> Get-AzureRMProviderOperation "Microsoft.Compute/virtualMachines/*" | FT OperationName, Operation, Description -AutoSize

OperationName                                  Operation                                                      Description
-------------                                  ---------                                                      -----------
Get Virtual Machine                            Microsoft.Compute/virtualMachines/read                         Get the propertie...
Create or Update Virtual Machine               Microsoft.Compute/virtualMachines/write                        Creates a new vir...
Delete Virtual Machine                         Microsoft.Compute/virtualMachines/delete                       Deletes the virtu...
Start Virtual Machine                          Microsoft.Compute/virtualMachines/start/action                 Starts the virtua...
...
```

### <a name="create-a-role-with-psroledefinition-object"></a>Criar uma função com PSRoleDefinition objeto

Quando utilizar o PowerShell para criar uma função personalizada, pode utilizar um do [funções incorporadas](built-in-roles.md) como um ponto de partida ou pode começar do zero. O primeiro exemplo nesta secção começa com uma função incorporada e, em seguida, customizes-lo com mais permissões. Editar os atributos para adicionar o `Actions`, `NotActions`, ou `AssignableScopes` que pretende e, em seguida, guarde as alterações como uma nova função.

O exemplo seguinte começa com o [contribuinte de Máquina Virtual](built-in-roles.md#virtual-machine-contributor) função incorporada para criar uma função personalizada com o nome *operador de Máquina Virtual*. A nova função concede acesso a todas as operações de leitura de *Microsoft. Compute*, *Microsoft*, e *Network* recursos fornecedores e concede acesso a iniciar , reiniciar e monitorizar máquinas virtuais. A função personalizada pode ser utilizada em duas subscrições.

```azurepowershell
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/00000000-0000-0000-0000-000000000000")
$role.AssignableScopes.Add("/subscriptions/11111111-1111-1111-1111-111111111111")
New-AzureRmRoleDefinition -Role $role
```

O exemplo seguinte mostra outra forma de criar o *operador de Máquina Virtual* função personalizada. Começa por criar um novo objeto de PSRoleDefinition. As operações de ação estão especificadas no `perms` variável e definido para o `Actions` propriedade. O `NotActions` propriedade é definida ao ler o `NotActions` do [contribuinte de Máquina Virtual](built-in-roles.md#virtual-machine-contributor) função incorporada. Uma vez que [contribuinte de Máquina Virtual](built-in-roles.md#virtual-machine-contributor) não tem nenhuma `NotActions`, não é necessária a esta linha, mas que mostra como podem obter as informações de outra função.

```azurepowershell
$role = [Microsoft.Azure.Commands.Resources.Models.Authorization.PSRoleDefinition]::new()
$role.Name = 'Virtual Machine Operator 2'
$role.Description = 'Can monitor and restart virtual machines.'
$role.IsCustom = $true
$perms = 'Microsoft.Storage/*/read','Microsoft.Network/*/read','Microsoft.Compute/*/read'
$perms += 'Microsoft.Compute/virtualMachines/start/action','Microsoft.Compute/virtualMachines/restart/action'
$perms += 'Microsoft.Authorization/*/read','Microsoft.Resources/subscriptions/resourceGroups/read'
$perms += 'Microsoft.Insights/alertRules/*','Microsoft.Support/*'
$role.Actions = $perms
$role.NotActions = (Get-AzureRmRoleDefinition -Name 'Virtual Machine Contributor').NotActions
$subs = '/subscriptions/00000000-0000-0000-0000-000000000000','/subscriptions/11111111-1111-1111-1111-111111111111'
$role.AssignableScopes = $subs
New-AzureRmRoleDefinition -Role $role
```

### <a name="create-role-with-json-template"></a>Criar função com o modelo JSON

Um modelo JSON pode ser utilizado como a definição de origem para a função personalizada. O exemplo seguinte cria uma função personalizada que permite acesso de leitura aos recursos de armazenamento e computação, acesso ao suportar, e adiciona a função para duas subscrições. Criar um novo ficheiro `C:\CustomRoles\customrole1.json` com o exemplo seguinte. O Id deve ser definido como `null` na criação da função inicial como um novo ID for gerado automaticamente. 

```json
{
  "Name": "Custom Role 1",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Para adicionar a função para as subscrições, execute o seguinte comando do PowerShell:

```azurepowershell
New-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="modify-a-custom-role"></a>Modificar uma função personalizada

Semelhante ao criar uma função personalizada, pode modificar uma função personalizada existente utilizando o `PSRoleDefinition` objeto ou um modelo JSON.

### <a name="modify-role-with-psroledefinition-object"></a>Modifique a função PSRoleDefinition objeto

Para modificar uma função personalizada, primeiro, utilize o [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) comando para obter a definição de função. Segundo, efetue as alterações pretendidas para a definição de função. Por último, utilize o [Set-AzureRmRoleDefinition](/powershell/module/azurerm.resources/set-azurermroledefinition) comando para guardar a definição de função modificadas.

O exemplo seguinte adiciona o `Microsoft.Insights/diagnosticSettings/*` operação para o *operador de Máquina Virtual* função personalizada.

```azurepowershell
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

```Example
PS C:\> $role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
PS C:\> $role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
PS C:\> Set-AzureRmRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}
```

O exemplo seguinte adiciona uma subscrição do Azure para os âmbitos atribuíveis do *operador de Máquina Virtual* função personalizada.

```azurepowershell
Get-AzureRmSubscription -SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
Set-AzureRmRoleDefinition -Role $role
```

```Example
PS C:\> Get-AzureRmSubscription -SubscriptionName Production3

Name     : Production3
Id       : 22222222-2222-2222-2222-222222222222
TenantId : 99999999-9999-9999-9999-999999999999
State    : Enabled

PS C:\> $role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
PS C:\> $role.AssignableScopes.Add("/subscriptions/22222222-2222-2222-2222-222222222222")
PS C:\> Set-AzureRmRoleDefinition -Role $role

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111,
                   /subscriptions/22222222-2222-2222-2222-222222222222}
```

### <a name="modify-role-with-json-template"></a>Modifique a função com o modelo JSON

Utilizando o modelo JSON anterior, pode facilmente modificar uma função personalizada existente para adicionar ou remover as ações. Atualizar o modelo JSON e adicionar a ação de leitura para funcionamento em rede, conforme mostrado no exemplo seguinte. As definições apresentadas no modelo cumulativamente e não são aplicadas a uma definição existente, que significa que a função aparece exatamente como especificar no modelo. Terá também de atualizar o campo Id com o ID da função. Se não tem a certeza que este valor é, pode utilizar o [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) cmdlet para obter estas informações.

```json
{
  "Name": "Custom Role 1",
  "Id": "acce7ded-2559-449d-bcd5-e9604e50bad1",
  "IsCustom": true,
  "Description": "Allows for read access to Azure storage and compute resources and access to support",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/00000000-0000-0000-0000-000000000000",
    "/subscriptions/11111111-1111-1111-1111-111111111111"
  ]
}
```

Para atualizar a função existente, execute o seguinte comando do PowerShell:

```azurepowershell
Set-AzureRmRoleDefinition -InputFile "C:\CustomRoles\customrole1.json"
```

## <a name="delete-a-custom-role"></a>Eliminar uma função personalizada

Para eliminar uma função personalizada, utilize o [Remove-AzureRmRoleDefinition](/powershell/module/azurerm.resources/remove-azurermroledefinition) comando.

O exemplo a seguir remove o *operador de Máquina Virtual* função personalizada.

```azurepowershell
Get-AzureRmRoleDefinition "Virtual Machine Operator"
Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

```Example
PS C:\> Get-AzureRmRoleDefinition "Virtual Machine Operator"

Name             : Virtual Machine Operator
Id               : 88888888-8888-8888-8888-888888888888
IsCustom         : True
Description      : Can monitor and restart virtual machines.
Actions          : {Microsoft.Storage/*/read, Microsoft.Network/*/read, Microsoft.Compute/*/read,
                   Microsoft.Compute/virtualMachines/start/action...}
NotActions       : {}
AssignableScopes : {/subscriptions/00000000-0000-0000-0000-000000000000,
                   /subscriptions/11111111-1111-1111-1111-111111111111}

PS C:\> Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition

Confirm
Are you sure you want to remove role definition with name 'Virtual Machine Operator'.
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
```

## <a name="see-also"></a>Consulte também

* [Utilizar o Azure PowerShell com o Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
