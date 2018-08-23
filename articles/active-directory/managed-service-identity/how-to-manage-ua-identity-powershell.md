---
title: Como criar, listar e eliminar um utilizador atribuído (MSI) com o Azure PowerShell
description: Instruções passo a passo obter instruções sobre como criar, listar e eliminar o utilizador atribuído a identidade do serviço gerido com o Azure PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 3c4aa6d91d5f8c1e28cb52f6846f6ac9feeb4edd
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/11/2018
ms.locfileid: "42061478"
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-azure-powershell"></a>Criar, lista ou eliminar uma identidade atribuída ao utilizador com o Azure PowerShell

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida no Azure Active Directory. Pode utilizar esta identidade para autenticar para serviços que suportam a autenticação do Azure AD, sem a necessidade de credenciais no seu código. 

Neste artigo, saiba como criar, listar e eliminar uma identidade atribuída ao utilizador com o Azure PowerShell.

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade do serviço gerido, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre um sistema atribuído e a identidade atribuída ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Instale [a versão mais recente do Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) se ainda não o fez.
- Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer o Azure PowerShell 5.7.0 de versão do módulo ou posterior. Executar ` Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). 
- Se estiver executando o PowerShell localmente, terá também de: 
    - Execute `Login-AzureRmAccount` para criar uma ligação com o Azure.
    - Instalar o [versão mais recente do PowerShellGet](/powershell/gallery/installing-psget#for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget).
    - Execute `Install-Module -Name PowerShellGet -AllowPrerelease` para obter a versão de pré-lançamento do `PowerShellGet` módulo (poderá ter de `Exit` fora a atual sessão do PowerShell depois de executar este comando para instalar o `AzureRM.ManagedServiceIdentity` módulo).
    - Execute `Install-Module -Name AzureRM.ManagedServiceIdentity -AllowPrerelease` para instalar a versão de pré-lançamento do `AzureRM.ManagedServiceIdentity` módulo para executar o utilizador atribuído a operações de identidade neste artigo.
- Para efetuar as operações de gestão neste artigo, a conta tem das atribuições de funções seguintes:
    - [Contribuidor de identidade de geridos](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) função para criar, ler (lista), atualizar e eliminar uma identidade atribuída ao utilizador.
    - [Gerido operador de identidade](/azure/role-based-access-control/built-in-roles#managed-identity-operator) função para ler as propriedades de uma identidade de utilizador atribuída de (lista).

## <a name="create-a-user-assigned-identity"></a>Criar uma identidade atribuída pelo utilizador

Para criar uma identidade atribuída ao utilizador, utilize o [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) comando. O `ResourceGroupName` parâmetro especifica o grupo de recursos onde criar a identidade atribuída ao utilizador e o `-Name` parâmetro especifica o respetivo nome. Substitua a `<RESOURCE GROUP>` e `<USER ASSIGNED IDENTITY NAME>` valores de parâmetros com seus próprios valores:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurepowershell-interactive
New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-identities"></a>Utilizador de lista identidades atribuído

Para listar as identidades atribuídas por utilizadores, utilize o [Get-AzureRmUserAssigned](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity) comando.  O `-ResourceGroupName` parâmetro especifica o grupo de recursos em que a identidade atribuída ao utilizador foi criado. Substitua o `<RESOURCE GROUP>` com seu próprio valor:

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP>
```
Em resposta, tem identidades de utilizador `"Microsoft.ManagedIdentity/userAssignedIdentities"` valor retornado para a chave, `Type`.

`Type :Microsoft.ManagedIdentity/userAssignedIdentities`

## <a name="delete-a-user-assigned-identity"></a>Eliminar um identidade atribuída ao utilizador

Para eliminar uma identidade de utilizador, utilize o [Remove-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/remove-azurermuserassignedidentity) comando.  O `-ResourceGroupName` parâmetro especifica o grupo de recursos em que a identidade atribuída ao utilizador foi criado e o `-Name` parâmetro especifica o respetivo nome. Substitua a `<RESOURCE GROUP>` e o `<USER ASSIGNED IDENTITY NAME>` valores de parâmetros com seus próprios valores:

 ```azurepowershell-interactive
Remove-AzurRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
> [!NOTE]
> A eliminar um identidade atribuída ao utilizador não remover a referência de qualquer recurso que foi atribuído a. Atribuições de identidade tem de ser removidos em separado.

## <a name="related-content"></a>Conteúdo relacionado

Para obter uma lista completa e mais detalhes sobre os comandos do Azure PowerShell MSI, consulte [AzureRM.ManagedServiceIdentity](/powershell/module/azurerm.managedserviceidentity#managed_service_identity).
