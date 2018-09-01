---
title: Utilizar o endpoint de predição do serviço de visão personalizada - serviços cognitivos do Azure | Documentos da Microsoft
description: Saiba como utilizar a API para imagens com o seu classificador de serviço de visão personalizada de teste por meio de programação.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: d7f9b90db06811e16cd0cd6ad2b32a27912cfee5
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43341798"
---
# <a name="use-the-prediction-endpoint-to-test-images-programmatically-with-a-custom-vision-service-classifier"></a>Utilizar o ponto de final de predição para testar as imagens por meio de programação com um classificador de serviço de visão personalizada

Depois de preparar o seu modelo, pode testar imagens por meio de programação submetê-los para a API de predição. 

> [!NOTE]
> Este documento demonstra como utilizar c# para enviar uma imagem para a API de predição. Para obter mais informações e exemplos de utilização da API, consulte a [referência da API de predição](https://go.microsoft.com/fwlink/?linkid=865445).

## <a name="get-the-url-and-prediction-key"></a>Obter a chave de URL e previsão

Partir do [página da web de visão personalizada](https://customvision.ai), selecione o seu projeto e, em seguida, selecione a __desempenho__ separador. Para apresentar informações sobre como utilizar a API de predição, incluindo o __chave de predição__, selecione __URL de predição__. Para projetos anexados a um recurso do Azure, sua __chave de predição__ também podem ser encontrados no [Portal do Azure](https://portal.azure.com) página de recursos do Azure associados sob __chaves__. Copie as seguintes informações para uso no aplicativo:

* __URL__ para a utilização de um __ficheiro de imagem__.
* __Chave de predição__ valor.

> [!TIP]
> Se tiver várias iterações, pode controlar qual será usado ao defini-la como predefinição. Selecione a iteração do __iterações__ secção, em seguida, selecione __predefinir__ na parte superior da página.

![A guia de desempenho é mostrada com um retângulo vermelho que envolvem o URL de predição.](./media/use-prediction-api/prediction-url.png)

## <a name="create-the-application"></a>Criar a aplicação

1. A partir do Visual Studio, crie um novo aplicativo de console do c#.

2. Utilize o seguinte código no corpo da __Program.cs__ ficheiro.

    > [!IMPORTANT]
    > Altere as seguintes informações:
    >
    > * Definir o __espaço de nomes__ para o nome do seu projeto.
    > * Definir o __chave de predição__ valor recebido anteriormente na linha que começa com `client.DefaultRequestHeaders.Add("Prediction-Key",`.
    > * Definir o __URL__ valor recebido anteriormente na linha que começa com `string url =`.

    ```csharp
    using System;
    using System.IO;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;

    namespace CSPredictionSample
    {
        static class Program
        {
            static void Main()
            {
                Console.Write("Enter image file path: ");
                string imageFilePath = Console.ReadLine();

                MakePredictionRequest(imageFilePath).Wait();

                Console.WriteLine("\n\n\nHit ENTER to exit...");
                Console.ReadLine();
            }

            static byte[] GetImageAsByteArray(string imageFilePath)
            {
                FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
                BinaryReader binaryReader = new BinaryReader(fileStream);
                return binaryReader.ReadBytes((int)fileStream.Length);
            }

            static async Task MakePredictionRequest(string imageFilePath)
            {
                var client = new HttpClient();

                // Request headers - replace this example key with your valid subscription key.
                client.DefaultRequestHeaders.Add("Prediction-Key", "13hc77781f7e4b19b5fcdd72a8df7156");

                // Prediction URL - replace this example URL with your valid prediction URL.
                string url = "http://southcentralus.api.cognitive.microsoft.com/customvision/v1.0/prediction/d16e136c-5b0b-4b84-9341-6a3fff8fa7fe/image?iterationId=f4e573f6-9843-46db-8018-b01d034fd0f2";

                HttpResponseMessage response;

                // Request body. Try this sample with a locally stored image.
                byte[] byteData = GetImageAsByteArray(imageFilePath);

                using (var content = new ByteArrayContent(byteData))
                {
                    content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
                    response = await client.PostAsync(url, content);
                    Console.WriteLine(await response.Content.ReadAsStringAsync());
                }
            }
        }
    }
    ```

## <a name="use-the-application"></a>Utilizar a aplicação

Quando executar a aplicação, introduza o caminho para um ficheiro de imagem. A imagem foi submetida para a API e os resultados são retornados como um documento JSON. O JSON seguinte é um exemplo da resposta

```json
{
    "Id":"3f76364c-b8ae-4818-a2b2-2794cfbe377a",
    "Project":"2277aca4-7aff-4742-8afb-3682e251c913",
    "Iteration":"84105bfe-73b5-4fcc-addb-756c0de17df2",
    "Created":"2018-05-03T14:15:22.5659829Z",
    "Predictions":[
        {"TagId":"35ac2ad0-e3ef-4e60-b81f-052a1057a1ca","Tag":"dog","Probability":0.102716163},
        {"TagId":"28e1a872-3776-434c-8cf0-b612dd1a953c","Tag":"cat","Probability":0.02037274}
    ]
}
```

## <a name="next-steps"></a>Passos Seguintes

[Exportar o modelo para utilização móvel](export-your-model.md)
