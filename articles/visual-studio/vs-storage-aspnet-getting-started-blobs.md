---
title: "Introdução ao Blob do Azure Visual Studio e de armazenamento ligado serviços (ASP.NET) | Microsoft Docs"
description: "Como começar a utilizar o armazenamento de Blobs do Azure num projeto ASP.NET no Visual Studio, depois de ligar a uma conta de armazenamento utilizando o Visual Studio ligado serviços"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: b3497055-bef8-4c95-8567-181556b50d95
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: kraig
ms.openlocfilehash: cb406e528568dafd1e142943f5273ad58e550609
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/06/2018
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a>Introdução ao Blob do Azure Visual Studio e de armazenamento ligado serviços (ASP.NET)

> [!div class="op_single_selector"]
> - [ASP.NET](./vs-storage-aspnet-getting-started-blobs.md)
> - [Núcleo do ASP.NET](./vs-storage-aspnet-core-getting-started-blobs.md)

Armazenamento de Blobs do Azure é um serviço que armazena dados não estruturados na nuvem como objetos ou os blobs. O Blob Storage pode armazenar qualquer tipo de texto ou de dados binários, tal como um documento, um ficheiro de multimédia ou um instalador da aplicação. O Blob Storage também é referido como um armazenamento de objetos.

Este tutorial mostra como escrever código ASP.NET para alguns cenários comuns que utilizam o armazenamento de Blobs. Os cenários incluem a criação de um contentor de blob, carregar, listar, transferir e eliminar blobs.

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="prerequisites"></a>Pré-requisitos

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]


[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

## <a name="create-an-mvc-controller"></a>Criar um controlador MVC 

1. No **Explorador de soluções**, faça duplo clique **controladores**.

2. No menu de contexto, selecione **adicionar** > **controlador**.

    ![Captura de ecrã do Explorador de soluções, com adicionar e controlador realçado](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. No **adicionar andaime** caixa de diálogo, selecione **controlador MVC 5 – vazio**e selecione **adicionar**.

    ![Caixa de diálogo de captura de ecrã de adicionar andaime](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. No **Adicionar controlador** caixa de diálogo, o nome do controlador *BlobsController*e selecione **adicionar**.

    ![Caixa de diálogo de captura de ecrã de adicionar controlador](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. Adicione o seguinte `using` diretivas para a `BlobsController.cs` ficheiro:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="connect-to-a-storage-account-and-get-a-container-reference"></a>Ligar a uma conta de armazenamento e obter uma referência de contentor

Um contentor do blob é uma hierarquia aninhada de blobs e de pastas. O resto dos passos neste documento exigem uma referência a um contentor de blob, para que o código deve ser colocado no seu próprio método para reusability.

Os seguintes passos criar um método para ligar à conta de armazenamento, utilizando a cadeia de ligação no **Web. config**. Os passos também criar uma referência a um contentor.  A definição de cadeia de ligação no **Web. config** é denominado com o formato `<storageaccountname>_AzureStorageConnectionString`. 

1. Abra o ficheiro `BlobsController.cs`.

1. Adicione um método denominado **GetCloudBlobContainer** que devolve um **CloudBlobContainer**.  Não se esqueça de substituir `<storageaccountname>_AzureStorageConnectionString` com o nome real da chave no **Web. config**.
    
    ```csharp
    private CloudBlobContainer GetCloudBlobContainer()
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
                CloudConfigurationManager.GetSetting("<storageaccountname>_AzureStorageConnectionString"));
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
        return container;
    }
    ```

> [!NOTE]
> Apesar de *contentor de BLOBs de teste* não existe ainda, este código cria uma referência ao mesmo. Isto é, para o contentor pode ser criado com o `CreateIfNotExists` método mostrado no próximo passo.

## <a name="create-a-blob-container"></a>Criar um contentor de blob

Os passos seguintes mostram como criar um contentor do blob:

1. Adicione um método denominado `CreateBlobContainer` que devolve um `ActionResult`.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // The code in this section goes here.

        return View();
    }
    ```
 
1. Obter um `CloudBlobContainer` objeto que representa uma referência para o nome do contentor de blob pretendido. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Chamar o `CloudBlobContainer.CreateIfNotExists` método para criar o contentor, se ainda não existir. O `CloudBlobContainer.CreateIfNotExists` método devolve **verdadeiro** se o contentor não existe e é criado com êxito. Caso contrário, o método devolve **falso**.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. Atualização `ViewBag` com o nome do contentor do blob.

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```
    
    O seguinte mostra o concluída `CreateBlobContainer` método:

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        ViewBag.Success = container.CreateIfNotExists();
        ViewBag.BlobContainerName = container.Name;

        return View();
    }
    ```

1. No **Explorador de soluções**, clique com botão direito do **vistas** pasta.

2. No menu de contexto, selecione **adicionar** > **nova pasta**. Nome da nova pasta *Blobs*. 
 
1. No **Explorador de soluções**, expanda o **vistas** pasta e o rato **Blobs**.

4. No menu de contexto, selecione **adicionar** > **vista**.

1. No **Adicionar vista** caixa de diálogo, introduza **CreateBlobContainer** para o nome da vista e selecione **adicionar**.

1. Abra `CreateBlobContainer.cshtml`e modificá-lo de modo a que se parece com o seguinte fragmento de código:

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. No **Explorador de soluções**, expanda o **vistas** > **partilhados** pasta e abra `_Layout.cshtml`.

1. Após a última **Html.ActionLink**, adicione o seguinte **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. Executar a aplicação e selecione **criar contentor de Blob** para ver os resultados semelhantes à captura de ecrã seguinte:
  
    ![Captura de ecrã de criar o contentor de blob](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    Tal como mencionado anteriormente, o `CloudBlobContainer.CreateIfNotExists` método devolve **verdadeiro** apenas quando o contentor não existe e é criado. Por conseguinte, se a aplicação é executada quando o contentor existe, o método devolve **falso**.

## <a name="upload-a-blob-into-a-blob-container"></a>Carregar um blob para um contentor do blob

Quando o [contentor do blob é criado](#create-a-blob-container), carregar ficheiros para esse contentor. Esta secção explica a carregar um ficheiro local para um contentor do blob. Os passos partem do princípio de há um contentor do blob denominado *contentor de BLOBs de teste*. 

1. Abra o ficheiro `BlobsController.cs`.

1. Adicione um método denominado `UploadBlob` que devolve uma cadeia.

    ```csharp
    public string UploadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. Dentro do `UploadBlob` método, obter um `CloudBlobContainer` objeto que representa uma referência para o nome do contentor de blob pretendido. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. Storage do Azure suporta tipos de blob diferente. Este tutorial utiliza os blobs de blocos. Para obter uma referência para um blob de bloco, chame o `CloudBlobContainer.GetBlockBlobReference` método.

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```
    
    > [!NOTE]
    > O nome do blob faz parte do URL utilizado para obter um blob e pode ser qualquer cadeia, incluindo o nome do ficheiro.

1. Após uma referência de blob, pode carregar qualquer fluxo de dados para a mesma ao chamar o objeto de referência de blob `UploadFromStream` método. O `UploadFromStream` método cria o blob, se não existir ou substitui-lo se existir. (Alterar  *&lt;carregamento do ficheiro >* para um caminho totalmente qualificado para um ficheiro a carregar.)

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(@"<file-to-upload>"))
    {
        blob.UploadFromStream(fileStream);
    }
    ```
    
    O seguinte mostra o concluída `UploadBlob` método (com um caminho totalmente qualificado para o ficheiro a ser carregado):

    ```csharp
    public string UploadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenRead(@"c:\src\sample.txt"))
        {
            blob.UploadFromStream(fileStream);
        }
        return "success!";
    }
    ```

1. No **Explorador de soluções**, expanda o **vistas** > **partilhados** pasta e abra `_Layout.cshtml`.

1. Após a última **Html.ActionLink**, adicione o seguinte **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. Executar a aplicação e selecione **carregar blob**.  A palavra *êxito!* deve aparecer.
    
    ![Captura de ecrã de verificação com êxito](./media/vs-storage-aspnet-getting-started-blobs/upload-blob.png)
  
## <a name="list-the-blobs-in-a-blob-container"></a>Listar os blobs num contentor de blob

Esta secção ilustra como listar os blobs num contentor de blob. As referências do código de exemplo a *contentor de BLOBs de teste* criou na secção, [criar um contentor de blob](#create-a-blob-container).

1. Abra o ficheiro `BlobsController.cs`.

1. Adicione um método denominado `ListBlobs` que devolve um `ActionResult`.

    ```csharp
    public ActionResult ListBlobs()
    {
        // The code in this section goes here.

    }
    ```
 
1. Dentro do `ListBlobs` método, obter um `CloudBlobContainer` objeto que representa uma referência para o contentor de blob. 
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```
   
1. Para listar os blobs num contentor de BLOBs, utilize o `CloudBlobContainer.ListBlobs` método. O `CloudBlobContainer.ListBlobs` método devolve um `IListBlobItem` objeto que pode ser convertido para um `CloudBlockBlob`, `CloudPageBlob`, ou `CloudBlobDirectory` objeto. O fragmento de código seguinte enumera todos os blobs num contentor de blob. Cada blob é convertido para o objecto apropriado, com base no respetivo tipo. O nome (ou URI em case de um **CloudBlobDirectory**) é adicionado a uma lista.

    ```csharp
    List<string> blobs = new List<string>();

    foreach (IListBlobItem item in container.ListBlobs())
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob blob = (CloudPageBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory dir = (CloudBlobDirectory)item;
            blobs.Add(dir.Uri.ToString());
        }
    }

    return View(blobs);
    ```

    Para além de blobs, os contentores de blob podem conter diretórios. Suponhamos há um contentor do blob denominado *contentor de BLOBs de teste*, com a hierarquia seguinte:

        foo.png
        dir1/bar.png
        dir2/baz.png

    Utilizando o exemplo de código anterior, o **blobs** lista de cadeias contém valores semelhantes ao seguinte:

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    Como é mostrado, a lista inclui apenas as entidades nível superior, aqueles não aninhados (*bar.png* e *baz.png*). Para listar todas as entidades dentro de um contentor de blob, alterar o código, para que o **CloudBlobContainer.ListBlobs** método transmite **verdadeiro** para o **useFlatBlobListing** parâmetro.    

    ```csharp
    //...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    //...
    ```

    Definir o **useFlatBlobListing** parâmetro **verdadeiro** devolve uma lista não hierárquica de todas as entidades no contentor do blob. Isto gera os seguintes resultados:

        foo.png
        dir1/bar.png
        dir2/baz.png
    
    O seguinte mostra o concluída **ListBlobs** método:

    ```csharp
    public ActionResult ListBlobs()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        List<string> blobs = new List<string>();
        foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing: true))
        {
            if (item.GetType() == typeof(CloudBlockBlob))
            {
                CloudBlockBlob blob = (CloudBlockBlob)item;
                blobs.Add(blob.Name);
            }
            else if (item.GetType() == typeof(CloudPageBlob))
            {
                CloudPageBlob blob = (CloudPageBlob)item;
                blobs.Add(blob.Name);
            }
            else if (item.GetType() == typeof(CloudBlobDirectory))
            {
                CloudBlobDirectory dir = (CloudBlobDirectory)item;
                blobs.Add(dir.Uri.ToString());
            }
        }

        return View(blobs);
    }
    ```

1. No **Explorador de soluções**, expanda o **vistas** pasta e o rato **Blobs**.

2. No menu de contexto, selecione **adicionar** > **vista**.

1. No **Adicionar vista** caixa de diálogo, introduza `ListBlobs` para o nome da vista e selecione **adicionar**.

1. Abra `ListBlobs.cshtml`e substitua o conteúdo com o seguinte código:

    ```html
    @model List<string>
    @{
        ViewBag.Title = "List blobs";
    }
    
    <h2>List blobs</h2>
    
    <ul>
        @foreach (var item in Model)
        {
        <li>@item</li>
        }
    </ul>
    ```

1. No **Explorador de soluções**, expanda o **vistas** > **partilhados** pasta e abra `_Layout.cshtml`.

1. Após a última **Html.ActionLink**, adicione o seguinte **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. Executar a aplicação e selecione **listar blobs** para ver os resultados semelhantes à captura de ecrã seguinte:
  
    ![Captura de ecrã da lista de blobs](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a>Transferir blobs

Esta secção ilustra como transferir um blob. Pode manter este armazenamento local ou ler o conteúdo numa cadeia. As referências do código de exemplo a *contentor de BLOBs de teste* criou na secção, [criar um contentor de blob](#create-a-blob-container).

1. Abra o ficheiro `BlobsController.cs`.

1. Adicione um método denominado `DownloadBlob` que devolve uma cadeia.

    ```csharp
    public string DownloadBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```
 
1. Dentro do `DownloadBlob` método, obter um `CloudBlobContainer` objeto que representa uma referência para o contentor de blob.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. A obtenção de um objeto de referência de blob, chamando o `CloudBlobContainer.GetBlockBlobReference` método. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Para transferir um blob, utilize o `CloudBlockBlob.DownloadToStream` método. O código seguinte transfere conteúdos de um blob para um objeto de fluxo. Em seguida, esse objeto é mantido um ficheiro local. (Alterar  *&lt;nome de ficheiro local >* para o nome de ficheiro completamente qualificado que representa onde o blob está a ser transferido.) 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```
    
    O seguinte mostra o concluída `ListBlobs` método (com um caminho completamente qualificado para o ficheiro local que está a ser criado):
    
    ```csharp
    public string DownloadBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        using (var fileStream = System.IO.File.OpenWrite(@"c:\src\downloadedBlob.txt"))
        {
            blob.DownloadToStream(fileStream);
        }
        return "success!";
    }
    ```

1. No **Explorador de soluções**, expanda o **vistas** > **partilhados** pasta e abra `_Layout.cshtml`.

1. Após a última **Html.ActionLink**, adicione o seguinte **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. Executar a aplicação e selecione **transferir BLOBs** para transferir o blob. O blob especificado no `CloudBlobContainer.GetBlockBlobReference` transferências de chamada de método para a localização especificada no `File.OpenWrite` chamada de método.  O texto *êxito!* deve aparecer no browser. 

## <a name="delete-blobs"></a>Eliminar blobs

Os passos seguintes ilustram como eliminar um blob:

1. Abra o ficheiro `BlobsController.cs`.

1. Adicione um método denominado `DeleteBlob` que devolve uma cadeia.

    ```csharp
    public string DeleteBlob()
    {
        // The code in this section goes here.

        return "success!";
    }
    ```

1. Dentro do `DeleteBlob` método, obter um `CloudBlobContainer` objeto que representa uma referência para o contentor de blob.
   
    ```csharp
    CloudBlobContainer container = GetCloudBlobContainer();
    ```

1. A obtenção de um objeto de referência de blob, chamando o `CloudBlobContainer.GetBlockBlobReference` método. 

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
    ```

1. Para eliminar um blob, utilize o `Delete` método.

    ```csharp
    blob.Delete();
    ```
    
    A conclusão `DeleteBlob` método deverá aparecer da seguinte forma:
    
    ```csharp
    public string DeleteBlob()
    {
        CloudBlobContainer container = GetCloudBlobContainer();
        CloudBlockBlob blob = container.GetBlockBlobReference("myBlob");
        blob.Delete();
        return "success!";
    }
    ```

1. No **Explorador de soluções**, expanda o **vistas** > **partilhados** pasta e abra `_Layout.cshtml`.

1. Após a última **Html.ActionLink**, adicione o seguinte **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. Executar a aplicação e selecione **eliminar BLOBs** para eliminar o blob especificado no `CloudBlobContainer.GetBlockBlobReference` chamada de método. O texto *êxito!* deve aparecer no browser. Selecione o browser **novamente** botão e, em seguida, selecione **listar blobs** para verificar se o blob já não está a ser o contentor.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu como armazenar, lista e obter os blobs no armazenamento do Azure utilizando o ASP.NET. Ver mais guias de funcionalidades para saber mais sobre as opções adicionais para armazenar dados no Azure.

  * [Introdução ao Azure Table storage e o Visual Studio ligado serviços (ASP.NET)](vs-storage-aspnet-getting-started-tables.md)
  * [Começar a utilizar filas do Azure Visual Studio e de armazenamento ligado serviços (ASP.NET)](vs-storage-aspnet-getting-started-queues.md)
