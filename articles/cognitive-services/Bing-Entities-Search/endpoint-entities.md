---
title: Ponto final da Pesquisa de Entidades do Bing
titlesuffix: Azure Cognitive Services
description: Resumo do ponto de final de API de pesquisa de entidades do Bing.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: conceptual
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: d781a4b3cd0119f5624b4dd20b514894ea339414
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816587"
---
# <a name="entity-search-endpoint"></a>Ponto final de pesquisa de entidades
O **API de pesquisa de entidades** inclui um ponto de extremidade que retorne entidades a partir da Web com base numa consulta.

## <a name="endpoint"></a>Ponto Final
Para obter os resultados utilizando o **API do Bing**, enviar um `GET` pedido para o seguinte ponto de extremidade. Utilize os cabeçalhos e os parâmetros de URL para definir ainda mais especificações.

**Ponto final:** devolve entidades que são relevantes para a consulta de pesquisa do utilizador definida por `?q=""`.
```
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

Para obter detalhes sobre os cabeçalhos, parâmetros, códigos de mercado, objetos de resposta, erros, etc., consulte a [API de pesquisa de entidades do Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) referência.

## <a name="response-json"></a>Resposta JSON
A resposta a um pedido de pesquisa de entidades inclui resultados como objetos JSON. Para obter exemplos dos resultados, consulte [começar](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start).

## <a name="next-steps"></a>Passos Seguintes
O **Bing** ações de pesquisa devolvem resultados de acordo com seu tipo de suporte de APIs. Todos os pontos finais de pesquisa devolvem resultados como objetos de resposta JSON.  Todos os pontos finais suportam consultas que devolvem um idioma específico e/ou localizações, longitude, latitude e radius de pesquisa.

Para obter informações completas sobre os parâmetros suportados por cada ponto de extremidade, consulte as páginas de referência para cada tipo.
Para obter exemplos utilizam a API de pesquisa de entidades, veja [começar](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start) e [redimensionamento e corte imagens em miniatura](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/resize-and-crop-thumbnails).