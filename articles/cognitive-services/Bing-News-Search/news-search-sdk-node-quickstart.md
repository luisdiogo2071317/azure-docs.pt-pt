---
title: Início rápido do nó do SDK de pesquisa de notícias | Microsoft Docs
description: Configurar a aplicação de consola do SDK de pesquisa de notícias de última hora
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 4ae99aa100b697a0dd75863c6f0c3c556dfa3d21
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355285"
---
# <a name="news-search-sdk-node-quickstart"></a>Início rápido do nó do SDK de pesquisa de notícias de última hora

O SDK de pesquisa do Bing notícias contém as funcionalidades da API REST para consultas de notícias e os resultados da análise. 

O [origem código para exemplos de nó Bing notícias pesquisa SDK](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/newsSearch.js) está disponível no Hub de Git.

## <a name="application-dependencies"></a>Dependências da aplicação

Para configurar uma aplicação de consola utilizando o SDK de pesquisa do Bing notícias de última hora, execute `npm install azure-cognitiveservices-newssearch` no seu ambiente de desenvolvimento.

## <a name="news-search-client"></a>Cliente de pesquisa de notícias de última hora
Obter um [chave de acesso de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) em *pesquisa*. Criar uma instância do `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Em seguida, instanciar o cliente:
```
const NewsSearchAPIClient = require('azure-cognitiveservices-newssearch');
let client = new NewsSearchAPIClient(credentials);
```
Utilize o cliente para procurar com um texto de consulta, neste caso 'Inverno Olympics':
```
client.newsOperations.search('Winter Olympics').then((result) => {
    console.log(result.value);
}).catch((err) => {
    throw err;
});

```
O código imprime `result.value` itens para a consola sem qualquer texto de análise. Os resultados, se qualquer por categoria, irão incluir:
- escreva: 'NewsArticle'
- escreva: "Página Web"
- escreva: 'VideoObject'
- escreva: 'ImageObject'

<!-- Remove until we can replace with santized version
![News results](media/node-sdk-quickstart-results.png)
-->

## <a name="next-steps"></a>Passos Seguintes

[Serviços cognitivos amostras de Node.js SDK](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
