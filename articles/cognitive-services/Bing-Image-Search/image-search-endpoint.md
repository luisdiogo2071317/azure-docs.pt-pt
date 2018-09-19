---
title: Pontos finais pesquisa de imagem - API de pesquisa de imagens do Bing
titleSuffix: Azure Cognitive Services
description: Uma lista de pontos finais disponíveis para a API de pesquisa de imagens do Bing.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 11/30/2017
ms.author: v-gedod
ms.openlocfilehash: ca38943908bf3eee04c40cf4decf81fd20b08a1f
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46295925"
---
# <a name="image-search-endpoints"></a>Pontos finais de pesquisa de imagem

O **API de pesquisa de imagens** inclui três pontos de extremidade.  Ponto final 1 devolve imagens da Web com base numa consulta. Ponto final 2 devolve [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse).  Ponto final 3 devolve imagens em destaque.
## <a name="endpoints"></a>Pontos Finais
Para obter resultados de imagem usando a API do Bing, envie um pedido para um dos seguintes pontos finais. Utilize os cabeçalhos e os parâmetros de URL para definir ainda mais especificações.

**Ponto final 1:** devolve imagens que são relevantes para a consulta de pesquisa do utilizador definida por `?q=""`.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search
```

**Ponto final 2:** retorna informações sobre uma imagem, usando `GET` ou `POST`.
```
 GET or POST https://api.cognitive.microsoft.com/bing/v7.0/images/details
```
Um pedido GET devolve informações sobre uma imagem, como páginas da Web que incluem a imagem. Incluir o [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) parâmetro com um `GET` pedido.

Ou, pode incluir uma imagem binária no corpo de um `POST` pedir e defina o [módulos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested) parâmetro `RecognizedEntities`. Isto irá devolver uma [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v5-reference#insightstoken) para utilizar como um parâmetro numa subsequentes `GET` solicitação, que retorna informações sobre as pessoas na imagem.  Definir `modules` para `All` para obter informações de todos, exceto `RecognizedEntities` nos resultados da `POST` sem fazer outra chamada que use o `insightsToken`.


**Ponto final de 3:** imagens devolve que são mais populares com base nos pedidos de pesquisa realizados por terceiros. As imagens são separadas em diferentes categorias, por exemplo, com base em eventos ou pessoas digno de nota.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending
```

Para obter uma lista de mercados que oferecem suporte a imagens em destaque, consulte [tendências imagens](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/trending-images).

Para obter detalhes sobre os cabeçalhos, parâmetros, códigos de mercado, objetos de resposta, erros, etc., consulte a [API de pesquisa de imagens do Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) referência.
## <a name="response-json"></a>Resposta JSON
A resposta a um pedido de pesquisa de imagem inclui resultados como objetos JSON. Para obter exemplos de analisar os resultados, consulte a [tutorial](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app) e [código-fonte](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app-source).

## <a name="next-steps"></a>Passos Seguintes
O **Bing** ações de pesquisa devolvem resultados de acordo com seu tipo de suporte de APIs. Todos os pontos finais de pesquisa devolvem resultados como objetos de resposta JSON.  Todos os pontos finais suportam consultas que devolvem um idioma específico e/ou localizações, longitude, latitude e radius de pesquisa.

Para obter informações completas sobre os parâmetros suportados por cada ponto de extremidade, consulte as páginas de referência para cada tipo.
Para obter exemplos de solicitações básicas com a API de pesquisa de imagens, consulte [inícios rápidos de pesquisa de imagens](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/search-the-web).
