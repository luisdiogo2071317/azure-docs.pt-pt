---
title: Emoções API c# introdução | Microsoft Docs
description: Obter informações e um exemplo de código para o ajudar a começar rapidamente, utilizando a API de emoções com c# nos serviços cognitivos.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 11/02/2017
ms.author: anroth
ms.openlocfilehash: acf30e7aded92c6d07331089fabd02836a8f3e9a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352423"
---
# <a name="emotion-api-c-quick-start"></a>Emoções API c# início rápido

> [!IMPORTANT]
> A Pré-visualização da API de Vídeo terminou a 30 de outubro de 2017. Para extrair facilmente insights vídeos, experimente a nova [pré-visualização do vídeo indexador API](https://azure.microsoft.com/services/cognitive-services/video-indexer/). Também pode utilizá-lo a melhorar as experiências de deteção de conteúdos, tais como resultados de pesquisa, através da deteção palavras ditas, faces, carateres e emotions. Para obter mais informações, consulte o [pré-visualização do vídeo indexador](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview) descrição geral.

Este artigo fornece informações e um exemplo de código para o ajudar a rapidamente começar, utilizando o [emoções API reconhece o método](https://dev.projectoxford.ai/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) com c#. Pode utilizá-lo para reconhecer os emotions expressados por um ou mais pessoas numa imagem. 

## <a name="prerequisites"></a>Pré-requisitos
* Obter os serviços cognitivos [emoções API Windows SDK](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/).
* Obter o livre [chave de subscrição](https://azure.microsoft.com/try/cognitive-services/).

## <a name="emotion-recognition-c-example-request"></a>Exemplo de pedido de emoções reconhecimento c#

Criar uma nova solução de consola no Visual Studio e, em seguida, substitua Program.cs com o seguinte código. Alterar o `string uri` para utilizar a região onde obteve as chaves de subscrição. Substitua o **Ocp-Apim-Subscription-Key** valor com a sua chave de subscrição válido. Para localizar a chave de subscrição, aceda ao portal do Azure. No painel de navegação à esquerda, sob o **chaves** secção, navegue para o recurso de emoções API. Da mesma forma, pode obter o adequado ligar URI no **descrição geral** painel para o seu recurso listado na **Endpoint**.

![As chaves de recursos de API](../../media/emotion-api/keys.png)

Para processar a resposta do pedido, utilize uma biblioteca como `Newtonsoft.Json`. Desta forma pode processar uma cadeia JSON como uma série de objetos geríveis chamado Tokens. Para adicionar esta biblioteca ao seu pacote, clique no projeto no Explorador de soluções e selecione **gerir pacotes Nuget**. Em seguida, procure **Newtonsoft**. O resultado primeiro deve ser **newtonsoft**. Selecione **Instalar**. Agora, pode referenciar esta biblioteca na sua aplicação.

![Instalar newtonsoft](../../media/emotion-api/newtonsoft-nuget.png)

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

## <a name="recognize-emotions-sample-response"></a>Reconhecer a resposta de exemplo emotions
Uma chamada com êxito devolve uma matriz de entradas de letra e os respetivos pontuações de emoções associado. Estes estão ordenadas pelo tamanho do retângulo de rostos em ordem descendente. Uma resposta vazia indica que não existem faces foram detetados. Uma entrada de emoções contém os seguintes campos:

* faceRectangle: localização do retângulo de rosto na imagem
* pontuações: emoções pontuações para cada enfrentam reside na imagem 

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
