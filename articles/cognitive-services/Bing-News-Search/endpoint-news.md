---
title: Pontos finais de pesquisa de notícias do Bing | Documentos da Microsoft
description: Resumo do ponto de extremidade de API de pesquisa de notícias.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-gedod
ms.openlocfilehash: ab892e947566adf025499382b213a52ed3e96e35
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47433689"
---
# <a name="news-search-endpoints"></a>Pontos finais de pesquisa de notícias
O **API de pesquisa de notícias** devolve notícias imagens de páginas da Web, artigos, vídeos, e [entidades](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Entidades contêm informações de resumo sobre uma pessoa, lugar ou tópico.
## <a name="endpoints"></a>Pontos Finais
Para obter resultados de pesquisa de notícias com a API do Bing, enviar um `GET` solicitação a um dos seguintes pontos finais. Os cabeçalhos e os parâmetros de URL definem ainda mais especificações.

**Ponto final 1:** retorna itens de notícias com base na consulta de pesquisa do usuário. Se a consulta de pesquisa estiver vazia, a chamada retorna os artigos de notícias superior. A consulta `?q=""` opção também pode ser utilizada com o `/news` URL. Para disponibilidade, consulte [países e mercados suportados](language-support.md#supported-markets-for-news-search-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search
```

**Ponto final 2:** devolve os itens de notícias superior por categoria. Pode pedir especificamente o negócio superior, desporto ou artigos de entretenimento usando `category=business`, `category=sports`, ou `category=entertainment`.  O `category` parâmetro só pode ser utilizado com o `/news` URL. Existem alguns requisitos formais para especificar categorias; consulte a `category` no [parâmetro de consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#query-parameters) documentação. Para disponibilidade, consulte [países e mercados suportados](language-support.md#supported-markets-for-news-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news  
```

**Ponto final de 3:** tópicos de notícias de devolve são popularidade atual nas redes sociais. Quando o `/trendingtopics` opção é incluída, pesquisa do Bing ignora vários outros parâmetros, tais como `freshness` e `?q=""`. Para disponibilidade, consulte [países e mercados suportados](language-support.md#supported-markets-for-news-trending-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/trendingtopics
```

Para obter detalhes sobre os cabeçalhos, parâmetros, códigos de mercado, objetos de resposta, erros, etc., consulte a [pesquisa de notícias do Bing API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference) referência.
## <a name="response-json"></a>Resposta JSON
A resposta a um pedido de pesquisa de notícias inclui resultados como objetos JSON. Os resultados de análise necessita de procedimentos que lidar com elementos de cada tipo. Consulte a [tutorial](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/tutorial-bing-news-search-single-page-app) e [código-fonte](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/tutorial-bing-news-search-single-page-app-source) para obter exemplos.

## <a name="next-steps"></a>Passos Seguintes
O **Bing** ações de pesquisa devolvem resultados de acordo com seu tipo de suporte de APIs. Todos os pontos finais de pesquisa devolvem resultados como objetos de resposta JSON.  Todos os pontos finais suportam consultas que devolvem um idioma específico e/ou localizações, longitude, latitude e radius de pesquisa.

Para obter informações completas sobre os parâmetros suportados por cada ponto de extremidade, consulte as páginas de referência para cada tipo.
Para obter exemplos de solicitações básicas com a API de pesquisa de notícias, consulte [inícios rápidos de pesquisa de notícias do Bing](https://docs.microsoft.com/azure/cognitive-services/bing-news-search).
