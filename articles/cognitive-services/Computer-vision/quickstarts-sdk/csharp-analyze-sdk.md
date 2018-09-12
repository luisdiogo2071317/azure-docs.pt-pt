---
title: Início rápido SDK de análise de uma imagem local de API de Imagem Digitalizada com C# | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido, irá analisar uma imagem através da biblioteca de cliente da Imagem Digitalizada do Windows com C# nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 3ff3a4702ab0b1fb663ee896f268065caf043809
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43772327"
---
# <a name="quickstart-analyze-an-image---sdk-c35"></a>Início rápido: Analisar uma imagem – SDK, C&#35;

Neste início rápido, analisa tanto uma imagem local como uma remota para extrair funcionalidades visuais através da biblioteca de cliente da Imagem Digitalizada do Windows.

## <a name="prerequisites"></a>Pré-requisitos

* Para utilizar a Imagem Digitalizada, necessita de uma chave de subscrição; consulte [A Obter Chaves de Subscrição](../Vision-API-How-to-Topics/HowToSubscribe.md).
* Qualquer edição do [Visual Studio 2015 ou 2017](https://www.visualstudio.com/downloads/).
* O pacote NuGet da biblioteca de cliente [Microsoft.Azure.CognitiveServices.Vision.ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision). Não é necessário transferir o pacote. Abaixo, são fornecidas as instruções de instalação.

## <a name="analyzeimageasync-method"></a>Método AnalyzeImageAsync

Os métodos `AnalyzeImageAsync` e `AnalyzeImageInStreamAsync` encapsulam a [API de Análise de Imagem](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) para imagens remotas e locais, respetivamente. Pode utilizar estes métodos para extrair funcionalidades visuais com base no conteúdo da imagem e escolher as funcionalidades que deve devolver, incluindo:

* Uma lista detalhada das etiquetas relacionadas com o conteúdo da imagem.
* Uma descrição do conteúdo da imagem numa frase completa.
* As coordenadas, o sexo e a idade de qualquer rosto que a imagem contenha.
* O ImageType (ClipArt ou um desenho de linha).
* A cor dominante, a cor de destaque ou se uma imagem é a preto e branco.
* A categoria definida nesta [taxonomia](../Category-Taxonomy.md).
* A imagem contém conteúdo para adultos ou é sexualmente sugestiva?

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
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;

namespace ImageAnalyze
{
    class Program
    {
        // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // localImagePath = @"C:\Documents\LocalImage.jpg"
        private const string localImagePath = @"<LocalImage>";

        private const string remoteImageUrl =
            "http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg";

        // Specify the features to return
        private static readonly List<VisualFeatureTypes> features =
            new List<VisualFeatureTypes>()
        {
            VisualFeatureTypes.Categories, VisualFeatureTypes.Description,
            VisualFeatureTypes.Faces, VisualFeatureTypes.ImageType,
            VisualFeatureTypes.Tags
        };

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
            var t1 = AnalyzeRemoteAsync(computerVision, remoteImageUrl);
            var t2 = AnalyzeLocalAsync(computerVision, localImagePath);

            Task.WhenAll(t1, t2).Wait(5000);
            Console.WriteLine("Press any key to exit");
            Console.ReadLine();
        }

        // Analyze a remote image
        private static async Task AnalyzeRemoteAsync(
            ComputerVisionAPI computerVision, string imageUrl)
        {
            if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
            {
                Console.WriteLine(
                    "\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                return;
            }

            ImageAnalysis analysis =
                await computerVision.AnalyzeImageAsync(imageUrl, features);
            DisplayResults(analysis, imageUrl);
        }

        // Analyze a local image
        private static async Task AnalyzeLocalAsync(
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
                ImageAnalysis analysis = await computerVision.AnalyzeImageInStreamAsync(
                    imageStream, features);
                DisplayResults(analysis, imagePath);
            }
        }

        // Display the most relevant caption for the image
        private static void DisplayResults(ImageAnalysis analysis, string imageUri)
        {
            Console.WriteLine(imageUri);
            Console.WriteLine(analysis.Description.Captions[0].Text + "\n");
        }
    }
}
```

## <a name="analyzeimageasync-response"></a>Método AnalyzeImageAsync

Uma resposta de êxito apresenta a legenda mais relevante para cada imagem.

Consulte [Inícios Rápidos da API: Analisar uma imagem local com C#](../QuickStarts/CSharp-analyze.md#analyze-image-response) para obter um exemplo de uma saída JSON não processada.

```cmd
http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg
a large waterfall over a rocky cliff
```

## <a name="next-steps"></a>Passos seguintes

Explore as API de Imagem Digitalizada utilizadas para analisar uma imagem, detetar celebridades e marcos, criar uma miniatura e extrair texto impresso e manuscrito.

> [!div class="nextstepaction"]
> [Explorar API de Imagem Digitalizada](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
