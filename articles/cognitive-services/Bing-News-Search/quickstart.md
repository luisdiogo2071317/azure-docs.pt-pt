---
title: Início rápido da API de pesquisa de notícias | Microsoft Docs
description: Mostra como começar a utilizar a API de pesquisa do Bing notícias de última hora.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 9CF6EAF3-42D8-4321-983C-4AC3896E8E03
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 2aa1b3de07bd61eebfc1d410cda76c32fc7c958e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354866"
---
# <a name="your-first-news-search-query"></a>A primeira consulta de pesquisa de notícias de última hora

Antes de poder tornar a sua primeira chamada, terá de obter uma chave de subscrição de serviços cognitivos. Para obter uma chave, consulte [tente serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=bing-news-search-api).

Para obter os resultados da pesquisa só de notícias, teria de enviar um pedido GET para o seguinte ponto final:

```http
https://api.cognitive.microsoft.com/bing/v7.0/news/search
```

O pedido tem de utilizar o protocolo HTTPS.

Recomendamos que todos os pedidos provenientes de um servidor. Distribuir a chave como parte de uma aplicação de cliente fornece mais oportunidade para terceiros maliciosa para aceder ao mesmo. Além disso, efetuar chamadas a partir de um servidor fornece um único ponto de atualização para versões futuras da API.

O pedido tem de especificar o [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#query) parâmetro de consulta, que contém o termo de pesquisa do utilizador. Embora seja opcional, pedido deve também especificar o [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#mkt) parâmetro de consulta, que identifica o mercado onde pretende que os resultados de. Para obter uma lista opcional de parâmetros de consulta como `freshness` e `textDecorations`, consulte [parâmetros de consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#query-parameters). Todos os valores de parâmetro de consulta tem de ser codificado de URL.

O pedido tem de especificar o [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#subscriptionkey) cabeçalho. Embora seja opcional, são encorajados a também especificar os cabeçalhos seguintes:

- [Agente de utilizador](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#useragent)
- [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#clientid)
- [X-pesquisa-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#clientip)
- [Localização de pesquisa X](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#location)

Os cabeçalhos IP e a localização do cliente são importantes para devolver o conteúdo com suporte para localização.

Para obter uma lista de todos os cabeçalhos de pedido e resposta, consulte [cabeçalhos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#headers).

## <a name="the-request"></a>O pedido

O seguinte mostra um pedido de notícias, que inclui todos os parâmetros de consulta sugerida e nos cabeçalhos. Se for a primeira vez ao chamar qualquer uma das APIs do Bing, não incluem o cabeçalho de ID de cliente. Inclua apenas o ID de cliente se anteriormente tiver chamar uma API do Bing e Bing devolveu um ID de cliente para o utilizador e a combinação de dispositivo.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)
X-Search-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

## <a name="the-response"></a>A resposta

O seguinte mostra a resposta ao pedido de anterior. O exemplo mostra também os cabeçalhos de resposta do Bing específicos.

```http
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "News",
    "readLink" : "https:\/\/api.cognitive.microsoft.com\/bing\/v7\/news\/search?q=sailing+dinghies",
    "totalEstimatedMatches" : 88400,
    "value" : [{
        "name" : "Sailing Vies for Four Trophies",
        "url" : "http:\/\/www.bing.com\/cr?IG=CCE2F06CA...",
        "image" : {
            "thumbnail" : {
                "contentUrl" : "https:\/\/www.bing.com\/th?id=ON.9C23AA5...",
                "width" : 650,
                "height" : 341
            }
        },
        "description" : "Sailing College Rankings, presented by Zim...",
        "provider" : [{
            "_type" : "Organization",
            "name" : "contoso.com"
        }],
        "datePublished" : "2017-04-14T15:28:00"
    },
    {
        "name" : "Reunion at Fabrikam Lakes Sailing Club, celebrates 50 years...",
        "url" : "http:\/\/www.bing.com\/cr?IG=CCE2F06CA750455891F...",
        "image" : {
            "thumbnail" : {
                "contentUrl" : "https:\/\/www.bing.com\/th?id=ON.38210...",
                "width" : 650,
                "height" : 366
            }
        },
        "description" : "The reunion on April 29, at Fabrikam Lakes Sailing...",
        "provider" : [{
            "_type" : "Organization",
            "name" : "Contoso"
        }],
        "datePublished" : "2017-04-14T13:08:00"
    },
    {
        "name" : "Sailing Club to host Dinghy Sailing World...",
        "url" : "http:\/\/www.bing.com\/cr?IG=CCE2F06CA750455891FE99...",
        "image" : {
            "thumbnail" : {
                "contentUrl" : "https:\/\/www.bing.com\/th?id=ON.364AB41...",
                "width" : 448,
                "height" : 300
            }
        },
        "description" : "The sailing club that trained Olympian Ben...",
        "provider" : [{
            "_type" : "Organization",
            "name" : "Contoso"
        }],
        "datePublished" : "2017-04-04T11:02:00",
        "category" : "Sports"
    },
    {
        "name" : "A 24-Carat Dinghy",
        "url" : "http:\/\/www.bing.com\/cr?IG=CCE2F06CA750455891F...",
        "image" : {
            "thumbnail" : {
                "contentUrl" : "https:\/\/www.bing.com\/th?id=ON.6CC2...",
                "width" : 700,
                "height" : 466
            }
        },
        "description" : "“Hard dinghies are for purists, the kind of people who...",
        "provider" : [{
            "_type" : "Organization",
            "name" : "contoso.com"
        }],
        "datePublished" : "2017-04-03T12:14:00",
        "category" : "Politics"
    }]
}
```

## <a name="next-steps"></a>Passos Seguintes

Experimente a API. Aceda a [consola de teste de API de pesquisa de notícias](https://dev.cognitive.microsoft.com/docs/services/56b43f72cf5ff8098cef380a/operations/56f02400dbe2d91900c68553).

Para obter detalhes sobre a consumir os objetos de resposta, consulte [que é que o Bing notícias pesquisa?](./search-the-web.md). Também irá encontrar mais informações sobre as seguintes ações comuns:

- [Obter superior notícias de última hora hoje](./search-the-web.md#getting-todays-top-news)
- [Obter notícias de última hora por categoria](./search-the-web.md#getting-news-by-category)
- [Obter análises históricas notícias de última hora](./search-the-web.md#getting-trending-news)
