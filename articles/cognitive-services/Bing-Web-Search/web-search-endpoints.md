---
title: Ponto final de pesquisa de Web | Microsoft Docs
description: Resumo do ponto final da API Web pesquisa.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-gedod
ms.openlocfilehash: 72fbe1a0eb4379ad032e649f7299e37a0214adfb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351536"
---
# <a name="web-search-endpoint"></a>Ponto final de pesquisa de Web
O **Web pesquisa API** Devolve páginas Web, notícias de última hora, imagens, vídeos, e [entidades](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Entidades contém informações de resumo sobre uma pessoa, local ou tópico.
## <a name="endpoint"></a>Ponto Final
Para obter os resultados da pesquisa Web utilizando a API do Bing, enviar uma `GET` pedido para o seguinte ponto final. Os parâmetros de URL e cabeçalhos definem ainda mais as especificações.

**Ponto final**: resultados devolve Web que são relevantes para o utilizador pesquisar consulta definida pelo `?q=""`.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/search
```
Ponto final: Para obter mais informações sobre cabeçalhos, parâmetros, mercado códigos, objetos de resposta, erros, etc., consulte o [Web API do Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) referência.

## <a name="response-json"></a>Resposta JSON
A resposta a um pedido de pesquisa Web inclui todos os resultados como objetos JSON. Ao analisar o resultado necessita de procedimentos que processar os elementos de cada tipo. Consulte o [tutorial](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app) e [código fonte](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app-source) para obter exemplos.

## <a name="next-steps"></a>Passos Seguintes
O **Bing** ações de pesquisa devolvem resultados de acordo com o respetivo tipo de suporte de APIs. Todos os pontos finais de pesquisa devolvem resultados como objetos de resposta JSON.  Todos os pontos finais suportam consultas que devolvem um idioma específico e/ou a localização por radius de pesquisa, latitude e longitude.

Para obter informações completas acerca dos parâmetros suportados por cada ponto final, consulte as páginas de referência para cada tipo.
Para obter exemplos de pedidos básicos utilizando a pesquisa de Web API, consulte [procurar Web rápida-é iniciado o](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/search-the-web).