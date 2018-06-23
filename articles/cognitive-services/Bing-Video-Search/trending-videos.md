---
title: Procurar na web vídeos tendências | Microsoft Docs
description: Mostra como utilizar a API de pesquisa de vídeo do Bing para pesquisar na web vídeos tendências.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 897A28A3-0980-484E-814F-FFE1D5C885E6
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 8db7fcf77042631260b4b165bd3d44053827f3ce
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351416"
---
# <a name="get-trending-videos"></a>Obter análises históricas vídeos  

Para obter vídeos tendências hoje, envie o pedido GET seguinte:  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending?mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

Os seguintes mercados suportam vídeos tendências.  
 
-   EN-AU (inglês, Austrália)  
-   en-CA (inglês, Canadá)  
-   en-GB (inglês, ótimo Britain)  
-   EN-ID (inglês, Indonesia)  
-   EN-IE (inglês, Irlanda)  
-   EN-IN (inglês, Índia)  
-   EN-NZ (inglês, Nova Zelândia)  
-   EN-PH (inglês, Filipinas)  
-   EN-SG (inglês, Singapura)  
-   EN-US (inglês, Estados Unidos)  
-   EN WW (código de agregação de inglês, Worldwide)  
-   EN-ZA (inglês, África do Sul)  
-   zh-CN (chinês, China)

  
O exemplo seguinte mostra uma resposta que contém vídeos tendências.  

```  
{  
    "_type" : "TrendingVideos",  
    "bannerTiles" : [
        {  
            "query" : {  
                "text" : "Hello - Smith",  
                "displayText" : "Hello - Smith",  
                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3E8F5..."
            },  
            "image" : {  
                "description" : "Image from: contosowallpapers.com",  
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=RsA%2fdPlTmx4zS...",  
                "headLine" : "\"Hello\" is a song by...",  
                "contentUrl" : "http:\/\/www.contosowallpapers.com\/wp-content..."  
            }  
        },  
        . . .  
    ],  
    "categories" : [
        {  
            "title" : "Music videos",  
            "subcategories" : [
                {  
                    "tiles" : [
                        {  
                            "query" : {  
                                "text" : "Song Title - Artist Name",  
                                "displayText" : "Song Title - Artist Name",  
                                "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3E8F5..."
                            },  
                            "image" : {  
                                "description" : "Image from: contoso.com",  
                                "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?id=...",  
                                "contentUrl" : "http:\/\/images6.contoso.com\/image..."  
                            }  
                        },  
                        . . .  
                    ],
                    "title" : "Top "  
                },
                {
                    "tiles" : [...],
                    "title" : "Trending"
                },
                . . .
            ],  
        },
        {
            "title" : "Viral videos",
            "subcategories" : [
                {
                    "tiles" : [...],
                    "title" : "Trending"
                },
                . . .
            ],  
        },
        . . .  
    ]  
}  
  
```  
A resposta contém uma lista de vídeos por categoria e subcategoria. Por exemplo, se a lista de categorias contidas uma categoria de vídeos de música e um dos respetivos subcategorias foi parte superior, pode criar uma categoria de vídeos de música superior na sua experiência de utilizador. Pode, em seguida, utilizar o `thumbnailUrl`, `displayText`, e `webSearchUrl` campos para criar um mosaico clicável em cada categoria (por exemplo, vídeos de música superior). Quando o utilizador clica no mosaico, se estiver a direcionado para browser de vídeo de Bing onde o vídeo é reproduzido.

A resposta também contém vídeos de faixa, que são os vídeos tendências mais populares. Os vídeos de faixa podem ter um ou mais das categorias.  
  
