---
title: Como gerir o utilizador do Azure através de REST de identidades geridas de atribuídos
description: Instruções passo a passo obter instruções sobre como criar, listar e eliminar um utilizador atribuído a identidade gerida para fazer chamadas de REST API.
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
ms.date: 06/26/2018
ms.author: daveba
ms.openlocfilehash: 70a8c9018cdc2929abc85336211beecf82bf32cb
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188051"
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-rest-api-calls"></a>Criar, lista ou eliminar uma identidade de utilizador atribuída através de chamadas à REST API

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Identidade gerida fornece a capacidade de autenticar nos serviços de que a autenticação de suporte do Azure AD, sem a necessidade de credenciais no seu código de serviços do Azure. 

Neste artigo, saiba como criar, listar e eliminar um utilizador atribuído a identidade gerida com o CURL para fazer chamadas de REST API.

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade do serviço gerido, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre um sistema atribuído e a identidade atribuída ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Se estiver a utilizar o Windows, instale o [subsistema Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about) ou utilizar o [Azure Cloud Shell](../../cloud-shell/overview.md) no portal do Azure.
- Se utilizar o [subsistema Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about) ou uma [SO de distribuição de Linux](/cli/azure/install-azure-cli-apt?view=azure-cli-latest), [instalar a consola local da CLI do Azure](/azure/install-azure-cli).
- Se estiver a utilizar consola local da CLI do Azure, inicie sessão no Azure com `az login` com uma conta que está associada à subscrição do Azure que pretende implementar ou obter o utilizador atribuído informações de identidade gerida.
- Para efetuar as operações de gestão neste artigo, a conta tem das atribuições de funções seguintes:
    - [Contribuidor de identidade de geridos](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) função para criar, ler (lista), atualizar e eliminar uma identidade atribuída ao utilizador.
    - [Gerido operador de identidade](/azure/role-based-access-control/built-in-roles#managed-identity-operator) função para ler as propriedades de uma identidade de utilizador atribuída de (lista).
- Obter um portador acesso token utilizando `az account get-access-token` efetuar o seguinte utilizador atribuído operações de identidade gerida.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-user-assigned-managed-identity"></a>Criar um utilizador atribuído a identidade gerida 

Para criar um identidade gerida atribuída ao utilizador, utilize o seguinte pedido CURL para a API do Azure Resource Manager. Substitua a `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<USER ASSIGNED IDENTITY NAME>`,`<LOCATION>`, e `<ACCESS TOKEN>` valores pelos seus próprios valores:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X PUT -d '{"loc
ation": "<LOCATION>"}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
```

## <a name="list-user-assigned-managed-identities"></a>Utilizador da lista atribuída identidades geridas

Para listar atribuídos identidades geridas pelo utilizador, utilize o seguinte pedido CURL para a API do Azure Resource Manager. Substitua a `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, e `<ACCESS TOKEN>` valores pelos seus próprios valores:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities?api-version=2015-08-31-preview' -H "Authorization: Bearer <ACCESS TOKEN>"
```
## <a name="delete-a-user-assigned-managed-identity"></a>Eliminar um utilizador atribuído a identidade gerida

Para eliminar um utilizador atribuído a identidade gerida, utilize o seguinte pedido CURL para a API do Azure Resource Manager. Substitua a `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, e `<ACCESS TOKEN>` valores de parâmetros com seus próprios valores:

> [!NOTE]
> A eliminar um identidade atribuída ao utilizador não remover a referência de qualquer recurso que foi atribuído a. Para remover um utilizador atribuído gerida a partir de uma VM com o CURL ver [remover uma identidade atribuída ao utilizador a partir de uma VM do Azure](qs-configure-rest-vm.md#remove-a-user-assigned identity-from-an-azure-vm).

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroup
s/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>?api-version=2015-08-31-preview' -X DELETE -H "Authorization: Bearer <ACCESS TOKEN>"
```

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre como atribuir uma identidade atribuída ao utilizador para um Azure VM/VMSS com o CURL, consulte [configurar a identidade gerida numa VM do Azure com o CURL](qs-configure-rest-vm.md#user-assigned-identity) e [Configurar identidade gerido num conjunto de dimensionamento com o CURL ](qs-configure-rest-vmss.md#user-assigned-identity).


