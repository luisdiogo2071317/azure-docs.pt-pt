---
title: Guia de introdução do notícias pesquisa SDK Python | Microsoft Docs
description: Configuração de aplicação de consola do SDK de pesquisa de notícias de última hora.
titleSuffix: Azure News Search SDK Python quickstart
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 02/14/2018
ms.author: v-gedod
ms.openlocfilehash: 6d212d1477ecf583a038e33e72aab3d60f6aa050
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355273"
---
# <a name="news-search-sdk-python-quickstart"></a>Guia de introdução de pesquisa SDK Python de notícias de última hora

O SDK de pesquisa de notícias contém as funcionalidades da API REST para consultas de web e os resultados da análise. 

O [origem código para exemplos do SDK de pesquisa do Python Bing notícias](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/news_search_samples.py) está disponível no Hub de Git.

## <a name="application-dependencies"></a>Dependências da aplicação
Se ainda não tivê-lo, instale o Python. O SDK é compatível com o Python 2.7, 3.3, 3.4, 3.5 e 3.6.

As recomendações gerais para desenvolvimento do Python é utilizar um [ambiente virtual](https://docs.python.org/3/tutorial/venv.html). Instalar e inicializar o ambiente virtual com o [venv módulo](https://pypi.python.org/pypi/virtualenv). Tem de instalar virtualenv para Python 2.7.
```
python -m venv mytestenv
```
Instale dependências do SDK de pesquisa do Bing notícias de última hora:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-newssearch
```
## <a name="news-search-client"></a>Cliente de pesquisa de notícias de última hora
Obter um [chave de acesso de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) em *pesquisa*. Adicione importações:
```
from azure.cognitiveservices.search.newssearch import NewsSearchAPI
from msrest.authentication import CognitiveServicesCredentials

subscription_key = "YOUR-SUBSCRIPTION-KEY"
```
Criar uma instância do `CognitiveServicesCredentials`. Instanciar o cliente:
```
client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))
```
Para obter os resultados de pesquisa e imprimir o resultado da página Web primeiro:
```
news_result = client.news.search(query="Quantum Computing", market="en-us", count=10)
print("Search news for query \"Quantum Computing\" with market and count")

if news_result.value:
    first_news_result = news_result.value[0]
    print("Total estimated matches value: {}".format(news_result.total_estimated_matches))
    print("News result count: {}".format(len(news_result.value)))
    print("First news name: {}".format(first_news_result.name))
    print("First news url: {}".format(first_news_result.url))
    print("First news description: {}".format(first_news_result.description))
    print("First published time: {}".format(first_news_result.date_published))
    print("First news provider: {}".format(first_news_result.provider[0].name))
else:
    print("Didn't see any news result data..")

```
Procurar com filtros de notícias sobre "Artificial Intelligence" com `freshness` e `sortBy` parâmetros. Verifique o número de resultados e imprimir `totalEstimatedMatches`, `name`, `url`, `description`, `published time`, e `name of provider` do resultado de item de notícias primeiro.
```
def news_search_with_filtering(subscription_key):

    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        news_result = client.news.search(
            query="Artificial Intelligence",
            market="en-us",
            freshness="Week",
            sort_by="Date"
        )
        print("\r\nSearch most recent news for query \"Artificial Intelligence\" with freshness and sortBy")

        if news_result.value:
            first_news_result = news_result.value[0]
            print("News result count: {}".format(len(news_result.value)))
            print("First news name: {}".format(first_news_result.name))
            print("First news url: {}".format(first_news_result.url))
            print("First news description: {}".format(first_news_result.description))
            print("First published time: {}".format(first_news_result.date_published))
            print("First news provider: {}".format(first_news_result.provider[0].name))
        else:
            print("Didn't see any news result data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```
Notícias de categoria de pesquisa de filmes e entretenimento TV Search seguro. Verifique o número de resultados e imprimir `category`, `name`, `url`, `description`, `published time`, e `name of provider` do resultado de item de notícias primeiro.
```
def news_category(subscription_key):

    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        news_result = client.news.category(
            category="Entertainment_MovieAndTV",
            market="en-us",
            safe_search="strict"
        )
        print("\r\nSearch category news for movie and TV entertainment with safe search")

        if news_result.value:
            first_news_result = news_result.value[0]
            print("News result count: {}".format(len(news_result.value)))
            print("First news category: {}".format(first_news_result.category))
            print("First news name: {}".format(first_news_result.name))
            print("First news url: {}".format(first_news_result.url))
            print("First news description: {}".format(first_news_result.description))
            print("First published time: {}".format(first_news_result.date_published))
            print("First news provider: {}".format(first_news_result.provider[0].name))
        else:
            print("Didn't see any news result data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))


```
Tópicos de tendências de notícias de última hora de pesquisa no Bing.  Verifique o número de resultados e imprimir `name`, `text of query`, `webSearchUrl`, `newsSearchUrl`, e `image Url` do resultado da primeira notícias de última hora.
```
def news_trending(subscription_key):

    client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))

    try:
        trending_topics = client.news.trending(market="en-us")
        print("\r\nSearch news trending topics in Bing")

        if trending_topics.value:
            first_topic = trending_topics.value[0]
            print("News result count: {}".format(len(trending_topics.value)))
            print("First topic name: {}".format(first_topic.name))
            print("First topic query: {}".format(first_topic.query.text))
            print("First topic image url: {}".format(first_topic.image.url))
            print("First topic webSearchUrl: {}".format(first_topic.web_search_url))
            print("First topic newsSearchUrl: {}".format(first_topic.news_search_url))
        else:
            print("Didn't see any topics result data..")

    except Exception as err:
        print("Encountered exception. {}".format(err))

```

## <a name="next-steps"></a>Passos Seguintes

[Exemplos de SDK Python de serviços cognitivos](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)


