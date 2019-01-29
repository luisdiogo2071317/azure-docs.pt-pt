---
title: Ponto final de Sugestão Automática do Bing
titlesuffix: Azure Cognitive Services
description: Resumo do ponto de extremidade a API de sugestão automática do Bing.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: conceptual
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: 03f7ac11b08d9cad633e207337ff963114f2c68f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55149242"
---
# <a name="bing-autosuggest-endpoint"></a>Ponto final de Sugestão Automática do Bing

O **a API de sugestão automática do Bing** inclui um ponto de extremidade, que retorna uma lista de consultas sugeridas a partir de um termo de pesquisa parcial.

## <a name="endpoint"></a>Ponto Final

Para obter consultas sugeridas com a API do Bing, enviar um `GET` pedido para o seguinte ponto de extremidade. Utilize os cabeçalhos e os parâmetros de URL para definir ainda mais especificações.

**Ponto final:** Sugestões de pesquisa devolve resultados JSON que são relevantes para a entrada do usuário definida por `?q=""`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/Suggestions 
```

Para obter detalhes sobre os cabeçalhos, parâmetros, códigos de mercado, objetos de resposta, erros, etc., consulte a [a API de sugestão automática do Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference) referência.

## <a name="response-json"></a>Resposta JSON

A resposta a um pedido de sugestão automática inclui resultados como objetos JSON. Para obter exemplos de analisar os resultados, consulte a [tutorial](tutorials/autosuggest.md) e [código-fonte](tutorials/autosuggest-source.md).

## <a name="next-steps"></a>Passos Seguintes

O **Bing** ações de pesquisa devolvem resultados de acordo com seu tipo de suporte de APIs. Todos os pontos finais de pesquisa devolvem resultados como objetos de resposta JSON.
Todos os pontos finais suportam consultas que devolvem um idioma específico e/ou localizações, longitude, latitude e radius de pesquisa.

Para obter informações completas sobre os parâmetros suportados por cada ponto de extremidade, consulte as páginas de referência para cada tipo.
Para obter exemplos de solicitações básicas com a API de sugestão automática, consulte [inícios rápidos de sugestão automática](https://docs.microsoft.com/azure/cognitive-services/Bing-Autosuggest).
