---
title: Início rápido de Java para API de pesquisa Visual do Bing | Documentos da Microsoft
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Mostra como carregar uma imagem para a API de pesquisa Visual do Bing e obter informações sobre a imagem.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 41e0855b126ca6e54d0a487a88fe59a0be6f72f6
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39072000"
---
# <a name="your-first-bing-visual-search-query-in-java"></a>Sua primeira consulta de pesquisa Visual do Bing em Java

API de pesquisa Visual do Bing devolve informações sobre uma imagem que fornece. Pode fornecer a imagem utilizando o URL da imagem, um insights token, ou ao carregar uma imagem. Para obter informações sobre estas opções, consulte [o que é a API de pesquisa Visual do Bing?](../overview.md) Este artigo demonstra a carregar uma imagem. Carregar uma imagem pode ser útil em cenários móveis onde tirar uma fotografia de um ponto de referência bem conhecido e obter informações sobre ele. Por exemplo, as informações podem incluir trivia sobre o ponto de referência. 

Se carregar uma imagem do local, o código a seguir mostra os dados do formulário que tem de incluir no corpo da POSTAGEM. Os dados do formulário tem de incluir o cabeçalho Content-Disposition. Seus `name` parâmetro tem de ser definido como "imagem" e o `filename` parâmetro pode ser definido como qualquer cadeia de caracteres. O conteúdo do formulário é o binário da imagem. O tamanho da imagem máximo que pode carregar é de 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Este artigo inclui uma aplicação de consola simples que envia um pedido de API de pesquisa Visual do Bing e exibe os resultados da pesquisa JSON. Embora esse aplicativo é escrito em Java, a API é um serviço RESTful Web compatível com qualquer linguagem de programação que pode fazer solicitações HTTP e analisar JSON. 


## <a name="prerequisites"></a>Pré-requisitos

Precisará [JDK 7 ou 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) para compilar e executar esse código. Se tiver um favorito, mas um editor de texto será suficiente, pode usar um IDE Java.

Neste início rápido, pode utilizar um [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) chave de subscrição ou uma chave de subscrição paga.

## <a name="running-the-application"></a>Executar a aplicação

O código a seguir mostra como carregar a imagem usando MultipartEntityBuilder em Java.

Para executar esta aplicação, siga estes passos:

1. Transferir ou instalar o [gson biblioteca](https://github.com/google/gson). Também pode obtê-lo através do Maven.
2. Crie um novo projeto de Java no seu IDE ou editor favorito.
3. Adicione o código fornecido num arquivo chamado `VisualSearch.java`.
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

[Obtenha informações sobre uma imagem com um token de insights](../use-insights-token.md)  
[Tutorial de carregamento de imagem de pesquisa Visual do Bing](../tutorial-visual-search-image-upload.md)
[tutorial de aplicação de página única de pesquisa Visual do Bing](../tutorial-bing-visual-search-single-page-app.md)  
[Descrição geral de pesquisa Visual do Bing](../overview.md)  
[Experimente-o](https://aka.ms/bingvisualsearchtryforfree)  
[Obter uma chave de acesso de avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Referência da API de pesquisa Visual do Bing](https://aka.ms/bingvisualsearchreferencedoc)

