---
title: 'Início rápido: API de Verificação Ortográfica do Bing'
titlesuffix: Azure Cognitive Services
description: Mostra como começar a utilizar a API de Verificação de Ortografia do Bing.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 06/21/2016
ms.author: scottwhi
ms.openlocfilehash: 70aec949dd8f1b27a2a04c28a892b4caa2122844
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55867822"
---
# <a name="quickstart-your-first-spell-check-request"></a>Início rápido: Pedido de verificação de sua ortográfica primeiro

Obtenha uma [chave de acesso aos Serviços Cognitivos](https://azure.microsoft.com/try/cognitive-services/) em **Pesquisa**.  Consulte também [dos serviços cognitivos preços - API de pesquisa Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

Para verificar se existem erros de ortografia e de gramática numa cadeia de texto, enviaria um pedido GET para o seguinte ponto final:  
  
```
https://api.cognitive.microsoft.com/bing/v5.0/spellcheck
```

> [!NOTE]
> Ponto final da Pré-visualização V7:
> 
> ```
> https://api.cognitive.microsoft.com/bing/v7.0/spellcheck
> ```  
  
O pedido tem de utilizar o protocolo HTTPS.

Recomendamos que todos os pedidos tenham origem num servidor. Distribuir a chave como parte de uma aplicação cliente fornece mais oportunidades para terceiros mal-intencionados acederem à mesma. Além disso, fazer chamadas a partir de um servidor fornece um único ponto de atualização para as futuras versões da API.

O pedido tem de especificar o parâmetro de consulta [text](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#text), que contém a cadeia de texto a verificar. Embora seja opcional, o pedido deve também especificar o parâmetro de consulta [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#mkt), que identifica o mercado de onde pretende que os resultados provenham. Para obter uma lista opcional de parâmetros de consulta, como `mode`, veja [Parâmetros de Consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#query-parameters). Todos os valores de parâmetro de consulta têm de estar codificados com URL.  
  
O pedido tem de especificar o cabeçalho [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#subscriptionkey). Embora seja opcional, recomenda-se que especifique também os seguintes cabeçalhos:  
  
-   [User-Agent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#clientid)  
-   [X-Search-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#clientip)  
-   [X-Search-Location](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#location)  

Para obter uma lista de todos os cabeçalhos de pedido e resposta, veja [Cabeçalhos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#headers).

## <a name="the-request"></a>O pedido

A seguir, é-lhe mostrado um pedido que inclui todos os parâmetros de consulta e cabeçalhos sugeridos. Se for a primeira vez que está a chamar qualquer uma das APIs do Bing, não inclua o cabeçalho de ID de cliente. Inclua apenas o ID de cliente se tiver chamado anteriormente uma API do Bing e o Bing tiver devolvido um ID de cliente para o utilizador e a combinação de dispositivo. 
  
```  
GET https://api.cognitive.microsoft.com/bing/v5.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

> [!NOTE]
> Pedido da Pré-visualização V7:

> ```  
> GET https://api.cognitive.microsoft.com/bing/v7.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us HTTP/1.1
> Ocp-Apim-Subscription-Key: 123456789ABCDE  
> X-MSEdge-ClientIP: 999.999.999.999  
> X-Search-Location: lat:47.60357;long:-122.3295;re:100  
> X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
> Host: api.cognitive.microsoft.com  
> ```  

O código a seguir mostra a resposta ao pedido anterior. O exemplo também mostra os cabeçalhos de resposta específicos do Bing.

```
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{  
    "_type" : "SpellCheck",  
    "flaggedTokens" : [{  
        "offset" : 5,  
        "token" : "its",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "it's",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 25,  
        "token" : "john",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "John",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 19,  
        "token" : "turn",  
        "type" : "RepeatedToken",  
        "suggestions" : [{  
            "suggestion" : "",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 35,  
        "token" : "runing",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "running",  
            "score" : 1  
        }]  
    }]  
}  
```  


## <a name="next-steps"></a>Passos Seguintes

Experimente a API. Aceda à [Consola de Teste da API de Verificação de Ortografia do Bing](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358). 

Para obter detalhes sobre o consumo dos objetos de resposta, veja [Verificar a ortografia de cadeias de texto](./proof-text.md).

