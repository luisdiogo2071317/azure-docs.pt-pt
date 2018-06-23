---
title: Ponto final de sugestão automática | Microsoft Docs
description: Resumo do ponto final de API de sugestão automática.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: article
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: 5aaddd09006cb6f1812bb6ae213a2f5e6720fb1b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354493"
---
# <a name="autosuggest-endpoint"></a>Ponto final de sugestão automática

O **API de sugestão automática** inclui um ponto final, que devolve uma lista de consultas sugeridas a partir de um termo de pesquisa parcial.

## <a name="endpoint"></a>Ponto Final

Para obter sugestões consultas utilizando a API do Bing, enviar uma `GET` pedido para o seguinte ponto final. Utilize os parâmetros de URL e cabeçalhos para definir mais especificações.

**Ponto final:** sugestões de pesquisa devolve como resultados JSON que são relevantes para a entrada do utilizador definida por `?q=""`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/Suggestions 
```

Para obter mais informações sobre cabeçalhos, parâmetros, códigos de mercado, objetos de resposta, erros, etc., consulte o [a API de sugestão automática do Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference) referência.

## <a name="response-json"></a>Resposta JSON

A resposta a um pedido de autosuggest inclui os resultados como objetos JSON. Para obter exemplos de analisar os resultados, consulte o [tutorial](tutorials/autosuggest.md) e [código fonte](tutorials/autosuggest-source.md).

## <a name="next-steps"></a>Passos Seguintes

O **Bing** ações de pesquisa devolvem resultados de acordo com o respetivo tipo de suporte de APIs. Todos os pontos finais de pesquisa devolvem resultados como objetos de resposta JSON.
Todos os pontos finais suportam consultas que devolvem um idioma específico e/ou a localização por radius de pesquisa, latitude e longitude.

Para obter informações completas acerca dos parâmetros suportados por cada ponto final, consulte as páginas de referência para cada tipo.
Para obter exemplos de pedidos básicos utilizando a API de sugestão automática, consulte [inícios rápidos de sugestão automática](https://docs.microsoft.com/azure/cognitive-services/Bing-Autosuggest).