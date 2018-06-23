---
title: Ponto final de pesquisa personalizada | Microsoft Docs
description: Resumo do ponto final de API de pesquisa personalizada.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: 8d9851f3687a783f52a80a8dffcf2580d4710551
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352874"
---
# <a name="custom-search"></a>Pesquisa Personalizada
Pesquisa do Bing personalizada permite-lhe criar experiências de pesquisa personalizáveis para tópicos que mais lhe interessam. Os seus utilizadores verão os resultados da pesquisa adaptados para o conteúdo que mais importantes para si em vez de ter a página percorrer os resultados de pesquisa que possuem conteúdo irrelevantes.

## <a name="custom-search-endpoint"></a>Ponto final de pesquisa personalizada
Para obter resultados utilizando a API de pesquisa do Bing personalizada, enviar uma `GET` pedido para o seguinte ponto final. Utilize os parâmetros de URL e cabeçalhos para definir mais especificações.

Ponto final: Devolve procura sugestões como resultados JSON que são relevantes para a entrada do utilizador definida por `?q=""`.
```  
 GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search  
```

Para obter exemplos que descrevem como configurar origens de pesquisa personalizada, consulte o [tutorial](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/tutorials/custom-search-web-page). Para obter mais informações sobre cabeçalhos, parâmetros, códigos de mercado, objetos de resposta, erros, etc., consulte o [v7 de API de pesquisa do Bing personalizada](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference) referência.

## <a name="custom-search-response-json"></a>Resposta de pesquisa personalizada JSON
Um pedido de pesquisa personalizada devolve resultados como objetos JSON, consulte [objetos de resposta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#response-objects). 

## <a name="custom-autosuggest"></a>Sugestão automática do personalizada
A API de sugestão automática personalizada permite-lhe enviar um termo de consulta de pesquisa parcial para o Bing e voltar a uma lista de consultas sugeridas que pode configurar. Com sugestão automática do personalizada, adicione sugestões devolvidos pela API e, opcionalmente, especifique se inclui sugestões gerados ao Bing.

## <a name="custom-autosuggest-endpoint"></a>Ponto final de sugestão automática personalizada
Para pedir sugestões de consulta personalizada, envie um pedido GET para:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions
```  

Para obter informações sobre como definir sugestões personalizadas, consulte [definir sugestões de pesquisa personalizada](define-custom-suggestions.md).

## <a name="custom-image-search"></a>Pesquisa de imagem personalizada
A API de pesquisa de imagem personalizada permite-lhe enviar uma consulta de pesquisa para o Bing e voltar a uma lista de imagens relevantes da sua instância de pesquisa personalizada.

## <a name="custom-image-search-endpoint"></a>Ponto final de pesquisa de imagem personalizada
Para pedir as imagens da sua instância de pesquisa personalizada, envie um pedido GET para o seguinte URL:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/images/search
```

Para obter informações sobre como configurar uma instância de pesquisa personalizada, consulte [configurar a sua experiência de pesquisa personalizada](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/define-your-custom-view).

## <a name="next-steps"></a>Passos Seguintes
O **Bing** ações de pesquisa devolvem resultados de acordo com o respetivo tipo de suporte de APIs. Todos os pontos finais de pesquisa devolvem resultados como objetos de resposta JSON.  Todos os pontos finais suportam consultas que devolvem um idioma específico e/ou a localização por radius de pesquisa, latitude e longitude.

Para obter informações completas acerca dos parâmetros suportados por cada ponto final, consulte as páginas de referência para cada tipo.
Para obter exemplos de pedidos básicos utilizando a API de pesquisa personalizada, consulte [personalizada pesquisa rápida-é iniciado](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/)