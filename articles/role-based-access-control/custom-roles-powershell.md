---
title: Criar funções personalizadas com o Azure PowerShell | Microsoft Docs
description: Saiba como criar funções personalizadas para o controlo de acesso baseado em funções (RBAC) com o Azure PowerShell. Isto inclui como listar, criar, atualizar e eliminar funções personalizadas.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 9e225dba-9044-4b13-b573-2f30d77925a9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 6205198d3f9c4ec5bfa2311014cf9b90dcade6dc
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320521"
---
# <a name="create-custom-roles-using-azure-powershell"></a>Criar funções personalizadas com o Azure PowerShell

Se o [funções incorporadas](built-in-roles.md) não satisfazer as necessidades específicas da sua organização, pode criar as suas próprias funções personalizadas. Este artigo descreve como criar e gerir funções personalizadas com o Azure PowerShell.

## <a name="prerequisites"></a>Pré-requisitos

Para criar funções personalizadas, tem de:

- Permissões para criar funções personalizadas, tais como [proprietário](built-in-roles.md#owner) ou [administrador de acesso de utilizador](built-in-roles.md#user-access-administrator)
- [O Azure PowerShell](/powershell/azure/install-azurerm-ps) instalado localmente

## <a name="list-custom-roles"></a>Lista de funções personalizadas

Para listar as funções que estão disponíveis para atribuição a um âmbito, utilize o [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) comando. O exemplo seguinte apresenta uma lista de todas as funções que estão disponíveis para atribuição na subscrição selecionada.

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

### <a name="create-a-custom-role-with-the-psroledefinition-object"></a>Criar uma função personalizada com o objeto de PSRoleDefinition

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

O exemplo seguinte mostra outra forma de criar o *operador de Máquina Virtual* função personalizada. Começa por criar um novo `PSRoleDefinition` objeto. As operações de ação estão especificadas no `perms` variável e definido para o `Actions` propriedade. O `NotActions` propriedade é definida ao ler o `NotActions` do [contribuinte de Máquina Virtual](built-in-roles.md#virtual-machine-contributor) função incorporada. Uma vez que [contribuinte de Máquina Virtual](built-in-roles.md#virtual-machine-contributor) não tem nenhuma `NotActions`, não é necessária a esta linha, mas que mostra como podem obter as informações de outra função.

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

### <a name="create-a-custom-role-with-json-template"></a>Criar uma função personalizada com o modelo JSON

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

## <a name="update-a-custom-role"></a>Atualizar uma função personalizada

Semelhante ao criar uma função personalizada, pode modificar uma função personalizada existente utilizando o `PSRoleDefinition` objeto ou um modelo JSON.

### <a name="update-a-custom-role-with-the-psroledefinition-object"></a>Atualizar uma função personalizada com o objeto de PSRoleDefinition

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

### <a name="update-a-custom-role-with-a-json-template"></a>Atualizar uma função personalizada com um modelo JSON

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

## <a name="next-steps"></a>Passos Seguintes

- [Tutorial: Criar uma função personalizada com o Azure PowerShell](tutorial-custom-role-powershell.md)
- [Funções personalizadas no Azure](custom-roles.md)
- [Operações de fornecedor de recursos do Azure Resource Manager](resource-provider-operations.md)
