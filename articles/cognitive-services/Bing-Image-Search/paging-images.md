---
title: Como a página através de imagens disponíveis | Microsoft Docs
description: Mostra como percorrer todas as imagens que o Bing pode devolver.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 3C8423F8-41E0-4F89-86B6-697E840610A7
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: a74ee817e84be5bb563c5fdaf25afc1dc14732e5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351386"
---
# <a name="paging-results"></a>Resultados de paginação

Quando chamar a API de pesquisa de imagem, o Bing devolve uma lista de resultados. A lista é um subconjunto do número total de resultados que são relevantes para a consulta. Para obter o número estimado de total de resultados disponíveis, aceder ao objeto de resposta [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#totalestimatedmatches) campo.  
  
O seguinte exemplo mostra o `totalEstimatedMatches` campo que inclui uma resposta de imagens.  
  
```json
{
    "_type" : "Images",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118C8...",
    "totalEstimatedMatches" : 838,
    "value" : [...]  
}  
```  
  
A página através de imagens disponíveis, utilize o [contagem](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#count) e [desvio](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offset) parâmetros de consulta.  
  
O `count` parâmetro especifica o número de resultados para devolver na resposta. O número máximo de resultados pode solicitar na resposta é 150. A predefinição é 35. O número real entregar pode ser inferior a pedido.

O `offset` parâmetro especifica o número de resultados para ignorar. O `offset` é baseado em zero e deve ser inferior a (`totalEstimatedMatches` - `count`).  
  
Se pretender apresentar 20 imagens por página, iria definir `count` 20 e `offset` como 0 para obter a primeira página de resultados. O seguinte mostra um exemplo que os pedidos de 20 imagens a partir do desvio 40.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Se a predefinição `count` valor funciona para a implementação, apenas tem de especificar o `offset` parâmetro de consulta.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
Que seria de esperar que se página 35 imagens de cada vez, deverá definir o `offset` parâmetro como 0, o primeiro pedido de consulta e, em seguida, aumentar `offset` por 35 em cada pedido subsequente. No entanto, alguns dos resultados na resposta subsequente podem ser duplicados da resposta anterior. Por exemplo, as primeiras duas imagens na resposta podem ser o mesmo que as últimas duas imagens da resposta anterior.

Para eliminar duplicados resultados, utilize o [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#nextoffset) campo a `Images` objeto. O `nextOffset` campo indica o `offset` a utilizar para o pedido seguinte. Por exemplo, se pretender que a página 30 imagens de cada vez, iria definir `count` para 30 e `offset` para 0 na sua primeiro pedido. No pedido seguinte, iria definir `count` para 30 e `offset` para o valor da resposta anterior `nextOffset`. A página trás, sugerimos que mantém uma pilha dos desvios anteriores e popping mais recente.

> [!NOTE]
> A paginação aplica-se apenas a pesquisa de imagem (imagens/pesquisa) e não as informações de imagem ou tendências imagens (imagens/tendências).