---
title: Obter imagens .gif - serviços cognitivos Microsoft | Microsoft Docs
description: Como utilizar a API de pesquisa do Bing imagem para obter mais informações sobre imagens de GIF.
services: cognitive-services
author: MikeDodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 04/24/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 10e922b0cd15868bfe8f09b3846c76a368052e69
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355063"
---
# <a name="search-for-gif-images"></a>Procure .gif imagens
A API de pesquisa do Bing imagem permite-lhe também procurar em toda a Web todo para as imagens de .gif mais relevantes.  Os programadores podem integrar a interagir gifs em vários cenários de conversação. 

O URL seguinte é uma consulta para imagens de GIF animado.
````
https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=interesting&imageType=AnimatedGif&mkt=en-us
````
O [q](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-images-api-v7-reference#query) parâmetro especifica os termos de pesquisa.  A consulta anterior também especifica `animatedGif` utilizando o [imageType](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype) filtrar parâmetro.

Para ver os exemplos de resultados, utilize o seguinte URL para procurar bing.com.
````
https://www.bing.com/images/search?q=interesting&qft=%20filterui%3Aphoto-animatedgif 

````
## <a name="query-parameters"></a>Parâmetros de consulta

Para obter mais informações sobre as opções e parâmetros de consulta, consulte o [referência da API de pesquisa de imagem](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-images-api-v7-reference#query-parameters). Segue-se de um exemplo sob o cabeçalho [pesquisa de exemplo para gif animado utilizando Java](#gifExample).

## <a name="tips-and-suggestions"></a>Sugestões e sugestões

- Pode especificar [maxFileSize](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-images-api-v7-reference#maxfilesize) e [minFileSize](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-images-api-v7-reference#minfilesize) parâmetros. Recomendamos que defina o maxFileSize = 2000000 como a maioria das gifs no nosso índice são em 2MB.  Isto também ajuda a controlar o tamanho de dados, se a largura de banda é uma preocupação, tal como em cenários móveis de rede móvel.
- Para ajudar a melhorar o desempenho percetível, carregar a miniatura primeiro antes ao carregar o url de origem.  
- Para a primeira execução ou a experiência de página de destino onde ainda não tiver uma consulta do utilizador, tente utilizar o nosso pesquisas gif tendências para ajudar a partir de [tendências imagens API](trending-images.md).
- Existem três definições para o [safeSearch](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-images-api-v7-reference#safesearch) parâmetro.  O `strict` opção bloqueia o conteúdo para adultos. 
- Consulte [mkt](supported-countries-markets.md) para obter uma lista completa de idiomas e as localizações suportadas.
- *AnimatedGifHttps* devolve apenas animada gif imagens que pertençam um endereço https. Para segurança, muitas aplicações necessitam de ligação para ligações da web externo através de https. Por exemplo, a Apple App Store requer ligação a serviços web através de HTTPS, o que encripta os dados de utilizador seguros em trânsito.

<a name="gifExample" />
## <a name="example-search-for-animated-gif-using-java"></a>Pesquisa de exemplo para gif animado linguagem Java

GIF animado imagens procura o seguinte URL: `q=interesting`
````
https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=interesting&imageType=AnimatedGif&mkt=en-us

````
Como é mostrado no exemplo seguinte, a consulta de URL requer [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bing-images-api-v7-reference#headers) cabeçalho.

O seguinte exemplo de Java baseia-se e envia o pedido.

````
package gifSearch;
import java.net.*;
import java.util.*;
import java.io.*;
import javax.net.ssl.HttpsURLConnection;

import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.1
 *
 * Once you have compiled or downloaded gson-2.8.1.jar, assuming you have placed it in the
 * same folder as this file (BingImageSearch.java), you can compile and run this program at
 * the command line as follows.
 *
 * javac GIFsearch.java -classpath .;gson-2.8.1.jar -encoding UTF-8
 * java -cp .;gson-2.8.1.jar GIFsearch
 */


public class GIFsearch {

    // Replace the subscriptionKey string value with your valid subscription key.
    static String subscriptionKey = "YOUR-ACCESS-KEY";

    static String host = "https://api.cognitive.microsoft.com";
    static String path = "/bing/v7.0/images/search";

    static String searchTerm = "interesting";

    public static SearchResults SearchImages (String searchQuery) throws Exception {
        // construct URL of search request (endpoint + query string)
        URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchQuery, "UTF-8") + "&imageType=AnimatedGif&mkt=en-us");
        HttpsURLConnection connection = (HttpsURLConnection)url.openConnection();
        connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);

        // receive JSON body
        InputStream stream = connection.getInputStream();
        String response = new Scanner(stream).useDelimiter("\\A").next();

        // construct result object for return
        SearchResults results = new SearchResults(new HashMap<String, String>(), response);

        // extract Bing-related HTTP headers
        Map<String, List<String>> headers = connection.getHeaderFields();
        for (String header : headers.keySet()) {
            if (header == null) continue;      // may have null key
            if (header.startsWith("BingAPIs-") || header.startsWith("X-MSEdge-")) {
                results.relevantHeaders.put(header, headers.get(header).get(0));
            }
        }

        stream.close();
        return results;
    }

    // pretty-printer for JSON; uses GSON parser to parse and re-serialize
    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonObject json = parser.parse(json_text).getAsJsonObject();
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    public static void main (String[] args) {
        if (subscriptionKey.length() != 32) {
            System.out.println("Invalid Bing Search API subscription key!");
            System.out.println("Please paste yours into the source code.");
            System.exit(1);
        }

        try {
            System.out.println("Searching the Web for: " + searchTerm);

            SearchResults result = SearchImages(searchTerm);

            System.out.println("\nRelevant HTTP Headers:\n");
            for (String header : result.relevantHeaders.keySet())
                System.out.println(header + ": " + result.relevantHeaders.get(header));

            System.out.println("\nJSON Response:\n");
            System.out.println(prettify(result.jsonResponse));
        }
        catch (Exception e) {
            e.printStackTrace(System.out);
            System.exit(1);
        }
    }

}

//Container class for search results encapsulates relevant headers and JSON data
class SearchResults{
 HashMap<String, String> relevantHeaders;
 String jsonResponse;
 SearchResults(HashMap<String, String> headers, String json) {
     relevantHeaders = headers;
     jsonResponse = json;
 }
}

````

## <a name="results"></a>Resultados
O código obtém os seguintes resultados como objetos JSON:

```json
    {
      "webSearchUrl": "https://www.bing.com/images/search?view\u003ddetai...",
      "name": "Very Interesting GIF - Thats Very Interesting - ...",
      "thumbnailUrl": "https://tse1.mm.bing.net/th?id\u003dOIP.yJX6Vz345JPK...",
      "datePublished": "2017-03-12T01:35:00.0000000Z",
      "contentUrl": "https://media.contoso.co/images/c895fa573df8e493ca8d0dec7d93b/raw",
      "hostPageUrl": "https://www.contoso.co/view/thats-very-interesting-christi...",
      "contentSize": "1295633 B",
      "encodingFormat": "animatedgif",
      "hostPageDisplayUrl": "https://www.contoso.co/view/thats-very-christian...",
      "width": 440,
      "height": 186,
      "thumbnail": {
        "width": 474,
        "height": 200
      },
      "imageInsightsToken": "ccid_yJX6Vz34*mid_9FF0FFA42AADA1357F042443D2103B40EA...",
      "insightsMetadata": {
        "recipeSourcesCount": 0,
        "bestRepresentativeQuery": {
          "text": "That\u0027s Very Interesting",
          "displayText": "That\u0027s Very Interesting",
          "webSearchUrl": "https://www.bing.com/images/search?q\u003dThat..."
        },
        "pagesIncludingCount": 19,
        "availableSizesCount": 2
      },
      "imageId": "9FF0FFA42AADA1357F042443D21030EAAA225F",
      "accentColor": "62452D"
    },

```

## <a name="next-steps"></a>Passos Seguintes
- [Início rápido c#](quickstarts/csharp.md)
- [Tutorial da aplicação de página única pesquisa de imagem](tutorial-bing-image-search-single-page-app.md)