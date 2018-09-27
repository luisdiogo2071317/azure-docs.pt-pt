---
title: Pontos finais de pesquisa de vídeos - vídeo de pesquisa do Bing
titlesuffix: Azure Cognitive Services
description: Resumo do ponto de final de API de pesquisa de vídeos do Bing.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: conceptual
ms.date: 12/04/2017
ms.author: rosh
ms.openlocfilehash: c153f577f76944d22f9a1b0fb4b24d332d2a02c8
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220776"
---
# <a name="video-search-endpoints"></a>Vídeo pesquisar pontos finais
O **API de pesquisa de vídeo** inclui três pontos de extremidade.  Ponto final 1 devolve vídeos da Web com base numa consulta. Ponto final 2 devolve informações sobre um vídeo com base no `modules` parâmetro de URL.  Ponto final 3 devolve imagens em destaque.

## <a name="endpoints"></a>Pontos Finais
Para obter resultados de vídeo com a API do Bing, enviar um `GET` solicitação a um dos seguintes pontos finais. Utilize os cabeçalhos e os parâmetros de URL para definir ainda mais especificações.

**Endpoint1:** devolve os vídeos que são relevantes para a consulta de pesquisa do utilizador definida por `?q=""`.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search
```

**Ponto final 2:** devolve informações sobre um vídeo, como vídeos relacionados. Incluir o `modules` [parâmetro de consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query-parameters), que é uma lista delimitada por vírgulas de informações para pedir.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/details
```

**Ponto final de 3:** vídeos devolve que são mais populares com base nos pedidos de pesquisa realizados por terceiros. Os vídeos são separados em diferentes categorias, por exemplo, com base em eventos ou pessoas digno de nota.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending
```

Para obter detalhes sobre os cabeçalhos, parâmetros, códigos de mercado, objetos de resposta, erros, etc., consulte a [API de pesquisa de vídeos do Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) referência.
## <a name="response-json"></a>Resposta JSON
A resposta a um pedido de pesquisa de vídeos inclui resultados como objetos JSON. Para obter exemplos de analisar os resultados, consulte a [tutorial](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app) e [código-fonte](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app-source).

## <a name="next-steps"></a>Passos Seguintes
O **Bing** ações de pesquisa devolvem resultados de acordo com seu tipo de suporte de APIs. Todos os pontos finais de pesquisa devolvem resultados como objetos de resposta JSON.  Todos os pontos finais suportam consultas que devolvem um idioma específico e/ou localizações, longitude, latitude e radius de pesquisa.

Para obter informações completas sobre os parâmetros suportados por cada ponto de extremidade, consulte as páginas de referência para cada tipo.
Para obter exemplos de solicitações básicas com a API de pesquisa de vídeo, consulte [pesquisa de vídeos inícios rápidos das](https://docs.microsoft.com/azure/cognitive-services/bing-video-search).