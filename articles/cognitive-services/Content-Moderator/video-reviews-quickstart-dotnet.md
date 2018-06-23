---
title: Moderator conteúdo do Azure - criar as vídeos revisões através do .NET | Microsoft Docs
description: Como criar as vídeo revê utilizando o SDK de Moderator conteúdo do Azure para .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/18/2018
ms.author: sajagtap
ms.openlocfilehash: cb487314b8695f3676fdb22a9d7e3ec5ca3ed9f2
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351877"
---
# <a name="create-video-reviews-using-net"></a>Criar as vídeos revisões através do .NET

Este artigo fornece informações e exemplos de código para o ajudar a rapidamente começar a utilizar o SDK Moderator conteúdo com c# para:

- Criar uma revisão de vídeo para moderators humanos
- Adicionar frames para uma revisão
- Obter os frames para a revisão 
- Obter o estado e os detalhes de revisão
- Publicar a revisão

## <a name="prerequisites"></a>Pré-requisitos

Este artigo pressupõe que tem [moderated o vídeo (consulte o guia de introdução)](video-moderation-api.md) e tem os dados da resposta. Precisa para criar com base no período de revisões para moderators humanos.

Este artigo também assume que já estiver familiarizado com o Visual Studio e c#.

### <a name="sign-up-for-content-moderator-services"></a>Inscreva-se os serviços de Moderator conteúdo

Antes de poder utilizar os serviços de Moderator conteúdo através da API REST ou o SDK, precisa de uma chave de subscrição.

No Dashboard do conteúdo Moderator, pode encontrar a chave de subscrição no **definições** > **credenciais** > **API**  >  **Avaliação Ocp-Apim-Subscription-Key**. Para obter mais informações, consulte [descrição geral](overview.md).

### <a name="prepare-your-video-and-the-video-frames-for-review"></a>Preparar o vídeo e as vídeos frames para revisão

As vídeos e exemplo frames vídeos para rever tem de ser publicados online porque terá dos URLs.

> [!NOTE]
> O programa utiliza manualmente guardadas capturas de ecrã da vídeo com pontuações adulto racy aleatórias para ilustrar a utilização da API de revisão. Numa situação de mundo real, utilize o [saída moderação de interrupção gráfica](video-moderation-api.md#run-the-program-and-review-the-output) para criar imagens e atribuir pontuações. 

Para o vídeo, precisa de um ponto final de transmissão em fluxo para que a ferramenta de revisão desempenha o vídeo na vista de leitor.

![Miniatura de vídeo de demonstração](images/ams-video-demo-view.PNG)

- Copiar o **URL** neste [demonstração de Media Services do Azure](https://aka.ms/azuremediaplayer?url=https%3A%2F%2Famssamples.streaming.mediaservices.windows.net%2F91492735-c523-432b-ba01-faba6c2206a2%2FAzureMediaServicesPromo.ism%2Fmanifest) página para o URL do manifesto.

Para as vídeos frames (imagens), utilize as seguintes imagens:

![Miniatura de moldura vídeo 1](images/ams-video-frame-thumbnails-1.PNG) | ![Miniatura de moldura vídeo 2](images/ams-video-frame-thumbnails-2.PNG) | ![Miniatura de moldura vídeo 3](images/ams-video-frame-thumbnails-3.PNG) |
| :---: | :---: | :---: |
[Intervalo de 1](https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame1-00-17.PNG) | [Período de 2](https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-2-01-04.PNG) | [Intervalo de 3](https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-3-02-24.PNG) |

## <a name="create-your-visual-studio-project"></a>Criar o projeto do Visual Studio

1. Adicione um novo **aplicação de consola (.NET Framework)** projeto para a sua solução.

1. Nomeie o projeto **VideoReviews**.

1. Selecione este projeto como o projeto de arranque único para a solução.

### <a name="install-required-packages"></a>Instalar pacotes necessários

Instale os seguintes pacotes de NuGet para o projeto de TermLists.

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Microsoft.Rest.ClientRuntime.Azure
- Newtonsoft

### <a name="update-the-programs-using-statements"></a>O programa de atualização do instruções de utilização

Modificar o programa de instruções de utilização da seguinte forma.

    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;
    using Microsoft.Azure.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using Newtonsoft.Json;


### <a name="add-private-properties"></a>Adicionar propriedades privadas

Adicione as seguintes propriedades privadas ao espaço de nomes VideoReviews, classe de programa.

Sempre que for indicado, substitua os valores de exemplo para estas propriedades.


    namespace VideoReviews
    {
        class Program
        {
            // NOTE: Replace this example location with the location for your Content Moderator account.
            /// <summary>
            /// The region/location for your Content Moderator account, 
            /// for example, westus.
            /// </summary>
            private static readonly string AzureRegion = "YOUR CONTENT MODERATOR REGION";

            // NOTE: Replace this example key with a valid subscription key.
            /// <summary>
            /// Your Content Moderator subscription key.
            /// </summary>
            private static readonly string CMSubscriptionKey = "YOUR CONTENT MODERATOR KEY";

            // NOTE: Replace this example team name with your Content Moderator team name.
            /// <summary>
            /// The name of the team to assign the job to.
            /// </summary>
            /// <remarks>This must be the team name you used to create your 
            /// Content Moderator account. You can retrieve your team name from
            /// the Conent Moderator web site. Your team name is the Id associated 
            /// with your subscription.</remarks>
            public static readonly string TeamName = "YOUR CONTENT MODERATOR TEAM ID";

            /// <summary>
            /// The base URL fragment for Content Moderator calls.
            /// </summary>
            private static readonly string AzureBaseURL =
                $"{AzureRegion}.api.cognitive.microsoft.com";

            /// <summary>
            /// The minimum amount of time, in milliseconds, to wait between calls
            /// to the Content Moderator APIs.
            /// </summary>
            private const int throttleRate = 2000;


### <a name="create-content-moderator-client-object"></a>Criar o objeto de cliente de Moderator conteúdo

Adicione a seguinte definição de método ao espaço de nomes VideoReviews, classe de programa.

    /// <summary>
    /// Returns a new Content Moderator client for your subscription.
    /// </summary>
    /// <returns>The new client.</returns>
    /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
    /// When you have finished using the client,
    /// you should dispose of it either directly or indirectly. </remarks>
    public static ContentModeratorClient NewClient()
    {
        return new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey))
        {
            BaseUrl = AzureBaseURL
        };
    }

## <a name="create-a-video-review"></a>Criar uma revisão de vídeo

Criar uma revisão de vídeo com **ContentModeratorClient.Reviews.CreateVideoReviews**. Para obter mais informações, consulte o [referência da API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4).

**CreateVideoReviews** seguintes requer parâmetros:
1. Uma cadeia que contém um tipo MIME, que deve ser "application/json". 
1. O nome do agrupamento de conteúdo Moderator.
1. Um **IList<CreateVideoReviewsBodyItem>**  objeto. Cada **CreateVideoReviewsBodyItem** objeto representa uma revisão de vídeo. Este guia de introdução cria uma revisão cada vez.

**CreateVideoReviewsBodyItem** tem várias propriedades. No mínimo, defina as seguintes propriedades:
- **Conteúdo**. O URL do vídeo para ser revisto.
- **ContentId**. Um ID para atribuir a rever a vídeo.
- **Estado**. Definir o valor para "Unpublished." Se não pode defini-la, assume como "Pendentes", que significa que a revisão vídeo foi publicado e pendente revisão humano. Depois de uma revisão de vídeo for publicada, já não pode adicionar frames vídeos, um transcript ou um resultado de moderação interrupção transcript ao mesmo.

> [!NOTE]
> **CreateVideoReviews** devolve uma IList<string>. Cada um destas cadeias contém um ID para uma revisão de vídeo. Estes IDs são GUIDs e não são os mesmos que o valor da **ContentId** propriedade. 

Adicione a seguinte definição de método ao espaço de nomes VideoReviews, classe de programa.

    /// <summary>
    /// Create a video review. For more information, see the API reference:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4 
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="id">The ID to assign to the video review.</param>
    /// <param name="content">The URL of the video to review.</param>
    /// <returns>The ID of the video review.</returns>
    private static string CreateReview(ContentModeratorClient client, string id, string content)
    {
        Console.WriteLine("Creating a video review.");

        List<CreateVideoReviewsBodyItem> body = new List<CreateVideoReviewsBodyItem>() {
            new CreateVideoReviewsBodyItem
            {
                Content = content,
                ContentId = id,
                /* Note: to create a published review, set the Status to "Pending".
                However, you cannot add video frames or a transcript to a published review. */
                Status = "Unpublished",
            }
        };

        var result = client.Reviews.CreateVideoReviews("application/json", TeamName, body);

        Thread.Sleep(throttleRate);

        // We created only one review.
        return result[0];
    }

> [!NOTE]
> A chave do serviço Moderator conteúdo tem um pedidos por segundo (RPS) para o limite de velocidade e, se for excedido o limite, o SDK emite uma exceção com um código de 429 erro. 
>
> Uma chave de escalão gratuito tem um limite de taxa de um RPS.

## <a name="add-video-frames-to-the-video-review"></a>Adicionar as vídeos frames para rever a vídeo

Adicionar frames vídeos para uma revisão de vídeo com **ContentModeratorClient.Reviews.AddVideoFrameUrl** (se o seu vídeos frames alojadas online) ou **ContentModeratorClient.Reviews.AddVideoFrameStream** ( Se estiverem alojados as vídeos frames localmente). Este guia de introdução pressupõe que as vídeos frames alojadas online e, por isso, utiliza **AddVideoFrameUrl**. Para obter mais informações, consulte o [referência da API](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b76ae7151f0b10d451fd).

**AddVideoFrameUrl** seguintes requer parâmetros:
1. Uma cadeia que contém um tipo MIME, que deve ser "application/json".
1. O nome do agrupamento de conteúdo Moderator.
1. O ID de vídeo revisão devolvido por **CreateVideoReviews**.
1. Um **IList<VideoFrameBodyItem>**  objeto. Cada **VideoFrameBodyItem** objeto representa um período de vídeo.

**VideoFrameBodyItem** tem as seguintes propriedades:
- **Timestamp**. Uma cadeia que contém, em segundos, a hora a partir da qual a vídeo moldura foi tomada de vídeo.
- **FrameImage**. O URL da moldura de vídeo.
- **Metadados**. Uma IList<VideoFrameBodyItemMetadataItem>. **VideoFrameBodyItemMetadataItem** é simplesmente um par chave/valor. Chaves válidas incluem:
- **reviewRecommended**. TRUE se recomenda uma revisão humana da moldura vídeo.
- **adultScore**. Um valor entre 0 e 1 que classifica a gravidade do conteúdo para adultos na moldura de vídeo.
- **um**. TRUE se o vídeo contém conteúdo para adultos.
- **racyScore**. Um valor entre 0 e 1 que classifica a gravidade do conteúdo racy na moldura de vídeo.
- **r**. TRUE se o intervalo de vídeo contém conteúdo racy.
- **ReviewerResultTags**. Uma IList<VideoFrameBodyItemReviewerResultTagsItem>. **VideoFrameBodyItemReviewerResultTagsItem** é simplesmente um par chave/valor. Uma aplicação pode utilizar estas etiquetas para organizar as vídeos fotogramas.

> [!NOTE]
> Este guia de introdução gera valores aleatórios para o **adultScore** e **racyScore** propriedades. Numa aplicação de produção, pode obter estes valores no [serviço moderação de interrupção gráfica](video-moderation-api.md), implementado como um serviço de suporte de dados do Azure.

Adicione as seguintes definições de método ao espaço de nomes VideoReviews, classe de programa.

    <summary>
    /// Create a video frame to add to a video review after the video review is created.
    /// </summary>
    /// <param name="url">The URL of the video frame image.</param>
    /// <returns>The video frame.</returns>
    private static VideoFrameBodyItem CreateFrameToAddToReview(string url, string timestamp_seconds)
    {
        // We generate random "adult" and "racy" scores for the video frame.
        Random rand = new Random();

        var frame = new VideoFrameBodyItem
        {
            // The timestamp is measured in milliseconds. Convert from seconds.
            Timestamp = (int.Parse(timestamp_seconds) * 1000).ToString(),
            FrameImage = url,

            Metadata = new List<VideoFrameBodyItemMetadataItem>
            {
                new VideoFrameBodyItemMetadataItem("reviewRecommended", "true"),
                new VideoFrameBodyItemMetadataItem("adultScore", rand.NextDouble().ToString()),
                new VideoFrameBodyItemMetadataItem("a", "false"),
                new VideoFrameBodyItemMetadataItem("racyScore", rand.NextDouble().ToString()),
                new VideoFrameBodyItemMetadataItem("r", "false")
            },

            ReviewerResultTags = new List<VideoFrameBodyItemReviewerResultTagsItem>()
            {
                new VideoFrameBodyItemReviewerResultTagsItem("tag1", "value1")
            }
        };

        return frame;
    }

    /// <summary>
    /// Add a video frame to the indicated video review. For more information, see the API reference:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b76ae7151f0b10d451fd
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="review_id">The video review ID.</param>
    /// <param name="url">The URL of the video frame image.</param>
    static void AddFrame(ContentModeratorClient client, string review_id, string url, string timestamp_seconds)
    {
        Console.WriteLine("Adding a frame to the review with ID {0}.", review_id);

        var frames = new List<VideoFrameBodyItem>()
        {
            CreateFrameToAddToReview(url, timestamp_seconds)
        };
            
        client.Reviews.AddVideoFrameUrl("application/json", TeamName, review_id, frames);

        Thread.Sleep(throttleRate);
    

## <a name="get-video-frames-for-video-review"></a>Obter frames vídeos para revisão de vídeo

Pode obter as vídeos frames para uma revisão de vídeo com **ContentModeratorClient.Reviews.GetVideoFrames**. **GetVideoFrames** seguintes requer parâmetros:
1. O nome do agrupamento de conteúdo Moderator.
1. O ID de vídeo revisão devolvido por **CreateVideoReviews**.
1. O índice baseado em zero da moldura vídeo primeiro a obter.
1. O número de fotogramas vídeos para obter.

Adicione a seguinte definição de método ao espaço de nomes VideoReviews, classe de programa.

    /// <summary>
    /// Get the video frames assigned to the indicated video review.  For more information, see the API reference:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7ba43e7151f0b10d45200
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="review_id">The video review ID.</param>
    static void GetFrames(ContentModeratorClient client, string review_id)
    {
        Console.WriteLine("Getting frames for the review with ID {0}.", review_id);

        Frames result = client.Reviews.GetVideoFrames(TeamName, review_id, 0, Int32.MaxValue);
        Console.WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));

        Thread.Sleep(throttleRate);
    }

## <a name="get-video-review-information"></a>Obter informações de revisão de vídeo

Obter informações para uma revisão de vídeo com **ContentModeratorClient.Reviews.GetReview**. **GetReview** seguintes requer parâmetros:
1. O nome do agrupamento de conteúdo Moderator.
1. O ID de vídeo revisão devolvido por **CreateVideoReviews**.

Adicione a seguinte definição de método ao espaço de nomes VideoReviews, classe de programa.

    /// <summary>
    /// Get the information for the indicated video review. For more information, see the reference API:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="review_id">The video review ID.</param>
    private static void GetReview(ContentModeratorClient client, string review_id)
    {
        Console.WriteLine("Getting the status for the review with ID {0}.", review_id);

        var result = client.Reviews.GetReview(ModeratorHelper.Clients.TeamName, review_id);
        Console.WriteLine(JsonConvert.SerializeObject(result, Formatting.Indented));

        Thread.Sleep(throttleRate);
    }

## <a name="publish-video-review"></a>Publicar vídeo revisão

Publicar uma revisão de vídeo com **ContentModeratorClient.Reviews.PublishVideoReview**. **PublishVideoReview** seguintes requer parâmetros:
1. O nome do agrupamento de conteúdo Moderator.
1. O ID de vídeo revisão devolvido por **CreateVideoReviews**.

Adicione a seguinte definição de método ao espaço de nomes VideoReviews, classe de programa.

    /// <summary>
    /// Publish the indicated video review. For more information, see the reference API:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7bb29e7151f0b10d45201
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="review_id">The video review ID.</param>
    private static void PublishReview(ContentModeratorClient client, string review_id)
    {
        Console.WriteLine("Publishing the review with ID {0}.", review_id);
        client.Reviews.PublishVideoReview(TeamName, review_id);
        Thread.Sleep(throttleRate);
    }

## <a name="putting-it-all-together"></a>Passar todos os em conjunto

Adicionar o **Main** definição de método ao espaço de nomes VideoReviews, classe de programa. Por fim, feche a classe de programa e o espaço de nomes VideoReviews.

    static void Main(string[] args)
    {
        using (ContentModeratorClient client = NewClient())
        {
            // Create a review with the content pointing to a streaming endpoint (manifest)
            var streamingcontent = "https://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest";
            string review_id = CreateReview(client, "review1", streamingcontent);

            var frame1_url = "https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame1-00-17.PNG";
            var frame2_url = "https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-2-01-04.PNG";
            var frame3_url = "https://blobthebuilder.blob.core.windows.net/sampleframes/ams-video-frame-3-02-24.PNG";

            // Add the frames from 17, 64, and 144 seconds.
            AddFrame(client, review_id, frame1_url, "17");
            AddFrame(client, review_id, frame2_url, "64");
            AddFrame(client, review_id, frame3_url, "144");

            // Get frames information and show
            GetFrames(client, review_id);
            GetReview(client, review_id);

            // Publish the review
            PublishReview(client, review_id);

            Console.WriteLine("Open your Content Moderator Dashboard and select Review > Video to see the review.");
            Console.WriteLine("Press any key to close the application.");
            Console.Read();
        }
    }

## <a name="run-the-program-and-review-the-output"></a>Execute o programa e reveja o resultado
Quando executar a aplicação, ver um resultado nas seguintes linhas:

    Creating a video review.
    Adding a frame to the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
    Adding a frame to the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
    Adding a frame to the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
    Getting frames for the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
    {
        "ReviewId": "201801v3212bda70ced4928b2cd7459c290c7dc",
        "VideoFrames": [
        {
            "Timestamp": "17000",
            "FrameImage": "https://reviewcontentprod.blob.core.windows.net/testreview6/FRM_201801v3212bda70ced4928b2cd7459c290c7dc_17000.PNG",
            "Metadata": [
            {
                "Key": "reviewRecommended",
                "Value": "true"
            },
            {
                "Key": "adultScore",
                "Value": "0.808312381528463"
            },
            {
                "Key": "a",
                "Value": "false"
            },
            {
                "Key": "racyScore",
                "Value": "0.846378884206702"
            },
            {
                "Key": "r",
                "Value": "false"
            }
            ],
            "ReviewerResultTags": [
            {
                "Key": "tag1",
                "Value": "value1"
            }
        ]
        },
        {
            "Timestamp": "64000",
            "FrameImage": "https://reviewcontentprod.blob.core.windows.net/testreview6/FRM_201801v3212bda70ced4928b2cd7459c290c7dc_64000.PNG",
            "Metadata": [
            {
                "Key": "reviewRecommended",
                "Value": "true"
            },
            {
                "Key": "adultScore",
                "Value": "0.576078300166912"
            },
            {
                "Key": "a",
                "Value": "false"
            },
            {
                "Key": "racyScore",
                "Value": "0.244768953064815"
            },
            {
                "Key": "r",
                "Value": "false"
            }
            ],
            "ReviewerResultTags": [
            {
                "Key": "tag1",
                "Value": "value1"
            }
        ]
        },
        {
            "Timestamp": "144000",
            "FrameImage": "https://reviewcontentprod.blob.core.windows.net/testreview6/FRM_201801v3212bda70ced4928b2cd7459c290c7dc_144000.PNG",
            "Metadata": [
            {
                "Key": "reviewRecommended",
                "Value": "true"
            },
            {
                "Key": "adultScore",
                "Value": "0.664480847150311"
            },
            {
                "Key": "a",
                "Value": "false"
            },
            {
                "Key": "racyScore",
                "Value": "0.933817870418456"
            },
            {
                "Key": "r",
                "Value": "false"
            }
            ],
            "ReviewerResultTags": [
            {
                "Key": "tag1",
                "Value": "value1"
            }
            ]
        }
        ]
    }
    
    Getting the status for the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
    {
        "ReviewId": "201801v3212bda70ced4928b2cd7459c290c7dc",
        "SubTeam": "public",
        "Status": "UnPublished",
        "ReviewerResultTags": [],
        "CreatedBy": "testreview6",
        "Metadata": [
        {
            "Key": "FrameCount",
            "Value": "3"
        }
        ],
        "Type": "Video",
        "Content": "https://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest",
        "ContentId": "review1",
        "CallbackEndpoint": null
    }

    Publishing the review with ID 201801v3212bda70ced4928b2cd7459c290c7dc.
    Open your Content Moderator Dashboard and select Review > Video to see the review.
    Press any key to close the application.

## <a name="check-out-your-video-review"></a>Veja o vídeo revisão

Por fim, pode ver o vídeo revisão no seu conteúdo Moderator rever conta da ferramenta do **rever**>**vídeo** ecrã.

![Revisão de vídeo para moderators humanos](images/ams-video-review.PNG)

## <a name="next-steps"></a>Passos Seguintes

Saiba como adicionar [moderação de interrupção transcript](video-transcript-moderation-review-tutorial-dotnet.md) para rever a vídeo. 

Veja o tutorial detalhado sobre como desenvolver uma [concluir solução moderação de interrupção gráfica](video-transcript-moderation-review-tutorial-dotnet.md).

[Transferir a solução do Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para esta e outros inícios rápidos de Moderator conteúdo para o .NET.
