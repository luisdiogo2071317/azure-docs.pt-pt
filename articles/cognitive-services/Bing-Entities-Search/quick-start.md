---
title: Início rápido da API de pesquisa de entidades | Microsoft Docs
description: Mostra como começar a utilizar a API de pesquisa de entidades do Bing.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: B206A254-B7E9-49FF-AFD5-87B1E4D6D30B
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 07/06/2017
ms.author: scottwhi
ms.openlocfilehash: 12031d2447920c7e2d6180f35cf4fb29aa1b6150
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351524"
---
# <a name="making-your-first-entities-request"></a>Efetuar as primeira entidades do pedido

A API de pesquisa de entidade envia uma consulta de pesquisa para o Bing e obtém os resultados que incluem entidades e locais. Resultados de local incluem restaurants, átrios ou outras empresas locais. Para locais, a consulta pode especificar o nome do negócio local ou pode pedir uma lista (por exemplo, restaurants quase-me). Incluem resultados de entidade pessoas, locais ou coisas. Local neste contexto é tourist attractions, Estados, países, etc. 

## <a name="first-steps"></a>Passos primeiro

Antes de poder tornar a sua primeira chamada, terá de obter uma chave de subscrição de serviços cognitivos. Para obter uma chave, consulte [tente serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=bing-entities-search-api). (Se a API de pesquisa de entidades não estiver visível na parte superior, clique em de **pesquisa** separador e desloque para baixo até o vir.)

## <a name="the-endpoint"></a>O ponto final

Para obter a entidade e colocar os resultados da pesquisa, envie um pedido GET para o seguinte ponto final:  
  
```
https://api.cognitive.microsoft.com/bing/v7.0/entities
```

O pedido tem de utilizar o protocolo HTTPS.

Recomendamos que todos os pedidos provenientes de um servidor. Distribuir a chave como parte de uma aplicação de cliente fornece mais oportunidade para terceiros maliciosa para aceder ao mesmo. Além disso, efetuar chamadas a partir de um servidor fornece um único ponto de atualização para versões futuras da API.

## <a name="specifying-query-parameters-and-headers"></a>Especificar os parâmetros de consulta e nos cabeçalhos

O pedido tem de especificar o [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#query) parâmetro de consulta, que contém o termo de pesquisa do utilizador. O pedido tem de especificar também o [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#mkt) parâmetro de consulta, que identifica o mercado onde pretende que os resultados de. Para obter uma lista dos parâmetros de consulta opcionais, consulte [parâmetros de consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#query-parameters). URL codificar todos os parâmetros de consulta.  
  
O pedido tem de especificar o [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#subscriptionkey) cabeçalho. Embora seja opcional, são encorajados a também especificar os cabeçalhos seguintes:  
  
-   [Agente de utilizador](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#clientid)  
-   [X-MSEdge-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#clientip)  
-   [Localização de pesquisa X](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#location)  

Os cabeçalhos IP e a localização do cliente são importantes para devolver o conteúdo com suporte para localização.  

Para obter uma lista de todos os cabeçalhos de pedido e resposta, consulte [cabeçalhos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#headers).

## <a name="the-request"></a>O pedido

O seguinte mostra um pedido de entidades que inclui todos os parâmetros de consulta sugerida e nos cabeçalhos. 

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/entities?q=mount+rainier&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Se for a primeira vez ao chamar qualquer uma das APIs do Bing, não incluem o cabeçalho de ID de cliente. Inclua apenas o ID de cliente se anteriormente tiver chamar uma API do Bing e Bing devolveu um ID de cliente para o utilizador e a combinação de dispositivo.

## <a name="the-response"></a>A resposta

O seguinte mostra a resposta ao pedido de anterior. O exemplo mostra também os cabeçalhos de resposta do Bing específicos. Para obter informações sobre o objeto de resposta, consulte [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#searchresponse).

```
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "mount rainier"
    },
    "entities" : {
        "queryScenario" : "DominantEntity",
        "value" : [{
            "contractualRules" : [{
                "_type" : "ContractualRules\/LicenseAttribution",
                "targetPropertyName" : "description",
                "mustBeCloseToContent" : true,
                "license" : {
                    "name" : "CC-BY-SA",
                    "url" : "http:\/\/creativecommons.org\/licenses\/by-sa\/3.0\/"
                },
                "licenseNotice" : "Text under CC-BY-SA license"
            },
            {
                "_type" : "ContractualRules\/LinkAttribution",
                "targetPropertyName" : "description",
                "mustBeCloseToContent" : true,
                "text" : "en.wikipedia.org",
                "url" : "http:\/\/en.wikipedia.org\/wiki\/Mount_Rainier"
            },
            {
                "_type" : "ContractualRules\/MediaAttribution",
                "targetPropertyName" : "image",
                "mustBeCloseToContent" : true,
                "url" : "http:\/\/en.wikipedia.org\/wiki\/Mount_Rainier"
            }],
            "webSearchUrl" : "https:\/\/www.bing.com\/search?q=Mount%20Rainier...",
            "name" : "Mount Rainier",
            "image" : {
                "name" : "Mount Rainier",
                "thumbnailUrl" : "https:\/\/www.bing.com\/th?id=A21890c0e1f...",
                "provider" : [{
                    "_type" : "Organization",
                    "url" : "http:\/\/en.wikipedia.org\/wiki\/Mount_Rainier"
                }],
                "hostPageUrl" : "http:\/\/upload.wikimedia.org\/wikipedia...",
                "width" : 110,
                "height" : 110
            },
            "description" : "Mount Rainier, Mount Tacoma, or Mount Tahoma is the highest...",
            "entityPresentationInfo" : {
                "entityScenario" : "DominantEntity",
                "entityTypeHints" : ["Attraction"],
                "entityTypeDisplayHint" : "Mountain"
            },
            "bingId" : "9ae3e6ca-81ea-6fa1-ffa0-42e1d78906"
        }]
    }
}
```



## <a name="next-steps"></a>Passos Seguintes

Experimente a API. Aceda a [entidades de pesquisa API testar consola](https://dev.cognitive.microsoft.com/docs/services/7a3fb374be374859a823b79fd938cc65/). 

Para obter detalhes sobre a consumir os objetos de resposta, consulte [pesquisa na Web entidades e locais](./search-the-web.md).

Não se esqueça de ler [utilizar do Bing e requisitos de apresentação](./use-display-requirements.md) , de modo não quebrar qualquer uma das regras sobre como utilizar os resultados da pesquisa.
