---
title: Ponto final de Sugestão Automática do Bing
titlesuffix: Azure Cognitive Services
description: Resumo do ponto de extremidade a API de sugestão automática do Bing.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: conceptual
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: c2d1c97ad2af266558f9b664162526d5006d2092
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/06/2018
ms.locfileid: "48830449"
---
# <a name="bing-autosuggest-endpoint"></a>Ponto final de Sugestão Automática do Bing

O **a API de sugestão automática do Bing** inclui um ponto de extremidade, que retorna uma lista de consultas sugeridas a partir de um termo de pesquisa parcial.

## <a name="endpoint"></a>Ponto Final

Para obter consultas sugeridas com a API do Bing, enviar um `GET` pedido para o seguinte ponto de extremidade. Utilize os cabeçalhos e os parâmetros de URL para definir ainda mais especificações.

**Ponto final:** sugestões de pesquisa devolve resultados JSON que são relevantes para a entrada do usuário definida por `?q=""`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/Suggestions 
```

Para obter detalhes sobre os cabeçalhos, parâmetros, códigos de mercado, objetos de resposta, erros, etc., consulte a [a API de sugestão automática do Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference) referência.

## <a name="response-json"></a>Resposta JSON

A resposta a um pedido de sugestão automática inclui resultados como objetos JSON. Para obter exemplos de analisar os resultados, consulte a [tutorial](tutorials/autosuggest.md) e [código-fonte](tutorials/autosuggest-source.md).

## <a name="next-steps"></a>Passos Seguintes

O **Bing** ações de pesquisa devolvem resultados de acordo com seu tipo de suporte de APIs. Todos os pontos finais de pesquisa devolvem resultados como objetos de resposta JSON.
Todos os pontos finais suportam consultas que devolvem um idioma específico e/ou localizações, longitude, latitude e radius de pesquisa.

Para obter informações completas sobre os parâmetros suportados por cada ponto de extremidade, consulte as páginas de referência para cada tipo.
Para obter exemplos de solicitações básicas com a API de sugestão automática, consulte [inícios rápidos de sugestão automática](https://docs.microsoft.com/azure/cognitive-services/Bing-Autosuggest).