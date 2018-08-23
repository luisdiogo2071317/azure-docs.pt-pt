---
title: 'Pedido de início rápido: E filtrar imagens usando o SDK em node. js'
description: Neste início rápido, pedir e filtrar as imagens devolvidas pela pesquisa de imagens Bing, com node. js.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: e88c045b220192a617e6b8caf5d8d53f70a25b5e
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/13/2018
ms.locfileid: "41987621"
---
# <a name="quickstart-request-and-filter-images-using-the-sdk-and-nodejs"></a>Pedido de início rápido: E filtrar imagens com o SDK e o node. js

O SDK de pesquisa de imagens do Bing contém a funcionalidade da API REST para consultas de imagem e os resultados da análise. 

O [da origem de código para amostras de SDK de pesquisa de imagem do nó do Bing](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/imageSearch.js) está disponível no Hub de Git.

## <a name="application-dependencies"></a>Dependências de aplicações

Para configurar uma aplicação de consola com o SDK de pesquisa de imagens do Bing, execute `npm install azure-cognitiveservices-imagesearch` no seu ambiente de desenvolvimento.

## <a name="image-search-client"></a>Cliente de pesquisa de imagem
Obter um [chave de acesso de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) sob *pesquisa*. Criar uma instância do `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Em seguida, criar uma instância do cliente:
```
const ImageSearchAPIClient = require('azure-cognitiveservices-imagesearch');
let client = new ImageSearchAPIClient(credentials);
```
Utilize o cliente para pesquisar com um texto de consulta, neste caso, "El Capitan":
```
client.imagesOperations.search('El Capitan', function (err, result, request, response) {
    if (err) throw err;
    console.log(result.value);
});

```
<!-- Need to sanitize result
The code prints `result.value` items to the console without parsing any text. The results will be:
- _type: 'ImageObjectElementType'

![Imageresults](media/node-sdk-quickstart-image-results.png)
-->

## <a name="next-steps"></a>Passos Seguintes

[Amostras de node. js SDK dos serviços cognitivos](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)