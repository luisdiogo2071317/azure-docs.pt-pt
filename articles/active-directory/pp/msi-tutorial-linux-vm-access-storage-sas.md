---
title: Utilizar um MSI de VM do Linux para aceder ao armazenamento do Azure utilizando uma credencial SAS
description: Um tutorial que mostra-lhe como utilizar um Linux VM Managed Service Identity (MSI) para aceder ao armazenamento do Azure, utilizando uma credencial SAS em vez de uma chave de acesso da conta de armazenamento.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: ccf6b41edbd085abeb358cf4fe624956b54946b9
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056122"
---
# <a name="use-a-linux-vm-managed-service-identity-to-access-azure-storage-via-a-sas-credential"></a>Utilizar uma identidade de serviço gerida do Linux VM para aceder ao armazenamento do Azure através de uma credencial SAS

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Este tutorial mostra como ativar a identidade de serviço gerida (MSI) para uma Máquina Virtual do Linux, em seguida, utilizar o MSI para obter uma credencial de assinatura de acesso partilhado (SAS) de armazenamento. Especificamente, um [SAS de serviço de credenciais](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#types-of-shared-access-signatures). 

Uma SAS de serviço fornece a capacidade de conceder acesso limitado aos objetos na conta de armazenamento para um período limitado e um serviço específico (no nosso caso, o serviço de BLOBs), sem expor uma chave de acesso da conta. Pode utilizar uma credencial SAS quando a fazer operações de armazenamento, por exemplo, ao utilizar o SDK de armazenamento como de costume. Para este tutorial, iremos demonstrar carregar e descarregar um blob com a CLI de armazenamento do Azure. Ficará a saber como:


> [!div class="checklist"]
> * Ativar o MSI numa máquina Virtual do Linux 
> * Conceder o acesso VM para uma conta de armazenamento SAS no Resource Manager 
> * Obter um token de acesso com a identidade da VM e utilizá-lo a obter a SAS do Resource Manager 

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure
Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).


## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual Linux num novo grupo de recursos

Para este tutorial, vamos criar uma nova VM do Linux. Também pode ativar o MSI numa VM existente.

1. Clique nas **c++ /CLI criar novo serviço** botão encontrado no canto superior esquerdo do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Ubuntu Server 16.04 LTS**.
3. Introduza as informações da máquina virtual. Para **tipo de autenticação**, selecione **chave pública SSH** ou **palavra-passe**. As credenciais criadas permitem-lhe iniciar sessão VM.

    ![Texto alternativo da imagem](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Escolher uma **subscrição** para a máquina virtual na lista pendente.
5. Para selecionar um novo **grupo de recursos** pretende que a máquina virtual para ser criado no, escolha **criar nova**. Quando terminar, clique em **OK**.
6. Selecione o tamanho da VM. Para ver mais tamanhos, selecione **ver todos os** ou alterar o filtro de tipo de disco suportados. No painel de definições, mantenha as predefinições e clique em **OK**.

## <a name="enable-msi-on-your-vm"></a>Ativar o MSI na sua VM

Um MSI de Máquina Virtual permite-lhe obter os tokens de acesso do Azure AD sem a necessidade de colocar as credenciais em seu código. Nos bastidores, ativar o MSI faz duas coisas: instala a extensão de VM de MSI na sua VM e permite a identidade do serviço gerido para a VM.  

1. Navegue para o grupo de recursos de sua nova máquina virtual e selecione a máquina virtual que criou no passo anterior.
2. Na VM "Definições" à esquerda, clique em **configuração**.
3. Para registar e ativar o MSI, selecione **Sim**, se desejar para desabilitá-lo, selecione não.
4. Certifique-se de que clica **guardar** para guardar a configuração.

    ![Texto alternativo da imagem](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Se pretender verificar quais as extensões são na VM, clique em **extensões**. Se o MSI é ativado, o **ManagedIdentityExtensionforLinux** aparece na lista.

    ![Texto alternativo da imagem](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento 

Se ainda não tiver uma, agora, irá criar uma conta de armazenamento.  Também pode ignorar este passo e conceder o acesso MSI de VM para as chaves de uma conta de armazenamento existente. 

1. Clique nas **c++ /CLI criar novo serviço** botão encontrado no canto superior esquerdo do portal do Azure.
2. Clique em **armazenamento**, em seguida, **conta de armazenamento**, e um novo painel "Criar a conta de armazenamento" será exibida.
3. Introduza um **nome** para a conta de armazenamento, que irá utilizar mais tarde.  
4. **Modelo de implementação** e **tipo de conta** deve ser definido como "Resource manager" e "Fins gerais", respectivamente. 
5. Certifique-se a **subscrição** e **grupo de recursos** corresponder aos perfis que especificou quando criou a VM no passo anterior.
6. Clique em **Criar**.

    ![Criar nova conta de armazenamento](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Criar um contentor de BLOBs na conta de armazenamento

Iremos mais tarde carregar e transferir um ficheiro para a nova conta de armazenamento. Uma vez que ficheiros requerem armazenamento de BLOBs, é necessário criar um contentor de BLOBs para armazenar o ficheiro.

1. Navegue de volta para a sua conta de armazenamento recentemente criada.
2. Clique nas **contentores** ligação no painel do lado esquerdo, sob "Serviço de Blob".
3. Clique em **+ contentor** na parte superior da página e um "novo contentor" painel desliza.
4. Dê um nome ao contentor, selecione um nível de acesso, em seguida, clique em **OK**. O nome especificado será utilizado mais tarde no tutorial. 

    ![Criar contentor de armazenamento](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-msi-access-to-use-a-storage-sas"></a>Conceder acesso MSI da VM para utilizar um armazenamento de SAS 

O armazenamento do Azure não suporta nativamente a autenticação do Azure AD.  No entanto, pode utilizar um MSI para obter um SAS de armazenamento do Resource Manager, em seguida, utilizar a SAS para aceder ao armazenamento.  Neste passo, pode conceder o acesso MSI de VM para a sua conta de armazenamento SAS.   

1. Navegue de volta para a sua conta de armazenamento recentemente criada....   
2. Clique nas **controlo de acesso (IAM)** ligação no painel do lado esquerdo.  
3. Clique em **+ adicionar** na parte superior da página para adicionar uma nova atribuição de função para a sua VM
4. Definir **função** para "Armazenamento contribuinte de conta", no lado direito da página. 
5. Na lista pendente seguinte, defina **atribuir acesso a** o recurso "Máquina Virtual".  
6. Em seguida, certifique-se a subscrição correta está listada na **subscrição** menu pendente, em seguida, defina **grupo de recursos** para "Todos os grupos de recursos".  
7. Por fim, em **selecionar** escolha sua máquina Virtual do Linux na lista pendente, em seguida, clique em **guardar**.  

    ![Texto alternativo da imagem](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/msi-storage-role-sas.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>Obter um token de acesso com a identidade da VM e utilizá-la para chamar o Azure Resource Manager

Para o resto do tutorial, iremos trabalhar da VM que criámos anteriormente.

Para concluir estes passos, terá de um cliente SSH. Se estiver a utilizar o Windows, pode utilizar o cliente SSH no [subsistema Windows para Linux](https://msdn.microsoft.com/commandline/wsl/install_guide). Se precisar de ajuda a configurar chaves de seu cliente SSH, veja [como utilizar chaves SSH com Windows no Azure](~/articles/virtual-machines/linux/ssh-from-windows.md), ou [como criar e utilizar um par de chaves público e privado SSH para VMs do Linux no Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. No portal do Azure, navegue para **máquinas virtuais**, aceda à sua máquina virtual Linux, em seguida, a partir do **descrição geral** página clique **Connect** na parte superior. Copie a cadeia de caracteres para ligar à sua VM. 
2. Ligar à VM através de seu cliente SSH.  
3. Em seguida, será solicitado que introduza no seu **palavra-passe** adicionou ao criar o **VM do Linux**. Deve, em seguida, ser com êxito conectado.  
4. Utilize o CURL para obter um token de acesso para o Azure Resource Manager.  

    O pedido CURL e a resposta para o token de acesso está ilustrado abaixo:
    
    ```bash
    curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true    
    ```
    
    > [!NOTE]
    > Na solicitação anterior, o valor do parâmetro "recurso" tem de ser uma correspondência exata para o que é esperado pelo Azure AD. Ao utilizar o ID de recurso do Azure Resource Manager, tem de incluir a barra no URI.
    > Na resposta seguinte, o elemento de access_token como foram abreviados para fins de brevidade.
    
    ```bash
    {"access_token":"eyJ0eXAiOiJ...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://management.azure.com",
    "token_type":"Bearer"} 
     ```

## <a name="get-a-sas-credential-from-azure-resource-manager-to-make-storage-calls"></a>Obter uma credencial SAS do Azure Resource Manager para fazer chamadas de armazenamento

Agora utilize o CURL para chamar o Resource Manager com o token de acesso que foi obtidos na secção anterior, para criar uma credencial SAS de armazenamento. Assim que tivermos a credencial SAS, podemos chamar operações de carregamento/transferência de armazenamento.

Para este pedido, usaremos os parâmetros de pedido HTTP de seguir para criar a credencial SAS:

```JSON
{
    "canonicalizedResource":"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>",
    "signedResource":"c",              // The kind of resource accessible with the SAS, in this case a container (c).
    "signedPermission":"rcw",          // Permissions for this SAS, in this case (r)ead, (c)reate, and (w)rite.  Order is important.
    "signedProtocol":"https",          // Require the SAS be used on https protocol.
    "signedExpiry":"<EXPIRATION TIME>" // UTC expiration time for SAS in ISO 8601 format, for example 2017-09-22T00:06:00Z.
}
```

Estes parâmetros estão incluídos no corpo POST da solicitação para a credencial SAS. Para obter mais informações sobre os parâmetros para a criação de uma credencial SAS, consulte a [referência de REST de SAS de serviço de lista](/rest/api/storagerp/storageaccounts/listservicesas).

Utilize o pedido CURL seguinte para obter a credencial SAS. Certifique-se de que substitua a `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<STORAGE ACCOUNT NAME>`, `<CONTAINER NAME>`, e `<EXPIRATION TIME>` valores de parâmetros com seus próprios valores. Substitua o `<ACCESS TOKEN>` valor com o token de acesso que obteve anteriormente:

```bash 
curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>/listServiceSas/?api-version=2017-06-01 -X POST -d "{\"canonicalizedResource\":\"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>\",\"signedResource\":\"c\",\"signedPermission\":\"rcw\",\"signedProtocol\":\"https\",\"signedExpiry\":\"<EXPIRATION TIME>\"}" -H "Authorization: Bearer <ACCESS TOKEN>"
```

> [!NOTE]
> O texto no URL anterior diferencia maiúsculas de minúsculas, por isso, certifique-se se estiver a utilizar o canto superior-minúsculas para seus grupos de recursos para refletir isso adequadamente. Além disso, é importante saber que se trata de um pedido POST não um pedido GET.

A resposta CURL retorna a credencial SAS:  

```bash 
{"serviceSasToken":"sv=2015-04-05&sr=c&spr=https&st=2017-09-22T00%3A10%3A00Z&se=2017-09-22T02%3A00%3A00Z&sp=rcw&sig=QcVwljccgWcNMbe9roAJbD8J5oEkYoq%2F0cUPlgriBn0%3D"} 
```

Crie um ficheiro de blob de exemplo para carregar para o seu contentor de armazenamento de Blobs. Numa VM do Linux que pode fazer isso com o seguinte comando. 

```bash
echo "This is a test file." > test.txt
```

Em seguida, autenticar com a CLI `az storage` usando a credencial SAS de comandos e carregar o ficheiro para o contentor de Blobs. Para este passo, precisará [instalar a CLI do Azure mais recente](https://docs.microsoft.com/cli/azure/install-azure-cli) na sua VM, se ainda não o fez.

```azurecli-interactive
 az storage blob upload --container-name 
                        --file 
                        --name
                        --account-name 
                        --sas-token
```

Resposta: 

```JSON
Finished[#############################################################]  100.0000%
{
  "etag": "\"0x8D4F9929765C139\"",
  "lastModified": "2017-09-21T03:58:56+00:00"
}
```

Além disso, pode baixar o arquivo com a CLI do Azure e a autenticação com a credencial SAS. 

Pedido: 

```azurecli-interactive
az storage blob download --container-name
                         --file 
                         --name 
                         --account-name
                         --sas-token
```

Resposta: 

```JSON
{
  "content": null,
  "metadata": {},
  "name": "testblob",
  "properties": {
    "appendBlobCommittedBlockCount": null,
    "blobType": "BlockBlob",
    "contentLength": 16,
    "contentRange": "bytes 0-15/16",
    "contentSettings": {
      "cacheControl": null,
      "contentDisposition": null,
      "contentEncoding": null,
      "contentLanguage": null,
      "contentMd5": "Aryr///Rb+D8JQ8IytleDA==",
      "contentType": "text/plain"
    },
    "copy": {
      "completionTime": null,
      "id": null,
      "progress": null,
      "source": null,
      "status": null,
      "statusDescription": null
    },
    "etag": "\"0x8D4F9929765C139\"",
    "lastModified": "2017-09-21T03:58:56+00:00",
    "lease": {
      "duration": null,
      "state": "available",
      "status": "unlocked"
    },
    "pageBlobSequenceNumber": null,
    "serverEncrypted": false
  },
  "snapshot": null
}
```

## <a name="next-steps"></a>Passos Seguintes

- Para uma descrição geral do MSI, consulte [descrição geral de identidade do serviço gerido](msi-overview.md).
- Para saber como realizar este tutorial mesmo com uma chave de conta de armazenamento, consulte [utilizar uma identidade de serviço gerida do Linux VM para aceder ao armazenamento do Azure](msi-tutorial-linux-vm-access-storage.md)
- Para obter mais informações sobre a funcionalidade SAS de conta de armazenamento do Azure, consulte:
  - [Utilizar assinaturas de acesso partilhado (SAS)](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Construindo um serviço SAS](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Utilize a seguinte secção de comentários para fornecer comentários e ajude-na refinar e moldar o nosso conteúdo.
