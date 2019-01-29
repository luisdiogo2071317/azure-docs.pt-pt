---
title: 'Início rápido: API de Sugestão Automática do Bing'
titlesuffix: Azure Cognitive Services
description: Mostra como começar a utilizar a API de Sugestão Automática do Bing.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: quickstart
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: c748f58028d747b2d963d6ac28ea2661e41ec68c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55184517"
---
# <a name="quickstart-making-your-first-autosuggest-query"></a>Início rápido: Tornar a sua primeira consulta de sugestão automática

Antes de poder fazer a primeira chamada, precisa de obter uma chave de subscrição dos Serviços Cognitivos. Para obter uma chave, veja [Experimentar os Serviços Cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=autosuggest-api).

Para obter os resultados da pesquisa na Web, enviaria um pedido GET ao seguinte ponto final:

```http
https://api.cognitive.microsoft.com/bing/v5.0/Suggestions
```

> [!NOTE]
> Ponto final da Pré-visualização V7:
>
> ```http
> https://api.cognitive.microsoft.com/bing/v7.0/Suggestions
> ```

O pedido tem de utilizar o protocolo HTTPS.

Recomendamos que todos os pedidos tenham origem num servidor. Distribuir a chave como parte de uma aplicação cliente fornece mais oportunidades para terceiros mal-intencionados acederem à mesma. Além disso, fazer chamadas a partir de um servidor fornece um único ponto de atualização para as futuras versões da API.

O pedido tem de especificar o parâmetro de consulta [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query), que contém o termo de pesquisa parcial do utilizador. Embora seja opcional, o pedido também deve especificar o parâmetro de consulta [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#mkt), que identifica o mercado de onde pretende que os resultados provenham. Para obter uma lista opcional de parâmetros de consulta, veja [Parâmetros de Consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query-parameters). Todos os valores de parâmetro de consulta têm de estar codificados com URL.

O pedido tem de especificar o cabeçalho [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#subscriptionkey). Embora seja opcional, recomenda-se que especifique também os seguintes cabeçalhos:

- [User-Agent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#useragent)
- [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientid)
- [X-Search-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientip)
- [X-Search-Location](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#location)

Os cabeçalhos de IP e localização do cliente são importantes para devolver o conteúdo com reconhecimento de local.

Para obter uma lista de todos os cabeçalhos de pedido e resposta, veja [Cabeçalhos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#headers).

## <a name="the-request"></a>O pedido

O pedido deve incluir todos os parâmetros e cabeçalhos de consulta sugeridos. Esta API será chamada sempre que o utilizador escrever um novo caráter na caixa de pesquisa. A exatidão da cadeia de consulta afeta a relevância dos termos de consulta sugeridos devolvidos pela API. Quanto mais completa for a cadeia de consulta, mais relevante será a lista de termos de consulta sugeridos. Por exemplo, as sugestões que a API pode devolver para *s* são provavelmente menos relevantes do que as consultas devolvidas para *sailing dinghies*. 

O seguinte exemplo mostra um pedido que devolve as cadeias de consulta sugeridas para *sail*.

```http
GET https://api.cognitive.microsoft.com/bing/v5.0/suggestions?q=sail&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

> [!NOTE]
> Pedido da Pré-visualização V7:

> ```http
> GET https://api.cognitive.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us HTTP/1.1
> Ocp-Apim-Subscription-Key: 123456789ABCDE
> X-MSEdge-ClientIP: 999.999.999.999
> X-Search-Location: lat:47.60357;long:-122.3295;re:100
> X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
> Host: api.cognitive.microsoft.com
> ```

Se for a primeira vez que está a chamar qualquer uma das APIs do Bing, não inclua o cabeçalho de ID de cliente. Inclua apenas o cabeçalho do ID de cliente se tiver chamado anteriormente uma API do Bing e o Bing tiver devolvido um ID de cliente para a combinação de utilizador e dispositivo.

O seguinte mostra a resposta ao pedido anterior. A resposta inclui um grupo de sugestões da Web que contém uma lista de sugestões de consulta de pesquisa. Cada sugestão inclui um campo `displayText`, `query` e `url`.

O campo `displayText` contém a consulta sugerida que utilizaria para preencher a lista pendente da sua caixa de pesquisa. Tem de apresentar todas as sugestões incluídas na resposta e pela ordem fornecida.  

Se o utilizador selecionar uma consulta na lista pendente, pode utilizar a cadeia de consulta no campo `query` para chamar a [API de Pesquisa do Bing](../bing-web-search/search-the-web.md) e apresentar os resultados. Em alternativa, pode utilizar o URL no campo `url` para direcionar o utilizador para a página de resultados da pesquisa do Bing.

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

Experimente a API. Aceda à [Consola de Testes da API de Sugestão Automática](https://dev.cognitive.microsoft.com/docs/services/56c7694ecf5ff801a090fbd1/operations/56c769a2cf5ff801a090fbd2).

Para obter detalhes sobre os objetos de resposta, veja [Obter Termos de Pesquisa Sugeridos](./get-suggested-search-terms.md).
