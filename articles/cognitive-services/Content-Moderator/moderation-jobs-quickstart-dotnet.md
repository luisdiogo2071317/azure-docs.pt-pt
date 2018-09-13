---
title: Azure Content Moderator - das tarefas de moderação de início com o .NET | Documentos da Microsoft
description: Como iniciar trabalhos de moderação utilizando o SDK de moderador de conteúdos do Azure para .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/10/2018
ms.author: sajagtap
ms.openlocfilehash: 0402d9dc1dfee5e146d3550d095f4fb53e52f12b
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44720933"
---
# <a name="start-moderation-jobs-using-net"></a>Iniciar tarefas de moderação usando o .NET

Este artigo fornece informações e exemplos de código para ajudá-lo a começar a utilizar o [conteúdo de moderador de SDK para .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) para:
 
- Iniciar uma tarefa de moderação para analisar e criar as revisões de moderadores humanos
- Obter o estado da revisão pendente
- Monitorizar e obter o estado final da revisão
- Submeter o resultado para o Url de retorno de chamada

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

## <a name="define-a-custom-moderation-workflow"></a>Definir um fluxo de trabalho de moderação personalizado

Uma tarefa de moderação analisa o conteúdo usando as APIs e utiliza um **fluxo de trabalho** para determinar se deve criar revisões ou não.
Embora a ferramenta de revisão contém um fluxo de trabalho predefinida, vamos [definir um fluxo de trabalho personalizado](Review-Tool-User-Guide/Workflows.md) para este início rápido.

Utilize o nome do fluxo de trabalho no seu código que inicia a tarefa de moderação.

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

            client.BaseUrl = AzureBaseURL;
            return client;
        }
    }

### <a name="initialize-application-specific-settings"></a>Inicializar configurações específicas do aplicativo

Adicione as seguintes constantes e campos estáticos para o **programa** classe no Program.cs.

> [!NOTE]
> Definir a constante TeamName para o nome que utilizou quando criou a sua subscrição do Content Moderator. Obter o TeamName do [web site do Content Moderator](https://westus.contentmoderator.cognitive.microsoft.com/).
> Depois de iniciar sessão, selecione **credenciais** partir a **definições** menu (engrenagem).
>
> O nome da sua equipe é o valor do **Id** campo na **API** secção.


    /// <summary>
    /// The moderation job will use this workflow that you defined earlier.
    /// See the quickstart article to learn how to setup custom workflows.
    /// </summary>
    private const string WorkflowName = "OCR";
    
    /// <summary>
    /// The name of the team to assign the job to.
    /// </summary>
    /// <remarks>This must be the team name you used to create your 
    /// Content Moderator account. You can retrieve your team name from
    /// the Conent Moderator web site. Your team name is the Id associated 
    /// with your subscription.</remarks>
    private const string TeamName = "***";

    /// <summary>
    /// The URL of the image to create a review job for.
    /// </summary>
    private const string ImageUrl =
        "https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png";

    /// <summary>
    /// The name of the log file to create.
    /// </summary>
    /// <remarks>Relative paths are ralative the execution directory.</remarks>
    private const string OutputFile = "OutputLog.txt";

    /// <summary>
    /// The number of seconds to delay after a review has finished before
    /// getting the review results from the server.
    /// </summary>
    private const int latencyDelay = 45;

    /// <summary>
    /// The callback endpoint for completed reviews.
    /// </summary>
    /// <remarks>Revies show up for reviewers on your team. 
    /// As reviewers complete reviews, results are sent to the
    /// callback endpoint using an HTTP POST request.</remarks>
    private const string CallbackEndpoint = "";

## <a name="add-code-to-auto-moderate-create-a-review-and-get-the-job-details"></a>Adicione o código a moderado de automática, criar uma revisão e obter os detalhes da tarefa

> [!Note]
> Na prática, define o URL de chamada de retorno **CallbackEndpoint** para o URL que recebe os resultados da revisão manual (por meio de um pedido de HTTP POST).

Comece por adicionar o seguinte código para o **Main** método.

    using (TextWriter writer = new StreamWriter(OutputFile, false))
    {
        using (var client = Clients.NewClient())
        {
            writer.WriteLine("Create review job for an image.");
            var content = new Content(ImageUrl);
        
            // The WorkflowName contains the nameof the workflow defined in the online review tool.
            // See the quickstart article to learn more.
            var jobResult = client.Reviews.CreateJobWithHttpMessagesAsync(
                    TeamName, "image", "contentID", WorkflowName, "application/json", content, CallbackEndpoint);

            // Record the job ID.
            var jobId = jobResult.Result.Body.JobIdProperty;

            // Log just the response body from the returned task.
            writer.WriteLine(JsonConvert.SerializeObject(
                jobResult.Result.Body, Formatting.Indented));

            Thread.Sleep(2000);
            writer.WriteLine();

            writer.WriteLine("Get review job status.");
            var jobDetails = client.Reviews.GetJobDetailsWithHttpMessagesAsync(
                    TeamName, jobId);

            // Log just the response body from the returned task.
            writer.WriteLine(JsonConvert.SerializeObject(
                    jobDetails.Result.Body, Formatting.Indented));

            Console.WriteLine();
            Console.WriteLine("Perform manual reviews on the Content Moderator site.");
            Console.WriteLine("Then, press any key to continue.");
            Console.ReadKey();

            Console.WriteLine();
            Console.WriteLine($"Waiting {latencyDelay} seconds for results to propagate.");
            Thread.Sleep(latencyDelay * 1000);

            writer.WriteLine("Get review details.");
            jobDetails = client.Reviews.GetJobDetailsWithHttpMessagesAsync(
            TeamName, jobId);

            // Log just the response body from the returned task.
            writer.WriteLine(JsonConvert.SerializeObject(
            jobDetails.Result.Body, Formatting.Indented));
        }
        writer.Flush();
        writer.Close();
    }

> [!NOTE]
> A chave de serviço do Content Moderator tem um pedidos por segundo (RPS) para o limite de velocidade. Se ultrapassar o limite, o SDK lançará uma exceção com um código de 429 erro. 
>
> Uma chave de escalão gratuito tem um limite de taxa de um RPS.

## <a name="run-the-program-and-review-the-output"></a>Execute o programa e reveja o resultado

Verá a saída de exemplo seguintes na consola do:

    Perform manual reviews on the Content Moderator site.
    Then, press any key to continue.

Ferramenta para ver a imagem pendente rever de revisão de início de sessão para o Content Moderator.

Utilize o **seguinte** botão para submeter.

![Revisão de imagem para moderadores humanos](images/ocr-sample-image.PNG)

## <a name="see-the-sample-output-in-the-log-file"></a>Ver a saída de exemplo no ficheiro de registo

> [!NOTE]
> No seu ficheiro de saída, as cadeias de caracteres **Teamname**, **ContentId**, **CallBackEndpoint**, e **WorkflowId** refletir os valores que utilizou anteriormente.

    Create moderation job for an image.
    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe"
    }

    Get review details.
    {
        "Id": "2018014caceddebfe9446fab29056fd8d31ffe",
        "TeamName": "some team name",
        "Status": "InProgress",
        "WorkflowId": "OCR",
        "Type": "Image",
        "CallBackEndpoint": "",
        "ReviewId": "",
        "ResultMetaData": [],
        "JobExecutionReport": [
        {
            "Ts": "2018-01-07T00:38:26.7714671",
            "Msg": "Successfully got hasText response from Moderator"
        },
        {
            "Ts": "2018-01-07T00:38:26.4181346",
            "Msg": "Getting hasText from Moderator"
        },
        {
            "Ts": "2018-01-07T00:38:25.5122828",
            "Msg": "Starting Execution - Try 1"
        }
        ]
    }


## <a name="your-callback-url-if-provided-receives-this-response"></a>Se for fornecido, o Url de retorno de chamada recebe esta resposta.

Verá uma resposta semelhante ao seguinte exemplo:

> [!NOTE]
> Em sua resposta de retorno de chamada, as cadeias de caracteres **ContentId** e **WorkflowId** refletir os valores que utilizou anteriormente.

    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe",
        "ReviewId": "201801i28fc0f7cbf424447846e509af853ea54",
        "WorkFlowId": "OCR",
        "Status": "Complete",
        "ContentType": "Image",
        "CallBackType": "Job",
        "ContentId": "contentID",
        "Metadata": {
            "hastext": "True",
            "ocrtext": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n",
            "imagename": "contentID"
        }
    }


## <a name="next-steps"></a>Passos Seguintes

Obter o [SDK de .NET do conteúdo moderador](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) e o [solução do Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para esse e outros guias de introdução do Content Moderator para .NET e começar a trabalhar com sua integração.
