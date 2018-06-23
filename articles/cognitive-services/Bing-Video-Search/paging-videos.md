---
title: Como a página através de vídeos disponíveis | Microsoft Docs
description: Mostra como a página através de todos os vídeos que pode devolver Bing.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 910A485F-BCF3-42B9-958D-DD48BDEDA965
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 00476825eb3fc1008c3f2172b591d8b7a2f35884
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351433"
---
# <a name="paging-videos"></a>Vídeos de paginação

Quando chamar a API de pesquisa de vídeo, o Bing devolve uma lista de resultados. A lista é um subconjunto do número total de resultados que são relevantes para a consulta. Para obter o número estimado de total de resultados disponíveis, aceder ao objeto de resposta [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-totalestimatedmatches) campo.  
  
O seguinte exemplo mostra o `totalEstimatedMatches` campo que inclui uma resposta de vídeo.  
  
```  
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56...",
    "totalEstimatedMatches" : 1000,
    "value" : [...]
}  
```  
  
A página através de vídeos disponíveis, utilize o [contagem](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#count) e [desvio](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#offset) parâmetros de consulta.  
  
O `count` parâmetro especifica o número de resultados para devolver na resposta. O número máximo de resultados pode solicitar na resposta é 105. A predefinição é 35. O número real entregar pode ser inferior a pedido.

O `offset` parâmetro especifica o número de resultados para ignorar. O `offset` é baseado em zero e deve ser inferior a (`totalEstimatedMatches` - `count`).  
  
Se pretender apresentar 20 vídeos por página, iria definir `count` 20 e `offset` como 0 para obter a primeira página de resultados. Para todas as páginas subsequentes, seria incrementar `offset` por 20 (por exemplo, 20, 40).  

O seguinte mostra um exemplo que os pedidos de 20 vídeos começando no desvio 40.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Se a predefinição `count` valor funciona para a implementação, apenas tem de especificar o `offset` parâmetro de consulta.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Normalmente, se a página 35 vídeos cada vez, iria definir o `offset` parâmetro como 0, o primeiro pedido de consulta e, em seguida, aumentar `offset` por 35 em cada pedido subsequente. No entanto, alguns dos resultados na resposta subsequente podem ser duplicados da resposta anterior. Por exemplo, os dois primeiros vídeos na resposta podem ser o mesmo que os dois últimos vídeos da resposta anterior.

Para eliminar duplicados resultados, utilize o [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-nextoffset) campo a `Videos` objeto.

Por exemplo, se pretender que a página 30 vídeos cada vez, iria definir `count` para 30 e `offset` para 0 na sua primeiro pedido. No pedido seguinte, iria definir o `offset` consultar parâmetro para o `nextOffset` valor.


> [!NOTE]
> A paginação aplica-se apenas a pesquisa de vídeos (vídeos/pesquisa) e não insights vídeos (vídeos/detalhes) ou tendências vídeos (vídeos/tendências).