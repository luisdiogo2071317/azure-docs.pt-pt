---
title: Utilizar um MSI numa VM do Linux de atribuída ao utilizador para aceder ao armazenamento do Azure
description: Um tutorial que explica o processo de uso de um utilizador atribuído Managed Service Identity (MSI) numa VM do Linux para aceder ao armazenamento do Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: arluca
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 4a1a2d0c40012649f6cd89193fd3f704f325e38a
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611049"
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-linux-vm-to-access-azure-storage"></a>Utilize um utilizador atribuído Managed Service Identity (MSI) numa VM do Linux para aceder ao armazenamento do Azure

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Este tutorial mostra-lhe como criar e utilizar um atribuído ao utilizador Managed Service Identity (MSI) de uma Máquina Virtual do Linux, em seguida, utilizá-lo para aceder ao armazenamento do Azure. Saiba como:

> [!div class="checklist"]
> * Criar um utilizador atribuído a identidade de serviço gerida (MSI)
> * Atribuir o MSI atribuído ao utilizador a uma máquina de Virtual do Linux
> * Conceda acesso MSI para uma instância de armazenamento do Azure
> * Obter um token de acesso com a identidade MSI atribuído ao utilizador e utilizá-lo para aceder ao armazenamento do Azure

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Para executar os exemplos de script da CLI neste tutorial, tem duas opções:

- Uso [Azure Cloud Shell](~/articles/cloud-shell/overview.md) do portal do Azure ou através do botão "Experimente-", localizado no canto superior direito de cada bloco de código.
- [Instalar a versão mais recente da CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 ou posterior) se preferir utilizar uma consola CLI local.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual Linux num novo grupo de recursos

Em primeiro lugar criar uma nova VM do Linux. Também pode ativar o MSI numa VM existente se preferir.

1. Clique nas **c++ /CLI criar novo serviço** botão encontrado no canto superior esquerdo do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Ubuntu Server 16.04 LTS**.
3. Introduza as informações da máquina virtual. Para **tipo de autenticação**, selecione **chave pública SSH** ou **palavra-passe**. As credenciais criadas permitem-lhe iniciar sessão VM.

    ![Texto alternativo da imagem](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Escolher uma **subscrição** para a máquina virtual na lista pendente.
5. Para selecionar um novo **grupo de recursos** pretende que a máquina virtual para ser criado no, escolha **criar nova**. Quando terminar, clique em **OK**.
6. Selecione o tamanho da VM. Para ver mais tamanhos, selecione **ver todos os** ou alterar o filtro de tipo de disco suportados. No painel de definições, mantenha as predefinições e clique em **OK**.

## <a name="create-a-user-assigned-msi"></a>Criar um MSI atribuído ao utilizador

1. Se estiver a utilizar a consola CLI (em vez de uma sessão do Azure Cloud Shell), comece por iniciar sessão no Azure. Utilize uma conta que seja associada à subscrição do Azure sob a qual pretende criar o novo MSI:

    ```azurecli
    az login
    ```

2. Criar um MSI atribuído ao utilizador utilizando [identidade az criar](/cli/azure/identity#az_identity_create). O `-g` parâmetro especifica o grupo de recursos onde o MSI é criado, e o `-n` parâmetro especifica o respetivo nome. Certifique-se de que substitua a `<RESOURCE GROUP>` e `<MSI NAME>` valores de parâmetros com seus próprios valores:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```

    A resposta contém detalhes para o MSI atribuído ao utilizador criado, semelhante ao seguinte exemplo. Tenha em atenção o `id` o valor para o MSI, que será utilizado no próximo passo:

    ```json
    {
    "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
    "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=123444643-8088-4d70-9532-c3a0fdc190fz",
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

## <a name="assign-your-user-assigned-msi-to-your-linux-vm"></a>Atribuir o MSI atribuído ao utilizador para a VM do Linux

Ao contrário de um MSI de atribuído de sistema de mensagens em fila, um MSI atribuído ao utilizador pode ser utilizado pelos clientes em vários recursos do Azure. Para este tutorial, atribuí-lo a uma única VM. Também pode atribuir a mais do que uma VM.

Atribuir o MSI atribuído ao utilizador para a VM do Linux utilizar [az vm atribuir-identity](/cli/azure/vm#az-vm-identity-assign). Certifique-se de que substitua a `<RESOURCE GROUP>` e `<VM NAME>` valores de parâmetros com seus próprios valores. Utilize o `id` propriedade devolvido no passo anterior para o `--identities` valor do parâmetro:

```azurecli-interactive
az vm identity assign -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento 

Se ainda não tiver uma, agora crie uma conta de armazenamento. Também pode ignorar este passo e utilizar uma conta de armazenamento existente, se preferir. 

1. Clique nas **c++ /CLI criar novo serviço** botão encontrado no canto superior esquerdo do portal do Azure.
2. Clique em **armazenamento**, em seguida, **conta de armazenamento**, e apresenta um novo painel "Criar a conta de armazenamento".
3. Introduza um **nome** para a conta de armazenamento, que irá utilizar mais tarde.  
4. **Modelo de implementação** e **tipo de conta** deve ser definido como "Resource manager" e "Fins gerais", respectivamente. 
5. Certifique-se a **subscrição** e **grupo de recursos** corresponder aos perfis que especificou quando criou a VM no passo anterior.
6. Clique em **Criar**.

    ![Criar nova conta de armazenamento](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Criar um contentor de BLOBs na conta de armazenamento

Uma vez que ficheiros requerem armazenamento de BLOBs, tem de criar um contentor de BLOBs para armazenar o ficheiro. Em seguida, carregar e transferir um ficheiro para o contentor de BLOBs, a nova conta de armazenamento.

1. Navegue de volta para a sua conta de armazenamento recentemente criada.
2. Clique nas **contentores** ligação no lado esquerdo, sob "Serviço de Blob".
3. Clique em **+ contentor** na parte superior da página e um "novo contentor" painel desliza.
4. Dê um nome ao contentor, selecione um nível de acesso, em seguida, clique em **OK**. O nome que especificou também é utilizado mais tarde no tutorial. 

    ![Criar contentor de armazenamento](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Carregar um ficheiro para o contentor recém-criado ao clicar no nome do contentor e, em seguida, **carregue**, em seguida, selecione um ficheiro, em seguida, clique em **carregar**.

    ![Carregar ficheiro de texto](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-user-assigned-msi-access-to-an-azure-storage-container"></a>Conceder o acesso MSI atribuídos do utilizador para um contentor de armazenamento do Azure

Ao utilizar um MSI, seu código pode obter tokens de acesso para se autenticarem nos recursos que suportam a autenticação do Azure AD. Neste tutorial, vai utilizar o armazenamento do Azure.

Em primeiro lugar, pode conceder o acesso de identidade do MSI para um contentor de armazenamento do Azure. Neste caso, usar o contentor que criou anteriormente. Atualize os valores para `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<STORAGE ACCOUNT NAME>`, e `<CONTAINER NAME>` conforme adequado para o seu ambiente. Além disso, substitua `<MSI PRINCIPALID>` com o `principalId` propriedade devolvida pela `az identity create` comando [criar um MSI atribuído ao utilizador](#create-a-user-assigned-msi):

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role 'Reader' --scope "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>/blobServices/default/containers/<CONTAINER NAME>"
```

A resposta inclui os detalhes para a atribuição de função que criou:

```
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Authorization/roleAssignments/b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "name": "b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "properties": {
    "principalId": "f5fdfdc1-ed84-4d48-8551-999fb9dedfbl",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.storage/storageAccounts/<STORAGE ACCOUNT NAME>/blogServices/default/<CONTAINER NAME>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="get-an-access-token-using-the-user-assigned-msis-identity-and-use-it-to-call-azure-storage"></a>Obter um token de acesso com identidade o MSI atribuído ao utilizador e utilizá-la para chamar o armazenamento do Azure

Para o resto do tutorial, terá de trabalhar a partir da VM que criou anteriormente.

Para concluir estes passos, terá de um cliente SSH. Se estiver a utilizar o Windows, pode utilizar o cliente SSH no [subsistema Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about). Se precisar de ajuda a configurar chaves de seu cliente SSH, veja [como utilizar chaves SSH com Windows no Azure](~/articles/virtual-machines/linux/ssh-from-windows.md), ou [como criar e utilizar um par de chaves público e privado SSH para VMs do Linux no Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. No portal do Azure, navegue para **máquinas virtuais**, aceda à sua máquina virtual Linux, em seguida, a partir do **descrição geral** página clique **Connect** na parte superior. Copie a cadeia de caracteres para ligar à sua VM.
2. **Ligar** para a VM com o cliente SSH à sua escolha. 
3. Na janela de terminal, com o CURL, fazer um pedido para o ponto de final de MSI local para obter um acesso de token do armazenamento do Azure.

   O pedido CURL para adquirir um token de acesso é mostrado no exemplo a seguir. Não se esqueça de substituir `<CLIENT ID>` com o `clientId` propriedade devolvida pela `az identity create` comando [criar um MSI atribuído ao utilizador](#create-a-user-assigned-msi):
   
   ```bash
   curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fstorage.azure.com/&client_id=<MSI CLIENT ID>" 
   ```

   > [!NOTE]
   > Na solicitação anterior, o valor da `resource` parâmetro tem de ser uma correspondência exata para o que é esperado pelo Azure AD. Ao utilizar o ID de recurso de armazenamento do Azure, tem de incluir a barra no URI.
   > Na resposta seguinte, o elemento de access_token como foram abreviados para fins de brevidade.

   ```bash
   {"access_token":"eyJ0eXAiOiJ...",
   "refresh_token":"",
   "expires_in":"3599",
   "expires_on":"1504130527",
   "not_before":"1504126627",
   "resource":"https://storage.azure.com",
   "token_type":"Bearer"}
   ```

4. Utilize o token de acesso para aceder ao armazenamento do Azure, por exemplo, para ler o conteúdo do ficheiro de exemplo que tenha carregado anteriormente para o contentor. Substitua os valores dos `<STORAGE ACCOUNT>`, `<CONTAINER NAME>`, e `<FILE NAME>` pelos valores que especificou anteriormente, e `<ACCESS TOKEN>` com o token devolvido no passo anterior.

   ```bash
   curl https://<STORAGE ACCOUNT>.blob.core.windows.net/<CONTAINER NAME>/<FILE NAME> -H "x-ms-version: 2017-11-09" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   A resposta inclui o conteúdo do ficheiro:

   ```bash
   Hello world! :)
   ```

## <a name="next-steps"></a>Passos Seguintes

- Para uma descrição geral do MSI, consulte [descrição geral de identidade do serviço gerido](msi-overview.md).
- Para saber como fazer esse mesmo tutorial, utilizando um credencial SAS de armazenamento, veja [utilizar uma identidade de serviço gerida do Linux VM para aceder ao armazenamento do Azure através de uma credencial SAS](msi-tutorial-linux-vm-access-storage-sas.md)
- Para obter mais informações sobre a funcionalidade SAS de conta de armazenamento do Azure, consulte:
  - [Utilizar assinaturas de acesso partilhado (SAS)](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Construindo um serviço SAS](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Utilize a seguinte secção de comentários para fornecer comentários e ajude-na refinar e moldar o nosso conteúdo.





