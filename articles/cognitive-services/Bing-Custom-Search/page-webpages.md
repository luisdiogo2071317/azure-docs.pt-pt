---
title: 'A pesquisa personalizada do Bing: Página por meio de páginas da Web disponíveis | Documentos da Microsoft'
description: Mostra como página por meio de todas as páginas de Web Bing pode devolver.
services: cognitive-services
author: brapel
manager: ehansen
ms.assetid: 26CA595B-0866-43E8-93A2-F2B5E09D1F3B
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 5692776736090c55547c2fe934e2c0aaf840d3c5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982363"
---
# <a name="paging-webpages"></a>Páginas Web de paginação 

Quando chama a API de pesquisa personalizada, o Bing devolve uma lista de resultados. A lista é um subconjunto do número total de resultados que podem ser relevantes para a consulta. Para obter o número total de resultados disponíveis, acessar o objeto de resposta [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#totalestimatedmatches) campo.  
  
A exemplo a seguir mostra o `totalEstimatedMatches` campo que inclui uma resposta de Web.  
  
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
  
Para a página por meio de páginas da Web disponíveis, utilize o [contagem](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#count) e [deslocamento](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#offset) parâmetros de consulta.  
  
O `count` parâmetro especifica o número de resultados a devolver na resposta. O número máximo de resultados que pode solicitar na resposta é 50. A predefinição é 10. O número real entregue pode ser menor do que o pedido.

O `offset` parâmetro especifica o número de resultados a ignorar. O `offset` é baseado em zero e deve ser inferior a (`totalEstimatedMatches` - `count`).  
  
Se deseja exibir 15 páginas da Web por página, definiria `count` 15 e `offset` como 0 para obter a primeira página de resultados. Para cada página subsequente, teria de incrementar `offset` por 15 (por exemplo, 15, 30).  
  
O código a seguir mostra um exemplo que solicite 15 páginas Web começando no desvio 45.  
  
```  
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=sailing+dinghies&count=15&offset=45&mkt=en-us&customConfig=123456 HTTP/1.1  
Ocp-Apim-Subscription-Key: <subscription ID>
Host: api.cognitive.microsoft.com  
```  

Se a predefinição `count` valor funciona para a sua implementação, só precisa de especificar o `offset` parâmetro de consulta.  
  
```  
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=sailing+dinghies&offset=45&mkt=en-us&customConfig=123456 HTTP/1.1  
Ocp-Apim-Subscription-Key: <subscription ID>  
Host: api.cognitive.microsoft.com  
```  

