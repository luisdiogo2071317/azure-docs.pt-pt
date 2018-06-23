---
title: Moderator conteúdo do Azure - iniciar tarefas de moderação interrupção através do .NET | Microsoft Docs
description: Como iniciar tarefas de moderação interrupção utilizando o SDK de Moderator conteúdo do Azure para .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/06/2018
ms.author: sajagtap
ms.openlocfilehash: a103875607355993e216ce1ddea02009fc8fa1c4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351692"
---
# <a name="start-moderation-jobs-using-net"></a>Iniciar tarefas de moderação interrupção através do .NET

Este artigo fornece informações e exemplos de código para o ajudar a começar a utilizar o SDK Moderator conteúdo para o .NET para:
 
- Iniciar uma tarefa de moderação interrupção para analisar e criar revisões para moderators humanos
- Obter o estado do Consulte pendente
- Controlar e obter o estado final de revisão
- O resultado para o Url de chamada de retorno de envio

Este artigo pressupõe que já estiver familiarizado com o Visual Studio e c#.

## <a name="sign-up-for-content-moderator-services"></a>Inscreva-se os serviços de Moderator conteúdo

Antes de poder utilizar os serviços de Moderator conteúdo através da API REST ou o SDK, precisa de uma chave de subscrição.
Consulte o [início rápido](quick-start.md) para saber como pode obter a chave.

## <a name="define-a-custom-moderation-workflow"></a>Definir um fluxo de trabalho personalizado moderação de interrupção

Uma tarefa de moderação interrupção analisa o conteúdo utilizando as APIs e utiliza um **fluxo de trabalho** para determinar se deve criar revisões ou não.
Enquanto a ferramenta de revisão contém um fluxo de trabalho predefinido, vamos [definir um fluxo de trabalho personalizado](Review-Tool-User-Guide/Workflows.md) para este início rápido.

Utilize o nome do fluxo de trabalho no seu código que inicia a tarefa de moderação interrupção.

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

### <a name="initialize-application-specific-settings"></a>Inicializar definições específicas da aplicação

Adicione as seguintes constantes e campos estáticos para o **programa** a classe Program. cs.

> [!NOTE]
> Definir a constante TeamName para o nome que utilizou quando criou a sua subscrição Moderator conteúdo. Obter TeamName do [Moderator conteúdo web site](https://westus.contentmoderator.cognitive.microsoft.com/).
> Depois de iniciar sessão, selecione **credenciais** do **definições** menu (engrenagem).
>
> O nome do agrupamento é o valor da **ID** campo no **API** secção.


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

## <a name="add-code-to-auto-moderate-create-a-review-and-get-the-job-details"></a>Adicione código à automática moderado, crie uma revisão e obter os detalhes da tarefa

> [!Note]
> Na prática, definir o URL de chamada de retorno **CallbackEndpoint** para o URL que recebe os resultados da revisão manual (através de um pedido POST de HTTP).

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
> A chave do serviço Moderator conteúdo tem um pedidos por segundo (RPS) para o limite de velocidade. Se exceder o limite, o SDK emite uma exceção com um código de 429 erro. 
>
> Uma chave de escalão gratuito tem um limite de taxa de um RPS.

## <a name="run-the-program-and-review-the-output"></a>Execute o programa e reveja o resultado

Consulte o seguinte resultado de exemplo na consola:

    Perform manual reviews on the Content Moderator site.
    Then, press any key to continue.

Inicie sessão no conteúdo Moderator reveja ferramenta para ver a imagem pendente rever.

Utilize o **seguinte** botão para submeter.

![Revisão de imagem para moderators humanos](images/ocr-sample-image.PNG)

## <a name="see-the-sample-output-in-the-log-file"></a>Ver o resultado de exemplo no ficheiro de registo

> [!NOTE]
> No seu ficheiro de saída, as cadeias **Teamname**, **ContentId**, **CallBackEndpoint**, e **WorkflowId** refletir os valores que utilizou versões anteriores.

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


## <a name="your-callback-url-if-provided-receives-this-response"></a>O Url de chamada de retorno, se fornecidas, recebe esta resposta.

Consulte uma resposta semelhante ao seguinte exemplo:

> [!NOTE]
> A resposta de chamada de retorno, as cadeias **ContentId** e **WorkflowId** refletir os valores utilizados anteriormente.

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

[Transferir a solução do Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para esta e outros inícios rápidos de Moderator conteúdo para o .NET e começar a utilizar a integração.
