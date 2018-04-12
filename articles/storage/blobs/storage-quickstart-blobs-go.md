---
title: Início Rápido do Azure - carregar, transferir e listar os blobs no Armazenamento do Microsoft Azure através de Go | Microsoft Docs
description: Neste início rápido, crie uma conta de armazenamento e um contentor. Em seguida, utilize a biblioteca de clientes de armazenamento para Go, para carregar um blob para o Armazenamento do Microsoft Azure, transferir um blob e listar os blobs num contentor.
services: storage
author: seguler
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 02/22/2018
ms.author: seguler
ms.openlocfilehash: b288c764c3a6b5821f913170f8f9c88e5cc86956
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="quickstart-upload-download-and-list-blobs-using-go"></a>Início Rápido: carregar, transferir e listar blobs através de Go

Neste início rápido, vai aprender a utilizar a linguagem de programação Go para carregar, transferir e listar blobs de blocos num contentor no armazenamento de Blobs do Azure. 

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de início rápido: 
* Instale o [Go 1.8 ou superior](https://golang.org/dl/)
* Transfira e instale o [SDK do Azure Storage Blob para Go](https://github.com/azure/azure-storage-blob-go/) com `go get -u github.com/azure/azure-storage-blob-go/2016-05-31/azblob`. 

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Transferir a aplicação de exemplo
O [exemplo de aplicação](https://github.com/Azure-Samples/storage-blobs-go-quickstart.git) utilizado neste início rápido é uma aplicação Go básica.  

Utilize o [git](https://git-scm.com/) para transferir uma cópia da aplicação para o seu ambiente de desenvolvimento. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-go-quickstart 
```

Este comando clona o repositório para a sua pasta local do git. Para abrir o exemplo de Go para o Armazenamento de blobs, procure o ficheiro storage-quickstart.go.  

[!INCLUDE [storage-copy-account-key-portal](../../../includes/storage-copy-account-key-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Configurar a cadeia de ligação de armazenamento
Esta solução requer o nome e a chave da conta de armazenamento para ser armazenada em segurança nas variáveis de ambiente locais do computador que executa o exemplo. Siga um dos exemplos abaixo, consoante o sistema operativo, para criar as variáveis de ambiente.

# <a name="linuxtablinux"></a>[Linux](#tab/Linux)

```
export AZURE_STORAGE_ACCOUNT="<youraccountname>"
export AZURE_STORAGE_ACCESS_KEY="<youraccountkey>"
```

# <a name="windowstabwindows"></a>[Windows](#tab/Windows)

```
setx AZURE_STORAGE_ACCOUNT "<youraccountname>"
setx AZURE_STORAGE_ACCESS_KEY "<youraccountkey>"
```

---

## <a name="run-the-sample"></a>Executar o exemplo
Este exemplo cria um ficheiro de teste na pasta atual, carrega o ficheiro de teste para o armazenamento de Blobs, lista os blobs no contentor e transfere o ficheiro para uma memória intermédia. 

Para executar o exemplo, emita o comando seguinte: 

```go run storage-quickstart.go```

O resultado seguinte é um exemplo do resultado devolvido ao executar a aplicação:
  
```
Azure Blob storage quick start sample
Creating a container named quickstart-5568059279520899415
Creating a dummy file to test the upload and download
Uploading the file with blob name: 630910657703031215
Blob name: 630910657703031215
Downloaded the blob: hello world
this is a blob
Press the enter key to delete the sample files, example container, and exit the application.
```
Quando prime a tecla para continuar, o programa de exemplo elimina o contentor de armazenamento e os ficheiros. 

> [!TIP]
> Também pode utilizar uma ferramenta como o [Explorador de Armazenamento do Azure](http://storageexplorer.com) para ver os ficheiros no armazenamento de Blobs. O Explorador de Armazenamento do Azure é uma ferramenta multiplataformas gratuita que lhe permite aceder às informações da sua conta de armazenamento. 
>

## <a name="understand-the-sample-code"></a>Compreender o código de exemplo

Em seguida, vamos analisar o código de exemplo, para que saiba como funciona.

### <a name="create-containerurl-and-bloburl-objects"></a>Criar objetos ContainerURL e BlobURL
A primeira coisa a fazer é criar as referências para os objetos ContainerURL e BlobURL utilizados para aceder e gerir o armazenamento de Blobs. Estes objetos oferecem APIs de baixo nível, tais como Create, PutBlob e GetBlob, para emitir APIs REST.

* Utilize a estrutura **SharedKeyCredential** para armazenar as suas credenciais. 

* Crie um **Pipeline** com as credenciais e as opções. O pipeline especifica coisas como as políticas de repetição, registo, desserialização de payloads de resposta HTTP e muito mais.  

* Instancie um novo objeto ContainerURL e um novo objeto BlobURL para executar operações no contentor (Criar) e nos blobs (PutBlob e GetBlob).


Assim que tiver o ContainerURL, pode instanciar o objeto **BlobURL** que aponta para um blob e executar operações como carregar, transferir e copiar.

> [!IMPORTANT]
> Os nomes dos contentores têm de estar em minúscula. Para obter informações sobre os nomes dos contentores e dos blobs, veja [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Dar Nomes e Referenciar Contentores, Blobs e Metadados).

Nesta secção, vai criar um novo contentor. O contentor é chamado **quickstartblobs-[random string]**. 

```go 
// From the Azure portal, get your storage account name and key and set environment variables.
accountName, accountKey := os.Getenv("AZURE_STORAGE_ACCOUNT"), os.Getenv("AZURE_STORAGE_ACCESS_KEY")
if len(accountName) == 0 || len(accountKey) == 0 {
    log.Fatal("Either the AZURE_STORAGE_ACCOUNT or AZURE_STORAGE_ACCESS_KEY environment variable is not set")
}

// Create a default request pipeline using your storage account name and account key.
credential := azblob.NewSharedKeyCredential(accountName, accountKey)
p := azblob.NewPipeline(credential, azblob.PipelineOptions{})

// Create a random string for the quick start container
containerName := fmt.Sprintf("quickstart-%s", randomString())

// From the Azure portal, get your storage account blob service URL endpoint.
URL, _ := url.Parse(
    fmt.Sprintf("https://%s.blob.core.windows.net/%s", accountName, containerName))

// Create a ContainerURL object that wraps the container URL and a request
// pipeline to make requests.
containerURL := azblob.NewContainerURL(*URL, p)

// Create the container
fmt.Printf("Creating a container named %s\n", containerName)
ctx := context.Background() // This example uses a never-expiring context
_, err := containerURL.Create(ctx, azblob.Metadata{}, azblob.PublicAccessNone)
handleErrors(err)
```
### <a name="upload-blobs-to-the-container"></a>Carregar blobs para o contentor

O armazenamento de blobs suporta blobs de blocos, blobs de acréscimo e blobs de páginas. Os blobs de blocos são os mais utilizados e vamos utilizá-los neste guia de introdução.  

Para carregar um ficheiro para um blob, abra o ficheiro com **os.Open**. Em seguida, pode carregar o ficheiro para o caminho especificado com um das APIs REST: PutBlob, PutBlock/PutBlockList. 

Em alternativa, o SDK oferece [APIs de alto nível](https://github.com/Azure/azure-storage-blob-go/blob/master/2016-05-31/azblob/highlevel.go) que são criadas sobre as APIs REST de baixo nível. Por exemplo, a função ***UploadFileToBlockBlob*** utiliza operações PutBlock para carregar simultaneamente um ficheiro em segmentos para otimizar o débito. Se o ficheiro tiver menos de 256 MB, utiliza a operação PutBlob para concluir a transferência numa única transação.

O exemplo seguinte carrega o ficheiro para o seu contentor com o nome **quickstartblobs-[randomstring]**.

```go
// Here's how to upload a blob.
blobURL := containerURL.NewBlockBlobURL(fileName)
file, err := os.Open(fileName)
handleErrors(err)

// You can use the low-level PutBlob API to upload files. Low-level APIs are simple wrappers for the Azure Storage REST APIs.
// Note that PutBlob can upload up to 256MB data in one shot. Details: https://docs.microsoft.com/en-us/rest/api/storageservices/put-blob
// Following is commented out intentionally because we will instead use UploadFileToBlockBlob API to upload the blob
// _, err = blobURL.PutBlob(ctx, file, azblob.BlobHTTPHeaders{}, azblob.Metadata{}, azblob.BlobAccessConditions{})
// handleErrors(err)

// The high-level API UploadFileToBlockBlob function uploads blocks in parallel for optimal performance, and can handle large files as well.
// This function calls PutBlock/PutBlockList for files larger 256 MBs, and calls PutBlob for any file smaller
fmt.Printf("Uploading the file with blob name: %s\n", fileName)
_, err = azblob.UploadFileToBlockBlob(ctx, file, blobURL, azblob.UploadToBlockBlobOptions{
    BlockSize:   4 * 1024 * 1024,
    Parallelism: 16})
handleErrors(err)
```

### <a name="list-the-blobs-in-a-container"></a>Listar os blobs num contentor

Obtenha uma lista de ficheiros no contentor com o método **ListBlobs** num **ContainerURL**. ListBlobs devolve um segmento individual de blobs (até 5000) a partir do **Marcador** especificado. Utilize um Marcador vazio para iniciar a enumeração desde o início. Os nomes dos blobs são devolvidos por ordem lexicográfica. Depois de obter um segmento, processe-o e, em seguida, chame ListBlobs novamente, transmitindo o Marcador anteriormente devolvido.  

```go
// List the blobs in the container
for marker := (azblob.Marker{}); marker.NotDone(); {
    // Get a result segment starting with the blob indicated by the current Marker.
    listBlob, err := containerURL.ListBlobs(ctx, marker, azblob.ListBlobsOptions{})
    handleErrors(err)

    // ListBlobs returns the start of the next segment; you MUST use this to get
    // the next segment (after processing the current result segment).
    marker = listBlob.NextMarker

    // Process the blobs returned in this result segment (if the segment is empty, the loop body won't execute)
    for _, blobInfo := range listBlob.Blobs.Blob {
        fmt.Print("Blob name: " + blobInfo.Name + "\n")
    }
}
```

### <a name="download-the-blob"></a>Transferir o blob

Transfira blobs com o método de baixo nível **GetBlob** num BlobURL. Em alternativa, pode criar um Fluxo e intervalos de leitura a partir do mesmo com a API de alto nível **NewDownloadStream** fornecida em [highlevel.go](https://github.com/Azure/azure-storage-blob-go/blob/master/2016-05-31/azblob/highlevel.go). A função NewDownloadStream é repetida em caso de falha de ligação, enquanto a API Get Blob apenas repete nos códigos de estado HTTP, tais como 503 (Servidor Ocupado). O código seguinte transfere o blob com a função **NewDownloadStream**. O conteúdo do blob é escrito numa memória intermédia e apresentado na consola.

```go
// Here's how to download the blob. NOTE: This method automatically retries if the connection fails
// during download (the low-level GetBlob function does NOT retry errors when reading from its stream).
stream := azblob.NewDownloadStream(ctx, blobURL.GetBlob, azblob.DownloadStreamOptions{})
downloadedData := &bytes.Buffer{}
_, err = downloadedData.ReadFrom(stream)
handleErrors(err)
```

### <a name="clean-up-resources"></a>Limpar recursos
Se já não precisar dos blobs carregados neste início rápido, pode eliminar o contentor inteiro com o método **Delete**. 

```go
// Cleaning up the quick start by deleting the container and the file created locally
fmt.Printf("Press the enter key to delete the sample files, example container, and exit the application.\n")
bufio.NewReader(os.Stdin).ReadBytes('\n')
fmt.Printf("Cleaning up.\n")
containerURL.Delete(ctx, azblob.ContainerAccessConditions{})
file.Close()
os.Remove(fileName)
```

## <a name="resources-for-developing-go-applications-with-blobs"></a>Recursos para desenvolver aplicações Go com blobs

Veja estes recursos adicionais para o desenvolvimento de Go com armazenamento de blobs:

- Veja e instale o [código fonte da biblioteca de cliente do Go](https://github.com/Azure/azure-storage-blob-go) para o Armazenamento do Azure no GitHub.
- Explore os [exemplos de armazenamento de blobs](https://godoc.org/github.com/Azure/azure-storage-blob-go/2016-05-31/azblob#pkg-examples) escritos com a biblioteca de cliente Go.

## <a name="next-steps"></a>Passos seguintes
 
Neste início rápido, aprendeu a transferir ficheiros entre um disco local e o armazenamento de Blobs do Azure com Go. Para obter mais informações sobre o SDK do Azure Storage Blob, veja as páginas [Código Fonte](https://github.com/Azure/azure-storage-blob-go/) e [Referência da API](https://godoc.org/github.com/Azure/azure-storage-blob-go/2016-05-31/azblob).
