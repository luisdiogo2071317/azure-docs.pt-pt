---
title: Moderator conteúdo do Azure - moderação de interrupção gráfica | Microsoft Docs
description: Utilize a moderação de interrupção gráfica para procurar o conteúdo para adultos e racy possíveis.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 02/02/2018
ms.author: sajagtap
ms.openlocfilehash: ef58f5990d4a0a19ab2b8c61b42ab2a0754dc6fa
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355099"
---
# <a name="video-moderation"></a>Moderação de vídeos

Hoje em dia, visualizadores online geram billions das vistas de vídeos através de redes sociais populares e mais regional web sites e aumentar. Através da aplicação com base dos serviços de aprendizagem automática para prever a conteúdo para adultos e racy potencial, reduzir o custo dos seus esforços de moderação interrupção.

## <a name="sign-up-for-the-content-moderator-media-processor-public-preview"></a>Inscreva-se o processador de suporte de dados de conteúdo Moderator (pré-visualização pública)

### <a name="create-a-free-azure-account"></a>Crie uma conta do Azure gratuita

[Comece por aqui](https://azure.microsoft.com/free/) para criar uma conta gratuita do Azure se ainda não tiver um.

### <a name="create-an-azure-media-services-account"></a>Criar uma conta dos Azure Media Services

Capacidade de vídeo a Moderator conteúdo está disponível como uma pré-visualização pública **processador de multimédia** dos Media Services no Azure (AMS), sem encargos.

[Criar uma conta de Media Services do Azure](https://docs.microsoft.com/azure/media-services/media-services-portal-create-account) na sua subscrição do Azure.

### <a name="get-azure-active-directory-credentials"></a>Obter as credenciais do Azure Active Directory

   1. Leia o [artigo portal Media Services do Azure](https://docs.microsoft.com/azure/media-services/media-services-portal-get-started-with-aad) para aprender a utilizar o portal do Azure para obter as credenciais de autenticação do Azure AD.
   1. Leia o [artigo do .NET dos Media Services do Azure](https://docs.microsoft.com/azure/media-services/media-services-dotnet-get-started-with-aad) para saber como utilizar as credenciais do Azure Active Directory com o SDK .NET.

   > [!NOTE]
   > O código de exemplo neste guia de introdução utiliza a **autenticação principal de serviço** método descrito em ambos os artigos.

Depois de obter as credenciais do AMS, existem duas formas para experimentar o processador de multimédia Moderator conteúdo.

## <a name="use-azure-media-services-explorer"></a>Utilize o Explorador de serviços de multimédia do Azure

Utilize o interativa [Explorador de serviços de suporte de dados do Azure (AMS)](https://azure.microsoft.com/blog/managing-media-workflows-with-the-new-azure-media-services-explorer-tool/) para procurar a sua conta de AMS, carregar vídeos e analisar com o processador de multimédia Moderator conteúdo. [Transfira e instale-](https://github.com/Azure/Azure-Media-Services-Explorer/releases) a partir do GitHub, e [procurar o código de origem](http://github.com/Azure/Azure-Media-Services-Explorer) para aprofundar utilizando o SDK do AMS.

![Explorador de Media Services do Azure com Moderator conteúdo](images/ams-explorer-content-moderator.PNG)

## <a name="net-quickstart-with-visual-studio-and-c"></a>Guia de introdução do .NET com o Visual Studio e c#

1. Adicione um novo **aplicação de consola (.NET Framework)** projeto para a sua solução.

   No código de exemplo, nome do projeto **VideoModeration**.

1. Selecione este projeto como o projeto de arranque único para a solução.

### <a name="install-required-packages"></a>Instalar pacotes necessários

Instalar os seguintes pacotes de NuGet disponíveis no [NuGet](https://www.nuget.org/).

- windowsazure.mediaservices
- windowsazure.mediaservices.Extensions

### <a name="update-the-programs-using-statements"></a>O programa de atualização do instruções de utilização

Modificar o programa de instruções de utilização.

    using System;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.IO;
    using System.Threading;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using System.Collections.Generic;


### <a name="initialize-application-specific-settings"></a>Inicializar definições específicas da aplicação

Adicione os seguintes campos estáticos para o **programa** a classe Program. cs.

    // declare constants and globals
    private static CloudMediaContext _context = null;
    private static CloudStorageAccount _StorageAccount = null;

    // Azure Media Services (AMS) associated Storage Account, Key, and the Container that has 
    // a list of Blobs to be processed.
    static string STORAGE_NAME = "YOUR AMS ASSOCIATED BLOB STORAGE NAME";
    static string STORAGE_KEY = "YOUR AMS ASSOCIATED BLOB STORAGE KEY";
    static string STORAGE_CONTAINER_NAME = "YOUR BLOB CONTAINER FOR VIDEO FILES";

    private static StorageCredentials _StorageCredentials = null;

    // Azure Media Services authentication. See the quickstart for how to get these.
    private const string AZURE_AD_TENANT_NAME = "microsoft.onmicrosoft.com";
    private const string CLIENT_ID = "YOUR CLIENT ID"
    private const string CLIENT_SECRET = "YOUR CLIENT SECRET";

    // REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".      
    private const string REST_API_ENDPOINT = "YOUR API ENDPOINT";

    // Content Moderator Media Processor Nam
    private const string MEDIA_PROCESSOR = "Azure Media Content Moderator";

    // Input and Output files in the current directory of the executable
    private const string INPUT_FILE = "VIDEO FILE NAME";
    private const string OUTPUT_FOLDER = "";

### <a name="create-a-preset-file-json"></a>Criar um ficheiro predefinido (json)

Crie um ficheiro JSON no diretório atual com o número de versão.

    private static readonly string CONTENT_MODERATOR_PRESET_FILE = "preset.json";
    //Example file content:
    //        {
    //             "version": "2.0"
    //        }
    private static readonly string CONTENT_MODERATOR_PRESET_FILE = "preset.json";

### <a name="add-the-following-code-to-the-main-method"></a>Adicione o seguinte código ao método principal

O método principal cria pela primeira vez um contexto de suporte de dados do Azure e, em seguida, um contexto de armazenamento do Azure no caso dos vídeos se encontrem no blob storage.
O código restantes analisa um vídeo a partir de uma pasta local, blob ou blobs vários dentro de um contentor de armazenamento do Azure.
Pode experimentar todas as opções ao comentar terminar as outras linhas de código.

    // Create Azure Media Context
    CreateMediaContext();

    // Create Storage Context
    CreateStorageContext();

    // Use a file as the input.
    // IAsset asset = CreateAssetfromFile();
    
    // -- OR ---
    
    // Or a blob as the input
    // IAsset asset = CreateAssetfromBlob((CloudBlockBlob)GetBlobsList().First());

    // Then submit the asset to Content Moderator
    // RunContentModeratorJob(asset);

    //-- OR ----

    // Just run the content moderator on all blobs in a list (from a Blob Container)
    RunContentModeratorJobOnBlobs();

### <a name="add-the-code-to-create-an-azure-media-context"></a>Adicione o código para criar um contexto de suporte de dados do Azure

    /// <summary>
    /// Creates a media context from azure credentials
    /// </summary>
    static void CreateMediaContext()
    {
        // Get Azure AD credentials
        var tokenCredentials = new AzureAdTokenCredentials(AZURE_AD_TENANT_NAME,
            new AzureAdClientSymmetricKey(CLIENT_ID, CLIENT_SECRET),
            AzureEnvironments.AzureCloudEnvironment);

        // Initialize an Azure AD token
        var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

        // Create a media context
        _context = new CloudMediaContext(new Uri(REST_API_ENDPOINT), tokenProvider);
    }

### <a name="add-the-code-to-create-an-azure-storage-context"></a>Adicione o código para criar um contexto de armazenamento do Azure
Utilize o contexto de armazenamento criado das suas credenciais de armazenamento, para aceder ao seu armazenamento de Blobs.

    /// <summary>
    /// Creates a storage context from the AMS associated storage name and key
    /// </summary>
    static void CreateStorageContext()
    {
        // Get a reference to the storage account associated with a Media Services account. 
        if (_StorageCredentials == null)
        {
            _StorageCredentials = new StorageCredentials(STORAGE_NAME, STORAGE_KEY);
        }
        _StorageAccount = new CloudStorageAccount(_StorageCredentials, false);
    }

### <a name="add-the-code-to-create-azure-media-assets-from-local-file-and-blob"></a>Adicione o código para criar recursos de suporte de dados do Azure a partir do ficheiro local e BLOBs
O processador de suporte de dados de conteúdo Moderator executa tarefas **ativos** dentro da plataforma de Media Services do Azure.
Estes métodos de criar os recursos a partir de um ficheiro local ou um blob associado.

    /// <summary>
    /// Creates an Azure Media Services Asset from the video file
    /// </summary>
    /// <returns>Asset</returns>
    static IAsset CreateAssetfromFile()
    {
        return _context.Assets.CreateFromFile(INPUT_FILE, AssetCreationOptions.None); ;
    }

    /// <summary>
    /// Creates an Azure Media Services asset from your blog storage
    /// </summary>
    /// <param name="Blob"></param>
    /// <returns>Asset</returns>
    static IAsset CreateAssetfromBlob(CloudBlockBlob Blob)
    {
        // Create asset from the FIRST blob in the list and return it
        return _context.Assets.CreateFromBlob(Blob, _StorageCredentials, AssetCreationOptions.None);
    }

### <a name="add-the-code-to-scan-a-collection-of-videos-as-blobs-within-a-container"></a>Adicione o código para uma coleção de vídeos (como blobs) dentro de um contentor de análise

    /// <summary>
    /// Runs the Content Moderator Job on all Blobs in a given container name
    /// </summary>
    static void RunContentModeratorJobOnBlobs()
    {
        // Get the reference to the list of Blobs. See the following method.
        var blobList = GetBlobsList();

        // Iterate over the Blob list items or work on specific ones as needed
        foreach (var sourceBlob in blobList)
        {
            // Create an Asset
            IAsset asset = _context.Assets.CreateFromBlob((CloudBlockBlob)sourceBlob,
                               _StorageCredentials, AssetCreationOptions.None);
            asset.Update();

            // Submit to Content Moderator
            RunContentModeratorJob(asset);
        }
    }

    /// <summary>
    /// Get all blobs in your container
    /// </summary>
    /// <returns></returns>
    static IEnumerable<IListBlobItem> GetBlobsList()
    {
        // Get a reference to the Container within the Storage Account
        // that has the files (blobs) for moderation
        CloudBlobClient CloudBlobClient = _StorageAccount.CreateCloudBlobClient();
        CloudBlobContainer MediaBlobContainer = CloudBlobClient.GetContainerReference(STORAGE_CONTAINER_NAME);

        // Get the reference to the list of Blobs 
        var blobList = MediaBlobContainer.ListBlobs();
        return blobList;
    }

### <a name="add-the-method-to-run-the-content-moderator-job"></a>Adicione o método para executar a tarefa de Moderator conteúdo

    /// <summary>
    /// Run the Content Moderator job on the designated Asset from local file or blob storage
    /// </summary>
    /// <param name="asset"></param>
    static void RunContentModeratorJob(IAsset asset)
    {
        // Grab the presets
        string configuration = File.ReadAllText(CONTENT_MODERATOR_PRESET_FILE);

        // grab instance of Azure Media Content Moderator MP
        IMediaProcessor mp = _context.MediaProcessors.GetLatestMediaProcessorByName(MEDIA_PROCESSOR);

        // create Job with Content Moderator task
        IJob job = _context.Jobs.Create(String.Format("Content Moderator {0}",
                asset.AssetFiles.First() + "_" + Guid.NewGuid()));

        ITask contentModeratorTask = job.Tasks.AddNew("Adult and racy classifier task",
                mp, configuration,
                TaskOptions.None);
        contentModeratorTask.InputAssets.Add(asset);
        contentModeratorTask.OutputAssets.AddNew("Adult and racy classifier output",
            AssetCreationOptions.None);

        job.Submit();


        // Create progress printing and querying tasks
        Task progressPrintTask = new Task(() =>
        {
            IJob jobQuery = null;
            do
            {
                var progressContext = _context;
                jobQuery = progressContext.Jobs
                .Where(j => j.Id == job.Id)
                    .First();
                    Console.WriteLine(string.Format("{0}\t{1}",
                    DateTime.Now,
                    jobQuery.State));
                    Thread.Sleep(10000);
             }
             while (jobQuery.State != JobState.Finished &&
             jobQuery.State != JobState.Error &&
             jobQuery.State != JobState.Canceled);
        });
        progressPrintTask.Start();

        Task progressJobTask = job.GetExecutionProgressTask(
        CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling 
        // method for job progress should log errors.  Here we check 
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            ErrorDetail error = job.Tasks.First().ErrorDetails.First();
            Console.WriteLine(string.Format("Error: {0}. {1}",
            error.Code,
            error.Message));
        }

        DownloadAsset(job.OutputMediaAssets.First(), OUTPUT_FOLDER);
    }

### <a name="add-a-couple-of-helper-functions"></a>Adicione algumas funções de programa auxiliar

Estes métodos de transferir o ficheiro de saída do conteúdo Moderator (JSON) do recurso de Media Services do Azure e ajudam a controlar o estado da tarefa de moderação interrupção para que o programa pode iniciar um Estado de execução para a consola.

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }

    // event handler for Job State
    static void StateChanged(object sender, JobStateChangedEventArgs e)
    {
        Console.WriteLine("Job state changed event:");
        Console.WriteLine("  Previous state: " + e.PreviousState);
        Console.WriteLine("  Current state: " + e.CurrentState);
        switch (e.CurrentState)
        {
            case JobState.Finished:
                Console.WriteLine();
                Console.WriteLine("Job finished.");
                break;
            case JobState.Canceling:
            case JobState.Queued:
            case JobState.Scheduled:
            case JobState.Processing:
                Console.WriteLine("Please wait...\n");
                break;
            case JobState.Canceled:
                Console.WriteLine("Job is canceled.\n");
                break;
            case JobState.Error:
                Console.WriteLine("Job failed.\n");
                break;
            default:
                break;
        }
    }

### <a name="run-the-program-and-review-the-output"></a>Execute o programa e reveja o resultado

Uma vez concluída a tarefa de conteúdo moderação de interrupção, analise a resposta JSON. É composto por estes elementos:

- Vídeos informações de resumidas
- **Capturas de** como "**fragmentos**"
- **Chave frames** como "**eventos**" com um **reviewRecommended "(= true ou false)"** sinalizador com base no **para adultos** e **Racy** pontuações
- **Iniciar**, **duração**, **totalDuration**, e **timestamp** estão em "ticks". Dividir por **escala temporal** para obter o número de segundos.
 
> [!NOTE]

> - `adultScore` representa a classificação de presença e predição potencial de conteúdo que pode ser considerada sexually explícita ou para adultos em determinadas situações.
> - `racyScore` representa a classificação de presença e predição potencial de conteúdo que pode ser considerada sexually suggestive ou madura em determinadas situações.
> - `adultScore` e `racyScore` encontram-se entre 0 e 1. Quanto maior for a classificação, quanto maior for o modelo é a previsão que a categoria pode ser aplicável. Esta pré-visualização depende de um modelo de análises, em vez de resultados manualmente codificados. Recomendamos que teste com o seus próprios conteúdo para determinar a forma como cada categoria alinha os seus requisitos.
> - `reviewRecommended` é true ou false, dependendo do modelo de pontuação interno limiares. Os clientes devem avaliar se pretende utilizar este valor ou opte por utilizar limiares personalizados com base nas políticas os respetivos conteúdas.
>

    {
    "version": 2,
    "timescale": 90000,
    "offset": 0,
    "framerate": 50,
    "width": 1280,
    "height": 720,
    "totalDuration": 18696321,
    "fragments": [
    {
      "start": 0,
      "duration": 18000
    },
    {
      "start": 18000,
      "duration": 3600,
      "interval": 3600,
      "events": [
        [
          {
            "reviewRecommended": false,
            "adultScore": 0.00001,
            "racyScore": 0.03077,
            "index": 5,
            "timestamp": 18000,
            "shotIndex": 0
          }
        ]
      ]
    },
    {
      "start": 18386372,
      "duration": 119149,
      "interval": 119149,
      "events": [
        [
          {
            "reviewRecommended": true,
            "adultScore": 0.00000,
            "racyScore": 0.91902,
            "index": 5085,
            "timestamp": 18386372,
            "shotIndex": 62
          }
        ]
      ]
    }
    ]
    }

## <a name="next-steps"></a>Passos Seguintes

Saiba como gerar [vídeo revê](video-reviews-quickstart-dotnet.md) da sua saída moderação interrupção.

Adicionar [moderação de interrupção transcript](video-transcript-moderation-review-tutorial-dotnet.md) para as vídeos revisões.

Veja o tutorial detalhado sobre como construir um [concluir as vídeo e transcript solução moderação interrupção](video-transcript-moderation-review-tutorial-dotnet.md).

[Transferir a solução do Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para esta e outros inícios rápidos de Moderator conteúdo para o .NET.
