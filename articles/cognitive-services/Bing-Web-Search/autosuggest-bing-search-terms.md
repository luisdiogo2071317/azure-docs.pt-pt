---
title: Sugestão automática termos de pesquisa do Bing
titleSuffix: Microsoft Cognitive Services
description: Emparelhe a API de pesquisa Web Bing com a API de sugestão automática do Bing para fornecer aos utilizadores uma experiência de pesquisa melhorada.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 8/13/2018
ms.author: erhopf
ms.openlocfilehash: df8a57b3136abfacce971f4d01ccb2296dfa784c
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42889438"
---
# <a name="autosuggest-bing-search-terms"></a>Sugestão automática termos de pesquisa do Bing

Se disponibilizar uma caixa de pesquisa na qual o utilizador introduz o seu termo de pesquisa, utilize a [API de Sugestão Automática do Bing](../bing-autosuggest/get-suggested-search-terms.md) para melhorar a experiência. A API devolve cadeias de consulta sugerida com base em termos de pesquisa parcial à medida que o utilizador escreve.

Depois do utilizador introduzir um termo de pesquisa, tem de ser URL codificada antes do [p](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#query) parâmetro de consulta está definido. Por exemplo, se o utilizador introduzir *sailing dinghies*, defina `q` como `sailing+dinghies` ou `sailing%20dinghies`.

Se o termo de consulta contém um erro de ortografia, a resposta de pesquisa inclui um [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#querycontext) objeto. O objeto mostra a ortografia original e a ortografia corrigida que Bing utilizado para a pesquisa.

```json
"queryContext": {
    "originalQuery": "sialing dingy for sale",
    "alteredQuery": "sailing dinghy for sale",
    "alterationOverrideQuery": "+sialing +dingy for sale"
}
```

Pode usar essas informações para permitir que o utilizador sabe que modificou sua cadeia de consulta quando exibir os resultados da pesquisa.

![Exemplo de experiência do Usuário do contexto de consulta](./media/cognitive-services-bing-web-api/bing-query-context.PNG)  

## <a name="next-steps"></a>Passos Seguintes  

* Exemplo de revisão [respostas de API de pesquisa Web Bing](search-responses.md).  

## <a name="see-also"></a>Consulte também  

* [Referência da API de pesquisa Web Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
