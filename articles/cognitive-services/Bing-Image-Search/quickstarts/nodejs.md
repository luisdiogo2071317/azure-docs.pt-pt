---
title: 'Início rápido: Enviar consultas de pesquisa com a API de pesquisa de imagens do Bing com node. js'
titleSuffix: Azure Cognitive Services
description: Utilize este guia de introdução para procurar e localizar imagens na web usando a API de pesquisa Web Bing.
services: cognitive-services
documentationcenter: ''
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 8/20/2018
ms.author: aahi
ms.openlocfilehash: 43f0cfec6aa2d4263b6a627736c2a432b2943145
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45576651"
---
# <a name="quickstart-send-search-queries-using-the-bing-image-search-rest-api-and-nodejs"></a>Início rápido: Consultas de pesquisa de envio utilizar a API de REST de pesquisa de imagens do Bing e o node. js

Utilize este guia de introdução para efetuar a primeira chamada para a API de pesquisa de imagens do Bing e receber uma resposta JSON. Esta aplicação simple de JavaScript envia uma consulta de pesquisa para a API e exibe os resultados brutos.

Embora este aplicativo é escrito em JavaScript e é executado em node. js, a API, é um RESTful do serviço da Web compatível com a maioria das linguagens de programação.

O código-fonte para este exemplo está disponível [no GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingImageSearchv7Quickstart.js) com manipulação de erros adicionais e anotações de código.

## <a name="prerequisites"></a>Pré-requisitos

* A versão mais recente do [node. js](https://nodejs.org/en/download/).

* O [biblioteca de pedido de JavaScript](https://github.com/request/request)
[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Criar e inicializar o aplicativo

1. Criar um novo ficheiro JavaScript no seu IDE ou editor favorito e defina os requisitos de strictness e https.

    ```javascript
    'use strict';
    let https = require('https');
    ```

2. Criar variáveis para o ponto final de API, caminho de pesquisa da API de imagem, a chave de subscrição e o termo de pesquisa.
    ```javascript
    let subscriptionKey = 'enter key here';
    let host = 'api.cognitive.microsoft.com';
    let path = '/bing/v7.0/images/search';
    let term = 'tropical ocean';
    ```

## <a name="construct-the-search-request-and-query"></a>Construa o pedido de pesquisa e a consulta.

1. Utilize as variáveis do último passo para formatar um URL de pesquisa para o pedido de API. Tenha em atenção que o termo de pesquisa tem de ser codificados de URL antes de serem enviados para a API.

    ```javascript
    let request_params = {
        method : 'GET',
        hostname : host,
        path : path + '?q=' + encodeURIComponent(search),
        headers : {
        'Ocp-Apim-Subscription-Key' : subscriptionKey,
        }
    };
    ```

2. Utilize a biblioteca de pedido para enviar sua consulta para a API. `response_handler` serão definidos na secção seguinte.
    ```javascript
    let req = https.request(request_params, response_handler);
    req.end();
    ```

## <a name="handle-and-parse-the-response"></a>Processar e analisar a resposta

1. definir uma função chamada `response_handler` que assume uma chamada HTTP, `response`, como um parâmetro. dentro desta função, execute os seguintes passos:
    
    1. Defina uma variável para conter o corpo da resposta JSON.  
        ```javascript
        let response_handler = function (response) {
            let body = '';
        };
        ```

    2. Store o corpo da resposta quando os **dados** é chamado de sinalizador 
        ```javascript
        response.on('data', function (d) {
            body += d;
        });
        ```

    3. Quando um **final** sinalizador é assinalado, o JSON pode ser processado e URL da imagem pode ser impresso, juntamente com o número total de imagens retornados.
    
        ```javascript
        response.on('end', function () {
            let firstImageResult = imageResults.value[0];
            console.log(`Image result count: ${imageResults.value.length}`);
            console.log(`First image thumbnail url: ${firstImageResult.thumbnailUrl}`);
            console.log(`First image web search url: ${firstImageResult.webSearchUrl}`);
         });
        ```

## <a name="json-response"></a>Resposta JSON

Respostas a partir da API de pesquisa de imagens do Bing são retornadas como JSON. Esta resposta de amostra foi truncada para mostrar um único resultado.

```json
{
"_type":"Images",
"instrumentation":{
    "_type":"ResponseInstrumentation"
},
"readLink":"images\/search?q=tropical ocean",
"webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=tropical ocean&FORM=OIIARP",
"totalEstimatedMatches":842,
"nextOffset":47,
"value":[
    {
        "webSearchUrl":"https:\/\/www.bing.com\/images\/search?view=detailv2&FORM=OIIRPO&q=tropical+ocean&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&simid=608027248313960152",
        "name":"My Life in the Ocean | The greatest WordPress.com site in ...",
        "thumbnailUrl":"https:\/\/tse3.mm.bing.net\/th?id=OIP.fmwSKKmKpmZtJiBDps1kLAHaEo&pid=Api",
        "datePublished":"2017-11-03T08:51:00.0000000Z",
        "contentUrl":"https:\/\/mylifeintheocean.files.wordpress.com\/2012\/11\/tropical-ocean-wallpaper-1920x12003.jpg",
        "hostPageUrl":"https:\/\/mylifeintheocean.wordpress.com\/",
        "contentSize":"897388 B",
        "encodingFormat":"jpeg",
        "hostPageDisplayUrl":"https:\/\/mylifeintheocean.wordpress.com",
        "width":1920,
        "height":1200,
        "thumbnail":{
        "width":474,
        "height":296
        },
        "imageInsightsToken":"ccid_fmwSKKmK*mid_8607ACDACB243BDEA7E1EF78127DA931E680E3A5*simid_608027248313960152*thid_OIP.fmwSKKmKpmZtJiBDps1kLAHaEo",
        "insightsMetadata":{
        "recipeSourcesCount":0,
        "bestRepresentativeQuery":{
            "text":"Tropical Beaches Desktop Wallpaper",
            "displayText":"Tropical Beaches Desktop Wallpaper",
            "webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=Tropical+Beaches+Desktop+Wallpaper&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&FORM=IDBQDM"
        },
        "pagesIncludingCount":115,
        "availableSizesCount":44
        },
        "imageId":"8607ACDACB243BDEA7E1EF78127DA931E680E3A5",
        "accentColor":"0050B2"
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Tutorial de aplicação de página única de pesquisa de imagens Bing](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Consulte também 

* [O que é a pesquisa de imagens do Bing?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Tente uma demonstração interativa online](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Obter uma chave de acesso de serviços cognitivos gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Documentação dos serviços cognitivos do Azure](https://docs.microsoft.com/azure/cognitive-services)
* [Referência da API de pesquisa de imagens do Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)