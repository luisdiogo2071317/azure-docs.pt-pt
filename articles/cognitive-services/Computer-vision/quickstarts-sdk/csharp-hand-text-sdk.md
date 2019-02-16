---
title: 'Início rápido: Extrair texto manuscrito - SDK,C#'
titleSuffix: Azure Cognitive Services
description: Neste guia de início rápido, irá extrair texto de uma imagem através da biblioteca de cliente C# do Windows de Imagem Digitalizada.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 02/12/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: d71a566d5c6dc5505b4bd939e294f8428e9a5b93
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/15/2019
ms.locfileid: "56312914"
---
# <a name="quickstart-extract-text-using-the-computer-vision-sdk-and-c"></a>Início rápido: Extrair texto com o SDK de visão do computador eC#

Neste início rápido, irá extrair texto manuscrito ou impresso de uma imagem usando o SDK de visão do computador para C#. Se desejar, pode transferir o código neste guia como uma aplicação de exemplo completa a partir da [visão de Csharp de serviços cognitivos](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/ComputerVision) repositório no GitHub.

## <a name="prerequisites"></a>Pré-requisitos

* Para utilizar a Imagem Digitalizada, precisa de uma chave de subscrição; veja [Obtaining Subscription Keys](../Vision-API-How-to-Topics/HowToSubscribe.md) (Obter Chaves de Subscrição).
* Qualquer edição do [Visual Studio 2015 ou 2017](https://www.visualstudio.com/downloads/).
* O pacote NuGet da biblioteca de cliente [Microsoft.Azure.CognitiveServices.Vision.ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision). Não é necessário transferir o pacote. Abaixo, são fornecidas as instruções de instalação.

## <a name="create-and-run-the-sample-app"></a>Criar e executar aplicação de exemplo

Para executar o exemplo, siga os seguintes passos:

1. Crie uma nova Aplicação da Consola Visual C# no Visual Studio.
1. Instale o pacote NuGet da biblioteca de cliente de Imagem Digitalizada.
    1. No menu, clique em **Ferramentas**, selecione **Gestor de Pacotes NuGet** e, em seguida, **Gerir Pacotes NuGet para Solução**.
    1. Clique no separador **Procurar** e na caixa **Pesquisar** escreva "Microsoft.Azure.CognitiveServices.Vision.ComputerVision".
    1. Selecione **Microsoft.Azure.CognitiveServices.Vision.ComputerVision** quando for apresentado e, em seguida, clique na caixa de verificação junto do nome do projeto e em **Instalar**.
1. Substitua `Program.cs` pelo código abaixo. Os métodos `RecognizeTextAsync` e `RecognizeTextInStreamAsync` encapsulam num wrapper a [API de Reconhecimento de Texto](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) para imagens remotas e locais, respetivamente. O método `GetTextOperationResultAsync` encapsula num wrapper a [API para Obter Resultados da Operação de Reconhecimento de Texto](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2cf1154055056008f201).

    ```csharp
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;

    using System;
    using System.IO;
    using System.Threading.Tasks;

    namespace ExtractText
    {
        class Program
        {
            // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
            private const string subscriptionKey = "<SubscriptionKey>";

            // For printed text, change to TextRecognitionMode.Printed
            private const TextRecognitionMode textRecognitionMode =
                TextRecognitionMode.Handwritten;

            // localImagePath = @"C:\Documents\LocalImage.jpg"
            private const string localImagePath = @"<LocalImage>";

            private const string remoteImageUrl =
                "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/" +
                "Cursive_Writing_on_Notebook_paper.jpg/" +
                "800px-Cursive_Writing_on_Notebook_paper.jpg";

            private const int numberOfCharsInOperationId = 36;

            static void Main(string[] args)
            {
                ComputerVisionClient computerVision = new ComputerVisionClient(
                    new ApiKeyServiceClientCredentials(subscriptionKey),
                    new System.Net.Http.DelegatingHandler[] { });

                // You must use the same region as you used to get your subscription
                // keys. For example, if you got your subscription keys from westus,
                // replace "westcentralus" with "westus".
                //
                // Free trial subscription keys are generated in the "westus"
                // region. If you use a free trial subscription key, you shouldn't
                // need to change the region.

                // Specify the Azure region
                computerVision.Endpoint = "https://westcentralus.api.cognitive.microsoft.com";

                Console.WriteLine("Images being analyzed ...");
                var t1 = ExtractRemoteTextAsync(computerVision, remoteImageUrl);
                var t2 = ExtractLocalTextAsync(computerVision, localImagePath);

                Task.WhenAll(t1, t2).Wait(5000);
                Console.WriteLine("Press ENTER to exit");
                Console.ReadLine();
            }

            // Recognize text from a remote image
            private static async Task ExtractRemoteTextAsync(
                ComputerVisionClient computerVision, string imageUrl)
            {
                if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
                {
                    Console.WriteLine(
                        "\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                    return;
                }

                // Start the async process to recognize the text
                RecognizeTextHeaders textHeaders =
                    await computerVision.RecognizeTextAsync(
                        imageUrl, textRecognitionMode);

                await GetTextAsync(computerVision, textHeaders.OperationLocation);
            }

            // Recognize text from a local image
            private static async Task ExtractLocalTextAsync(
                ComputerVisionClient computerVision, string imagePath)
            {
                if (!File.Exists(imagePath))
                {
                    Console.WriteLine(
                        "\nUnable to open or read localImagePath:\n{0} \n", imagePath);
                    return;
                }

                using (Stream imageStream = File.OpenRead(imagePath))
                {
                    // Start the async process to recognize the text
                    RecognizeTextInStreamHeaders textHeaders =
                        await computerVision.RecognizeTextInStreamAsync(
                            imageStream, textRecognitionMode);

                    await GetTextAsync(computerVision, textHeaders.OperationLocation);
                }
            }

            // Retrieve the recognized text
            private static async Task GetTextAsync(
                ComputerVisionClient computerVision, string operationLocation)
            {
                // Retrieve the URI where the recognized text will be
                // stored from the Operation-Location header
                string operationId = operationLocation.Substring(
                    operationLocation.Length - numberOfCharsInOperationId);

                Console.WriteLine("\nCalling GetHandwritingRecognitionOperationResultAsync()");
                TextOperationResult result =
                    await computerVision.GetTextOperationResultAsync(operationId);

                // Wait for the operation to complete
                int i = 0;
                int maxRetries = 10;
                while ((result.Status == TextOperationStatusCodes.Running ||
                        result.Status == TextOperationStatusCodes.NotStarted) && i++ < maxRetries)
                {
                    Console.WriteLine(
                        "Server status: {0}, waiting {1} seconds...", result.Status, i);
                    await Task.Delay(1000);

                    result = await computerVision.GetTextOperationResultAsync(operationId);
                }

                // Display the results
                Console.WriteLine();
                var lines = result.RecognitionResult.Lines;
                foreach (Line line in lines)
                {
                    Console.WriteLine(line.Text);
                }
                Console.WriteLine();
            }
        }
    }
    ```

1. Substitua `<Subscription Key>` pela sua chave de subscrição válida.
1. Altere `computerVision.Endpoint` para a região do Azure associada às chaves de subscrição, se necessário.
1. Opcionalmente, defina `textRecognitionMode` como `TextRecognitionMode.Printed`.
1. Substitua `<LocalImage>` pelo caminho e o nome de ficheiro de uma imagem local.
1. Opcionalmente, defina `remoteImageUrl` para uma imagem diferente.
1. Execute o programa.


## <a name="examine-the-response"></a>Examinar a resposta

Uma resposta com êxito imprime as linhas de texto reconhecido para cada imagem.

```console
Calling GetHandwritingRecognitionOperationResultAsync()

Calling GetHandwritingRecognitionOperationResultAsync()
Server status: Running, waiting 1 seconds...
Server status: Running, waiting 1 seconds...

dog
The quick brown fox jumps over the lazy
Pack my box with five dozen liquor jugs
```

Consulte [início rápido: Extrair texto manuscrito - REST, C# ](../QuickStarts/CSharp-hand-text.md#examine-the-response) para obter um exemplo da saída JSON não processado da chamada API.

## <a name="next-steps"></a>Passos Seguintes

Explore as API de Imagem Digitalizada utilizadas para analisar uma imagem, detetar celebridades e marcos, criar uma miniatura e extrair texto impresso e manuscrito.

> [!div class="nextstepaction"]
> [Explorar API de Imagem Digitalizada](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
