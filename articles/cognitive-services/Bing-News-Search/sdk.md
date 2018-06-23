---
title: Pesquisa do Bing SDK | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: SDK de pesquisa do Bing para aplicações web de pesquisa.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.assetid: ''
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 1/24/2018
ms.author: v-gedod
ms.openlocfilehash: 4a40ea665e153536d2322706b455598902ce41eb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355268"
---
# <a name="bing-search-sdk"></a>Pesquisa do Bing SDK
Os exemplos de API de pesquisa do Bing notícias incluem cenários que:
1. Notícias de última hora de consulta para os termos de pesquisa com `market` e `count` parâmetros, verifique o número de resultados e imprimir `totalEstimatedMatches`, nome, URL, descrição, publicada hora e nome do fornecedor do resultado de notícias de última hora da primeira.
2. Consultar os termos de pesquisa com notícias `freshness` e `sortBy` parâmetros, verifique o número de resultados e imprimir `totalEstimatedMatches`, URL, a descrição, a hora publicada e nome do fornecedor do resultado da primeira notícias de última hora.
3. Consultar notícias de categoria para `movie` e `TV entertainment` Search seguro, verifique o número de resultados e imprimir categoria, nome, URL, descrição, publicada hora e nome do fornecedor do resultado de notícias de última hora da primeira.
4. Consulta tópicos de tendências de notícias no Bing, verifique o número de resultados e imprimir o nome, o texto da consulta, `webSearchUrl`, `newsSearchUrl` e URL do resultado da primeira notícias de imagem.

Os SDKs de pesquisa do Bing torne a funcionalidade de pesquisa de web prontamente acessíveis nos seguintes idiomas de programação:
* Introdução ao [amostras do .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
    * [Pacote NuGet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.NewsSearch/1.2.0)
    * Consulte também [bibliotecas .NET](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Search/BingNewsSearch) para dependências e definições.
* Introdução ao [amostras de Node.js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) 
    * Consulte também [Node.js bibliotecas](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/newsSearch) para dependências e definições.
* Introdução ao [exemplos de Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) 
    * Consulte também [bibliotecas de Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingNewsSearch) para dependências e definições.
* Introdução ao [amostras de Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) 
    * Consulte também [bibliotecas de Python](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-search-newssearch) para dependências e definições.

Exemplos SDK para cada idioma incluem um ficheiro Leia-me com detalhes sobre os pré-requisitos e instalar/em execução os exemplos.