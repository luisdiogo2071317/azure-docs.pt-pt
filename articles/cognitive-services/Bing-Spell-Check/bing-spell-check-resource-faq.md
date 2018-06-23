---
title: Perguntas mais frequentes perguntas sobre o Bing ortográfica verificação API - serviços cognitivos do Azure | Microsoft Docs
description: Obtenha respostas a questões recorrentes sobre a API de verificação ortográfica do Bing no Azure.
services: cognitive-services
author: HeidiSteen
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: conceptual
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: 87b1f3ed3e0aaa9f3c3c804dc9eac3ee60b4a565
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354949"
---
# <a name="frequently-asked-questions-about-the-bing-spell-check-api"></a>Perguntas mais frequentes perguntas sobre a ortográfica do Bing verifique API

 Encontrar respostas a perguntas mais comuns sobre conceitos, código e cenários relacionados com a API de verificação ortográfica do Bing cognitivos nos serviços da Microsoft no Azure.

## <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-spell-check-api-from-javascript"></a>Como obter os cabeçalhos de cliente opcional ao chamar a API de verificação ortográfica do Bing de JavaScript?

Os seguintes cabeçalhos são opcionais, mas recomendamos que tratá-los conforme necessário. Estes cabeçalhos ajudam os resultados de retorno mais exatos a API de verificação ortográfica do Bing.

- Localização de pesquisa X
- X-MSEdge-ClientID
- X-MSEdge-ClientIP

No entanto, quando chamar a API de verificação ortográfica do Bing do JavaScript, funcionalidades de segurança incorporada do seu browser poderão impedir os valores destes cabeçalhos a aceder.

Para resolver este problema, pode efetuar o pedido de API de verificação ortográfica do Bing através de um proxy CORS. A resposta de um proxy essa tem um `Access-Control-Expose-Headers` cabeçalho que cabeçalhos de resposta whitelists e disponibiliza-los JavaScript.

É fácil instalar um proxy CORS para permitir que o [aplicação tutorial](tutorials/spellcheck.md) para aceder os cabeçalhos de cliente opcional. Primeiro, se ainda não tiver, [instale o Node.js](https://nodejs.org/en/download/). Em seguida, introduza o seguinte comando numa linha de comandos.

    npm install -g cors-proxy-server

Em seguida, altere o ponto final de API de verificação ortográfica do Bing no ficheiro HTML para:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/

Por fim, inicie o proxy CORS com o seguinte comando:

    cors-proxy-server

Deixe a janela de comandos aberta e, ao utilizar a aplicação tutorial; fechar a janela interrompe o proxy. Na secção de cabeçalhos de HTTP expansível abaixo os resultados da pesquisa, agora, pode ver o `X-MSEdge-ClientID` cabeçalho (entre outras) e certifique-se de que é o mesmo para cada pedido.

## <a name="next-steps"></a>Passos Seguintes

É a sua pergunta sobre uma funcionalidade ou funcionalidade em falta? Considere solicitar ou votá-lo [UserVoice web site](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Consulte também

 [StackOverflow: Serviços cognitivos](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
