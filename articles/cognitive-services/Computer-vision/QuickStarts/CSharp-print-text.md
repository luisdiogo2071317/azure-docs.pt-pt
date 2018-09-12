---
title: Início rápido de OCR da API de Imagem Digitalizada com C# | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido, irá extrair texto impresso de uma imagem através da Imagem Digitalizada com C# nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 46193e7aa27285a887005a67bb662449e18d7b36
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43772334"
---
# <a name="quickstart-extract-printed-text-ocr---rest-c35"></a>Início Rápido: Extrair texto impresso (OCR) – REST, C&#35;

Neste início rápido, irá extrair texto impresso, também denominado reconhecimento ótico de carateres (OCR), de uma imagem através da Imagem Digitalizada.

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar a Imagem Digitalizada, necessita de uma chave de subscrição; consulte [A Obter Chaves de Subscrição](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="ocr-request"></a>Pedido de OCR

Com o [método OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc), pode detetar texto impresso numa imagem e extrair os carateres reconhecidos para um fluxo de carateres que podem ser utilizados por um computador.

Para executar o exemplo, siga os passos seguintes:

1. Crie uma nova Aplicação da Consola Visual C# no Visual Studio.
1. Instale o pacote NuGet Newtonsoft.Json.
    1. No menu, clique em **Ferramentas**, selecione **Gestor de Pacotes NuGet** e, em seguida, **Gerir Pacotes NuGet para Solução**.
    1. Clique no separador **Procurar** e, na caixa do tipo **Pesquisa** "Newtonsoft.Json".
    1. Selecione **Newtonsoft.Json** quando for apresentado e, em seguida, clique na caixa de verificação junto do nome do seu projeto, e em **Instalar**.
1. Substitua `Program.cs` pelo código abaixo.
1. Substitua `<Subscription Key>` pela sua chave de subscrição válida.
1. Altere o valor `uriBase` para a localização onde obteve as suas chaves de subscrição, se necessário.
1. Execute o programa.
1. Na linha de comandos, introduza o caminho para uma imagem local.

```csharp
using Newtonsoft.Json.Linq;
using System;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Threading.Tasks;

namespace CSHttpClientSample
{
    static class Program
    {
        // Replace <Subscription Key> with your valid subscription key.
        const string subscriptionKey = "<Subscription Key>";

        // You must use the same region in your REST call as you used to
        // get your subscription keys. For example, if you got your
        // subscription keys from westus, replace "westcentralus" in the URL
        // below with "westus".
        //
        // Free trial subscription keys are generated in the westcentralus region.
        // If you use a free trial subscription key, you shouldn't need to change
        // this region.
        const string uriBase =
            "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/ocr";

        static void Main()
        {
            // Get the path and filename to process from the user.
            Console.WriteLine("Optical Character Recognition:");
            Console.Write("Enter the path to an image with text you wish to read: ");
            string imageFilePath = Console.ReadLine();

            if (File.Exists(imageFilePath))
            {
                // Make the REST API call.
                Console.WriteLine("\nWait a moment for the results to appear.\n");
                MakeOCRRequest(imageFilePath).Wait();
            }
            else
            {
                Console.WriteLine("\nInvalid file path");
            }
            Console.WriteLine("\nPress Enter to exit...");
            Console.ReadLine();
        }

        /// <summary>
        /// Gets the text visible in the specified image file by using
        /// the Computer Vision REST API.
        /// </summary>
        /// <param name="imageFilePath">The image file with printed text.</param>
        static async Task MakeOCRRequest(string imageFilePath)
        {
            try
            {
                HttpClient client = new HttpClient();

                // Request headers.
                client.DefaultRequestHeaders.Add(
                    "Ocp-Apim-Subscription-Key", subscriptionKey);

                // Request parameters.
                string requestParameters = "language=unk&detectOrientation=true";

                // Assemble the URI for the REST API Call.
                string uri = uriBase + "?" + requestParameters;

                HttpResponseMessage response;

                // Request body. Posts a locally stored JPEG image.
                byte[] byteData = GetImageAsByteArray(imageFilePath);

                using (ByteArrayContent content = new ByteArrayContent(byteData))
                {
                    // This example uses content type "application/octet-stream".
                    // The other content types you can use are "application/json"
                    // and "multipart/form-data".
                    content.Headers.ContentType =
                        new MediaTypeHeaderValue("application/octet-stream");

                    // Make the REST API call.
                    response = await client.PostAsync(uri, content);
                }

                // Get the JSON response.
                string contentString = await response.Content.ReadAsStringAsync();

                // Display the JSON response.
                Console.WriteLine("\nResponse:\n\n{0}\n",
                    JToken.Parse(contentString).ToString());
            }
            catch (Exception e)
            {
                Console.WriteLine("\n" + e.Message);
            }
        }

        /// <summary>
        /// Returns the contents of the specified file as a byte array.
        /// </summary>
        /// <param name="imageFilePath">The image file to read.</param>
        /// <returns>The byte array of the image data.</returns>
        static byte[] GetImageAsByteArray(string imageFilePath)
        {
            using (FileStream fileStream =
                new FileStream(imageFilePath, FileMode.Open, FileAccess.Read))
            {
                BinaryReader binaryReader = new BinaryReader(fileStream);
                return binaryReader.ReadBytes((int)fileStream.Length);
            }
        }
    }
}
```

## <a name="ocr-response"></a>Resposta de OCR

Após a conclusão com êxito, os resultados do OCR devolvidos incluem texto, caixa delimitadora para regiões, linhas e palavras, por exemplo:

```json
{
   "language": "en",
   "textAngle": -1.5000000000000335,
   "orientation": "Up",
   "regions": [
      {
         "boundingBox": "154,49,351,575",
         "lines": [
            {
               "boundingBox": "165,49,340,117",
               "words": [
                  {
                     "boundingBox": "165,49,63,109",
                     "text": "A"
                  },
                  {
                     "boundingBox": "261,50,244,116",
                     "text": "GOAL"
                  }
               ]
            },
            {
               "boundingBox": "165,169,339,93",
               "words": [
                  {
                     "boundingBox": "165,169,339,93",
                     "text": "WITHOUT"
                  }
               ]
            },
            {
               "boundingBox": "159,264,342,117",
               "words": [
                  {
                     "boundingBox": "159,264,64,110",
                     "text": "A"
                  },
                  {
                     "boundingBox": "255,266,246,115",
                     "text": "PLAN"
                  }
               ]
            },
            {
               "boundingBox": "161,384,338,119",
               "words": [
                  {
                     "boundingBox": "161,384,86,113",
                     "text": "IS"
                  },
                  {
                     "boundingBox": "274,387,225,116",
                     "text": "JUST"
                  }
               ]
            },
            {
               "boundingBox": "154,506,341,118",
               "words": [
                  {
                     "boundingBox": "154,506,62,111",
                     "text": "A"
                  },
                  {
                     "boundingBox": "248,508,247,116",
                     "text": "WISH"
                  }
               ]
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Passos seguintes

Explore uma aplicação básica do Windows que utilize a Imagem Digitalizada para realizar o reconhecimento ótico de carateres (OCR); criar miniaturas com recorte inteligente; além de detetar, categorizar, etiquetar e descrever funcionalidades visuais, incluindo rostos, numa imagem. Para experimentar rapidamente as API de Imagem Digitalizada, experimente a [Consola de teste de API aberta](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [API de Imagem Digitalizada com C&#35; Tutorial](../Tutorials/CSharpTutorial.md)
