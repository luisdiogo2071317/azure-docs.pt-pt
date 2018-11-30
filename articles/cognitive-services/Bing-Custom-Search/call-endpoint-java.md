---
title: 'Início Rápido: chamar o ponto final com Java - Pesquisa Personalizada do Bing'
titlesuffix: Azure Cognitive Services
description: Este início rápido mostra como pedir os resultados de pesquisa da sua instância de pesquisa personalizada com Java para chamar o ponto final de Pesquisa Personalizada do Bing.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: aahi
ms.openlocfilehash: 5b4494f7840dd9b32cad88ecda800e4dac9c4d8a
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/26/2018
ms.locfileid: "52315924"
---
# <a name="quickstart-call-bing-custom-search-endpoint-java"></a>Início Rápido: ponto final de Pesquisa Personalizada do Bing (Java)

Este início rápido mostra como pedir os resultados de pesquisa da sua instância de pesquisa personalizada através de Java para chamar o ponto final de Pesquisa Personalizada do Bing. 

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de início rápido, necessita de:

- Uma instância de pesquisa personalizada e pronta a utilizar. Veja [Criar a sua primeira instância de Pesquisa Personalizada do Bing](quick-start.md).
- [Java](https://www.java.com) instalado.
- Uma chave de subscrição. Pode obter uma chave de subscrição ao ativar a sua [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) ou pode utilizar uma chave de subscrição paga no dashboard do Azure (veja [Conta da API dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).   Consulte também [dos serviços cognitivos preços - API de pesquisa Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="run-the-code"></a>Executar o código

Para executar este exemplo, siga estes passos:

1. Com um IDE Java à sua escolha, crie um pacote.  
  
2. Crie um ficheiro com o nome CustomSrchJava.java no pacote e copie o código seguinte para o mesmo. Substitua **YOUR-SUBSCRIPTION-KEY** e **YOUR-CUSTOM-CONFIG-ID** pela sua chave de subscrição e ID de configuração.  
  
    ```java
    import java.io.InputStream;
    import java.net.URL;
    import java.net.URLEncoder;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;
    import java.util.Scanner;
    
    import javax.net.ssl.HttpsURLConnection;
    
    import com.google.gson.Gson;
    import com.google.gson.GsonBuilder;
    import com.google.gson.JsonObject;
    import com.google.gson.JsonParser;
    
    public class CustomSrchJava {       
        static String host = "https://api.cognitive.microsoft.com";
        static String path = "/bingcustomsearch/v7.0/search";
        static String subscriptionKey = "YOUR-SUBSCRIPTION-KEY"; 
        static String customConfigId = "YOUR-CUSTOM-CONFIG-ID";  
    
        static String searchTerm = "Microsoft";  // Replace with search term specific to your search scenario.
    
        public static SearchResults SearchWeb (String searchQuery) throws Exception {
            // construct URL of search request (endpoint + query string)
            URL url = new URL(host + path + "?q=" +  URLEncoder.encode(searchTerm, "UTF-8") + "&CustomConfig=" + customConfigId);
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
                System.out.println("Invalid Custom Search subscription key!");
                System.out.println("Please paste yours into the source code.");
                System.exit(1);
            }
    
            try {
                System.out.println("Searching your slice of the Web for: " + searchTerm);
    
                SearchResults result = SearchWeb(searchTerm);
    
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
    
    // Container class for search results encapsulates relevant headers and JSON data
    static class SearchResults{
        HashMap<String, String> relevantHeaders;
        String jsonResponse;
        SearchResults(HashMap<String, String> headers, String json) {
            relevantHeaders = headers;
            jsonResponse = json;
        }
    
    }
    ```  
  
4. Execute o programa.
    
## <a name="next-steps"></a>Passos Seguintes
- [Configurar a sua experiência de IU alojada](./hosted-ui.md)
- [Utilizar marcadores decorativos para realçar o texto](./hit-highlighting.md)
- [Paginação de páginas Web](./page-webpages.md)
