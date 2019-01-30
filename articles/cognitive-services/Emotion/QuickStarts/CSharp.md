---
title: 'Início rápido: Reconhecer emoções no rostos numa imagem - API de emoçõesC#'
titlesuffix: Azure Cognitive Services
description: Obtenha informações e um exemplo de código para o ajudar a começar a utilizar rapidamente a API de Emoções com C#.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: quickstart
ms.date: 11/02/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: 45282c4ad534930fa72a8d8fb71105dca4af23de
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55218245"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Início rápido: Crie uma aplicação para reconhecer emoções no rostos numa imagem.

> [!IMPORTANT]
> A API de Emoções vai ser preterida no dia 15 de fevereiro de 2019. A função de reconhecimento de emoções está agora geralmente disponível como parte da [API Face](https://docs.microsoft.com/azure/cognitive-services/face/).

Este artigo disponibiliza informações e um exemplo de código para o ajudar a começar a utilizar rapidamente o [método de Reconhecimento da API de Emoções](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) com C#. Pode utilizá-lo para reconhecer as emoções expressas por uma ou mais pessoas numa imagem.

## <a name="prerequisites"></a>Pré-requisitos
* Obtenha o [Windows SDK da API de Emoções](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/) dos Serviços Cognitivos.
* Obtenha a sua [chave de subscrição](https://azure.microsoft.com/try/cognitive-services/) gratuita.

## <a name="emotion-recognition-c-example-request"></a>Exemplo de pedido em C# de reconhecimento de emoções

Crie uma nova solução de Consola no Visual Studio e, em seguida, substitua Program.cs pelo código seguinte. Altere o `string uri` para utilizar a região na qual obteve as suas chaves de subscrição. Substitua o valor de **Ocp-Apim-Subscription-Key** pela sua chave de subscrição válida. Para localizar a chave de subscrição, aceda ao portal do Azure. No painel de navegação à esquerda, na secção **Chaves**, navegue para o recurso da API de Emoções. Da mesma forma, pode obter o URI de ligação adequado no painel **Descrição Geral** para o seu recurso, listado em **Ponto Final**.

![As chaves de recurso da API](../../media/emotion-api/keys.png)

Para processar a resposta do seu pedido, utilize uma biblioteca como `Newtonsoft.Json`. Desta forma, pode processar uma cadeia de caracteres JSON como uma série de objetos geríveis, chamados Tokens. Para adicionar esta biblioteca ao seu pacote, clique com o botão direito do rato no Explorador de Soluções e selecione **Gerir Pacotes Nuget**. Em seguida, procure **Newtonsoft**. O primeiro resultado deve ser **Newtonsoft.Json**. Selecione **Instalar**. Agora, pode referenciar esta biblioteca na sua aplicação.

![Instalar o Newtonsoft.Json](../../media/emotion-api/newtonsoft-nuget.png)

```csharp
using System;
using System.IO;
using System.Net.Http.Headers;
using System.Net.Http;
using Newtonsoft.Json.Linq;

namespace CSHttpClientSample
{
    static class Program
    {
        static void Main()
        {
            Console.Write("Enter the path to a JPEG image file:");
            string imageFilePath = Console.ReadLine();

            MakeRequest(imageFilePath);

            Console.WriteLine("\n\n\nWait for the result below, then hit ENTER to exit...\n\n\n");
            Console.ReadLine(); // wait for ENTER to exit program
        }

        static byte[] GetImageAsByteArray(string imageFilePath)
        {
            FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
            BinaryReader binaryReader = new BinaryReader(fileStream);
            return binaryReader.ReadBytes((int)fileStream.Length);
        }

        static async void MakeRequest(string imageFilePath)
        {
            var client = new HttpClient();

            // Request headers - replace this example key with your valid key.
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "<your-subscription-key>"); //

            // NOTE: You must use the same region in your REST call as you used to obtain your subscription keys.
            //   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the
            //   URI below with "westcentralus".
            string uri = "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize?";
            HttpResponseMessage response;
            string responseContent;

            // Request body. Try this sample with a locally stored JPEG image.
            byte[] byteData = GetImageAsByteArray(imageFilePath);

            using (var content = new ByteArrayContent(byteData))
            {
                // This example uses content type "application/octet-stream".
                // The other content types you can use are "application/json" and "multipart/form-data".
                content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
                response = await client.PostAsync(uri, content);
                responseContent = response.Content.ReadAsStringAsync().Result;
            }

            // A peek at the raw JSON response.
            Console.WriteLine(responseContent);

            // Processing the JSON into manageable objects.
            JToken rootToken = JArray.Parse(responseContent).First;

            // First token is always the faceRectangle identified by the API.
            JToken faceRectangleToken = rootToken.First;

            // Second token is all emotion scores.
            JToken scoresToken = rootToken.Last;

            // Show all face rectangle dimensions
            JEnumerable<JToken> faceRectangleSizeList = faceRectangleToken.First.Children();
            foreach (var size in faceRectangleSizeList) {
                Console.WriteLine(size);
            }

            // Show all scores
            JEnumerable<JToken> scoreList = scoresToken.First.Children();
            foreach (var score in scoreList) {
                Console.WriteLine(score);
            }
        }
    }
}
```

## <a name="recognize-emotions-sample-response"></a>Resposta de exemplo para reconhecer emoções
Uma chamada bem-sucedida devolve uma matriz de entradas de rostos e as pontuações das respetivas emoções associadas. São ordenadas pelo tamanho dos retângulos de rostos em ordem descendente. Uma resposta vazia indica que não foram detetados rostos. Uma entrada de emoção contém os seguintes campos:

* faceRectangle: Localização do retângulo de rosto na imagem
* classificações: Pontuações de emoções para cada rosto na imagem

```json
application/json
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]
