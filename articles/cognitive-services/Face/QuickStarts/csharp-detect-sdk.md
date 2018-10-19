---
title: 'Guia de Início Rápido: Detetar rostos numa imagem – SDK, C#'
titleSuffix: Azure Cognitive Services
description: Neste guia de início rápido, irá detetar rostos numa imagem com a biblioteca de cliente em C# para Windows do Face nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: quickstart
ms.date: 09/14/2018
ms.author: nolachar
ms.openlocfilehash: a9a7efd89f8e7462812064615d07acf12acbc3a3
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364118"
---
# <a name="quickstart-detect-faces-in-an-image-using-c35---face"></a>Guia de Início Rápido: Detetar rostos numa imagem com C&#35; - Face

Neste guia de início rápido, irá detetar rostos humanos numa imagem com a biblioteca de cliente para Windows do Face.

O código fonte para este exemplo está disponível no [Github](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/Face).

## <a name="prerequisites"></a>Pré-requisitos

* Precisa de uma chave de subscrição para executar o exemplo. Pode obter chaves de subscrição de avaliação gratuita em [Experimente os Serviços Cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=face-api).
* Qualquer edição do [Visual Studio 2017](https://www.visualstudio.com/downloads/).
* O pacote NuGet da biblioteca de cliente [Microsoft.Azure.CognitiveServices.Vision.Face 2.2.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview). Não é necessário transferir o pacote. Abaixo, são fornecidas as instruções de instalação.

## <a name="detectwithurlasync-method"></a>Método DetectWithUrlAsync

Os métodos `DetectWithUrlAsync` e `DetectWithStreamAsync` encapsulam num wrapper a [API Rosto – Detetar](https://westcentralus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) para imagens remotas e locais, respetivamente. Pode utilizar estes métodos para detetar rostos numa imagem e devolver atributos de rosto, incluindo:

* Face ID: ID exclusivo utilizado em vários cenários da API Face.
* Retângulo de Rostos: a esquerda, o topo, a largura e a altura indicam a localização do rosto na imagem, a largura e a altura indicam a localização do rosto na imagem.
* Marcos: uma matriz de marcos de rosto com 27 pontos aponta para as posições importantes dos componentes do rosto.
* Atributos faciais, como a idade, o sexo, a intensidade do sorriso, a posição da cabeça e os pelos faciais.

Para executar o exemplo, siga os seguintes passos:

1. Crie uma nova Aplicação da Consola Visual C# no Visual Studio.
1. Instale o pacote NuGet da biblioteca de cliente da API Face.
    1. No menu superior, clique em **Ferramentas**, selecione **Gestor de Pacotes NuGet** e, em seguida, **Gerir Pacotes NuGet para Solução**.
    1. Clique no separador **Procurar** e, em seguida, selecione **Incluir pré-lançamento**.
    1. Na caixa **Procurar**, escreva "Microsoft.Azure.CognitiveServices.Vision.Face".
    1. Selecione **Microsoft.Azure.CognitiveServices.Vision.Face** quando for apresentado e, em seguida, clique na caixa de verificação junto ao nome do projeto e em **Instalar**.
1. Substitua *Program.cs* pelo seguinte código.
1. Substitua `<Subscription Key>` pela sua chave de subscrição válida.
1. Altere `faceEndpoint` para a região do Azure associada às chaves de subscrição, se necessário.
1. Opcionalmente, substitua <`LocalImage>` pelo caminho e pelo nome de ficheiro de uma imagem local (se não for definido, é ignorado).
1. Opcionalmente, defina `remoteImageUrl` para uma imagem diferente.
1. Execute o programa.

```csharp
using Microsoft.Azure.CognitiveServices.Vision.Face;
using Microsoft.Azure.CognitiveServices.Vision.Face.Models;

using System;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;

namespace DetectFace
{
    class Program
    {
        // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // You must use the same region as you used to get your subscription
        // keys. For example, if you got your subscription keys from westus,
        // replace "westcentralus" with "westus".
        //
        // Free trial subscription keys are generated in the westcentralus
        // region. If you use a free trial subscription key, you shouldn't
        // need to change the region.
        // Specify the Azure region
        private const string faceEndpoint =
            "https://westcentralus.api.cognitive.microsoft.com";

        // localImagePath = @"C:\Documents\LocalImage.jpg"
        private const string localImagePath = @"<LocalImage>";

        private const string remoteImageUrl =
            "https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg";

        private static readonly FaceAttributeType[] faceAttributes =
            { FaceAttributeType.Age, FaceAttributeType.Gender };

        static void Main(string[] args)
        {
            FaceClient faceClient = new FaceClient(
                new ApiKeyServiceClientCredentials(subscriptionKey),
                new System.Net.Http.DelegatingHandler[] { });
            faceClient.Endpoint = faceEndpoint;

            Console.WriteLine("Faces being detected ...");
            var t1 = DetectRemoteAsync(faceClient, remoteImageUrl);
            var t2 = DetectLocalAsync(faceClient, localImagePath);

            Task.WhenAll(t1, t2).Wait(5000);
            Console.WriteLine("Press any key to exit");
            Console.ReadLine();
        }

        // Detect faces in a remote image
        private static async Task DetectRemoteAsync(
            FaceClient faceClient, string imageUrl)
        {
            if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
            {
                Console.WriteLine("\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                return;
            }

            try
            {
                IList<DetectedFace> faceList =
                    await faceClient.Face.DetectWithUrlAsync(
                        imageUrl, true, false, faceAttributes);

                DisplayAttributes(GetFaceAttributes(faceList, imageUrl), imageUrl);
            }
            catch (APIErrorException e)
            {
                Console.WriteLine(imageUrl + ": " + e.Message);
            }
        }

        // Detect faces in a local image
        private static async Task DetectLocalAsync(FaceClient faceClient, string imagePath)
        {
            if (!File.Exists(imagePath))
            {
                Console.WriteLine(
                    "\nUnable to open or read localImagePath:\n{0} \n", imagePath);
                return;
            }

            try
            {
                using (Stream imageStream = File.OpenRead(imagePath))
                {
                    IList<DetectedFace> faceList =
                            await faceClient.Face.DetectWithStreamAsync(
                                imageStream, true, false, faceAttributes);
                    DisplayAttributes(
                        GetFaceAttributes(faceList, imagePath), imagePath);
                }
            }
            catch (APIErrorException e)
            {
                Console.WriteLine(imagePath + ": " + e.Message);
            }
        }

        private static string GetFaceAttributes(
            IList<DetectedFace> faceList, string imagePath)
        {
            string attributes = string.Empty;

            foreach (DetectedFace face in faceList)
            {
                double? age = face.FaceAttributes.Age;
                string gender = face.FaceAttributes.Gender.ToString();
                attributes += gender + " " + age + "   ";
            }

            return attributes;
        }

        // Display the face attributes
        private static void DisplayAttributes(string attributes, string imageUri)
        {
            Console.WriteLine(imageUri);
            Console.WriteLine(attributes + "\n");
        }
    }
}
```

### <a name="detectwithurlasync-response"></a>Resposta DetectWithUrlAsync

As respostas bem-sucedidas apresentam o género e a idade de cada rosto na imagem.

Veja [Guias de Início Rápido da API: detetar rostos numa imagem com C#](CSharp.md) para obter um exemplo de uma saída em JSON não processada.

```
https://upload.wikimedia.org/wikipedia/commons/3/37/Dagestani_man_and_woman.jpg
Male 37   Female 56
```

## <a name="next-steps"></a>Passos seguintes

Saiba como criar uma aplicação WPF do Windows que utilize o serviço Face para detetar rostos numa imagem. A aplicação desenha uma moldura em torno de cada rosto e apresenta uma descrição do rosto na barra de estado.

> [!div class="nextstepaction"]
> [Tutorial: Criar uma aplicação do WPF para detetar e enquadrar rostos numa imagem](../Tutorials/FaceAPIinCSharpTutorial.md)
