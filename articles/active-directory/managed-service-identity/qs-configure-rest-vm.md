---
title: Como configurar o sistema e do usuário atribuído identidades na VM do Azure com REST
description: Instruções passo a passo instruções para configurar um sistema e um utilizador atribuído identidades numa VM do Azure com o CURL para fazer chamadas de REST API.
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
ms.date: 06/25/2018
ms.author: daveba
ms.openlocfilehash: 825f34d174c37c2ee5d4187048f7a31fbaeef226
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215934"
---
# <a name="configure-managed-identity-on-an-azure-vm-using-rest-api-calls"></a>Configurar a identidade gerida numa VM do Azure através de chamadas à REST API

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade gerida fornece serviços do Azure com uma identidade de sistema automaticamente gerido no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, aprenderá a efetuar as seguintes operações de identidade gerido na VM do Azure, com o CURL para fazer chamadas para o ponto de extremidade REST do Azure Resource Manager:

- Ativar e desativar o sistema de identidade numa VM do Azure atribuído
- Adicionar e remover um utilizador atribuído a identidade numa VM do Azure

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade do serviço gerido, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre um sistema atribuído e a identidade atribuída ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para efetuar as operações de gestão neste artigo, a conta tem das atribuições de funções seguintes:
    - [Contribuinte de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) criar uma VM e ativar e remover o sistema e/ou de utilizador atribuída identidade gerida a partir de uma VM do Azure.
    - [Contribuidor de identidade de geridos](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) função para criar uma identidade atribuída ao utilizador.
    - [Gerido operador de identidade](/azure/role-based-access-control/built-in-roles#managed-identity-operator) função para atribuir e remover uma identidade de utilizador atribuída de e para uma VM.
- Se estiver a utilizar o Windows, instale o [subsistema Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about) ou utilizar o [Azure Cloud Shell](../../cloud-shell/overview.md) no portal do Azure.
- [Instalar a consola local da CLI do Azure](/azure/install-azure-cli), se usar o [subsistema Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about) ou uma [SO de distribuição de Linux](/cli/azure/install-azure-cli-apt?view=azure-cli-latest).
- Se estiver a utilizar consola local da CLI do Azure, inicie sessão no Azure com `az login` com uma conta que está associada com o Azure, subscrição que pretende gerir o sistema ou o utilizador atribuído de identidades.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Sistema de identidade atribuído

Nesta secção, saiba como ativar e desativar o sistema de identidade numa VM do Azure com o CURL para fazer chamadas para o ponto de extremidade REST do Azure Resource Manager atribuído.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm"></a>Ativar o sistema de identidade atribuído durante a criação de uma VM do Azure

Para criar uma VM do Azure com o sistema de identidade ativada atribuído, precisa criar uma VM e obter um token de acesso para utilizar CURL para chamar o ponto de final do Gestor de recursos com o sistema atribuído ao valor de tipo de identidade.

1. Crie uma [grupo de recursos](../../azure-resource-manager/resource-group-overview.md#terminology) para contenção e implementação da VM e os respetivos recursos relacionados, utilizando [az group create](/cli/azure/group/#az_group_create). Pode ignorar este passo se já tiver o grupo de recursos que pretende utilizar em vez disso:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

2. Criar uma [interface de rede](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) para a sua VM:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Obter um token de acesso de portador, que irá utilizar no próximo passo no cabeçalho de autorização para criar a sua VM com um sistema de identidade gerida atribuído.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Crie uma VM com o CURL para chamar o ponto de extremidade REST do Azure Resource Manager. O exemplo seguinte cria uma VM com o nome *myVM* com uma identidade de sistema atribuído, conforme identificado no corpo do pedido pelo valor `"identity":{"type":"SystemAssigned"}`. Substitua `<ACCESS TOKEN>` com o valor que recebeu no passo anterior quando tiver solicitado um token de portador de acesso e o `<SUBSCRIPTION ID>` valor conforme apropriado para o seu ambiente.
 
    ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PUT -d '{"location":"westus","name":"myVM","identity":{"type":"SystemAssigned"},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"TestVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
    ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-vm"></a>Ativar o sistema de identidade numa VM do Azure existente atribuído

Para ativar o sistema de identidade numa VM existente atribuído, terá de adquirir um token de acesso e, em seguida, utilize o CURL para chamar o ponto de extremidade REST do Resource Manager para atualizar o tipo de identidade.

1. Obter um token de acesso de portador, que irá utilizar no próximo passo no cabeçalho de autorização para criar a sua VM com um sistema de identidade gerida atribuído.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Utilize o seguinte comando CURL para chamar o ponto de extremidade REST do Azure Resource Manager para ativar a identidade do sistema atribuída na sua VM, conforme identificado no corpo do pedido pelo valor `{"identity":{"type":"SystemAssigned"}` para uma VM com o nome *myVM*.  Substitua `<ACCESS TOKEN>` com o valor que recebeu no passo anterior quando tiver solicitado um token de portador de acesso e o `<SUBSCRIPTION ID>` valor conforme apropriado para o seu ambiente.
   
   > [!IMPORTANT]
   > Para garantir que não elimina a qualquer utilizador existente atribuído identidades geridas que são atribuídas à VM, precisa de listar as identidades de utilizador atribuída ao utilizar este comando CURL: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Se tiver quaisquer identidades atribuídas por utilizadores atribuídas à VM, conforme indicado no `identity` valor na resposta, avance para o passo 3, que mostra como manter as identidades dos utilizadores atribuídos ao ativar a identidade de sistema atribuída na sua VM.

   ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Para ativar o sistema de identidade numa VM com identidades de utilizador atribuída existente atribuído, tem de adicionar `SystemAssigned` para o `type` valor.  
   
   Por exemplo, se a VM tem as identidades de utilizador atribuída `ID1` e `ID2` atribuídos à mesma, e gostaria de adicionar a identidade de sistema atribuído à VM, utilize a seguinte chamada CURL. Substitua `<ACCESS TOKEN>` e `<SUBSCRIPTION ID>` com valores adequados ao seu ambiente.
   
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<<SUBSCRIPTION ID>>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="disable-system-assigned-identity-from-an-azure-vm"></a>Desativar o sistema de identidade atribuído a partir de uma VM do Azure

Para desativar uma identidade de sistema atribuído numa VM existente, terá de adquirir um token de acesso e, em seguida, utilize o CURL para chamar o ponto de extremidade REST do Resource Manager para atualizar o tipo de identidade para `None`.

1. Obter um token de acesso de portador, que irá utilizar no próximo passo no cabeçalho de autorização para criar a sua VM com um sistema de identidade gerida atribuído.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Atualize a VM com o CURL para chamar o ponto de extremidade REST do Azure Resource Manager para desativar o sistema de identidade atribuído.  O exemplo seguinte desativa uma identidade de sistema atribuído, conforme identificado no corpo do pedido pelo valor `{"identity":{"type":"None"}}` de uma VM chamada *myVM*.  Substitua `<ACCESS TOKEN>` com o valor que recebeu no passo anterior quando tiver solicitado um token de portador de acesso e o `<SUBSCRIPTION ID>` valor conforme apropriado para o seu ambiente.

   > [!IMPORTANT]
   > Para garantir que não elimina a qualquer utilizador existente atribuído identidades geridas que são atribuídas à VM, precisa de listar as identidades de utilizador atribuída ao utilizar este comando CURL: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Se tiver quaisquer identidades atribuídas por utilizadores atribuídas à VM, conforme indicado no `identity` valor na resposta, avance para o passo 3, que mostra como manter as identidades dos utilizadores atribuídos ao desativar a identidade de sistema atribuída na sua VM.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Para remover o sistema de identidade atribuído a partir de uma VM com identidades atribuídas por utilizadores, remova `SystemAssigned` partir a `{"identity":{"type:" "}}` valor ao manter o `UserAssigned` valor e `identityIds` matriz que define quais identidades atribuídas por utilizadores são atribuídas à VM.

## <a name="user-assigned-identity"></a>Identidade atribuída ao utilizador

Nesta secção, saiba como adicionar e remover a identidade na VM do Azure com o CURL para fazer chamadas para o ponto de extremidade REST do Azure Resource Manager atribuída ao utilizador.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-an-azure-vm"></a>Atribuir um utilizador a identidade atribuído durante a criação de uma VM do Azure

1. Obter um token de acesso de portador, que irá utilizar no próximo passo no cabeçalho de autorização para criar a sua VM com um sistema de identidade gerida atribuído.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Criar uma [interface de rede](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) para a sua VM:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Obter um token de acesso de portador, que irá utilizar no próximo passo no cabeçalho de autorização para criar a sua VM com um sistema de identidade gerida atribuído.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Criar uma identidade atribuída ao utilizador com as instruções aqui: [criar um utilizador atribuído a identidade gerida](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

5. Crie uma VM com o CURL para chamar o ponto de extremidade REST do Azure Resource Manager. O exemplo seguinte cria uma VM com o nome *myVM* no grupo de recursos *myResourceGroup* com uma identidade de utilizador atribuída `ID1`, conforme identificado no corpo do pedido pelo valor `"identity":{"type":"UserAssigned"}`. Substitua `<ACCESS TOKEN>` com o valor que recebeu no passo anterior quando tiver solicitado um token de portador de acesso e o `<SUBSCRIPTION ID>` valor conforme apropriado para o seu ambiente.
 
   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PUT -d '{"location":"westus","name":"myVM",{"identity":{"type":"UserAssigned", "identityIds":["/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}},"properties":{"hardwareProfile":{"vmSize":"Standard_D2_v2"},"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"name":"TestVM3osdisk","createOption":"FromImage"},"dataDisks":[{"diskSizeGB":1023,"createOption":"Empty","lun":0},{"diskSizeGB":1023,"createOption":"Empty","lun":1}]},"osProfile":{"adminUsername":"azureuser","computerName":"myVM","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaces":[{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic","properties":{"primary":true}}]}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-vm"></a>Atribuir um utilizador a identidade atribuído a uma VM do Azure existente

1. Obter um token de acesso de portador, que irá utilizar no próximo passo no cabeçalho de autorização para criar a sua VM com um sistema de identidade gerida atribuído.

   ```azurecli-interactive
   az account get-access-token
   ```

2.  Criar uma identidade atribuída ao utilizador com as instruções aqui, [criar um utilizador atribuído a identidade gerida](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

3.  Para garantir que não a eliminar o utilizador existente ou sistema atribuída de identidades geridas que são atribuídas à VM, precisa de listar os tipos de identidade atribuídos à VM utilizando o comando CURL seguinte. Se o ter gerido identidades atribuídas ao conjunto de dimensionamento de máquina virtual, eles estão listados no `identity` valor.

    ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```

    Se tiver qualquer utilizador ou identidades do sistema atribuído atribuídas à VM, conforme indicado no `identity` valor na resposta, avance para o passo 5, que mostra como manter as identidades dos utilizadores atribuídos ao ativar a identidade de sistema atribuída na sua VM.

4. Se não tiver qualquer utilizador atribuído identidades atribuídas à sua VM, utilize o seguinte comando CURL para chamar o ponto de extremidade REST do Azure Resource Manager para atribuir o primeiro utilizador identidade atribuído à VM.  Se tiver um utilizador atribuído identity(s) atribuído à VM, avance para o passo seguinte que mostra como adicionar várias identidades de utilizador atribuída a uma VM.

   O exemplo seguinte atribui uma identidade de utilizador atribuída `ID1` a uma VM com o nome *myVM* no grupo de recursos *myResourceGroup*.  Substitua `<ACCESS TOKEN>` com o valor que recebeu no passo anterior quando tiver solicitado um token de portador de acesso e o `<SUBSCRIPTION ID>` valor conforme apropriado para o seu ambiente.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

5. Se tiver de utilizador ou de identidades do sistema atribuído atribuídas à sua VM, terá de adicionar a nova identidade de utilizador atribuída para `identityIDs` matriz, retendo também o utilizador e as identidades do sistema atribuído atualmente atribuídos à VM.

   Para exemplo se tiver sistema atribuídos a identidade e a identidade de utilizador atribuída `ID1` atualmente atribuído à VM e gostaria de adicionar a identidade do utilizador `ID2` para ele, utilize o comando CURL seguinte. Substitua `<ACCESS TOKEN>` com o valor que recebeu nos passos quando tiver solicitado um token de portador de acesso e o `<SUBSCRIPTION ID>` valor conforme apropriado para o seu ambiente.

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/TestVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="remove-a-user-assigned-identity-from-an-azure-vm"></a>Remover um utilizador atribuído a identidade de uma VM do Azure

1. Obter um token de acesso de portador, que irá utilizar no próximo passo no cabeçalho de autorização para criar a sua VM com um sistema de identidade gerida atribuído.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Para garantir que não elimina quaisquer atribuída identidades geridas que gostaria de manter atribuído à VM ou remover o sistema de identidade atribuído ao utilizador existente, terá de lista de identidades geridas utilizando o comando CURL seguinte: 
 
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAME>?api-version=2017-12-01' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```
 
   Se o ter gerido identidades atribuídas à VM, estão listados na resposta no `identity` valor.

   Por exemplo, se tiver de identidades de utilizador atribuída `ID1` e `ID2` atribuído à VM e apenas gostaria de manter `ID1` atribuído e reter a identidade do sistema atribuído, usaria o mesmo comando CURL como atribuir um utilizador atribuído a identidade para uma VM apenas a manter gerido a `ID1` valor e manter o `SystemAssigned` valor. Esta ação remove o `ID2` identidade atribuída ao utilizador da VM, mantendo a identidade do sistema atribuído.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourcegroups/TestRG/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

Se a VM tem o sistema atribuída e as identidades de atribuída ao utilizador, pode remover todos os utilizadores identidades atribuídos ao mudar para utilizar apenas o sistema atribuído com o seguinte comando:

```bash
curl 'https://management.azure.com/subscriptions/80c696ff-5efa-4909-a64d-f1b616f423ca/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/TestVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```
    
Se a VM tem o único utilizador identidades atribuído e pretende removê-los todos os, utilize o seguinte comando:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```
## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre como criar, listar ou eliminar o utilizador atribuído através de REST, consulte:

- [Criar, lista ou eliminar uma identidade de utilizador atribuída através de chamadas à REST API](how-to-manage-ua-identity-rest.md)