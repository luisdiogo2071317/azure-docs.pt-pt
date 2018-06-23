---
title: Como a página através dos artigos de notícias disponíveis | Microsoft Docs
description: Mostra como a página através de todos os artigos de notícias de última hora que o Bing pode devolver.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: EA388F72-FA43-493B-967C-9560B3243C62
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 2c90d468536f0864d7deac073667e29e9a54692f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351464"
---
# <a name="paging-news"></a>Notícias de paginação

Quando chamar a API de pesquisa de notícias, o Bing devolve uma lista de resultados. A lista é um subconjunto do número total de resultados que podem ser relevantes para a consulta. Para obter o número estimado de total de resultados disponíveis, aceder ao objeto de resposta [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news-totalmatches) campo.  
  
O seguinte exemplo mostra o `totalEstimatedMatches` campo que inclui uma resposta de notícias de última hora.  
  
```  
{  
    "_type" : "News",  
    "readLink" : "https:\/\/api.cognitive.microsoft.com\/bing\/v7\/news\/search?q=sailing+dinghies",  
    "totalEstimatedMatches" : 88400,  
    "value" : [...]  
}  
```  
  
A página através de artigos disponíveis, utilize o [contagem](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#count) e [desvio](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#offset) parâmetros de consulta.  
  
O `count` parâmetro especifica o número de resultados para devolver na resposta. O número máximo de resultados pode solicitar na resposta é 100. A predefinição é 10. O número real entregar pode ser inferior a pedido.

O `offset` parâmetro especifica o número de resultados para ignorar. O `offset` é baseado em zero e deve ser inferior a (`totalEstimatedMatches` - `count`).  

Se pretender apresentar 20 artigos por página, iria definir `count` 20 e `offset` como 0 para obter a primeira página de resultados. Para todas as páginas subsequentes, seria incrementar `offset` por 20 (por exemplo, 20, 40).  
  
O seguinte mostra um exemplo que os pedidos de 20 novos artigos, começando no desvio 40.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
Se a predefinição `count` valor funciona para a implementação, especifique apenas o `offset` parâmetro de consulta, conforme mostrado no exemplo seguinte:  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
> [!NOTE]
> A paginação aplica-se apenas a pesquisa de notícias (/ notícias de última hora de pesquisa) e não tópicos tendências (notícias/trendingtopics) ou categorias de notícias (/ notícias de última hora).