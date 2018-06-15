---
title: Pesquisa de entidade início rápido do nó de SDK | Microsoft Docs
description: O programa de configuração para a pesquisa de entidade aplicação de consola do SDK.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: 2904ecfed33334458f9b6a9ca2500cd0bfef13bc
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355736"
---
# <a name="entity-search-sdk-node-quickstart"></a>Início rápido do nó do SDK de pesquisa de entidade

O SDK de pesquisa do Bing entidade contém as funcionalidades da API REST para consultas de entidade e os resultados da análise. 

O [origem código para amostras de c# Bing entidade SDK Search](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/entitySearch.js) está disponível no Hub de Git.
## <a name="application-dependencies"></a>Dependências da aplicação

Para configurar uma aplicação de consola utilizando o SDK de pesquisa do Bing entidade, execute `npm install azure-cognitiveservices-entitysearch` no seu ambiente de desenvolvimento.

## <a name="entity-search-client"></a>Cliente de pesquisa de entidade
Obter um [chave de acesso de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) em *pesquisa*. Criar uma instância do `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Em seguida, instanciar o cliente e para obter os resultados de pesquisa:
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
O código imprime `result.value` itens para a consola sem qualquer texto de análise.  Os resultados, se qualquer por categoria, irão incluir:
- escreva: 'Coisa'
- escreva: 'ImageObject'

<!-- Removing until we can replace with a sanitized version.
![Entity results](media/entity-search-sdk-node-quickstart-results.png)
-->

## <a name="next-steps"></a>Passos Seguintes

[Serviços cognitivos amostras de Node.js SDK](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)