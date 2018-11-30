---
title: 'Início Rápido: SDK de Pesquisa de Entidades do Bing, Node'
titleSuffix: Azure Cognitive Services
description: Configure a aplicação de consola do SDK de Pesquisa de Entidades com Node.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: quickstart
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: f94e3b5a6070da5ef9510216abd3f52a958030c5
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/26/2018
ms.locfileid: "52311386"
---
# <a name="quickstart-bing-entity-search-sdk-with-node"></a>Início Rápido: SDK de Pesquisa de Entidades do Bing com Node

O SDK de Pesquisa de Entidades do Bing contém a funcionalidade da API REST para consultas de entidades e análise de resultados. 

O [código fonte dos exemplos de SDK de Pesquisa de Entidades do Bing em C#](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/entitySearch.js) está disponível no GitHub.
## <a name="application-dependencies"></a>Dependências da aplicação
Obtenha uma [chave de acesso aos Serviços Cognitivos](https://azure.microsoft.com/try/cognitive-services/) em **Pesquisa**.  Consulte também [dos serviços cognitivos preços - API de pesquisa Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

Para configurar uma aplicação de consola com o SDK de pesquisa de entidades do Bing:
* Executar `npm install ms-rest-azure` no seu ambiente de desenvolvimento.
* Executar `npm install azure-cognitiveservices-entitysearch` no seu ambiente de desenvolvimento.

## <a name="entity-search-client"></a>Cliente da Pesquisa de Entidades
Obtenha uma [chave de acesso aos Serviços Cognitivos](https://azure.microsoft.com/try/cognitive-services/) em *Pesquisa*. Crie uma instância do `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Em seguida, instancie o cliente e procure resultados:
```
const EntitySearchAPIClient = require('azure-cognitiveservices-entitysearch');

let entitySearchApiClient = new EntitySearchAPIClient(credentials);

entitySearchApiClient.entitiesOperations.search('seahawks').then((result) => {
    console.log(result.queryContext);
    console.log(result.entities.value);
    console.log(result.entities.value[0].description);
}).catch((err) => {
    throw err;
});

```
O código imprime `result.value` itens na consola sem analisar o texto.  Os resultados, se existirem por categoria, incluirão:
- _type: 'Thing'
- _type: 'ImageObject'

<!-- Removing until we can replace with a sanitized version.
![Entity results](media/entity-search-sdk-node-quickstart-results.png)
-->

## <a name="next-steps"></a>Passos Seguintes

[Exemplos de SDK Node.js dos Serviços Cognitivos](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
