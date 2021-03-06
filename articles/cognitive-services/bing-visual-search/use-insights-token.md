---
title: Com o token de insights - pesquisa Visual do Bing
titleSuffix: Azure Cognitive Services
description: Mostra como utilizar o insight de uma imagem token com a API pesquisa Visual do Bing para obter informações sobre uma imagem.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: conceptual
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: a1a2128e4e13494a76dfad9f051be56e02dbd995
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878804"
---
# <a name="using-an-insights-token-to-get-insights-about-an-image"></a>Com um token de informações para obter informações sobre uma imagem

A API de Pesquisa Visual do Bing devolve informações sobre uma imagem que indicar. Pode fornecer a imagem através do URL da mesma, de um token de informações ou do carregamento da mesma. Para obter informações sobre estas opções, consulte [o que é a API de pesquisa Visual do Bing?](overview.md). Este artigo demonstra como utilizar um token de informações. Para exemplos que demonstram a carregar uma imagem para obter informações, veja os inícios rápidos ([c#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node. js](quickstarts/nodejs.md)  |  [Python](quickstarts/python.md)).


Se enviar o token ou o URL de uma imagem para a Pesquisa Visual, o seguinte mostra os dados de formato que tem de incluir no corpo de POST. Os dados do formulário tem de incluir o cabeçalho Content-Disposition e a respetiva `name` parâmetro tem de ser definido como "knowledgeRequest". Para obter detalhes sobre o `imageInfo` de objeto, consulte o pedido.

```json
{
    "imageInfo" : {
        "url" : "",
        "imageInsightsToken" : "",
        "cropArea" : {
            "top" : 0.1,
            "left" : 0.5,
            "right" : 0.9,
            "bottom" : 0.9
        }
    },
    "knowledgeRequest" : {
      "filters" : {
        "site" : ""
      }
    }
}
```

Os exemplos neste artigo mostram como utilizar o token de informações. Obtenha o token de informações de um objeto de imagem numa resposta de API /images/search. Para obter informações sobre como obter o token de informações, consulte [API de pesquisa de imagens do Bing](../Bing-Image-Search/overview.md).

```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "imageInsightsToken" : "ccid_tmaGQ2eU*mid_D12339146CFEDF3D40...",
    }
}

--boundary_1234-abcd--
```


Para obter exemplos que utilizam o token de informações, consulte [c#](#using-csharp) | [Java](#using-java) | [node. js](#using-nodejs) | [Python](#using-python).

<a name="using-csharp" />

## <a name="using-c"></a>Utilizar C#

### <a name="prerequisites"></a>Pré-requisitos

Precisa do [Visual Studio 2017](https://www.visualstudio.com/downloads/) para executar este código no Windows. (A Edição de Comunidade gratuita irá funcionar.)

Neste guia de início rápido, pode utilizar uma chave de subscrição de uma [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ou uma chave de subscrição paga.

## <a name="running-the-application"></a>Executar a aplicação

Para executar esta aplicação, siga os passos abaixo:

1. Crie uma solução de Consola nova no Visual Studio.
1. Substitua os conteúdos de `Program.cs` pelo código mostrado neste início rápido.
2. Substitua o valor `accessKey` pela sua chave de subscrição.
2. Substitua o `insightsToken` valor com um token de informações a partir de uma resposta de pesquisa/imagens /.
3. Execute o programa.

```csharp
using System;
using System.Text;
using System.Net;
using System.IO;
using System.Collections.Generic;
using System.Net.Http;
using System.Threading.Tasks;
using System.Threading;

namespace VisualSearchInsightsToken
{

    class Program
    {
        // Replace the accessKey string value with your valid subscription key.
        const string accessKey = "<yoursubscriptionkeygoeshere>";

        const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";

        // Update with an insights token from an image object that the /images/search API endpoint returns.
        static string insightsToken = @"ccid_tmaGQ2eU*mid_D12339146CFEDF3D409CC7A66D2C98D0D71904D4*simid_608022145667564759*thid_OIP.tmaGQ2eUI1yq3yll!_jn9kwHaFZ";

        static string BoundaryTemplate = "batch_{0}";

        static void Main(string[] args)
        {
            try { 
                Console.WriteLine("Getting image insights using insights token: " + insightsToken);

                var knowledgeRequest = "{\"imageInfo\" : {\"imageInsightsToken\" : \"" + insightsToken + "\"}}";
                var boundary = string.Format(BoundaryTemplate, Guid.NewGuid());

                var json = BingVisualSearch(knowledgeRequest, boundary, uriBase, accessKey);

                Console.WriteLine("\nJSON Response:\n");
                Console.WriteLine(JsonPrettyPrint(json));

                Console.Write("\nPress Enter to exit ");
                Console.ReadLine();
            }
            catch (Exception e)
            {
                Console.WriteLine(e.Message);
            }

        }


        /// <summary>
        /// Calls the Bing visual search endpoint and returns the JSON response with insights.
        /// </summary>
        static string BingVisualSearch(string knowledgeRequest, string boundary, string uri, string subscriptionKey)
        {
            var requestMessage = new HttpRequestMessage(HttpMethod.Post, uri);
            requestMessage.Headers.Add("Ocp-Apim-Subscription-Key", accessKey);

            var content = new MultipartFormDataContent(boundary);
            content.Add(new StringContent(knowledgeRequest, Encoding.UTF8, "application/json"), "knowledgeRequest");
            requestMessage.Content = content;

            var httpClient = new HttpClient();

            Task<HttpResponseMessage> httpRequest = httpClient.SendAsync(requestMessage, HttpCompletionOption.ResponseContentRead, CancellationToken.None);
            HttpResponseMessage httpResponse = httpRequest.Result;
            HttpStatusCode statusCode = httpResponse.StatusCode;
            HttpContent responseContent = httpResponse.Content;

            string json = null;

            if (responseContent != null)
            {
                Task<String> stringContentsTask = responseContent.ReadAsStringAsync();
                json = stringContentsTask.Result;
            }


            return json;
        }


        /// <summary>
        /// Formats the given JSON string by adding line breaks and indents.
        /// </summary>
        /// <param name="json">The raw JSON string to format.</param>
        /// <returns>The formatted JSON string.</returns>
        static string JsonPrettyPrint(string json)
        {
            if (string.IsNullOrEmpty(json))
                return string.Empty;

            json = json.Replace(Environment.NewLine, "").Replace("\t", "");

            StringBuilder sb = new StringBuilder();
            bool quote = false;
            bool ignore = false;
            char last = ' ';
            int offset = 0;
            int indentLength = 2;

            foreach (char ch in json)
            {
                switch (ch)
                {
                    case '"':
                        if (!ignore) quote = !quote;
                        break;
                    case '\\':
                        if (quote && last != '\\') ignore = true;
                        break;
                }

                if (quote)
                {
                    sb.Append(ch);
                    if (last == '\\' && ignore) ignore = false;
                }
                else
                {
                    switch (ch)
                    {
                        case '{':
                        case '[':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', ++offset * indentLength));
                            break;
                        case '}':
                        case ']':
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', --offset * indentLength));
                            sb.Append(ch);
                            break;
                        case ',':
                            sb.Append(ch);
                            sb.Append(Environment.NewLine);
                            sb.Append(new string(' ', offset * indentLength));
                            break;
                        case ':':
                            sb.Append(ch);
                            sb.Append(' ');
                            break;
                        default:
                            if (quote || ch != ' ') sb.Append(ch);
                            break;
                    }
                }
                last = ch;
            }

            return sb.ToString().Trim();
        }
    }
}
```

<a name="using-java" />

## <a name="using-java"></a>Utilizar o Java

### <a name="prerequisites"></a>Pré-requisitos

Irá precisar do [JDK 7 ou 8](https://aka.ms/azure-jdks) para compilar e executar este código. Pode utilizar um IDE Java se tiver um favorito, mas um editor de texto será suficiente.

Neste guia de início rápido, pode utilizar uma chave de subscrição de uma [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ou uma chave de subscrição paga.

## <a name="running-the-application"></a>Executar a aplicação

Para executar esta aplicação, siga os passos abaixo:

1. Transfira ou instale a [biblioteca gson](https://github.com/google/gson). Também pode obtê-la através do Maven.
2. Crie um novo projeto Java no seu IDE ou editor favorito.
3. Adicione o código fornecido a um ficheiro com o nome `VisualSearch.java`.
4. Substitua o valor `subscriptionKey` pela sua chave de subscrição.
5. Execute o programa.

```java
package insightstoken;

import java.util.*;
import java.io.*;



/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.2
 *
 * Once you have compiled or downloaded gson-2.8.2.jar, assuming you have placed it in the
 * same folder as this file (BingImageSearch.java), you can compile and run this program at
 * the command line as follows.
 *
 * javac BingImageSearch.java -classpath .;gson-2.8.2.jar -encoding UTF-8
 * java -cp .;gson-2.8.2.jar BingImageSearch
 */

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

// http://hc.apache.org/downloads.cgi (HttpComponents Downloads) HttpClient 4.5.5

import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.ContentType;
import org.apache.http.entity.mime.MultipartEntityBuilder;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClientBuilder;


public class InsightsToken {

    
    static String endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "<yoursubscriptionkeygoeshere>";

    // To get an insights, call the /images/search endpoint. Get the token from
    // the imageInsightsToken field in the Image object.
    static String insightsToken = "ccid_tmaGQ2eU*mid_D12339146CFEDF3D409CC7A66D2C98D0D71904D4*simid_608022145667564759*thid_OIP.tmaGQ2eUI1yq3yll!_jn9kwHaFZ";

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        
        String knowledgeRequest = "{\"imageInfo\" : {\"imageInsightsToken\" : \"" + insightsToken + "\"}}";

        try {
            HttpEntity entity = MultipartEntityBuilder
                .create()
                .addTextBody("knowledgeRequest", knowledgeRequest, ContentType.create("application/json"))
                .build();

            HttpPost httpPost = new HttpPost(endpoint);
            httpPost.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
            httpPost.setEntity(entity);
            HttpResponse response = httpClient.execute(httpPost);

            InputStream stream = response.getEntity().getContent();
            String json = new Scanner(stream).useDelimiter("\\A").next();

            System.out.println("\nJSON Response:\n");
            System.out.println(prettify(json));
        }
        catch (IOException e)
        {
            e.printStackTrace(System.out);
            System.exit(1);
        }
        catch (Exception e) {
            e.printStackTrace(System.out);
            System.exit(1);
        }
    }
    
    // pretty-printer for JSON; uses GSON parser to parse and re-serialize
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    
}
```


<a name="using-nodejs" />

## <a name="using-nodejs"></a>Com Node.js

### <a name="prerequisites"></a>Pré-requisitos

Vai necessitar do [Node.js 6](https://nodejs.org/en/download/) para executar este código.

Neste guia de início rápido, pode utilizar uma chave de subscrição de uma [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ou uma chave de subscrição paga.

## <a name="running-the-application"></a>Executar a aplicação

Para executar esta aplicação, siga os passos abaixo:

1. Crie uma pasta para o seu projeto (ou utilize o seu IDE ou editor favorito).
2. Numa linha de comandos ou terminal, navegue para a pasta que acabou de criar.
3. Instale os módulos do pedido:  
  ```  
  npm install request  
  ```  
3. Instale os módulos de dados de formulário:  
  ```  
  npm install form-data  
  ```  
4. Crie um ficheiro com o nome GetVisualInsights.js e adicione-lhe o seguinte código.
5. Substitua o valor `subscriptionKey` pela sua chave de subscrição.
7. Execute o programa.  
  ```
  node GetVisualInsights.js
  ```

```javascript
var request = require('request');
var FormData = require('form-data');

var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
var subscriptionKey = '<yoursubscriptionkeygoeshere>';

// To get an insights, call the /images/search endpoint. Get the token from
// the imageInsightsToken field in the Image object.
var insightsToken = "ccid_tmaGQ2eU*mid_D12339146CFEDF3D409CC7A66D2C98D0D71904D4*simid_608022145667564759*thid_OIP.tmaGQ2eUI1yq3yll!_jn9kwHaFZ";

var knowledgeRequest = {
    "imageInfo" : {
        "imageInsightsToken" : insightsToken
    }
};

var form = new FormData();
form.append('knowledgeRequest', JSON.stringify(knowledgeRequest));

form.getLength(function(err, length){
  if (err) {
    return requestCallback(err);
  }

  var r = request.post(baseUri, requestCallback);
  r._form = form; 
  r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
});

function requestCallback(err, res, body) {
    console.log(JSON.stringify(JSON.parse(body), null, '  '))
}
```


<a name="using-python" />

## <a name="using-python"></a>Utilizar o Python


### <a name="prerequisites"></a>Pré-requisitos

Irá precisar do [Python 3](https://www.python.org/) para executar este código.

Neste guia de início rápido, pode utilizar uma chave de subscrição de uma [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ou uma chave de subscrição paga.

## <a name="running-the-walkthrough"></a>Executar as instruções

Para executar esta aplicação, siga os passos abaixo:

1. Crie um novo projeto de Python no seu editor ou IDE favorito.
2. Crie um ficheiro com o nome visualsearch.py e adicione o código apresentado neste guia de início rápido.
3. Substitua o valor `SUBSCRIPTION_KEY` pela sua chave de subscrição.
4. Execute o programa.


```python
"""Bing Visual Search example"""

# Download and install Python at https://www.python.org/
# Run the following in a command console window
# pip3 install requests

import requests, json

BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'

SUBSCRIPTION_KEY = '<yoursubscriptionkeygoeshere>'

HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}

# To get an insights, call the /images/search endpoint. Get the token from
# the imageInsightsToken field in the Image object.
insightsToken = 'ccid_tmaGQ2eU*mid_D12339146CFEDF3D409CC7A66D2C98D0D71904D4*simid_608022145667564759*thid_OIP.tmaGQ2eUI1yq3yll!_jn9kwHaFZ'

formData = '{"imageInfo":{"imageInsightsToken":"' + insightsToken + '"}}'


file = {'knowledgeRequest' : (None, formData)}

def main():
    
    try:
        response = requests.post(BASE_URI, headers=HEADERS, files=file)
        response.raise_for_status()
        print_json(response.json())

    except Exception as ex:
        raise ex


def print_json(obj):
    """Print the object as json"""
    print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))



# Main execution
if __name__ == '__main__':
    main()
```

## <a name="next-steps"></a>Passos Seguintes

[Tutorial de aplicação de página única de pesquisa Visual do Bing](tutorial-bing-visual-search-single-page-app.md)  
[Descrição geral da Pesquisa Visual do Bing](overview.md)  
[Experimente](https://aka.ms/bingvisualsearchtryforfree)  
[Obter uma chave de acesso de avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing Visual Search API reference](https://aka.ms/bingvisualsearchreferencedoc) (Referência da API de Pesquisa Visual do Bing)
