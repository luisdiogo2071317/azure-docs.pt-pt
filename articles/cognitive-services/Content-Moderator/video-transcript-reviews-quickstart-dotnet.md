---
title: Moderator conteúdo do Azure - criar revisões de vídeo transcript através do .NET | Microsoft Docs
description: Como criar as vídeo transcript revê utilizando o SDK de Moderator conteúdo do Azure para .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/19/2018
ms.author: sajagtap
ms.openlocfilehash: 3286da6e38f0fba02386d877a835fb694ed0fdec
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352448"
---
# <a name="create-video-transcript-reviews-using-net"></a>Criar revisões de vídeo transcript através do .NET

Este artigo fornece informações e exemplos de código para o ajudar a rapidamente começar a utilizar o SDK Moderator conteúdo com c# para:

- Criar uma revisão de vídeo para moderators humanos
- Adicione um transcript moderated de revisão
- Publicar a revisão

## <a name="prerequisites"></a>Pré-requisitos

Este artigo pressupõe que tem [moderated o vídeo](video-moderation-api.md) e [criado a vídeo revisão](video-reviews-quickstart-dotnet.md) na ferramenta de revisão para fazer de decisão humanos. Agora pretende adicionar transcrições de vídeos moderated na ferramenta de revisão.

Este artigo também assume que já estiver familiarizado com o Visual Studio e c#.

### <a name="sign-up-for-content-moderator-services"></a>Inscreva-se os serviços de Moderator conteúdo

Antes de poder utilizar os serviços de Moderator conteúdo através da API REST ou o SDK, precisa de uma chave de subscrição.

No Dashboard do conteúdo Moderator, encontrará a sua chave de subscrição no **definições** > **credenciais** > **API**  >   **Avaliação Ocp-Apim-Subscription-Key**. Para obter mais informações, consulte [descrição geral](overview.md).

### <a name="prepare-your-video-for-review"></a>Preparar o vídeo para revisão

Adicione o transcript para uma revisão de vídeo. O vídeo tem de ser publicado online. É necessário o seu ponto final de transmissão em fluxo. O ponto final de transmissão em fluxo permite que o leitor de vídeo de ferramenta de revisão reproduzir o vídeo.

![Miniatura de vídeo de demonstração](images/ams-video-demo-view.PNG)

- Copiar o **URL** neste [demonstração de Media Services do Azure](https://aka.ms/azuremediaplayer?url=https%3A%2F%2Famssamples.streaming.mediaservices.windows.net%2F91492735-c523-432b-ba01-faba6c2206a2%2FAzureMediaServicesPromo.ism%2Fmanifest) página para o URL do manifesto.

## <a name="create-your-visual-studio-project"></a>Criar o projeto do Visual Studio

1. Adicione um novo **aplicação de consola (.NET Framework)** projeto para a sua solução.

1. Nomeie o projeto **VideoTranscriptReviews**.

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

Adicione as seguintes propriedades privadas ao espaço de nomes VideoTranscriptReviews, classe de programa.

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

Adicione a seguinte definição de método ao espaço de nomes VideoTranscriptReviews, classe de programa.

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
> A chave do serviço Moderator conteúdo tem um pedidos por segundo (RPS) para o limite de velocidade. Se exceder o limite, o SDK emite uma exceção com um código de 429 erro. 
>
> Uma chave de escalão gratuito tem um limite de taxa de um RPS.

## <a name="add-transcript-to-video-review"></a>Adicionar transcript a revisão de vídeo

Adicionar um transcript para uma revisão de vídeo com **ContentModeratorClient.Reviews.AddVideoTranscript**. **AddVideoTranscript** seguintes requer parâmetros:
1. A equipa de conteúdos Moderator ID.
1. O ID de vídeo revisão devolvido por **CreateVideoReviews**.
1. A **fluxo** objeto que contém o transcript.

O transcript tem de estar no formato WebVTT. Para obter mais informações, consulte [WebVTT: O Web vídeo controla o formato de texto](https://www.w3.org/TR/webvtt1/).

> [!NOTE]
> O programa que utiliza um transcript de exemplo no formato VTT. Uma solução do mundo real, pode utiliza o serviço de indexador de suporte de dados do Azure para [gerar um transcript](https://docs.microsoft.com/azure/media-services/media-services-index-content) de um vídeo.

Adicione a seguinte definição de método ao espaço de nomes VideotranscriptReviews, classe de programa.

    /// <summary>
    /// Add a transcript to the indicated video review.
    /// The transcript must be in the WebVTT format.
    /// For more information, see the API reference:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b8b2e7151f0b10d451fe
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="review_id">The video review ID.</param>
    /// <param name="transcript">The video transcript.</param>
    static void AddTranscript(ContentModeratorClient client, string review_id, string transcript)
    {
        Console.WriteLine("Adding a transcript to the review with ID {0}.", review_id);
        client.Reviews.AddVideoTranscript(TeamName, review_id, new MemoryStream(System.Text.Encoding.UTF8.GetBytes(transcript)));
        Thread.Sleep(throttleRate);
    }

## <a name="add-a-transcript-moderation-result-to-video-review"></a>Adicionar um resultado de moderação interrupção transcript a revisão de vídeo

Para além de adicionar um transcript para uma revisão de vídeo, adicione também o resultado da moderating esse transcript. Fazê-lo com **ContentModeratorClient.Reviews.AddVideoTranscriptModerationResult**. Para obter mais informações, consulte o [referência da API](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b93ce7151f0b10d451ff).

**AddVideoTranscriptModerationResult** seguintes requer parâmetros:
1. Uma cadeia que contém um tipo MIME, que deve ser "application/json". 
1. O nome do agrupamento de conteúdo Moderator.
1. O ID de vídeo revisão devolvido por **CreateVideoReviews**.
1. Uma IList<TranscriptModerationBodyItem>. A **TranscriptModerationBodyItem** tem as seguintes propriedades:
- **Termos**. Uma IList<TranscriptModerationBodyItemTermsItem>. A **TranscriptModerationBodyItemTermsItem** tem as seguintes propriedades:
- **Índice**. O índice baseado em zero do termo.
- **Termo**. Uma cadeia que contém o termo.
- **Timestamp**. Uma cadeia que contém, em segundos, o tempo em transcript que se encontram os termos de licenciamento.

O transcript tem de estar no formato WebVTT. Para obter mais informações, consulte [WebVTT: O Web vídeo controla o formato de texto](https://www.w3.org/TR/webvtt1/).

Adicione a seguinte definição de método ao espaço de nomes VideoTranscriptReviews, classe de programa. Este método envia uma transcript para o **ContentModeratorClient.TextModeration.ScreenText** método. Também tenha traduz o resultado para uma IList<TranscriptModerationBodyItem>e envia para **AddVideoTranscriptModerationResult**.

    /// <summary>
    /// Add the results of moderating a video transcript to the indicated video review.
    /// For more information, see the API reference:
    /// https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/59e7b93ce7151f0b10d451ff
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="review_id">The video review ID.</param>
    /// <param name="transcript">The video transcript.</param>
    static void AddTranscriptModerationResult(ContentModeratorClient client, string review_id, string transcript)
    {
        Console.WriteLine("Adding a transcript moderation result to the review with ID {0}.", review_id);

        // Screen the transcript using the Text Moderation API. For more information, see:
        // https://westus2.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f
        Screen screen = client.TextModeration.ScreenText("eng", "text/plain", transcript);

        // Map the term list returned by ScreenText into a term list we can pass to AddVideoTranscriptModerationResult.
        List<TranscriptModerationBodyItemTermsItem> terms = new List<TranscriptModerationBodyItemTermsItem>();
        if (null != screen.Terms)
        {
            foreach (var term in screen.Terms)
            {
                if (term.Index.HasValue)
                {
                    terms.Add(new TranscriptModerationBodyItemTermsItem(term.Index.Value, term.Term));
                }
            }
        }

        List<TranscriptModerationBodyItem> body = new List<TranscriptModerationBodyItem>()
        {
            new TranscriptModerationBodyItem()
            {
                Timestamp = "0",
                Terms = terms
            }
        };

        client.Reviews.AddVideoTranscriptModerationResult("application/json", TeamName, review_id, body);

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

Adicionar o **Main** definição de método ao espaço de nomes VideoTranscriptReviews, classe de programa. Por fim, feche a classe de programa e o espaço de nomes VideoTranscriptReviews.

> [!NOTE]
> O programa que utiliza um transcript de exemplo no formato VTT. Uma solução do mundo real, pode utiliza o serviço de indexador de suporte de dados do Azure para [gerar um transcript](https://docs.microsoft.com/azure/media-services/media-services-index-content) de um vídeo. 

    static void Main(string[] args)
    {
        using (ContentModeratorClient client = NewClient())
        {
            // Create a review with the content pointing to a streaming endpoint (manifest)
            var streamingcontent = "https://amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest";
            string review_id = CreateReview(client, "review1", streamingcontent);

            var transcript = @"WEBVTT

            01:01.000 --> 02:02.000
            First line with a crap word in a transcript.

            02:03.000 --> 02:25.000
            This is another line in the transcript.
            ";

            AddTranscript(client, review_id, transcript);

            AddTranscriptModerationResult(client, review_id, transcript);

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
    Adding a transcript to the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
    Adding a transcript moderation result to the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
    Publishing the review with ID 201801v5b08eefa0d2d4d64a1942aec7f5cacc3.
    Open your Content Moderator Dashboard and select Review > Video to see the review.
    Press any key to close the application.

## <a name="navigate-to-your-video-transcript-review"></a>Navegue para rever as vídeo transcript

Acede a rever as vídeo transcript na ferramenta de rever conteúdo Moderator o **rever**>**vídeo**>**Transcript** ecrã.

Consulte as seguintes funcionalidades:
- Duas linhas de transcript adicionou
- O termo profanity encontrado e realçado pelo serviço de moderação interrupção de texto
- Selecionar um texto transcription inicia o vídeo desse timestamp

![Revisão de vídeo transcript para moderators humanos](images/ams-video-transcript-review.PNG)

## <a name="next-steps"></a>Passos Seguintes

Saiba como gerar [vídeo revê](video-reviews-quickstart-dotnet.md) na ferramenta de revisão.

Veja o tutorial detalhado sobre como desenvolver uma [concluir solução moderação de interrupção gráfica](video-transcript-moderation-review-tutorial-dotnet.md).

[Transferir a solução do Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para esta e outros inícios rápidos de Moderator conteúdo para o .NET.
