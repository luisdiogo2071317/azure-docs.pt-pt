---
title: Azure Moderator conteúdo - moderados vídeos e transcrições no .NET | Microsoft Docs
description: Como utilizar conteúdo Moderator para moderada vídeos e transcrições no .NET.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 1/27/2018
ms.author: sajagtap
ms.openlocfilehash: a084b50e44fe26ba2547d0f7b7ed184fb71b190c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352525"
---
# <a name="video-and-transcript-moderation-tutorial"></a>Tutorial de moderação interrupção de vídeos e transcript

APIs de vídeos do conteúdo Moderator permitem-lhe moderada vídeos e criar as vídeos revisões na ferramenta de revisão humanos. 

Este tutorial ajuda a compreender como criar uma solução de moderação interrupção de vídeos e transcript completa com moderação de interrupção assistido por computador e a criação de revisão humanos no ciclo em pormenor.

Transferir o [aplicação de consola c#](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) para este tutorial. A aplicação de consola utiliza o SDK e os pacotes relacionados para realizar as seguintes tarefas:

- Comprimir a entrada video(s) para processamento mais rápido
- Moderada o vídeo para obter capturas e frames com insights
- Utilize os carimbos de moldura para criar miniaturas (imagens)
- Submeter carimbos e as miniaturas para criar as vídeos revisões
- Converter o reconhecimento de voz vídeo de texto (transcript) com a API do indexador de suporte de dados
- Moderada transcript com o serviço de moderação interrupção de texto
- Adicionar o transcript moderated para rever a vídeo

## <a name="sample-program-outputs"></a>Saídas do programa de exemplo

Antes de ir além disso, vamos ver as saídas de exemplo follwing do programa:

- [Resultado da consola](#program-output)
- [Revisão de vídeo](#video-review-default-view)
- [Vista de transcript](#video-review-transcript-view)

## <a name="prerequisites"></a>Pré-requisitos

1. Inscrever-se a [ferramenta de revisão Moderator conteúdo](https://contentmoderator.cognitive.microsoft.com/) do web site e [criar etiquetas personalizadas](Review-Tool-User-Guide/tags.md) que a aplicação de consola c# atribui a partir de dentro do código. O ecrã seguinte mostra as etiquetas personalizadas.

  ![Etiquetas personalizadas de moderação de interrupção gráfica](images/video-tutorial-custom-tags.png)

1. Para executar a aplicação de exemplo, precisa de uma conta do Azure e uma conta de Media Services do Azure. Além disso, tem de aceder a pré-visualização privada Moderator conteúdo. Por último, precisa de credenciais de autenticação do Azure Active Directory. Para obter mais informações sobre como obter estas informações, consulte [o guia de introdução do vídeo moderação interrupção API](video-moderation-api.md).

1. Edite o ficheiro `App.config` e adicione o nome de inquilino do Active Directory, pontos finais de serviço e chaves de subscrição indicaram pelo `#####`. Terá das seguintes informações:

|Chave|Descrição|
|-|-|
|`AzureMediaServiceRestApiEndpoint`|Ponto final para a API de serviços (AMS) de Media Services do Azure|
|`ClientSecret`|Chave de subscrição de Media Services do Azure|
|`ClientId`|ID de cliente para serviços de multimédia do Azure|
|`AzureAdTenantName`|Nome de inquilino do Active Directory, que representa a sua organização|
|`ContentModeratorReviewApiSubscriptionKey`|Chave de subscrição para o conteúdo Moderator rever API|
|`ContentModeratorApiEndpoint`|Ponto final para o conteúdo Moderator API|
|`ContentModeratorTeamId`|ID de equipa moderator conteúdo|

## <a name="getting-started"></a>Introdução

A classe `Program` no `Program.cs` é o ponto de entrada principal para a aplicação de moderação de interrupção gráfica.

### <a name="methods-of-class-program"></a>Métodos de classe de programa

|Método|Descrição|
|-|-|
|`Main`|Analisa a linha de comandos, reúne intervenção do utilizador e inicia o processamento.|
|`ProcessVideo`|Comprimir, carrega, moderates e cria as vídeos revisões.|
|`CreateVideoStreamingRequest`|Cria uma sequência para carregar um vídeo|
|`GetUserInputs`|Reúne intervenção do utilizador; utilizado quando não existem opções da linha de comandos estão presentes|
|`Initialize`|Inicializa os objetos necessários para o processo de moderação interrupção|

### <a name="the-main-method"></a>O método principal

`Main()` está em execução for iniciada, pelo que é o local para iniciar a compreender o processo de moderação de interrupção gráfica.

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

`Main()` processa os argumentos da linha de comandos seguintes:

- O caminho para um diretório que contém ficheiros de vídeo de MPEG-4 para ser submetido para moderação interrupção. Todos os `*.mp4` ficheiros neste diretório e nos seus subdiretórios são submetidos para moderação interrupção.
- Opcionalmente, um booleano (verdadeiro/falso) sinalizador que indica se devem ser geradas transcrições de texto para fins de áudio moderating.

Se existirem sem argumentos da linha de comandos, `Main()` chamadas `GetUserInputs()`. Este método pede ao utilizador para introduzir o caminho para um único ficheiro de vídeo e especificar se deve ser gerado um transcript de texto.

> [!NOTE]
> A aplicação de consola utiliza o [API de indexador de suporte de dados do Azure](https://docs.microsoft.com/azure/media-services/media-services-process-content-with-indexer2) para gerar transcrições de controlar as vídeo carregado de áudio. Os resultados são fornecidos no formato WebVTT. Para obter mais informações sobre este formato, consulte [Web vídeo texto controla formato](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API).

### <a name="initialize-and-processvideo-methods"></a>Métodos de inicializar e ProcessVideo

Independentemente se as opções do programa fornecido na linha de comandos ou de entrada de utilizador interativa, `Main()` chamadas seguintes `Initialize()` para criar as seguintes instâncias:

|Classe|Descrição|
|-|-|
|`AMSComponent`|Comprimir ficheiros de vídeo antes de submetê-los para moderação interrupção.|
|`AMSconfigurations`|Interface para dados de configuração da aplicação, foi encontrados no `App.config`.|
|`VideoModerator`| Carregamento, codificação, encriptação e a moderação de interrupção utilizando o SDK do AMS|
|`VideoReviewApi`|Gere as vídeos revisões no serviço Moderator conteúdo|

Estas classes (aside de `AMSConfigurations`, que é simples) descritos mais detalhadamente nas secções futuras deste tutorial.

Por fim, os ficheiros de vídeos são processado um de cada vez chamando `ProcessVideo()` para cada.

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


O `ProcessVideo()` método é bastante simples. Executa as seguintes operações pela ordem:

- Comprimir o vídeo
- Carrega o vídeo para um recurso de Media Services do Azure
- Cria uma tarefa de AMS para moderada o vídeo
- Cria uma revisão de vídeo no Moderator conteúdo

Considere as seguintes secções mais detalhadamente alguns dos processos individuais invocados pelo `ProcessVideo()`. 

## <a name="compressing-the-video"></a>A compressão o vídeo

Para minimizar o tráfego de rede, a aplicação converte os ficheiros de vídeo no formato de 264 (MPEG-4 AVC) e dimensiona-los para uma largura máxima de 640 pixéis. O codec 264 é recomendado devido à respetiva eficácia elevada (taxa de compressão). A compressão é feita utilizando o livre `ffmpeg` ferramenta da linha de comandos, que está incluída no `Lib` pasta da solução Visual Studio. Os ficheiros de entrada podem ser de qualquer formato suportado pelo `ffmpeg`, incluindo os formatos de ficheiro de vídeo frequentemente utilizadas e codecs.

> [!NOTE]
> Quando inicia o programa com as opções da linha de comandos, especifique um diretório que contém os ficheiros sejam submetidos para moderação interrupção gráficas. Todos os ficheiros neste diretório, tendo o `.mp4` extensão de nome de ficheiro são processados. Para processar outras extensões de nome de ficheiro, atualize o `Main()` método `Program.cs` para incluir as extensões pretendidas.

O código de comprime um ficheiro de vídeo único é o `AmsComponent` classe no `AMSComponent.cs`. O método responsável por esta funcionalidade é `CompressVideo()`, apresentados aqui.

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

O código efetua os seguintes passos:

- As verificações de certificar-se de que a configuração no `App.config` contém todos os dados necessários
- As verificações de certificar-se a `ffmpeg` binário está presente
- Cria o nome de ficheiro de saída, acrescentando `_c.mp4` para o nome do ficheiro de base (tais como `Example.mp4`  ->  `E>xample_c.mp4`)
- Cria uma cadeia de linha de comandos para efetuar a conversão
- Inicia um `ffmpeg` processar utilizando a linha de comandos
- Aguarda que o vídeo ser processado

> [!NOTE]
> Se souber seus vídeos já são comprimidos utilizando 264 e tem dimensões adequadas, possam reescrever `CompressVideo()` para ignorar a compressão.

O método devolve o nome de ficheiro do ficheiro de saída comprimido.

## <a name="uploading-and-moderating-the-video"></a>Carregar e moderating o vídeo

O vídeo deve ser armazenado no Media Services do Azure antes de podem ser processado pelo serviço de moderação de interrupção de conteúdo. O `Program` classe no `Program.cs` tem um método curto `CreateVideoStreamingRequest()` que devolve um objeto que representa o pedido de transmissão em fluxo utilizado para carregar o vídeo.

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

Resultante `UploadVideoStreamRequest` objeto está definido no `UploadVideoStreamRequest.cs` (e o respetivo principal `UploadVideoRequest`, na `UploadVideoRequest.cs`). Estas classes não são mostradas aqui; Se estiver a curtos e servem apenas para conter as informações sobre o assunto e dados comprimidos de vídeos. Outra classe apenas de dados, `UploadAssetResult` (`UploadAssetResult.cs`) é utilizado para armazenar os resultados do processo de carregamento. Agora é possível compreender estas linhas no `ProcessVideo()`:

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

- Criar um `UploadVideoStreamRequest` para carregar o vídeo comprimido
- Definir o pedido `GenerateVTT` sinalizador se o utilizador pediu uma transcript de texto
- Chamadas `CreateAzureMediaServicesJobToModerateVideo()` para realizar o carregamento e receber o resultado

## <a name="deep-dive-into-video-moderation"></a>Descrição detalhada para moderação de interrupção gráfica

O método `CreateAzureMediaServicesJobToModerateVideo()` está a ser `VideoModerator.cs`, que contém o volume do código que interage com Media Services do Azure. Código de origem do método é mostrado no extrair seguinte.

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

Este código efetua as seguintes tarefas:

- Cria uma tarefa de AMS para o processamento ser concluído
- Adiciona as tarefas de codificação de ficheiro de vídeo, moderating-lo e gerar um transcript de texto
- Submete a tarefa, carregar o ficheiro e o processamento de início
- Obtém os resultados de moderação interrupção, o transcript de texto (se requerido) e outras informações

## <a name="sample-video-moderation-response"></a>Resposta de moderação de interrupção gráfica de amostra

O resultado da tarefa de moderação de interrupção gráfica (consulte [início rápido de moderação de interrupção gráfica](video-moderation-api.md) é uma estrutura de dados JSON que contém os resultados de moderação interrupção. Estes resultados incluem uma repartição dos fragmentos (capturas) dentro de vídeo, cada eventos que contêm (aplicações de clips) com chaves frames que tenham sido sinalizados para revisão. Cada fotograma chave é classificada pela probabilidade que contém conteúdo para adultos ou racy. O exemplo seguinte mostra uma resposta JSON:

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

Um transcription de áudio do vídeo também é produzido quando o `GenerateVTT` sinalizador está definido.

> [!NOTE]
> Utiliza a aplicação de consola o [API de indexador de suporte de dados do Azure](https://docs.microsoft.com/azure/media-services/media-services-process-content-with-indexer2) para gerar transcrições da controlar as vídeo carregado de áudio. Os resultados são fornecidos no formato WebVTT. Para obter mais informações sobre este formato, consulte [Web vídeo texto controla formato](https://developer.mozilla.org/en-US/docs/Web/API/WebVTT_API).


## <a name="creating-the-human-in-the-loop-review"></a>Criar a revisão humanos no ciclo

O processo de moderação interrupção devolve uma lista de chaves fotogramas do vídeo, juntamente com um transcript do respetiva controla áudio. O passo seguinte é criar uma revisão na ferramenta de revisão de conteúdo Moderator para moderators humanos. Retroceder para o `ProcessVideo()` método `Program.cs`, consulte a chamada para o `CreateVideoReviewInContentModerator()` método. Este método não é o `videoReviewApi` classe, que está a ser `VideoReviewAPI.cs`e é apresentado aqui.

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

`CreateVideoReviewInContentModerator()` chama vários outros métodos para efetuar as seguintes tarefas:

> [!NOTE]
> A aplicação de consola utiliza o [FFmpeg](https://ffmpeg.org/) biblioteca para gerar miniaturas. Estes miniaturas (imagens) correspondem para os carimbos de moldura no [saída moderação de interrupção gráfica](#sample-video-moderation-response).

|Tarefa|Métodos|Ficheiro|
|-|-|-|
|Extrair a chave enquadra-se do vídeo e cria as imagens em miniatura dos mesmos|`CreateVideoFrames()`<br>`GenerateFrameImages()`|`FrameGeneratorServices.cs`|
|Analisar transcript texto, se estiver disponível, para localizar áudio racy ou para adultos|`GenerateTextScreenProfanity()`| `VideoReviewAPI.cs`|
|Preparar e submete um pedido de revisão de vídeo para inspecção humano|`CreateReviewRequestObject()`<br> `ExecuteCreateReviewApi()`<br>`CreateAndPublishReviewInContentModerator()`|`VideoReviewAPI.cs`|

## <a name="video-review-default-view"></a>Vista do vídeo de revisão predefinida

O ecrã seguinte mostra os resultados dos passos anteriores.

![Vista do vídeo de revisão predefinida](images/video-tutorial-default-view.PNG)

## <a name="transcript-generation"></a>Geração de transcript

Até agora, o código apresentado neste tutorial tem concentra-se no conteúdo visual. Revisão de conteúdo de reconhecimento de voz é um processo separado e opcional que, tal como mencionado, utiliza um transcript gerado a partir de áudio. Está agora na altura observe como transcrições de texto são criadas e utilizadas no processo de revisão. A tarefa de gerar o transcript recai o [indexador de suporte de dados do Azure](https://docs.microsoft.com/azure/media-services/media-services-index-content) serviço.

A aplicação executa as seguintes tarefas:

|Tarefa|Métodos|Ficheiro|
|-|-|-|
|Determinar se transcrições de texto são para ser gerado|`Main()`<br>`GetUserInputs()`|`Program.cs`|
|Se assim for, submeter uma tarefa de transcription como parte da moderação interrupção|`ConfigureTranscriptTask()`|`VideoModerator.cs`|
|Obter uma cópia local do transcript|`GenerateTranscript()`|`VideoModerator.cs`|
|Sinalizador frames do vídeo que contêm áudio inadequado|`GenerateTextScreenProfanity()`<br>`TextScreen()`|`VideoReviewAPI.cs`|
|Adicionar os resultados para a rever|`UploadScreenTextResult()`<br>`ExecuteAddTranscriptSupportFile()`|`VideoReviewAPI.cs`|

### <a name="task-configuration"></a>Configuração da tarefa

Vamos avançar diretamente para submeter a tarefa de transcription. `CreateAzureMediaServicesJobToModerateVideo()` chamadas (já descritas) `ConfigureTranscriptTask()`.

    private void ConfigureTranscriptTask(IJob job)
    {
        string mediaProcessorName = _amsConfigurations.MediaIndexer2MediaProcessor;
        IMediaProcessor processor = _mediaContext.MediaProcessors.GetLatestMediaProcessorByName(mediaProcessorName);

        string configuration = File.ReadAllText(_amsConfigurations.MediaIndexerConfigurationJson);
        ITask task = job.Tasks.AddNew("AudioIndexing Task", processor, configuration, TaskOptions.None);
        task.InputAssets.Add(asset);
        task.OutputAssets.AddNew("AudioIndexing Output Asset", AssetCreationOptions.None);
    }

A configuração para a tarefa de transcript é lida do ficheiro `MediaIndexerConfig.json` na solução de `Lib` pasta. Ativos do AMS são criados para o ficheiro de configuração e para a saída do processo de transcription. Quando é executada a tarefa de AMS, esta tarefa cria um transcript de texto a partir de controlar o ficheiro de vídeo de áudio.

> [!NOTE]
> A aplicação de exemplo reconhece voz apenas em inglês.

### <a name="transcript-generation"></a>Geração de transcript

O transcript for publicado como um recurso de AMS. Para analisar o transcript objectionable conteúdo, a aplicação transfere o elemento de Media Services do Azure. `CreateAzureMediaServicesJobToModerateVideo()` chamadas `GenerateTranscript()`, mostrados aqui, para obter o ficheiro.

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

Após alguns configuração AMS necessários, a transferência real é executada chamando `DownloadAssetToLocal()`, uma função genérica que copia um recurso de AMS para um ficheiro local.

## <a name="transcript-moderation"></a>Moderação de interrupção transcript

Com transcript fechar em execução, é analisado e utilizado na revisão. A criação de revisão é a alçada do `CreateVideoReviewInContentModerator()`, esse chamadas `GenerateTextScreenProfanity()` para efetuar a tarefa. Por sua vez, este método chama `TextScreen()`, que contém a maioria da funcionalidade. 

`TextScreen()` efetua as seguintes tarefas:

- Análise tamps transcript por período de tempo e legendas
- Submeter legenda de cada para moderação de interrupção de texto
- Sinalizador fotogramas que podem ter conteúdo de reconhecimento de voz objectionable

Vamos examinar cada uma destas tarefas mais detalhadamente:

### <a name="initialize-the-code"></a>Inicializar o código

Em primeiro lugar, inicializar todas as variáveis e coleções.

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
    

### <a name="parse-the-transcript-for-captions"></a>Analisar transcript para legendas

Em seguida, analisar o transcript formatado VTT para legendas e carimbos. A ferramenta de revisão apresenta estas legendas no separador Transcript no ecrã de revisão de vídeo. Os carimbos são utilizados para sincronizar legendas com os frames vídeos correspondentes.

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

### <a name="moderate-captions-with-the-text-moderation-service"></a>Legendas moderadas com o serviço de moderação interrupção de texto

Em seguida, vamos analisar legendas analisados texto com texto de conteúdo Moderator API.

> [!NOTE]
> A chave do serviço Moderator conteúdo tem um pedidos por segundo (RPS) para o limite de velocidade. Se exceder o limite, o SDK emite uma exceção com um código de 429 erro. 
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

### <a name="breaking-down-the-text-moderation-step"></a>Última hora para baixo o passo de moderação interrupção de texto

`TextScreen()` é um método significativo, por isso, vamos dividi-lo.

1. Em primeiro lugar, o método lê o ficheiro de transcript linha por linha. Ignora os linhas em branco e as linhas que contêm um `NOTE` com uma pontuação de confiança. Extrai os carimbos de data / hora e a itens de texto do *ajudas* no ficheiro. Um rectângulos representa o texto de controlar o áudio e incluem os tempos de início e de fim. Um rectângulos começam com a linha de carimbo de data / hora com a cadeia `-->`. É seguida por uma ou mais linhas de texto.

1. Instâncias de `CaptionScreentextResult` (definido no `TranscriptProfanity.cs`) são utilizados para conter as informações analisadas a partir do cada rectângulos.  Quando é detetada uma nova linha de carimbo de data / hora ou um comprimento de máximo de texto de 1024 carateres for atingido, um novo `CaptionScreentextResult` é adicionado ao `csrList`. 

1. O método submete junto a cada rectângulos para a API de moderação interrupção de texto. Chama ambos `ContentModeratorClient.TextModeration.DetectLanguageAsync()` e `ContentModeratorClient.TextModeration.ScreenTextWithHttpMessagesAsync()`, que são definidas no `Microsoft.Azure.CognitiveServices.ContentModerator` assemblagem. Para evitar a ser limitado de taxa, o método interrompe para um segundo antes de submeter cada rectângulos.

1. Depois de receber os resultados do serviço de moderação de interrupção de texto, o método, em seguida, analisa-as para ver se as mesmas cumprem os limiares de confiança. Estes valores são estabelecidos no `App.config` como `OffensiveTextThreshold`, `RacyTextThreshold`, e `AdultTextThreshold`. Por fim, os termos de licenciamento objectionable próprios também são armazenados. Todos os fotogramas dentro do intervalo de tempo a rectângulos sinalizadas como contendo ofensiva, racy e/ou texto para adultos.

1. `TextScreen()` Devolve um `TranscriptScreenTextResult` instância que contém o resultado de moderação interrupção de texto de vídeo como um todo. Este objeto inclui sinalizadores e pontuações para vários tipos de conteúdo objectionable, juntamente com uma lista de todos os objectionable termos. O chamador `CreateVideoReviewInContentModerator()`, chamadas `UploadScreenTextResult()` ligar estas informações para a revisão para que fique disponível para revisores humanos.
 
## <a name="video-review-transcript-view"></a>Vista de transcript revisão de vídeo

O ecrã seguinte mostra o resultado do transcript passos geração e moderação interrupção.

![Vista de transcript moderação de interrupção gráfica](images/video-tutorial-transcript-view.PNG)

## <a name="program-output"></a>Saída do programa

A seguinte saída da linha de comandos do programa mostra várias tarefas, como a serem resolvidos. Além disso, o resultado de moderação interrupção (no formato JSON) e o transcript de reconhecimento de voz estão disponíveis no mesmo diretório que os ficheiros de vídeos originais.

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

[Transferir a solução do Visual Studio](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) exemplo ficheiros e as bibliotecas necessárias para este tutorial e começar a utilizar a integração.
