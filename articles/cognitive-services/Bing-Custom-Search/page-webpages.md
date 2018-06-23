---
title: 'Pesquisa do Bing personalizada: Página através de páginas Web disponíveis | Microsoft Docs'
description: Mostra como percorrer todas as páginas Web que pode devolver Bing.
services: cognitive-services
author: brapel
manager: ehansen
ms.assetid: 26CA595B-0866-43E8-93A2-F2B5E09D1F3B
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: f2f545a5a9195fc65515ea716f277723600cbb78
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351410"
---
# <a name="paging-webpages"></a>Páginas Web de paginação 

Quando chamar a API de pesquisa personalizada, o Bing devolve uma lista de resultados. A lista é um subconjunto do número total de resultados que podem ser relevantes para a consulta. Para obter o número estimado de total de resultados disponíveis, aceder ao objeto de resposta [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#totalestimatedmatches) campo.  
  
O seguinte exemplo mostra o `totalEstimatedMatches` campo que inclui uma resposta de Web.  
  
```  
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43CA...",
        "totalEstimatedMatches" : 262000,
        "value" : [...]
    }
}  
```  
  
A página através de páginas Web disponíveis, utilize o [contagem](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#count) e [desvio](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#offset) parâmetros de consulta.  
  
O `count` parâmetro especifica o número de resultados para devolver na resposta. O número máximo de resultados pode solicitar na resposta é 50. A predefinição é 10. O número real entregar pode ser inferior a pedido.

O `offset` parâmetro especifica o número de resultados para ignorar. O `offset` é baseado em zero e deve ser inferior a (`totalEstimatedMatches` - `count`).  
  
Se pretender apresentar as páginas 15 Web por página, iria definir `count` para 15 e `offset` como 0 para obter a primeira página de resultados. Para todas as páginas subsequentes, seria incrementar `offset` por 15 (por exemplo, 15, 30).  
  
O seguinte mostra um exemplo que os pedidos de páginas 15 Web começando no desvio 45.  
  
```  
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=sailing+dinghies&count=15&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: <subscription ID>
Host: api.cognitive.microsoft.com  
```  

Se a predefinição `count` valor funciona para a implementação, apenas tem de especificar o `offset` parâmetro de consulta.  
  
```  
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=sailing+dinghies&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: <subscription ID>  
Host: api.cognitive.microsoft.com  
```  

