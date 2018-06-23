---
title: Utilizar a classificação de mensagens em fila para apresentar respostas | Microsoft Docs
description: Mostra como utilizar a classificação de mensagens em fila para apresentar as respostas que devolve a API de pesquisa do Bing entidade.
services: cognitive-services
author: v-jerkin
manager: ehansen
ms.assetid: BBF87972-B6C3-4910-BB52-DE90893F6C71
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 12/12/2017
ms.author: v-jerkin
ms.openlocfilehash: ff5b004aaa863dbdfc460a774a5dfd658ce52537
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351428"
---
# <a name="using-ranking-to-display-results"></a>Utilizar a classificação de mensagens em fila para apresentar resultados  

Cada resposta de pesquisa de entidade inclui um [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-entities-v7-reference#rankingresponse) responder, semelhante à de uma resposta de pesquisa na Web Bing, que especifica como tem de apresentar os resultados da pesquisa. A resposta de classificação agrupa os resultados em pólo, mainline e o conteúdo da barra lateral. O resultado ao pólo é o resultado mais importante ou prominent e deverá ser apresentado pela primeira vez. Se não visualizar os resultados restantes num tradicional mainline e o formato da barra lateral, tem de fornecer a visibilidade superior conteúda mainline que o conteúdo da barra lateral. 
  
Em cada grupo, o [itens](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankinggroup-items) matriz identifica a ordem em que o conteúdo tem de aparecer no. Cada item fornece duas formas para identificar o resultado dentro de uma resposta.  
  
-   `answerType` e `resultIndex` — o `answerType` campo identifica a resposta (entidade ou direta) e `resultIndex` identifica um resultado na resposta (por exemplo, uma entidade). O índice é baseada zero.  
  
-   `value` — O `value` campo contém um ID que corresponde ao ID de uma resposta ou um resultado na resposta. A resposta ou os resultados de conter o ID, mas não ambos.  
  
Com o ID requer que corresponde ao ID de classificação com o ID de uma resposta ou um dos respetivos resultados. Se um objeto de resposta inclui um `id` campo, apresentar os resultados da resposta em conjunto. Por exemplo, se o `Entities` objeto inclui o `id` campo, apresentar todos os artigos de entidades em conjunto. Se o `Entities` objeto não inclui o `id` campo, em seguida, cada entidade contém um `id` campo e a resposta de classificação mistura as entidades com os resultados de locais.  
  
Utilizar o `answerType` e `resultIndex` é um processo de dois passos. Em primeiro lugar, utilize `answerType` para identificar a resposta que contém os resultados a apresentar. Em seguida, utilizar `resultIndex` índice para os resultados essa resposta para obter o resultado para apresentar. (O `answerType` valor é o nome do campo no [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse) objeto.) Se estiver a deveria para apresentar resultados de todos os a resposta em conjunto, a resposta de classificação item não inclui o `resultIndex` campo.

## <a name="ranking-response-example"></a>Exemplo de resposta de classificação

O seguinte mostra um exemplo [RankingResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#rankingresponse).
  
```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "Jimi Hendrix"
  },
  "entities": { ... },
  "rankingResponse": {
    "sidebar": {
      "items": [
        {
          "answerType": "Entities",
          "resultIndex": 0,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.0"
          }
        },
        {
          "answerType": "Entities",
          "resultIndex": 1,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.1"
          }
        }
      ]
    }
  }
}
```

Com base nesta resposta de classificação, a barra lateral seria mostrar os resultados de dois entidade relacionados com Jimi Hendrix.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Tutorial de pesquisa do Bing entidade](tutorial-bing-entities-search-single-page-app.md)
