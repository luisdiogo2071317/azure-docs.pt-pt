---
title: Como paginar as imagens disponíveis - API de pesquisa de imagens do Bing
titleSuffix: Azure Cognitive Services
description: Aprenda a página por meio de todas as imagens que o Bing pode devolver.
services: cognitive-services
author: swhite-msft
manager: cgonlun
ms.assetid: 3C8423F8-41E0-4F89-86B6-697E840610A7
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 019d91f6a86bab5c4f446085e0244f9b5323f1fb
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46294412"
---
# <a name="paging-results"></a>Resultados de paginação

Quando chama a API de pesquisa de imagens, o Bing devolve uma lista de resultados. A lista é um subconjunto do número total de resultados que são relevantes para a consulta. Para obter o número total de resultados disponíveis, acessar o objeto de resposta [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#totalestimatedmatches) campo.  

A exemplo a seguir mostra o `totalEstimatedMatches` campo que inclui uma resposta de imagens.  

```json
{
    "_type" : "Images",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118C8...",
    "totalEstimatedMatches" : 838,
    "value" : [...]  
}  
```  

Paginar através de imagens disponíveis, utilize o [contagem](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#count) e [deslocamento](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offset) parâmetros de consulta.  

O `count` parâmetro especifica o número de resultados a devolver na resposta. O número máximo de resultados que pode solicitar na resposta é 150. A predefinição é 35. O número real entregue pode ser menor do que o pedido.

O `offset` parâmetro especifica o número de resultados a ignorar. O `offset` é baseado em zero e deve ser inferior a (`totalEstimatedMatches` - `count`).  

Se desejar exibir 20 imagens por página, definiria `count` para 20 e `offset` como 0 para obter a primeira página de resultados. O código a seguir mostra um exemplo que solicite 20 imagens começando no desvio 40.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Se a predefinição `count` valor funciona para a sua implementação, só precisa de especificar o `offset` parâmetro de consulta.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Pode esperar que se a página 35 imagens ao mesmo tempo, definiria a `offset` parâmetro como 0 em sua primeira solicitação de consulta e, em seguida, incrementar `offset` por 35 em cada pedido subsequente. No entanto, alguns dos resultados na resposta subsequente podem estar duplicados da resposta anterior. Por exemplo, as duas primeiras imagens na resposta podem ser o mesmo que as duas últimas imagens da resposta anterior.

Para eliminar duplicados resultados, utilize o [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#nextoffset) campo o `Images` objeto. O `nextOffset` campo indica o `offset` a utilizar para a sua próxima solicitação. Por exemplo, se quiser página 30 imagens ao mesmo tempo, definiria `count` de 30 e `offset` para 0 na sua primeira solicitação. No seu pedido seguinte, definiria `count` de 30 e `offset` para o valor da resposta anterior `nextOffset`. A página para trás, sugerimos que mantém uma pilha dos deslocamentos anteriores e POP mais recente.

> [!NOTE]
> Paginação aplica-se apenas a pesquisa de imagens (/ imagens/search) e não para informações de imagem ou imagens em destaque (/ imagens/tendências).
