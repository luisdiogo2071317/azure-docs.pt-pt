---
title: Tutorial - Conceder acesso a um utilizador com o RBAC e o Azure PowerShell | Microsoft Docs
description: Utilize o controlo de acesso baseado em funções (RBAC) para conceder um utilizador de acesso para ver tudo numa subscrição e gerir tudo num grupo de recursos com o Azure PowerShell.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 06/11/2018
ms.author: rolyon
ms.openlocfilehash: 4eeae864721040bda18409b48abf5d33b81ca609
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295903"
---
# <a name="tutorial-grant-access-for-a-user-using-rbac-and-azure-powershell"></a>Tutorial: Conceder acesso a um utilizador com o RBAC e o Azure PowerShell

[O controlo de acesso baseado em funções (RBAC)](overview.md) é a forma de gerir o acesso a recursos no Azure. Neste tutorial irá conceder acesso a um utilizador para ver tudo numa subscrição e gerir tudo num grupo de recursos com o Azure PowerShell.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Conceder acesso a um utilizador de âmbitos diferentes
> * Listar o acesso
> * Remover o acesso

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, irá precisar de:

- Permissões para criar utilizadores no Azure Active Directory (ou ter um utilizador existente)
- [Azure Cloud Shell](/azure/cloud-shell/quickstart-powershell)

## <a name="role-assignments"></a>Atribuições de funções

No RBAC, para conceder acesso, crie uma atribuição de função. Uma atribuição de função é composta por três elementos: principal de segurança, definição de função e âmbito. Seguem-se as duas atribuições de funções que vai realizar neste tutorial:

| Principal de segurança | Definição de função | Âmbito |
| --- | --- | --- |
| Utilizador<br>(Utilizador do Tutorial do RBAC) | [Leitor](built-in-roles.md#reader) | Subscrição |
| Utilizador<br>(Utilizador do Tutorial do RBAC)| [Contribuidor](built-in-roles.md#contributor) | Grupo de recursos<br>(rbac-tutorial-resource-group) |

   ![Atribuições de funções para um utilizador](./media/tutorial-role-assignments-user-powershell/rbac-role-assignments-user.png)

## <a name="create-a-user"></a>Criar um utilizador

Para atribuir uma função, precisa de um utilizador, um grupo ou um principal de serviço. Se ainda não tiver um utilizador, pode criar um.

1. No Azure Cloud Shell, crie uma palavra-passe que esteja em conformidade com os seus requisitos de complexidade de palavra-passe.

    ```azurepowershell
    $PasswordProfile = New-Object -TypeName Microsoft.Open.AzureAD.Model.PasswordProfile
    $PasswordProfile.Password = "Password"
    ```

1. Crie um novo utilizador de domínio com o comando [New-AzureADUser](/powershell/module/azuread/new-azureaduser).

    ```azurepowershell
    New-AzureADUser -DisplayName "RBAC Tutorial User" -PasswordProfile $PasswordProfile `
      -UserPrincipalName "rbacuser@example.com" -AccountEnabled $true -MailNickName "rbacuser"
    ```
    
    ```Example
    ObjectId                             DisplayName        UserPrincipalName    UserType
    --------                             -----------        -----------------    --------
    11111111-1111-1111-1111-111111111111 RBAC Tutorial User rbacuser@example.com Member
    ```

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Utilize um grupo de recursos para mostrar como atribuir uma função num âmbito do grupo de recursos.

1. Obtenha uma lista de localizações de regiões com o comando [Get-AzureRmLocation](/powershell/module/azurerm.resources/get-azurermlocation).

   ```azurepowershell
   Get-AzureRmLocation | select Location
   ```

1. Selecione uma localização perto de si e atribua-a a uma variável.

   ```azurepowershell
   $location = "westus"
   ```

1. Crie um novo grupo de recursos com o comando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).

   ```azurepowershell
   New-AzureRmResourceGroup -Name "rbac-tutorial-resource-group" -Location $location
   ```

   ```Example
   ResourceGroupName : rbac-tutorial-resource-group
   Location          : westus
   ProvisioningState : Succeeded
   Tags              :
   ResourceId        : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group
   ```

## <a name="grant-access"></a>Conceder acesso

Para conceder acesso ao utilizador, utilize o comando [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) para atribuir uma função. Tem de especificar o principal de segurança, a definição de função e o âmbito.

1. Obtenha o ID da sua subscrição com o comando [Get-AzureRmSubscription](/powershell/module/azurerm.resources/get-azurermsubscription).

    ```azurepowershell
    Get-AzureRmSubscription
    ```

    ```Example
    Name     : Pay-As-You-Go
    Id       : 00000000-0000-0000-0000-000000000000
    TenantId : 22222222-2222-2222-2222-222222222222
    State    : Enabled
    ```

1. Guarde o âmbito da subscrição numa variável.

    ```azurepowershell
    $subScope = "/subscriptions/00000000-0000-0000-0000-000000000000"
    ```

1. Atribua a função [Leitor](built-in-roles.md#reader) ao utilizador no âmbito da subscrição.

    ```azurepowershell
    New-AzureRmRoleAssignment -SignInName rbacuser@example.com `
      -RoleDefinitionName "Reader" `
      -Scope $subScope
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/44444444-4444-4444-4444-444444444444
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Reader
    RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

1. Atribua a função [Contribuidor](built-in-roles.md#contributor) ao utilizador no âmbito do grupo de recursos.

    ```azurepowershell
    New-AzureRmRoleAssignment -SignInName rbacuser@example.com `
      -RoleDefinitionName "Contributor" `
      -ResourceGroupName "rbac-tutorial-resource-group"
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Contributor
    RoleDefinitionId   : b24988ac-6180-42a0-ab88-20f7382dd24c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

## <a name="list-access"></a>Listar o acesso

1. Para verificar o acesso à subscrição, utilize o comando [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) para listar as atribuições de funções.

    ```azurepowershell
    Get-AzureRmRoleAssignment -SignInName rbacuser@example.com -Scope $subScope
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Reader
    RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

    No resultado, pode ver que a função de Leitor foi atribuída ao Utilizador do Tutorial do RBAC no âmbito da subscrição.

1. Para verificar o acesso ao grupo de recursos, utilize o comando [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) para listar as atribuições de funções.

    ```azurepowershell
    Get-AzureRmRoleAssignment -SignInName rbacuser@example.com -ResourceGroupName "rbac-tutorial-resource-group"
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Contributor
    RoleDefinitionId   : b24988ac-6180-42a0-ab88-20f7382dd24c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
    DisplayName        : RBAC Tutorial User
    SignInName         : rbacuser@example.com
    RoleDefinitionName : Reader
    RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

    No resultado, pode ver que as funções de Contribuidor e de Leitor foram atribuídas ao Utilizador do Tutorial do RBAC. A função de Contribuidor está no âmbito do rbac-tutorial-resource-group e a função de Leitor é herdada no âmbito da subscrição.

## <a name="optional-list-access-using-the-azure-portal"></a>(Opcional) Listar acesso com o Portal do Azure

1. Para ver as atribuições de funções, aceda ao portal do Azure, veja o painel **Controlo de acesso (IAM)** para a subscrição.

    ![Atribuições de funções para um utilizador no âmbito da subscrição](./media/tutorial-role-assignments-user-powershell/role-assignments-subscription-user.png)

1. Veja o painel **Controlo de acesso (IAM)** para o grupo de recursos.

    ![Atribuições de funções para um utilizador no âmbito do grupo de recursos](./media/tutorial-role-assignments-user-powershell/role-assignments-resource-group-user.png)

## <a name="remove-access"></a>Remover o acesso

Para remover o acesso dos utilizadores, grupos e aplicações, utilize [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) para remover uma atribuição de função.

1. Utilize o seguinte comando para remover a atribuição da função de Contribuidor para o utilizador no âmbito do grupo de recursos.

    ```azurepowershell
    Remove-AzureRmRoleAssignment -SignInName rbacuser@example.com `
      -RoleDefinitionName "Contributor" `
      -ResourceGroupName "rbac-tutorial-resource-group"
    ```

1. Utilize o seguinte comando para remover a atribuição da função de Leitor para a subscrição do utilizador de recursos.

    ```azurepowershell
    Remove-AzureRmRoleAssignment -SignInName rbacuser@example.com `
      -RoleDefinitionName "Reader" `
      -Scope $subScope
    ```

## <a name="clean-up-resources"></a>Limpar recursos

Para limpar os recursos criados por este tutorial, elimine o grupo de recursos e o utilizador.

1. Elimine o grupo de recursos com o comando [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup).

    ```azurepowershell
    Remove-AzureRmResourceGroup -Name "rbac-tutorial-resource-group"
    ```

    ```Example
    Confirm
    Are you sure you want to remove resource group 'rbac-tutorial-resource-group'
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    ```
    
1. Quando lhe for pedido para confirmar, escreva **Y**. Irá demorar alguns segundos para eliminar.

1. Elimine o utilizador com o comando [Remove-AzureADUser](/powershell/module/azuread/remove-azureaduser).

    ```azurepowershell
    Remove-AzureADUser -ObjectId "rbacuser@example.com"
    ```

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Gerir acesso através do PowerShell e do RBAC](role-assignments-powershell.md)