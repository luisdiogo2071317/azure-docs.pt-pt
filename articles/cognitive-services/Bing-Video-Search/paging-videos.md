---
title: Como paginar através de vídeos disponíveis - pesquisa de vídeos do Bing
titlesuffix: Azure Cognitive Services
description: Mostra como paginar através de todos os vídeos do Bing pode devolver.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 26e706b054653b8f0ad1ea14d0a9c2ca97cd227f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55181797"
---
# <a name="paging-videos"></a>Vídeos de paginação

Quando chamar a API de Pesquisa de Vídeos, o Bing devolve uma lista de resultados. A lista é um subconjunto do número total de resultados que são relevantes para a consulta. Para obter o número total de resultados disponíveis, acessar o objeto de resposta [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-totalestimatedmatches) campo.  
  
A exemplo a seguir mostra o `totalEstimatedMatches` campo que inclui uma resposta de vídeo.  
  
```  
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56...",
    "totalEstimatedMatches" : 1000,
    "value" : [...]
}  
```  
  
Para a página por meio dos vídeos disponíveis, utilize o [contagem](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#count) e [deslocamento](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#offset) parâmetros de consulta.  
  
O `count` parâmetro especifica o número de resultados a devolver na resposta. O número máximo de resultados que pode solicitar na resposta é 105. A predefinição é 35. O número real entregue pode ser menor do que o pedido.

O `offset` parâmetro especifica o número de resultados a ignorar. O `offset` é baseado em zero e deve ser inferior a (`totalEstimatedMatches` - `count`).  
  
Se deseja exibir 20 vídeos por página, definiria `count` para 20 e `offset` como 0 para obter a primeira página de resultados. Para cada página subsequente, teria de incrementar `offset` por 20 (por exemplo, 20, 40).  

O código a seguir mostra um exemplo que solicite 20 vídeos começando no desvio 40.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Se a predefinição `count` valor funciona para a sua implementação, só precisa de especificar o `offset` parâmetro de consulta.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Normalmente, se a página 35 vídeos ao mesmo tempo, definiria a `offset` parâmetro como 0 em sua primeira solicitação de consulta e, em seguida, incrementar `offset` por 35 em cada pedido subsequente. No entanto, alguns dos resultados na resposta subsequente podem estar duplicados da resposta anterior. Por exemplo, os dois primeiros vídeos na resposta podem ser o mesmo que os dois últimos vídeos da resposta anterior.

Para eliminar duplicados resultados, utilize o [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos-nextoffset) campo o `Videos` objeto.

Por exemplo, se quiser página 30 vídeos ao mesmo tempo, definiria `count` de 30 e `offset` para 0 na sua primeira solicitação. No seu pedido seguinte, definiria a `offset` consultar o parâmetro para o `nextOffset` valor.


> [!NOTE]
> Paginação aplica-se apenas a pesquisa de vídeos (/ vídeos/search) e não para informações de vídeo (/ vídeos/detalhes) ou vídeos populares (/ vídeos/tendências).

> [!NOTE]
> O `TotalEstimatedAnswers` campo é uma estimativa do número total de resultados de pesquisa, pode recuperar para a consulta atual.  Se definir `count` e `offset` parâmetros, o `TotalEstimatedAnswers` número pode ser alterada. 
