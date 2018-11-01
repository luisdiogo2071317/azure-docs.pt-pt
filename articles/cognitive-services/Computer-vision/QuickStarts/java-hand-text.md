---
title: 'Guia de Início Rápido: Extrair texto manuscrito – REST, Java – Imagem Digitalizada'
titleSuffix: Azure Cognitive Services
description: Neste guia de início rápido, irá extrair texto manuscrito de uma imagem através da API de Imagem Digitalizada com o Java.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: pafarley
ms.openlocfilehash: f3600c5cf64614bcb5600b664147c9f5abb3865b
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50417261"
---
# <a name="quickstart-extract-handwritten-text-using-the-rest-api-and-java-in-computer-vision"></a>Guia de Início Rápido: Extrair texto manuscrito com a API REST e o Java na Imagem Digitalizada

Neste guia de início rápido, irá extrair texto manuscrito de uma imagem através da API REST de Imagem Digitalizada. Com os métodos [Reconhecimento de Texto](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) e [Get Recognize Text Operation Result](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2cf1154055056008f201) (Obter Resultado da Operação de Reconhecimento de Texto), pode detetar texto manuscrito numa imagem e, em seguida, extrair os carateres reconhecidos para um fluxo de carateres que podem ser utilizados por um computador.

> [!IMPORTANT]
> Ao contrário do método [OCR](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc), o método [Reconhecimento de Texto](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) é executado de forma assíncrona. Este método não devolve quaisquer informações no corpo de uma resposta de êxito. Em alternativa, o método Reconhecimento de Texto devolve um URI no valor do campo de cabeçalho de resposta `Operation-Content`. Em seguida, pode chamar este URI, que representa o método [Get Recognize Text Operation Result](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2cf1154055056008f201) (Obter Resultado da Operação de Reconhecimento de Texto), para verificar o estado e devolver os resultados da chamada do método Reconhecimento de Texto.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

- Tem de ter a plataforma [Java&trade;, Standard Edition Development Kit 7 ou 8](https://aka.ms/azure-jdks) (JDK 7 ou 8) instalada.
- Tem de ter uma chave de subscrição da Imagem Digitalizada. Para obter uma chave de subscrição, veja [Obter Chaves de Subscrição](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="create-and-run-the-sample-application"></a>Criar e executar a aplicação de exemplo

Para criar e executar o exemplo, siga os seguintes passos:

1. Crie um novo projeto Java no seu IDE ou editor favorito. Se a opção estiver disponível, crie o projeto Java a partir de um modelo de aplicação de linha de comandos.
1. Importe as seguintes bibliotecas para o projeto Java. Se estiver a utilizar o Maven, as coordenadas do Maven são fornecidas para cada biblioteca.
   - [Cliente Apache HTTP](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpclient:4.5.5)
   - [Núcleo Apache HTTP](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpcore:4.4.9)
   - [Biblioteca JSON](https://github.com/stleary/JSON-java) (org.json:json:20180130)
1. Adicione as seguintes instruções `import` ao ficheiro que contém a classe pública `Main` do seu projeto.  

   ```java
   import java.net.URI;
   import org.apache.http.HttpEntity;
   import org.apache.http.HttpResponse;
   import org.apache.http.client.methods.HttpGet;
   import org.apache.http.client.methods.HttpPost;
   import org.apache.http.client.utils.URIBuilder;
   import org.apache.http.entity.StringEntity;
   import org.apache.http.impl.client.CloseableHttpClient;
   import org.apache.http.impl.client.HttpClientBuilder;
   import org.apache.http.util.EntityUtils;
   import org.apache.http.Header;
   import org.json.JSONObject;
   ```

1. Substitua a classe pública `Main` pelo código seguinte e faça as seguintes alterações no código onde for necessário:
   1. Substitua o valor de `subscriptionKey` pela chave de subscrição.
   1. Substitua o valor de `uriBase` pelo URL de ponto final do método [Reconhecimento de Texto](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) da região do Azure onde obteve as chaves de subscrição, se necessário.
   1. Opcionalmente, substitua o valor de `imageToAnalyze` pelo URL de uma imagem diferente da qual pretende extrair texto manuscrito.
1. Guarde e, em seguida, crie o projeto Java.
1. Se estiver a utilizar um IDE, execute `Main`. Caso contrário, abra uma janela da linha de comandos e, em seguida, utilize o comando `java` para executar a classe compilada. Por exemplo, `java Main`.

```java
public class Main {
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Replace <Subscription Key> with your valid subscription key.
    private static final String subscriptionKey = "<Subscription Key>";

    // You must use the same Azure region in your REST API method as you used to
    // get your subscription keys. For example, if you got your subscription keys
    // from the West US region, replace "westcentralus" in the URL
    // below with "westus".
    //
    // Free trial subscription keys are generated in the West Central US region.
    // If you use a free trial subscription key, you shouldn't need to change
    // this region.
    private static final String uriBase =
        "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/recognizeText";

    private static final String imageToAnalyze =
        "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/" +
        "Cursive_Writing_on_Notebook_paper.jpg/800px-Cursive_Writing_on_Notebook_paper.jpg";

    public static void main(String[] args) {
        CloseableHttpClient httpTextClient = HttpClientBuilder.create().build();
        CloseableHttpClient httpResultClient = HttpClientBuilder.create().build();;

        try {
            // This operation requires two REST API calls. One to submit the image
            // for processing, the other to retrieve the text found in the image.

            URIBuilder builder = new URIBuilder(uriBase);

            // Request parameter.
            builder.setParameter("mode", "Handwritten");

            // Prepare the URI for the REST API method.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            // Request body.
            StringEntity requestEntity =
                    new StringEntity("{\"url\":\"" + imageToAnalyze + "\"}");
            request.setEntity(requestEntity);

            // Two REST API methods are required to extract handwritten text.
            // One method to submit the image for processing, the other method
            // to retrieve the text found in the image.

            // Call the first REST API method to detect the text.
            HttpResponse response = httpTextClient.execute(request);

            // Check for success.
            if (response.getStatusLine().getStatusCode() != 202) {
                // Format and display the JSON error message.
                HttpEntity entity = response.getEntity();
                String jsonString = EntityUtils.toString(entity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("Error:\n");
                System.out.println(json.toString(2));
                return;
            }

            // Store the URI of the second REST API method.
            // This URI is where you can get the results of the first REST API method.
            String operationLocation = null;

            // The 'Operation-Location' response header value contains the URI for
            // the second REST API method.
            Header[] responseHeaders = response.getAllHeaders();
            for (Header header : responseHeaders) {
                if (header.getName().equals("Operation-Location")) {
                    operationLocation = header.getValue();
                    break;
                }
            }

            if (operationLocation == null) {
                System.out.println("\nError retrieving Operation-Location.\nExiting.");
                System.exit(1);
            }

            // If the first REST API method completes successfully, the second
            // REST API method retrieves the text written in the image.
            //
            // Note: The response may not be immediately available. Handwriting
            // recognition is an asynchronous operation that can take a variable
            // amount of time depending on the length of the handwritten text.
            // You may need to wait or retry this operation.

            System.out.println("\nHandwritten text submitted.\n" +
                    "Waiting 10 seconds to retrieve the recognized text.\n");
            Thread.sleep(10000);

            // Call the second REST API method and get the response.
            HttpGet resultRequest = new HttpGet(operationLocation);
            resultRequest.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

            HttpResponse resultResponse = httpResultClient.execute(resultRequest);
            HttpEntity responseEntity = resultResponse.getEntity();

            if (responseEntity != null) {
                // Format and display the JSON response.
                String jsonString = EntityUtils.toString(responseEntity);
                JSONObject json = new JSONObject(jsonString);
                System.out.println("Text recognition result response: \n");
                System.out.println(json.toString(2));
            }
        } catch (Exception e) {
            System.out.println(e.getMessage());
        }
    }
}
```

## <a name="examine-the-response"></a>Examinar a resposta

O JSON devolve uma resposta de êxito. A aplicação de exemplo analisa e apresenta uma resposta de êxito na janela da consola, semelhante ao seguinte exemplo:

```json
Handwritten text submitted. Waiting 10 seconds to retrieve the recognized text.

Text recognition result response:

{
  "status": "Succeeded",
  "recognitionResult": {"lines": [
    {
      "boundingBox": [
        2,
        84,
        783,
        96,
        782,
        154,
        1,
        148
      ],
      "words": [
        {
          "boundingBox": [
            6,
            86,
            92,
            87,
            71,
            151,
            0,
            150
          ],
          "text": "Pack"
        },
        {
          "boundingBox": [
            86,
            87,
            172,
            88,
            150,
            152,
            64,
            151
          ],
          "text": "my"
        },
        {
          "boundingBox": [
            165,
            88,
            241,
            89,
            219,
            152,
            144,
            152
          ],
          "text": "box"
        },
        {
          "boundingBox": [
            234,
            89,
            343,
            90,
            322,
            154,
            213,
            152
          ],
          "text": "with"
        },
        {
          "boundingBox": [
            347,
            90,
            432,
            91,
            411,
            154,
            325,
            154
          ],
          "text": "five"
        },
        {
          "boundingBox": [
            432,
            91,
            538,
            92,
            516,
            154,
            411,
            154
          ],
          "text": "dozen"
        },
        {
          "boundingBox": [
            554,
            92,
            696,
            94,
            675,
            154,
            533,
            154
          ],
          "text": "liquor"
        },
        {
          "boundingBox": [
            710,
            94,
            800,
            96,
            800,
            154,
            688,
            154
          ],
          "text": "jugs"
        }
      ],
      "text": "Pack my box with five dozen liquor jugs"
    },
    {
      "boundingBox": [
        2,
        52,
        65,
        46,
        69,
        89,
        7,
        95
      ],
      "words": [{
        "boundingBox": [
          0,
          62,
          79,
          39,
          94,
          82,
          0,
          105
        ],
        "text": "dog"
      }],
      "text": "dog"
    },
    {
      "boundingBox": [
        6,
        2,
        771,
        13,
        770,
        75,
        5,
        64
      ],
      "words": [
        {
          "boundingBox": [
            8,
            4,
            92,
            5,
            77,
            71,
            0,
            71
          ],
          "text": "The"
        },
        {
          "boundingBox": [
            89,
            5,
            188,
            5,
            173,
            72,
            74,
            71
          ],
          "text": "quick"
        },
        {
          "boundingBox": [
            188,
            5,
            323,
            6,
            308,
            73,
            173,
            72
          ],
          "text": "brown"
        },
        {
          "boundingBox": [
            316,
            6,
            386,
            6,
            371,
            73,
            302,
            73
          ],
          "text": "fox"
        },
        {
          "boundingBox": [
            396,
            7,
            508,
            7,
            493,
            74,
            381,
            73
          ],
          "text": "jumps"
        },
        {
          "boundingBox": [
            501,
            7,
            604,
            8,
            589,
            75,
            487,
            74
          ],
          "text": "over"
        },
        {
          "boundingBox": [
            600,
            8,
            673,
            8,
            658,
            75,
            586,
            75
          ],
          "text": "the"
        },
        {
          "boundingBox": [
            670,
            8,
            800,
            9,
            787,
            76,
            655,
            75
          ],
          "text": "lazy"
        }
      ],
      "text": "The quick brown fox jumps over the lazy"
    }
  ]}
}
```

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine o projeto Java, incluindo a classe compilada e as bibliotecas importadas.

## <a name="next-steps"></a>Passos seguintes

Explore uma aplicação do Java Swing que utilize a Imagem Digitalizada para realizar o reconhecimento ótico de carateres (OCR); criar miniaturas com recorte inteligente; além de detetar, categorizar, etiquetar e descrever funcionalidades visuais, incluindo rostos, numa imagem. Para experimentar rapidamente a API de Imagem Digitalizada, experimente a [Consola de teste de API aberta](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Tutorial de Java de API de Imagem Digitalizada](../Tutorials/java-tutorial.md)
