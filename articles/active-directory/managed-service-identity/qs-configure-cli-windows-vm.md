---
title: Como configurar o sistema e de utilizador atribuída identidades numa VM do Azure utilizando a CLI do Azure
description: Passo a passo instruções para configurar um sistema e o utilizador atribuído identidades numa VM do Azure, utilizando a CLI do Azure.
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
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 0f167fef2303ff3b552b9924db3eae9a07c1cb69
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="configure-managed-service-identity-msi-on-an-azure-vm-using-azure-cli"></a>Configure a identidade de serviço geridas (MSI) na VM do Azure utilizando a CLI do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter de credenciais no seu código. 

Neste artigo, irá aprender a realizar as seguintes operações de identidade de serviço gerida numa VM do Azure, utilizando a CLI do Azure:
- Ativar e desativar o sistema atribuído identidade numa VM do Azure
- Adicionar e remover um utilizador atribuído identidade numa VM do Azure

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade de serviço geridas, consulte o [secção Descrição geral](overview.md). **Certifique-se rever o [diferença entre um sistema atribuída e o utilizador atribuído identidade](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscrever-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para executar os exemplos de script do CLI, tem três opções:

    - Utilize [Shell de nuvem do Azure](../../cloud-shell/overview.md) do portal do Azure (consulte a secção seguinte).
    - Utilize a Shell de nuvem do Azure incorporados através de "Tente-" botão, localizado no canto superior direito de cada bloco de código.
    - [Instale a versão mais recente do CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 ou posterior) se preferir utilizar uma consola local do CLI. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Atribuído a identidade de sistema

Nesta secção, saiba como ativar e desativar o sistema atribuído identidade numa VM do Azure utilizando a CLI do Azure.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Ativar o sistema atribuído identidade durante a criação de uma VM do Azure

Para criar uma VM do Azure com o sistema atribuído identidade ativada:

1. Se estiver a utilizar a CLI do Azure numa consola local, primeiro inicie sessão no Azure com [início de sessão az](/cli/azure/reference-index#az_login). Utilize uma conta que está associada à subscrição do Azure na qual gostaria de implementar a VM:

   ```azurecli-interactive
   az login
   ```

2. Criar um [grupo de recursos](../../azure-resource-manager/resource-group-overview.md#terminology) para a implementação da VM e os respetivos recursos relacionados, utilizar e contenção [criar grupo az](/cli/azure/group/#az_group_create). Pode ignorar este passo se já tiver o grupo de recursos que pretende utilizar em vez disso:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Criar uma VM utilizando [az vm criar](/cli/azure/vm/#az_vm_create). O exemplo seguinte cria uma VM chamada *myVM* com uma identidade de sistema atribuída, conforme solicitado pelo `--assign-identity` parâmetro. O `--admin-username` e `--admin-password` parâmetros especificam a conta de nome e palavra-passe de utilizador administrativo para início de sessão na máquina virtual. Atualize estes valores conforme adequado para o seu ambiente: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Ativar o sistema atribuído identidade numa VM do Azure existente

Se precisar de ativar a identidade de sistema atribuída de VM existente:

1. Se estiver a utilizar a CLI do Azure numa consola local, primeiro inicie sessão no Azure com [início de sessão az](/cli/azure/reference-index#az_login). Utilize uma conta que está associada à subscrição do Azure que contém a VM. Certifique-se a sua conta pertencer a uma função que dá-lhe também permissões de escrita na VM, tais como "Contribuinte de Máquina Virtual":

   ```azurecli-interactive
   az login
   ```

2. Utilize [identidade de vm az atribuir](/cli/azure/vm/identity/#az_vm_identity_assign) com o `identity assign` comando ativar a identidade do sistema atribuído a uma VM existente:

   ```azurecli-interactive
   az vm identity assign -g myResourceGroup -n myVm
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-vm"></a>Desativar o sistema atribuído a identidade de uma VM do Azure

> [!NOTE]
> Desativar a identidade de serviço gerida de uma Máquina Virtual não é atualmente suportada. Entretanto, pode alternar entre a utilização do sistema atribuídas e identidades atribuídas do utilizador.

Se tiver uma Máquina Virtual que já não tem o sistema atribuído de identidade, mas ainda tem atribuído identidades de utilizador, utilize o seguinte comando:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```
Para remover a extensão de VM de MSI, utilizador `-n ManagedIdentityExtensionForWindows` ou `-n ManagedIdentityExtensionForLinux` mudar (dependendo do tipo de VM) com [delete de extensão de vm az](https://docs.microsoft.com/cli/azure/vm/#assign-identity):

```azurecli-interactive
az vm identity --resource-group myResourceGroup --vm-name myVm -n ManagedIdentityExtensionForWindows
```

## <a name="user-assigned-identity"></a>Identidade do utilizador atribuído

Nesta secção, irá aprender como adicionar e remover um utilizador atribuído a identidade da VM do Azure, utilizando a CLI do Azure.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vm"></a>Atribuir um utilizador atribuído identidade durante a criação de uma VM do Azure

Esta secção explica a criação de uma VM com a atribuição de um utilizador atribuído a identidade. Se já tiver uma VM que pretende utilizar, ignore esta secção e avance para o seguinte.

1. Pode ignorar este passo se já tiver um grupo de recursos que pretende utilizar. Criar um [grupo de recursos](~/articles/azure-resource-manager/resource-group-overview.md#terminology) para a implementação do seu MSI e contenção utilizando [criar grupo az](/cli/azure/group/#az_group_create). Não se esqueça de substituir o `<RESOURCE GROUP>` e `<LOCATION>` valores de parâmetros com os seus próprios valores. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Criar um utilizador atribuído através de identidade [identidade az criar](/cli/azure/identity#az_identity_create).  O `-g` parâmetro especifica o grupo de recursos onde será criado a identidade de atribuída ao utilizador, e o `-n` parâmetro especifica o respetivo nome.
    
    > [!IMPORTANT]
    > Criar atribuído identidades com carateres especiais (ou seja, um caráter de sublinhado) no nome de utilizador não é atualmente suportado. Utilize apenas carateres alfanuméricos. Verifique novamente para as atualizações.  Para obter mais informações consulte [perguntas mais frequentes e problemas conhecidos](known-issues.md)

    ```azurecli-interactive
    az identity create -g myResourceGroup -n myUserAssignedIdentity
    ```
A resposta contém detalhes para a identidade de utilizador atribuído criado, semelhante ao seguinte. O valor de id de recurso atribuído ao utilizador atribuído identidade é utilizado no passo seguinte.

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

3. Criar uma VM utilizando [az vm criar](/cli/azure/vm/#az_vm_create). O exemplo seguinte cria uma VM associada à identidade atribuída ao utilizador nova, tal como especificado pelo `--assign-identity` parâmetro. Não se esqueça de substituir o `<RESOURCE GROUP>`, `<VM NAME>`, `<USER NAME>`, `<PASSWORD>`, e `<MSI ID>` valores de parâmetros com os seus próprios valores. Para `<MSI ID>`, utilize o recurso a identidade de utilizador atribuído `id` propriedade criada no passo anterior: 

   ```azurecli-interactive 
   az vm create --resource-group <RESOURCE GROUP> --name <VM NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <MSI ID>
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Atribuir um utilizador atribuído a identidade para uma VM do Azure existente

1. Criar um utilizador atribuído através de identidade [identidade az criar](/cli/azure/identity#az-identity-create).  O `-g` parâmetro especifica o grupo de recursos onde será criado a identidade de atribuída ao utilizador, e o `-n` parâmetro especifica o respetivo nome. Não se esqueça de substituir o `<RESOURCE GROUP>` e `<MSI NAME>` valores de parâmetros com os seus próprios valores:

    > [!IMPORTANT]
    > Criar atribuído identidades com carateres especiais (ou seja, um caráter de sublinhado) no nome de utilizador não é atualmente suportado. Utilize apenas carateres alfanuméricos. Verifique novamente para as atualizações.  Para obter mais informações consulte [perguntas mais frequentes e problemas conhecidos](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```
A resposta contém detalhes para o utilizador atribuído MSI criado, semelhante ao seguinte. O recurso `id` valor atribuído à identidade do utilizador atribuído é utilizada no passo seguinte.

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

2. Atribua a identidade do utilizador atribuído à sua utilização de VM [identidade de vm az atribuir](/cli/azure/vm#az-vm-identity-assign). Não se esqueça de substituir o `<RESOURCE GROUP>` e `<VM NAME>` valores de parâmetros com os seus próprios valores. O `<MSI ID>` será recursos a identidade de utilizador atribuída `id` propriedade, como criado no passo anterior:

    ```azurecli-interactive
    az vm identity assign -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI ID>
    ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vm"></a>Remover um utilizador atribuído a identidade da VM do Azure

> [!NOTE]
> Remover todas as identidades de utilizador atribuído a partir de uma Máquina Virtual atualmente não é suportada, a menos que tenha um sistema atribuído de identidade.

Se a VM tem múltiplas identidades de utilizador atribuído, pode remover todas, exceto os últimos utilizando [remover a identidade de vm az](/cli/azure/vm#az-vm-identity-remove). Não se esqueça de substituir o `<RESOURCE GROUP>` e `<VM NAME>` valores de parâmetros com os seus próprios valores. O `<MSI NAME>` será a identidade de utilizador atribuída `name` propriedade, o que pode encontrar, na secção da identidade da VM utilizando `az vm show`:

```azurecli-interactive
az vm identity remove -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI NAME>
```
Se a VM possui sistema atribuída e atribuído identidades de utilizador, pode remover todos os a atribuída ao utilizador identidades por mudar para utilizar apenas o sistema atribuído. Utilize o seguinte comando:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='SystemAssigned' identity.identityIds=null 
```

## <a name="related-content"></a>Conteúdo relacionado
- [Descrição geral de identidade de serviço gerida](overview.md)
- Para a criação de VM do Azure completa inícios rápidos, consulte: 
  - [Criar uma máquina virtual do Windows com a CLI](../../virtual-machines/windows/quick-create-cli.md)  
  - [Criar uma máquina virtual Linux com a CLI](../../virtual-machines/linux/quick-create-cli.md) 

















