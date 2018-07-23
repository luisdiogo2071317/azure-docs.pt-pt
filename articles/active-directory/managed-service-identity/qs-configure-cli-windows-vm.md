---
title: Como configurar o sistema e do usuário atribuído identidades na VM do Azure com a CLI do Azure
description: Instruções passo a passo instruções para configurar um sistema e um utilizador atribuído identidades na VM do Azure, cli do Azure.
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
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 064c2dfe625435d3faddae45c413d6b91eac8346
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188034"
---
# <a name="configure-managed-service-identity-msi-on-an-azure-vm-using-azure-cli"></a>Configurar a identidade de serviço gerida (MSI) na VM do Azure com a CLI do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, aprenderá a efetuar as seguintes operações de identidade do serviço gerido na VM do Azure, com a CLI do Azure:
- Ativar e desativar o sistema de identidade numa VM do Azure atribuído
- Adicionar e remover um utilizador atribuído a identidade numa VM do Azure

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade do serviço gerido, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre um sistema atribuído e a identidade atribuída ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para efetuar as operações de gestão neste artigo, a conta tem das atribuições de funções seguintes:
    - [Contribuinte de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para criar uma VM e ativar e remover sistema atribuído a identidade gerida a partir de uma VM do Azure.
    - [Contribuidor de identidade de geridos](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) função para criar uma identidade atribuída ao utilizador.
    - [Gerido operador de identidade](/azure/role-based-access-control/built-in-roles#managed-identity-operator) função para atribuir e remover uma identidade de utilizador atribuída de e para uma VM.
- Para executar os exemplos de script da CLI, tem três opções:
    - Uso [Azure Cloud Shell](../../cloud-shell/overview.md) do portal do Azure (consulte a secção seguinte).
    - Utilize o embedded Azure Cloud Shell através do "Experimente-lo" botão do, localizado no canto superior direito de cada bloco de código.
    - [Instalar a versão mais recente da CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 ou posterior) se preferir utilizar uma consola CLI local. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Sistema de identidade atribuído

Nesta secção, saiba como ativar e desativar o sistema de identidade na VM do Azure com a CLI do Azure atribuído.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Ativar o sistema de identidade atribuído durante a criação de uma VM do Azure

Para criar uma VM do Azure com o sistema de identidade ativada atribuído:

1. Se estiver a utilizar a CLI do Azure numa consola local, primeiro inicie sessão no Azure com [az login](/cli/azure/reference-index#az_login). Utilize uma conta que esteja associada à subscrição do Azure na qual pretende implementar a VM:

   ```azurecli-interactive
   az login
   ```

2. Crie uma [grupo de recursos](../../azure-resource-manager/resource-group-overview.md#terminology) para contenção e implementação da VM e os respetivos recursos relacionados, utilizando [az group create](/cli/azure/group/#az_group_create). Pode ignorar este passo se já tiver o grupo de recursos que pretende utilizar em vez disso:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Crie uma VM com [az vm create](/cli/azure/vm/#az_vm_create). O exemplo seguinte cria uma VM com o nome *myVM* com uma identidade de sistema atribuído, conforme solicitado pelo `--assign-identity` parâmetro. Os parâmetros `--admin-username` e `--admin-password` especificam o nome e a palavra-passe da conta de utilizador administrativo para início de sessão na máquina virtual. Atualize estes valores conforme adequado para o seu ambiente: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Ativar o sistema de identidade numa VM do Azure existente atribuído

Se precisar de ativar a identidade do sistema atribuído numa VM existente:

1. Se estiver a utilizar a CLI do Azure numa consola local, primeiro inicie sessão no Azure com [az login](/cli/azure/reference-index#az_login). Utilize uma conta que está associada à subscrição do Azure que contém a VM.

   ```azurecli-interactive
   az login
   ```

2. Uso [atribuir identidade da vm de az](/cli/azure/vm/identity/#az_vm_identity_assign) com o `identity assign` comando ativar a identidade de sistema atribuído a uma VM existente:

   ```azurecli-interactive
   az vm identity assign -g myResourceGroup -n myVm
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-vm"></a>Desativar o sistema de identidade atribuído a partir de uma VM do Azure

> [!NOTE]
> A desativar a identidade de serviço gerida de uma Máquina Virtual não é atualmente suportada. Entretanto, pode alternar entre a utilização do sistema atribuído e identidades de utilizador atribuída.

Se tiver uma Máquina Virtual que já não tem o sistema de identidade atribuído, mas ainda precisa de identidades de atribuída ao utilizador, utilize o seguinte comando:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```
Para remover a extensão de VM de MSI, usuário `-n ManagedIdentityExtensionForWindows` ou `-n ManagedIdentityExtensionForLinux` mudar (consoante o tipo de VM) com [delete de extensão az vm](https://docs.microsoft.com/cli/azure/vm/#assign-identity):

```azurecli-interactive
az vm identity --resource-group myResourceGroup --vm-name myVm -n ManagedIdentityExtensionForWindows
```

## <a name="user-assigned-identity"></a>Identidade do utilizador atribuído

Nesta secção, irá aprender como adicionar e remover um utilizador atribuído a identidade da VM do Azure com a CLI do Azure.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vm"></a>Atribuir um utilizador a identidade atribuído durante a criação de uma VM do Azure

Esta secção orienta-o através da criação de uma VM com a atribuição de uma identidade atribuída ao utilizador. Se já tiver uma VM que pretende utilizar, ignore esta secção e avance para a próxima.

1. Pode ignorar este passo se já tiver um grupo de recursos que pretende utilizar. Criar uma [grupo de recursos](~/articles/azure-resource-manager/resource-group-overview.md#terminology) para a contenção e a implantação de seu MSI, usando [criar grupo az](/cli/azure/group/#az_group_create). Certifique-se de que substitui os valores de parâmetros `<RESOURCE GROUP>` e `<LOCATION>` pelos seus próprios valores. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Criar um utilizador atribuído através de identidade [identidade az criar](/cli/azure/identity#az_identity_create).  O `-g` parâmetro especifica o grupo de recursos onde o identidade atribuída ao utilizador é criado, e o `-n` parâmetro especifica o respetivo nome.    
    
[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]


```azurecli-interactive
az identity create -g myResourceGroup -n myUserAssignedIdentity
```
A resposta contém detalhes para a identidade atribuída ao utilizador que criou, semelhante ao seguinte. O valor de id de recurso atribuído para o identidade atribuída ao utilizador é utilizado no passo seguinte.

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

3. Crie uma VM com [az vm create](/cli/azure/vm/#az_vm_create). O exemplo seguinte cria uma VM associada à identidade atribuída ao utilizador novo, como especificado pelo `--assign-identity` parâmetro. Certifique-se de que substitui os valores de parâmetros `<RESOURCE GROUP>`, `<VM NAME>`, `<USER NAME>`, `<PASSWORD>` e `<MSI ID>` pelos seus próprios valores. Para `<MSI ID>`, utilizar o recurso a identidade de utilizador atribuída `id` propriedade criada no passo anterior: 

```azurecli-interactive 
az vm create --resource-group <RESOURCE GROUP> --name <VM NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <MSI ID>
```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Atribuir um utilizador a identidade atribuído a uma VM do Azure existente

1. Criar um utilizador atribuído através de identidade [identidade az criar](/cli/azure/identity#az-identity-create).  O `-g` parâmetro especifica o grupo de recursos onde o identidade atribuída ao utilizador é criado, e o `-n` parâmetro especifica o respetivo nome. Certifique-se de que substitui os valores de parâmetros `<RESOURCE GROUP>`e `<MSI NAME>` pelos seus próprios valores:

    > [!IMPORTANT]
    > Atualmente, o utilizador atribuído identidades com carateres especiais (por exemplo, um caráter de sublinhado) no nome de criação não é suportada. Utilize carateres alfanuméricos. Volte mais tarde para obter atualizações.  Para obter mais informações consulte [FAQ e problemas conhecidos](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```
A resposta contém detalhes para o utilizador atribuído MSI criado, semelhante ao seguinte. O recurso `id` valor atribuído para a identidade atribuída ao utilizador é utilizado no passo seguinte.

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

2. Atribuir a identidade atribuída ao utilizador para VM com [atribuir identidade da vm de az](/cli/azure/vm#az-vm-identity-assign). Certifique-se de que substitui os valores de parâmetros `<RESOURCE GROUP>` e `<VM NAME>` pelos seus próprios valores. O `<MSI ID>` será o recurso a identidade de utilizador atribuída `id` propriedade, como foi criado no passo anterior:

    ```azurecli-interactive
    az vm identity assign -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI ID>
    ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vm"></a>Remover um utilizador atribuído a identidade de uma VM do Azure

> [!NOTE]
> Remover todas as identidades de utilizador atribuída uma Máquina Virtual atualmente não é suportada, a menos que tenha um sistema de identidade atribuído.

Se a VM tiver múltiplas identidades de utilizador atribuído, pode remover tudo exceto o último que usa [remover a identidade da vm de az](/cli/azure/vm#az-vm-identity-remove). Certifique-se de que substitui os valores de parâmetros `<RESOURCE GROUP>` e `<VM NAME>` pelos seus próprios valores. O `<MSI NAME>` será a identidade de utilizador atribuída `name` propriedade, que pode ser encontrada na secção da identidade da VM usando `az vm show`:

```azurecli-interactive
az vm identity remove -g <RESOURCE GROUP> -n <VM NAME> --identities <MSI NAME>
```
Se a VM tem o sistema atribuída e as identidades de atribuída ao utilizador, pode remover todos os utilizadores identidades atribuídos ao mudar para utilizar apenas o sistema atribuído. Utilize o seguinte comando:

```azurecli-interactive
az vm update -n myVM -g myResourceGroup --set identity.type='SystemAssigned' identity.identityIds=null 
```

## <a name="related-content"></a>Conteúdo relacionado
- [Descrição geral de identidade do serviço gerido](overview.md)
- Para a criação de VM do Azure completa inícios rápidos, consulte: 
  - [Criar uma máquina virtual do Windows com a CLI](../../virtual-machines/windows/quick-create-cli.md)  
  - [Criar uma máquina virtual Linux com a CLI](../../virtual-machines/linux/quick-create-cli.md) 

















