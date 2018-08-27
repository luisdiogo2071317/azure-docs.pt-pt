---
title: 'Início rápido: Utilizar a SDK de pesquisa Web Bing para node. js'
description: Configuração de pesquisa na Web, aplicação de consola do SDK.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 08/16/2018
ms.author: v-gedod, erhopf
ms.openlocfilehash: e25c295fc0fc144110325d3c494a513ea35aeb05
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42888595"
---
# <a name="quickstart-use-the-bing-web-search-sdk-for-nodejs"></a>Início rápido: Utilizar a SDK de pesquisa Web Bing para node. js

O SDK de pesquisa Web Bing contém a funcionalidade da API REST para consultas de web e os resultados da análise.

O [da origem de código para amostras de SDK de pesquisa Web do nó do Bing](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/webSearch.js) está disponível no GitHub.

## <a name="application-dependencies"></a>Dependências de aplicações

Para configurar uma aplicação de consola utilizando o SDK de pesquisa Web Bing, execute `npm install azure-cognitiveservices-websearch` no seu ambiente de desenvolvimento.

## <a name="web-search-client"></a>Cliente de pesquisa Web
Obter um [chave de subscrição de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) sob *pesquisa*. Criar uma instância do `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Em seguida, criar uma instância do cliente:
```
const WebSearchAPIClient = require('azure-cognitiveservices-websearch');
let webSearchApiClient = new WebSearchAPIClient(credentials);
```
Procurar resultados:
```
webSearchApiClient.web.search('seahawks').then((result) => {
    let properties = ["images", "webPages", "news", "videos"];
    for (let i = 0; i < properties.length; i++) {
        if (result[properties[i]]) {
            console.log(result[properties[i]].value);
        } else {
            console.log(`No ${properties[i]} data`);
        }
    }
}).catch((err) => {
    throw err;
})

```
O código imprime `result.value` itens para a consola sem qualquer texto de análise.  Os resultados, se houver por categoria, irão incluir:
- tipo: 'ImageObject'
- tipo: 'NewsArticle'
- tipo: "Página da Web"
- tipo: 'VideoObjectElementType'

<!-- Remove until this can be replaced with a sanitized version.
![Video results](media/web-search-sdk-node-results.png)
-->

## <a name="next-steps"></a>Passos Seguintes

[Amostras de node. js SDK dos serviços cognitivos](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
