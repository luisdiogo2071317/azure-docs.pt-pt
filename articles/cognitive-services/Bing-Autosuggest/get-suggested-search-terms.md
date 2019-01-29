---
title: O que é a Sugestão Automática do Bing?
titlesuffix: Azure Cognitive Services
description: Saiba como utilizar a API de Sugestão Automática do Bing.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: overview
ms.date: 09/12/2017
ms.author: scottwhi
ms.openlocfilehash: e1e1ec2a9f5329f5d7331b7ce3ced4924d001a49
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55174181"
---
# <a name="what-is-bing-autosuggest"></a>O que é a Sugestão Automática do Bing?

Se enviar consultas para qualquer uma das APIs de Pesquisa do Bing, pode utilizar a API de Sugestão Automática do Bing para melhorar a experiência da caixa de pesquisa. A API de Sugestão Automática do Bing devolve uma lista de consultas sugeridas com base na cadeia de consulta parcial que o utilizador introduz na caixa de pesquisa. Apresente as sugestões na lista pendente da caixa de pesquisa. Os termos sugeridos são baseados em consultas sugeridas que outros utilizadores pesquisaram e na intenção do utilizador.

Normalmente, esta API será chamada sempre que o utilizador escreve um novo caráter na caixa de pesquisa. A exatidão da cadeia de consulta afeta a relevância dos termos de consulta sugeridos devolvidos pela API. Quanto mais completa for a cadeia de consulta, mais relevante será a lista de termos de consulta sugeridos. Por exemplo, as sugestões que a API pode devolver para *s* são provavelmente menos relevantes do que as consultas devolvidas para *sailing dinghies*.

## <a name="getting-suggested-search-terms"></a>Obter termos de pesquisa sugeridos

O seguinte exemplo mostra um pedido que devolve as cadeias de consulta sugeridas para *sail*. Não se esqueça de codificar com URL o termo de consulta parcial do utilizador quando definir o parâmetro de consulta [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference#query). Por exemplo, se o utilizador introduzir *sailing les*, defina `q` como `sailing+les` ou `sailing%20les`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

A seguinte resposta contém uma lista de objetos [SearchAction](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference#searchaction) com os termos de consulta sugeridos.

```json
{
    "url" : "https:\/\/www.bing.com\/search?q=sailing+lessons+seattle&FORM=USBAPI",
    "displayText" : "sailing lessons seattle",
    "query" : "sailing lessons seattle",
    "searchKind" : "WebSearch"
}, ...
```

Cada sugestão inclui um campo `displayText`, `query` e `url`. O campo `displayText` contém a consulta sugerida que utiliza para preencher a lista pendente da sua caixa de pesquisa. Tem de apresentar todas as sugestões incluídas na resposta e pela ordem fornecida.

Segue-se um exemplo de caixa de pesquisa pendente com termos de consulta sugeridos.

![Lista da caixa de pesquisa pendente de sugestão automática](./media/cognitive-services-bing-autosuggest-api/bing-autosuggest-drop-down-list.PNG)

Se o utilizador selecionar uma consulta sugerida na lista pendente, utilizará o termo de consulta no campo `query` para chamar a [API de Pesquisa na Web do Bing](../bing-web-search/search-the-web.md) e apresentar os resultados. Em alternativa, pode utilizar o URL no campo `url` para direcionar o utilizador para a página de resultados da pesquisa do Bing.

## <a name="throttling-requests"></a>Limitar pedidos

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Passos Seguintes

Para começar a trabalhar rapidamente com o seu primeiro pedido, veja [Fazer a Primeira Consulta](quickstarts/csharp.md).

Familiarize-se com a referência da [API de Sugestão Automática do Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference). A referência contém a lista de pontos finais, cabeçalhos e parâmetros de consulta que teria de utilizar para pedir termos de consulta sugeridos e as definições dos objetos de resposta.

Saiba como procurar na Web com a [API de Pesquisa na Web do Bing](../bing-web-search/search-the-web.md).

Não se esqueça de ler o artigo [Requisitos de Utilização e Apresentação do Bing](./useanddisplayrequirements.md) para que não infrinja nenhuma das regras relativas à utilização dos resultados da pesquisa.
