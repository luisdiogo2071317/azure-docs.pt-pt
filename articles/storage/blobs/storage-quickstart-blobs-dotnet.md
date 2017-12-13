---
title: "Início Rápido do Azure – Transferir objetos de/para o armazenamento de Blobs do Azure com .NET | Microsoft Docs"
description: Aprenda rapidamente a transferir objetos de/para o armazenamento de Blobs do Azure com .NET
services: storage
documentationcenter: storage
author: tamram
manager: jeconnoc
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/10/2017
ms.author: tamram
ms.openlocfilehash: ca4cb2dea9cdd2e46c3aef042e525acdfc09de8e
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/23/2017
---
# <a name="transfer-objects-tofrom-azure-blob-storage-using-net"></a>Transferir objetos de/para o armazenamento de Blobs do Azure com .NET

Neste início rápido, vai aprender a utilizar a biblioteca de cliente de .NET para o Armazenamento do Azure, para carregar, transferir e listar blobs de blocos num contentor.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este início rápido, instale o [Visual Studio 2017](https://www.visualstudio.com/visual-studio-homepage-vs.aspx) com a carga de trabalho seguinte:

- **Desenvolvimento do Azure**

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Transferir a aplicação de exemplo

A aplicação de exemplo utilizada neste início rápido é uma aplicação de consola básica. 

Utilize o [git](https://git-scm.com/) para transferir uma cópia da aplicação para o seu ambiente de desenvolvimento. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-dotnet-quickstart.git
```

Este comando clona o repositório para a sua pasta local do git. Para abrir a solução do Visual Studio, procure a pasta storage-blobs-dotnet-quickstart, abra-a e faça duplo clique em storage-blobs-dotnet-quickstart.sln. 

## <a name="configure-your-storage-connection-string"></a>Configurar a cadeia de ligação de armazenamento

Na aplicação, tem de indicar a cadeia de ligação da sua conta de armazenamento. Abra o ficheiro `app.config` no Explorador de Soluções do Visual Studio. Encontre a entrada `StorageConnectionString`. Em **valor**, substitua o valor completo da cadeia de ligação pelo que guardou a partir do portal do Azure. `storageConnectionString` deve ter um aspeto semelhante ao seguinte:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
  <appSettings>
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;
    AccountName=<NameHere>;
    AccountKey=<KeyHere>" />
  </appSettings>
</configuration>
```

## <a name="run-the-sample"></a>Executar o exemplo

Este exemplo cria um ficheiro de teste em Os Meus Documentos, carrega-o para o armazenamento de Blobs, lista os blobs no contentor e, depois, transfere o ficheiro com um nome novo, para que possa comparar o ficheiro novo e o antigo. 

Prima F5 para executar o exemplo. Mostra a saída numa janela de consola que é semelhante a: 

```
Temp file = C:\Users\azureuser\Documents\QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt
Uploading to Blob storage as blob 'QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt'
List blobs in container.
https://mystorage.blob.core.windows.net/quickstartblobs/QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8.txt
Downloading blob to C:\Users\azureuser\Documents\QuickStart_cbd5f95c-6ab8-4cbf-b8d2-a58e85d7a8e8_DOWNLOADED.txt
```

Quando prime qualquer tecla para continuar, o contentor de armazenamento e os ficheiros são eliminados. Antes de continuar, procure os dois ficheiros em Os Meus Documentos. Pode abrir e ver que são idênticos. Copie o URL para o blob fora da janela da consola e cole-o num browser para ver os conteúdos do ficheiro no armazenamento de Blobs.

Também pode utilizar uma ferramenta como o [Explorador de Armazenamento do Azure](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) para ver os ficheiros no armazenamento de Blobs. O Explorador de Armazenamento do Azure é uma ferramenta multiplataformas gratuita que lhe permite aceder às informações da sua conta de armazenamento. 

Depois de verificar os ficheiros, prima qualquer tecla para concluir a demonstração e eliminar os ficheiros de teste. Agora que sabe o que o exemplo faz, abra o ficheiro Program.cs para ver o código. 

## <a name="understand-the-sample-code"></a>Compreender o código de exemplo

Em seguida, vamos analisar o código de exemplo, para que saiba como funciona.

### <a name="get-references-to-the-storage-objects"></a>Obter referências para os objetos de armazenamento

A primeira coisa a fazer é criar as referências para os objetos utilizados para aceder e gerir ao armazenamento de Blobs. Estes objetos dependem uns dos outros; cada um é utilizado pelo que vem a seguir na lista.

* Crie uma instância do objeto **CloudStorageAccount** que aponte para a conta de armazenamento. 

* Crie uma instância do objeto **CloudBlobClient** que aponte para o serviço de Blobs na sua conta de armazenamento. 

* Crie uma instância do objeto **CloudBlobContainer**, que represente o contentor a que está aceder. Os contentores são utilizados para organizar os blobs, da mesma forma como utiliza pastas para organizar os ficheiros.

Quando tiver **CloudBlobContainer**, pode criar uma instância do objeto **CloudBlockBlob**, que aponte para o blob específico no qual está interessado e realizar uma operação de carregamento, transferência, cópia, etc.

> [!IMPORTANT]
> Os nomes dos contentores têm de estar em minúscula. Para obter informações sobre os nomes dos contentores e dos blobs, veja [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Dar Nomes e Referenciar Contentores, Blobs e Metadados).

Nesta secção, vai criar uma instância dos objetos, criar um contentor novo e, em seguida, definir as permissões no contentor, para que os blobs sejam públicos e possam ser acedidos com apenas um URL. O contentor é designado **quickstartblobs**. 

Este exemplo utiliza **CreateIfNotExists**, uma vez que queremos criar um contentor novo sempre que o exemplo é executado. Num ambiente de produção onde for utilizado o mesmo contentor em toda a aplicação, é melhor prática chamar **CreateIfNotExists** apenas uma vez. Em alternativa, pode criar o contentor com antecedência, para que não tenha de criar o código.

```csharp
// Create a CloudStorageAccount instance pointing to your storage account.
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the CloudBlobClient that is used to call the Blob Service for that storage account.
CloudBlobClient cloudBlobClient = storageAccount.CreateCloudBlobClient();

// Create a container called 'quickstartblobs'. 
CloudBlobContainer cloudBlobContainer = 
    cloudBlobClient.GetContainerReference("quickstartblobs");
await cloudBlobContainer.CreateIfNotExistsAsync();

// Set the permissions so the blobs are public. 
BlobContainerPermissions permissions = new BlobContainerPermissions();
permissions.PublicAccess = BlobContainerPublicAccessType.Blob;
await cloudBlobContainer.SetPermissionsAsync(permissions);
```

### <a name="upload-blobs-to-the-container"></a>Carregar blobs para o contentor

O armazenamento de blobs suporta blobs de blocos, blobs de acréscimo e blobs de páginas. Os blobs de blocos são os mais utilizados e vamos utilizá-los neste início rápido. 

Para carregar um ficheiro para um blob, obtenha uma referência para o blob no contentor de destino. Assim que tiver a referência de blob, pode carregar dados para o mesmo com [CloudBlockBlob.UploadFromFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromfileasync). Esta operação cria o blob, caso este ainda não exista, ou substitui-o se o mesmo já existir.

O código de exemplo cria um ficheiro local que vai ser utilizado para o carregamento e a transferência, armazenando o ficheiro a carregar como **fileAndPath** e o nome do blob em **localFileName**. O exemplo seguinte carrega o ficheiro para o seu contentor com o nome **quickstartblobs**.

```csharp
// Create a file in MyDocuments to test the upload and download.
string localPath = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
string localFileName = "QuickStart_" + Guid.NewGuid().ToString() + ".txt";
string fileAndPath = Path.Combine(localPath, localFileName);
File.WriteAllText(fileAndPath, "Hello, World!");

//Upload the file.
CloudBlockBlob blockBlob = container.GetBlockBlobReference(localFileName);
await blockBlob.UploadFromFileAsync(fileAndPath);
```

Pode utilizar diversos métodos de carregamento com o armazenamento de Blobs. Por exemplo, se tiver um fluxo de memória, pode utilizar o método UploadFromStreamAsync em vez de UploadFromFileAsync. 

Os blobs de blocos podem ser qualquer tipo de ficheiro binário ou de texto. Os blobs de páginas utilizam-se principalmente para os ficheiros VHD utilizados para fazer cópias de VMs de IaaS. Os blobs de acréscimo são utilizados para registo, como quando quer escrever num ficheiro e continuar a adicionar mais informações. A maioria dos objetos guardados no armazenamento de Blobs são blobs de blocos.

### <a name="list-the-blobs-in-a-container"></a>Listar os blobs num contentor

Pode obter uma lista de ficheiros no contentor com [CloudBlobContainer.ListBlobsSegmentedAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmentedasync). O código seguinte obtém a lista de blobs, depois percorre-a e mostra os URIs dos blobs encontrados. Pode copiar o URI a partir da janela de comandos e colá-lo num browser para ver o ficheiro.

Se tiver cinco mil ou menos blobs no contentor, são obtidos todos os nomes dos blobs numa chamada para ListBlobsSegmentedAsync. Se tiver mais de cinco mil blobs no contentor, o serviço obtém a lista em grupos de cinco mil até terem sido obtidos todos os nomes. Assim, da primeira vez que esta API for chamada, devolve os primeiros cinco mil nomes de blobs e um token de continuação. Da segunda vez, é fornecido o token e o serviço obtém o grupo seguinte de nomes de blobs e assim sucessivamente, até que o token de continuação seja nulo, o que indica que já foram obtidos todos os nomes. 

```csharp
// List the blobs in the container.
Console.WriteLine("List blobs in container.");
BlobContinuationToken blobContinuationToken = null;
do
{
    var results = await cloudBlobContainer.ListBlobsSegmentedAsync(null, blobContinuationToken);
    foreach(IListBlobItem item in results.Results)
    {
        Console.WriteLine(item.Uri);
    }
} while (blobContinuationToken != null);
```

### <a name="download-blobs"></a>Transferir blobs

Utilize [CloudBlob.DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync) para transferir blobs para o seu disco local.

O código seguinte transfere o blob carregado numa secção anterior, adicionando o sufixo "_DOWNLOADED", ao nome do mesmo, para que possa ver ambos os ficheiros no disco local. 

```csharp
// Download blob. In most cases, you would have to retrieve the reference
//   to cloudBlockBlob here. However, we created that reference earlier, and 
//   haven't changed the blob we're interested in, so we can reuse it. 
// First, add a _DOWNLOADED before the .txt so you can see both files in MyDocuments.
string fileAndPath2 = fileAndPath.Replace(".txt", "_DOWNLOADED.txt");
Console.WriteLine("Downloading blob to {0}", fileAndPath2);
await cloudBlockBlob.DownloadToFileAsync(fileAndPath2, FileMode.Create);
```

### <a name="clean-up-resources"></a>Limpar recursos

Se já não precisa dos blobs carregados neste início rápido, pode eliminar o contentor inteiro com [CloudBlobContainer.DeleteAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteasync). Elimine também os ficheiros criados, se já não precisar dos mesmos.

```csharp
await cloudBlobContainer.DeleteAsync();
File.Delete(fileAndPath);
File.Delete(fileAndPath2);    
```

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, aprendeu a transferir ficheiros entre um disco local e o armazenamento de Blobs do Azure com .NET. Para saber mais sobre a utilização do armazenamento de Blobs, avance para os procedimentos do armazenamento de blobs.

> [!div class="nextstepaction"]
> [Procedimentos de Operações de Armazenamento de Blobs](storage-dotnet-how-to-use-blobs.md)

Para obter exemplos de código do Armazenamento do Azure adicionais que pode transferir e executar, veja a lista [Azure Storage samples using .NET](../common/storage-samples-dotnet.md) (Exemplos do Armazenamento do Azure com .NET).

Para obter mais informações sobre o Explorador de armazenamento e os Blobs, veja [Manage Azure Blob storage resources with Storage Explorer](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Gerir recursos do armazenamento de Blobs do Azure com o Explorador de Armazenamento).
