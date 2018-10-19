---
title: 'Início Rápido: SDK de Pesquisa de Notícias do Bing, Python'
titleSuffix: Azure Cognitive Services
description: Configure a aplicação de consola do SDK de Pesquisa de Notícias do Bing.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 02/14/2018
ms.author: v-gedod
ms.openlocfilehash: 8e4343b053835c0fc2219373ad60f96c7b80636a
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803346"
---
# <a name="quickstart-bing-news-search-sdk-with-python"></a>Início Rápido: SDK de Pesquisa de Notícias do Bing com Python

O SDK de Pesquisa de Notícias do Bing contém a funcionalidade da API REST para consultas Web e análise de resultados. 

O [código fonte dos exemplos de SDK de Pesquisa de Notícias do Bing do Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/news_search_samples.py) está disponível no GitHub.

## <a name="application-dependencies"></a>Dependências da aplicação
Se ainda não o tiver, instale o Python. O SDK é compatível com o Python 2.7, 3.3, 3.4, 3.5 e 3.6.

A recomendação geral para o desenvolvimento de Python é utilizar um [ambiente virtual](https://docs.python.org/3/tutorial/venv.html). Instale e inicialize o ambiente virtual com o [módulo venv](https://pypi.python.org/pypi/virtualenv). Tem de instalar o virtualenv para o Python 2.7.
```
python -m venv mytestenv
```
Instale as dependências do SKD da Pesquisa de Notícias do Bing:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-newssearch
```
## <a name="news-search-client"></a>Cliente da Pesquisa de Notícias
Obtenha uma [chave de acesso aos Serviços Cognitivos](https://azure.microsoft.com/try/cognitive-services/) em *Pesquisa*. Adicione as importações:
```
from azure.cognitiveservices.search.newssearch import NewsSearchAPI
from msrest.authentication import CognitiveServicesCredentials

subscription_key = "YOUR-SUBSCRIPTION-KEY"
```
Crie uma instância de `CognitiveServicesCredentials`. Instancie o cliente:
```
client = NewsSearchAPI(CognitiveServicesCredentials(subscription_key))
```
Procure os resultados e imprima o primeiro resultado de página Web:
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
Pesquise com filtros para obter as notícias mais recentes sobre a “Inteligência Artificial” com os parâmetros `freshness` e `sortBy`. Verifique o número de resultados e imprima `totalEstimatedMatches`, `name`, `url`,`description`, `published time` e `name of provider` do primeiro item dos resultados de notícias.
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
Pesquise notícias na categoria de filmes e programas televisivos com a pesquisa segura. Verifique o número de resultados e imprima `category`, `name`, `url`,`description`, `published time` e `name of provider` do primeiro item dos resultados de notícias.
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
Pesquise tópicos de notícias populares no Bing.  Verifique o número de resultados e imprima `name`, `text of query`, `webSearchUrl`,`newsSearchUrl`e `image Url` do primeiro item dos resultados de notícias.
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

## <a name="next-steps"></a>Passos seguintes

[Exemplos de SDK do Python nos Serviços Cognitivos](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)


