---
title: "Transferir grandes quantidades de dados aleatórios do armazenamento do Azure | Microsoft Docs"
description: "Saiba como utilizar o Azure SDK para transferir grandes quantidades de dados aleatórios a partir de uma conta de armazenamento do Azure"
services: storage
documentationcenter: 
author: georgewallace
manager: jeconnoc
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 12/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 3842860acb1c0fdd9e07f6d2f678ac5d5304003b
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="download-large-amounts-of-random-data-from-azure-storage"></a>Transferir grandes quantidades de dados aleatórios a partir do armazenamento do Azure

Este tutorial é parte três de uma série. Este tutorial mostra como transferir grandes quantidades de dados do armazenamento do Azure.

Na parte três da série, saiba como:

> [!div class="checklist"]
> * Atualizar a aplicação
> * Executar a aplicação
> * Validar o número de ligações

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, tem de ter concluído o tutorial de armazenamento anterior: [carregar grandes quantidades de dados aleatórios em paralelo com armazenamento do Azure][previous-tutorial].

## <a name="remote-into-your-virtual-machine"></a>Remoto na sua máquina virtual

 Para criar uma sessão de ambiente de trabalho remoto com a máquina virtual, utilize o seguinte comando na sua máquina local. Substitua o endereço IP publicIPAddress da sua máquina virtual. Quando lhe for pedido, introduza as credenciais utilizadas ao criar a máquina virtual.

```
mstsc /v:<publicIpAddress>
```

## <a name="update-the-application"></a>Atualizar a aplicação

O tutorial anterior, carregado apenas ficheiros à conta de armazenamento. Abra `D:\git\storage-dotnet-perf-scale-app\Program.cs` num editor de texto. Substitua o `Main` método com o exemplo seguinte. Comentários este exemplo fora da tarefa de carregamento e uncomments a tarefa de transferência e a tarefa para eliminar o conteúdo na conta de armazenamento, quando terminar.

```csharp
public static void Main(string[] args)
{
    Console.WriteLine("Azure Blob storage performance and scalability sample");
    // Set threading and default connection limit to 100 to ensure multiple threads and connections can be opened.
    // This is in addition to parallelism with the storage client library that is defined in the functions below.
    ThreadPool.SetMinThreads(100, 4);
    ServicePointManager.DefaultConnectionLimit = 100; // (Or More)

    bool exception = false;
    try
    {
        // Call the UploadFilesAsync function.
        UploadFilesAsync().GetAwaiter().GetResult();

        // Uncomment the following line to enable downloading of files from the storage account.  This is commented out
        // initially to support the tutorial at https://docs.microsoft.com/azure/storage/blobs/storage-blob-scaleable-app-download-files.
        // DownloadFilesAsync().GetAwaiter().GetResult();
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
        exception = true;
    }
    finally
    {
        // The following function will delete the container and all files contained in them.  This is commented out initialy
        // As the tutorial at https://docs.microsoft.com/azure/storage/blobs/storage-blob-scaleable-app-download-files has you upload only for one tutorial and download for the other. 
        if (!exception)
        {
            // DeleteExistingContainersAsync().GetAwaiter().GetResult();
        }
        Console.WriteLine("Press any key to exit the application");
        Console.ReadKey();
    }
}
```

Depois da aplicação foi atualizada, terá de criar a aplicação novamente. Abra um `Command Prompt` e navegue para `D:\git\storage-dotnet-perf-scale-app`. Reconstruir a aplicação executando `dotnet build` como mostrado no exemplo seguinte:

```
dotnet build
```

## <a name="run-the-application"></a>Executar a aplicação

Agora que a aplicação foi reconstruída está na altura de executar a aplicação com o código atualizado. Se não já aberto, abra uma `Command Prompt` e navegue para `D:\git\storage-dotnet-perf-scale-app`.

Tipo `dotnet run` para executar a aplicação.

```
dotnet run
```

A aplicação lê os contentores localizados na conta de armazenamento especificada no **storageconnectionstring**. -Itera através de blobs 10 a um tempo utilizando o [ListBlobsSegmented](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobssegmented?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_ListBlobsSegmented_System_String_System_Boolean_Microsoft_WindowsAzure_Storage_Blob_BlobListingDetails_System_Nullable_System_Int32__Microsoft_WindowsAzure_Storage_Blob_BlobContinuationToken_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) método nos contentores e transfere-os para o local do computador utilizando o [DownloadToFileAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadtofileasync?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlob_DownloadToFileAsync_System_String_System_IO_FileMode_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) método.
A tabela seguinte mostra o [BlobRequestOptions](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions?view=azure-dotnet) que estão definidos para cada blob como é transferido.

|Propriedade|Valor|Descrição|
|---|---|---|
|[DisableContentMD5Validation](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.disablecontentmd5validation?view=azure-dotnet)| true| Esta propriedade desativa a verificar o hash MD5 o conteúdo carregado. Desativar validação MD5 produz uma transferência mais rápida. Mas não confirma a integridade dos ficheiros a serem transferidos ou validade. |
|[StorBlobContentMD5](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.storeblobcontentmd5?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_StoreBlobContentMD5)| falso| Esta propriedade determina se um hash MD5 é calculado e armazenado.   |

O `DownloadFilesAsync` tarefa é mostrada no exemplo seguinte:

```csharp
private static async Task DownloadFilesAsync()
{
    CloudBlobClient blobClient = GetCloudBlobClient();

    // Define the BlobRequestionOptions on the download, including disabling MD5 hash validation for this example, this improves the download speed.
    BlobRequestOptions options = new BlobRequestOptions
    {
        DisableContentMD5Validation = true,
        StoreBlobContentMD5 = false
    };

    // Retrieve the list of containers in the storage account.  Create a directory and configure variables for use later.
    BlobContinuationToken continuationToken = null;
    List<CloudBlobContainer> containers = new List<CloudBlobContainer>();
    do
    {
        var listingResult = await blobClient.ListContainersSegmentedAsync(continuationToken);
        continuationToken = listingResult.ContinuationToken;
        containers.AddRange(listingResult.Results);
    }
    while (continuationToken != null);

    var directory = Directory.CreateDirectory("download");
    BlobResultSegment resultSegment = null;
    Stopwatch time = Stopwatch.StartNew();

    // Download the blobs
    try
    {
        List<Task> tasks = new List<Task>();
        int max_outstanding = 100;
        int completed_count = 0;

        // Create a new instance of the SemaphoreSlim class to define the number of threads to use in the application.
        SemaphoreSlim sem = new SemaphoreSlim(max_outstanding, max_outstanding);

        // Iterate through the containers
        foreach (CloudBlobContainer container in containers)
        {
            do
            {
                // Return the blobs from the container lazily 10 at a time.
                resultSegment = await container.ListBlobsSegmentedAsync(null, true, BlobListingDetails.All, 10, continuationToken, null, null);
                continuationToken = resultSegment.ContinuationToken;
                {
                    foreach (var blobItem in resultSegment.Results)
                    {

                        if (((CloudBlob)blobItem).Properties.BlobType == BlobType.BlockBlob)
                        {
                            // Get the blob and add a task to download the blob asynchronously from the storage account.
                            CloudBlockBlob blockBlob = container.GetBlockBlobReference(((CloudBlockBlob)blobItem).Name);
                            Console.WriteLine("Downloading {0} from container {1}", blockBlob.Name, container.Name);
                            await sem.WaitAsync();
                            tasks.Add(blockBlob.DownloadToFileAsync(directory.FullName + "\\" + blockBlob.Name, FileMode.Create, null, options, null).ContinueWith((t) =>
                            {
                                sem.Release();
                                Interlocked.Increment(ref completed_count);
                            }));

                        }
                    }
                }
            }
            while (continuationToken != null);
        }

        // Creates an asynchronous task that completes when all the downloads complete.
        await Task.WhenAll(tasks);
    }
    catch (Exception e)
    {
        Console.WriteLine("\nError encountered during transfer: {0}", e.Message);
    }

    time.Stop();
    Console.WriteLine("Download has been completed in {0} seconds. Press any key to continue", time.Elapsed.TotalSeconds.ToString());
    Console.ReadLine();
}
```

### <a name="validate-the-connections"></a>Validar as ligações

Enquanto os ficheiros estão a ser transferidos, pode verificar o número de ligações em simultâneo à sua conta de armazenamento. Abra um `Command Prompt` e tipo `netstat -a | find /c "blob:https"`. Este comando apresenta o número de ligações que estão atualmente abertos com `netstat`. O exemplo seguinte mostra um resultado semelhante para ver quando executar o tutorial por si. Como pode ver do exemplo, ao longo 280 ligações tiverem sido abertas quando transferir os ficheiros aleatórios da conta do storage.

```
C:\>netstat -a | find /c "blob:https"
289

C:\>
```

## <a name="next-steps"></a>Passos Seguintes

Parte três da série, aprendeu sobre transferir grandes quantidades de dados aleatórios a partir de uma conta de armazenamento, tais como:

> [!div class="checklist"]
> * Executar a aplicação
> * Validar o número de ligações

Avançar para a parte quatro da série para verificar o débito e latência métricas no portal.

> [!div class="nextstepaction"]
> [Certifique-se de débito e latência métricas no portal](storage-blob-scalable-app-verify-metrics.md)

[previous-tutorial]: storage-blob-scalable-app-upload-files.md