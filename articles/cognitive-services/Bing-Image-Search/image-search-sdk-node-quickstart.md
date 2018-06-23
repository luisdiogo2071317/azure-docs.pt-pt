---
title: Pesquisa de imagem início rápido do nó de SDK | Microsoft Docs
description: O programa de configuração para a pesquisa de imagem aplicação de consola do SDK.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: e4c8303e39accbb7caec15c0ef47d701971ce632
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355255"
---
# <a name="image-search-sdk-node-quickstart"></a>Início rápido do nó do SDK de pesquisa de imagem

O SDK de pesquisa do Bing imagem contém as funcionalidades da API REST para consultas de imagem e os resultados da análise. 

O [origem código para exemplos do SDK de pesquisa de imagem do nó Bing](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/imageSearch.js) está disponível no Hub de Git.

## <a name="application-dependencies"></a>Dependências da aplicação

Para configurar uma aplicação de consola utilizando o SDK de pesquisa do Bing imagens, execute `npm install azure-cognitiveservices-imagesearch` no seu ambiente de desenvolvimento.

## <a name="image-search-client"></a>Cliente de pesquisa de imagem
Obter um [chave de acesso de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) em *pesquisa*. Criar uma instância do `CognitiveServicesCredentials`:
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
Em seguida, instanciar o cliente:
```
const ImageSearchAPIClient = require('azure-cognitiveservices-imagesearch');
let client = new ImageSearchAPIClient(credentials);
```
Utilize o cliente para procurar com um texto de consulta, neste caso 'El Capitan':
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

[Serviços cognitivos amostras de Node.js SDK](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)