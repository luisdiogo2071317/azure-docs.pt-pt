---
title: Como configurar o sistema e do usuário atribuído identidades num VMSS do Azure com REST
description: Instruções passo a passo instruções para configurar um sistema e um utilizador atribuído identidades num VMSS do Azure com o CURL para fazer chamadas de REST API.
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
ms.openlocfilehash: b590bb4f5eca0041cda97204b368de0da31620d0
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42886264"
---
# <a name="configure-managed-identity-on-a-virtual-machine-scale-set-using-rest-api-calls"></a>Configurar a identidade gerida num conjunto de dimensionamento de Máquina Virtual através de chamadas à REST API

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Identidade gerida fornece serviços do Azure com uma identidade de sistema automaticamente gerido no Azure Active Directory. Pode utilizar esta identidade para autenticar a qualquer serviço que suporta a autenticação do Azure AD, sem ter credenciais em seu código. 

Neste artigo, aprenderá a efetuar as seguintes operações de identidade gerido num conjunto de dimensionamento de máquina virtual, com o CURL para fazer chamadas para o ponto de extremidade REST do Azure Resource Manager:

- Ativar e desativar a identidade do sistema atribuído num conjunto de dimensionamento de máquina virtual do Azure
- Adicionar e remover uma identidade de utilizador atribuída num conjunto de dimensionamento de máquina virtual do Azure

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade do serviço gerido, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre um sistema atribuído e a identidade atribuída ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para efetuar as operações de gestão neste artigo, a conta tem das atribuições de funções seguintes:
    - [Contribuinte de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) criar um dimensionamento de máquina virtual definido e ativar e remover sistema e/ou o utilizador atribuído a identidade gerida de um conjunto de dimensionamento de máquina virtual.
    - [Contribuidor de identidade de geridos](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) função para criar uma identidade atribuída ao utilizador.
    - [Gerido operador de identidade](/azure/role-based-access-control/built-in-roles#managed-identity-operator) função para atribuir e remover uma identidade de utilizador atribuída de e para um conjunto de dimensionamento de máquina virtual.
- Se estiver a utilizar o Windows, instale o [subsistema Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about) ou utilizar o [Azure Cloud Shell](../../cloud-shell/overview.md) no portal do Azure.
- [Instalar a consola local da CLI do Azure](/cli/azure/install-azure-cli), se usar o [subsistema Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about) ou uma [SO de distribuição de Linux](/cli/azure/install-azure-cli-apt?view=azure-cli-latest).
- Se estiver a utilizar consola local da CLI do Azure, inicie sessão no Azure com `az login` com uma conta que está associada com o Azure, subscrição que pretende gerir o sistema ou o utilizador atribuído de identidades.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="system-assigned-identity"></a>Sistema de identidade atribuído

Nesta secção, saiba como ativar e desativar o sistema de identidade num conjunto de dimensionamento com o CURL para fazer chamadas para o ponto de extremidade REST do Azure Resource Manager atribuído.

### <a name="enable-system-assigned-identity-during-creation-of-a-virtual-machine-scale-set"></a>Ativar a identidade do sistema atribuído durante a criação de um conjunto de dimensionamento de máquinas virtuais

Para criar um conjunto de dimensionamento com o sistema de identidade ativada atribuído, precisa criar um conjunto de dimensionamento de máquina virtual e obter um token de acesso para utilizar CURL para chamar o ponto de final do Gestor de recursos com o sistema atribuído ao valor de tipo de identidade.

1. Criar uma [grupo de recursos](../../azure-resource-manager/resource-group-overview.md#terminology) para a contenção e a implantação de seu conjunto de dimensionamento de máquina virtual e respetivos recursos relacionados, usando [criar grupo az](/cli/azure/group/#az-group-create). Pode ignorar este passo se já tiver o grupo de recursos que pretende utilizar em vez disso:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

2. Criar uma [interface de rede](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) para o dimensionamento de máquinas virtuais definido:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Obter um token de acesso de portador, que irá utilizar no próximo passo no cabeçalho de autorização para criar o seu conjunto de dimensionamento com um sistema de identidade gerida atribuído.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Crie um conjunto de dimensionamento com o CURL para chamar o ponto de extremidade REST do Azure Resource Manager. O exemplo seguinte cria um conjunto nomeado de dimensionamento de máquina virtual *myVMSS* no *myResourceGroup* com uma identidade de sistema atribuído, conforme identificado no corpo do pedido pelo valor `"identity":{"type":"SystemAssigned"}`. Substitua `<ACCESS TOKEN>` com o valor que recebeu no passo anterior quando tiver solicitado um token de portador de acesso e o `<SUBSCRIPTION ID>` valor conforme apropriado para o seu ambiente.
 
    ```bash   
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus","identity":{"type":"SystemAssigned"},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
    ```

### <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Ativar a identidade do sistema atribuído num conjunto de dimensionamento de máquina virtual do Azure existente

Para ativar o sistema de identidade num conjunto de dimensionamento de máquina virtual existente atribuído, terá de adquirir um token de acesso e, em seguida, utilize o CURL para chamar o ponto de extremidade REST do Resource Manager para atualizar o tipo de identidade.

1. Obter um token de acesso de portador, que irá utilizar no próximo passo no cabeçalho de autorização para criar o seu conjunto de dimensionamento com um sistema de identidade gerida atribuído.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Utilize o seguinte comando CURL para chamar o ponto de extremidade REST do Azure Resource Manager para ativar a identidade do sistema atribuído no seu dimensionamento de máquina virtual definido conforme identificado no corpo do pedido pelo valor `{"identity":{"type":"SystemAssigned"}` para um conjunto nomeado de dimensionamento de máquina virtual  *myVMSS*.  Substitua `<ACCESS TOKEN>` com o valor que recebeu no passo anterior quando tiver solicitado um token de portador de acesso e o `<SUBSCRIPTION ID>` valor conforme apropriado para o seu ambiente.
   
   > [!IMPORTANT]
   > Para garantir que não elimina a qualquer utilizador existente atribuído identidades geridas que são atribuídas ao conjunto de dimensionamento de máquina virtual, tem de listar as identidades de utilizador atribuída ao utilizar este comando CURL: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Se tiver quaisquer identidades atribuídas por utilizadores atribuídas ao conjunto conforme indicado de dimensionamento de máquina virtual a `identity` valor na resposta, avance para o passo 3, que mostra como manter as identidades dos utilizadores atribuídos ao ativar a identidade do sistema atribuído no seu virtual conjunto de dimensionamento de máquina.

   ```bash
    curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Para ativar o sistema de identidade num conjunto de dimensionamento com identidades de utilizador atribuída existente atribuído, tem de adicionar `SystemAssigned` para o `type` valor.  
   
   Por exemplo, se o conjunto de dimensionamento de máquinas virtuais tem as identidades de utilizador atribuída `ID1` e `ID2` atribuídos à mesma, e gostaria de adicionar a identidade do sistema atribuído ao conjunto de dimensionamento de máquina virtual, utilize a seguinte chamada CURL. Substitua `<ACCESS TOKEN>` e `<SUBSCRIPTION ID>` com valores adequados ao seu ambiente.

   Versão de API `2018-06-01` armazena identidades de utilizador atribuída no `userAssignedIdentities` valor num formato de dicionário, em vez do `identityIds` valor num formato de matriz, utilizado a versão de API `2017-12-01` e versões anteriores.
   
   **API VERSÃO 2018-06-01**

   ```bash
   curl -v 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned,UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```
   
   **API versão 2017-12-01 e versões anteriores**

   ```bash
   curl -v 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="disable-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Desativar a identidade do sistema atribuído de um conjunto de dimensionamento de máquina virtual do Azure

Para desativar uma identidade de sistema atribuído num conjunto de dimensionamento de máquina virtual existente, terá de adquirir um token de acesso e, em seguida, utilize o CURL para chamar o ponto de extremidade REST do Resource Manager para atualizar o tipo de identidade para `None`.

1. Obter um token de acesso de portador, que irá utilizar no próximo passo no cabeçalho de autorização para criar o seu conjunto de dimensionamento com um sistema de identidade gerida atribuído.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Atualize o conjunto de dimensionamento com o CURL para chamar o ponto de extremidade REST do Azure Resource Manager para desativar o sistema de identidade atribuído.  O exemplo seguinte desativa uma identidade de sistema atribuído, conforme identificado no corpo do pedido pelo valor `{"identity":{"type":"None"}}` de um conjunto nomeado de dimensionamento de máquina virtual *myVMSS*.  Substitua `<ACCESS TOKEN>` com o valor que recebeu no passo anterior quando tiver solicitado um token de portador de acesso e o `<SUBSCRIPTION ID>` valor conforme apropriado para o seu ambiente.

   > [!IMPORTANT]
   > Para garantir que não elimina a qualquer utilizador existente atribuído identidades geridas que são atribuídas ao conjunto de dimensionamento de máquina virtual, tem de listar as identidades de utilizador atribuída ao utilizar este comando CURL: `curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"`. Se tiver qualquer identidade atribuída ao conjunto de dimensionamento de máquina virtual atribuída ao utilizador, avance para o passo 3, que mostra como a manter o usuário identidades atribuído ao remover o sistema de identidade atribuído do seu conjunto de dimensionamento de máquina virtual.

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

3. Para remover o sistema de identidade atribuído de um conjunto de dimensionamento de máquina virtual com identidades atribuídas por utilizadores, remova `SystemAssigned` do `{"identity":{"type:" "}}` valor ao manter o `UserAssigned` valor e o `userAssignedIdentities` dicionário de valores se estiver a utilizar **2018-01 06 de versão de API**. Se estiver a utilizar **API versão 2017-12-01** ou anterior, mantenha o `identityIds` matriz.

## <a name="user-assigned-identity"></a>Identidade atribuída ao utilizador

Nesta secção, saiba como adicionar e remover a identidade num conjunto de dimensionamento com o CURL para fazer chamadas para o ponto de extremidade REST do Azure Resource Manager atribuída ao utilizador.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Atribuir uma identidade atribuída ao utilizador durante a criação de um conjunto de dimensionamento de máquinas virtuais

1. Obter um token de acesso de portador, que irá utilizar no próximo passo no cabeçalho de autorização para criar o seu conjunto de dimensionamento com um sistema de identidade gerida atribuído.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Criar uma [interface de rede](/cli/azure/network/nic?view=azure-cli-latest#az-network-nic-create) para o dimensionamento de máquinas virtuais definido:

   ```azurecli-interactive
    az network nic create -g myResourceGroup --vnet-name myVnet --subnet mySubnet -n myNic
   ```

3.  Obter um token de acesso de portador, que irá utilizar no próximo passo no cabeçalho de autorização para criar o seu conjunto de dimensionamento com um sistema de identidade gerida atribuído.

   ```azurecli-interactive
   az account get-access-token
   ``` 

4. Criar uma identidade atribuída ao utilizador com as instruções aqui: [criar um utilizador atribuído a identidade gerida](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

5. Crie um conjunto de dimensionamento com o CURL para chamar o ponto de extremidade REST do Azure Resource Manager. O exemplo seguinte cria um conjunto nomeado de dimensionamento de máquina virtual *myVMSS* no grupo de recursos *myResourceGroup* com uma identidade de utilizador atribuída `ID1`, conforme identificado no corpo do pedido, o valor `"identity":{"type":"UserAssigned"}`. Substitua `<ACCESS TOKEN>` com o valor que recebeu no passo anterior quando tiver solicitado um token de portador de acesso e o `<SUBSCRIPTION ID>` valor conforme apropriado para o seu ambiente.
 
   **API VERSÃO 2018-06-01**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus",{"identity":{"type":"UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}}},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   **API versão 2017-12-01 e versões anteriores**

   ```bash   
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PUT -d '{"sku":{"tier":"Standard","capacity":3,"name":"Standard_D1_v2"},"location":"eastus",{"identity":{"type":"UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}},"properties":{"overprovision":true,"virtualMachineProfile":{"storageProfile":{"imageReference":{"sku":"2016-Datacenter","publisher":"MicrosoftWindowsServer","version":"latest","offer":"WindowsServer"},"osDisk":{"caching":"ReadWrite","managedDisk":{"storageAccountType":"Standard_LRS"},"createOption":"FromImage"}},"osProfile":{"computerNamePrefix":"myVMSS","adminUsername":"azureuser","adminPassword":"myPassword12"},"networkProfile":{"networkInterfaceConfigurations":[{"name":"myVMSS","properties":{"primary":true,"enableIPForwarding":true,"ipConfigurations":[{"name":"myVMSS","properties":{"subnet":{"id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"}}}]}}]}},"upgradePolicy":{"mode":"Manual"}}}' -H "Content-Type: application/json" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

### <a name="assign-a-user-assigned-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Atribuir uma identidade de utilizador atribuída a um conjunto de dimensionamento de máquina virtual do Azure existente

1. Obter um token de acesso de portador, que irá utilizar no próximo passo no cabeçalho de autorização para criar o seu conjunto de dimensionamento com um sistema de identidade gerida atribuído.

   ```azurecli-interactive
   az account get-access-token
   ```

2.  Criar uma identidade atribuída ao utilizador com as instruções aqui, [criar um utilizador atribuído a identidade gerida](how-to-manage-ua-identity-rest.md#create-a-user-assigned-managed-identity).

3. Para garantir que não a eliminar o utilizador existente ou sistema atribuída identidades geridas que são atribuídas ao conjunto de dimensionamento de máquina virtual, precisa de listar a identidade do conjunto de tipos atribuídos para o dimensionamento de máquinas virtuais utilizando o comando CURL seguinte. Se o ter gerido identidades atribuídas ao conjunto de dimensionamento de máquina virtual, que são apresentadas no `identity` valor.
 
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

4. Se não tiver qualquer utilizador ou sistema atribuído identidades atribuídas ao conjunto de dimensionamento de máquina virtual, utilize o seguinte comando CURL para chamar o ponto de extremidade REST do Azure Resource Manager para atribuir o primeiro utilizador identidade atribuído ao conjunto de dimensionamento de máquina virtual.  Se tiver um usuário ou sistema atribuído identity(s) atribuídos ao conjunto de dimensionamento de máquina virtual, avance para o passo 5, que lhe mostra como adicionar várias identidades de utilizador atribuída a um conjunto enquanto mantém a identidade de sistema atribuída de dimensionamento de máquina virtual.

   O exemplo seguinte atribui uma identidade de utilizador atribuída `ID1` para um conjunto nomeado de dimensionamento de máquina virtual *myVMSS* no grupo de recursos *myResourceGroup*.  Substitua `<ACCESS TOKEN>` com o valor que recebeu no passo anterior quando tiver solicitado um token de portador de acesso e o `<SUBSCRIPTION ID>` valor conforme apropriado para o seu ambiente.

   **API VERSÃO 2018-06-01**

    ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```   
    
   **API versão 2017-12-01 e versões anteriores**

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"userAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

5. Se tiver um atribuída ao utilizador existente ou uma identidade de sistema atribuído atribuídos ao conjunto de dimensionamento de máquina virtual:
   
   **API VERSÃO 2018-06-01**

   Adicionar a identidade atribuída ao utilizador para o `userAssignedIdentities` valor de dicionário.

   Por exemplo, se tiver sistema atribuída a identidade atribuída ao utilizador e de identidades `ID1` atualmente atribuída ao seu dimensionamento de máquinas virtuais e gostaria de adicionar a identidade atribuída ao utilizador `ID2` a ele:

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{},"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   **API versão 2017-12-01 e versões anteriores**

   Manter as identidades de utilizador atribuída que pretende manter `identityIds` matriz valor ao adicionar a nova identidade atribuída ao utilizador.

   Por exemplo, se tiver sistema atribuída a identidade atribuída ao utilizador e de identidades `ID1` atualmente atribuídos ao conjunto de dimensionamento de máquina virtual e gostaria de adicionar a identidade do utilizador `ID2` a ele: 

   ```bash
   curl  'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1","/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID2"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

### <a name="remove-a-user-assigned-identity-from-a-virtual-machine-scale-set"></a>Remover uma identidade atribuída ao utilizador a partir de um conjunto de dimensionamento de máquina virtual

1. Obter um token de acesso de portador, que irá utilizar no próximo passo no cabeçalho de autorização para criar o seu conjunto de dimensionamento com um sistema de identidade gerida atribuído.

   ```azurecli-interactive
   az account get-access-token
   ```

2. Para garantir que não elimina quaisquer atribuída identidades geridas que gostaria de manter atribuídos ao conjunto de dimensionamento de máquina virtual ou remover o sistema de identidade atribuído ao utilizador existente, terá de lista de identidades geridas utilizando o comando CURL seguinte: 
   
   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachineScaleSets/<VMSS NAME>?api-version=2018-06-01' -H "Authorization: Bearer <ACCESS TOKEN>" 
   ```
   
   Se o ter gerido identidades atribuídas à VM, estão listados na resposta no `identity` valor. 
    
   Por exemplo, se tiver de identidades de utilizador atribuída `ID1` e `ID2` atribuídos ao conjunto de dimensionamento de máquina virtual e pretender manter `ID1` atribuído e reter a identidade do sistema atribuído:

   **API VERSÃO 2018-06-01**

   Adicionar `null` para a identidade atribuída ao utilizador que pretende remover:

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned, UserAssigned", "userAssignedIdentities":{"/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1":{}}}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

   **API versão 2017-12-01 e versões anteriores**

   Manter apenas o utilizador atribuído identity(s) gostaria de manter no `identityIds` matriz:   

   ```bash
   curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2017-12-01' -X PATCH -d '{"identity":{"type":"SystemAssigned","UserAssigned", "identityIds":["/subscriptions/<SUBSCRIPTION ID>/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/ID1"]}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
   ```

Se o conjunto de dimensionamento de máquina virtual tem o sistema atribuída e as identidades de atribuída ao utilizador, pode remover todos os utilizadores identidades atribuídos ao mudar para utilizar apenas o sistema atribuído com o seguinte comando:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"SystemAssigned"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```
    
Se o conjunto de dimensionamento de máquina virtual tem o único utilizador identidades atribuído e pretende removê-los todos os, utilize o seguinte comando:

```bash
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myVMSS?api-version=2018-06-01' -X PATCH -d '{"identity":{"type":"None"}}' -H "Content-Type: application/json" -H Authorization:"Bearer <ACCESS TOKEN>"
```

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre como criar, listar ou eliminar o utilizador atribuído através de REST, consulte:

- [Criar, lista ou eliminar uma identidade de utilizador atribuída através de chamadas à REST API](how-to-manage-ua-identity-rest.md)
