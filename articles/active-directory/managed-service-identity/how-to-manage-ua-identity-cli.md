---
title: Como gerir um utilizador atribuído Managed Service Identity (MSI) com a CLI do Azure
description: Instruções passo a passo obter instruções sobre como criar, listar e eliminar um utilizador atribuído serviço de identidade com a CLI do Azure gerido.
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
ms.openlocfilehash: 5deaace49bfff994defc06a5f60597add6affc0b
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188153"
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-the-azure-cli"></a>Criar, lista ou eliminar um utilizador atribuído a identidade com a CLI do Azure

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida no Azure Active Directory. Pode utilizar esta identidade para autenticar para serviços que suportam a autenticação do Azure AD, sem a necessidade de credenciais no seu código. 

Neste artigo, saiba como criar, listar e eliminar uma identidade atribuída ao utilizador com a CLI do Azure.

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade do serviço gerido, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre um sistema atribuído e a identidade atribuída ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para efetuar as operações de gestão neste artigo, a conta tem das atribuições de funções seguintes:
    - [Contribuidor de identidade de geridos](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) função para criar, ler (lista), atualizar e eliminar uma identidade atribuída ao utilizador.
    - [Gerido operador de identidade](/azure/role-based-access-control/built-in-roles#managed-identity-operator) função para ler as propriedades de uma identidade de utilizador atribuída de (lista).
- Para executar os exemplos de script da CLI, tem três opções:
    - Uso [Azure Cloud Shell](../../cloud-shell/overview.md) do portal do Azure (consulte a secção seguinte).
    - Utilize o embedded Azure Cloud Shell através do "Experimente-lo" botão do, localizado no canto superior direito de cada bloco de código.
    - [Instalar a versão mais recente da CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 ou posterior) se preferir utilizar uma consola CLI local. Inicie sessão no Azure com `az login`, utilizando uma conta que está associada à subscrição do Azure sob a qual pretende implementar a identidade atribuída ao utilizador.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Criar um utilizador atribuído a identidade gerida 

Para criar uma identidade atribuída ao utilizador, utilize o [identidade az criar](/cli/azure/identity#az-identity-create) comando. O `-g` parâmetro especifica o grupo de recursos onde criar a identidade atribuída ao utilizador e o `-n` parâmetro especifica o respetivo nome. Substitua a `<RESOURCE GROUP>` e `<USER ASSIGNED IDENTITY NAME>` valores de parâmetros com seus próprios valores:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

 ```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
```
## <a name="list-user-assigned-identities"></a>Utilizador de lista identidades atribuído

Para listar as identidades atribuídas por utilizadores, utilize o [lista de identidade az](/cli/azure/identity#az-identity-list) comando. Substitua o `<RESOURCE GROUP>` com seu próprio valor:

```azurecli-interactive
az identity list -g <RESOURCE GROUP>
```
Na resposta json, tem identidades de utilizador `"Microsoft.ManagedIdentity/userAssignedIdentities"` valor retornado para a chave, `type`.

`"type": "Microsoft.ManagedIdentity/userAssignedIdentities"`

## <a name="delete-a-user-assigned-identity"></a>Eliminar um identidade atribuída ao utilizador

Para eliminar uma identidade atribuída ao utilizador, utilize o [identidade az eliminar](/cli/azure/identity#az-identity-delete) comando.  O parâmetro - n Especifica o nome e o parâmetro -g Especifica o grupo de recursos em que a identidade atribuída ao utilizador foi criado. Substitua a `<USER ASSIGNED IDENTITY NAME>` e `<RESOURCE GROUP>` valores de parâmetros com seus próprios valores:

 ```azurecli-interactive
az identity delete -n <USER ASSIGNED IDENTITY NAME> -g <RESOURCE GROUP>
```
> [!NOTE]
> A eliminar um identidade atribuída ao utilizador não remover a referência de qualquer recurso que foi atribuído a. Remova os da VM/VMSS usando o `az vm/vmss identity remove` comando

## <a name="related-content"></a>Conteúdo relacionado

Para obter uma lista completa dos comandos de identidade da CLI do Azure, consulte [identidade az](/cli/azure/identity).

Para obter informações sobre como atribuir uma identidade atribuída ao utilizador para ver uma VM do Azure, [configurar Managed Service Identity (MSI) com a CLI do Azure](qs-configure-cli-windows-vm.md#user-assigned-identity)


 
