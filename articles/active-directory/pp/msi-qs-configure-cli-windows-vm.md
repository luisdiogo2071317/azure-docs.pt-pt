---
title: "Como configurar um MSI atribuído por utilizador para uma VM do Azure utilizando a CLI do Azure"
description: "Passo pelo passo as instruções para configurar um utilizador atribuído geridos serviço de identidade (MSI) para uma VM do Azure, utilizando a CLI do Azure."
services: active-directory
documentationcenter: 
author: BryanLa
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
ms.openlocfilehash: 4b6f4e2b0e42724276448fd4726c8326de8ea6ee
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/09/2018
---
# <a name="configure-a-user-assigned-managed-service-identity-msi-for-a-vm-using-azure-cli"></a>Configurar um utilizador atribuído geridos serviço de identidade (MSI) para uma VM, utilizando a CLI do Azure

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido no Azure Active Directory. Pode utilizar esta identidade para se autenticarem em serviços que suportam a autenticação do Azure AD, sem necessitar de credenciais no seu código. 

Neste artigo, irá aprender como ativar e remover um MSI atribuído por utilizador para uma VM do Azure, utilizando a CLI do Azure.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

Para executar os exemplos de script CLI neste tutorial, tem duas opções:

- Utilize [Shell de nuvem do Azure](~/articles/cloud-shell/overview.md) do portal do Azure ou através do botão "Tente-", localizada no canto superior direito de cada bloco de código.
- [Instale a versão mais recente do CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 ou posterior) se preferir utilizar uma consola local do CLI. Em seguida, inicie sessão no Azure com [início de sessão az](/cli/azure/#login). Utilize uma conta que está associada à subscrição do Azure na qual gostaria de implementar o MSI atribuída de utilizador e a VM:

   ```azurecli
   az login
   ```

## <a name="enable-msi-during-creation-of-an-azure-vm"></a>Ativar MSI durante a criação de uma VM do Azure

Esta secção explica a criação da VM e atribuição do MSI utilizador atribuído à VM. Se já tiver uma VM que pretende utilizar, ignore esta secção e avance para o seguinte.

1. Pode ignorar este passo se já tiver um grupo de recursos que pretende utilizar. Criar um [grupo de recursos](~/articles/azure-resource-manager/resource-group-overview.md#terminology) para a implementação do seu MSI e contenção utilizando [criar grupo az](/cli/azure/group/#create). Não se esqueça de substituir o `<RESOURCE GROUP>` e `<LOCATION>` valores de parâmetros com os seus próprios valores. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Criar um utilizador atribuído MSI, utilizando [identidade az criar](/cli/azure/identity#az_identity_create).  O `-g` parâmetro especifica o grupo de recursos onde o MSI é criado, e o `-n` parâmetro especifica o respetivo nome. Não se esqueça de substituir o `<RESOURCE GROUP>` e `<MSI NAME>` valores de parâmetros com os seus próprios valores:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```
A resposta contém detalhes para o utilizador atribuído do MSI criado, semelhante ao seguinte. O recurso `id` valor atribuído à MSI é utilizada no passo seguinte.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
        "location": "westcentralus",
        "name": "<MSI NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

3. Criar uma VM utilizando [az vm criar](/cli/azure/vm/#create). O exemplo seguinte cria uma VM associada novo utilizador atribuído do MSI, tal como especificado pelo `--assign-identity` parâmetro. Não se esqueça de substituir o `<RESOURCE GROUP>`, `<VM NAME>`, `<USER NAME>`, `<PASSWORD>`, e `<`MSI ID >` parameter values with your own values. For `<MSI ID>`, use the user-assigned MSI's resource `id' propriedade criada no passo anterior: 

   ```azurecli-interactive 
   az vm create --resource-group <RESOURCE GROUP> --name <VM NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <MSI ID>
   ```

## <a name="enable-msi-on-an-existing-azure-vm"></a>Ativar MSI numa VM do Azure existente

1. Criar um utilizador atribuído MSI, utilizando [identidade az criar](/cli/azure/identity#az_identity_create).  O `-g` parâmetro especifica o grupo de recursos onde o MSI é criado, e o `-n` parâmetro especifica o respetivo nome. Não se esqueça de substituir o `<RESOURCE GROUP>` e `<MSI NAME>` valores de parâmetros com os seus próprios valores:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```
A resposta contém detalhes para o utilizador atribuído do MSI criado, semelhante ao seguinte. O recurso `id` valor atribuído à MSI é utilizada no passo seguinte.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
        "location": "westcentralus",
        "name": "<MSI NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

2. Atribuir o MSI utilizador atribuído à sua utilização de VM [az vm atribuir-identity](/cli/azure/vm#az_vm_assign_identity). Não se esqueça de substituir o `<RESOURCE GROUP>` e `<VM NAME>` valores de parâmetros com os seus próprios valores. O `<MSI ID>` será recurso o MSI utilizador atribuído `id` propriedade, como criado no passo anterior:

    ```azurecli-interactive
    az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI ID>
    ```

## <a name="remove-msi-from-an-azure-vm"></a>Remover MSI a partir de uma VM do Azure

1. Remover utilizador atribuído do MSI da VM utilizando [az vm remove-identity](/cli/azure/vm#az_vm_remove_identity). Não se esqueça de substituir o `<RESOURCE GROUP>` e `<VM NAME>` valores de parâmetros com os seus próprios valores. O `<MSI NAME>` será o MSI utilizador atribuído `name` propriedade, como especificado durante o `az identity create` comando (Consulte exemplos nas secções anteriores):

   ```azurecli-interactive
   az vm remove-identity -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI NAME>
   ```

## <a name="next-steps"></a>Passos Seguintes

- [Descrição geral de identidade de serviço gerida](msi-overview.md)
- Para a criação de VM do Azure completa inícios rápidos, consulte: 

  - [Criar uma máquina virtual do Windows com a CLI](~/articles/virtual-machines/windows/quick-create-cli.md)  
  - [Criar uma máquina virtual Linux com a CLI](~/articles/virtual-machines/linux/quick-create-cli.md) 

Utilize a seguinte secção de comentários para fornecer comentários e ajudam-nos refinar e formam o nosso conteúdo.
















