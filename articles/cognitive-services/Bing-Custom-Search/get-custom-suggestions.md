---
title: 'Pesquisa do Bing personalizada: Obter sugestões de sugestão automática do personalizado | Microsoft Docs'
description: Descreve como obter sugestões de Autosuggest personalizados
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.technology: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 73059038018602f7aa76377bf7c98d4658576576
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352861"
---
# <a name="get-custom-suggestions"></a>Obter sugestões personalizados
Antes de enviar consultas para a pesquisa do Bing personalizada, chame a API de sugestão automática do personalizada para sugestões de termo de pesquisa e melhorar a experiência de pesquisa. A API de sugestão automática personalizada devolve uma lista de consultas sugeridas baseadas numa cadeia de consulta parcial que o utilizador fornece. Quaisquer termos de personalizadas de consulta que especificou são apresentados antes das sugestões que Autosuggest gera. Para obter mais informações, consulte [definir sugestões de pesquisa personalizada](define-custom-suggestions.md).

## <a name="endpoint"></a>Ponto Final
Para obter sugeridas consultas utilizando a API de pesquisa do Bing personalizada, enviar uma `GET` pedido para o seguinte ponto final.

```
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions 
```

## <a name="response-json"></a>Resposta JSON
A resposta contém uma lista de objetos de SearchAction que contêm os termos de sugestões de consulta.

```
        {  
            "displayText" : "sailing lessons seattle",  
            "query" : "sailing lessons seattle",  
            "searchKind" : "CustomSearch"  
        },  
```

Cada sugestão inclui um `displayText` e `query` campo. O `displayText` campo contém a consulta sugerida que utiliza para preencher na lista pendente a caixa de pesquisa.

Se o utilizador seleciona uma consulta sugerida na lista pendente, utilize o termo de consulta no `query` campo ao chamar o [API de pesquisa do Bing personalizada](overview.md).

## <a name="throttling-requests"></a>Limitar pedidos

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Passos Seguintes

- [Chamar a pesquisa personalizada](./search-your-custom-view.md)
- [Configurar a sua experiência de IU alojada](./hosted-ui.md)