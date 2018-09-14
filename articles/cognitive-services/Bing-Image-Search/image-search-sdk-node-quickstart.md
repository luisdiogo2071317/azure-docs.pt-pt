---
title: 'Início rápido: Pesquisa de imagens com o SDK de pesquisa de imagens do Bing e o node. js'
description: Neste início rápido, pedir e filtrar as imagens devolvidas pela pesquisa de imagens Bing, com node. js.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: cagronlund
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 08/28/2018
ms.author: aahi
ms.openlocfilehash: ebe6629344c076119c0bfdaee76a69df6274ca28
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45572588"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-and-nodejs"></a>Início rápido: Pesquisa de imagens com o SDK de pesquisa de imagens do Bing e o node. js

Utilize este guia de introdução para tornar a sua primeira pesquisa de imagens com o SDK de pesquisa de imagens do Bing, que é um wrapper para a API e contém os mesmos recursos. Esta aplicação simple de JavaScript envia uma consulta de pesquisa de imagem, analisa a resposta JSON e apresenta o URL da imagem do primeiro devolvido.

O código-fonte para este exemplo está disponível no [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/imageSearch.js) com tratamento de erros adicionais e anotações.

## <a name="prerequisites"></a>Pré-requisitos

* O [pesquisa SDK para node. js de imagem de serviços cognitivos](https://www.npmjs.com/package/azure-cognitiveservices-imagesearch)
    * Instalar utilizando `npm install azure-cognitiveservices-imagesearch`
* O [node. js do Azure Rest](https://www.npmjs.com/package/ms-rest-azure) módulo
    * Instalar utilizando `npm install ms-rest-azure`

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Criar e inicializar o aplicativo

1. Criar um novo ficheiro JavaScript no seu IDE ou editor favorito e defina o strictness, https e outros requisitos.

    ```javascript
    'use strict';
    https = require('https');
    const Search = require('azure-cognitiveservices-imagesearch');
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    ```

2. O método principal de seu projeto, crie variáveis para a sua chave de subscrição válido, os resultados de imagem a ser devolvida pelo Bing e um termo de pesquisa. Em seguida, instancio o cliente de pesquisa de imagem a utilizar a chave.

    ```javascript
    //replace this value with your valid subscription key.
    let serviceKey = "ENTER YOUR KEY HERE";

    //the search term for the request 
    let searchTerm = "canadian rockies";

    //instantiate the the image search client 
    let credentials = new CognitiveServicesCredentials(serviceKey);
    let imageSearchApiClient = new Search.ImageSearchAPIClient(credentials);

    ```

## <a name="create-an-asynchronous-helper-function"></a>Criar uma função de auxiliares assíncronos

1. Criar uma função para chamar o cliente de forma assíncrona e retornar a resposta do serviço de pesquisa de imagens do Bing.  
    ```javascript
    //a helper function to perform an async call to the Bing Image Search API
    const sendQuery = async () => {
        return await imageSearchApiClient.imagesOperations.search(searchTerm);
    };
    ```
## <a name="send-a-query-and-handle-the-response"></a>Enviar uma consulta e tratar da resposta 

1. Chamar a função auxiliar e o identificador de seu `promise` para analisar os resultados de imagem devolvidos na resposta.

    Se a resposta contém os resultados da pesquisa, armazenar o primeiro resultado e imprimir os detalhes, como uma miniatura devolveu o URL, o URL original, juntamente com o número total de imagens.  
    ```javascript
    sendQuery().then(imageResults => {
        if (imageResults == null) {
        console.log("No image results were found.");
        }
        else {
            console.log(`Total number of images returned: ${imageResults.value.length}`);
            let firstImageResult = imageResults.value[0];
            //display the details for the first image result. After running the application,
            //you can copy the resulting URLs from the console into your browser to view the image. 
            console.log(`Total number of images found: ${imageResults.value.length}`);
            console.log(`Copy these URLs to view the first image returned:`);
            console.log(`First image thumbnail url: ${firstImageResult.thumbnailUrl}`);
            console.log(`First image content url: ${firstImageResult.contentUrl}`); 
        }
      })
      .catch(err => console.error(err))
    ```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Tutorial de aplicação de página única de pesquisa de imagens Bing](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Consulte também 

* [O que é a pesquisa de imagens do Bing?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Tente uma demonstração interativa online](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Obter uma chave de acesso de serviços cognitivos gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api) 
* [Exemplos de node. js para o SDK dos serviços cognitivos do Azure](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) 
* [Documentação dos serviços cognitivos do Azure](https://docs.microsoft.com/azure/cognitive-services)
* [Referência da API de pesquisa de imagens do Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)