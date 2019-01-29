---
title: Perguntas mais frequentes perguntas sobre a ortografia do Bing a API de verificação
titlesuffix: Azure Cognitive Services
description: Obtenha respostas a perguntas comuns sobre a API de verificação de ortografia do Bing no Azure.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: conceptual
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: 90acc80b19058c2a7eb963f9e423883846519b2c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55164933"
---
# <a name="frequently-asked-questions-about-the-bing-spell-check-api"></a>Perguntas mais frequentes perguntas sobre a ortografia do Bing a API de verificação

 Encontre respostas para perguntas freqüentes sobre conceitos, código e cenários relacionados à API de verificação de ortografia do Bing nos serviços cognitivos da Microsoft no Azure.

## <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-spell-check-api-from-javascript"></a>Como posso obter os cabeçalhos de cliente opcional ao chamar a API de verificação de ortografia do Bing a partir de JavaScript?

Os seguintes cabeçalhos são opcionais, mas recomendamos tratá-los conforme necessário. Esses cabeçalhos ajudam os resultados de retornados mais precisos a API de verificação ortográfica do Bing.

- X-Search-Location
- X-MSEdge-ClientID
- X-MSEdge-ClientIP

No entanto, quando chama a API de verificação de ortografia do Bing do JavaScript, funcionalidades de segurança incorporadas do seu browser podem impedir que os valores desses cabeçalhos a aceder.

Para resolver este problema, pode fazer o pedido de API de verificação de ortografia do Bing através de um proxy CORS. A resposta de um proxy deste tipo tem um `Access-Control-Expose-Headers` cabeçalho que cabeçalhos de resposta de listas de permissões e disponibiliza-os para o JavaScript.

É fácil de instalar um proxy CORS para permitir que o [tutorial de aplicação](tutorials/spellcheck.md) para acessar os cabeçalhos de opcional do cliente. Em primeiro lugar, se ainda não o tiver, [instale Node.js](https://nodejs.org/en/download/). Em seguida, introduza o seguinte comando no prompt de comando.

    npm install -g cors-proxy-server

Em seguida, altere o ponto final de API de verificação de ortografia do Bing no arquivo HTML para:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/

Por fim, inicie o proxy do CORS com o comando seguinte:

    cors-proxy-server

Deixe a janela de comando aberta enquanto utiliza a aplicação de tutorial. Se a janela for fechada, o proxy é interrompido. Na secção de cabeçalhos HTTP expansível abaixo os resultados da pesquisa, pode agora ver o `X-MSEdge-ClientID` cabeçalho (entre outras) e certifique-se de que é o mesmo para cada solicitação.

## <a name="next-steps"></a>Passos Seguintes

É sua pergunta sobre um recurso ou uma funcionalidade em falta? Considere solicitar ou Vote para o mesmo no [web site do UserVoice](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Consulte também

 [StackOverflow: Serviços cognitivos](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
