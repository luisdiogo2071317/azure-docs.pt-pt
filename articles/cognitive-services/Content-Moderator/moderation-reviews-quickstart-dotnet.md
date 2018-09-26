---
title: Moderador de conteúdos do Azure - criar revisões usando o .NET | Documentos da Microsoft
description: Como criar análises com o SDK de moderador de conteúdos do Azure para .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/10/2018
ms.author: sajagtap
ms.openlocfilehash: c5f301e7ed15100c39f0af77942147275b966ed9
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47180778"
---
# <a name="create-reviews-using-net"></a>Criar análises com .NET

Este artigo fornece informações e exemplos de código para ajudá-lo a começar a utilizar o [conteúdo de moderador de SDK para .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) para:
 
- Criar um conjunto de revisões para moderadores humanos
- Obter o estado das revisões de existentes de moderadores humanos

Em geral, o conteúdo vai alguns moderação automática antes de a ser agendada de revisão humana. Este artigo apenas aborda como criar a revisão para moderação humana. Para um cenário mais completo, consulte a [moderação de conteúdos do Facebook](facebook-post-moderation.md) e [moderação do catálogo de comércio eletrónico](ecommerce-retail-catalog-moderation.md) tutoriais.

Este artigo pressupõe que já está familiarizado com o Visual Studio e c#.

## <a name="sign-up-for-content-moderator"></a>Inscreva-se o Content Moderator

Antes de poder utilizar os serviços de Content Moderator através da API REST ou o SDK, precisa de uma chave de subscrição.
Consulte a [guia de introdução](quick-start.md) para saber como pode obter a chave.

## <a name="sign-up-for-a-review-tool-account-if-not-completed-in-the-previous-step"></a>Inscrever-se para uma conta da ferramenta de revisão se não foi concluída no passo anterior

Se recebeu o Content Moderator do portal do Azure, também [Inscreva-se a conta da ferramenta de revisão](https://contentmoderator.cognitive.microsoft.com/) e criar uma equipa de revisão. Terá da equipe de Id e a ferramenta de revisão para chamar a API de revisão para iniciar uma tarefa e ver as críticas na ferramenta de revisão.

## <a name="ensure-your-api-key-can-call-the-review-api-for-review-creation"></a>Certifique-se de que a chave de API pode chamar a API de revisão para a criação de revisão

Depois de concluir os passos anteriores, pode acabar com duas chaves do Content Moderator se tiver iniciado a partir do portal do Azure. 

Se planear utilizar a chave de API fornecida pelo Azure no seu exemplo SDK, siga os passos mencionados na [chave de utilizar o Azure com a API de revisão](review-tool-user-guide/credentials.md#use-the-azure-account-with-the-review-tool-and-review-api) secção para permitir que a aplicação chamar a API de revisão e criar as revisões.

Se utilizar a chave de avaliação gratuita, gerada pela ferramenta de revisão, sua conta da ferramenta de revisão já sabe sobre a chave e por isso, não existem passos adicionais necessários.

## <a name="create-your-visual-studio-project"></a>Criar o seu projeto do Visual Studio

1. Adicionar um novo **aplicação de consola (.NET Framework)** projeto à sua solução.

   No código de exemplo, nomeie o projeto **CreateReviews**.

1. Selecione este projeto como o projeto de arranque único para a solução.

### <a name="install-required-packages"></a>Instalar pacotes necessários

Instale os seguintes pacotes de NuGet:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>O programa de atualização usando instruções

Modifique o programa usando instruções.

    using Microsoft.Azure.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using Newtonsoft.Json;
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Threading;

### <a name="create-the-content-moderator-client"></a>Criar o cliente do Content Moderator

Adicione o seguinte código para criar um cliente do Content Moderator para a sua subscrição.

> [!IMPORTANT]
> Atualização do **AzureRegion** e **CMSubscriptionKey** campos com os valores da sua chave de subscrição e o identificador de região.


    /// <summary>
    /// Wraps the creation and configuration of a Content Moderator client.
    /// </summary>
    /// <remarks>This class library contains insecure code. If you adapt this 
    /// code for use in production, use a secure method of storing and using
    /// your Content Moderator subscription key.</remarks>
    public static class Clients
    {
        /// <summary>
        /// The region/location for your Content Moderator account, 
        /// for example, westus.
        /// </summary>
        private static readonly string AzureRegion = "YOUR API REGION";

        /// <summary>
        /// The base URL fragment for Content Moderator calls.
        /// </summary>
        private static readonly string AzureBaseURL =
            $"https://{AzureRegion}.api.cognitive.microsoft.com";

        /// <summary>
        /// Your Content Moderator subscription key.
        /// </summary>
        private static readonly string CMSubscriptionKey = "YOUR API KEY";

        /// <summary>
        /// Returns a new Content Moderator client for your subscription.
        /// </summary>
        /// <returns>The new client.</returns>
        /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
        /// When you have finished using the client,
        /// you should dispose of it either directly or indirectly. </remarks>
        public static ContentModeratorClient NewClient()
        {
            // Create and initialize an instance of the Content Moderator API wrapper.
            ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

            client.Endpoint = AzureBaseURL;
            return client;
        }
    }

## <a name="create-a-class-to-associate-internal-content-information-with-a-review-id"></a>Criar uma classe para associar as informações dos conteúdos internas com um ID de revisão

Adicione a seguinte classe para o **programa** classe.
Utilize esta classe para associar o ID de revisão ao seu ID de conteúdo interno para o item.

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

### <a name="initialize-application-specific-settings"></a>Inicializar configurações específicas do aplicativo

> [!NOTE]
> A chave de serviço do Content Moderator tem um pedidos por segundo limite de taxa (RPS) e, se exceder o limite, o SDK lançará uma exceção com um código de 429 erro. 
>
> Uma chave de escalão gratuito tem um limite de taxa de um RPS.

#### <a name="add-the-following-constants-to-the-program-class-in-programcs"></a>Adicione as seguintes constantes para o **programa** classe no Program.cs.
    
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
    /// <remarks>Relative paths are relative to the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";

#### <a name="add-the-following-constants-and-static-fields-to-the-program-class-in-programcs"></a>Adicione as seguintes constantes e campos estáticos para o **programa** classe no Program.cs.

Atualize estes valores para conter informações específicas para a sua subscrição e a equipe.

> [!NOTE]
> Definir a constante TeamName como o nome que utilizou quando criou sua [ferramenta de revisão do Content Moderator](https://contentmoderator.cognitive.microsoft.com/) subscrição. Recupera o TeamName do **credenciais** secção a **definições** menu (engrenagem).
>
> O nome da sua equipe é o valor do **Id** campo na **API** secção.

    /// <summary>
    /// The name of the team to assign the review to.
    /// </summary>
    /// <remarks>This must be the team name you used to create your 
    /// Content Moderator account. You can retrieve your team name from
    /// the Content Moderator web site. Your team name is the Id associated 
    /// with your subscription.</remarks>
    private const string TeamName = "YOUR REVIEW TEAM ID";

    /// <summary>
    /// The optional name of the subteam to assign the review to.
    /// </summary>
    private const string Subteam = null;

    /// <summary>
    /// The callback endpoint for completed reviews.
    /// </summary>
    /// <remarks>Reviews show up for reviewers on your team. 
    /// As reviewers complete reviews, results are sent to the
    /// callback endpoint using an HTTP POST request.</remarks>
    private const string CallbackEndpoint = "YOUR API ENDPOINT";

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

#### <a name="add-the-following-static-fields-to-the-program-class-in-programcs"></a>Adicione os seguintes campos estáticos para o **programa** classe no Program.cs.

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

Normalmente, tem alguma lógica de negócio para identificar a entrada imagens, texto, ou vídeo que tem de ser revisto. No entanto, aqui basta use uma lista fixa de imagens.

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

## <a name="create-a-method-to-get-the-status-of-existing-reviews"></a>Criar um método para obter o estado das revisões de existentes

Adicione o seguinte método à classe **Programa**. 

> [!Note]
> Na prática, seria definido o URL de retorno de chamada `CallbackEndpoint` para o URL que receberia os resultados da revisão manual (por meio de um pedido de HTTP POST).
> Poderia modificar esse método para verificar o estado de pendente revisões.

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

Esse código simula várias das operações que efetuar na definir e gerir a lista, bem como através da lista para imagens de ecrã. Os recursos de log permitem-lhe ver os objetos de resposta gerados pelas chamadas SDK para o serviço do Content Moderator.

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

Verá a saída de exemplo seguinte:

    Creating reviews for the following images:
        - https://moderatorsampleimages.blob.core.windows.net/samples/sample1.jpg; with id = 0.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Pending.

Inicie sessão no Content Moderator Rever ferramenta para ver a imagem pendente Reveja com o **sc** etiqueta definida como **verdadeiro**. Também pode ver a predefinição **um** e **r** etiquetas e quaisquer etiquetas personalizadas que pode ter definido dentro da ferramenta de revisão. 

Utilize o **seguinte** botão para submeter.

![Revisão de imagem para moderadores humanos](images/moderation-reviews-quickstart-dotnet.PNG)

Em seguida, prima qualquer tecla para continuar.

    Waiting 45 seconds for results to propagate.

    Getting review details:
    Review 201712i46950138c61a4740b118a43cac33f434 for item ID 0 is Complete.

    Press any key to exit...

## <a name="check-out-the-following-output-in-the-log-file"></a>Veja o seguinte no ficheiro de registo de saída.

> [!NOTE]
> No seu ficheiro de saída, as cadeias de caracteres "\{teamname}" e "\{callbackUrl}" refletir os valores para o `TeamName` e `CallbackEndpoint` campos, respectivamente.

O IDs de revisão e a imagem de conteúdo de URLs são diferentes de cada vez que executar o aplicativo e, quando uma revisão é concluída, o `reviewerResultTags` campo reflete a forma como o revisor etiquetados o item.

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

## <a name="your-callback-url-if-provided-receives-this-response"></a>O Url de retorno de chamada se for fornecido, recebe essa resposta

Verá uma resposta semelhante ao seguinte exemplo:

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

Obter o [SDK de .NET do conteúdo moderador](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) e o [solução do Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para esse e outros guias de introdução do Content Moderator para .NET e começar a trabalhar com sua integração.
