---
title: Início Rápido do Azure – Criar um blob no armazenamento de objetos com o SDK V10 de Armazenamento Java | Microsoft Docs
description: Neste início rápido, vai criar um contentor no armazenamento de objetos (Blob), carregar um ficheiro, listar os objetos e transferir com o SDK de Armazenamento Java.
services: storage
author: roygara
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 07/02/2018
ms.author: rogarana
ms.openlocfilehash: a789269e73e1817f6a45e1e5948dbfaa21efd283
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38704475"
---
# <a name="quickstart-upload-download-and-list-blobs-using-the-java-sdk-v10-preview"></a>Início Rápido: Carregar, transferir e listar blobs com o SDK V10 Java (Pré-visualização)

Neste início rápido, vai aprender a utilizar o novo SDK de Armazenamento Java para carregar, transferir e listar blobs de blocos num contentor no armazenamento de Blobs do Azure. O novo SDK Java utiliza o modelo de programação Reativa com RxJava, que permite operações assíncronas. Saiba mais sobre RxJava [aqui](https://github.com/ReactiveX/RxJava). 

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de início rápido:

* Instale e configure o [Maven](http://maven.apache.org/download.cgi) para funcionar a partir da linha de comandos ou qualquer IDE Java de sua preferência
* instalar e configurar um [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Transferir a aplicação de exemplo

A [aplicação de exemplo](https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart) utilizada neste início rápido é uma aplicação de consola básica. 

Utilize o [git](https://git-scm.com/) para transferir uma cópia da aplicação para o seu ambiente de desenvolvimento.

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-v10-quickstart.git
```

Este comando clona o repositório para a sua pasta local do git.

Quando for concluída a importação do projeto, abra **Quickstart.java** (localizado em **src/main/java/quickstart**).

[!INCLUDE [storage-copy-account-key-portal](../../../includes/storage-copy-account-key-portal.md)]

## <a name="configure-your-storage-connection-string"></a>Configurar a cadeia de ligação de armazenamento
Esta solução requer o nome e a chave da conta de armazenamento para ser armazenada em segurança nas variáveis de ambiente locais do computador que executa o exemplo. Siga um dos exemplos abaixo, consoante o sistema operativo, para criar as variáveis de ambiente.

### <a name="for-linux"></a>Para Linux

```
export AZURE_STORAGE_ACCOUNT="<youraccountname>"
export AZURE_STORAGE_ACCESS_KEY="<youraccountkey>"
```

### <a name="for-windows"></a>Para Windows

```
setx AZURE_STORAGE_ACCOUNT "<youraccountname>"
setx AZURE_STORAGE_ACCESS_KEY "<youraccountkey>"
```

## <a name="run-the-sample"></a>Executar o exemplo

Este exemplo cria um ficheiro de teste no seu diretório predefinido (AppData\Local\Temp, para utilizadores do Windows), em seguida, pede-lhe para introduzir os comandos para carregar o ficheiro de teste para o armazenamento de Blobs, listar os blobs no contentor e transferir o ficheiro carregado com um novo nome, para que possa comparar o ficheiro antigo e o novo. 

Se quiser executar o exemplo mediante a utilização do Maven na linha de comandos, abra uma shell e navegue para **storage-blobs-java-v10-quickstart**, dentro do diretório clonado. Em seguida, introduza `mvn compile exec:java`.

Abaixo, pode ver um exemplo da saída se executasse a aplicação no Windows.

```
Created quickstart container
Enter a command
(P)utBlob | (L)istBlobs | (G)etBlob | (D)eleteBlobs | (E)xitSample
# Enter a command :
P
Uploading the sample file into the container: https://<storageaccount>.blob.core.windows.net/quickstart
# Enter a command :
L
Listing blobs in the container: https://<storageaccount>.blob.core.windows.net/quickstart
Blob name: SampleBlob.txt
# Enter a command :
G
Get the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
The blob was downloaded to C:\Users\<useraccount>\AppData\Local\Temp\downloadedFile13097087873115855761.txt
# Enter a command :
D
Delete the blob: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt

# Enter a command :
>> Blob deleted: https://<storageaccount>.blob.core.windows.net/quickstart/SampleBlob.txt
E
Cleaning up the sample and exiting!
```

Tem controlo do exemplo, por isso, introduza comandos para o código ser executado. Lembre-se de que as entradas são sensíveis a maiúsculas e minúsculas.

Também pode utilizar uma ferramenta como o [Explorador de Armazenamento do Azure](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) para ver os ficheiros no armazenamento de Blobs. O Explorador de Armazenamento do Azure é uma ferramenta multiplataformas gratuita que lhe permite aceder às informações da sua conta de armazenamento. 

Depois de verificar os ficheiros, **E**, prima a tecla Enter para concluir a demonstração e eliminar os ficheiros de teste. Agora que sabe o que o exemplo faz, abra o ficheiro **Quickstart.java** para ver o código. 

## <a name="understand-the-sample-code"></a>Compreender o código de exemplo

Em seguida, vamos analisar o código de exemplo, para que saiba como funciona.

### <a name="get-references-to-the-storage-objects"></a>Obter referências para os objetos de armazenamento

A primeira coisa a fazer é criar as referências para os objetos utilizados para aceder e gerir ao armazenamento de Blobs. Estes objetos dependem uns dos outros - cada um é utilizado pelo que vem a seguir na lista.

* Crie uma instância do objeto StorageURL que aponte para a conta de armazenamento.

    O objeto [**StorageURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._storage_u_r_l?view=azure-java-preview) é uma representação da conta de armazenamento e permite gerar um novo pipeline. Um pipeline ([Pipeline HTTP](https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview#url-types--http-pipeline)) é um conjunto de políticas utilizadas para manipular pedidos e respostas com mecanismos de autorização, registo e repetição. Com o pipeline, pode criar uma instância do objeto [**ServiceURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._service_u_r_l?view=azure-java-preview), que permite criar uma instância de [**ContainerURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l?view=azure-java-preview), necessário para executar operações nos contentores de Blobs.

* Crie uma instância do objeto ContainerURL, que representa o contentor ao qual está a aceder. Os contentores servem para organizar os blobs, da mesma forma como utiliza pastas para organizar os ficheiros.

    **ContainerURL** fornece um ponto de acesso para o serviço de contentor. Com **ContainerURL**, pode criar uma instância do objeto [**BlobURL**](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l?view=azure-java-preview), necessário para criar blobs.

* Crie uma instância do objeto [BlobURL](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._blob_u_r_l?view=azure-java-preview), que aponta para o blob específico que lhe interessa.

> [!IMPORTANT]
> Os nomes dos contentores têm de estar em minúscula. Para obter informações sobre os nomes dos contentores e dos blobs, veja [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Dar Nomes e Referenciar Contentores, Blobs e Metadados).

### <a name="create-a-container"></a>Criar um contentor 

Nesta secção, vai criar uma instância de ContainerURL e um novo contentor. O contentor no exemplo é designado **quickstart**. 

Este exemplo utiliza [containerURL.create](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l.create?view=azure-java-preview), uma vez que queremos criar um contentor novo sempre que o exemplo é executado. Em alternativa, pode criar o contentor com antecedência, para que não tenha de criar o código.

```java
// Create a ServiceURL to call the Blob service. We will also use this to construct the ContainerURL
SharedKeyCredentials creds = new SharedKeyCredentials(accountName, accountKey);
// We are using a default pipeline here, you can learn more about it at https://github.com/Azure/azure-storage-java/wiki/Azure-Storage-Java-V10-Overview
final ServiceURL serviceURL = new ServiceURL(new URL("http://" + accountName + ".blob.core.windows.net"), StorageURL.createPipeline(creds, new PipelineOptions()));

// Let's create a container using a blocking call to Azure Storage
// If container exists, we'll catch and continue
containerURL = serviceURL.createContainerURL("quickstart");

try {
    ContainersCreateResponse response = containerURL.create(null, null).blockingGet();
    System.out.println("Container Create Response was " + response.statusCode());
} catch (RestException e){
    if (e instanceof RestException && ((RestException)e).response().statusCode() != 409) {
        throw e;
    } else {
        System.out.println("quickstart container already exists, resuming...");
    }
}
```

### <a name="upload-blobs-to-the-container"></a>Carregar blobs para o contentor

O armazenamento de blobs suporta blobs de blocos, blobs de acréscimo e blobs de páginas. Os blobs de blocos são os mais utilizados e vamos utilizá-los neste início rápido. 

Para carregar um ficheiro para um blob, obtenha uma referência para o blob no contentor de destino. Assim que tiver a referência de blob, pode carregar um ficheiro para o mesmo através das APIs de baixo nível, como [Upload](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._block_blob_u_r_l.upload?view=azure-java-preview), também conhecido como PutBlob, [StageBlock](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._block_blob_u_r_l.stageblock?view=azure-java-preview#com_microsoft_azure_storage_blob__block_blob_u_r_l_stageBlock_String_Flowable_ByteBuffer__long_LeaseAccessConditions_), também conhecido como PutBLock, na instância de BlockBlobURL, ou as APIs de alto nível fornecidas na classe [TransferManager](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._transfer_manager?view=azure-java-preview), como o método [TransferManager.uploadFileToBlockBlob](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._transfer_manager.uploadfiletoblockblob?view=azure-java-preview). Esta operação cria o blob, caso este ainda não exista, ou substitui o mesmo se já existir.

O código de exemplo cria um ficheiro local para ser utilizado para o carregamento e a transferência. Guarde o ficheiro que vai ser carregado como **sourceFile** e o URL do blob em **blob**. O exemplo seguinte carrega o ficheiro para o seu contentor com o nome **quickstart**.

```java
static void uploadFile(BlockBlobURL blob, File sourceFile) throws IOException {
    
    FileChannel fileChannel = FileChannel.open(sourceFile.toPath());
            
    // Uploading a file to the blobURL using the high-level methods available in TransferManager class
    // Alternatively call the Upload/StageBlock low-level methods from BlockBlobURL type
    TransferManager.uploadFileToBlockBlob(fileChannel, blob, 8*1024*1024, null)
        .subscribe(response-> {
            System.out.println("Completed upload request.");
            System.out.println(response.response().statusCode());
        });
}
```

Os blobs de blocos podem ser qualquer tipo de ficheiro binário ou de texto. Os blobs de páginas utilizam-se principalmente para os ficheiros VHD utilizados para fazer cópias de VMs de IaaS. Os blobs de acréscimo destinam-se a acrescentar dados ao final e são frequentemente utilizados para registo. A maioria dos objetos guardados no armazenamento de Blobs são blobs de blocos.

### <a name="list-the-blobs-in-a-container"></a>Listar os blobs num contentor

Pode obter uma lista de objetos num contentor com [containerURL.listBlobsFlatSegment](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l.listblobsflatsegment?view=azure-java-preview). Este método devolve até 5000 objetos em simultâneo, juntamente com um marcador de continuação (marcador seguinte) se existirem mais itens a listar no contentor. Para tal, vamos criar uma função auxiliar que se chama a si própria repetidamente desde que exista um marcador seguinte na resposta listBlobsFlatSegment anterior.

```java
static void listBlobs(ContainerURL containerURL) {
    // Each ContainerURL.listBlobsFlatSegment call return up to maxResults (maxResults=10 passed into ListBlobOptions below).
    // To list all Blobs, we are creating a helper static method called listAllBlobs, 
    // and calling it after the initial listBlobsFlatSegment call
    ListBlobsOptions options = new ListBlobsOptions(null, null, 10);

    containerURL.listBlobsFlatSegment(null, options)
        .flatMap(containersListBlobFlatSegmentResponse -> 
            listAllBlobs(containerURL, containersListBlobFlatSegmentResponse))    
                .subscribe(response-> {
                    System.out.println("Completed list blobs request.");
                    System.out.println(response.statusCode());
                });
}

private static Single <ContainersListBlobFlatSegmentResponse> listAllBlobs(ContainerURL url, ContainersListBlobFlatSegmentResponse response) {                
    // Process the blobs returned in this result segment (if the segment is empty, blobs() will be null.
    if (response.body().blobs() != null) {
        for (Blob b : response.body().blobs().blob()) {
            String output = "Blob name: " + b.name();
            if (b.snapshot() != null) {
                output += ", Snapshot: " + b.snapshot();
            }
            System.out.println(output);
        }
    }
    else {
        System.out.println("There are no more blobs to list off.");
    }
    
    // If there is not another segment, return this response as the final response.
    if (response.body().nextMarker() == null) {
        return Single.just(response);
    } else {
        /*
        IMPORTANT: ListBlobsFlatSegment returns the start of the next segment; you MUST use this to get the next
        segment (after processing the current result segment
        */
            
        String nextMarker = response.body().nextMarker();

        /*
        The presence of the marker indicates that there are more blobs to list, so we make another call to
        listBlobsFlatSegment and pass the result through this helper function.
        */
            
    return url.listBlobsFlatSegment(nextMarker, new ListBlobsOptions(null, null,1))
        .flatMap(containersListBlobFlatSegmentResponse ->
            listAllBlobs(url, containersListBlobFlatSegmentResponse));
    }
}
```

### <a name="download-blobs"></a>Transferir blobs

Transfira blobs para o seu disco local com [blobURL.download](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._blob_u_r_l.download?view=azure-java-preview).

O código seguinte transfere o blob carregado numa secção anterior, adicionando o sufixo "_DOWNLOADED", ao nome do mesmo, para que possa ver ambos os ficheiros no disco local. 

```java
static void getBlob(BlockBlobURL blobURL, File sourceFile) {
    try {
        // Get the blob using the low-level download method in BlockBlobURL type
        // com.microsoft.rest.v2.util.FlowableUtil is a static class that contains helpers to work with Flowable
        blobURL.download(new BlobRange(0, Long.MAX_VALUE), null, false)
            .flatMapCompletable(response -> {
                AsynchronousFileChannel channel = AsynchronousFileChannel.open(Paths
                    .get(sourceFile.getPath()), StandardOpenOption.CREATE,  StandardOpenOption.WRITE);
                        return FlowableUtil.writeFile(response.body(), channel);
            }).doOnComplete(()-> System.out.println("The blob was downloaded to " + sourceFile.getAbsolutePath()))
            // To call it synchronously add .blockingAwait()
            .subscribe();
    } catch (Exception ex){
        System.out.println(ex.toString());
    }
}
```

### <a name="clean-up-resources"></a>Limpar recursos

Se já não precisa dos blobs carregados neste início rápido, pode eliminar o contentor inteiro com [containerURL.delete](https://docs.microsoft.com/en-us/java/api/com.microsoft.azure.storage.blob._container_u_r_l.delete?view=azure-java-preview). Este método também elimina os ficheiros no contentor.

```java
containerURL.delete(null).blockingGet();
```

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, aprendeu a transferir ficheiros entre um disco local e o armazenamento de Blobs do Azure com Java. 

> [!div class="nextstepaction"]
> [SDK V10 de Armazenamento para código fonte de Java](https://github.com/Azure/azure-storage-java/tree/New-Storage-SDK-V10-Preview)
> [Referência da API](https://docs.microsoft.com/en-us/java/api/storage/client?view=azure-java-preview)
> [Saiba mais sobre RxJava](https://github.com/ReactiveX/RxJava)