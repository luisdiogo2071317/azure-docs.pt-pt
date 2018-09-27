---
title: Pesquisar na web para vídeos populares - pesquisa de vídeos do Bing
titlesuffix: Azure Cognitive Services
description: Mostra como utilizar a API de pesquisa de vídeos do Bing para pesquisar na web para vídeos populares.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 8a6ccc9ea8cf9468d7638360c9db8131bc6dc5be
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47222051"
---
# <a name="get-trending-videos"></a>Obter vídeos populares  

Para obter vídeos populares da atualidade, envie o pedido GET seguinte:  
  
```
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending?mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

Os seguintes mercados suportam vídeos populares.  
 
-   EN-AU (inglês, Austrália)  
-   en-CA (inglês, Canadá)  
-   en-GB (em inglês, Grã-Bretanha)  
-   EN-ID (inglês, Indonésia)  
-   EN-IE (inglês, Irlanda)  
-   EN-Índia (inglês, Índia)  
-   EN-NZ (inglês, Nova Zelândia)  
-   EN-PH (inglês, Filipinas)  
-   EN-SG (inglês, Singapura)  
-   EN-US (inglês, Estados Unidos)  
-   EN-WW (inglês, Worldwide agregar código)  
-   ZA en (inglês, África do Sul)  
-   zh-CN (chinês, China)

  
O exemplo seguinte mostra uma resposta que contém vídeos populares.  

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
A resposta contém uma lista de vídeos por categoria e subcategoria. Por exemplo, se a lista de categorias continha uma categoria de vídeos de música e foi uma das respetivas subcategorias parte superior, pode criar uma categoria de vídeos de música da parte superior na sua experiência de utilizador. Em seguida, pode utilizar o `thumbnailUrl`, `displayText`, e `webSearchUrl` campos para criar um mosaico clicável em cada categoria (por exemplo, vídeos de música superior). Quando o usuário clica no mosaico, ele estão levado ao navegador de vídeos do Bing em que o vídeo é reproduzido.

A resposta também contém vídeos de faixa, que são os mais populares vídeos mais populares. Os vídeos de faixa podem ter um ou mais das categorias.  
  
