---
title: Sugestão automática do início rápido da API | Microsoft Docs
description: Mostra como começar a utilizar a API de sugestão automática do Bing.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 1482E781-7352-4A3F-B1D5-B896381348C4
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: a7b54a1fb0b7c76eb72097357a6b51aa02e6e2fd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354578"
---
# <a name="making-your-first-autosuggest-query"></a>Tornar a sua primeira consulta Autosuggest

Antes de poder tornar a sua primeira chamada, terá de obter uma chave de subscrição de serviços cognitivos. Para obter uma chave, consulte [tente serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=autosuggest-api).

Para obter os resultados da pesquisa Web, teria de enviar um pedido GET para o seguinte ponto final:

```http
https://api.cognitive.microsoft.com/bing/v5.0/Suggestions
```

> [!NOTE]
> Ponto final de pré-visualização de v7:
>
> ```http
> https://api.cognitive.microsoft.com/bing/v7.0/Suggestions
> ```

O pedido tem de utilizar o protocolo HTTPS.

Recomendamos que todos os pedidos provenientes de um servidor. Distribuir a chave como parte de uma aplicação de cliente fornece mais oportunidade uma terceiros maliciosa para aceder ao mesmo. Além disso, efetuar chamadas a partir de um servidor fornece um único ponto de atualização para versões futuras da API.

O pedido tem de especificar o [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query) parâmetro de consulta, que contém o termo de pesquisa parcial do utilizador. Embora seja opcional, pedido deve também especificar o [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#mkt) parâmetro de consulta, que identifica o mercado onde pretende que os resultados de. Para obter uma lista dos parâmetros de consulta opcionais, consulte [parâmetros de consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query-parameters). Todos os valores de parâmetro de consulta tem de ser codificado de URL.

O pedido tem de especificar o [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#subscriptionkey) cabeçalho. Embora seja opcional, são encorajados a também especificar os cabeçalhos seguintes:

- [Agente de utilizador](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#useragent)
- [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientid)
- [X-pesquisa-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientip)
- [Localização de pesquisa X](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#location)

Os cabeçalhos IP e a localização do cliente são importantes para devolver o conteúdo com suporte para localização.

Para obter uma lista de todos os cabeçalhos de pedido e resposta, consulte [cabeçalhos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#headers).

## <a name="the-request"></a>O pedido

O pedido deve incluir todos os parâmetros de consulta sugerida e nos cabeçalhos. Iria chamar esta API sempre que o utilizador escrever um caráter de novo na caixa de pesquisa. Devolve a amplitude do impacto da cadeia de consulta a importância da consulta sugerida termos que a API. Mais concluir a cadeia de consulta, o mais relevante que a lista de sugestões de consulta termos encontram-se. Por exemplo, as sugestões que pode devolver a API para *s* probabilidades de menos relevantes que as consultas devolve para *sailing dinghies*. 

O exemplo seguinte mostra um pedido que devolve as cadeias de consulta sugerida para *sail*.

```http
GET https://api.cognitive.microsoft.com/bing/v5.0/suggestions?q=sail&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

> [!NOTE]
> Pedido de pré-visualização v7:

> ```http
> GET https://api.cognitive.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us HTTP/1.1
> Ocp-Apim-Subscription-Key: 123456789ABCDE
> X-MSEdge-ClientIP: 999.999.999.999
> X-Search-Location: lat:47.60357;long:-122.3295;re:100
> X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
> Host: api.cognitive.microsoft.com
> ```

Se for a primeira vez ao chamar qualquer uma das APIs do Bing, não incluem o cabeçalho de ID de cliente. Inclua apenas o cabeçalho de ID de cliente se anteriormente tiver chamar uma API do Bing e Bing devolveu um ID de cliente para o utilizador e a combinação de dispositivo.

O seguinte mostra a resposta ao pedido de anterior. A resposta inclui um grupo de sugestão da web que contém uma lista de sugestões de consulta de pesquisa. Cada sugestão inclui um `displayText`, `query`, e `url` campo.

O `displayText` campo contém a consulta sugerida que pretende utilizar para preencher na lista pendente a caixa de pesquisa. Tem de apresentar todas as sugestões que a resposta inclui, e pela ordem indicada.  

Se o utilizador seleciona uma consulta da lista pendente, pode utilizar a cadeia de consulta no `query` campo para chamar o [API de pesquisa do Bing](../bing-web-search/search-the-web.md) e apresentar os resultados por si. Em alternativa, pode utilizar o URL no `url` página de resultados de campo para enviar ao utilizador para a pesquisa do Bing.

```  
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "Suggestions",
    "queryContext" : {
        "originalQuery" : "sail"
    },
    "suggestionGroups" : [{
        "name" : "Web",
        "searchSuggestions" : [{
            "url" : "https:\/\/www.bing.com\/search?q=sailing+lessons+seattle&FORM=USBAPI",
            "displayText" : "sailing lessons seattle",
            "query" : "sailing lessons seattle",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailor+moon+news&FORM=USBAPI",
            "displayText" : "sailor moon news",
            "query" : "sailor moon news",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailor+jack%27s+lincoln+city&FORM=USBAPI",
            "displayText" : "sailor jack's lincoln city",
            "query" : "sailor jack's lincoln city",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailing+anarchy&FORM=USBAPI",
            "displayText" : "sailing anarchy",
            "query" : "sailing anarchy",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailboats+for+sale&FORM=USBAPI",
            "displayText" : "sailboats for sale",
            "query" : "sailboats for sale",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailstn.mylabsplus.com&FORM=USBAPI",
            "displayText" : "sailstn.mylabsplus.com",
            "query" : "sailstn.mylabsplus.com",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailusfood&FORM=USBAPI",
            "displayText" : "sailusfood",
            "query" : "sailusfood",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailboats+for+sale+seattle&FORM=USBAPI",
            "displayText" : "sailboats for sale seattle",
            "query" : "sailboats for sale seattle",
            "searchKind" : "WebSearch"
        }]
    }]
}
```

## <a name="next-steps"></a>Passos Seguintes

Experimente a API. Aceda a [consola de teste de API de sugestão automática](https://dev.cognitive.microsoft.com/docs/services/56c7694ecf5ff801a090fbd1/operations/56c769a2cf5ff801a090fbd2).

Para obter detalhes sobre a consumir os objetos de resposta, consulte [obter termos de pesquisa sugerida](./get-suggested-search-terms.md).
