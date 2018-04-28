---
title: Como criar, listar e eliminar um utilizador atribuído (MSI) com o Azure PowerShell
description: Passo a passo obter instruções sobre como criar, listar e eliminar utilizador atribuído a identidade de serviço geridas com o Azure PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 0f4041bd34a0b4978d820b64b45afd1f155cd6ab
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-azure-powershell"></a>Criar, lista ou eliminar uma identidade de utilizador atribuída com o Azure PowerShell

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido no Azure Active Directory. Pode utilizar esta identidade para se autenticarem em serviços que suportam a autenticação do Azure AD, sem necessitar de credenciais no seu código. 

Neste artigo, irá aprender a criar, listar e eliminar uma identidade de utilizador atribuída com o Azure PowerShell.

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade de serviço geridas, consulte o [secção Descrição geral](overview.md). **Certifique-se rever o [diferença entre um sistema atribuída e o utilizador atribuído identidade](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscrever-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Instalar [a versão mais recente do Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) se ainda não o fez.
- Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer o Azure PowerShell versão do módulo 5.7.0 ou posterior. Executar ` Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="create-a-user-assigned-identity"></a>Criar um utilizador atribuído identidade

Para criar uma identidade de utilizador atribuída, utilize o [New-AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/new-azurermuserassignedidentity) comando. O `ResourceGroupName` parâmetro especifica o grupo de recursos onde criar a identidade do utilizador atribuído e o `-Name` parâmetro especifica o respetivo nome. Substitua o `<RESOURCE GROUP>` e `<USER ASSIGNED IDENTITY NAME>` valores de parâmetros com os seus próprios valores:

> [!IMPORTANT]
> Criar atribuído identidades com carateres especiais (ou seja, um caráter de sublinhado) no nome de utilizador não é atualmente suportado. Utilize apenas carateres alfanuméricos. Verifique novamente para as atualizações.  Para obter mais informações consulte [perguntas mais frequentes e problemas conhecidos](known-issues.md).

 ```azurepowershell-interactive
New-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCEGROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-identities"></a>Atribuído identidades de utilizador de lista

A lista de identidades de utilizador atribuída, utilize o [Get-AzureRmUserAssigned](/powershell/module/azurerm.managedserviceidentity/get-azurermuserassignedidentity) comando.  O `-ResourceGroupName` parâmetro especifica o grupo de recursos em que a identidade de atribuída ao utilizador foi criado.  Substitua o `<RESOURCE GROUP>` com o seu próprio valor:

```azurepowershell-interactive
Get-AzureRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP>
```
A resposta de identidades de utilizador tem `"Microsoft.ManagedIdentity/userAssignedIdentities"` valor devolvido para a chave, `Type`.

`Type :Microsoft.ManagedIdentity/userAssignedIdentities`

## <a name="delete-a-user-assigned-identity"></a>Eliminar um utilizador atribuído identidade

Para eliminar uma identidade de utilizador, utilize o [remover AzureRmUserAssignedIdentity](/powershell/module/azurerm.managedserviceidentity/remove-azurermuserassignedidentity) comando.  O `-ResourceGroupName` parâmetro especifica o grupo de recursos em que a identidade de atribuída ao utilizador foi criado e a `-Name` parâmetro especifica o respetivo nome.  Substitua o `<RESOURCE GROUP>` e `<USER ASSIGNED IDENTITY NAME>` valores de parâmetros com os seus próprios valores:

 ```azurecli-interactive
Remove-AzurRmUserAssignedIdentity -ResourceGroupName <RESOURCE GROUP> -Name <USER ASSIGNED IDENTITY NAME>
```
> [!NOTE]
> Eliminação de um utilizador atribuído identidade não remover a referência de qualquer recurso que foi atribuído ao. Atribuições de identidade tem de ser removida em separado.

## <a name="related-content"></a>Conteúdo relacionado

Para obter mais detalhes sobre os comandos do Azure PowerShell MSI e uma lista completa, consulte [AzureRM.ManagedServiceIdentity](/powershell/module/azurerm.managedserviceidentity#managed_service_identity).


 
