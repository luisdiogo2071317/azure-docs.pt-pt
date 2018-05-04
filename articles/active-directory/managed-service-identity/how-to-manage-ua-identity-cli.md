---
title: Como gerir um utilizador atribuído geridos serviço de identidade (MSI) utilizando a CLI do Azure
description: Passo a passo sobre como criar, listar e eliminar um utilizador atribuído de serviço geridas de identidade com a CLI do Azure.
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
ms.openlocfilehash: ebd250a7006c1235b30eb195f4bf7383b6293022
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-the-azure-cli"></a>Criar, lista ou eliminar um utilizador atribuído a identidade com a CLI do Azure

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido no Azure Active Directory. Pode utilizar esta identidade para se autenticarem em serviços que suportam a autenticação do Azure AD, sem necessitar de credenciais no seu código. 

Neste artigo, irá aprender a criar, listar e eliminar uma identidade de utilizador atribuída ao utilizar a CLI do Azure.

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade de serviço geridas, consulte o [secção Descrição geral](overview.md). **Certifique-se rever o [diferença entre um sistema atribuída e o utilizador atribuído identidade](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscrever-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.

- Para executar os exemplos de script do CLI, tem três opções:

    - Utilize [Shell de nuvem do Azure](../../cloud-shell/overview.md) do portal do Azure (consulte a secção seguinte).
    - Utilize a Shell de nuvem do Azure incorporados através de "Tente-" botão, localizado no canto superior direito de cada bloco de código.
    - [Instale a versão mais recente do CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 ou posterior) se preferir utilizar uma consola local do CLI. Inicie sessão no Azure com `az login`, utilizando uma conta que está associada à subscrição do Azure na qual gostaria de implementar a identidade do utilizador atribuído.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Criar um utilizador atribuído identidade gerida 

Para criar uma identidade de utilizador atribuída, utilize o [identidade az criar](/cli/azure/identity#az-identity-create) comando. O `-g` parâmetro especifica o grupo de recursos onde criar a identidade do utilizador atribuído e o `-n` parâmetro especifica o respetivo nome. Substitua o `<RESOURCE GROUP>` e `<USER ASSIGNED IDENTITY NAME>` valores de parâmetros com os seus próprios valores:

> [!IMPORTANT]
> Criar atribuído identidades com carateres especiais (ou seja, um caráter de sublinhado) no nome de utilizador não é atualmente suportado. Utilize apenas carateres alfanuméricos. Verifique novamente para as atualizações.  Para obter mais informações consulte [perguntas mais frequentes e problemas conhecidos](known-issues.md)

 ```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-identities"></a>Atribuído identidades de utilizador de lista

A lista de identidades de utilizador atribuída, utilize o [lista de identidade az](/cli/azure/identity#az-identity-list) comando.  O `-g` parâmetro especifica o grupo de recursos em que a identidade de atribuída ao utilizador foi criado.  Substitua o `<RESOURCE GROUP>` com o seu próprio valor:

```azurecli-interactive
az identity list -g <RESOURCE GROUP>
```
Resposta json, identidades de utilizador tem `"Microsoft.ManagedIdentity/userAssignedIdentities"` valor devolvido para a chave, `type`.

`"type": "Microsoft.ManagedIdentity/userAssignedIdentities"`

## <a name="delete-a-user-assigned-identity"></a>Eliminar um utilizador atribuído identidade

Para eliminar uma identidade de utilizador atribuída, utilize o [identidade az eliminar](/cli/azure/identity#az-identity-delete) comando.  O parâmetro - n Especifica o nome e o parâmetro -g Especifica o grupo de recursos em que a identidade de atribuída ao utilizador foi criado.  Substitua o `<USER ASSIGNED IDENTITY NAME>` e `<RESOURCE GROUP>` valores de parâmetros com os seus próprios valores:

 ```azurecli-interactive
az identity delete -n <USER ASSIGNED IDENTITY NAME> -g <RESOURCE GROUP>
```
> [!NOTE]
> Eliminação de um utilizador atribuído identidade não remover a referência de qualquer recurso que foi atribuído ao. Remova as da VM/VMSS utilizando o `az vm/vmss identity remove` comando

## <a name="related-content"></a>Conteúdo relacionado

Para obter uma lista completa dos comandos de identidade da CLI do Azure, consulte [identidade az](/cli/azure/identity).

Para obter informações sobre como atribuir uma identidade de utilizador atribuída para ver uma VM do Azure, [configurar geridos serviço de identidade (MSI) utilizando a CLI do Azure](qs-configure-cli-windows-vm.md#user-assigned-identity)


 
