---
title: Procurar a web para imagens de tendências | Microsoft Docs
description: Mostra como utilizar a API de pesquisa do Bing imagens para a web para imagens de tendências de pesquisa.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: EAB92D35-5C0B-4A0A-8F49-02DF7FAD44B4
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: b12524cd4c1896501820209b3a45746b8f38b210
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351475"
---
# <a name="get-trending-images"></a>Obter as imagens de tendências  

Para obter as imagens de tendências de hoje, envie o pedido GET seguinte:  

```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending?mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

A API de imagens tendências atualmente suporta apenas os mercados seguintes:  

- EN-US (inglês, Estados Unidos)  
- en-CA (inglês, Canadá)  
- EN-AU (inglês, Austrália)  
- zh-CN (chinês, China)

A resposta contém um [TrendingImages](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#trendingimages) objeto que apresenta uma lista de imagens por categoria. Utilize a categoria `title` para agrupar as imagens na sua experiência de utilizador. As categorias podem ser alteradas diariamente.  
  
```json
{
    "_type" : "TrendingImages",  
    "categories" : [{  
        "title" : "Popular people searches",  
        "tiles" : [{  
            "query" : {  
                "text" : "Smith",  
                "displayText" : "Mr. Smith",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=smith&FORM=..."
            },  
            "image" : {  
                "id" : "C3C60AE779A054D5CD80D3CACF0F3",  
                "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OIP.M2532...",  
                "contentUrl" : "http:\/\/www.contoso.com.au\/assets\/Uploads\/smith-SH01.jpg",  
                "thumbnail" : {  
                    "width" : 288,  
                    "height" : 300  
                }  
            }  
        },  
        . . .  
        ]  
    },  
    . . .  
    {  
        "title" : "Popular Halloween searches",  
        "tiles" : [{  
            "query" : {  
                "text" : "Halloween costumes for adults",  
                "displayText" : "Halloween costumes for adults",  
                "webSearchUrl" : "https:\/\/www.bing.com\/images\/search?q=Halloween+costumes..."
            },  
            "image" : {  
                "id" : "0F3395F2983003F89DCEE711B55D7FA53E4",  
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OIP.Me429c...",  
                "contentUrl" : "http:\/\/images.domain.com\/products\/8179\/1-1\/adult-squirrel...",  
                "thumbnail" : {  
                    "width" : 336,  
                    "height" : 480  
                }  
            }  
        }]  
    }]  
}  
```  
  
Cada mosaico contém uma imagem e as opções para obter imagens relacionadas. Para obter as imagens relacionadas, pode utilizar a consulta `text` para chamar o [API de pesquisa de imagem](./search-the-web.md) e apresentar as imagens relacionadas por si. Em alternativa, pode utilizar o URL na `webSearchUrl` para tirar o utilizador imagens pesquisa página de resultados de Bing, que contém as imagens relacionadas. 

Se chamar a API de pesquisa de imagem para obter as imagens relacionadas, defina o [id](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#id) parâmetro ao ID de consulta a `id` campo. Especificar o ID garante que a resposta contém a imagem (é a primeira imagem na resposta) e as respectivas imagens relacionadas. Além disso, defina o [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#q) parâmetro para o texto de consulta a `query` do objeto `text` campo.

O exemplo seguinte mostra como utilizar o ID de imagem para obter imagens relacionadas de Mr. Santos na resposta da API de imagens de tendências anterior.

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=Smith&id=77FDE4A1C6529A23C7CF0EC073FAA64843E828F2&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  
