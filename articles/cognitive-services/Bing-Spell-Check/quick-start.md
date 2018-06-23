---
title: Início rápido a API de verificação ortográfica do Bing | Microsoft Docs
description: Mostra como começar a utilizar a API de verificação ortográfica do Bing.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: AF8EB1F0-386D-4555-9354-735611435F04
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: article
ms.date: 06/21/2016
ms.author: scottwhi
ms.openlocfilehash: cae8353e5be6e70eca90e5995b29b6774fc7d6a9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351500"
---
# <a name="your-first-spell-check-request"></a>Pedido de verificação ortográfica a primeiro

Antes de poder tornar a sua primeira chamada, terá de obter uma chave de subscrição de serviços cognitivos. Para obter uma chave, consulte [tente serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api).

Para verificar uma cadeia de texto para ortografia e erros de gramática, teria de enviar um pedido GET para o seguinte ponto final:  
  
```
https://api.cognitive.microsoft.com/bing/v5.0/spellcheck
```

> [!NOTE]
> Ponto final de pré-visualização de v7:
> 
> ```
> https://api.cognitive.microsoft.com/bing/v7.0/spellcheck
> ```  
  
O pedido tem de utilizar o protocolo HTTPS.

Recomendamos que todos os pedidos provenientes de um servidor. Distribuir a chave como parte de uma aplicação de cliente fornece mais oportunidade uma terceiros maliciosa para aceder ao mesmo. Além disso, efetuar chamadas a partir de um servidor fornece um único ponto de atualização para versões futuras da API.

O pedido tem de especificar o [texto](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#text) parâmetro de consulta, que contém a cadeia de texto a prova. Embora seja opcional, pedido deve também especificar o [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#mkt) parâmetro de consulta, que identifica o mercado onde pretende que os resultados de. Para obter uma lista opcional de parâmetros de consulta como `mode`, consulte [parâmetros de consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#query-parameters). Todos os valores de parâmetro de consulta tem de ser codificado de URL.  
  
O pedido tem de especificar o [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#subscriptionkey) cabeçalho. Embora seja opcional, são encorajados a também especificar os cabeçalhos seguintes:  
  
-   [Agente de utilizador](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#clientid)  
-   [X-pesquisa-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#clientip)  
-   [Localização de pesquisa X](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#location)  

Para obter uma lista de todos os cabeçalhos de pedido e resposta, consulte [cabeçalhos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#headers).

## <a name="the-request"></a>O pedido

O seguinte mostra um pedido que inclui todos os parâmetros de consulta sugerida e nos cabeçalhos. Se for a primeira vez ao chamar qualquer uma das APIs do Bing, não incluem o cabeçalho de ID de cliente. Inclua apenas o ID de cliente se anteriormente tiver chamar uma API do Bing e Bing devolveu um ID de cliente para o utilizador e a combinação de dispositivo. 
  
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
> Pedido de pré-visualização v7:

> ```  
> GET https://api.cognitive.microsoft.com/bing/v7.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us HTTP/1.1
> Ocp-Apim-Subscription-Key: 123456789ABCDE  
> X-MSEdge-ClientIP: 999.999.999.999  
> X-Search-Location: lat:47.60357;long:-122.3295;re:100  
> X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
> Host: api.cognitive.microsoft.com  
> ```  

O seguinte mostra a resposta ao pedido de anterior. O exemplo mostra também os cabeçalhos de resposta do Bing específicos.

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

Experimente a API. Aceda a [ortográfica consola de teste de verificação API](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358). 

Para obter detalhes sobre a consumir os objetos de resposta, consulte [cadeias de texto de verificação ortográfica](./proof-text.md).

