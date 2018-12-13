---
title: 'Início rápido: Efetua uma pesquisa de notícias - SDK de pesquisa de notícias do Bing para node. js'
titleSuffix: Azure Cognitive Services
description: Utilize este guia de introdução para pesquisar notícias utilizando o SDK de pesquisa de notícias do Bing para node. js e processar a resposta.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 02/12/2018
ms.author: v-gedod
ms.custom: seodec2018
ms.openlocfilehash: 0fa78474d6400f890626859c32915c8657f92d4e
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2018
ms.locfileid: "53249350"
---
# <a name="quickstart-perform-a-news-search-with-the-bing-news-search-sdk-for-nodejs"></a>Início rápido: Efetua uma pesquisa de notícias com o SDK de pesquisa de notícias do Bing para node. js

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
O código imprime `result.value` itens para a consola sem analisar qualquer texto. Os resultados, se existirem por categoria, incluirão:
- tipo: 'NewsArticle'
- tipo: "Página da Web"
- tipo: 'VideoObject'
- tipo: 'ImageObject'

<!-- Remove until we can replace with santized version
![News results](media/node-sdk-quickstart-results.png)
-->

## <a name="next-steps"></a>Passos Seguintes

[Exemplos de SDK Node.js dos Serviços Cognitivos](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
