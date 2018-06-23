---
title: Moderator conteúdo do Azure - criar revisões através do .NET | Microsoft Docs
description: Como criar revê utilizando o SDK de Moderator conteúdo do Azure para .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: 6a0ff48f4ea17f9c800f3e6c096df2492699f87a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351841"
---
# <a name="create-reviews-using-net"></a>Criar revisões através do .NET

Este artigo fornece informações e exemplos de código para o ajudar a começar a utilizar o SDK Moderator conteúdo para o .NET para:
 
- Criar um conjunto de revisões para moderators humanos
- Obter o estado de revisões existentes para moderators humanos

Geralmente, o conteúdo passa através de alguns moderação de interrupção automatizada antes de serem agendadas para revisão humano. Este artigo abrange apenas como criar a revisão de moderação de interrupção humana. Para um cenário mais completo, consulte o [moderação de interrupção conteúda do Facebook](facebook-post-moderation.md) e [moderação de interrupção do eCommerce catálogo](ecommerce-retail-catalog-moderation.md) tutoriais.

Este artigo pressupõe que já estiver familiarizado com o Visual Studio e c#.

## <a name="sign-up-for-content-moderator-services"></a>Inscreva-se os serviços de Moderator conteúdo

Antes de poder utilizar os serviços de Moderator conteúdo através da API REST ou o SDK, precisa de uma chave de subscrição.
Consulte o [início rápido](quick-start.md) para saber como pode obter a chave.

## <a name="create-your-visual-studio-project"></a>Criar o projeto do Visual Studio

1. Adicione um novo **aplicação de consola (.NET Framework)** projeto para a sua solução.

   No código de exemplo, nome do projeto **CreateReviews**.

1. Selecione este projeto como o projeto de arranque único para a solução.

1. Adicione uma referência para o **ModeratorHelper** projeto de assemblagem que criou no [início rápido do programa auxiliar de cliente Moderator conteúdo](content-moderator-helper-quickstart-dotnet.md).

### <a name="install-required-packages"></a>Instalar pacotes necessários

Instale os pacotes de NuGet seguintes:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft

### <a name="update-the-programs-using-statements"></a>O programa de atualização do instruções de utilização

Modificar o programa de instruções de utilização.

    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;

## <a name="create-a-class-to-associate-internal-content-information-with-a-review-id"></a>Crie uma classe para associar as informações dos conteúdos internas com um ID de revisão

Adicione a seguinte classe para o **programa** classe.
Utilize esta classe para associar o ID de revisão para o ID de conteúdo interno para o item.

    /// <summary>
    /// Associates the review ID (assigned by the service) to the internal
    /// content ID of the item.
    /// </summary>
    public class ReviewItem
    {
        /// <summary>
        /// The media type for the item to review.
        /// </summary>
        public string Type;

        /// <summary>
        /// The URL of the item to review.
        /// </summary>
        public string Url;

        /// <summary>
        /// The internal content ID for the item to review.
        /// </summary>
        public string ContentId;

        /// <summary>
        /// The ID that the service assigned to the review.
        /// </summary>
        public string ReviewId;
    }

### <a name="initialize-application-specific-settings"></a>Inicializar definições específicas da aplicação

> [!NOTE]
> A chave do serviço Moderator conteúdo tem um pedidos por segundo (RPS) para o limite de velocidade e, se for excedido o limite, o SDK emite uma exceção com um código de 429 erro. 
>
> Uma chave de escalão gratuito tem um limite de taxa de um RPS.

#### <a name="add-the-following-constants-to-the-program-class-in-programcs"></a>Adicione as seguintes constantes para o **programa** a classe Program. cs.
    
    /// <summary>
    /// The minimum amount of time, in milliseconds, to wait between calls
    /// to the Image List API.
    /// </summary>
    private const int throttleRate = 3000;

    /// <summary>
    /// The number of seconds to delay after a review has finished before
    /// getting the review results from the server.
    /// </summary>
    private const double latencyDelay = 45;

    /// <summary>
    /// The name of the log file to create.
    /// </summary>
    /// <remarks>Relative paths are ralative the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";

#### <a name="add-the-following-constants-and-static-fields-to-the-program-class-in-programcs"></a>Adicione as seguintes constantes e campos estáticos para o **programa** a classe Program. cs.

Atualize estes valores para conter informações específicas para a sua subscrição e a equipa.

> [!NOTE]
> Definir a constante TeamName para o nome que utilizou quando criou o [ferramenta de revisão Moderator conteúdo](https://contentmoderator.cognitive.microsoft.com/) subscrição. Obter o TeamName do **credenciais** secção o **definições** menu (engrenagem).
>
> O nome do agrupamento é o valor da **Id** campo no **API** secção.

    /// <summary>
    /// The name of the team to assign the review to.
    /// </summary>
    /// <remarks>This must be the team name you used to create your 
    /// Content Moderator account. You can retrieve your team name from
    /// the Conent Moderator web site. Your team name is the Id associated 
    /// with your subscription.</remarks>
    private const string TeamName = "{teamname}";

    /// <summary>
    /// The optional name of the subteam to assign the review to.
    /// </summary>
    private const string Subteam = null;

    /// <summary>
    /// The callback endpoint for completed reviews.
    /// </summary>
    /// <remarks>Revies show up for reviewers on your team. 
    /// As reviewers complete reviews, results are sent to the
    /// callback endpoint using an HTTP POST request.</remarks>
    private const string CallbackEndpoint = "{callbackUrl}";

    /// <summary>
    /// The media type for the item to review.
    /// </summary>
    /// <remarks>Valid values are "image", "text", and "video".</remarks>
    private const string MediaType = "image";

    /// <summary>
    /// The URLs of the images to create review jobs for.
    /// </summary>
    private static readonly string[] ImageUrls = new string[] {
        "https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg"
        // add more if you want
    };

    /// <summary>
    /// The metadata key to initially add to each review item.
    /// </summary>
    private const string MetadataKey = "sc";

    /// <summary>
    /// The metadata value to initially add to each review item.
    /// </summary>
    private const string MetadataValue = "true";

#### <a name="add-the-following-static-fields-to-the-program-class-in-programcs"></a>Adicione os seguintes campos estáticos para o **programa** a classe Program. cs.

Utilize estes campos para controlar o estado da aplicação.

    /// <summary>
    /// A static reference to the text writer to use for logging.
    /// </summary>
    private static TextWriter writer;

    /// <summary>
    /// The cached review information, associating a local content ID
    /// to the created review ID for each item.
    /// </summary>
    private static List<ReviewItem> reviewItems =
        new List<ReviewItem>();

## <a name="create-a-method-to-write-messages-to-the-log-file"></a>Criar um método para escrever mensagens para o ficheiro de registo

Adicione o seguinte método à classe **Programa**. 

    /// <summary>
    /// Writes a message to the log file, and optionally to the console.
    /// </summary>
    /// <param name="message">The message.</param>
    /// <param name="echo">if set to <c>true</c>, write the message to the console.</param>
    private static void WriteLine(string message = null, bool echo = false)
    {
        writer.WriteLine(message ?? String.Empty);

        if (echo)
        {
            Console.WriteLine(message ?? String.Empty);
        }
    }

## <a name="create-a-method-to-create-a-set-of-reviews"></a>Criar um método para criar um conjunto de revisões

Normalmente, tem algumas lógica de negócio para identificar as imagens de entrada, texto, ou o vídeo que tem de ser revistas. No entanto, aqui apenas utilize numa lista fixa de imagens.

Adicione o seguinte método à classe **Programa**.

    /// <summary>
    /// Create the reviews using the fixed list of images.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    private static void CreateReviews(ContentModeratorClient client)
    {
        WriteLine(null, true);
        WriteLine("Creating reviews for the following images:", true);

        // Create the structure to hold the request body information.
        List<CreateReviewBodyItem> requestInfo =
            new List<CreateReviewBodyItem>();

        // Create some standard metadata to add to each item.
        List<CreateReviewBodyItemMetadataItem> metadata =
            new List<CreateReviewBodyItemMetadataItem>(
            new CreateReviewBodyItemMetadataItem[] {
                new CreateReviewBodyItemMetadataItem(
                    MetadataKey, MetadataValue)
            });

        // Populate the request body information and the initial cached review information.
        for (int i = 0; i < ImageUrls.Length; i++)
        {
            // Cache the local information with which to create the review.
            var itemInfo = new ReviewItem()
            {
                Type = MediaType,
                ContentId = i.ToString(),
                Url = ImageUrls[i],
                ReviewId = null
            };

            WriteLine($" - {itemInfo.Url}; with id = {itemInfo.ContentId}.", true);

            // Add the item informaton to the request information.
            requestInfo.Add(new CreateReviewBodyItem(
                itemInfo.Type, itemInfo.Url, itemInfo.ContentId,
                CallbackEndpoint, metadata));

            // Cache the review creation information.
            reviewItems.Add(itemInfo);
        }

        var reviewResponse = client.Reviews.CreateReviewsWithHttpMessagesAsync(
            "application/json", TeamName, requestInfo);

        // Update the local cache to associate the created review IDs with
        // the associated content.
        var reviewIds = reviewResponse.Result.Body;
        for (int i = 0; i < reviewIds.Count; i++)
        {
            Program.reviewItems[i].ReviewId = reviewIds[i];
        }

        WriteLine(JsonConvert.SerializeObject(
        reviewIds, Formatting.Indented));

        Thread.Sleep(throttleRate);
    }

## <a name="create-a-method-to-get-the-status-of-existing-reviews"></a>Criar um método para obter o estado de revisões existentes

Adicione o seguinte método à classe **Programa**. 

> [!Note]
> Na prática, seria a definir o URL de chamada de retorno `CallbackEndpoint` para o URL que pretende receber os resultados da revisão manual (através de um pedido POST de HTTP).
> Pode modificar este método para verificar o estado pendente revisões.

    /// <summary>
    /// Gets the review details from the server.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    private static void GetReviewDetails(ContentModeratorClient client)
    {
        WriteLine(null, true);
        WriteLine("Getting review details:", true);
        foreach (var item in reviewItems)
        {
            var reviewDetail = client.Reviews.GetReviewWithHttpMessagesAsync(
                TeamName, item.ReviewId);

            WriteLine(
                $"Review {item.ReviewId} for item ID {item.ContentId} is " +
                $"{reviewDetail.Result.Body.Status}.", true);
            WriteLine(JsonConvert.SerializeObject(
                reviewDetail.Result.Body, Formatting.Indented));

            Thread.Sleep(throttleRate);
        }
    }

## <a name="add-code-to-create-a-set-of-reviews-and-check-its-status"></a>Adicionar código para criar um conjunto de revisões e verifique o respetivo estado

Adicione o seguinte código para o **Main** método.

Este código simula muitas das operações que efetuar no definir e gerir a lista, bem como utilizar a lista de imagens do ecrã. As funcionalidades de registo permitem-lhe ver os objetos de resposta gerados pelas chamadas SDK para o serviço de Moderator conteúdo.

    using (TextWriter outputWriter = new StreamWriter(OutputFile, false))
    {
        writer = outputWriter;
        using (var client = Clients.NewClient())
        {
            CreateReviews(client);
            GetReviewDetails(client);

            Console.WriteLine();
            Console.WriteLine("Perform manual reviews on the Content Moderator site.");
            Console.WriteLine("Then, press any key to continue.");
            Console.ReadKey();

            Console.WriteLine();
            Console.WriteLine($"Waiting {latencyDelay} seconds for results to propigate.");
            Thread.Sleep(latencyDelay * 1000);

            GetReviewDetails(client);
        }

        writer = null;
        outputWriter.Flush();
        outputWriter.Close();
    }

    Console.WriteLine();
    Console.WriteLine("Press any key to exit...");
    Console.ReadKey();

## <a name="run-the-program-and-review-the-output"></a>Execute o programa e reveja o resultado

Consulte o seguinte resultado de exemplo:

    Creating reviews for the following images:
        - https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.

Inicie sessão no conteúdo Moderator Rever ferramenta para ver a imagem pendente Reveja com o **sc** etiqueta definida como **verdadeiro**. Consulte também a predefinição **um** e **r** etiquetas e quaisquer etiquetas personalizadas que pode definidas dentro da ferramenta de revisão. 

Utilize o **seguinte** botão para submeter.

![Revisão de imagem para moderators humanos](images/moderation-reviews-quickstart-dotnet.PNG)

Em seguida, prima qualquer tecla para continuar.

    Waiting 45 seconds for results to propagate.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.

    Press any key to exit...

## <a name="check-out-the-following-output-in-the-log-file"></a>Consulte o seguinte no ficheiro de registo de saída.

> [!NOTE]
> No seu ficheiro de saída, as cadeias de "\{teamname}" e "\{callbackUrl}" refletir os valores para o `TeamName` e `CallbackEndpoint` campos, respetivamente.

O IDs de revisão e a imagem de conteúdo de URLs forem diferentes sempre que executar a aplicação e, quando uma revisão estiver concluída, o `reviewerResultTags` campo reflete a forma como o revisor etiquetados o item.

    Creating reviews for the following images:
        - https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.
    [
        "201712i46950138c61a4740b118a43cac33f434",
    ]

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.
    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Pending",
        "reviewerResultTags": [],
        "createdBy": "{teamname}",
        "metadata": [
        {
            "key": "sc",
            "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.
    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Complete",
        "reviewerResultTags": [
        {
            "key": "a",
            "value": "False"
        },
        {
            "key": "r",
            "value": "True"
        },
        {
            "key": "sc",
            "value": "True"
        }
        ],
        "createdBy": "{teamname}",
        "metadata": [
        {
            "key": "sc",
            "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }

## <a name="your-callback-url-if-provided-receives-this-response"></a>O Url de chamada de retorno, se fornecidas, recebe esta resposta

Consulte uma resposta semelhante ao seguinte exemplo:

    {
        "ReviewId": "201801i48a2937e679a41c7966e838c92f5e649",
        "ModifiedOn": "2018-01-06T05:04:40.5525865Z",
        "ModifiedBy": "yourusername",
        "CallBackType": "Review",
        "ContentId": "0",
        "ContentType": "Image",
        "Metadata": {
            "sc": "true"
            },
        "ReviewerResultTags": {
            "a": "False",
            "r": "False",
        }
    }


## <a name="next-steps"></a>Passos Seguintes

[Transferir a solução do Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para esta e outros inícios rápidos de Moderator conteúdo para o .NET e começar a utilizar a integração.
