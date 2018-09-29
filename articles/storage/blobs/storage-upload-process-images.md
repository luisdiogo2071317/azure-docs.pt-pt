---
title: Carregar dados de imagem na cloud com o Armazenamento do Azure | Microsoft Docs
description: Utilizar o armazenamento de blobs do Azure com uma aplicação Web para armazenar dados da aplicação
services: storage
documentationcenter: ''
author: tamram
ms.service: storage
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 02/20/2018
ms.author: tamram
ms.custom: mvc
ms.openlocfilehash: 237599a5dbd39147b02e9a85cbe34502d0d91923
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227049"
---
# <a name="upload-image-data-in-the-cloud-with-azure-storage"></a>Carregar dados de imagem na cloud com o Armazenamento do Azure

Este tutorial é a primeira parte de uma série. Este tutorial mostra como implementar uma aplicação Web que utiliza a Biblioteca de Cliente de Armazenamento do Azure para carregar as imagens para uma conta de armazenamento. Quando tiver terminado, terá uma aplicação Web que armazena e apresenta as imagens a partir do armazenamento do Azure.

# <a name="nettabnet"></a>[\.NET](#tab/net)
![Vista do contentor de imagens](media/storage-upload-process-images/figure2.png)

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)
![Vista do contentor de imagens](media/storage-upload-process-images/upload-app-nodejs-thumb.png)

---

Na primeira parte da série, saiba como:

> [!div class="checklist"]
> * Criar uma conta de armazenamento
> * Criar um contentor e definir permissões
> * Obter uma chave de acesso
> * Configurar definições da aplicação
> * Implementar uma Aplicação Web no Azure
> * Interagir com a aplicação Web

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este tutorial requer a execução da versão 2.0.4 ou posterior da CLI do Azure. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Criar um grupo de recursos 

Crie um grupo de recursos com o comando [az group create](/cli/azure/group#az_group_create). Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos.

O exemplo seguinte cria um grupo de recursos com o nome `myResourceGroup`.

```azurecli-interactive
az group create --name myResourceGroup --location westcentralus 
```

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento

O exemplo carrega imagens para um contentor de blobs numa conta de Armazenamento do Azure. Uma conta de armazenamento fornece um espaço de nomes exclusivo para armazenar e aceder aos objetos de dados de armazenamento do Azure. Utilize o comando [az storage account create](/cli/azure/storage/account#az_storage_account_create) para criar uma conta de armazenamento no grupo de recursos que criou.

> [!IMPORTANT]
> Na segunda parte do tutorial, utiliza subscrições de Eventos para o armazenamento de blobs. As subscrições de eventos são atualmente suportadas apenas para contas de armazenamento de Blobs nas seguintes localizações: Sudeste Asiático, Leste da Ásia, Leste da Austrália, Sudeste da Austrália, E.U.A. Central, E.U.A Leste, Asia Southeast 2, Oeste da Europa, Norte da Europa, Leste do Japão, Oeste do Japão, E.U.A. Centro-Oeste, E.U.A. Oeste e E.U.A. Leste 2. Devido a esta restrição, tem de criar uma conta de armazenamento de Blobs que seja utilizada pela aplicação de exemplo para armazenar imagens e miniaturas.

No comando seguinte, substitua o nome da conta de armazenamento de Blobs globalmente exclusivo onde vir o marcador de posição `<blob_storage_account>`.  

```azurecli-interactive
az storage account create --name <blob_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind blobstorage --access-tier hot 
```

## <a name="create-blob-storage-containers"></a>Criar contentores de armazenamento de blobs

A aplicação utiliza dois contentores na conta de armazenamento de Blobs. Os contentores são semelhantes às pastas e são utilizados para armazenar blobs. O contentor de _imagens_ é para onde a aplicação carrega imagens com máxima resolução. Numa secção mais adiante da série, uma aplicação de função do Azure carrega as miniaturas de imagem dimensionadas para o contentor de _miniaturas_.

Obtenha a chave da conta de armazenamento com o comando [az storage account keys list](/cli/azure/storage/account/keys#az_storage_account_keys_list). Em seguida, utilize esta chave para criar dois contentores com o comando [az storage container create](/cli/azure/storage/container#az_storage_container_create).  

Neste caso, `<blob_storage_account>` é o nome da conta de armazenamento de Blobs que criou. O acesso público dos contentores de _imagens_ está definido como `off` e o acesso público dos contentores de _miniaturas_ está definido como `container`. A definição de acesso público de `container` permite às pessoas que acedem à página Web visualizar as miniaturas.

```azurecli-interactive
blobStorageAccount=<blob_storage_account>

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv)

az storage container create -n images --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access off

az storage container create -n thumbnails --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access container

echo "Make a note of your blob storage account key..."
echo $blobStorageAccountKey
```

Tome nota da chave e do nome da conta de armazenamento de blobs. A aplicação de exemplo utiliza estas definições para ligar à conta de armazenamento para carregar imagens. 

## <a name="create-an-app-service-plan"></a>Crie um plano do Serviço de Aplicações

Um [plano do serviço de aplicações](../../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) especifica o local, tamanho e funcionalidades da farm de servidores Web que aloja a aplicação.

Crie um plano do Serviço de Aplicações com o comando [az appservice plan create](/cli/azure/appservice/plan#az_appservice_plan_create).

O exemplo seguinte cria um plano do Serviço de Aplicações com o nome `myAppServicePlan`, que utiliza o escalão de preços **Gratuito**.

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

## <a name="create-a-web-app"></a>Criar uma aplicação Web

A aplicação Web fornece um espaço de alojamento para o código da aplicação de exemplo implementado a partir do repositório de exemplo do GitHub. Crie uma [aplicação Web](../../app-service/app-service-web-overview.md) no plano do `myAppServicePlan`Serviço de Aplicações com o comando[az webapp create](/cli/azure/webapp#az_webapp_create).  

No comando seguinte, substitua `<web_app>` por um nome exclusivo (os carateres válidos são `a-z`, `0-9` e `-`). Se `<web_app>` não for exclusivo, obtém a mensagem de erro _O Website com o nome `<web_app>` já existe._ O URL predefinido da aplicação Web é `https://<web_app>.azurewebsites.net`.  

```azurecli-interactive
az webapp create --name <web_app> --resource-group myResourceGroup --plan myAppServicePlan
```

## <a name="deploy-the-sample-app-from-the-github-repository"></a>Implementar a aplicação de exemplo a partir do repositório do GitHub

# <a name="nettabnet"></a>[\.NET](#tab/net)

O Serviço de Aplicações suporta várias formas de implementar conteúdo numa aplicação Web. Neste tutorial, vai implementar a aplicação Web a partir de um [repositório de exemplo do GitHub público](https://github.com/Azure-Samples/storage-blob-upload-from-webapp). Configure a implementação do GitHub para a aplicação Web com o comando [az webapp deployment source config](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config). Substitua `<web_app>` pelo nome da aplicação Web que criou no passo anterior.

O projeto de exemplo contém uma aplicação [ASP.NET MVC](https://www.asp.net/mvc) que aceita uma imagem, guarda-a numa conta de armazenamento e apresenta imagens a partir de um contentor de miniaturas. A aplicação Web utiliza os espaços de nomes [Microsoft.WindowsAzure.Storage](/dotnet/api/microsoft.windowsazure.storage?view=azure-dotnet), [Microsoft.WindowsAzure.Storage.Blob](/dotnet/api/microsoft.windowsazure.storage.blob?view=azure-dotnet)e [Microsoft.WindowsAzure.Storage.Auth](/dotnet/api/microsoft.windowsazure.storage.auth?view=azure-dotnet) a partir da Biblioteca de Cliente de armazenamento do Azure para interagir com o armazenamento do Azure.

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)
O Serviço de Aplicações suporta várias formas de implementar conteúdo numa aplicação Web. Neste tutorial, vai implementar a aplicação Web a partir de um [repositório de exemplo do GitHub público](https://github.com/Azure-Samples/storage-blob-upload-from-webapp-node). Configure a implementação do GitHub para a aplicação Web com o comando [az webapp deployment source config](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config). Substitua `<web_app>` pelo nome da aplicação Web que criou no passo anterior.

---

```azurecli-interactive
az webapp deployment source config --name <web_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
```

## <a name="configure-web-app-settings"></a>Configurar as definições da aplicação Web

A aplicação Web de exemplo utiliza a [Biblioteca de Cliente de Armazenamento do Azure](/dotnet/api/overview/azure/storage?view=azure-dotnet) para pedir tokens de acesso, os quais são utilizados para carregar imagens. As credenciais da conta de armazenamento utilizadas pelo SDK de Armazenamento são estabelecidas nas definições da aplicação Web. Adicione as definições à aplicação implementada com o comando [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set).

No comando seguinte, `<blob_storage_account>` é o nome da conta de armazenamento de Blobs e `<blob_storage_key>` é a chave associada. Substitua `<web_app>` pelo nome da aplicação Web que criou no passo anterior.

```azurecli-interactive
az webapp config appsettings set --name <web_app> --resource-group myResourceGroup \
--settings AzureStorageConfig__AccountName=<blob_storage_account> \
AzureStorageConfig__ImageContainer=images  \
AzureStorageConfig__ThumbnailContainer=thumbnails \
AzureStorageConfig__AccountKey=<blob_storage_key>  
```

Após a implementação e configuração da aplicação Web, pode testar a funcionalidade de carregamento de imagens na aplicação.

## <a name="upload-an-image"></a>Carregar uma imagem

Para testar a aplicação Web, navegue para o URL da aplicação publicada. O URL predefinido da aplicação Web é `https://<web_app>.azurewebsites.net`.
Selecione a região **Carregar fotografias** para selecionar e carregar um ficheiro ou arrastar e largar um ficheiro na região. A imagem desaparece se for carregada com êxito.

# <a name="nettabnet"></a>[\.NET](#tab/net)

![Aplicação ImageResizer](media/storage-upload-process-images/figure1.png)

No código de exemplo, a tarefa `UploadFiletoStorage` no ficheiro `Storagehelper.cs` é utilizada para carregar as imagens para o contentor `images` na conta de armazenamento através do método [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet). O exemplo de código seguinte contém a tarefa `UploadFiletoStorage`.

```csharp
public static async Task<bool> UploadFileToStorage(Stream fileStream, string fileName, AzureStorageConfig _storageConfig)
{
    // Create storagecredentials object by reading the values from the configuration (appsettings.json)
    StorageCredentials storageCredentials = new StorageCredentials(_storageConfig.AccountName, _storageConfig.AccountKey);

    // Create cloudstorage account by passing the storagecredentials
    CloudStorageAccount storageAccount = new CloudStorageAccount(storageCredentials, true);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get reference to the blob container by passing the name by reading the value from the configuration (appsettings.json)
    CloudBlobContainer container = blobClient.GetContainerReference(_storageConfig.ImageContainer);

    // Get the reference to the block blob from the container
    CloudBlockBlob blockBlob = container.GetBlockBlobReference(fileName);

    // Upload the file
    await blockBlob.UploadFromStreamAsync(fileStream);

    return await Task.FromResult(true);
}
```

As classes e métodos seguintes são utilizados na tarefa anterior:

|Classe  |Método  |
|---------|---------|
|[StorageCredentials](/dotnet/api/microsoft.windowsazure.storage.auth.storagecredentials?view=azure-dotnet)     |         |
|[CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount?view=azure-dotnet)    |  [CreateCloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.createcloudblobclient?view=azure-dotnet#Microsoft_WindowsAzure_Storage_CloudStorageAccount_CreateCloudBlobClient)       |
|[CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient?view=azure-dotnet)     |[GetContainerReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient.getcontainerreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobClient_GetContainerReference_System_String_)         |
|[CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet)    | [GetBlockBlobReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.getblockblobreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_GetBlockBlobReference_System_String_)        |
|[CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azure-dotnet)     | [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet)        |

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)

![Aplicação de Carregamento de Imagens](media/storage-upload-process-images/upload-app-nodejs.png)

No código de exemplo, a rota `post` é responsável por carregar a imagem para um contentor de blobs. A rota utiliza os módulos para ajudar a processar o carregamento:

- [multer](https://github.com/expressjs/multer) implementa a estratégia de carregamento para o processador de rotas
- [into-stream](https://github.com/sindresorhus/into-stream) converte a memória intermédia num fluxo, conforme exigido pelo [createBlockBlobFromStream](http://azure.github.io/azure-sdk-for-node/azure-storage-legacy/latest/BlobService.html#createBlockBlobFromStream)

À medida que o ficheiro é enviado para a rota, os conteúdos do ficheiro permanecem na memória até que o ficheiro é carregado para o contentor de blobs.

> [!IMPORTANT]
> O carregamento de ficheiros muito grandes para a memória pode ter um efeito negativo no desempenho da sua aplicação Web. Se pretende que os utilizadores publiquem ficheiros grandes, convém considerar a preparação dos ficheiros no sistema de ficheiros do servidor Web e, em seguida, agendar carregamentos para o armazenamento de blobs. Assim que os ficheiros estiverem no armazenamento de blobs, pode removê-los do sistema de ficheiros do servidor.

```javascript
const
      express = require('express')
    , router = express.Router()

    , multer = require('multer')
    , inMemoryStorage = multer.memoryStorage()
    , uploadStrategy = multer({ storage: inMemoryStorage }).single('image')

    , azureStorage = require('azure-storage')
    , blobService = azureStorage.createBlobService()

    , getStream = require('into-stream')
    , containerName = 'images'
;

const handleError = (err, res) => {
    res.status(500);
    res.render('error', { error: err });
};

const getBlobName = originalName => {
    const identifier = Math.random().toString().replace(/0\./, ''); // remove "0." from start of string
    return `${originalName}-${identifier}`;
};

router.post('/', uploadStrategy, (req, res) => {

    const
          blobName = getBlobName(req.file.originalname)
        , stream = getStream(req.file.buffer)
        , streamLength = req.file.buffer.length
    ;

    blobService.createBlockBlobFromStream(containerName, blobName, stream, streamLength, err => {

        if(err) {
            handleError(err);
            return;
        }

        res.render('success', { 
            message: 'File uploaded to Azure Blob storage.' 
        });
    });
});
```
---

## <a name="verify-the-image-is-shown-in-the-storage-account"></a>Verifique se a imagem é apresentada na conta de armazenamento

Inicie sessão no [portal do Azure](https://portal.azure.com). No menu à esquerda, selecione **Contas de armazenamento** e selecione o nome da conta de armazenamento. Em **Descrição geral**, selecione o contentor de **imagens**.

Verifique se a imagem é apresentada no contentor.

![Vista do contentor de imagens](media/storage-upload-process-images/figure13.png)

## <a name="test-thumbnail-viewing"></a>Testar a visualização de miniaturas

Para testar a visualização de miniaturas, carregue uma imagem para o contentor de miniaturas para garantir que a aplicação consegue ler o contentor de miniaturas.

Inicie sessão no [portal do Azure](https://portal.azure.com). No menu à esquerda, selecione **Contas de armazenamento** e selecione o nome da conta de armazenamento. Selecione **Contentores** em **Serviço Blob** e selecione o contentor de **miniaturas**. Selecione **Carregar** para abrir o painel **Carregar blob**.

Escolha um ficheiro com o seletor de ficheiros e selecione **Carregar**.

Navegue novamente para a aplicação para verificar se a imagem carregada no contentor de **miniaturas** está visível.

# <a name="nettabnet"></a>[\.NET](#tab/net)
![Vista do contentor de imagens](media/storage-upload-process-images/figure2.png)

# <a name="nodejstabnodejs"></a>[Node.js](#tab/nodejs)
![Vista do contentor de imagens](media/storage-upload-process-images/upload-app-nodejs-thumb.png)

---

No contentor de **miniaturas** no portal do Azure, selecione a imagem que carregou e selecione **Eliminar** para eliminar a imagem. Na segunda parte da série, vai automatizar a criação de imagens em miniatura, pelo que esta imagem de teste não é necessária.

A CDN pode ser ativada para o conteúdo da cache a partir da conta de armazenamento do Azure. Embora não esteja descrito neste tutorial, para saber como ativar a CDN com a sua conta de armazenamento do Azure, pode visitar: [Integrar uma conta de armazenamento do Azure na CDN do Azure](../../cdn/cdn-create-a-storage-account-with-cdn.md).

## <a name="next-steps"></a>Passos seguintes

Na primeira parte da série, aprendeu a configurar uma aplicação Web ao interagir com o armazenamento, como:

> [!div class="checklist"]
> * Criar uma conta de armazenamento
> * Criar um contentor e definir permissões
> * Obter uma chave de acesso
> * Configurar definições da aplicação
> * Implementar uma Aplicação Web no Azure
> * Interagir com a aplicação Web

Avance para a segunda parte da série para saber como utilizar o Event Grid para acionar uma função do Azure para redimensionar uma imagem.

> [!div class="nextstepaction"]
> [Utilizar o Event Grid para acionar uma Função do Azure para redimensionar uma imagem carregada](../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
