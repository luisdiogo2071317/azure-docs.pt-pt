---
title: "Utilize um utilizador atribuído MSI numa VM com Linux para aceder ao armazenamento do Azure"
description: "Um tutorial que explica o processo de utilização de um utilizador atribuído geridos serviço de identidade (MSI) numa VM com Linux para aceder ao armazenamento do Azure."
services: active-directory
documentationcenter: 
author: bryanLa
manager: mtillman
editor: arluca
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 91fe06825d1db586b715617241b0ca39115414c0
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-linux-vm-to-access-azure-storage"></a>Utilize um utilizador atribuído geridos serviço de identidade (MSI) numa VM com Linux para aceder ao armazenamento do Azure

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Este tutorial mostra como criar e utilizar um utilizador atribuído geridos serviço de identidade (MSI) uma Máquina Virtual do Linux, em seguida, utilizá-lo para aceder ao armazenamento do Azure. Saiba como:

> [!div class="checklist"]
> * Criar um utilizador atribuído a identidade de serviço geridas (MSI)
> * Atribuir o MSI utilizador atribuído a uma máquina de Virtual do Linux
> * Conceder o acesso do MSI para uma instância de armazenamento do Azure
> * Obter um token de acesso utilizando a identidade MSI atribuída de utilizador e utilizá-lo para aceder ao armazenamento do Azure

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Para executar os exemplos de script CLI neste tutorial, tem duas opções:

- Utilize [Shell de nuvem do Azure](~/articles/cloud-shell/overview.md) do portal do Azure ou através do botão "Tente-", localizada no canto superior direito de cada bloco de código.
- [Instale a versão mais recente do CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 ou posterior) se preferir utilizar uma consola local do CLI.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual Linux num novo grupo de recursos

Primeiro de criar uma nova VM do Linux. Também pode ativar MSI numa VM existente se preferir.

1. Clique em de **+ /Safari/Chrome criar novo serviço** botão encontrado no canto superior esquerdo do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Ubuntu Server 16.04 LTS**.
3. Introduza as informações da máquina virtual. Para **tipo de autenticação**, selecione **chave pública SSH** ou **palavra-passe**. As credenciais criadas permitem-lhe iniciar sessão VM.

    ![Texto alternativo da imagem](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

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

## <a name="assign-your-user-assigned-msi-to-your-linux-vm"></a>Atribuir o MSI utilizador atribuído à sua VM do Linux

Ao contrário de um MSI atribuído do sistema, um MSI atribuída de utilizador pode ser utilizado por clientes em vários recursos do Azure. Para este tutorial, atribua-a uma única VM. Também pode atribuir a mais do que uma VM.

Atribuir o MSI utilizador atribuído à VM com Linux utilizando [az vm atribuir-identity](/cli/azure/vm#az_vm_assign_identity). Não se esqueça de substituir o `<RESOURCE GROUP>` e `<VM NAME>` valores de parâmetros com os seus próprios valores. Utilize o `id` propriedade devolvida no passo anterior para o `--identities` valor do parâmetro:

```azurecli-interactive
az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="create-a-storage-account"></a>Criar uma conta do Storage 

Se ainda não tiver um, agora crie uma conta de armazenamento. Também pode ignorar este passo e utilizar uma conta de armazenamento existente, se preferir. 

1. Clique em de **+ /Safari/Chrome criar novo serviço** botão encontrado no canto superior esquerdo do portal do Azure.
2. Clique em **armazenamento**, em seguida, **conta de armazenamento**, e apresenta um novo painel "Criar a conta de armazenamento".
3. Introduza um **nome** para a conta de armazenamento, o que utilizar mais tarde.  
4. **Modelo de implementação** e **conta kind** deve ser definido como "Gestor de recursos" e "Objetivo geral", respetivamente. 
5. Certifique-se a **subscrição** e **grupo de recursos** corresponder aqueles que especificou quando criou a VM no passo anterior.
6. Clique em **Criar**.

    ![Criar nova conta de armazenamento](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Criar um contentor de BLOBs na conta de armazenamento

Porque os ficheiros requerem armazenamento de BLOBs, terá de criar um contentor do blob no qual pretende armazenar o ficheiro. Em seguida, carregar e transferir um ficheiro para o contentor de blob na conta de armazenamento nova.

1. Navegue de volta para a sua conta de armazenamento criados recentemente.
2. Clique em de **contentores** ligação no lado esquerdo, em BLOBs "serviço".
3. Clique em **+ contentor** no topo da página e um contentor"novo" painel slides enviados.
4. Dê um nome de contentor, selecione um nível de acesso, em seguida, clique em **OK**. O nome que especificou também é utilizado mais tarde no tutorial. 

    ![Criar contentor de armazenamento](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Carregar um ficheiro para o contentor recentemente criado ao clicar no nome do contentor, em seguida, **carregar**, em seguida, selecione um ficheiro, em seguida, clique em **carregar**.

    ![Carregar ficheiro de texto](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-user-assigned-msi-access-to-an-azure-storage-container"></a>Conceder o acesso de utilizador atribuída MSI para um contentor de armazenamento do Azure

Ao utilizar um MSI, o seu código pode obter tokens de acesso para autenticar em recursos que suportam a autenticação do Azure AD. Neste tutorial, utilize o armazenamento do Azure.

Primeiro conceder o acesso de identidade do MSI para um contentor de armazenamento do Azure. Neste caso, utilize o contentor que criou anteriormente. Atualize os valores para `<MSI PRINCIPALID>`, `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<STORAGE ACCOUNT NAME>`, e `<CONTAINER NAME>` conforme adequado para o seu ambiente. Substitua `<CLIENT ID>` com o `clientId` propriedade devolvida pelo `az identity create` no [criar um MSI utilizador atribuído](#create-a-user-assigned-msi):

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role ‘Reader’ --scope "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>/blobServices/default/containers/<CONTAINER NAME>"
```

A resposta inclui os detalhes para a atribuição de função criada:

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

## <a name="get-an-access-token-using-the-user-assigned-msis-identity-and-use-it-to-call-azure-storage"></a>Obter um token de acesso através da identidade do utilizador atribuído do MSI e utilizá-la para chamar o Storage do Azure

Para o resto do tutorial, terá de trabalhar a partir da VM que criou anteriormente.

Para concluir estes passos, precisa de um cliente SSH. Se estiver a utilizar o Windows, pode utilizar o cliente SSH o [subsistema Windows para Linux](https://msdn.microsoft.com/commandline/wsl/about). Se precisar de assistência para configurar as chaves do seu cliente SSH, consulte [como chaves de utilizar o SSH com o Windows no Azure](~/articles/virtual-machines/linux/ssh-from-windows.md), ou [como criar e utilizar um par de chaves público e privado SSH para VMs com Linux no Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. No portal do Azure, navegue para **máquinas virtuais**, vá para a máquina virtual Linux, em seguida, a partir de **descrição geral** página clique **Connect** na parte superior. Copie a cadeia para ligar à VM.
2. **Ligar** para a VM com o cliente SSH à sua escolha. 
3. Na janela de terminal, utilizando CURL, efetue um pedido para o ponto final local de MSI para obter acesso token para armazenamento do Azure.

   O pedido CURL para adquirir um token de acesso é mostrado no exemplo seguinte. Não se esqueça de substituir `<CLIENT ID>` com o `clientId` propriedade devolvida pelo `az identity create` no [criar um MSI utilizador atribuído](#create-a-user-assigned-msi):

   ```bash
   curl -H Metadata:true "http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fstorage.azure.com/&client_id=<CLIENT ID>"
   ```

   > [!NOTE]
   > No pedido anterior, o valor da `resource` parâmetro tem de ser uma correspondência exata para que é esperado pelo Azure AD. Ao utilizar o ID de recurso de armazenamento do Azure, tem de incluir a barra no final no URI.
   > Na resposta seguinte, o elemento de access_token conforme foi shortened de uma forma abreviada.

   ```bash
   {"access_token":"eyJ0eXAiOiJ...",
   "refresh_token":"",
   "expires_in":"3599",
   "expires_on":"1504130527",
   "not_before":"1504126627",
   "resource":"https://storage.azure.com",
   "token_type":"Bearer"}
   ```

4. Agora utilize o token de acesso para aceder ao armazenamento do Azure, por exemplo, para ler o conteúdo do ficheiro de exemplo que carregou anteriormente para o contentor. Substitua os valores de `<STORAGE ACCOUNT>`, `<CONTAINER NAME>`, e `<FILE NAME>` com os valores que especificou anteriormente, e `<ACCESS TOKEN>` com o token devolvido no passo anterior.

   ```bash
   curl https://<STORAGE ACCOUNT>.blob.core.windows.net/<CONTAINER NAME>/<FILE NAME>?api-version=2017-11-09 -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   A resposta contém o conteúdo do ficheiro:

   ```bash
   Hello world! :)
   ```

## <a name="next-steps"></a>Passos Seguintes

- Para obter uma descrição geral do MSI, consulte [descrição geral de identidade de serviço geridas](msi-overview.md).
- Para saber como fazê-lo este mesmo tutorial, utilizando um credencial SAS do armazenamento, consulte [utilizar uma identidade de serviço geridas do Linux VM para aceder ao armazenamento do Azure através de uma credencial SAS](msi-tutorial-linux-vm-access-storage-sas.md)
- Para obter mais informações sobre a funcionalidade SAS de conta do Storage do Azure, consulte:
  - [Utilizar assinaturas de acesso partilhado (SAS)](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Construir um serviço SAS](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Utilize a seguinte secção de comentários para fornecer comentários e ajudam-nos refinar e formam o nosso conteúdo.





