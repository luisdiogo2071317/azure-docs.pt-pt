---
title: SDK de pesquisa de notícias do Bing
titleSuffix: Azure Cognitive Services
description: SDK de pesquisa de notícias do Bing para aplicativos que pesquisar na web.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: conceptual
ms.date: 1/24/2018
ms.author: v-gedod
ms.openlocfilehash: b1d9eaa35416adfa11647f2116171256f82fe095
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801238"
---
# <a name="bing-search-sdk"></a>SDK de pesquisa do Bing
Os exemplos de API de pesquisa de notícias do Bing incluem cenários que:
1. Consultar os termos de pesquisa com notícias `market` e `count` parâmetros, verifique se o número de resultados e imprimir `totalEstimatedMatches`, nome, URL, descrição, hora de publicação e nome do fornecedor do primeiro resultado de notícias.
2. Consultar termos de pesquisa com os mais recentes notícias `freshness` e `sortBy` parâmetros, verifique se o número de resultados e imprimir `totalEstimatedMatches`, URL, a descrição, a hora de publicação e nome do fornecedor do primeiro resultado de notícias.
3. Notícias de categoria para de consulta `movie` e `TV entertainment` com pesquisa segura, verifique se o número de resultados e imprimir a categoria, nome, URL, descrição, hora de publicação e nome do fornecedor do primeiro resultado de notícias.
4. Consulta tópicos mais populares de notícias do Bing, verifique se o número de resultados e imprimir nome, o texto da consulta, `webSearchUrl`, `newsSearchUrl` e o URL do primeiro resultado de notícias da imagem.

Os SDKs de pesquisa do Bing tornam a funcionalidade de pesquisa web prontamente acessível em linguagens de programação seguintes:
* Introdução ao [amostras do .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
    * [Pacote NuGet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.NewsSearch/1.2.0)
    * Consulte também [bibliotecas .NET](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Search/BingNewsSearch) para definições e as dependências.
* Introdução ao [amostras de node. js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) 
    * Consulte também [bibliotecas de node. js](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/newsSearch) para definições e as dependências.
* Introdução ao [exemplos de Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) 
    * Consulte também [bibliotecas Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingNewsSearch) para definições e as dependências.
* Introdução ao [amostras de Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) 
    * Consulte também [bibliotecas Python](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-search-newssearch) para definições e as dependências.

Amostras SDK para cada idioma incluem um arquivo Leiame com detalhes sobre os pré-requisitos e instalação/em execução os exemplos.