---
title: Filtragem de respostas da web que o Bing devolve | Microsoft Docs
description: Mostra como utilizar responseFilter para filtrar as respostas que devolve a API de pesquisa do Bing Web.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 8B837DC2-70F1-41C7-9496-11EDFD1A888D
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 01/12/2017
ms.author: scottwhi
ms.openlocfilehash: a5ee6241630ee24a05c2c4b932453bd7946a7508
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352898"
---
# <a name="filtering-the-answers-that-the-search-response-includes"></a>As respostas que inclui a resposta de pesquisa de filtragem  

Quando consultar web, o Bing devolve todos os conteúdos que pensa são relevante para a pesquisa. Por exemplo, se a consulta de pesquisa "sailing + dinghies", a resposta pode conter as respostas seguintes:

```json
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43C...",
        "totalEstimatedMatches" : 262000,
        "value" : [...]
    },
    "images" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=sail...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43CA5CA6464E5D...",
        "isFamilyFriendly" : true,
        "value" : [...]
    },
    "rankingResponse" : {
        "mainline" : {
            "items" : [...]
        }
    }
}    
```

Se estiver interessado em tipos de conteúdo, tais como imagens, notícias e vídeos, específicos, pode pedir apenas essas respostas utilizando o [responseFilter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#responsefilter) parâmetro de consulta. Se Bing localiza o conteúdo relevante para as respostas especificadas, o Bing devolve-lo. O filtro de resposta é uma lista delimitada por vírgulas de respostas. O seguinte procedimento mostra como utilizar `responseFilter` para imagens de pedido, vídeos e notícias de sailing dinghies. Se a codificar a cadeia de consulta, as vírgulas alterar para %2, C.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&responseFilter=images%2Cvideos%2Cnews&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

O seguinte mostra a resposta à consulta anterior. Como pode ver Bing foram encontrados resultados de vídeos e notícias relevantes, pelo que a resposta não inclui-los.

```json
{
    "_type" : "SearchResponse",
    "images" : {
        "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images",
        "readLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=sail...",
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3AD78B183C56456C...",
        "isFamilyFriendly" : true,
        "value" : [...]
    },
    "rankingResponse" : {
        "mainline" : {
            "items" : [{
                "answerType" : "Images",
                "value" : {
                    "id" : "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#Images"
                }
            }]
        }
    }
}
```

Embora o Bing não devolveu resultados de vídeo e Novidades na resposta anterior, não significa que os conteúdos de vídeo e notícias não existe. Basta significa que a página não inclui-los. No entanto, se lhe [página](./paging-webpages.md) através de resultados mais, as páginas subsequentes, provavelmente, inclui-los. Além disso, se chama o [API de pesquisa de vídeo](../bing-video-search/search-the-web.md) e [API de pesquisa de notícias](../bing-news-search/search-the-web.md) os pontos finais diretamente, a resposta provável que deveria conter os resultados. 

É desencorajada da utilização `responseFilter` para obter resultados de uma única API. Se pretender que o conteúdo a partir de uma única API do Bing, chame essa API diretamente. Por exemplo, para receber apenas imagens, enviar um pedido para o ponto final da API de pesquisa de imagem, `https://api.cognitive.microsoft.com/bing/v7.0/images/search` ou um dos outros [imagens](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#endpoints) pontos finais. A chamar a API único é importante não só por motivos de desempenho, mas porque as APIs de conteúdos específicos oferecem mais rico resultados. Por exemplo, pode utilizar filtros que não estão disponíveis para a API Web do pesquisa para filtrar os resultados.  
  
Para obter os resultados da pesquisa a partir de um domínio específico, inclua o `site:` operador de consulta na cadeia de consulta.  

```
https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us
```

> [!NOTE] 
> Consoante a consulta, se utilizar o `site:` operador de consulta, há a possibilidade da resposta pode conter conteúdo para adultos, independentemente do [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#safesearch) definição. Deve utilizar `site:` apenas se estiver em consideração o conteúdo do site e a possibilidade de conteúdo para adultos suporta o seu cenário. 
  
## <a name="limiting-the-number-of-answers-in-the-response"></a>Limitar o número de respostas na resposta

Bing inclui respostas na resposta com base na classificação. Por exemplo, se consultar *sailing + dinghies*, Bing devolve `webpages`, `images`, `videos`, e `relatedSearches`.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailing dinghies"
    },
    "webPages" : {...},
    "images" : {...},
    "relatedSearches" : {...},
    "videos" : {...},
    "rankingResponse" : {...}
}
```

Para limitar o número de respostas que o Bing devolve para as respostas de topo dois (páginas Web e imagens), conjunto de [answerCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#answercount) parâmetro 2 de consulta. 

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&answerCount=2&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

A resposta inclui apenas `webPages` e `images`.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailing dinghies"
    },
    "webPages" : {...},
    "images" : {...},
    "rankingResponse" : {...}
}
```

Se adicionar o `responseFilter` parâmetro para a consulta anterior e o conjunto de páginas Web e notícias de última hora, a resposta contém páginas Web apenas porque não são classificadas o notícias de última hora de consulta.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailing dinghies"
    },
    "webPages" : {...},
    "rankingResponse" : {...}
}
```

## <a name="promoting-answers-that-are-not-ranked"></a>Promover respostas não estão ordenadas

Se a parte superior classificada as respostas que o Bing devolve para uma consulta páginas Web, imagens, vídeos e relatedSearches, a resposta inclui as respostas. Se definir [answerCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#answercount) para dois (2), Bing devolve as respostas de classificados dois principais: páginas Web e imagens. Se pretender que o Bing para incluir imagens e vídeos na resposta, especifique o [promover](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#promote) parâmetro de consulta e defina-o como imagens e vídeos. 

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&answerCount=2&promote=images%2Cvideos&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location:  47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Segue-se a resposta ao pedido acima. Bing devolve o superiores duas respostas, páginas Web e as imagens e promove vídeos para a resposta.

```json
{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "sailiing dinghies"
    },
    "webPages" : {...},
    "images" : {...},
    "videos" : {...},
    "rankingResponse" : {...}
}
```

Se definir `promote` para notícias de última hora, a resposta não inclui a resposta de notícias, porque não é uma resposta classificada&mdash;pode promover só classificado respostas.

As respostas que pretende promover não contagem contra o `answerCount` limite. Por exemplo, se as respostas classificadas são notícias de última hora, imagens e vídeos e definir `answerCount` a 1 e `promote` em notícias de última hora, a resposta contém notícias e imagens. Em alternativa, se as respostas classificadas são vídeos, imagens e notícias de última hora, a resposta contém vídeos e notícias de última hora.

Pode utilizar `promote` apenas se especificou o `answerCount` parâmetro de consulta.
