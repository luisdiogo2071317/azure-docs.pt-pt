---
title: 'Início Rápido: SDK da Pesquisa de Notícias do Bing, Node'
titleSuffix: Azure Cognitive Services
description: Configurar a aplicação de consola do SDK da Pesquisa de Notícias do Bing
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 075ce771b6fa92663f172734aabf57b089b294a9
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/26/2018
ms.locfileid: "52316944"
---
# <a name="quickstart-bing-news-search-sdk-with-node"></a>Início Rápido: SDK da Pesquisa de Notícias do Bing com Node

O SDK da Pesquisa de Notícias do Bing contém a funcionalidade da API REST para consultas de notícias e análise de resultados. 

O [código-fonte para os exemplos do SDK da Pesquisa de Notícias do Bing em Node](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/newsSearch.js) está disponível no Git Hub.

## <a name="application-dependencies"></a>Dependências da aplicação
Obtenha uma [chave de acesso aos Serviços Cognitivos](https://azure.microsoft.com/try/cognitive-services/) em **Pesquisa**.  Consulte também [dos serviços cognitivos preços - API de pesquisa Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/). 

Para configurar uma aplicação de consola com o SDK de pesquisa de notícias do Bing:
* Executar `npm install ms-rest-azure` no seu ambiente de desenvolvimento.
* Executar `npm install azure-cognitiveservices-newssearch` no seu ambiente de desenvolvimento.

## <a name="news-search-client"></a>Cliente da Pesquisa de Notícias
Obtenha uma [chave de acesso aos Serviços Cognitivos](https://azure.microsoft.com/try/cognitive-services/) em *Pesquisa*. Crie uma instância do `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Em seguida, inicie o cliente:
```
const NewsSearchAPIClient = require('azure-cognitiveservices-newssearch');
let client = new NewsSearchAPIClient(credentials);
```
Utilize o cliente para pesquisar com um texto de consulta, neste caso, "Jogos Olímpicos de Inverno":
```
client.newsOperations.search('Winter Olympics').then((result) => {
    console.log(result.value);
}).catch((err) => {
    throw err;
});

```
O código imprime `result.value` itens para a consola sem analisar qualquer texto. Os resultados, se ocorrerem por categoria, irão incluir:
- _type: 'NewsArticle'
- _type: 'WebPage'
- _type: 'VideoObject'
- _type: 'ImageObject'

<!-- Remove until we can replace with santized version
![News results](media/node-sdk-quickstart-results.png)
-->

## <a name="next-steps"></a>Passos Seguintes

[Exemplos de SDK Node.js dos Serviços Cognitivos](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
