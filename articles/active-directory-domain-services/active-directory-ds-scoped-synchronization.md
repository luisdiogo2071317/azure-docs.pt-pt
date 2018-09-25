---
title: 'Azure Active Directory Domain Services: Sincronização de âmbito | Documentos da Microsoft'
description: Configurar a sincronização de âmbito do Azure AD para os domínios geridos
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 9389cf0f-0036-4b17-95da-80838edd2225
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2018
ms.author: maheshu
ms.openlocfilehash: ada6e7536ec91e5b1f35f1740177ae264fe68cf7
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063495"
---
# <a name="configure-scoped-synchronization-from-azure-ad-to-your-managed-domain"></a>Configurar a sincronização de âmbito do Azure AD para o seu domínio gerido
Este artigo mostra-lhe como configurar contas de usuários específicos a serem sincronizados do diretório do Azure AD ao seu domínio gerido do Azure AD Domain Services.


## <a name="group-based-scoped-synchronization"></a>Com base no grupo de sincronização de âmbito
Por predefinição, todos os utilizadores e grupos no seu diretório do Azure AD são sincronizados com o seu domínio gerido. Se o domínio gerido está a ser utilizado apenas por alguns utilizadores, pode ser preferível sincronizar apenas as contas de utilizador para o domínio gerido. Com base no grupo de sincronização âmbito permite-lhe fazê-lo. Quando configurado, as únicas contas de utilizador que pertencem aos grupos que especificou são sincronizadas com o domínio gerido.


## <a name="get-started-install-the-required-powershell-modules"></a>Começar a utilizar: instalar os módulos necessários do PowerShell

### <a name="install-and-configure-azure-ad-powershell"></a>Instalar e configurar o Azure AD PowerShell
Siga as instruções no artigo para [instalar o módulo Azure AD PowerShell e ligar ao Azure AD](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

### <a name="install-and-configure-azure-powershell"></a>Instalar e configurar o Azure PowerShell
Siga as instruções no artigo para [instalar o módulo Azure PowerShell e ligue à sua subscrição do Azure](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).



## <a name="enable-group-based-scoped-synchronization"></a>Ativar a sincronização de âmbito com base em grupo
Conclua os seguintes passos para configurar a sincronização de âmbito com base em grupo ao seu domínio gerido:

1. Selecione os grupos que pretende sincronizar e forneça o nome a apresentar dos grupos que pretende sincronizar o domínio gerido.

2. Guarde o script na seção a seguir num arquivo chamado ```Select-GroupsToSync.ps1```. Execute o script, conforme mostrado abaixo:

  ```powershell
  .\Select-GroupsToSync.ps1 -groupsToAdd @(“GroupName1”, “GroupName2”)
  ```

3. Agora, ative a sincronização de âmbito com base em grupo para o domínio gerido.

  ```powershell
  // Login to your Azure AD tenant
  Login-AzureRmAccount

  // Retrieve the Azure AD Domain Services resource.
  $DomainServicesResource = Get-AzureRmResource -ResourceType "Microsoft.AAD/DomainServices"

  // Enable group-based scoped synchronization.
  $enableScopedSync = @{"filteredSync" = "Enabled"}

  Set-AzureRmResource -Id $DomainServicesResource.ResourceId -Properties $enableScopedSync
  ```

## <a name="disable-group-based-scoped-synchronization"></a>Desativar sincronização de âmbito com base em grupo
Utilize o seguinte script do PowerShell para desativar a baseada em grupo no âmbito de sincronização para o seu domínio gerido:

```powershell
// Login to your Azure AD tenant
Login-AzureRmAccount

// Retrieve the Azure AD Domain Services resource.
$DomainServicesResource = Get-AzureRmResource -ResourceType "Microsoft.AAD/DomainServices"

// Disable group-based scoped synchronization.
$disableScopedSync = @{"filteredSync" = "Disabled"}

Set-AzureRmResource -Id $DomainServicesResource.ResourceId -Properties $disableScopedSync
```

## <a name="script-to-select-groups-to-synchronize-to-the-managed-domain-select-groupstosyncps1"></a>Script para selecionar grupos para sincronizar com o domínio gerido (Select-GroupsToSync.ps1)
Guarde o seguinte script para um ficheiro (```Select-GroupsToSync.ps1```). Este script configura os serviços de domínio do Azure AD para sincronizar grupos selecionados para o domínio gerido. Todas as contas de utilizador que pertencem a grupos especificados serão sincronizadas com o domínio gerido.

```powershell
param (
    [Parameter(Position = 0)]
    [String[]]$groupsToAdd
)

Connect-AzureAD
$sp = Get-AzureADServicePrincipal -Filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
$role = $sp.AppRoles | where-object -FilterScript {$_.DisplayName -eq "User"}

Write-Output "`n****************************************************************************"

Write-Output "Total group-assignments need to be added: $($groupsToAdd.Count)"
$newGroupIds = New-Object 'System.Collections.Generic.HashSet[string]'
foreach ($groupName in $groupsToAdd)
{
    try
    {
        $group = Get-AzureADGroup -Filter "DisplayName eq '$groupName'"
        $newGroupIds.Add($group.ObjectId)

        Write-Output "Group-Name: $groupName, Id: $($group.ObjectId)"
    }
    catch
    {
        Write-Error "Failed to find group: $groupName. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
Write-Output "`n****************************************************************************"

$currentAssignments = Get-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId
Write-Output "Total current group-assignments: $($currentAssignments.Count), SP-ObjectId: $($sp.ObjectId)"

$currAssignedObjectIds = New-Object 'System.Collections.Generic.HashSet[string]'
foreach ($assignment in $currentAssignments)
{
    Write-Output "Assignment-ObjectId: $($assignment.PrincipalId)"

    if ($newGroupIds.Contains($assignment.PrincipalId) -eq $false)
    {
        Write-Output "This assignment is not needed anymore. Removing it! Assignment-ObjectId: $($assignment.PrincipalId)"
        Remove-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId -AppRoleAssignmentId $assignment.ObjectId
    }
    else
    {
        $currAssignedObjectIds.Add($assignment.PrincipalId)
    }
}

Write-Output "****************************************************************************`n"
Write-Output "`n****************************************************************************"

foreach ($id in $newGroupIds)
{
    try
    {
        if ($currAssignedObjectIds.Contains($id) -eq $false)
        {
            Write-Output "Adding new group-assignment. Role-Id: $($role.Id), Group-Object-Id: $id, ResourceId: $($sp.ObjectId)"
            New-AzureADGroupAppRoleAssignment -Id $role.Id -ObjectId $id -PrincipalId $id -ResourceId $sp.ObjectId
        }
        else
        {
            Write-Output "Group-ObjectId: $id is already assigned."
        }
    }
    catch
    {
        Write-Error "Exception occured assigning Object-ID: $id. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
```

## <a name="next-steps"></a>Passos Seguintes
* [Compreender a sincronização no Azure AD Domain Services](active-directory-ds-synchronization.md)
