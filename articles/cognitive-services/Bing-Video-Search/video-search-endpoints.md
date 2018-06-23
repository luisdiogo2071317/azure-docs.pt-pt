---
title: Pontos finais de pesquisa de vídeo | Microsoft Docs
description: Resumo de ponto final de API do pesquisa as vídeo.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 12/04/2017
ms.author: v-gedod
ms.openlocfilehash: 9836d9928362ab37b0a81ff5043d99f9bf353f22
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351499"
---
# <a name="video-search-endpoints"></a>Pontos finais de pesquisa vídeos
O **API de pesquisa de vídeo** inclui três pontos finais.  Ponto final 1 devolve vídeos da Web com base numa consulta. Ponto final 2 devolve informações sobre um vídeo com base no `modules` parâmetro de URL.  Ponto final 3 devolve imagens tendências.

## <a name="endpoints"></a>Pontos Finais
Para obter resultados vídeos utilizando a API do Bing, enviar uma `GET` pedido a um dos seguintes pontos finais. Utilize os parâmetros de URL e cabeçalhos para definir mais especificações.

**Endpoint1:** devolve vídeos que são relevantes para a consulta de pesquisa do utilizador definida por `?q=""`.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search
```

**Ponto final 2:** devolve informações sobre um vídeo, tais como vídeos relacionados. Incluir o `modules` [parâmetro de consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query-parameters), que é uma lista delimitada por vírgulas das informações para o pedido.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/details
```

**Ponto final 3:** vídeos devolve que são tendências com base nos pedidos de pesquisa efetuados por outras pessoas. Os vídeos estão separados em diferentes categorias, por exemplo, com base nas pessoas noteworthy ou eventos.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending
```

Para obter mais informações sobre cabeçalhos, parâmetros, códigos de mercado, objetos de resposta, erros, etc., consulte o [v7 de API de pesquisa do Bing vídeo](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) referência.
## <a name="response-json"></a>Resposta JSON
A resposta a um pedido de pesquisa de vídeos inclui os resultados como objetos JSON. Para obter exemplos de analisar os resultados, consulte o [tutorial](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app) e [código fonte](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app-source).

## <a name="next-steps"></a>Passos Seguintes
O **Bing** ações de pesquisa devolvem resultados de acordo com o respetivo tipo de suporte de APIs. Todos os pontos finais de pesquisa devolvem resultados como objetos de resposta JSON.  Todos os pontos finais suportam consultas que devolvem um idioma específico e/ou a localização por radius de pesquisa, latitude e longitude.

Para obter informações completas acerca dos parâmetros suportados por cada ponto final, consulte as páginas de referência para cada tipo.
Para obter exemplos de pedidos básicos utilizando a API de pesquisa de vídeo, consulte [vídeo pesquisa rápida-inicia](https://docs.microsoft.com/azure/cognitive-services/bing-video-search).