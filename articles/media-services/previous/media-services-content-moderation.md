---
title: Utilizar Moderator de conteúdo do suporte de dados do Azure para detetar o conteúdo para adultos e racy possíveis | Microsoft Docs
description: Moderação de interrupção gráfica ajuda a detetar potencial conteúdo para adultos e racy em vídeos.
services: media-services
documentationcenter: ''
author: sanjeev3
manager: mikemcca
editor: ''
ms.assetid: a245529f-3150-4afc-93ec-e40d8a6b761d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/06/2018
ms.author: sajagtap
ms.openlocfilehash: e44308f38a138c0e186e41fc8310f8b480cd4e09
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788670"
---
# <a name="use-azure-media-content-moderator-to-detect-possible-adult-and-racy-content"></a>Utilizar Moderator de conteúdo do suporte de dados do Azure para detetar o conteúdo para adultos e racy possíveis

## <a name="overview"></a>Descrição geral
O **Moderator conteúdo do suporte de dados do Azure** processador de multimédia (MP) permite-lhe utilizar a moderação de interrupção assistida por máquina para os seus vídeos. Por exemplo, pode querer detetar possíveis conteúdo para adultos e racy no vídeos e consultar o conteúdo sinalizado, suas equipas de moderação de interrupção humanos.

O **Moderator conteúdo do suporte de dados do Azure** MP está atualmente em pré-visualização.

Este artigo fornece detalhes sobre **Moderator conteúdo do suporte de dados do Azure** e mostra como utilizá-la com o SDK de Media Services para .NET.

## <a name="content-moderator-input-files"></a>Os ficheiros de entrada Moderator de conteúdo
Ficheiros de vídeo. Atualmente, são suportados os seguintes formatos: MP4, MOV e WMV.

## <a name="content-moderator-output-files"></a>Ficheiros de saída do conteúdo Moderator
A saída moderated no formato JSON inclui capturas de detecção automática e keyframes. Os keyframes são devolvidos com pontuações de confiança para o conteúdo para adultos ou racy possíveis. Também incluem um sinalizador de Booleano indicando se uma revisão é recomendada. O sinalizador de recomendação de revisão atribuído com base nos limiares internos para adultos e racy pontuações de valores.

## <a name="elements-of-the-output-json-file"></a>Elementos do ficheiro de saída JSON

A tarefa produz um ficheiro de saída JSON que contém metadados sobre capturas detetadas e keyframes e se contêm conteúdo para adultos ou racy.

A saída JSON inclui os seguintes elementos:

### <a name="root-json-elements"></a>Elementos raiz JSON

| Elemento | Descrição |
| --- | --- |
| versão |A versão do conteúdo Moderator. |
| escala temporal |"Ticks" por segundo do vídeo. |
| deslocamento |O desvio da hora para carimbos de hora. Na versão 1.0 do vídeo APIs, este valor será sempre 0. Este valor podem ser alterados no futuro. |
| framerate |Fotogramas por segundo do vídeo. |
| Largura |A largura da moldura vídeo saída, em pixels.|
| Altura |A altura da moldura vídeo saída, em pixels.|
| TotalDuration |A duração da entrada vídeo, no "ticks". |
| [fragments](#fragments-json-elements) |Os metadados é partes cópias de segurança em diferentes segmentos chamados fragmentos. Cada fragmento é uma captura automaticamente detetados com um início, a duração, o número do intervalo e o evento (s). |

### <a name="fragments-json-elements"></a>Os fragmentos de elementos JSON

|Elemento|Descrição|
|---|---|
| start |A hora de início do primeiro evento na "ticks". |
| Duração |O comprimento do fragmento, no "ticks". |
| intervalo |O intervalo de cada entrada de evento dentro de fragmento, no "ticks". |
| [Eventos](#events-json-elements) |Cada evento representa uma clip e cada clip contém keyframes detetados e monitorizados dentro desse duração de tempo. É uma matriz de eventos. A matriz externa representa um intervalo de tempo. A matriz interna é constituído por 0 ou mais eventos que ocorreram neste ponto no tempo.|

### <a name="events-json-elements"></a>Elementos JSON de eventos

|Elemento|Descrição|
|---|---|
| reviewRecommended | `true` ou `false` consoante esteja a **adultScore** ou **racyScore** exceder os limiares internos. |
| adultScore | Classificação de confiança para o conteúdo para adultos possíveis, numa escala de 0.00 para 0,99. |
| racyScore | Classificação de confiança para conteúdo racy possíveis, numa escala de 0.00 para 0,99. |
| índice | o índice da moldura numa escala de moldura primeiro para o último índice índice da moldura. |
| carimbo de data/hora | A localização da moldura no "ticks". |
| shotIndex | O índice de captura principal. |


## <a name="content-moderation-quickstart-and-sample-output"></a>Saída de início rápido e exemplos de moderação interrupção de conteúdo

### <a name="task-configuration-preset"></a>Configuração da tarefa (predefinição)
Quando criar uma tarefa com **Moderator conteúdo do suporte de dados do Azure**, tem de especificar uma predefinição de configuração. A predefinição de configuração seguinte é apenas para conteúdo moderação de interrupção.

    {
      "version":"2.0"
    }

### <a name="net-code-sample"></a>Exemplo de código do .NET

O seguinte exemplo de código do .NET utiliza o SDK de Media Services .NET para executar uma tarefa de Moderator conteúdo. Demora um suporte de dados dos serviços de recurso como entrada que contém o vídeo para ser moderated.
Consulte o [início rápido de vídeo Moderator conteúdo](../../cognitive-services/Content-Moderator/video-moderation-api.md) para o código de origem completo e o projeto do Visual Studio.


```csharp
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

For the full source code and the Visual Studio project, check out the [Content Moderator video quickstart](../../cognitive-services/Content-Moderator/video-moderation-api.md).

### JSON output

The following example of a Content Moderator JSON output was truncated.

> [!NOTE]
> Location of a keyframe in seconds = timestamp/timescale

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
```

## <a name="media-services-learning-paths"></a>Percursos de aprendizagem dos Media Services
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-links"></a>Ligações relacionadas
[Descrição geral da análise de serviços de multimédia do Azure](media-services-analytics-overview.md)

[Demonstrações de análise de multimédia do Azure](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o conteúdo Moderator [vídeo solução moderação interrupção e reveja](../../cognitive-services/Content-Moderator/video-moderation-human-review.md).

Obter o código de origem completo e o projeto do Visual Studio do [início rápido de moderação de interrupção gráfica](../../cognitive-services/Content-Moderator/video-moderation-api.md). 

Saiba como gerar [vídeo revê](../../cognitive-services/Content-Moderator/video-reviews-quickstart-dotnet.md) do resultado moderated, e [moderada transcrições](../../cognitive-services/Content-Moderator/video-transcript-reviews-quickstart-dotnet.md) no .NET.

Verificar o .NET detalhadas [vídeo tutorial moderação interrupção e reveja](../../cognitive-services/Content-Moderator/video-transcript-moderation-review-tutorial-dotnet.md). 
