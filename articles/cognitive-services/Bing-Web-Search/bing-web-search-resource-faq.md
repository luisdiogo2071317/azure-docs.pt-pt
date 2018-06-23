---
title: Perguntas mais frequentes (FAQ) sobre pesquisa na Web Bing API no Azure | Microsoft Docs
description: Obtenha respostas a questões recorrentes sobre pesquisa API do Microsoft cognitivos serviços Bing Web no Azure.
services: cognitive-services
author: v-jerkin
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 10/06/2017
ms.author: v-jerkin
ms.openlocfilehash: 321f571c48f2231d1ced43848cdefd17adaa1a08
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351673"
---
# <a name="frequently-asked-questions-faq-about-bing-web-search-api-cognitive-services"></a>Perguntas mais frequentes (FAQ) sobre a API de pesquisa do Bing Web (serviços cognitivos)
 
 Encontrar respostas a perguntas mais comuns sobre conceitos, código e cenários relacionados com a API de pesquisa do Bing Web cognitivos nos serviços da Microsoft no Azure.

## <a name="response-headers-in-javascript"></a>Cabeçalhos de resposta em JavaScript

Os cabeçalhos seguintes podem ocorrer nas respostas a partir da API de pesquisa do Bing Web.

|||
|-|-|
|`X-MSEdge-ClientID`|O ID exclusivo que o Bing foi atribuída ao utilizador|
|`BingAPIs-Market`|Mercado que foi utilizado para satisfazer o pedido|
|`BingAPIs-TraceId`|A entrada de registo no servidor de API do Bing para este pedido (para obter suporte)|

É particularmente importante manter o ID de cliente e devolvê-la com os pedidos subsequentes. Ao fazê-lo, a pesquisa será utilize passado contexto classificação os resultados da pesquisa e também fornecer uma experiência de utilizador consistente.

No entanto, quando chamar a API de pesquisa do Bing Web de JavaScript, funcionalidades de segurança incorporada do seu browser (CORS) poderão impedir os valores destes cabeçalhos a aceder.

Para obter acesso aos cabeçalhos, pode efetuar o pedido de API de pesquisa do Bing Web através de um proxy CORS. A resposta de um proxy essa tem um `Access-Control-Expose-Headers` cabeçalho que cabeçalhos de resposta whitelists e disponibiliza-los JavaScript.

É fácil instalar um proxy CORS para permitir a nossa [aplicação tutorial](tutorial-bing-web-search-single-page-app.md) para aceder os cabeçalhos de cliente opcional. Primeiro, se ainda não tiver, [instale o Node.js](https://nodejs.org/en/download/). Em seguida, introduza o seguinte comando numa linha de comandos.

    npm install -g cors-proxy-server

Em seguida, altere o ponto final de API de pesquisa do Bing Web no ficheiro HTML para:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Por fim, inicie o proxy CORS com o seguinte comando:

    cors-proxy-server

Deixe a janela de comandos aberta e, ao utilizar a aplicação tutorial; fechar a janela interrompe o proxy. Na secção de cabeçalhos de HTTP expansível abaixo os resultados da pesquisa, agora, pode ver o `X-MSEdge-ClientID` cabeçalho (entre outras) e certifique-se de que é o mesmo para cada pedido.

## <a name="response-headers-in-production"></a>Cabeçalhos de resposta na produção

A abordagem de proxy CORS descrita na resposta anterior é adequada para o desenvolvimento, teste e de aprendizagem. 

No entanto, num ambiente de produção, deverá alojar um script do lado do servidor no mesmo domínio que a página Web que utiliza a API de pesquisa do Bing Web. Este script, na verdade, deve fazer as chamadas de API mediante pedido de JavaScript a página Web e passar todos os resultados, incluindo os cabeçalhos, de volta para o cliente. Uma vez que os dois recursos (página e scripts) partilharem uma origem, CORS não entrem em play e os cabeçalhos especiais são acessible para o JavaScript na página Web. 

Esta abordagem também protege a chave de API de exposição ao público, uma vez que só tem do script do lado do servidor. O script pode utilizar outro método para se certificar de que o pedido está autorizado.

## <a name="next-steps"></a>Passos Seguintes

É a sua pergunta sobre uma funcionalidade ou funcionalidade em falta? Considere solicitar ou votá-lo nosso [voz do utilizador web site](https://cognitive.uservoice.com/forums/555907-bing-search).

## <a name="see-also"></a>Consulte também

 [Capacidade excedida da pilha: Serviços cognitivos](http://stackoverflow.com/questions/tagged/bing-api)