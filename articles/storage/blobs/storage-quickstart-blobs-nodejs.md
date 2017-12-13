---
title: "Guia de Introdução do Azure – Transferir objetos de/para o armazenamento de Blobs do Azure com Node.js | Microsoft Docs"
description: Aprenda rapidamente a transferir objetos de/para o armazenamento de Blobs do Azure com Node.js
services: storage
documentationcenter: storage
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/30/2017
ms.author: gwallace
ms.openlocfilehash: dd4d3abf082767c40760d020c0997b365452e769
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-nodejs"></a>Transferir objetos de/para o armazenamento de Blobs do Azure com Node.js

Neste guia de introdução, vai aprender a utilizar o Node.js, para carregar, transferir e listar blobs de blocos num contentor no armazenamento de Blobs do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de início rápido:

* Instalar o [Node. js](https://nodejs.org/en/)

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Transferir a aplicação de exemplo

A [aplicação de exemplo](https://github.com/Azure-Samples/storage-blobs-node-quickstart.git) utilizada neste guia de introdução é uma aplicação de consola básica. 

Utilize o [git](https://git-scm.com/) para transferir uma cópia da aplicação para o seu ambiente de desenvolvimento.

```bash
git clone https://github.com/Azure-Samples/storage-blobs-node-quickstart.git
```

Este comando clona o repositório para a sua pasta local do git. Para abrir a aplicação, procure a pasta storage-blobs-node-quickstart, abra-a e faça duplo clique em index.js.

## <a name="configure-your-storage-connection-string"></a>Configurar a cadeia de ligação de armazenamento

Na aplicação, tem de indicar a cadeia de ligação da sua conta de armazenamento. Abra o ficheiro `index.js`, encontre a variável `connectionString`. Substitua o valor completo da cadeia de ligação pelo que guardou a partir do portal do Azure. A cadeia de ligação de armazenamento deve ter um aspeto semelhante ao seguinte:

```javascript
// Create a blob client for interacting with the blob service from connection string
// How to create a storage connection string - http://msdn.microsoft.com/library/azure/ee758697.aspx
var connectionString = '<Your connection string here>';
var blobService = storage.createBlobService(connectionString);
```

## <a name="install-required-packages"></a>Instalar pacotes necessários

No diretório de aplicação, execute `npm install` para instalar os pacotes necessários listados no ficheiro `package.json`.

```javascript
npm install
```

## <a name="run-the-sample"></a>Executar o exemplo

Este exemplo cria um ficheiro de teste em Os Meus Documentos, carrega-o para o armazenamento de Blobs, lista os blobs no contentor e, depois, transfere o ficheiro com um nome novo, para que possa comparar o ficheiro novo e o antigo.

Execute o exemplo ao escrever `node index.js`. O resultado seguinte provém de um sistema Windows.  Pode ser esperado um resultado semelhante com caminhos de ficheiros adequados, se utilizar o Linux.

```
Azure Storage Node.js Client Library Blobs Quick Start

1. Creating a container with public access: quickstartcontainer-79a3eea0-bec9-11e7-9a36-614cd00ca63d

2. Creating a file in ~/Documents folder to test the upload and download

   Local File: C:\Users\admin\Documents\HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt

3. Uploading BlockBlob: quickstartblob-HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt

   Uploaded Blob URL: https://mystorageaccount.blob.core.windows.net/quickstartcontainer-79a3eea0-bec9-11e7-9a36-614cd00ca63d/quickstartblob-HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt

4. Listing blobs in container

   - quickstartblob-HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d.txt (type: BlockBlob)


5. Downloading blob

   Downloaded File: C:\Users\admin\Documents\HelloWorld-79a3c790-bec9-11e7-9a36-614cd00ca63d_DOWNLOADED.txt

Sample finished running. When you hit <ENTER> key, the temporary files will be deleted and the sample application will exit.
```

Antes de continuar, procure os dois ficheiros em Os Meus Documentos. Pode abrir e ver que são idênticos.

Também pode utilizar uma ferramenta como o [Explorador de Armazenamento do Azure](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) para ver os ficheiros no armazenamento de Blobs. O Explorador de Armazenamento do Azure é uma ferramenta multiplataformas gratuita que lhe permite aceder às informações da sua conta de armazenamento.

Depois de verificar os ficheiros, prima qualquer tecla para concluir a demonstração e eliminar os ficheiros de teste. Agora que sabe o que o exemplo faz, abra o ficheiro index.js para ver o código. 

## <a name="understand-the-sample-code"></a>Compreender o código de exemplo

Em seguida, vamos analisar o código de exemplo, para que saiba como funciona.

### <a name="get-references-to-the-storage-objects"></a>Obter referências para os objetos de armazenamento

A primeira coisa a fazer é criar a referência para os `BlobService` utilizados para aceder e gerir ao armazenamento de Blobs. Estes objetos dependem uns dos outros - cada um é utilizado pelo que vem a seguir na lista.

* Crie uma instância do objeto **[BlobService](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService__ctor)** que aponte para o serviço de Blobs na sua conta de armazenamento.

* Crie um contentor novo e, em seguida, defina as permissões no contentor, para que os blobs sejam públicos e possam ser acedidos com apenas um URL. O contentor começa com **quickstartcontainer-**.

Este exemplo utiliza [createContainerCreateIfNotExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_createContainerIfNotExists), uma vez que queremos criar um contentor novo sempre que o exemplo é executado. Num ambiente de produção onde for utilizado o mesmo contentor em toda a aplicação, é melhor prática chamar CreateIfNotExists apenas uma vez. Em alternativa, pode criar o contentor com antecedência, para que não tenha de criar o código.

```javascript
// Create a container for organizing blobs within the storage account.
console.log('1. Creating a Container with Public Access:', blockBlobContainerName, '\n');
blobService.createContainerIfNotExists(blockBlobContainerName, { 'publicAccessLevel': 'blob' }, function (error) {
    if (error) return callback(error);
```

### <a name="upload-blobs-to-the-container"></a>Carregar blobs para o contentor

O armazenamento de blobs suporta blobs de blocos, blobs de acréscimo e blobs de páginas. Os blobs de blocos são os mais frequentemente utilizados. São ideais para armazenar o texto e os dados binários, sendo o motivo pelo qual são utilizados neste guia de introdução.

Para carregar um ficheiro para um blob, utilize o método [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromLocalFile). Esta operação cria o blob, caso este ainda não exista, ou substitui o mesmo se já existir.

O código de exemplo cria um ficheiro local que servirá para o carregamento e a transferência, armazenando o ficheiro a carregar como **localPath** e o nome do blob em **localFileToUpload**. O exemplo seguinte carrega o ficheiro para o seu contentor que começa com **quickstartcontainer-**.

```javascript
console.log('2. Creating a file in ~/Documents folder to test the upload and download\n');
console.log('   Local File:', LOCAL_FILE_PATH, '\n');
fs.writeFileSync(LOCAL_FILE_PATH, 'Greetings from Microsoft!');

console.log('3. Uploading BlockBlob:', BLOCK_BLOB_NAME, '\n');
blobService.createBlockBlobFromLocalFile(CONTAINER_NAME, BLOCK_BLOB_NAME, LOCAL_FILE_PATH, function (error) {
handleError(error);
console.log('   Uploaded Blob URL:', blobService.getUrl(CONTAINER_NAME, BLOCK_BLOB_NAME), '\n');
```

Pode utilizar diversos métodos de carregamento com o armazenamento de Blobs. Por exemplo, se tiver um fluxo de memória, pode utilizar o método [createBlockBlobFromStream](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromStream) em vez de [createBlockBlobFromLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_createBlockBlobFromLocalFile).

### <a name="list-the-blobs-in-a-container"></a>Listar os blobs num contentor

Em seguida, a aplicação obtém uma lista de ficheiros no contentor com [listBlobsSegmented](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_listBlobsSegmented). O código seguinte obtém a lista de blobs, depois percorre-a e mostra os URIs dos blobs encontrados. Pode copiar o URI a partir da janela de comandos e colá-lo num browser para ver o ficheiro.

Se tiver cinco mil ou menos blobs no contentor, são obtidos todos os nomes dos blobs numa chamada para [listBlobsSegmented](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_listBlobsSegmented). Se tiver mais de cinco mil blobs no contentor, o serviço obtém a lista em grupos de cinco mil até terem sido obtidos todos os nomes. Assim, da primeira vez que esta API for chamada, devolve os primeiros cinco mil nomes de blobs e um token de continuação. Da segunda vez, é fornecido o token e o serviço obtém o grupo seguinte de nomes de blobs e assim sucessivamente, até que o token de continuação seja nulo, o que indica que já foram obtidos todos os nomes.

```javascript
console.log('4. Listing blobs in container\n');
blobService.listBlobsSegmented(CONTAINER_NAME, null, function (error, data) {
    handleError(error);

    for (var i = 0; i < data.entries.length; i++) {
    console.log(util.format('   - %s (type: %s)'), data.entries[i].name, data.entries[i].blobType);
    }
    console.log('\n');
```

### <a name="download-blobs"></a>Transferir blobs

Transfira blobs para o seu disco local com [getBlobToLocalFile](/nodejs/api/azure-storage/blobservice?view=azure-node-2.2.0#azure_storage_BlobService_getBlobToLocalFile).

O código seguinte transfere o blob carregado numa secção anterior, adicionando o sufixo "_DOWNLOADED", ao nome do mesmo, para que possa ver ambos os ficheiros no disco local. 

```javascript
console.log('5. Downloading blob\n');
blobService.getBlobToLocalFile(CONTAINER_NAME, BLOCK_BLOB_NAME, DOWNLOADED_FILE_PATH, function (error) {
handleError(error);
console.log('   Downloaded File:', DOWNLOADED_FILE_PATH, '\n');
```

### <a name="clean-up-resources"></a>Limpar recursos

Se já não precisa dos blobs carregados neste guia de introdução, pode eliminar o contentor inteiro com [deleteBlobIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteBlobIfExists) e [deleteContainerIfExists](/nodejs/api/azure-storage/blobservice#azure_storage_BlobService_deleteContainerIfExists). Elimine também os ficheiros criados, se já não precisar dos mesmos. Isto é tratado na aplicação ao premir enter para sair da aplicação.

```javascript
console.log('6. Deleting block Blob\n');
    blobService.deleteBlobIfExists(CONTAINER_NAME, BLOCK_BLOB_NAME, function (error) {
        handleError(error);

    console.log('7. Deleting container\n');
    blobService.deleteContainerIfExists(CONTAINER_NAME, function (error) {
        handleError(error);
            
        fs.unlinkSync(LOCAL_FILE_PATH);
        fs.unlinkSync(DOWNLOADED_FILE_PATH);
```

## <a name="next-steps"></a>Passos seguintes

Neste guia de introdução, aprendeu a transferir ficheiros entre um disco local e o armazenamento de Blobs do Azure com Node.js. Para saber mais sobre a utilização do armazenamento de Blobs, avance para os procedimentos do armazenamento de blobs.

> [!div class="nextstepaction"]
> [Procedimentos de Operações de Armazenamento de Blobs](storage-nodejs-how-to-use-blob-storage.md)

Para obter mais informações sobre o Explorador de armazenamento e os Blobs, veja [Manage Azure Blob storage resources with Storage Explorer (Gerir recursos de armazenamento do Blob do Azure com o Explorador de Armazenamento](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)).
