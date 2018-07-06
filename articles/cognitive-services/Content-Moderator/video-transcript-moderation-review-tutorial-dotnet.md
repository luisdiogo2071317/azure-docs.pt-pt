---
title: Azure Content Moderator - vídeos moderadas e transcrições no .NET | Documentos da Microsoft
description: Como utilizar o Content Moderator moderar vídeos e transcrições no .NET.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 1/27/2018
ms.author: sajagtap
ms.openlocfilehash: 0f851c030a05880d79a998ed4b4a941082c057b9
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865476"
---
# <a name="video-and-transcript-moderation-tutorial"></a>Tutorial de moderação de vídeo e transcrição

APIs de vídeo do Content Moderator permitem-lhe moderada de vídeos e criar as revisões de vídeo na ferramenta de revisão humana. 

Detalhadas este tutorial ajuda para compreender como criar uma solução de moderação de vídeo e transcrição completa com moderação assistida por computador e a criação de revisão humana no loop.

Transfira o [aplicação de consola c#](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) para este tutorial. O aplicativo de console usa o SDK e os pacotes relacionados para realizar as seguintes tarefas:

- Comprimir a entrada video(s) para processamento mais rápido
- Moderar o vídeo para obter capturas e quadros com informações
- Utilize os carimbos de data / intervalo para criar miniaturas (imagens)
- Submeter carimbos e miniaturas para criar as revisões de vídeo
- Converter o vídeo de voz em texto (transcrição) com a API do indexador de multimédia
- Moderar transcrição com o serviço de moderação de texto
- Adicionar a transcrição de moderadas a análise de vídeo

## <a name="sample-program-outputs"></a>Saídas de programa de exemplo

Antes de continuarmos, vamos examinar as saídas de exemplo follwing do programa:

- [Resultado da consola](#program-output)
- [Revisão de vídeo](#video-review-default-view)
- [Vista de transcrição](#video-review-transcript-view)

## <a name="prerequisites"></a>Pré-requisitos

1. Inscreva-se a [ferramenta de revisão do Content Moderator](https://contentmoderator.cognitive.microsoft.com/) site da web e [criar etiquetas personalizadas](Review-Tool-User-Guide/tags.md) que a aplicação de consola c# atribui a partir de dentro do código. O ecrã seguinte mostra as etiquetas personalizadas.

  ![Etiquetas personalizadas de moderação de vídeos](images/video-tutorial-custom-tags.png)

1. Para executar o aplicativo de exemplo, precisa de uma conta do Azure e uma conta de Media Services do Azure. Além disso, precisa de acesso para a pré-visualização privada do Content Moderator. Por fim, precisa de credenciais de autenticação do Azure Active Directory. Para obter detalhes sobre a obtenção dessas informações, consulte [o guia de introdução da API de moderação de vídeo](video-moderation-api.md).

1. Edite o ficheiro `App.config` e adicione o nome de inquilino do Active Directory, pontos finais de serviço e chaves de subscrição indicaram pelo `#####`. Terá das seguintes informações:

|Chave|Descrição|
|-|-|
|`AzureMediaServiceRestApiEndpoint`|Ponto final para a API de serviços (AMS) de multimédia do Azure|
|`ClientSecret`|Chave de subscrição para os serviços de multimédia do Azure|
|`ClientId`|ID de cliente para serviços de multimédia do Azure|
|`AzureAdTenantName`|Nome de inquilino do Active Directory, que representa a sua organização|
|`ContentModeratorReviewApiSubscriptionKey`|Chave de subscrição para o Content Moderator rever API|
|`ContentModeratorApiEndpoint`|Ponto final para a API do Content Moderator|
|`ContentModeratorTeamId`|ID da equipa o Content moderator|

## <a name="getting-started"></a>Introdução

A classe `Program` em `Program.cs` é o principal ponto de entrada para a aplicação de moderação de vídeo.

### <a name="methods-of-class-program"></a>Métodos da classe de programa

|Método|Descrição|
|-|-|
|`Main`|Analisa a linha de comandos, reúne a entrada do usuário e começa a processar.|
|`ProcessVideo`|Compacta, carrega, moderates e cria as revisões de vídeo.|
|`CreateVideoStreamingRequest`|Cria um fluxo para carregar um vídeo|
|`GetUserInputs`|Recolhe a entrada do usuário; utilizado quando não existem opções de linha de comandos estão presentes|
|`Initialize`|Inicializa os objetos necessários para o processo de moderação|

### <a name="the-main-method"></a>O método Main

`Main()` é onde começa a execução, pelo que é o lugar para começar a compreender o processo de moderação de vídeo.

    static void Main(string[] args)
    {
        if (args.Length == 0)
        {
            string videoPath = string.Empty;
            GetUserInputs(out videoPath);
            Initialize();
            AmsConfigurations.logFilePath = Path.Combine(Path.GetDirectoryName(videoPath), "log.txt");
            try
            {
                ProcessVideo(videoPath).Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
        else
        {
            DirectoryInfo directoryInfo = new DirectoryInfo(args[0]);
            if (args.Length == 2)
                bool.TryParse(args[1], out generateVtt);
            Initialize();
            AmsConfigurations.logFilePath = Path.Combine(args[0], "log.txt");
            var files = directoryInfo.GetFiles("*.mp4", SearchOption.AllDirectories);
            foreach (var file in files)
            {
                try
                {
                    ProcessVideo(file.FullName).Wait();
                }
                catch (Exception ex)
                {
                    Console.WriteLine(ex.Message);
                }
            }
        }
    }

`Main()` processa os argumentos da linha de comandos a seguir:

- O caminho para um diretório que contém ficheiros de vídeo de MPEG-4 para serem submetidas para moderação. Todos os `*.mp4` ficheiros neste diretório e respetivos subdiretórios são submetidos para moderação.
- Opcionalmente, um booleano (verdadeiro/falso) sinalizador que indica se as transcrições de texto devem ser geradas para efeitos de áudio moderadores.

Se nenhum argumento da linha de comandos estiverem presente, `Main()` chamadas `GetUserInputs()`. Este método pede ao utilizador para introduzir o caminho para um único ficheiro de vídeo e especificar se uma transcrição de texto deve ser gerada.

> [!NOTE]
> Utiliza a aplicação de consola do [API de indexador de multimédia do Azure](https://docs.microsoft.com/azure/media-services/media-services-process-content-with-indexer2) para gerar transcrições de faixa de áudio o vídeo carregado. Os resultados são fornecidos no formato WebVTT. Para obter mais informações sobre este formato, consulte [formato de faixas de texto de vídeo de Web](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API).

### <a name="initialize-and-processvideo-methods"></a>Métodos Initialize e ProcessVideo

Independentemente de se as opções do programa vem da linha de comando ou de entrada do usuário interativo, `Main()` chamadas seguintes `Initialize()` para criar as seguintes instâncias:

|Classe|Descrição|
|-|-|
|`AMSComponent`|Comprimir ficheiros de vídeo antes de submetê-los para moderação.|
|`AMSconfigurations`|Interface para dados de configuração do aplicativo, encontrados no `App.config`.|
|`VideoModerator`| A carregar, codificação, encriptação e moderação com o SDK do AMS|
|`VideoReviewApi`|Gere as revisões de vídeo no serviço do Content Moderator|

Essas classes (dos caracteres `AMSConfigurations`, que é muito simples) são abordadas mais detalhadamente nas próximas secções deste tutorial.

Por fim, os arquivos de vídeo são processado um de cada vez ao chamar `ProcessVideo()` para cada um.

    private static async Task ProcessVideo(string videoPath)
    {
        Stopwatch sw = new Stopwatch();
        sw.Start();
        Console.ForegroundColor = ConsoleColor.White;
        Console.WriteLine("\nVideo compression process started...");

        var compressedVideoPath = amsComponent.CompressVideo(videoPath);
        if (string.IsNullOrWhiteSpace(compressedVideoPath))
        {
            Console.ForegroundColor = ConsoleColor.Red;
            Console.WriteLine("Video Compression failed.");
        }

        Console.WriteLine("\nVideo compression process completed...");

        UploadVideoStreamRequest uploadVideoStreamRequest = CreateVideoStreamingRequest(compressedVideoPath);
        UploadAssetResult uploadResult = new UploadAssetResult();

        if (generateVtt)
        {
            uploadResult.GenerateVTT = generateVtt;
        }
        Console.WriteLine("\nVideo moderation process started...");

        if (!videoModerator.CreateAzureMediaServicesJobToModerateVideo(uploadVideoStreamRequest, uploadResult))
        {
            Console.ForegroundColor = ConsoleColor.Red;
            Console.WriteLine("Video Review process failed.");
        }

        Console.WriteLine("\nVideo moderation process completed...");
        Console.WriteLine("\nVideo review process started...");
        string reviewId = await videoReviewApi.CreateVideoReviewInContentModerator(uploadResult);
        Console.WriteLine("\nVideo review successfully completed...");
        sw.Stop();
        Console.WriteLine("\nTotal Elapsed Time: {0}", sw.Elapsed);
        using (var stw = new StreamWriter(AmsConfigurations.logFilePath, true))
        {
            stw.WriteLine("Video File Name: " + Path.GetFileName(videoPath));
            stw.WriteLine($"ReviewId: {reviewId}");
            stw.WriteLine("Total Elapsed Time: {0}", sw.Elapsed);
        }
    }


O `ProcessVideo()` método é bem simples. Ele realiza as seguintes operações pela ordem:

- Comprime o vídeo
- Carrega o vídeo para um recurso de serviços de multimédia do Azure
- Cria uma tarefa de AMS modere o vídeo
- Cria uma revisão de vídeo no Content Moderator

As seções a seguir, considere mais detalhadamente alguns dos processos individuais invocados pelo `ProcessVideo()`. 

## <a name="compressing-the-video"></a>A compressão do vídeo

Para minimizar o tráfego de rede, o aplicativo converte os arquivos de vídeo para o formato H.264 (MPEG-4 AVC) e pode ser dimensionada para uma largura máxima de 640 pixels. O codec H.264 é recomendado devido a sua eficiência elevada (taxa de compressão). A compressão é feita usando o gratuito `ffmpeg` ferramenta da linha de comandos, o que está incluída no `Lib` pasta da solução do Visual Studio. Os ficheiros de entrada podem ser de qualquer formato suportado por `ffmpeg`, incluindo o uso mais comum de arquivo de vídeo de formatos e codecs.

> [!NOTE]
> Quando inicia o programa usando opções da linha de comandos, especificar um diretório que contém os arquivos sejam submetidos para moderação de vídeo. Todos os ficheiros neste diretório que o `.mp4` extensão de nome de ficheiro são processados. Para processar outras extensões de nome de ficheiro, atualize o `Main()` método na `Program.cs` para incluir as extensões pretendidas.

O código que compacta um único ficheiro de vídeo é o `AmsComponent` classe na `AMSComponent.cs`. O método responsável por esta funcionalidade é `CompressVideo()`, mostrado aqui.

    public string CompressVideo(string videoPath)
    {
        string ffmpegBlobUrl;
        if (!ValidatePreRequisites())
        {
            Console.WriteLine("Configurations check failed. Please cross check the configurations!");
            throw new Exception();
        }

        if (File.Exists(_configObj.FfmpegExecutablePath))
        {
            ffmpegBlobUrl = this._configObj.FfmpegExecutablePath;
        }
        else
        {
            Console.WriteLine("ffmpeg.exe is missing. Please check the Lib folder");
            throw new Exception();
        }

        string videoFilePathCom = videoPath.Split('.')[0] + "_c.mp4";
        ProcessStartInfo processStartInfo = new ProcessStartInfo();
        processStartInfo.WindowStyle = ProcessWindowStyle.Hidden;
        processStartInfo.FileName = ffmpegBlobUrl;
        processStartInfo.Arguments = "-i \"" + videoPath + "\" -vcodec libx264 -n -crf 32 -preset veryfast -vf scale=640:-1 -c:a aac -aq 1 -ac 2 -threads 0 \"" + videoFilePathCom + "\"";
        var process = Process.Start(processStartInfo);
        process.WaitForExit();
        process.Close();
        return videoFilePathCom;
    }

O código executa as seguintes etapas:

- Verificações de certificar-se de que a configuração no `App.config` contém todos os dados necessários
- Verificações para garantir que o `ffmpeg` binário está presente
- Baseia-se o nome de ficheiro de saída, acrescentando `_c.mp4` para o nome de base do ficheiro (tal como `Example.mp4`  ->  `E>xample_c.mp4`)
- Cria uma cadeia de caracteres para realizar a conversão da linha de comandos
- Inicia um `ffmpeg` processamento usando a linha de comandos
- Aguarda que o vídeo para serem processados

> [!NOTE]
> Se conhece os seus vídeos já estão comprimidos com H.264 e têm dimensões adequadas, pode reescrever `CompressVideo()` para ignorar a compressão.

O método retorna o nome de ficheiro do ficheiro de saída comprimido.

## <a name="uploading-and-moderating-the-video"></a>A carregar e moderating o vídeo

O vídeo deve ser armazenado nos serviços de multimédia do Azure antes de ele possa ser processado pelo serviço de moderação de conteúdos. O `Program` classe na `Program.cs` possui um método curto `CreateVideoStreamingRequest()` que retorna um objeto que representa o pedido de transmissão em fluxo utilizado para carregar o vídeo.

    private static UploadVideoStreamRequest CreateVideoStreamingRequest(string compressedVideoFilePath)
    {
        return
            new UploadVideoStreamRequest
            {
                VideoStream = File.ReadAllBytes(compressedVideoFilePath),
                VideoName = Path.GetFileName(compressedVideoFilePath),
                EncodingRequest = new EncodingRequest()
                {
                    EncodingBitrate = AmsEncoding.AdaptiveStreaming
                },
                VideoFilePath = compressedVideoFilePath
            };
    }

Resultante `UploadVideoStreamRequest` objeto é definido em `UploadVideoStreamRequest.cs` (e o respetivo principal `UploadVideoRequest`, na `UploadVideoRequest.cs`). Essas classes não são apresentadas aqui; estão a curtos e servem apenas para conter os dados comprimidos de vídeo e as informações sobre ele. Outra classe só de dados `UploadAssetResult` (`UploadAssetResult.cs`) é utilizada para armazenar os resultados do processo de carregamento. Agora é possível compreender estas linhas no `ProcessVideo()`:

    UploadVideoStreamRequest uploadVideoStreamRequest = CreateVideoStreamingRequest(compressedVideoPath);
    UploadAssetResult uploadResult = new UploadAssetResult();

    if (generateVtt)
    {
        uploadResult.GenerateVTT = generateVtt;
    }
    Console.WriteLine("\nVideo moderation process started...");

    if (!videoModerator.CreateAzureMediaServicesJobToModerateVideo(uploadVideoStreamRequest, uploadResult))
    {
        Console.ForegroundColor = ConsoleColor.Red;
        Console.WriteLine("Video Review process failed.");
    }

Estas linhas realizar as seguintes tarefas:

- Criar um `UploadVideoStreamRequest` para carregar o vídeo compactado
- Definiu a solicitação `GenerateVTT` sinalizar se o utilizador solicitou uma transcrição de texto
- Chamadas `CreateAzureMediaServicesJobToModerateVideo()` para efetuar o carregamento e receber o resultado

## <a name="deep-dive-into-video-moderation"></a>Descrição aprofundada moderação de vídeos

O método `CreateAzureMediaServicesJobToModerateVideo()` está em `VideoModerator.cs`, que contém a maior parte do código que interage com os serviços de multimédia do Azure. Código-fonte do método é mostrado no extrair seguinte.

    public bool CreateAzureMediaServicesJobToModerateVideo(UploadVideoStreamRequest uploadVideoRequest, UploadAssetResult uploadResult)
    {
        asset = CreateAsset(uploadVideoRequest);
        uploadResult.VideoName = uploadVideoRequest.VideoName;
        // Encoding the asset , Moderating the asset, Generating transcript in parallel
        IAsset encodedAsset = null;
        //Creates the job for the tasks.
        IJob job = this._mediaContext.Jobs.Create("AMS Review Job");

        //Adding encoding task to job.
        ConfigureEncodeAssetTask(uploadVideoRequest.EncodingRequest, job);

        ConfigureContentModerationTask(job);

        //adding transcript task to job.
        if (uploadResult.GenerateVTT)
        {
            ConfigureTranscriptTask(job);
        }

        Stopwatch timer = new Stopwatch();
        timer.Start();
        //submit and execute job.
        job.Submit();
        job.GetExecutionProgressTask(new CancellationTokenSource().Token).Wait();
        timer.Stop();
        using (var sw = new StreamWriter(AmsConfigurations.logFilePath, true))
        {
            sw.WriteLine("AMS Job Elapsed Time: {0}", timer.Elapsed);
        }

        if (job.State == JobState.Error)
        {
            throw new Exception("Video moderation has failed due to AMS Job error.");
        }

        UploadAssetResult result = uploadResult;
        encodedAsset = job.OutputMediaAssets[0];
        result.ModeratedJson = GetCmDetail(job.OutputMediaAssets[1]);
        // Check for valid Moderated JSON
        var jsonModerateObject = JsonConvert.DeserializeObject<VideoModerationResult>(result.ModeratedJson);

        if (jsonModerateObject == null)
        {
            return false;
        }
        if (uploadResult.GenerateVTT)
        {
            GenerateTranscript(job.OutputMediaAssets.Last());
        }

        uploadResult.StreamingUrlDetails = PublishAsset(encodedAsset);
        string downloadUrl = GenerateDownloadUrl(asset, uploadVideoRequest.VideoName);
        uploadResult.StreamingUrlDetails.DownloadUri = downloadUrl;
        uploadResult.VideoName = uploadVideoRequest.VideoName;
        uploadResult.VideoFilePath = uploadVideoRequest.VideoFilePath;
        return true;
    }

Esse código realiza as seguintes tarefas:

- Cria uma tarefa de AMS para o processamento ser feita
- Adiciona tarefas para codificar o ficheiro de vídeo, moderating-lo e gerar uma transcrição de texto
- Submete o trabalho, carregar o ficheiro e o processamento de início
- Obtém os resultados de moderação, a transcrição de texto (se requerido) e outras informações

## <a name="sample-video-moderation-response"></a>Resposta de moderação de vídeos de exemplo

O resultado da tarefa de moderação de vídeo (veja [guia de introdução de moderação de vídeos](video-moderation-api.md) é uma estrutura de dados JSON que contém os resultados de moderação. Esses resultados incluem uma análise detalhada dos fragmentos (capturas) no vídeo, cada eventos que contêm (clips) com quadros-chave que tenha sido sinalizados para revisão. Cada quadro-chave é classificado pela probabilidade de que ele contém conteúdo adultos. O exemplo seguinte mostra uma resposta JSON:

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

Também é uma transcrição de áudio do vídeo produzido quando o `GenerateVTT` sinalizador está definido.

> [!NOTE]
> Utiliza a aplicação de consola do [API de indexador de multimédia do Azure](https://docs.microsoft.com/azure/media-services/media-services-process-content-with-indexer2) para gerar transcrições de faixa de áudio o vídeo carregado. Os resultados são fornecidos no formato WebVTT. Para obter mais informações sobre este formato, consulte [formato de faixas de texto de vídeo de Web](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API).


## <a name="creating-the-human-in-the-loop-review"></a>Criar a revisão de humanos em loop

O processo de moderação devolve uma lista de quadros-chave do vídeo, juntamente com uma transcrição dos respetivas faixas de áudio. A próxima etapa é criar uma revisão na ferramenta de revisão do Content Moderator para moderadores humanos. Voltando para o `ProcessVideo()` método na `Program.cs`, consulte a chamada para o `CreateVideoReviewInContentModerator()` método. Este método é no `videoReviewApi` classe, que está no `VideoReviewAPI.cs`e é mostrada aqui.

    public async Task<string> CreateVideoReviewInContentModerator(UploadAssetResult uploadAssetResult)
    {
    
        string reviewId = string.Empty;
        List<ProcessedFrameDetails> frameEntityList = framegenerator.CreateVideoFrames(uploadAssetResult);
        string path = uploadAssetResult.GenerateVTT == true ? this._amsConfig.FfmpegFramesOutputPath + Path.GetFileNameWithoutExtension (uploadAssetResult.VideoName) + "_aud_SpReco.vtt" : "";
        TranscriptScreenTextResult screenTextResult = new TranscriptScreenTextResult();
        
    if (File.Exists(path))
        {
            screenTextResult = await GenerateTextScreenProfanity(reviewId, path, frameEntityList);
            uploadAssetResult.Category1TextScore = screenTextResult.Category1Score;
            uploadAssetResult.Category2TextScore = screenTextResult.Category2Score;
            uploadAssetResult.Category3TextScore = screenTextResult.Category3Score;
            uploadAssetResult.Category1TextTag = screenTextResult.Category1Tag;
            uploadAssetResult.Category2TextTag = screenTextResult.Category2Tag;
            uploadAssetResult.Category3TextTag = screenTextResult.Category3Tag;
        }
        
        var reviewVideoRequestJson = CreateReviewRequestObject(uploadAssetResult, frameEntityList);
        if (string.IsNullOrWhiteSpace(reviewVideoRequestJson))
        {
            throw new Exception("Video review process failed in CreateVideoReviewInContentModerator");
        }
        
        reviewId = JsonConvert.DeserializeObject<List<string>>(ExecuteCreateReviewApi(reviewVideoRequestJson).Result).FirstOrDefault();
        frameEntityList = framegenerator.GenerateFrameImages(frameEntityList, uploadAssetResult, reviewId);
        await CreateAndPublishReviewInContentModerator(uploadAssetResult, frameEntityList, reviewId, path, screenTextResult);
        return reviewId;
    
    }

`CreateVideoReviewInContentModerator()` chama vários outros métodos para realizar as seguintes tarefas:

> [!NOTE]
> Utiliza a aplicação de consola do [FFmpeg](https://ffmpeg.org/) biblioteca para gerar miniaturas. Dessas miniaturas (imagens) correspondem aos carimbos quadro na [saída de moderação de vídeos](#sample-video-moderation-response).

|Tarefa|Métodos|Ficheiro|
|-|-|-|
|Extrair a chave de quadros-do vídeo e cria imagens em miniatura dos mesmos|`CreateVideoFrames()`<br>`GenerateFrameImages()`|`FrameGeneratorServices.cs`|
|Analisar a transcrição de texto, se estiver disponível, para localizar o áudio de adultos|`GenerateTextScreenProfanity()`| `VideoReviewAPI.cs`|
|Preparar e envia uma solicitação de revisão de vídeo para inspeção humana|`CreateReviewRequestObject()`<br> `ExecuteCreateReviewApi()`<br>`CreateAndPublishReviewInContentModerator()`|`VideoReviewAPI.cs`|

## <a name="video-review-default-view"></a>Modo de exibição de padrão de revisão de vídeo

O ecrã seguinte mostra os resultados dos passos anteriores.

![Modo de exibição de padrão de revisão de vídeo](images/video-tutorial-default-view.PNG)

## <a name="transcript-generation"></a>Geração de transcrição

Até agora, o código apresentado neste tutorial tem se concentrado no conteúdo visual. Revisão do conteúdo de voz é um processo separado e opcional que, como mencionado, utiliza uma transcrição gerada a partir de áudio. Chegou a hora agora dar uma olhada em como transcrições de texto são criadas e usadas no processo de revisão. A tarefa de gerar a transcrição passa para o [indexador de multimédia do Azure](https://docs.microsoft.com/azure/media-services/media-services-index-content) serviço.

A aplicação realiza as seguintes tarefas:

|Tarefa|Métodos|Ficheiro|
|-|-|-|
|Determinar se são de transcrições de texto a ser gerado|`Main()`<br>`GetUserInputs()`|`Program.cs`|
|Se assim for, submeter uma tarefa de transcrição como parte da moderação|`ConfigureTranscriptTask()`|`VideoModerator.cs`|
|Obtenha uma cópia local da transcrição|`GenerateTranscript()`|`VideoModerator.cs`|
|Sinalizador de fotogramas do vídeo que contêm áudio inadequado|`GenerateTextScreenProfanity()`<br>`TextScreen()`|`VideoReviewAPI.cs`|
|Adicionar os resultados à revisão|`UploadScreenTextResult()`<br>`ExecuteAddTranscriptSupportFile()`|`VideoReviewAPI.cs`|

### <a name="task-configuration"></a>Configuração da tarefa

Vamos entrar de cabeça em submeter a tarefa de transcrição. `CreateAzureMediaServicesJobToModerateVideo()` (já foi descritas) chamadas `ConfigureTranscriptTask()`.

    private void ConfigureTranscriptTask(IJob job)
    {
        string mediaProcessorName = _amsConfigurations.MediaIndexer2MediaProcessor;
        IMediaProcessor processor = _mediaContext.MediaProcessors.GetLatestMediaProcessorByName(mediaProcessorName);

        string configuration = File.ReadAllText(_amsConfigurations.MediaIndexerConfigurationJson);
        ITask task = job.Tasks.AddNew("AudioIndexing Task", processor, configuration, TaskOptions.None);
        task.InputAssets.Add(asset);
        task.OutputAssets.AddNew("AudioIndexing Output Asset", AssetCreationOptions.None);
    }

A configuração para a tarefa de transcrição é lidos a partir do ficheiro `MediaIndexerConfig.json` da solução `Lib` pasta. Ativos de AMS são criados para o ficheiro de configuração e para a saída do processo de transcrição. Quando é executada a tarefa de AMS, esta tarefa cria uma transcrição de texto da faixa de áudio do ficheiro de vídeo.

> [!NOTE]
> O aplicativo de exemplo reconhece voz apenas em inglês Americano.

### <a name="transcript-generation"></a>Geração de transcrição

A transcrição for publicada como um recurso de AMS. Para analisar a transcrição para conteúdo objetáveis, o aplicativo transfere o elemento a partir dos serviços de multimédia do Azure. `CreateAzureMediaServicesJobToModerateVideo()` chamadas `GenerateTranscript()`, conforme apresentado aqui, para recuperar o arquivo.

    public bool GenerateTranscript(IAsset asset)
    {
        try
        {
            var outputFolder = this._amsConfigurations.FfmpegFramesOutputPath;
            IAsset outputAsset = asset;
            IAccessPolicy policy = null;
            ILocator locator = null;
            policy = _mediaContext.AccessPolicies.Create("My 30 days readonly policy", TimeSpan.FromDays(360), AccessPermissions.Read);
            locator = _mediaContext.Locators.CreateLocator(LocatorType.Sas, outputAsset, policy, DateTime.UtcNow.AddMinutes(-5));
            DownloadAssetToLocal(outputAsset, outputFolder);
            locator.Delete();
            return true;
        }
        catch
        {   //TODO:  Logging
            Console.WriteLine("Exception occured while generating index for video.");
            throw;
        }
    }

Depois de algum programa de configuração do AMS necessário, a transferência real é executada chamando `DownloadAssetToLocal()`, uma função genérica que copia um ativo de AMS para um ficheiro local.

## <a name="transcript-moderation"></a>Moderação de transcrição

Com a transcrição à disposição, ele é analisado e usado da revisão. Criar a revisão é da competência do `CreateVideoReviewInContentModerator()`, que chamadas `GenerateTextScreenProfanity()` para fazer o trabalho. Por sua vez, esse método chama `TextScreen()`, que contém a maior parte da funcionalidade. 

`TextScreen()` executa as seguintes tarefas:

- Analisar a transcrição de tempo tamps e legendagem de áudio
- Submeter a legenda de cada para moderação de texto
- Sinalizador qualquer quadros que possam ter conteúdo de voz objetáveis

Vamos examinar cada uma dessas tarefas mais detalhadamente:

### <a name="initialize-the-code"></a>Inicializar o código

Primeiro, iniciar todas as coleções e variáveis.

    private async Task<TranscriptScreenTextResult> TextScreen(string filepath, List<ProcessedFrameDetails> frameEntityList)
    {
        List<TranscriptProfanity> profanityList = new List<TranscriptProfanity>();
        string responseContent = string.Empty;
        HttpResponseMessage response;
        bool category1Tag = false;
        bool category2Tag = false;
        bool category3Tag = false;
        double category1Score = 0;
        double category2Score = 0;
        double category3Score = 0;
        List<string> vttLines = File.ReadAllLines(filepath).Where(line => !line.Contains("NOTE Confidence:") && line.Length > 0).ToList();
        StringBuilder sb = new StringBuilder();
        List<CaptionScreentextResult> csrList = new List<CaptionScreentextResult>();
        CaptionScreentextResult captionScreentextResult = new CaptionScreentextResult() { Captions = new List<string>() };

        // Code from the next sections in the tutorial
    

### <a name="parse-the-transcript-for-captions"></a>Analisar a transcrição de legendas de áudio

Em seguida, analisar a transcrição VTT formatada para legendas e carimbos de data /. A ferramenta de revisão apresenta estas legendas no separador de transcrição no ecrã de revisão de vídeo. Os carimbos de data / é utilizados para sincronizar as legendas com quadros de vídeo correspondentes.

        // Code from the previous section(s) in the tutorial

        //
        // Parse the transcript
        //
        foreach (var line in vttLines.Skip(1))
        {
                if (line.Contains("-->"))
                {
                    if (sb.Length > 0)
                    {
                        captionScreentextResult.Captions.Add(sb.ToString());
                        sb.Clear();
                    }
                    if (captionScreentextResult.Captions.Count > 0)
                    {
                        csrList.Add(captionScreentextResult);
                        captionScreentextResult = new CaptionScreentextResult() { Captions = new List<string>() };
                    }
                    string[] times = line.Split(new string[] { "-->" }, StringSplitOptions.RemoveEmptyEntries);
                    string startTimeString = times[0].Trim();
                    string endTimeString = times[1].Trim();
                    int startTime = (int)TimeSpan.ParseExact(startTimeString, @"hh\:mm\:ss\.fff", CultureInfo.InvariantCulture).TotalMilliseconds;
                    int endTime = (int)TimeSpan.ParseExact(endTimeString, @"hh\:mm\:ss\.fff", CultureInfo.InvariantCulture).TotalMilliseconds;
                    captionScreentextResult.StartTime = startTime;
                    captionScreentextResult.EndTime = endTime;
                }
                else
                {
                    sb.Append(line);
                }
                if (sb.Length + line.Length > 1024)
                {
                    captionScreentextResult.Captions.Add(sb.ToString());
                    sb.Clear();
                }
            }
            if (sb.Length > 0)
            {
                captionScreentextResult.Captions.Add(sb.ToString());
            }
            if (captionScreentextResult.Captions.Count > 0)
            {
                csrList.Add(captionScreentextResult);
            }

            // Code from the following section in the quickstart

### <a name="moderate-captions-with-the-text-moderation-service"></a>Legendas moderadas com o serviço de moderação de texto

Em seguida, vamos analisar as legendas de texto analisado com a API de texto de Content Moderator.

> [!NOTE]
> A chave de serviço do Content Moderator tem um pedidos por segundo (RPS) para o limite de velocidade. Se ultrapassar o limite, o SDK lançará uma exceção com um código de 429 erro. 
>
> Uma chave de escalão gratuito tem um limite de taxa de um RPS.

    //
    // Moderate the captions or cues
    //
    int waitTime = 1000;
    foreach (var csr in csrList)
    {
                bool captionAdultTextTag = false;
                bool captionRacyTextTag = false;
                bool captionOffensiveTextTag = false;
                bool retry = true;

                foreach (var caption in csr.Captions)
                {
                    while (retry)
                    {
                        try
                        {
                            System.Threading.Thread.Sleep(waitTime);
                            var lang = await CMClient.TextModeration.DetectLanguageAsync("text/plain", caption);
                            var oRes = await CMClient.TextModeration.ScreenTextWithHttpMessagesAsync(lang.DetectedLanguageProperty, "text/plain", caption, null, null, null, true);
                            response = oRes.Response;
                            responseContent = await response.Content.ReadAsStringAsync();
                            retry = false;
                        }
                        catch (Exception e)
                        {
                            if (e.Message.Contains("429"))
                            {
                                Console.WriteLine($"Moderation API call failed. Message: {e.Message}");
                                waitTime = (int)(waitTime * 1.5);
                                Console.WriteLine($"wait time: {waitTime}");
                            }
                            else
                            {
                                retry = false;
                                Console.WriteLine($"Moderation API call failed. Message: {e.Message}");
                            }
                        }
                    }
                    var jsonTextScreen = JsonConvert.DeserializeObject<TextScreen>(responseContent);
                    if (jsonTextScreen != null)
                    {
                        TranscriptProfanity transcriptProfanity = new TranscriptProfanity();
                        transcriptProfanity.TimeStamp = "";
                        List<Terms> transcriptTerm = new List<Terms>();
                        if (jsonTextScreen.Terms != null)
                        {
                            foreach (var term in jsonTextScreen.Terms)
                            {
                                var profanityobject = new Terms
                                {
                                    Term = term.Term,
                                    Index = term.Index
                                };
                                transcriptTerm.Add(profanityobject);
                            }
                            transcriptProfanity.Terms = transcriptTerm;
                            profanityList.Add(transcriptProfanity);
                        }
                        if (jsonTextScreen.Classification.Category1.Score > _amsConfig.Category1TextThreshold) captionAdultTextTag = true;
                        if (jsonTextScreen.Classification.Category2.Score > _amsConfig.Category2TextThreshold) captionRacyTextTag = true;
                        if (jsonTextScreen.Classification.Category3.Score > _amsConfig.Category3TextThreshold) captionOffensiveTextTag = true;
                        if (jsonTextScreen.Classification.Category1.Score > _amsConfig.Category1TextThreshold) category1Tag = true;
                        if (jsonTextScreen.Classification.Category2.Score > _amsConfig.Category2TextThreshold) category2Tag = true;
                        if (jsonTextScreen.Classification.Category3.Score > _amsConfig.Category3TextThreshold) category3Tag = true;
                        category1Score = jsonTextScreen.Classification.Category1.Score > category1Score ? jsonTextScreen.Classification.Category1.Score : category1Score;
                        category2Score = jsonTextScreen.Classification.Category2.Score > category2Score ? jsonTextScreen.Classification.Category2.Score : category2Score;
                        category3Score = jsonTextScreen.Classification.Category3.Score > category3Score ? jsonTextScreen.Classification.Category3.Score : category3Score;
                    }
                    foreach (var frame in frameEntityList.Where(x => x.TimeStamp >= csr.StartTime && x.TimeStamp <= csr.EndTime))
                    {
                        frame.IsAdultTextContent = captionAdultTextTag;
                        frame.IsRacyTextContent = captionRacyTextTag;
                        frame.IsOffensiveTextContent = captionOffensiveTextTag;
                    }
                }
            }
            TranscriptScreenTextResult screenTextResult = new TranscriptScreenTextResult()
            {
                TranscriptProfanity = profanityList,
                Category1Tag = category1Tag,
                Category2Tag = category2Tag,
                Category3Tag = category3Tag,
                Category1Score = category1Score,
                Category2Score = category2Score,
                Category3Score = category3Score
            };
            return screenTextResult;
    }

### <a name="breaking-down-the-text-moderation-step"></a>Rompendo a etapa de moderação de texto

`TextScreen()` é um método substancial, então, vamos dividi-la.

1. Em primeiro lugar, o método lê o arquivo de transcrição linha por linha. Ele ignora linhas em branco e as linhas que contêm um `NOTE` com uma pontuação de confiança. Extrai os carimbos de data / hora e itens de texto a partir do *indicações* no ficheiro. Uma indicação representa o texto de faixa de áudio e inclui horários de início e final. Uma indicação começa com a linha de carimbo de data / hora com a cadeia de caracteres `-->`. Ele é seguido por um ou mais linhas de texto.

1. Instâncias de `CaptionScreentextResult` (definidos na `TranscriptProfanity.cs`) são utilizados para armazenar as informações analisadas a partir de cada indicação.  Quando é detetada uma nova linha de carimbo de data / hora, ou um comprimento de texto do máximo de 1024 carateres for atingido, um novo `CaptionScreentextResult` é adicionado ao `csrList`. 

1. O método submete, em seguida, cada sugestão para a API de moderação de texto. Ele chama ambas `ContentModeratorClient.TextModeration.DetectLanguageAsync()` e `ContentModeratorClient.TextModeration.ScreenTextWithHttpMessagesAsync()`, que é definido no `Microsoft.Azure.CognitiveServices.ContentModerator` assembly. Para evitar a ser limitado de taxa, o método coloca em pausa por um segundo antes de submeter cada indicação.

1. Depois de receber os resultados do serviço de moderação de texto, o método, em seguida, analisa-lhes para verem se cumprem os limites de confiança. Estes valores são estabelecidos `App.config` como `OffensiveTextThreshold`, `RacyTextThreshold`, e `AdultTextThreshold`. Por fim, os termos de objetáveis propriamente ditos também são armazenados. Todos os quadros dentro do intervalo de tempo a indicação são sinalizados como contendo ofensiva, para adultos e/ou texto para adultos.

1. `TextScreen()` Devolve um `TranscriptScreenTextResult` instância que contém o resultado de moderação de texto do vídeo como um todo. Este objeto inclui sinalizadores e as notas de vários tipos de conteúdo objetáveis, juntamente com uma lista de todos os objetáveis termos. O chamador `CreateVideoReviewInContentModerator()`, chamadas `UploadScreenTextResult()` anexar estas informações para a revisão, de modo está disponível para os revisores humanos.
 
## <a name="video-review-transcript-view"></a>Vista de transcrição de vídeo de revisão

O ecrã seguinte mostra o resultado da transcrição passos geração e moderação.

![Vista de transcrição de moderação de vídeos](images/video-tutorial-transcript-view.PNG)

## <a name="program-output"></a>Saída do programa

A seguinte saída da linha de comandos do programa mostra as diversas tarefas à medida que estejam concluídos. Além disso, o resultado de moderação (no formato JSON) e a transcrição de voz estão disponíveis no mesmo diretório que os ficheiros de vídeo originais.

    Microsoft.ContentModerator.AMSComponentClient
    Enter the fully qualified local path for Uploading the video :
    "Your File Name.MP4"
    Generate Video Transcript? [y/n] : y
    
    Video compression process started...
    Video compression process completed...
    
    Video moderation process started...
    Video moderation process completed...
    
    Video review process started...
    Video Frames Creation inprogress...
    Frames(83) created successfully.
    Review Created Successfully and the review Id 201801va8ec2108d6e043229ba7a9e6373edec5
    Video review successfully completed...
    
    Total Elapsed Time: 00:05:56.8420355


## <a name="next-steps"></a>Passos Seguintes

[Transferir a solução do Visual Studio](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) arquivos e as bibliotecas necessárias de exemplo para este tutorial e começar a trabalhar com sua integração.
