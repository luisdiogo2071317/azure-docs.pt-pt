---
title: Como criar, listar e eliminar uma identidade gerida atribuído ao utilizador com o Azure PowerShell
description: Instruções passo a passo obter instruções sobre como criar, listar e eliminar atribuído ao utilizador gerido identidade com o Azure PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: daveba
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: d98cb449552bdbf4021a7f97a3253796bacc6e6d
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54427196"
---
# <a name="create-list-or-delete-a-user-assigned-managed-identity-using-azure-powershell"></a>Criar, lista ou eliminar uma identidade gerida atribuído ao utilizador com o Azure PowerShell

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Identidades geridas para recursos do Azure fornece serviços do Azure com uma identidade gerida no Azure Active Directory. Pode utilizar esta identidade para autenticar para serviços que suportam a autenticação do Azure AD, sem a necessidade de credenciais no seu código. 

Neste artigo, saiba como criar, listar e eliminar uma identidade gerida atribuído ao utilizador com o Azure PowerShell.

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com identidades geridas para recursos do Azure, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre uma identidade gerida atribuído de sistema e atribuído ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Instale [a versão mais recente do Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) se ainda não o fez.
- Se optar por instalar e utilizar o PowerShell localmente, este tutorial exige a versão 5.7.0 ou posterior do módulo Azure PowerShell. Executar ` Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/azurerm/install-azurerm-ps). 
- Se estiver a executar o PowerShell localmente, também irá precisar de: 
    - Execute `Login-AzureRmAccount` para criar uma ligação com o Azure.
    - Instale a [versão mais recente do PowerShellGet](/powershell/gallery/installing-psget#for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget).
    - Execute `Install-Module -Name PowerShellGet -AllowPrerelease` para obter a versão de pré-lançamento do módulo `PowerShellGet` (poderá precisar de `Exit` da sessão atual do PowerShell depois de executar este comando para instalar o módulo `AzureRM.ManagedServiceIdentity`).
    - Execute `Install-Module -Name AzureRM.ManagedServiceIdentity -AllowPrerelease` para instalar a versão de pré-lançamento do `AzureRM.ManagedServiceIdentity` módulo para executar o atribuído ao utilizador gerido operações de identidade neste artigo.

## <a name="create-a-user-assigned-managed-identity"></a>Criar uma identidade gerida atribuída pelo utilizador

Para criar uma identidade gerida atribuído ao utilizador, a conta tem do [Contribuidor de identidade gerida](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) atribuição de função.

Para criar uma identidade gerida atribuído ao utilizador, utilize o [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) comando. O `ResourceGroupName` parâmetro especifica o grupo de recursos onde criar a identidade gerida atribuído ao utilizador e o `-Name` parâmetro especifica o respetivo nome. Substitua a `<RESOURCE GROUP>` e `<USER ASSIGNED IDENTITY NAME>` valores de parâmetros com seus próprios valores:

[!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurepowershell-interactive
New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-managed-identities"></a>Lista de identidades geridas atribuído ao utilizador

A lista/leitura uma identidade gerida atribuído ao utilizador, a conta tem do [operador de identidade gerida](/azure/role-based-access-control/built-in-roles#managed-identity-operator) ou [Contribuidor de identidade gerida](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) atribuição de função.

Lista de identidades geridas atribuído ao utilizador, utilize o [Get-AzureRmUserAssigned](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity) comando.  O `-ResourceGroupName` parâmetro especifica o grupo de recursos em que a identidade gerida atribuído ao utilizador foi criada. Substitua o `<RESOURCE GROUP>` com seu próprio valor:

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP>
```
Em resposta, se tiver atribuído ao utilizador identidades geridas `"Microsoft.ManagedIdentity/userAssignedIdentities"` valor retornado para a chave, `Type`.

`Type :Microsoft.ManagedIdentity/userAssignedIdentities`

## <a name="delete-a-user-assigned-managed-identity"></a>Eliminar uma identidade gerida atribuído ao utilizador

Para eliminar uma identidade gerida atribuído ao utilizador, a conta tem do [Contribuidor de identidade gerida](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) atribuição de função.

Para eliminar uma identidade gerida atribuído ao utilizador, utilize o [Remove-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/remove-azurermuserassignedidentity) comando.  O `-ResourceGroupName` parâmetro especifica o grupo de recursos em que foi criada a identidade atribuída por utilizador e a `-Name` parâmetro especifica o respetivo nome. Substitua a `<RESOURCE GROUP>` e o `<USER ASSIGNED IDENTITY NAME>` valores de parâmetros com seus próprios valores:

 ```azurepowershell-interactive
Remove-AzurRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
> [!NOTE]
> A eliminar uma identidade gerida atribuído ao utilizador não remover a referência de qualquer recurso que foi atribuído a. Atribuições de identidade tem de ser removidos em separado.

## <a name="next-steps"></a>Passos Seguintes

Para ver uma lista completa e mais detalhes sobre o Azure PowerShell identidades geridas para comandos de recursos do Azure, consulte [AzureRM.ManagedServiceIdentity](/powershell/module/azurerm.managedserviceidentity#managed_service_identity).
