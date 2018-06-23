---
title: Ponto final de pesquisa de entidade | Microsoft Docs
description: Resumo do ponto final da API de pesquisa de entidade.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: a2557c6000445544b3b47a05d7d356ccaa9928b4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351403"
---
# <a name="entity-search-endpoint"></a>Ponto final de pesquisa de entidade
O **API de pesquisa de entidade** inclui um ponto final que devolve entidades da Web com base numa consulta.

## <a name="endpoint"></a>Ponto Final
Para obter a entidade os resultados utilizando o **API do Bing**, enviar uma `GET` pedido para o seguinte ponto final. Utilize os parâmetros de URL e cabeçalhos para definir mais especificações.

**Ponto final:** devolve entidades que são relevantes para a consulta de pesquisa do utilizador definida por `?q=""`.
```
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

Para obter mais informações sobre cabeçalhos, parâmetros, códigos de mercado, objetos de resposta, erros, etc., consulte o [v7 de API de pesquisa do Bing entidade](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) referência.

## <a name="response-json"></a>Resposta JSON
A resposta a um pedido de pesquisa de entidade inclui os resultados como objetos JSON. Para obter exemplos de resultados, consulte [começar](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start).

## <a name="next-steps"></a>Passos Seguintes
O **Bing** ações de pesquisa devolvem resultados de acordo com o respetivo tipo de suporte de APIs. Todos os pontos finais de pesquisa devolvem resultados como objetos de resposta JSON.  Todos os pontos finais suportam consultas que devolvem um idioma específico e/ou a localização por radius de pesquisa, latitude e longitude.

Para obter informações completas acerca dos parâmetros suportados por cada ponto final, consulte as páginas de referência para cada tipo.
Para obter exemplos utilizando a API de pesquisa de entidade, consulte [começar](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start) e [Resizing e cropping imagens em miniatura](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/resize-and-crop-thumbnails).