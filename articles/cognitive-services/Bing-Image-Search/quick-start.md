---
title: Imagens de início rápido da API de pesquisa | Microsoft Docs
description: Mostra como começar a utilizar a API de pesquisa do Bing imagens.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: BE2B2F8C-20B5-4E0B-AEC8-EAD505BA4C85
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 9e211cf5acd17ab80948d0b7161bdd2a9220c4a6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351620"
---
# <a name="your-first-images-search-query"></a>As imagens primeira consulta de pesquisa

Antes de poder tornar a sua primeira chamada, terá de obter uma chave de subscrição de serviços de cognitivos de pesquisa do Bing. Para obter uma chave, consulte [tente serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api).

Para obter os resultados da pesquisa de imagem, teria de enviar um pedido GET para o seguinte ponto final:  
  
```
https://api.cognitive.microsoft.com/bing/v7.0/images/search
```
  
O pedido tem de utilizar o protocolo HTTPS.

Recomendamos que todos os pedidos provenientes de um servidor. Distribuir a chave como parte de uma aplicação de cliente fornece mais oportunidade uma terceiros maliciosa para aceder ao mesmo. Além disso, efetuar chamadas a partir de um servidor fornece um único ponto de atualização para versões futuras da API.

O pedido tem de especificar o [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query) parâmetro de consulta, que contém o termo de pesquisa do utilizador. Embora seja opcional, pedido deve também especificar o [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#mkt) parâmetro de consulta, que identifica o mercado onde pretende que os resultados de. Para obter uma lista opcional de parâmetros de consulta como `freshness` e `size`, consulte [parâmetros de consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query-parameters). Todos os valores de parâmetro de consulta tem de ser codificado de URL.  
  
O pedido tem de especificar o [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#subscriptionkey) cabeçalho. Embora seja opcional, são encorajados a também especificar os cabeçalhos seguintes:  
  
-   [Agente de utilizador](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#clientid)  
-   [X-pesquisa-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#clientip)  
-   [Localização de pesquisa X](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#location)  

Os cabeçalhos IP e a localização do cliente são importantes para devolver o conteúdo com suporte para localização.  

Para obter uma lista de todos os cabeçalhos de pedido e resposta, consulte [cabeçalhos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#headers).

## <a name="the-request"></a>O pedido

O seguinte mostra um pedido de pesquisa que inclui todos os parâmetros de consulta sugerida e nos cabeçalhos. Se for é o primeiro tempo de chamar qualquer uma das APIs do Bing, não incluem o cabeçalho de ID de cliente. Inclua apenas o ID de cliente se anteriormente tiver chamar uma API do Bing e Bing devolveu um ID de cliente para o utilizador e a combinação de dispositivo. 
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

O seguinte mostra a resposta ao pedido de anterior.

```json
{
    "_type" : "Images",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118C8B4E3...",
    "totalEstimatedMatches" : 838,
    "value" : [
        {
            "name" : "File:Rich Passage Minto Sailing Dinghy.jpg - Wikipedia",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118C8B4E3...",
            "thumbnailUrl" : "https:\/\/tse1.mm.bing.net\/th?id=OIP.GNarK7m...",
            "datePublished" : "2011-10-29T11:26:00",
            "contentUrl" : "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3...",
            "hostPageUrl" : "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3687...",
            "contentSize" : "79239 B",
            "encodingFormat" : "jpeg",
            "hostPageDisplayUrl" : "en.wikipedia.org\/wiki\/File:Rich_Passage...",
            "width" : 526,
            "height" : 688,
            "thumbnail" : {
                "width" : 229,
                "height" : 300
            },
            "imageInsightsToken" : "ccid_GNarK7ma*mid_CCF85447ADA6...",
            "insightsSourcesSummary" : {
                "shoppingSourcesCount" : 0,
                "recipeSourcesCount" : 0
            },
            "imageId" : "CCF85447ADA6FFF9E96E7DF0B796F7A86E34593",
            "accentColor" : "376094"
        },
        . . .
    ],
    "queryExpansions" : [
        {
            "text" : "Small Sailing Dinghies",
            "displayText" : "Small",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118C8B4...",
            "searchLink" : "https:\/\/api.cognitive.microsoft.com\/api...",
            "thumbnail" : {
                "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?q=..."
            }
        },
        . . .
    ],
    "nextOffset" : 10,
    "pivotSuggestions" : [
        {
            "pivot" : "sailing",
            "suggestions" : []
        },
        {
            "pivot" : "dinghies",
            "suggestions" : [
                {
                    "text" : "Sailing Tender",
                    "displayText" : "Tender",
                    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118...",
                    "searchLink" : "https:\/\/api.cognitive.microsoft.com\/api...",
                    "thumbnail" : {
                        "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?q=..."
                    }
                },
                . . .
            ]
        }
    ],
    "displayShoppingSourcesBadges" : false,
    "displayRecipeSourcesBadges" : true
}
```

## <a name="next-steps"></a>Passos Seguintes

Experimente a API. Aceda a [consola de teste de API de pesquisa de imagem](https://dev.cognitive.microsoft.com/docs/services/8336afba49a84475ba401758c0dbf749/operations/571fab09dbe2d933e891028f). 

Para obter detalhes sobre a consumir os objetos de resposta, consulte [pesquisar Web](./search-the-web.md).

Para obter mais informações sobre como obter informações sobre uma imagem, tais como páginas web que incluem a imagem ou pessoas que foram reconhecidas na imagem, consulte [imagem Insights](./image-insights.md).  
  
Para obter detalhes sobre as imagens que são tendências em de redes sociais, consulte [tendências imagens](./trending-images.md).  
