---
title: "Utilizar MSI de utilizador atribuída uma VM com Linux para aceder ao Gestor de recursos do Azure"
description: "Um tutorial que explica o processo de utilizar um User-Assigned geridos serviço de identidade (MSI) numa VM com Linux, para aceder ao Gestor de recursos do Azure."
services: active-directory
documentationcenter: 
author: bryanLa
manager: mtillman
editor: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/22/2017
ms.author: arluca
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: bebdccb616a4677fdf36ac257ac36f1827958af7
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/09/2018
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-linux-vm-to-access-azure-resource-manager"></a>Utilize um utilizador atribuído geridos serviço de identidade (MSI) numa VM com Linux, para aceder ao Gestor de recursos do Azure

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Este tutorial explica como criar um utilizador atribuído geridos serviço de identidade (MSI), atribua-o para uma Máquina Virtual (VM Linux) e, em seguida, utilizar essa identidade para aceder à API do Gestor de recursos do Azure. 

Identidades de serviço geridas são automaticamente geridas pelo Azure. Ativar a autenticação nos serviços que suportam a autenticação do Azure AD, sem ser necessário incorporar as credenciais para o seu código.

Saiba como:

> [!div class="checklist"]
> * Criar um MSI utilizador atribuído
> * Atribuir o MSI para uma VM com Linux 
> * Conceder o acesso do MSI num grupo de recursos no Gestor de recursos do Azure 
> * Obter um token de acesso utilizando o MSI e utilizá-la para chamar o Azure Resource Manager 

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Para executar os exemplos de script CLI neste tutorial, tem duas opções:

- Utilize [Shell de nuvem do Azure](~/articles/cloud-shell/overview.md) do portal do Azure ou através do botão "Tente-", localizada no canto superior direito de cada bloco de código.
- [Instale a versão mais recente do CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 ou posterior) se preferir utilizar uma consola local do CLI.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Criar uma Máquina Virtual Linux num novo grupo de recursos

Para este tutorial, tem primeiro de criar uma nova VM do Linux. Também pode optar por utilizar uma VM existente.

1. Clique no botão **Novo** localizado no canto superior esquerdo do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Ubuntu Server 16.04 LTS**.
3. Introduza as informações da máquina virtual. Para **tipo de autenticação**, selecione **chave pública SSH** ou **palavra-passe**. As credenciais criadas permitem-lhe iniciar sessão VM.

    ![Criar a VM com Linux](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Escolha um **subscrição** para a máquina virtual na lista pendente.
5. Para selecionar um novo **grupo de recursos** gostaria que a máquina virtual ser criada no, escolha **criar novo**. Quando terminar, clique em **OK**.
6. Selecione o tamanho da VM. Para ver mais tamanhos, selecione **ver todos os** ou alterar o filtro de tipo de disco suportados. No painel de definições, mantenha as predefinições e clique em **OK**.

## <a name="create-a-user-assigned-msi"></a>Criar um MSI utilizador atribuído

1. Se estiver a utilizar a consola do CLI (em vez de uma sessão de Shell de nuvem do Azure), comece por iniciar sessão no Azure. Utilize uma conta que está associada à subscrição do Azure na qual gostaria de criar o novo MSI:

    ```azurecli
    az login
    ```

2. Criar um utilizador atribuído MSI, utilizando [identidade az criar](/cli/azure/identity#az_identity_create). O `-g` parâmetro especifica o grupo de recursos onde o MSI é criado, e o `-n` parâmetro especifica o respetivo nome. Não se esqueça de substituir o `<RESOURCE GROUP>` e `<MSI NAME>` valores de parâmetros com os seus próprios valores:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```

    A resposta contém detalhes para o utilizador atribuído do MSI criado, semelhante ao seguinte exemplo. Tenha em atenção o `id` valor para o MSI, que será utilizado no próximo passo:

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

## <a name="assign-your-user-assigned-msi-to-your-linux-vm"></a>Atribuir o MSI utilizador atribuído à sua VM do Linux

Ao contrário de um MSI atribuído do sistema, um MSI atribuída de utilizador pode ser utilizado por clientes em vários recursos do Azure. Para este tutorial, atribua-a uma única VM. Também pode atribuir a mais do que uma VM.

Atribuir o MSI utilizador atribuído à VM com Linux utilizando [az vm atribuir-identity](/cli/azure/vm#az_vm_assign_identity). Não se esqueça de substituir o `<RESOURCE GROUP>` e `<VM NAME>` valores de parâmetros com os seus próprios valores. Utilize o `id` propriedade devolvida no passo anterior para o `--identities` valor do parâmetro:

```azurecli-interactive
az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="grant-your-user-assigned-msi-access-to-a-resource-group-in-azure-resource-manager"></a>Conceder o acesso do MSI utilizador atribuído a um grupo de recursos no Gestor de recursos do Azure 

MSI fornece o seu código com um token de acesso para autenticar para o recurso APIs que suportam a autenticação do Azure AD. Neste tutorial, o seu código acede a API do Azure Resource Manager. 

Antes do seu código pode aceder à API do apesar, terá de conceder acesso de identidade do MSI para um recurso no Gestor de recursos do Azure. Neste caso, o grupo de recursos no qual a VM está contida. Não se esqueça de substituir o `<CLIENT ID>`, `<SUBSCRIPTION ID>`, e `<RESOURCE GROUP>` valores de parâmetros com os seus próprios valores. Substitua `<CLIENT ID>` com o `clientId` propriedade devolvida pelo `az identity create` no [criar um MSI utilizador atribuído](#create-a-user-assigned-msi): 

```azurecli-interactive
az role assignment create --assignee <CLIENT ID> --role ‘Reader’ --scope "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP> "
```

A resposta contém detalhes para a atribuição de função criada, semelhante ao seguinte exemplo:

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

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-resource-manager"></a>Obter um token de acesso através da identidade da VM e utilizá-la para chamar o Gestor de recursos 

Para o resto do tutorial, iremos trabalhar da VM que criou anteriormente.

Para concluir estes passos, precisa de um cliente SSH. Se estiver a utilizar o Windows, pode utilizar o cliente SSH o [subsistema Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about). Se precisar de assistência para configurar as chaves do seu cliente SSH, consulte [como chaves de utilizar o SSH com o Windows no Azure](~/articles/virtual-machines/linux/ssh-from-windows.md), ou [como criar e utilizar um par de chaves público e privado SSH para VMs com Linux no Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. No portal do Azure, navegue para **máquinas virtuais**, vá para a máquina virtual Linux, em seguida, a partir de **descrição geral** página clique **Connect** na parte superior. Copie a cadeia para ligar à VM.
2. **Ligar** para a VM com o cliente SSH à sua escolha.  
3. Na janela de terminal, utilizando CURL, efetue um pedido para o ponto final local de MSI para obter acesso token para o Azure Resource Manager.  

   O pedido CURL para adquirir um token de acesso é mostrado no exemplo seguinte. Não se esqueça de substituir `<CLIENT ID>` com o `clientId` propriedade devolvida pelo `az identity create` no [criar um MSI utilizador atribuído](#create-a-user-assigned-msi): 
    
   ```bash
   curl -H Metadata:true "http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com/&client_id=<CLIENT ID>"   
   ```
    
    > [!NOTE]
    > O valor da `resource` parâmetro tem de ser uma correspondência exata para que é esperado pelo Azure AD. Ao utilizar o ID de recurso do Resource Manager, tem de incluir a barra no final no URI. 
    
    A resposta inclui o token de acesso que precisa de aceder ao Gestor de recursos do Azure. 
    
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

4. Agora utilizar o token de acesso para aceder ao Gestor de recursos do Azure e ler as propriedades do grupo de recursos aos quais concedeu anteriormente o acesso do MSI utilizador atribuído. Não se esqueça de substituir `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` com os valores que especificou anteriormente, e `<ACCESS TOKEN>` com o token devolvido no passo anterior.

    > [!NOTE]
    > O URL é maiúsculas e minúsculas, pelo que não se esqueça de utilizar as maiúsculas exata que utilizou anteriormente quando denominado o grupo de recursos e, em maiúsculas "G" no `resourceGroups`.  

    ```bash 
    curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-09-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```

    A resposta contém as informações de grupo de recursos específicas, tal como no exemplo seguinte: 

    ```bash
    {
    "id":"/subscriptions/<SUBSCRIPTION ID>/resourceGroups/DevTest",
    "name":"DevTest",
    "location":"westus",
    "properties":{"provisioningState":"Succeeded"}
    } 
    ```
    
## <a name="next-steps"></a>Passos Seguintes

- Para obter uma descrição geral do MSI, consulte [descrição geral de identidade de serviço geridas](msi-overview.md).

Utilize a seguinte secção de comentários para fornecer comentários e ajudam-nos refinar e formam o nosso conteúdo.
