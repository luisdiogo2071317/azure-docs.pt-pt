---
title: 'Início Rápido: SDK de Pesquisa de Notícias do Bing, Java'
titleSuffix: Azure Cognitive Services
description: Saiba como configurar a aplicação de consola do SDK de Pesquisa de Notícias do Bing.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 02/16/2018
ms.author: v-gedod
ms.openlocfilehash: 7919f13e8c5aaa592a416190d3cd11edbf31af2c
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48802482"
---
# <a name="quickstart-bing-news-search-sdk-with-java"></a>Início Rápido: SDK de Pesquisa de Notícias do Bing com Java

O SDK de Pesquisa de Notícias do Bing disponibiliza a funcionalidade de API REST para consultas de notícias e análise de resultados. 

O [código fonte para os exemplos de SDK Java da Pesquisa de Notícias do Bing](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingNewsSearch) está disponível no Git Hub.

## <a name="application-dependencies"></a>Dependências da aplicação
Obtenha uma [chave de acesso aos Serviços Cognitivos](https://azure.microsoft.com/try/cognitive-services/) em **Pesquisa**. Instale as dependências do SDK de Pesquisa de Notícias do Bing com o Maven, Gradle ou outro sistema de gestão de dependências. O ficheiro POM do Maven requer a declaração:
```
  <dependencies>
    <dependency>
        <groupId>com.microsoft.azure.cognitiveservices</groupId>
        <artifactId>azure-cognitiveservices-newssearch</artifactId>
        <version>0.0.1-beta-SNAPSHOT</version>
    </dependency>
  </dependencies>
```
## <a name="news-search-client"></a>Cliente de Pesquisa de Notícias
Adicione as importações à implementação da classe.
```
import com.microsoft.azure.cognitiveservices.newssearch.*;
import com.microsoft.azure.cognitiveservices.newssearch.implementation.NewsInner;
import com.microsoft.azure.cognitiveservices.newssearch.implementation.NewsSearchAPIImpl;
import com.microsoft.azure.cognitiveservices.newssearch.implementation.TrendingTopicsInner;
import com.microsoft.rest.credentials.ServiceClientCredentials;
import okhttp3.Interceptor;
import okhttp3.OkHttpClient;
import okhttp3.Request;
import okhttp3.Response;
import java.io.IOException;
```
Implemente o cliente **NewsSearchAPIImpl**, que requer uma instância da classe **ServiceClientCredentials**.
```
public static NewsSearchAPIImpl getClient(final String subscriptionKey) {
    return new NewsSearchAPIImpl("https://api.cognitive.microsoft.com/bing/v7.0/",
            new ServiceClientCredentials() {
                @Override
                public void applyCredentialsFilter(OkHttpClient.Builder builder) {
                    builder.addNetworkInterceptor(
                            new Interceptor() {
                                @Override
                                public Response intercept(Chain chain) throws IOException {
                                    Request request = null;
                                    Request original = chain.request();
                                    // Request customization: add request headers.
                                    Request.Builder requestBuilder = original.newBuilder()
                                            .addHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
                                    request = requestBuilder.build();
                                    return chain.proceed(request);
                                }
                            });
                }
            });
}


```
Pesquise notícias com a consulta única de “Quantum Computing” (computação quântica). Filtre a pesquisa com os parâmetros *mercado* e *contagem*. Verifique o número de resultados. Imprima as informações do primeiro resultado de notícias: nome, URL, data de publicação, descrição, nome do fornecedor e número total de correspondências estimadas.
```
public static void newsSearch(String subscriptionKey)
{
    NewsSearchAPIImpl client = getClient(subscriptionKey);

    try
    {
        NewsInner newsResults = client.searchs().list("Quantum  Computing", null, null, null,
                null, null, 100, null, "en-us",
                null, null, null, null, null,
                null, null);

        System.out.println("\r\nSearch news for query \"Quantum  Computing\" with market and count");

        if (newsResults == null)
        {
            System.out.println("Didn't see any news result data..");
        }
        else
        {
            if (newsResults.value().size() > 0)
            {
                NewsArticle firstNewsResult = newsResults.value().get(0);

                System.out.println(String.format("TotalEstimatedMatches value: %d", newsResults.totalEstimatedMatches()));
                System.out.println(String.format("News result count: %d", newsResults.value().size()));
                System.out.println(String.format("First news name: %s", firstNewsResult.name()));
                System.out.println(String.format("First news url: %s", firstNewsResult.url()));
                System.out.println(String.format("First news description: %s", firstNewsResult.description()));
                System.out.println(String.format("First news published time: %s", firstNewsResult.datePublished()));
                System.out.println(String.format("First news provider: %s", firstNewsResult.provider().get(0).name()));
            }
            else
            {
                System.out.println("Couldn't find news results!");
            }
        }
    }

    catch (Exception ex)
    {
        System.out.println("Encountered exception. " + ex.getLocalizedMessage());
    }
}

```
Pesquise notícias recentes sobre “Artificial Intelligence” (inteligência artificial). Filtre a pesquisa com os parâmetros *atualização* e *sortBy*. Verifique o número de resultados. Imprima as informações do primeiro resultado de notícias: nome, URL, data de publicação, descrição, nome do fornecedor e número total de correspondências estimadas.
```
/**
 * Search recent news for (Artificial Intelligence) with the freshness and sortBy parameters.
 * Verify the number of results. Print the totalEstimatedMatches, name, url, description,
 * published time, and provider name for the first news result.
 * @param subscriptionKey cognitive services subscription key
 */
public static void newsSearchWithFilters(String subscriptionKey)
{
    NewsSearchAPIImpl client = getClient(subscriptionKey);

    try
    {
        NewsInner newsResults = client.searchs().list("Artificial Intelligence", null, null, null, null, null,
                    null, Freshness.WEEK, "en-us", null, null, null,
                    null, "Date", null, null);
        System.out.println("\r\nSearch most recent news for query \"Artificial Intelligence\" with freshness and sortBy");

        if (newsResults == null)
        {
            System.out.println("Didn't see any news result data..");
        }
        else
        {
            if (newsResults.value().size() > 0)
            {
                NewsArticle firstNewsResult = newsResults.value().get(0);

                System.out.println(String.format("TotalEstimatedMatches value: %d", newsResults.totalEstimatedMatches()));
                System.out.println(String.format("News result count: %d", newsResults.value().size()));
                System.out.println(String.format("First news name: %s", firstNewsResult.name()));
                System.out.println(String.format("First news url: %s", firstNewsResult.url()));
                System.out.println(String.format("First news description: %s", firstNewsResult.description()));
                System.out.println(String.format("First news published time: %s", firstNewsResult.datePublished()));
                System.out.println(String.format("First news provider: %s", firstNewsResult.provider().get(0).name()));
            }
            else
            {
                System.out.println("Couldn't find news results!");
            }
        }
    }

    catch (Exception ex)
    {
        System.out.println("Encountered exception. " + ex.getLocalizedMessage());
    }
}

```
Na **categoria** de notícias, pesquise os tópicos *movie and TV entertainment* e utilize a funcionalidade *pesquisa segura*. Verifique o número de resultados. Imprima a categoria, o nome, o URL, a descrição a data de publicação e o nome do fornecedor do primeiro resultado de notícias.
```
/**
 * Search the news category for (movie and TV entertainment) with safe search. Verify the number of results. 
 * Print the category, name, url, description, published time, and provider name for the first news result.
 * @param subscriptionKey cognitive services subscription key
 */
public static void newsCategory(String subscriptionKey)
{
    NewsSearchAPIImpl client = getClient(subscriptionKey);

    try
    {
        NewsInner newsResults = client.categorys().list(null, null, null, null, null, "Entertainment_MovieAndTV",
                null, null, "en-us", null, null, SafeSearch.STRICT,
                null, null, null);
        System.out.println("\r\nSearch category news for movie and TV entertainment with safe search");

        if (newsResults == null)
        {
            System.out.println("Didn't see any news result data..");
        }
        else
        {
            if (newsResults.value().size() > 0)
            {
                NewsArticle firstNewsResult = newsResults.value().get(0);

                System.out.println(String.format("News result count: %d", newsResults.value().size()));
                //System.out.println(String.format("First news category: %d", firstNewsResult.category()));
                System.out.println(String.format("First news name: %s", firstNewsResult.name()));
                System.out.println(String.format("First news url: %s", firstNewsResult.url()));
                System.out.println(String.format("First news description: %s", firstNewsResult.description()));
                System.out.println(String.format("First news published time: %s", firstNewsResult.datePublished()));
                System.out.println(String.format("First news provider: %s", firstNewsResult.provider().get(0).name()));
            }
            else
            {
                System.out.println("Couldn't find news results!");
            }
        }
    }

    catch (Exception ex)
    {
        System.out.println("Encountered exception. " + ex.getLocalizedMessage()
        );
    }
}

```
Pesquise tópicos de notícias populares. Verifique o número de resultados. Imprima o nome, o texto da consulta, o URL de pesquisa Web e o URL de pesquisa de notícias do primeiro resultado de notícias.
```
public static void trendingTopics(String subscriptionKey)
{
    NewsSearchAPIImpl client = getClient(subscriptionKey);

    try
    {
        TrendingTopicsInner trendingTopics = client.trendings().list(null, null, null, null, null, null,
                "en-us", null, null, null, null, null, null, null);
        System.out.println("\r\nSearch news trending topics in Bing");

        if (trendingTopics == null)
        {
            System.out.println("Didn't see any news trending topics..");
        }
        else
        {
            if (trendingTopics.value().size() > 0)
            {
                NewsTopic firstTopic = trendingTopics.value().get(0);

                System.out.println(String.format("Trending topics count: %s", trendingTopics.value().size()));
                System.out.println(String.format("First topic name: %s", firstTopic.name()));
                System.out.println(String.format("First topic query: %s", firstTopic.query().text()));
                System.out.println(String.format("First topic image url: %s", firstTopic.image().url()));
                System.out.println(String.format("First topic webSearchUrl: %s", firstTopic.webSearchUrl()));
                System.out.println(String.format("First topic newsSearchUrl: %s", firstTopic.newsSearchUrl()));
            }
            else
            {
                System.out.println("Couldn't find news trending topics!");
            }
        }
    }

    catch (Exception ex)
    {
        System.out.println("Encountered exception. " + ex.getLocalizedMessage());
    }
}
```
Adicione os métodos descritos neste artigo a uma classe com uma função principal para executar o código.
```
package javaNewsSDK;
import com.microsoft.azure.cognitiveservices.newssearch.*;

public class NewsSearchSDK {
    

    public static void main(String[] args) {
        String subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
        NewsSearchSDK.newsSearch("YOUR-SUBSCRIPTION-KEY");
        NewsSearchSDK.newsSearchWithFilters("YOUR-SUBSCRIPTION-KEY");
        NewsSearchSDK.newsCategory("YOUR-SUBSCRIPTION-KEY");
        NewsSearchSDK.trendingTopics("YOUR-SUBSCRIPTION-KEY");
    }

    // Include the methods described in this article.
}
```
## <a name="next-steps"></a>Passos seguintes

[Exemplos de SDK Java nos Serviços Cognitivos](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples)


