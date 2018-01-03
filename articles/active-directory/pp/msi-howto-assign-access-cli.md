---
title: "Como atribuir um acesso MSI utilizador atribuído a um recurso do Azure, utilizando a CLI do Azure"
description: "Passo a passo instruções para atribuir um MSI utilizador atribuído um recurso, aceder a outro recurso, utilizando a CLI do Azure."
services: active-directory
documentationcenter: 
author: bryanLa
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 732fc1981bdf95e7548ea0ebe0ca8ece00f483ce
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/22/2017
---
# <a name="assign-a-user-assigned-managed-service-identity-msi-access-to-a-resource-using-azure-cli"></a>Atribuir um acesso de identidade de serviço geridas (MSI) utilizador atribuído a um recurso, utilizando a CLI do Azure

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Assim que tiver criado um MSI atribuído por utilizador, é possível conceder o acesso do MSI para outro recurso, tal como qualquer principal de segurança. Este exemplo mostra como permitir que um utilizador atribuído MSI aceda a uma conta de armazenamento do Azure, utilizando a CLI do Azure.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Para executar os exemplos de script CLI neste tutorial, tem duas opções:

- Utilize [Shell de nuvem do Azure](~/articles/cloud-shell/overview.md) do portal do Azure ou através do botão "Tente-", localizada no canto superior direito de cada bloco de código.
- [Instale a versão mais recente do CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 ou posterior) se preferir utilizar uma consola local do CLI. Em seguida, inicie sessão no Azure com [início de sessão az](/cli/azure/#login). Utilize uma conta que está associada à subscrição do Azure na qual gostaria de implementar o MSI atribuída de utilizador e a VM:

   ```azurecli
   az login
   ```

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Utilizar o RBAC para atribuir o acesso MSI a outro recurso

Para utilizar um MSI com um recurso de anfitrião (por exemplo, uma VM), para acesso de início de sessão ou recurso, o MSI tem primeiro ser concedido acesso a recursos através de atribuição de função. Pode fazê-lo antes de associar o MSI com o anfitrião, ou depois. Para concluir os passos sobre como criar e associar uma VM, consulte [configurar um MSI atribuído por utilizador para uma VM, utilizando a CLI do Azure](msi-qs-configure-cli-windows-vm.md).

No exemplo seguinte, um MSI utilizador atribuído é dado acesso a uma conta de armazenamento.  

1. Para conceder o acesso MSI, terá do ID de cliente (também conhecido como ID de aplicação) do principal de serviço do MSI. O cliente ID fornecido pelo [identidade az criar](/cli/azure/identity#az_identity_create), após o aprovisionamento do MSI e o respetivo principal de serviço (mostrado aqui para referência):

   ```azurecli-interactive
   az identity create -g rgPrivate -n msiServiceApp
   ```

   Uma resposta com êxito contém o ID de cliente, do utilizador atribuído do MSI principal do serviço, no `clientId` propriedade:

   ```json
   {
        "clientId": "9391e5b1-dada-4a8z-834a-43ad44f296bl",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiServiceApp/credentials?tid=733a8f0p-ec41-4e69-8adz-971fc4b533bl&oid=z4baa4fc-fce4-4202-8250-5fb359abblll&aid=9391e5b1-dada-4a8z-834a-43ad44f296bl",
        "id": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.ManagedIdentity/userAssignedIdentities/msiServiceApp",
        "location": "westcentralus",
        "name": "msiServiceApp",
        "principalId": "z4baa4fc-fce4-4202-8250-5fb359abblll",
        "resourceGroup": "rgPrivate",
        "tags": {},
        "tenantId": "733a8f0p-ec41-4e69-8adz-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
   }
   ```

2. Assim que o ID de cliente é conhecido, utilize [de criação da atribuição de função az](/cli/azure/role/assignment#az_role_assignment_create) para atribuir o acesso MSI a outro recurso. Certifique-se de utilizar o seu ID de cliente para o `--assignee` parâmetro e o grupo de recursos e ID de subscrição correspondente nome, tal como devolvido quando executar `az identity create`. Aqui o MSI é atribuído acesso de "Leitor" a uma conta de armazenamento denominada "myStorageAcct":

   ```azurecli-interactive
   az role assignment create --assignee 9391e5b1-dada-4a8z-834a-43ad44f296bl --role Reader --scope /subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.Storage/storageAccounts/myStorageAcct
   ```

   Uma resposta com êxito semelhante ao seguinte saída:

   ```json
   {
        "id": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.Storage/storageAccounts/myStorageAcct/providers/Microsoft.Authorization/roleAssignments/f4766864-9493-43c6-91d7-abd131c3c017",
        "name": "f4766864-9493-43c6-91d7-abd131c3c017",
        "properties": {
            "additionalProperties": {
            "createdBy": null,
            "createdOn": "2017-12-21T20:49:37.5590544Z",
            "updatedBy": "pd78b21f-17a4-41az-b7db-9aadec3376bl",
            "updatedOn": "2017-12-21T20:49:37.5590544Z"
            },
        "principalId": "z4baa4fc-fce4-4202-8250-5fb359abblll",
        "roleDefinitionId": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "scope": "/subscriptions/90z696ff-5efa-4909-a64d-f1b616f423ll/resourcegroups/rgPrivate/providers/Microsoft.Storage/storageAccounts/myStorageAcct"
        },
        "resourceGroup": "rgPrivate",
        "type": "Microsoft.Authorization/roleAssignments"
   }
   ```

## <a name="next-steps"></a>Passos Seguintes

- Para obter uma descrição geral do MSI, consulte [descrição geral de identidade de serviço geridas](msi-overview.md).
- Para ativar um MSI utilizador atribuído numa VM do Azure, consulte [configurar um utilizador atribuído geridos serviço de identidade (MSI) para uma VM, utilizando a CLI do Azure](msi-qs-configure-cli-windows-vm.md).

Utilize a seguinte secção de comentários para fornecer comentários e ajudam-nos refinar e formam o nosso conteúdo.

