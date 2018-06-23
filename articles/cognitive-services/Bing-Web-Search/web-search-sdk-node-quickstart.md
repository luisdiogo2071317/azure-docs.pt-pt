---
title: Pesquisa na Web início rápido do nó de SDK | Microsoft Docs
description: O programa de configuração de pesquisa na Web aplicação de consola do SDK.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 44f7f97f6c442df3fbb1e5e08189b8db7d4b9db0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355298"
---
# <a name="web-search-sdk-node-quickstart"></a>Início rápido do nó do SDK de pesquisa de Web

O SDK de pesquisa do Bing Web contém as funcionalidades da API REST para consultas de web e os resultados da análise.

O [origem código para exemplos do SDK de pesquisa do nó Bing Web](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/webSearch.js) está disponível no Hub de Git.

## <a name="application-dependencies"></a>Dependências da aplicação

Para configurar uma aplicação de consola utilizando o SDK de pesquisa do Bing Web, execute `npm install azure-cognitiveservices-websearch` no seu ambiente de desenvolvimento.

## <a name="web-search-client"></a>Cliente de pesquisa da Web
Obter um [chave de acesso de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) em *pesquisa*. Criar uma instância do `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Em seguida, instanciar o cliente:
```
const WebSearchAPIClient = require('azure-cognitiveservices-websearch');
let webSearchApiClient = new WebSearchAPIClient(credentials);
```
Para obter os resultados da pesquisa:
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
O código imprime `result.value` itens para a consola sem qualquer texto de análise.  Os resultados, se qualquer por categoria, irão incluir:
- escreva: 'ImageObject'
- escreva: 'NewsArticle'
- escreva: "Página Web"
- escreva: 'VideoObjectElementType'

<!-- Remove until this can be replaced with a sanitized version.
![Video results](media/web-search-sdk-node-results.png)
-->

## <a name="next-steps"></a>Passos Seguintes

[Serviços cognitivos amostras de Node.js SDK](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
