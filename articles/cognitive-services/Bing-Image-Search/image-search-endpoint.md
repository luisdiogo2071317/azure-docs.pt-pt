---
title: Pontos finais de pesquisa de imagem | Microsoft Docs
description: Resumo de ponto final de API do pesquisa a imagem.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 11/30/2017
ms.author: v-gedod
ms.openlocfilehash: 0d5f28bfdb45b27b04df068f75e8a20d0235d12b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351398"
---
# <a name="image-search-endpoints"></a>Pontos finais de pesquisa de imagem
O **API de pesquisa de imagem** inclui três pontos finais.  Ponto final 1 devolve imagens a partir Web baseada numa consulta. Devolve o ponto final 2 [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse).  Ponto final 3 devolve imagens tendências.
## <a name="endpoints"></a>Pontos Finais
Para obter resultados de imagem utilizando a API do Bing, envie um pedido para um dos seguintes pontos finais. Utilize os parâmetros de URL e cabeçalhos para definir mais especificações.

**Ponto final 1:** devolve imagens que são relevantes para a consulta de pesquisa do utilizador definida por `?q=""`.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search
```

**Ponto final 2:** devolve informações sobre uma imagem, utilizando o `GET` ou `POST`.
``` 
 GET or POST https://api.cognitive.microsoft.com/bing/v7.0/images/details
```
Um pedido GET devolve informações sobre uma imagem, tais como páginas Web que inclui a imagem. Incluir o [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) parâmetro com um `GET` pedido.

Em alternativa, pode incluir uma imagem de binária no corpo de um `POST` pedido e defina o [módulos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested) parâmetro para `RecognizedEntities`. Esta ação irá devolver um [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v5-reference#insightstoken) para utilizar como um parâmetro num subsequentes `GET` pedido, que devolve informações sobre as pessoas na imagem.  Definir `modules` para `All` para obter todas as informações, exceto `RecognizedEntities` nos resultados do `POST` sem fazer através de chamada de outro o `insightsToken`. 


**Ponto final 3:** imagens devolve que são tendências com base nos pedidos de pesquisa efetuados por outras pessoas. As imagens estão separadas em diferentes categorias, por exemplo, com base nas pessoas noteworthy ou eventos.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending
```

Para obter uma lista de mercados suporta imagens tendências, consulte [tendências imagens](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/trending-images).

Para obter mais informações sobre cabeçalhos, parâmetros, códigos de mercado, objetos de resposta, erros, etc., consulte o [v7 de API de pesquisa do Bing imagem](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) referência.
## <a name="response-json"></a>Resposta JSON
A resposta a um pedido de pesquisa de imagem inclui os resultados como objetos JSON. Para obter exemplos de analisar os resultados, consulte o [tutorial](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app) e [código fonte](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app-source).

## <a name="next-steps"></a>Passos Seguintes
O **Bing** ações de pesquisa devolvem resultados de acordo com o respetivo tipo de suporte de APIs. Todos os pontos finais de pesquisa devolvem resultados como objetos de resposta JSON.  Todos os pontos finais suportam consultas que devolvem um idioma específico e/ou a localização por radius de pesquisa, latitude e longitude.

Para obter informações completas acerca dos parâmetros suportados por cada ponto final, consulte as páginas de referência para cada tipo.
Para obter exemplos de pedidos básicos utilizando a API de pesquisa de imagem, consulte [imagem pesquisa rápida-inicia](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/search-the-web).