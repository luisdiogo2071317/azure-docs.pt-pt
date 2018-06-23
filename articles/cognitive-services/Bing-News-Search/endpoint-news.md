---
title: Pontos finais de pesquisa do Bing notícias | Microsoft Docs
description: Resumo do ponto de final do Gestor de pesquisa de notícias API.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-gedod
ms.openlocfilehash: b8ae99698926a818bf22b0b4027af8610413aaa6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351511"
---
# <a name="news-search-endpoints"></a>Pontos finais de pesquisa de notícias de última hora
O **API de pesquisa de notícias** devolve notícias imagens de artigos, páginas Web, vídeos, e [entidades](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Entidades contém informações de resumo sobre uma pessoa, local ou tópico. 
## <a name="endpoints"></a>Pontos Finais
Para obter resultados de pesquisa de notícias utilizando a API do Bing, enviar uma `GET` pedido a um dos seguintes pontos finais. Os parâmetros de URL e cabeçalhos definem ainda mais as especificações.

**Ponto final 1:** devolve os itens de notícias de última hora com base na consulta de pesquisa do utilizador. Se a consulta de pesquisa estiver vazia, a chamada devolve os artigos de notícias superior. A consulta `?q=""` opção também pode ser utilizada com o `/news` URL. Para disponibilidade, consulte [suportados países e mercados](supported-countries-markets.md#supported-markets-for-news-search-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search 
``` 

**Ponto final 2:** devolve os itens principais notícias de última hora por categoria. Pode pedir especificamente o negócio superior, desporto ou entretenimento artigos utilizando `category=business`, `category=sports`, ou `category=entertainment`.  O `category` parâmetro só pode ser utilizado com o `/news` URL. Existem alguns requisitos formais para especificar categorias; consulte `category` no [parâmetro de consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#query-parameters) documentação. Para disponibilidade, consulte [suportados países e mercados](supported-countries-markets.md#supported-markets-for-news-endpoint). 
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news  
```

**Ponto final 3:** tópicos de notícias devolve que estão atualmente tendências em redes sociais. Quando o `/trendingtopics` opção está incluída, pesquisa do Bing ignora vários outros parâmetros, tais como `freshness` e `?q=""`. Para disponibilidade, consulte [suportados países e mercados](supported-countries-markets.md#supported-markets-for-news-trending-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/trendingtopics 
``` 

Para obter mais informações sobre cabeçalhos, parâmetros, códigos de mercado, objetos de resposta, erros, etc., consulte o [pesquisa do Bing notícias API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference) referência.
## <a name="response-json"></a>Resposta JSON
A resposta a um pedido de pesquisa de notícias inclui os resultados como objetos JSON. Os resultados de análise necessita de procedimentos que processar os elementos de cada tipo. Consulte o [tutorial](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/tutorial-bing-news-search-single-page-app) e [código fonte](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/tutorial-bing-news-search-single-page-app-source) para obter exemplos.

## <a name="next-steps"></a>Passos Seguintes
O **Bing** ações de pesquisa devolvem resultados de acordo com o respetivo tipo de suporte de APIs. Todos os pontos finais de pesquisa devolvem resultados como objetos de resposta JSON.  Todos os pontos finais suportam consultas que devolvem um idioma específico e/ou a localização por radius de pesquisa, latitude e longitude.

Para obter informações completas acerca dos parâmetros suportados por cada ponto final, consulte as páginas de referência para cada tipo.
Para obter exemplos de pedidos básicos utilizando a API de pesquisa de notícias, consulte [Bing notícias pesquisa rápida-inicia](https://docs.microsoft.com/azure/cognitive-services/bing-news-search).