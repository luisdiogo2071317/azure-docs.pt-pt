---
title: "Início Rápido do Azure – Transferir objetos de/para o armazenamento de Blobs do Azure com Java | Microsoft Docs"
description: Aprenda rapidamente a transferir objetos de/para o armazenamento de Blobs do Azure com Java
author: roygara
manager: timlt
services: storage
ms.service: storage
ms.topic: quickstart
ms.date: 11/01/2017
ms.author: v-rogara
ms.custom: mvc
ms.openlocfilehash: 5676cef446de7a68d3d8fd1a3b6833a5de184ea1
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/17/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-java"></a>Transferir objetos de/para o armazenamento de Blobs do Azure com Java

Neste início rápido, vai aprender a utilizar Java para carregar, transferir e listar blobs de blocos num contentor no armazenamento de Blobs do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de início rápido:

* Instale um IDE com integração do Maven

* Em alternativa, instale e configure o Maven para funcionar a partir da linha de comandos

Este tutorial utiliza [Eclipse](http://www.eclipse.org/downloads/) com a configuração “Eclipse IDE para programadores Java”.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Transferir a aplicação de exemplo

A [aplicação de exemplo](https://github.com/Azure-Samples/storage-blobs-java-quickstart) utilizada neste início rápido é uma aplicação de consola básica. 

Utilize o [git](https://git-scm.com/) para transferir uma cópia da aplicação para o seu ambiente de desenvolvimento. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-quickstart.git
```

Este comando clona o repositório para a sua pasta local do git. Para abrir o projeto, inicie o Eclipse e feche o ecrã de boas-vindas. Selecione **File** (Ficheiro) e **Open Projects from File System...** (Abrir Projetos a Partir do Sistema de Ficheiros...). Confirme que **Detect and configure project natures** (Detetar e configurar tipos de projeto) está assinalado. Selecione **Directory** (Diretório), navegue para o local onde armazenou o repositório clonado e, aí, selecione a pasta **javaBlobsQuickstart**. Certifique-se de que o projeto **javaBlobsQuickstarts** aparece como projeto do Eclipse e selecione **Finish** (Concluir).

Após a importação do projeto, abra **AzureApp.java** (localizado em **blobQuickstart.blobAzureApp**, dentro de **src/main/java**) e substitua `accountname` e `accountkey` dentro da cadeia`storageConnectionString`. Em seguida, execute a aplicação.
     

## <a name="configure-your-storage-connection-string"></a>Configurar a cadeia de ligação de armazenamento
    
Na aplicação, tem de indicar a cadeia de ligação da sua conta de armazenamento. Abra o ficheiro **AzureApp.Java**. Encontre a variável `storageConnectionString`. Substitua os valores `AccountName` e `AccountKey` na cadeia de ligação pelos valores que guardou no portal do Azure. `storageConnectionString` deve ter um aspeto semelhante ao seguinte:

```java
public static final String storageConnectionString =
"DefaultEndpointsProtocol=https;" +
"AccountName=<account-name>;" +
"AccountKey=<account-key>";
```

## <a name="run-the-sample"></a>Executar o exemplo

Este exemplo cria um ficheiro de teste no seu diretório predefinido (Os Meus Documentos, para utilizadores do Windows), carrega-o para o armazenamento de Blobs, lista os blobs no contentor e, depois, transfere o ficheiro com um nome novo, para que possa comparar o ficheiro novo e o antigo. 

Prima **Ctrl+F11** no Eclipse para executar o exemplo.

Se quiser executar o exemplo mediante a utilização do Maven na linha de comandos, abra uma shell e navegue para **blobAzureApp**, dentro do diretório clonado. Em seguida, introduza `mvn compile exec:java`.

Abaixo, pode ver um exemplo da saída se executasse a aplicação no Windows.

```
Azure Blob storage quick start sample
Creating container: quickstartcontainer
Creating a sample file at: C:\Users\<user>\AppData\Local\Temp\sampleFile514658495642546986.txt
Uploading the sample file 
URI of blob is: https://myexamplesacct.blob.core.windows.net/quickstartcontainer/sampleFile514658495642546986.txt
The program has completed successfully.
Press the 'Enter' key while in the console to delete the sample files, example container, and exit the application.

Deleting the container
Deleting the source, and downloaded files
```

 Antes de continuar, procure os dois ficheiros no diretório predefinido (Os Meus Documentos, para utilizadores do Windows). Pode abrir e ver que são idênticos. Copie o URL para o blob fora da janela da consola e cole-o num browser para ver os conteúdos do ficheiro no armazenamento de Blobs. Quando prime a tecla “enter”, o contentor de armazenamento e os ficheiros são eliminados.

Também pode utilizar uma ferramenta como o [Explorador de Armazenamento do Azure](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) para ver os ficheiros no armazenamento de Blobs. O Explorador de Armazenamento do Azure é uma ferramenta multiplataformas gratuita que lhe permite aceder às informações da sua conta de armazenamento. 

Depois de verificar os ficheiros, prima a tecla “enter” para concluir a demonstração e eliminar os ficheiros de teste. Agora que sabe o que o exemplo faz, abra o ficheiro **AzureApp.java** para ver o código. 

## <a name="understand-the-sample-code"></a>Compreender o código de exemplo

Em seguida, vamos analisar o código de exemplo, para que saiba como funciona.

### <a name="get-references-to-the-storage-objects"></a>Obter referências para os objetos de armazenamento

A primeira coisa a fazer é criar as referências para os objetos utilizados para aceder e gerir ao armazenamento de Blobs. Estes objetos dependem uns dos outros - cada um é utilizado pelo que vem a seguir na lista.

* Crie uma instância do objeto **CloudStorageAccount** que aponte para a [conta de armazenamento](/java/api/com.microsoft.azure.management.storage._storage_account).

    O objeto **CloudStorageAccount** é uma representação da sua conta de armazenamento e permite-lhe definir e aceder programaticamente às propriedades da conta de armazenamento. Ao utilizar o objeto **CloudStorageAccount**, pode criar uma instância de **CloudBlobClient**, que é necessário para aceder ao serviço de blobs.

* Crie uma instância do objeto **CloudBlobClient** que aponte para o [serviço de Blobs](/java/api/com.microsoft.azure.storage.blob._cloud_blob_client) na sua conta de armazenamento.

    **CloudBlobClient** disponibiliza-lhe um ponto de acesso ao serviço de blobs, permitindo-lhe definir e aceder às propriedades do armazenamento de blobs programaticamente. Ao utilizar **CloudBlobClient**, pode criar uma instância do objeto **CloudBlobContainer**, que é necessário para criar contentores.

* Crie uma instância do objeto **CloudBlobContainer**, que represente o [contentor](/java/api/com.microsoft.azure.storage.blob._cloud_blob_container) a que está aceder. Os contentores são utilizados para organizar os blobs, da mesma forma como utiliza pastas para organizar os ficheiros.    

    Quando tiver **CloudBlobContainer**, pode criar uma instância do objeto **CloudBlockBlob**, que aponte para o [blob](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob) específico no qual está interessado e realizar uma operação de carregamento, transferência, cópia, etc.

> [!IMPORTANT]
> Os nomes dos contentores têm de estar em minúscula. Para obter informações sobre os nomes dos contentores e dos blobs, veja [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Dar Nomes e Referenciar Contentores, Blobs e Metadados).

### <a name="create-a-container"></a>Criar um contentor 

Nesta secção, vai criar uma instância dos objetos, criar um contentor novo e, em seguida, definir as permissões no contentor, para que os blobs sejam públicos e possam ser acedidos com apenas um URL. O contentor é designado **quickstartblobs**. 

Este exemplo utiliza **CreateIfNotExists**, uma vez que queremos criar um contentor novo sempre que o exemplo é executado. Num ambiente de produção onde for utilizado o mesmo contentor em toda a aplicação, é melhor prática chamar **CreateIfNotExists** apenas uma vez. Em alternativa, pode criar o contentor com antecedência, para que não tenha de criar o código.

```java
// Parse the connection string and create a blob client to interact with Blob storage
storageAccount = CloudStorageAccount.parse(storageConnectionString);
blobClient = storageAccount.createCloudBlobClient();
container = blobClient.getContainerReference("quickstartcontainer");

// Create the container if it does not exist with public access.
System.out.println("Creating container: " + container.getName());
container.createIfNotExists(BlobContainerPublicAccessType.CONTAINER, new BlobRequestOptions(), new OperationContext());
```

### <a name="upload-blobs-to-the-container"></a>Carregar blobs para o contentor

O armazenamento de blobs suporta blobs de blocos, blobs de acréscimo e blobs de páginas. Os blobs de blocos são os mais utilizados e vamos utilizá-los neste início rápido. 

Para carregar um ficheiro para um blob, obtenha uma referência para o blob no contentor de destino. Assim que tiver a referência do blob, pode carregar dados para o mesmo com [CloudBlockBlob.Upload](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.upload#com_microsoft_azure_storage_blob__cloud_block_blob_upload_final_InputStream_final_long). Esta operação cria o blob, caso este ainda não exista, ou substitui-o se o mesmo já existir.

O código de exemplo cria um ficheiro local para ser utilizado para o carregamento e a transferência. Guarda o ficheiro que vai ser carregado como **origem** e o nome do objeto no **blob**. O exemplo seguinte carrega o ficheiro para o seu contentor com o nome **quickstartblobs**.

```java
//Creating a sample file
sourceFile = File.createTempFile("sampleFile", ".txt");
System.out.println("Creating a sample file at: " + sourceFile.toString());
Writer output = new BufferedWriter(new FileWriter(sourceFile));
output.write("Hello Azure!");
output.close();

//Getting a blob reference
CloudBlockBlob blob = container.getBlockBlobReference(sourceFile.getName());

//Creating blob and uploading file to it
System.out.println("Uploading the sample file ");
blob.uploadFromFile(sourceFile.getAbsolutePath());
```

Pode utilizar diversos [métodos de carregamento](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob) com o armazenamento de Blobs. Por exemplo, se tiver uma cadeia, pode utilizar o método UploadText em vez de Carregamento. 

Os blobs de blocos podem ser qualquer tipo de ficheiro binário ou de texto. Os blobs de páginas utilizam-se principalmente para os ficheiros VHD utilizados para fazer cópias de VMs de IaaS. Os blobs de acréscimo são utilizados para registo, como quando quer escrever num ficheiro e continuar a adicionar mais informações. A maioria dos objetos guardados no armazenamento de Blobs são blobs de blocos.

### <a name="list-the-blobs-in-a-container"></a>Listar os blobs num contentor

Pode obter uma lista de ficheiros no contentor com [CloudBlobContainer.ListBlobs](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.listblobs#com_microsoft_azure_storage_blob__cloud_blob_container_listBlobs). O código seguinte obtém a lista de blobs, depois percorre-a e mostra os URIs dos blobs encontrados. Pode copiar o URI a partir da janela de comandos e colá-lo num browser para ver o ficheiro.

```java
//Listing contents of container
for (ListBlobItem blobItem : container.listBlobs()) {
    System.out.println("URI of blob is: " + blobItem.getUri());
}
```

### <a name="download-blobs"></a>Transferir blobs

Utilize [CloudBlob.DownloadToFile](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob.downloadtofile#com_microsoft_azure_storage_blob__cloud_blob_downloadToFile_final_String) para transferir blobs para o seu disco local.

O código seguinte transfere o blob carregado numa secção anterior, adicionando o sufixo "_DOWNLOADED", ao nome do mesmo, para que possa ver ambos os ficheiros no disco local. 

```java
// Download blob. In most cases, you would have to retrieve the reference
// to cloudBlockBlob here. However, we created that reference earlier, and 
// haven't changed the blob we're interested in, so we can reuse it. 
// Here we are creating a new file to download to. Alternatively you can also pass in the path as a string into downloadToFile method: blob.downloadToFile("/path/to/new/file").
downloadedFile = new File(sourceFile.getParentFile(), "downloadedFile.txt");
blob.downloadToFile(downloadedFile.getAbsolutePath());
```

### <a name="clean-up-resources"></a>Limpar recursos

Se já não precisa dos blobs carregados neste início rápido, pode eliminar o contentor inteiro com [CloudBlobContainer.DeleteIfExists](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.deleteifexists#com_microsoft_azure_storage_blob__cloud_blob_container_deleteIfExists). Isto também elimina os ficheiros no contentor.

```java
try {
if(container != null)
    container.deleteIfExists();
} catch (StorageException ex) {
System.out.println(String.format("Service error. Http code: %d and error code: %s", ex.getHttpStatusCode(), ex.getErrorCode()));
}

System.out.println("Deleting the source, and downloaded files");

if(downloadedFile != null)
downloadedFile.deleteOnExit();
        
if(sourceFile != null)
sourceFile.deleteOnExit();
```

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, aprendeu a transferir ficheiros entre um disco local e o armazenamento de Blobs do Azure com Java. Para saber mais sobre a utilização do armazenamento de Blobs, avance para os procedimentos do armazenamento de blobs.

> [!div class="nextstepaction"]
> [Procedimentos de Operações de Armazenamento de Blobs](storage-java-how-to-use-blob-storage.md)

Para obter mais informações sobre o Explorador de armazenamento e os Blobs, veja [Manage Azure Blob storage resources with Storage Explorer](../../vs-azure-tools-storage-explorer-blobs.md) (Gerir recursos do armazenamento de Blobs do Azure com o Explorador de Armazenamento).
