---
title: Utilizar uma MSI de VM do Linux atribuída ao utilizador para aceder ao Azure Resource Manager
description: Um tutorial que explica o processo de utilização de uma Identidade de Serviço Gerida (MSI) Atribuída ao Utilizador numa VM do Linux, para aceder ao Azure Resource Manager.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 1195161a0c4045620447439bf9361b7c4c0189ae
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37904395"
---
# <a name="tutorial-use-a-user-assigned-identity-on-a-linux-vm-to-access-azure-resource-manager"></a>Tutorial: utilizar uma identidade atribuída ao utilizador numa VM do Linux, para aceder ao Azure Resource Manager

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Este tutorial explica como criar um identidade atribuída ao utilizador, atribuí-la a uma Máquina Virtual do Linux (VM do Linux) e, em seguida, utilizar essa identidade para aceder à API do Azure Resource Manager. As Identidades de Serviço Geridas são geridas automaticamente pelo Azure. Permitem a autenticação em serviços que suportam a autenticação do Azure AD, sem ter de incorporar as credenciais no código. 

As Identidades de Serviço Geridas são geridas automaticamente pelo Azure. Permitem a autenticação em serviços que suportam a autenticação do Azure AD, sem ter de incorporar as credenciais no código.

Saiba como:

> [!div class="checklist"]
> * Criar uma identidade atribuída ao utilizador
> * Atribuir a identidade atribuída ao utilizador a uma VM do Linux 
> * Conceder o acesso de identidade atribuída ao utilizador a um Grupo de Recursos no Azure Resource Manager 
> * Obter um token de acesso com a identidade atribuída ao utilizador e utilizá-lo para chamar o Azure Resource Manager 

## <a name="prerequisites"></a>Pré-requisitos

- Se está a familiarizado com a Identidade de Serviço Gerida, consulte a secção [Descrição Geral](overview.md). **Certifique-se de que revê as [diferenças entre as identidades atribuídas ao sistema e ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para executar os passos necessários de criação de recursos e gestão de funções neste tutorial, a sua conta precisa de permissões de “Proprietário” no âmbito adequado (a sua subscrição ou grupo de recursos). Se precisar de assistência com a atribuição de funções, veja [Utilizar o Controlo de Acesso Baseado em Funções para gerir o acesso aos recursos de subscrição do Azure](/azure/role-based-access-control/role-assignments-portal).

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0.4 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli).

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Criar uma Máquina Virtual do Linux num novo Grupo de Recursos

Neste tutorial, vamos criar primeiro uma nova VM do Linux. Também pode optar por utilizar uma VM existente.

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Ubuntu Server 16.04 LTS**.
3. Introduza as informações da máquina virtual. Em **Tipo de autenticação**, selecione **Chave SSH pública** ou **Palavra-passe**. As credenciais criadas permitem-lhe iniciar sessão na VM.

    ![Criar VM do Linux](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Escolha uma **Subscrição** para a máquina virtual na lista pendente.
5. Para selecionar um novo **Grupo de Recursos** no qual gostaria que a máquina virtual fosse criada, escolha **Criar Novo**. Quando terminar, clique em **OK**.
6. Selecione o tamanho da VM. Para ver mais tamanhos, selecione **Visualizar todos** ou altere o filtro Tipo de disco suportado. No painel de definições, mantenha as predefinições e clique em **OK**.

## <a name="create-a-user-assigned-identity"></a>Criar uma identidade atribuída ao utilizador

1. Se estiver a utilizar a consola CLI (em vez de uma sessão do Azure Cloud Shell), comece por iniciar sessão no Azure. Utilize uma conta que esteja associada à subscrição do Azure na qual pretende criar a identidade atribuída ao utilizador:

    ```azurecli
    az login
    ```

2. Crie uma identidade atribuída ao utilizador com [az identity create](/cli/azure/identity#az_identity_create). O parâmetro `-g` especifica o grupo de recursos onde a MSI é criada, e o parâmetro `-n` especifica o respetivo nome. Certifique-se de que substitui os valores de parâmetros `<RESOURCE GROUP>`e `<MSI NAME>` pelos seus próprios valores:
    
[!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]


```azurecli-interactive
az identity create -g <RESOURCE GROUP> -n <MSI NAME>
```

A resposta contém detalhes para a identidade atribuída ao utilizador criada, semelhante ao seguinte exemplo. Anote o valor `id` da identidade atribuída ao utilizador, uma vez que vai ser utilizada no próximo passo:

```json
{
"clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
"clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=12344643-8088-4d70-9532-c3a0fdc190fz",
"id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
"location": "westcentralus",
"name": "<MSI NAME>",
"principalId": "9012",
"resourceGroup": "<RESOURCE GROUP>",
"tags": {},
"tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
"type": "Microsoft.ManagedIdentity/userAssignedIdentities"
}
```

## <a name="assign-a-user-assigned-identity-to-your-linux-vm"></a>Atribuir uma identidade atribuída a um utilizador à sua VM do Linux

Uma identidade atribuída ao utilizador pode ser utilizada pelos clientes em vários recursos do Azure. Utilize os seguintes comandos para atribuir a identidade atribuída ao utilizador a uma única VM. Utilize a propriedade `Id` devolvida no passo anterior para o parâmetro `-IdentityID`.

Atribuir a MSI atribuída ao utilizador à VM do Linux, com [az vm assign-identity](/cli/azure/vm#az_vm_assign_identity). Certifique-se de que substitui os valores de parâmetros `<RESOURCE GROUP>` e `<VM NAME>` pelos seus próprios valores. Utilize a propriedade `id` devolvida no passo anterior para o valor de parâmetro `--identities`.

```azurecli-interactive
az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="grant-your-user-assigned-identity-access-to-a-resource-group-in-azure-resource-manager"></a>Conceder o acesso de identidade atribuída ao utilizador a um Grupo de Recursos no Azure Resource Manager 

A Identidade de Serviço Gerida (MSI) fornece identidades que podem ser utilizadas pelo seu código para pedir os tokens de acesso para autenticar para APIs de recurso que suportam a autenticação do Azure AD. Neste tutorial, o seu código irá aceder à API do Azure Resource Manager.  

Antes de o seu código pode aceder à API, tem de conceder o acesso de identidade a um recurso no Azure Resource Manager. Neste caso, o Grupo de Recursos no qual a VM está contida. Atualize o valor de `<SUBSCRIPTION ID>` e `<RESOURCE GROUP>`, conforme adequado para o seu ambiente. Além disso, substitua `<MSI PRINCIPALID>` pela propriedade `principalId` devolvida pelo comando `az identity create` em [Criar uma MSI atribuída ao utilizador](#create-a-user-assigned-msi):

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role 'Reader' --scope "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP> "
```

A resposta contém detalhes da atribuição de função criada, semelhante ao seguinte exemplo:

```json
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Authorization/roleAssignments/b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "name": "b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "properties": {
    "principalId": "f5fdfdc1-ed84-4d48-8551-999fb9dedfbl",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}

```

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>Obter um token de acesso com a identidade da VM e utilizá-lo para chamar o Resource Manager 

No resto do tutorial, iremos trabalhar a partir da VM que criámos anteriormente.

Para concluir estes passos, precisa de um cliente SSH. Se estiver a utilizar o Windows, pode utilizar o cliente SSH no [Subsistema Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about). 

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No portal, navegue para **Máquinas Virtuais**, vá para a máquina virtual do Linux e, na **Descrição Geral**, clique em **Ligar**. Copie a cadeia de ligação para ligar à sua VM.
3. Ligue à VM com o cliente SSH que escolher. Se estiver a utilizar o Windows, pode utilizar o cliente SSH no [Subsistema Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about). Se precisar de ajuda para configurar as chaves do seu cliente SSH, veja [Como utilizar chaves SSH com o Windows no Azure](~/articles/virtual-machines/linux/ssh-from-windows.md) ou [Como criar e utilizar um par de chaves SSH públicas e privadas para VMs do Linux no Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).
4. Na janela de terminal, com o CURL, faça um pedido ao ponto final do Instance Metadata Service (IMDS) do Azure para obter um token de acesso para o Azure Resource Manager.  

   O pedido CURL para adquirir um token de acesso é mostrado no exemplo seguinte. Certifique-se de que substitui `<CLIENT ID>` pela propriedade `clientId` devolvida pelo comando `az identity create` em [Criar uma identidade atribuída ao utilizador](#create-a-user-assigned-msi): 
    
   ```bash
   curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com/&client_id=<MSI CLIENT ID>"   
   ```
    
    > [!NOTE]
    > O valor do parâmetro `resource` tem de ser uma correspondência exata para o que é esperado pelo Azure AD. Ao utilizar o ID de recurso do Resource Manager, tem de incluir a barra à direita no URI. 
    
    A resposta inclui o token de acesso necessário para aceder ao Azure Resource Manager. 
    
    Exemplo de resposta:  

    ```bash
    {
    "access_token":"eyJ0eXAiOi...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://management.azure.com",
    "token_type":"Bearer"
    } 
    ```

5. Utilize o token de acesso para aceder ao Azure Resource Manager e ler as propriedades do Grupo de Recursos ao qual concedeu anteriormente o acesso de identidade atribuída ao utilizador. Certifique-se de que substitui `<SUBSCRIPTION ID>` e `<RESOURCE GROUP>` pelos valores que especificou anteriormente, e `<ACCESS TOKEN>` pelo token devolvido no passo anterior.

    > [!NOTE]
    > O URL é sensível às maiúsculas de minúsculas; por isso, certifique-se de que utiliza as mesmas maiúsculas e minúsculas que utilizou anteriormente, quando atribuiu o nome ao Grupo de Recursos, e a maiúscula “G” em `resourceGroups`.  

    ```bash 
    curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-09-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```

    A resposta contém as informações específicas do Grupo de Recursos, semelhantes ao seguinte exemplo: 

    ```bash
    {
    "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/DevTest",
    "name":"DevTest",
    "location":"westus",
    "properties":{"provisioningState":"Succeeded"}
    } 
    ```
    
## <a name="next-steps"></a>Passos seguintes

Este tutorial explica como criar um identidade atribuída ao utilizador e anexá-la a uma máquina virtual do Linux para aceder à API do Azure Resource Manager.  Para saber mais sobre o Azure Resource Manager, veja:

> [!div class="nextstepaction"]
>[Azure Resource Manager](/azure/azure-resource-manager/resource-group-overview)

