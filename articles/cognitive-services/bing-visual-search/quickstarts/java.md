---
title: 'Início Rápido: criar uma consulta de pesquisa visual, Java - Pesquisa Visual do Bing'
titleSuffix: Azure Cognitive Services
description: Como carregar uma imagem para a API de Pesquisa Visual do Bing e obter informações sobre a imagem.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 0e2f4d83904240cb32d3e162c4c873f0c8e469fd
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48885260"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-java"></a>Início Rápido: a sua primeira consulta de Pesquisa Visual do Bing em Java

A API de Pesquisa Visual do Bing devolve informações sobre uma imagem que indicar. Pode fornecer a imagem através do URL da mesma, de um token de informações ou do carregamento da mesma. Para obter informações sobre estas opções, veja [What is Bing Visual Search API?](../overview.md) (O que é a API de Pesquisa Visual do Bing?). Este artigo demonstra como carregar uma imagem. Carregar uma imagem pode ser útil em cenários móveis onde tira uma fotografia de um ponto de referência bem conhecido e obtém informações sobre o mesmo. Por exemplo, as informações podem incluir curiosidades sobre esse ponto de referência. 

Se carregar uma imagem local, o seguinte mostra os dados de formato que tem de incluir no corpo de POST. Os dados de formulário têm de incluir o cabeçalho Content-Disposition. O respetivo parâmetro `name` tem de ser definido como “image” e o parâmetro `filename` pode ser definido como qualquer cadeia. Os conteúdos do formulário são o binário da imagem. O tamanho máximo de imagem que pode carregar é 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Este artigo inclui uma aplicação de consola simples que envia um pedido da API de Pesquisa Visual do Bing e apresenta os resultados da pesquisa JSON. Embora esta aplicação seja escrita em Java, a API é um serviço Web RESTful compatível com qualquer linguagem de programação que consiga fazer pedidos HTTP e analisar JSON. 


## <a name="prerequisites"></a>Pré-requisitos

Irá precisar do [JDK 7 ou 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) para compilar e executar este código. Pode utilizar um IDE Java se tiver um favorito, mas um editor de texto será suficiente.

Neste guia de início rápido, pode utilizar uma chave de subscrição de uma [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) ou uma chave de subscrição paga.

## <a name="running-the-application"></a>Executar a aplicação

A seguir, é-lhe mostrado como carregar a imagem com MultipartEntityBuilder em Java.

Para executar esta aplicação, siga os passos abaixo:

1. Transfira ou instale a [biblioteca gson](https://github.com/google/gson). Também pode obtê-la através do Maven.
2. Crie um novo projeto Java no seu IDE ou editor favorito.
3. Adicione o código fornecido a um ficheiro com o nome `VisualSearch.java`.
4. Substitua o valor `subscriptionKey` pela sua chave de subscrição.
4. Substitua o valor `imagePath` pelo caminho da imagem a carregar.
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

## <a name="next-steps"></a>Passos seguintes

[Get insights about an image using an insights token](../use-insights-token.md) (Utilizar um token de informações para obter informações de uma imagem)  
[Tutorial de carregamento de imagens da Pesquisa Visual do Bing](../tutorial-visual-search-image-upload.md)
[Tutorial de aplicação de página única da Pesquisa Visual do Bing](../tutorial-bing-visual-search-single-page-app.md)  
[Descrição geral da Pesquisa Visual do Bing](../overview.md)  
[Experimente](https://aka.ms/bingvisualsearchtryforfree)  
[Obter uma chave de acesso de avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing Visual Search API reference](https://aka.ms/bingvisualsearchreferencedoc) (Referência da API de Pesquisa Visual do Bing)

