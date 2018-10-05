---
title: Como paginar os artigos de notícias disponíveis - pesquisa de notícias do Bing
titlesuffix: Azure Cognitive Services
description: Mostra como paginar através de todos os artigos de notícias nova pesquisa do Bing pode devolver.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: fff1da15df2e690cd0b37bb82654a4d30159325a
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48803397"
---
# <a name="paging-news"></a>Notícias de paginação

Quando chama a API de pesquisa de notícias, o Bing devolve uma lista de resultados. A lista é um subconjunto do número total de resultados que podem ser relevantes para a consulta. Para obter o número total de resultados disponíveis, acessar o objeto de resposta [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news-totalmatches) campo.  
  
A exemplo a seguir mostra o `totalEstimatedMatches` campo que inclui uma resposta de notícias.  
  
```  
{  
    "_type" : "News",  
    "readLink" : "https:\/\/api.cognitive.microsoft.com\/bing\/v7\/news\/search?q=sailing+dinghies",  
    "totalEstimatedMatches" : 88400,  
    "value" : [...]  
}  
```  
  
Para a página por meio dos artigos disponíveis, utilize o [contagem](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#count) e [deslocamento](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#offset) parâmetros de consulta.  
  
O `count` parâmetro especifica o número de resultados a devolver na resposta. O número máximo de resultados que pode solicitar na resposta é 100. A predefinição é 10. O número real entregue pode ser menor do que o pedido.

O `offset` parâmetro especifica o número de resultados a ignorar. O `offset` é baseado em zero e deve ser inferior a (`totalEstimatedMatches` - `count`).  

Se deseja exibir 20 artigos por página, definiria `count` para 20 e `offset` como 0 para obter a primeira página de resultados. Para cada página subsequente, teria de incrementar `offset` por 20 (por exemplo, 20, 40).  
  
O código a seguir mostra um exemplo que solicite 20 artigos de notícias, começando no desvio 40.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
Se a predefinição `count` valor funciona para a sua implementação, especifique apenas o `offset` parâmetro de consulta, conforme mostrado no exemplo a seguir:  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
> [!NOTE]
> Paginação aplica-se apenas a pesquisa de notícias (/ notícias/search) e não para tópicos em destaque (/ notícias/trendingtopics) ou categorias de notícias (/ notícias).