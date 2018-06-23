---
title: Início rápido de Java para pesquisa de Visual Bing API | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Mostra como carregar uma imagem para a API de pesquisa do Bing Visual e obter informações sobre a imagem a novamente.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 8160302faa373d69b65afe6b68a8efb44442850d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355135"
---
# <a name="your-first-bing-visual-search-query-in-java"></a>A primeira consulta de pesquisa de Visual Bing em Java

API de pesquisa do Bing Visual devolve informações sobre uma imagem que fornecer. Pode fornecer a imagem utilizando o URL da imagem, uma insights token, ou através do carregamento de uma imagem. Para obter informações sobre estas opções, consulte [o que é a API de pesquisa do Bing Visual?](../overview.md) Este artigo demonstra o carregamento de uma imagem. Carregamento de uma imagem pode ser útil em cenários móveis onde tirar uma fotografia de um landmark bem conhecido e voltar a informação sobre a mesma. Por exemplo, as informações podem incluir trivia sobre o landmark. 

Se carregar uma imagem local, o seguinte mostra os dados do formulário que tem de incluir no corpo do POST. Os dados do formulário têm de incluir o cabeçalho de disposição de conteúdo. O `name` parâmetro tem de ser definido como "de imagem" e o `filename` parâmetro pode ser definido como qualquer cadeia. O conteúdo do formulário é o binário da imagem. O tamanho da imagem máximo, pode carregar é de 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Este artigo inclui uma aplicação de consola simples que envia um pedido de API de pesquisa do Bing Visual e apresenta os resultados de pesquisa JSON. Enquanto esta aplicação é escrita em Java, a API é um serviço RESTful Web compatível com qualquer linguagem de programação que pode efetuar pedidos HTTP e analisar JSON. 


## <a name="prerequisites"></a>Pré-requisitos

Terá de [JDK 7 ou 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) para compilar e executar este código. Pode utilizar um IDE de Java se tiver um favorito, mas será suffice um editor de texto.

Para este início rápido, pode utilizar um [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) chave de subscrição ou uma chave de subscrição paga.

## <a name="running-the-application"></a>Executar a aplicação

O seguinte mostra como carregar a imagem utilizando MultipartEntityBuilder em Java.

Para executar esta aplicação, siga estes passos:

1. Transferir ou instalar o [gson biblioteca](https://github.com/google/gson). Também pode obtê-lo através do Maven.
2. Crie um novo projeto de Java no seu IDE ou editor favorito.
3. Adicione o código fornecido num ficheiro denominado `VisualSearch.java`.
4. Substitua o `subscriptionKey` valor com a sua chave de subscrição.
4. Substitua o `imagePath` valor com o caminho da imagem para carregar.
5. Execute o programa.


```java
package uploadimage;

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


public class UploadImage2 {

    static String endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "<yoursubscriptionkeygoeshere";
    static String imagePath = "<pathtoyourimagetouploadgoeshere>";

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        
        CloseableHttpClient httpClient = HttpClientBuilder.create().build();
        
        try {
            HttpEntity entity = MultipartEntityBuilder
                .create()
                .addBinaryBody("image", new File(imagePath))
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

## <a name="next-steps"></a>Passos Seguintes

[Obter conhecimentos aprofundados sobre uma imagem utilizando um token de insights](../use-insights-token.md)  
[Tutorial de aplicação de página única pesquisa Visual do Bing](../tutorial-bing-visual-search-single-page-app.md)  
[Descrição geral de pesquisa de Visual do Bing](../overview.md)  
[Experimente](https://aka.ms/bingvisualsearchtryforfree)  
[Obter uma chave de acesso de avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Referência da API de pesquisa do Bing Visual](https://aka.ms/bingvisualsearchreferencedoc)

