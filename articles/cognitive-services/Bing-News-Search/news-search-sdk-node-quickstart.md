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
ms.openlocfilehash: 2279a6475ab8c39b3ff599f7244caea59d622651
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803006"
---
# <a name="quickstart-bing-news-search-sdk-with-node"></a>Início Rápido: SDK da Pesquisa de Notícias do Bing com Node

O SDK da Pesquisa de Notícias do Bing contém a funcionalidade da API REST para consultas de notícias e análise de resultados. 

O [código-fonte para os exemplos do SDK da Pesquisa de Notícias do Bing em Node](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/newsSearch.js) está disponível no Git Hub.

## <a name="application-dependencies"></a>Dependências da aplicação

Para configurar uma aplicação de consola com o SDK da Pesquisa de Notícias do Bing, execute `npm install azure-cognitiveservices-newssearch` no seu ambiente de desenvolvimento.

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

## <a name="next-steps"></a>Passos seguintes

[Exemplos de SDK Node.js dos Serviços Cognitivos](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
