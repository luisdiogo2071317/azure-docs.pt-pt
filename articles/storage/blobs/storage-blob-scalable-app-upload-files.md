---
title: "Carregar grandes quantidades de dados aleatórios em paralelo com armazenamento do Azure | Microsoft Docs"
description: "Saiba como utilizar o Azure SDK para carregar grandes quantidades de dados aleatórios em paralelo com uma conta de armazenamento do Azure"
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
ms.openlocfilehash: 98f3f69c6025d61caac20e13b573651854952432
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/23/2017
---
# <a name="upload-large-amounts-of-random-data-in-parallel-to-azure-storage"></a>Carregar grandes quantidades de dados aleatórios em paralelo com armazenamento do Azure

Este tutorial é parte dois de uma série. Este tutorial mostra que implementar uma aplicação que carrega grande quantidade de dados aleatórios para uma conta de armazenamento do Azure.

Na parte dois da série, saiba como:

> [!div class="checklist"]
> * Configurar a cadeia de ligação
> * Criar a aplicação
> * Executar a aplicação
> * Validar o número de ligações

Armazenamento de Blobs do Azure fornece um serviço dimensionável para armazenar os dados. Para garantir a sua aplicação como performant quanto possível, uma compreensão sobre como funciona o blob storage é recomendada. Dados de conhecimento de limites para os blobs do Azure são importante, para obter mais informações sobre estes limites visitam: [metas de escalabilidade do armazenamento de BLOBs](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#azure-blob-storage-scale-targets).

[Atribuição de nomes de partição](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#subheading47) é outro fator importante ao conceber uma aplicação altamente realizar com blobs. Armazenamento do Azure utiliza um esquema de partições com base no intervalo para balanceamento de escala e a carga. Esta configuração significa que os ficheiros com as convenções de nomenclatura semelhantes ou prefixos Ir para a mesma partição. Esta lógica inclui o nome do contentor que os ficheiros estão a ser carregados para. Neste tutorial, utilize os ficheiros que tenham GUIDs para nomes como conteúdo gerado bem como aleatoriamente. Em seguida, são carregados cinco diferentes contentores com nomes aleatórios.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, tem de ter concluído o tutorial de armazenamento anterior: [criar uma máquina virtual e a conta de armazenamento para uma aplicação dimensionável][previous-tutorial].

## <a name="remote-into-your-virtual-machine"></a>Remoto na sua máquina virtual

Utilize o seguinte comando na sua máquina local para criar uma sessão de ambiente de trabalho remoto com a máquina virtual. Substitua o endereço IP publicIPAddress da sua máquina virtual. Quando lhe for pedido, introduza as credenciais utilizadas ao criar a máquina virtual.

```
mstsc /v:<publicIpAddress>
```

## <a name="configure-the-connection-string"></a>Configurar a cadeia de ligação

No portal do Azure, navegue até à sua conta de armazenamento. Selecione **chaves de acesso** em **definições** na sua conta de armazenamento. Copiar o **cadeia de ligação** partir da chave primária ou secundária. Inicie sessão máquina virtual que criou no tutorial anterior. Abra um **linha de comandos** como um administrador e execute o `setx` comando com o `/m` comutador, este comando guarda uma variável de ambiente de definição de máquina. A variável de ambiente não está disponível até que volte a carregar o **linha de comandos**. Substitua ** \<storageConnectionString\> ** no seguinte exemplo:

```
setx storageconnectionstring "<storageConnectionString>" /m
```

Quando terminar, abra o outro **linha de comandos**, navegue para `D:\git\storage-dotnet-perf-scale-app` e tipo `dotnet build` para criar a aplicação.

## <a name="run-the-application"></a>Executar a aplicação

Navegue para `D:\git\storage-dotnet-perf-scale-app`.

Tipo `dotnet run` para executar a aplicação. Da primeira vez que executa `dotnet` preenche a cache do pacote local, para melhorar a velocidade de restauro e ativar o acesso offline. Este comando ocupa a um minuto para concluir e só ocorre uma vez.

```
dotnet run
```

A aplicação cria cinco contentores aleatoriamente nomeados e começa a carregar os ficheiros no diretório de transição para a conta de armazenamento. A aplicação define os threads mínimos para 100 e o [DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(v=vs.110).aspx) a 100 para garantir que é permitido um grande número de ligações simultâneas quando executar a aplicação.

Além de definir as definições de limite de thread e a ligação, o [BlobRequestOptions](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions?view=azure-dotnet) para o [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet) método estão configurados para utilizar o paralelismo e desativar a validação de hash MD5. Os ficheiros são carregados em blocos de 100 mb, esta configuração fornece um melhor desempenho, mas pode ser dispendiosa se utilizar uma execução poorly rede como se houver uma falha do bloco de 100 mb completo é repetida.

|Propriedade|Valor|Descrição|
|---|---|---|
|[ParallelOperationThreadCount](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.paralleloperationthreadcount?view=azure-dotnet)| 8| A definição de quebras de blob em blocos ao carregar. Para mais elevado desempenho, este valor deve ser 8 vezes o número de núcleos. |
|[DisableContentMD5Validation](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.disablecontentmd5validation?view=azure-dotnet)| true| Esta propriedade desativa a verificar o hash MD5 o conteúdo carregado. Desativar validação MD5 produz uma transferência mais rápida. Mas não confirma a integridade dos ficheiros a serem transferidos ou validade.   |
|[StorBlobContentMD5](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.storeblobcontentmd5?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_StoreBlobContentMD5)| falso| Esta propriedade determina se um hash MD5 é calculado e armazenado com o ficheiro.   |
| [RetryPolicy](/dotnet/api/microsoft.windowsazure.storage.blob.blobrequestoptions.retrypolicy?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_RetryPolicy)| segundo 2 término com a repetição de máximo de 10 |Determina a política de repetição de pedidos. Falhas de ligação são repetidas, neste exemplo um [ExponentialRetry](/dotnet/api/microsoft.windowsazure.storage.retrypolicies.exponentialretry?view=azure-dotnet) política está configurada com um término de segundo 2 e uma contagem de repetições máxima de 10. Esta definição é importante quando a aplicação obtém próximo atingir o [metas de escalabilidade do armazenamento de BLOBs](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#azure-blob-storage-scale-targets).  |

O `UploadFilesAsync` tarefa é mostrada no exemplo seguinte:

```csharp
private static async Task UploadFilesAsync()
{
    // Create random 5 characters containers to upload files to.
    CloudBlobContainer[] containers = await GetRandomContainersAsync();
    var currentdir = System.IO.Directory.GetCurrentDirectory();

    // path to the directory to upload
    string uploadPath = currentdir + "\\upload";
    Stopwatch time = Stopwatch.StartNew();
    try
    {
        Console.WriteLine("Iterating in directory: {0}", uploadPath);
        int count = 0;
        int max_outstanding = 100;
        int completed_count = 0;

        // Define the BlobRequestionOptions on the upload.
        // This includes defining an exponential retry policy to ensure that failed connections are retried with a backoff policy. As multiple large files are being uploaded
        // large block sizes this can cause an issue if an exponential retry policy is not defined.  Additionally parallel operations are enabled with a thread count of 8
        // This could be should be multiple of the number of cores that the machine has. Lastly MD5 hash validation is disabled for this example, this improves the upload speed.
        BlobRequestOptions options = new BlobRequestOptions
        {
            ParallelOperationThreadCount = 8,
            DisableContentMD5Validation = true,
            StoreBlobContentMD5 = false
        };
        // Create a new instance of the SemaphoreSlim class to define the number of threads to use in the application.
        SemaphoreSlim sem = new SemaphoreSlim(max_outstanding, max_outstanding);

        List<Task> tasks = new List<Task>();
        Console.WriteLine("Found {0} file(s)", Directory.GetFiles(uploadPath).Count());

        // Iterate through the files
        foreach (string path in Directory.GetFiles(uploadPath))
        {
            // Create random file names and set the block size that is used for the upload.
            var container = containers[count % 5];
            string fileName = Path.GetFileName(path);
            Console.WriteLine("Uploading {0} to container {1}.", path, container.Name);
            CloudBlockBlob blockBlob = container.GetBlockBlobReference(fileName);

            // Set block size to 100MB.
            blockBlob.StreamWriteSizeInBytes = 100 * 1024 * 1024;
            await sem.WaitAsync();

            // Create tasks for each file that is uploaded. This is added to a collection that executes them all asyncronously.  
            tasks.Add(blockBlob.UploadFromFileAsync(path, null, options, null).ContinueWith((t) =>
            {
                sem.Release();
                Interlocked.Increment(ref completed_count);
            }));
            count++;
        }

        // Creates an asynchronous task that completes when all the uploads complete.
        await Task.WhenAll(tasks);

        time.Stop();

        Console.WriteLine("Upload has been completed in {0} seconds. Press any key to continue", time.Elapsed.TotalSeconds.ToString());

        Console.ReadLine();
    }
    catch (DirectoryNotFoundException ex)
    {
        Console.WriteLine("Error parsing files in the directory: {0}", ex.Message);
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.Message);
    }
}
```

O exemplo seguinte é uma saída da aplicação truncada em execução num sistema Windows.

```
Created container https://mystorageaccount.blob.core.windows.net/9efa7ecb-2b24-49ff-8e5b-1d25e5481076
Created container https://mystorageaccount.blob.core.windows.net/bbe5f0c8-be9e-4fc3-bcbd-2092433dbf6b
Created container https://mystorageaccount.blob.core.windows.net/9ac2f71c-6b44-40e7-b7be-8519d3ba4e8f
Created container https://mystorageaccount.blob.core.windows.net/47646f1a-c498-40cd-9dae-840f46072180
Created container https://mystorageaccount.blob.core.windows.net/38b2cdab-45fa-4cf9-94e7-d533837365aa
Iterating in directory: D:\git\storage-dotnet-perf-scale-app\upload
Found 50 file(s)
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\1d596d16-f6de-4c4c-8058-50ebd8141e4d.txt to container 9efa7ecb-2b24-49ff-8e5b-1d25e5481076.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\242ff392-78be-41fb-b9d4-aee8152a6279.txt to container bbe5f0c8-be9e-4fc3-bcbd-2092433dbf6b.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\38d4d7e2-acb4-4efc-ba39-f9611d0d55ef.txt to container 9ac2f71c-6b44-40e7-b7be-8519d3ba4e8f.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\45930d63-b0d0-425f-a766-cda27ff00d32.txt to container 47646f1a-c498-40cd-9dae-840f46072180.
Starting upload of D:\git\storage-dotnet-perf-scale-app\upload\5129b385-5781-43be-8bac-e2fbb7d2bd82.txt to container 38b2cdab-45fa-4cf9-94e7-d533837365aa.
...
Upload has been completed in 142.0429536 seconds. Press any key to continue
```

### <a name="validate-the-connections"></a>Validar as ligações

Enquanto os ficheiros estão a ser carregados, pode verificar o número de ligações em simultâneo à sua conta de armazenamento. Abra um **linha de comandos** e tipo `netstat -a | find /c "blob:https"`. Este comando apresenta o número de ligações que estão atualmente abertos com `netstat`. O exemplo seguinte mostra um resultado semelhante para ver quando executar o tutorial por si. Como pode ver do exemplo, 800 ligações tiverem sido abertas quando carregar os ficheiros aleatórios para a conta de armazenamento. Este valor é alterado em toda a executar o carregamento. Através do carregamento em segmentos de bloco parallel, a quantidade de tempo necessário para transferir o conteúdo é significativamente reduzida.

```
C:\>netstat -a | find /c "blob:https"
800

C:\>
```

## <a name="next-steps"></a>Passos Seguintes

Parte dois da série, aprendeu sobre carregar grandes quantidades de dados aleatórios para uma conta de armazenamento em paralelo, tais como:

> [!div class="checklist"]
> * Configurar a cadeia de ligação
> * Criar a aplicação
> * Executar a aplicação
> * Validar o número de ligações

Avançar para três parte a série da transferir grandes quantidades de dados a partir de uma conta de armazenamento.

> [!div class="nextstepaction"]
> [Carregar grandes quantidades de ficheiros grandes em paralelo com uma conta de armazenamento](storage-blob-scalable-app-download-files.md)

[previous-tutorial]: storage-blob-scalable-app-create-vm.md
