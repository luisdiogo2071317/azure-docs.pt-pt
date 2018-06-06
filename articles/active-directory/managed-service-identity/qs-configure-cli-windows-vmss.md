---
title: Como configurar o sistema e de utilizador atribuída identidades num VMSS do Azure utilizando a CLI do Azure
description: Passo a passo instruções para configurar o sistema e de utilizador atribuída identidades no VMSS do Azure, utilizando a CLI do Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/15/2018
ms.author: daveba
ms.openlocfilehash: a0e05543734ae0604149d18564ae1bc1eff1892b
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34714634"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-msi-using-azure-cli"></a>Configurar uma máquina virtual gerida serviço de identidade (MSI) utilizando a CLI do Azure do conjunto de dimensionamento

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade de serviço geridas fornece serviços do Azure com uma identidade gerido automaticamente no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter de credenciais no seu código. 

Neste artigo, irá aprender a realizar as seguintes operações de identidade de serviço geridas numa Azure Virtual Machine escala definido (VMSS), utilizando a CLI do Azure:
- Ativar e desativar o sistema atribuído a identidade de um VMSS do Azure
- Adicionar e remover um utilizador atribuído a identidade de um VMSS do Azure


## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade de serviço geridas, consulte o [secção Descrição geral](overview.md). **Certifique-se rever o [diferença entre um sistema atribuída e o utilizador atribuído identidade](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscrever-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.

Para executar os exemplos de script do CLI, tem três opções:

- Utilize [Shell de nuvem do Azure](../../cloud-shell/overview.md) do portal do Azure (consulte a secção seguinte).
- Utilize a Shell de nuvem do Azure incorporados através de "Tente-" botão, localizado no canto superior direito de cada bloco de código.
- [Instale a versão mais recente do CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.13 ou posterior) se preferir utilizar uma consola local do CLI. 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Atribuído a identidade de sistema

Nesta secção, saiba como ativar e desativar o sistema atribuído de identidade para um VMSS do Azure utilizando a CLI do Azure.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Ativar a identidade do sistema atribuído durante a criação de um conjunto de dimensionamento da máquina virtual do Azure

Para criar um conjunto com o sistema atribuído identidade ativada de dimensionamento de máquina virtual:

1. Se estiver a utilizar a CLI do Azure numa consola local, primeiro inicie sessão no Azure com [início de sessão az](/cli/azure/reference-index#az_login). Utilize uma conta que está associada à subscrição do Azure na qual gostaria de implementar o conjunto de dimensionamento de máquina virtual:

   ```azurecli-interactive
   az login
   ```

2. Criar um [grupo de recursos](../../azure-resource-manager/resource-group-overview.md#terminology) para a implementação do seu conjunto de dimensionamento da máquina virtual e os respetivos recursos relacionados, utilizar e contenção [criar grupo az](/cli/azure/group/#az_group_create). Pode ignorar este passo se já tiver um grupo de recursos que pretende utilizar em vez disso:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Criar um conjunto utilizando de dimensionamento de máquina virtual [az vmss criar](/cli/azure/vmss/#az_vmss_create) . O exemplo seguinte cria um conjunto nomeado de dimensionamento de máquina virtual *myVMSS* com uma identidade de sistema atribuída, conforme solicitado pelo `--assign-identity` parâmetro. O `--admin-username` e `--admin-password` parâmetros especificam a conta de nome e palavra-passe de utilizador administrativo para início de sessão na máquina virtual. Atualize estes valores conforme adequado para o seu ambiente: 

   ```azurecli-interactive 
   az vmss create --resource-group myResourceGroup --name myVMSS --image win2016datacenter --upgrade-policy-mode automatic --custom-data cloud-init.txt --admin-username azureuser --admin-password myPassword12 --assign-identity --generate-ssh-keys
   ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Ativar a identidade de sistema atribuída de um conjunto de dimensionamento de máquina virtual do Azure existente

Se precisar de ativar a identidade do sistema atribuído de um conjunto de dimensionamento de máquina virtual do Azure existente:

1. Se estiver a utilizar a CLI do Azure numa consola local, primeiro inicie sessão no Azure com [início de sessão az](/cli/azure/reference-index#az_login). Utilize uma conta que está associada à subscrição do Azure que contém o conjunto de dimensionamento de máquina virtual.

   ```azurecli-interactive
   az login
   ```

2. Utilize [identidade de vmss az atribuir](/cli/azure/vmss/identity/#az_vmss_identity_assign) comando para ativar uma identidade de sistema atribuído a uma VM existente:

   ```azurecli-interactive
   az vmss identity assign -g myResourceGroup -n myVMSS
   ```

### <a name="disable-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Desativar a identidade do sistema atribuído de um conjunto de dimensionamento de máquina virtual do Azure

> [!NOTE]
> Desativar a identidade de serviço geridos de um conjunto de dimensionamento de Máquina Virtual não é atualmente suportada. Entretanto, pode alternar entre a utilização do sistema atribuídas e identidades atribuídas do utilizador. Verifique novamente para as atualizações.

Se tiver um conjunto de dimensionamento de máquina virtual que já não necessita de um sistema atribuído de identidade, mas ainda tem de atribuir as identidades de utilizador, execute o seguinte comando:

```azurecli-interactive
az vmss update -n myVMSS -g myResourceGroup --set identity.type='UserAssigned' 
```

Para remover a extensão de VM de MSI, utilize [remover a identidade de vmss az](/cli/azure/vmss/identity/#az_vmss_remove_identity) comando para remover a identidade do sistema atribuído a um VMSS:

   ```azurecli-interactive
   az vmss extension delete -n ManagedIdentityExtensionForWindows -g myResourceGroup -vmss-name myVMSS
   ```

## <a name="user-assigned-identity"></a>Utilizador atribuído identidade

Nesta secção, saiba como ativar e remover uma identidade de utilizador atribuída ao utilizar a CLI do Azure.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vmss"></a>Atribuir um utilizador atribuído identidade durante a criação de um VMSS do Azure

Esta secção explica a criação de um VMSS e atribuição de um utilizador atribuído identidade a VMSS. Se já tiver um VMSS que pretende utilizar, ignore esta secção e avance para o seguinte.

1. Pode ignorar este passo se já tiver um grupo de recursos que pretende utilizar. Criar um [grupo de recursos](~/articles/azure-resource-manager/resource-group-overview.md#terminology) para a contenção e a implementação da sua identidade de utilizador atribuída, utilizando [criar grupo az](/cli/azure/group/#az_group_create). Não se esqueça de substituir o `<RESOURCE GROUP>` e `<LOCATION>` valores de parâmetros com os seus próprios valores. :

   ```azurecli-interactive 
   az group create --name <RESOURCE GROUP> --location <LOCATION>
   ```

2. Criar um utilizador atribuído através de identidade [identidade az criar](/cli/azure/identity#az-identity-create).  O `-g` parâmetro especifica o grupo de recursos onde será criado a identidade de atribuída ao utilizador, e o `-n` parâmetro especifica o respetivo nome. Não se esqueça de substituir o `<RESOURCE GROUP>` e `<USER ASSIGNED IDENTITY NAME>` valores de parâmetros com os seus próprios valores:

[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]


    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
A resposta contém detalhes para a identidade de utilizador atribuído criado, semelhante ao seguinte. O recurso `id` valor atribuído à identidade do utilizador atribuído é utilizada no passo seguinte.

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

3. Criar um VMSS utilizando [az vmss criar](/cli/azure/vmss/#az-vmss-create). O exemplo seguinte cria um VMSS associados à identidade atribuída ao utilizador nova, tal como especificado pelo `--assign-identity` parâmetro. Não se esqueça de substituir o `<RESOURCE GROUP>`, `<VMSS NAME>`, `<USER NAME>`, `<PASSWORD>`, e `<USER ASSIGNED IDENTITY ID>` valores de parâmetros com os seus próprios valores. Para `<USER ASSIGNED IDENTITY ID>`, utilize o recurso a identidade de utilizador atribuído `id` propriedade criada no passo anterior: 

   ```azurecli-interactive 
   az vmss create --resource-group <RESOURCE GROUP> --name <VMSS NAME> --image UbuntuLTS --admin-username <USER NAME> --admin-password <PASSWORD> --assign-identity <USER ASSIGNED IDENTITY ID>
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Atribuir um utilizador atribuído a identidade para uma VM do Azure existente

1. Criar um utilizador atribuído através de identidade [identidade az criar](/cli/azure/identity#az-identity-create).  O `-g` parâmetro especifica o grupo de recursos onde será criado a identidade de atribuída ao utilizador, e o `-n` parâmetro especifica o respetivo nome. Não se esqueça de substituir o `<RESOURCE GROUP>` e `<USER ASSIGNED IDENTITY NAME>` valores de parâmetros com os seus próprios valores:

    > [!IMPORTANT]
    > Criar atribuído identidades com carateres especiais (ou seja, um caráter de sublinhado) no nome de utilizador não é atualmente suportado. Utilize apenas carateres alfanuméricos. Verifique novamente para as atualizações.  Para obter mais informações consulte [perguntas mais frequentes e problemas conhecidos](known-issues.md)

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <USER ASSIGNED IDENTITY NAME>
    ```
A resposta contém detalhes para a identidade de utilizador atribuído criado, semelhante ao seguinte. O recurso `id` valor atribuído à identidade do utilizador atribuído é utilizada no passo seguinte.

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

2. Atribua a identidade do utilizador atribuído à sua utilização VMSS [identidade de vmss az atribuir](/cli/azure/vmss/identity#az_vm_assign_identity). Não se esqueça de substituir o `<RESOURCE GROUP>` e `<VMSS NAME>` valores de parâmetros com os seus próprios valores. O `<USER ASSIGNED IDENTITY ID>` será recursos a identidade de utilizador atribuída `id` propriedade, como criado no passo anterior:

    ```azurecli-interactive
    az vmss identity assign -g <RESOURCE GROUP> -n <VMSS NAME> --identities <USER ASSIGNED IDENTITY ID>
    ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vmss"></a>Remover um utilizador atribuído a identidade de um VMSS do Azure

> [!NOTE]
>  Remover todas as identidades de utilizador atribuída de um conjunto de dimensionamento de Máquina Virtual atualmente não é suportada, a menos que tenha um sistema atribuído de identidade. 

Se o seu VMSS tem múltiplas identidades de utilizador atribuído, pode remover todas, exceto os últimos utilizando [remover a identidade de vmss az](/cli/azure/vmss/identity#az-vmss-identity-remove). Não se esqueça de substituir o `<RESOURCE GROUP>` e `<VMSS NAME>` valores de parâmetros com os seus próprios valores. O `<MSI NAME>` é uma propriedade name a identidade de utilizador atribuída, que pode ser encontrada na secção da identidade da VM utilizando `az vm show`:

```azurecli-interactive
az vmss identity remove -g <RESOURCE GROUP> -n <VMSS NAME> --identities <MSI NAME>
```
Se tiver o VMSS sistema atribuída e atribuído identidades de utilizador, pode remover todos os a atribuída ao utilizador identidades por mudar para utilizar apenas o sistema atribuído. Utilize o seguinte comando: 

```azurecli-interactive
az vmss update -n <VMSS NAME> -g <RESOURCE GROUP> --set identity.type='SystemAssigned' identity.identityIds=null
```

## <a name="next-steps"></a>Passos Seguintes

- [Descrição geral de identidade de serviço gerida](overview.md)
- Para o Azure completo início rápido de criação de conjunto de dimensionamento de máquina virtual, consulte: 

  - [Criar um conjunto de dimensionamento de Máquina Virtual com a CLI](../../virtual-machines/linux/tutorial-create-vmss.md#create-a-scale-set)

















