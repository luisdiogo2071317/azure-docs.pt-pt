---
title: Texto manuscrito de sdk de início rápido do C# da API de Imagem Digitalizada | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido, irá extrair texto manuscrito de uma imagem através da biblioteca de cliente do C# do Windows de Imagem Digitalizada nos serviços cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 7eb87e3d4b1703bf1ee0e30c930b0bc724b7f22f
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43771921"
---
# <a name="quickstart-extract-handwritten-text---sdk-c35"></a>Início rápido: Extrair texto manuscrito - SDK, C&#35;

Neste início rápido, irá extrair texto manuscrito de uma imagem através da biblioteca de cliente do Windows de Imagem Digitalizada.

## <a name="prerequisites"></a>Pré-requisitos

* Para utilizar a Imagem Digitalizada, necessita de uma chave de subscrição; consulte [A Obter Chaves de Subscrição](../Vision-API-How-to-Topics/HowToSubscribe.md).
* Qualquer edição do [Visual Studio 2015 ou 2017](https://www.visualstudio.com/downloads/).
* O pacote NuGet da biblioteca de cliente [Microsoft.Azure.CognitiveServices.Vision.ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision). Não é necessário transferir o pacote. Abaixo, são fornecidas as instruções de instalação.

## <a name="recognizetextasync-method"></a>Método RecognizeTextAsync

Os métodos `RecognizeTextAsync` e `RecognizeTextInStreamAsync` encapsulam num wrapper a [API de Reconhecimento de Texto](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) para imagens remotas e locais, respetivamente. O método `GetTextOperationResultAsync` encapsula num wrapper a [API para Obter Resultados da Operação de Reconhecimento de Texto](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2cf1154055056008f201).  Pode utilizar estes métodos para detetar texto manuscrito numa imagem e extrair os carateres reconhecidos para um fluxo de carateres que podem ser utilizados por um computador.

Para executar o exemplo, siga os passos seguintes:

1. Crie uma nova Aplicação da Consola Visual C# no Visual Studio.
1. Instale o pacote NuGet da biblioteca de cliente de Imagem Digitalizada.
    1. No menu, clique em **Ferramentas**, selecione **Gestor de Pacotes NuGet** e, em seguida, **Gerir Pacotes NuGet para Solução**.
    1. Clique no separador **Procurar** e na caixa **Pesquisar** escreva "Microsoft.Azure.CognitiveServices.Vision.ComputerVision".
    1. Selecione **Microsoft.Azure.CognitiveServices.Vision.ComputerVision** quando for apresentado e, em seguida, clique na caixa de verificação junto do nome do projeto e em **Instalar**.
1. Substitua `Program.cs` pelo código abaixo.
1. Substitua `<Subscription Key>` pela sua chave de subscrição válida.
1. Altere `computerVision.AzureRegion = AzureRegions.Westcentralus` para a localização onde obteve as suas chaves de subscrição, se necessário.
1. Substitua `<LocalImage>` pelo caminho e o nome de ficheiro de uma imagem local.
1. Opcionalmente, defina `remoteImageUrl` para uma imagem diferente.
1. Execute o programa.

```csharp
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;

using System;
using System.IO;
using System.Threading.Tasks;

namespace ImageHandText
{
    class Program
    {
        // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // localImagePath = @"C:\Documents\LocalImage.jpg"
        private const string localImagePath = @"<LocalImage>";

        private const string remoteImageUrl =
            "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/" +
            "Cursive_Writing_on_Notebook_paper.jpg/" +
            "800px-Cursive_Writing_on_Notebook_paper.jpg";

        private const int numberOfCharsInOperationId = 36;

        static void Main(string[] args)
        {
            ComputerVisionAPI computerVision = new ComputerVisionAPI(
                new ApiKeyServiceClientCredentials(subscriptionKey),
                new System.Net.Http.DelegatingHandler[] { });

            // You must use the same region as you used to get your subscription
            // keys. For example, if you got your subscription keys from westus,
            // replace "Westcentralus" with "Westus".
            //
            // Free trial subscription keys are generated in the westcentralus
            // region. If you use a free trial subscription key, you shouldn't
            // need to change the region.

            // Specify the Azure region
            computerVision.AzureRegion = AzureRegions.Westcentralus;

            Console.WriteLine("Images being analyzed ...");
            var t1 = ExtractRemoteHandTextAsync(computerVision, remoteImageUrl);
            var t2 = ExtractLocalHandTextAsync(computerVision, localImagePath);

            Task.WhenAll(t1, t2).Wait(5000);
            Console.WriteLine("Press any key to exit");
            Console.ReadLine();
        }

        // Recognize text from a remote image
        private static async Task ExtractRemoteHandTextAsync(
            ComputerVisionAPI computerVision, string imageUrl)
        {
            if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
            {
                Console.WriteLine(
                    "\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                return;
            }

            // Start the async process to recognize the text
            RecognizeTextHeaders textHeaders = await computerVision.RecognizeTextAsync(
                    imageUrl, TextRecognitionMode.Handwritten);

            await GetTextAsync(computerVision, textHeaders.OperationLocation);
        }

        // Recognize text from a local image
        private static async Task ExtractLocalHandTextAsync(
            ComputerVisionAPI computerVision, string imagePath)
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
                        imageStream, TextRecognitionMode.Handwritten);

                await GetTextAsync(computerVision, textHeaders.OperationLocation);
            }
        }

        // Retrieve the recognized text
        private static async Task GetTextAsync(
            ComputerVisionAPI computerVision, string operationLocation)
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
            foreach(Line line in lines)
            {
                Console.WriteLine(line.Text);
            }
            Console.WriteLine();
        }
    }
}
```

## <a name="recognizetextasync-response"></a>Resposta RecognizeTextAsync

Uma resposta com êxito apresenta as linhas de texto reconhecido para cada imagem.

Veja [Inícios Rápidos da API: Extrair texto manuscrito com C#](../QuickStarts/CSharp-hand-text.md#recognize-text-response) para obter um resultado de JSON não processado.

```cmd
Calling GetHandwritingRecognitionOperationResultAsync()

Calling GetHandwritingRecognitionOperationResultAsync()
Server status: Running, waiting 1 seconds...
Server status: Running, waiting 1 seconds...

dog
The quick brown fox jumps over the lazy
Pack my box with five dozen liquor jugs
```

## <a name="next-steps"></a>Passos seguintes

Explore as API de Imagem Digitalizada utilizadas para analisar uma imagem, detetar celebridades e marcos, criar uma miniatura e extrair texto impresso e manuscrito.

> [!div class="nextstepaction"]
> [Explorar API de Imagem Digitalizada](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
