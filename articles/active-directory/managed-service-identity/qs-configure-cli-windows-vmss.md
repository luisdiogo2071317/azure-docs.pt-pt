---
title: Como configurar o sistema e do usuário atribuído identidades num VMSS do Azure com a CLI do Azure
description: Instruções passo a passo instruções para configurar o sistema e do usuário identidades atribuídas numa VMSS do Azure, cli do Azure.
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
ms.date: 02/15/2018
ms.author: daveba
ms.openlocfilehash: 36df9d00d41f3c092320fa88772b41c9a41c6d8e
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39237286"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-msi-using-azure-cli"></a>Configurar uma máquina virtual Managed Service Identity (MSI) com a CLI do Azure do conjunto de dimensionamento

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, aprenderá a efetuar as seguintes operações de identidade do serviço gerido numa Azure Virtual Machine dimensionamento definido (VMSS), com a CLI do Azure:
- Ativar e desativar o sistema de identidade no VMSS Azure atribuído
- Adicionar e remover um utilizador atribuído a identidade num VMSS do Azure


## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade do serviço gerido, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre um sistema atribuído e a identidade atribuída ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para efetuar as operações de gestão neste artigo, a conta tem das atribuições de funções seguintes:
    - [Contribuinte de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) criar um dimensionamento de máquina virtual definido e ativar e remover sistema e/ou utilizador atribuídos identidade gerida a partir de um conjunto de dimensionamento de máquina virtual.
    - [Contribuidor de identidade de geridos](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) função para criar uma identidade atribuída ao utilizador.
    - [Gerido operador de identidade](/azure/role-based-access-control/built-in-roles#managed-identity-operator) função para atribuir e remover uma identidade de utilizador atribuída de e para um conjunto de dimensionamento de máquina virtual.
- Para executar os exemplos de script da CLI, tem três opções:
    - Uso [Azure Cloud Shell](../../cloud-shell/overview.md) do portal do Azure (consulte a secção seguinte).
    - Utilize o embedded Azure Cloud Shell através do "Experimente-lo" botão do, localizado no canto superior direito de cada bloco de código.
    - [Instalar a versão mais recente da CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 ou posterior) se preferir utilizar uma consola CLI local. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Sistema de identidade atribuído

Nesta secção, saiba como ativar e desativar o sistema de identidade atribuído para um VMSS do Azure com a CLI do Azure.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Ativar a identidade do sistema atribuído durante a criação de um conjunto de dimensionamento de máquina virtual do Azure

Para criar um conjunto de dimensionamento com o sistema de identidade ativada atribuído:

1. Se estiver a utilizar a CLI do Azure numa consola local, primeiro inicie sessão no Azure com [az login](/cli/azure/reference-index#az_login). Utilize uma conta que seja associada à subscrição do Azure sob a qual pretende implementar o conjunto de dimensionamento de máquina virtual:

   ```azurecli-interactive
   az login
   ```

2. Criar uma [grupo de recursos](../../azure-resource-manager/resource-group-overview.md#terminology) para a contenção e a implantação de seu conjunto de dimensionamento de máquina virtual e respetivos recursos relacionados, usando [criar grupo az](/cli/azure/group/#az_group_create). Pode ignorar este passo se já tiver um grupo de recursos que pretende utilizar em vez disso:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Criar um conjunto de dimensionamento usando [az vmss criar](/cli/azure/vmss/#az_vmss_create) . O exemplo seguinte cria um conjunto nomeado de dimensionamento de máquina virtual *myVMSS* com uma identidade de sistema atribuído, conforme solicitado pelo `--assign-identity` parâmetro. Os parâmetros `--admin-username` e `--admin-password` especificam o nome e a palavra-passe da conta de utilizador administrativo para início de sessão na máquina virtual. Atualize estes valores conforme adequado para o seu ambiente: 

   ```azurecli-interactive 
   az vmss create --resource-group myResourceGroup --name myVMSS --image win2016datacenter --upgrade-policy-mode automatic --custom-data cloud-init.txt --admin-username azureuser --admin-password myPassword12 --assign-identity --generate-ssh-keys
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Ativar a identidade do sistema atribuído num conjunto de dimensionamento de máquina virtual do Azure existente

Se precisar de ativar a identidade do sistema atribuído num conjunto de dimensionamento de máquina virtual do Azure existente:

1. Se estiver a utilizar a CLI do Azure numa consola local, primeiro inicie sessão no Azure com [az login](/cli/azure/reference-index#az_login). Utilize uma conta que está associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual.

   ```azurecli-interactive
   az login
   ```

2. Uso [identidade do vmss az atribuir](/cli/azure/vmss/identity/#az_vmss_identity_assign) comando para ativar uma identidade de sistema atribuído a uma VM existente:

   ```azurecli-interactive
   az vmss identity assign -g myResourceGroup -n myVMSS
   ```

### <a name="disable-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Desativar a identidade do sistema atribuído de um conjunto de dimensionamento de máquina virtual do Azure

Se tiver um conjunto de dimensionamento de máquina virtual que já não tem o sistema de identidade atribuído, mas ainda precisa de identidades de atribuída ao utilizador, utilize o seguinte comando:

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type='UserAssigned' 
```

Se tiver uma máquina virtual que já não necessita de sistema de identidade atribuído e não tem nenhum utilizador identidades atribuída, utilize o seguinte comando:

> [!NOTE]
> O valor `none` diferencia maiúsculas de minúsculas. Tem de estar em minúsculas. 

```azurecli-interactive
az vmss update -n myVM -g myResourceGroup --set identity.type="none"
```

Para remover a extensão de VM de MSI, utilize [remover a identidade do vmss az](/cli/azure/vmss/identity/#az_vmss_remove_identity) comando para remover a identidade do sistema atribuído a um VMSS:

```azurecli-interactive
az vmss extension delete -n ManagedIdentityExtensionForWindows -g myResourceGroup -vmss-name myVMSS
```

## <a name="user-assigned-identity"></a>Identidade atribuída ao utilizador

Nesta secção, saiba como ativar e remover uma identidade atribuída ao utilizador com a CLI do Azure.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vmss"></a>Atribuir um utilizador a identidade atribuído durante a criação de um VMSS do Azure

Esta secção orienta-o através da criação de um VMSS e atribuição de um utilizador atribuído a identidade para o VMSS. Se já tiver um VMSS que pretende utilizar, ignore esta secção e avance para a próxima.

1. Pode ignorar este passo se já tiver um grupo de recursos que pretende utilizar. Criar uma [grupo de recursos](~/articles/azure-resource-manager/resource-group-overview.md#terminology) para a contenção e a implementação da sua identidade atribuída ao utilizador, utilizando [criar grupo az](/cli/azure/group/#az_group_create). Certifique-se de que substitui os valores de parâmetros `<RESOURCE GROUP>` e `<LOCATION>` pelos seus próprios valores. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Criar um utilizador atribuído através de identidade [identidade az criar](/cli/azure/identity#az-identity-create).  O `-g` parâmetro especifica o grupo de recursos onde o identidade atribuída ao utilizador é criado, e o `-n` parâmetro especifica o respetivo nome. Certifique-se de que substitui os valores de parâmetros `<RESOURCE GROUP>`e `<USER ASSIGNED IDENTITY NAME>` pelos seus próprios valores:

   [!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```azurecli-interactive
   az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
   ```
   A resposta contém detalhes para a identidade atribuída ao utilizador que criou, semelhante ao seguinte. O recurso `id` valor atribuído para a identidade atribuída ao utilizador é utilizado no passo seguinte.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY NAME>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY NAME>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

3. Criar uma com VMSS [az vmss criar](/cli/azure/vmss/#az-vmss-create). O exemplo seguinte cria um VMSS associados à identidade atribuída ao utilizador novo, como especificado pelo `--assign-identity` parâmetro. Certifique-se de que substitui os valores de parâmetros `<RESOURCE GROUP>`, `<VMSS NAME>`, `<USER NAME>`, `<PASSWORD>` e `<USER ASSIGNED IDENTITY ID>` pelos seus próprios valores. Para `<USER ASSIGNED IDENTITY ID>`, utilizar o recurso a identidade de utilizador atribuída `id` propriedade criada no passo anterior: 

   ```azurecli-interactive 
   az vmss create --resource-group <RESOURCE GROUP> --name <VMSS NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY ID>
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Atribuir um utilizador a identidade atribuído a uma VM do Azure existente

1. Criar um utilizador atribuído através de identidade [identidade az criar](/cli/azure/identity#az-identity-create).  O `-g` parâmetro especifica o grupo de recursos onde o identidade atribuída ao utilizador é criado, e o `-n` parâmetro especifica o respetivo nome. Certifique-se de que substitui os valores de parâmetros `<RESOURCE GROUP>`e `<USER ASSIGNED IDENTITY NAME>` pelos seus próprios valores:

    > [!IMPORTANT]
    > Atualmente, o utilizador atribuído identidades com carateres especiais (por exemplo, um caráter de sublinhado) no nome de criação não é suportada. Utilize carateres alfanuméricos. Volte mais tarde para obter atualizações.  Para obter mais informações consulte [FAQ e problemas conhecidos](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
A resposta contém detalhes para a identidade atribuída ao utilizador que criou, semelhante ao seguinte. O recurso `id` valor atribuído para a identidade atribuída ao utilizador é utilizado no passo seguinte.

   ```json
   {
        "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
        "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY >/credentials?tid=5678&oid=9012&aid=73444643-8088-4d70-9532-c3a0fdc190fz",
        "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<USER ASSIGNED IDENTITY>",
        "location": "westcentralus",
        "name": "<USER ASSIGNED IDENTITY>",
        "principalId": "e5fdfdc1-ed84-4d48-8551-fe9fb9dedfll",
        "resourceGroup": "<RESOURCE GROUP>",
        "tags": {},
        "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
        "type": "Microsoft.ManagedIdentity/userAssignedIdentities"    
   }
   ```

2. Atribuir a identidade atribuída ao utilizador para a sua utilização de VMSS [identidade do vmss az atribuir](/cli/azure/vmss/identity#az_vm_assign_identity). Certifique-se de que substitui os valores de parâmetros `<RESOURCE GROUP>` e `<VMSS NAME>` pelos seus próprios valores. O `<USER ASSIGNED IDENTITY ID>` será o recurso a identidade de utilizador atribuída `id` propriedade, como foi criado no passo anterior:

    ```azurecli-interactive
    az vmss identity assign -g <RESOURCE GROUP> -n <VMSS NAME> --identities <USER ASSIGNED IDENTITY ID>
    ```

### <a name="remove-a-user-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Remover uma identidade de utilizador atribuída a um conjunto de dimensionamento de máquina virtual do Azure

Para remover uma identidade de utilizador atribuída a uma utilização de conjunto de dimensionamento de máquina virtual [remover a identidade do vmss az](/cli/azure/vmss/identity#az-vmss-identity-remove). Certifique-se de que substitui os valores de parâmetros `<RESOURCE GROUP>` e `<VMSS NAME>` pelos seus próprios valores. O `<MSI NAME>` será a identidade de utilizador atribuída `name` propriedade, que pode ser encontrada na secção da identidade da VM usando `az vmss identity show`:

```azurecli-interactive
az vmss identity remove -g <RESOURCE GROUP> -n <VMSS NAME> --identities <MSI NAME>
```

Se o conjunto de dimensionamento de máquina virtual não tem um sistema de identidade atribuído e que pretende remover o utilizador de todas as identidades atribuídas da mesma, utilize o seguinte comando:

> [!NOTE]
> O valor `none` diferencia maiúsculas de minúsculas. Tem de estar em minúsculas.

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type="none" identity.identityIds=null
```

Se o conjunto de dimensionamento de máquina virtual tem o sistema atribuída e as identidades de atribuída ao utilizador, pode remover todos os utilizadores identidades atribuídos ao mudar para utilizar apenas o sistema atribuído. Utilize o seguinte comando:

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type='SystemAssigned' identity.identityIds=null 
```

## <a name="next-steps"></a>Passos Seguintes

- [Descrição geral de identidade do serviço gerido](overview.md)
- Para o Azure completo criação manual de início rápido do conjunto de dimensionamento de máquina virtual, consulte: 

  - [Criar um conjunto de dimensionamento de máquinas virtuais com a CLI](../../virtual-machines/linux/tutorial-create-vmss.md#create-a-scale-set)

















