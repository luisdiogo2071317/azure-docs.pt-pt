---
title: Perguntas mais frequentes perguntas sobre a ortografia do Bing a API de verificação
titlesuffix: Azure Cognitive Services
description: Obtenha respostas a perguntas comuns sobre a API de verificação de ortografia do Bing no Azure.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: conceptual
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: e6662ffcbab9ea274a67bc4437ca1600f1625ff1
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801510"
---
# <a name="frequently-asked-questions-about-the-bing-spell-check-api"></a>Perguntas mais frequentes perguntas sobre a ortografia do Bing a API de verificação

 Encontre respostas para perguntas freqüentes sobre conceitos, código e cenários relacionados à API de verificação de ortografia do Bing nos serviços cognitivos da Microsoft no Azure.

## <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-spell-check-api-from-javascript"></a>Como posso obter os cabeçalhos de cliente opcional ao chamar a API de verificação de ortografia do Bing a partir de JavaScript?

Os seguintes cabeçalhos são opcionais, mas recomendamos tratá-los conforme necessário. Esses cabeçalhos ajudam os resultados de retornados mais precisos a API de verificação ortográfica do Bing.

- Localização de pesquisa de X
- X-MSEdge-ID de cliente
- X-MSEdge-ClientIP

No entanto, quando chama a API de verificação de ortografia do Bing do JavaScript, funcionalidades de segurança incorporadas do seu browser podem impedir que os valores desses cabeçalhos a aceder.

Para resolver este problema, pode fazer o pedido de API de verificação de ortografia do Bing através de um proxy CORS. A resposta de um proxy deste tipo tem um `Access-Control-Expose-Headers` cabeçalho que cabeçalhos de resposta de listas de permissões e disponibiliza-os para o JavaScript.

É fácil de instalar um proxy CORS para permitir que o [tutorial de aplicação](tutorials/spellcheck.md) para acessar os cabeçalhos de opcional do cliente. Primeiro, se ainda não o tiver, [instalar node. js](https://nodejs.org/en/download/). Em seguida, introduza o seguinte comando no prompt de comando.

    npm install -g cors-proxy-server

Em seguida, altere o ponto final de API de verificação de ortografia do Bing no arquivo HTML para:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/

Por fim, inicie o proxy CORS com o seguinte comando:

    cors-proxy-server

Deixe a janela de comando aberta e, embora usar o tutorial de aplicação; fechar a janela será interrompido o proxy. Na secção de cabeçalhos HTTP expansível abaixo os resultados da pesquisa, pode agora ver o `X-MSEdge-ClientID` cabeçalho (entre outras) e certifique-se de que é o mesmo para cada solicitação.

## <a name="next-steps"></a>Passos Seguintes

É sua pergunta sobre um recurso ou uma funcionalidade em falta? Considere solicitar ou Vote para o mesmo no [web site do UserVoice](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Consulte também

 [StackOverflow: Serviços cognitivos](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
