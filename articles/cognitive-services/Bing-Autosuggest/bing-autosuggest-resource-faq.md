---
title: Perguntas mais frequentes (FAQ) dúvidas Azure API de sugestão automática | Microsoft Docs
description: Obtenha respostas a questões recorrentes sobre o Azure cognitivos API dos serviços de sugestão automática no Azure.
services: cognitive-services
author: HeidiSteen
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: article
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: 00b91728bcfec52ff30697f080d5c2619bab79a8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354475"
---
# <a name="frequently-asked-questions-faq-about-autosuggest-api-cognitive-services"></a>Perguntas mais frequentes perguntas (FAQ) sobre API (serviços cognitivos) de sugestão automática
 
 Encontrar respostas a perguntas mais comuns sobre conceitos, código e cenários relacionados com a API de sugestão automática para serviços cognitivos do Azure.

### <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-autosuggest-api-from-javascript"></a>Como obter os cabeçalhos de cliente opcional ao chamar a API de sugestão automática do Bing de JavaScript?

Os seguintes cabeçalhos são opcionais, mas recomendamos que tratá-los conforme necessário. Estes cabeçalhos ajudam a API de sugestão automática do Bing devolver resultados mais exatos.

- Localização de pesquisa X
- X-MSEdge-ClientID
- X-MSEdge-ClientIP

No entanto, quando chamar a API de sugestão automática do Bing do JavaScript, funcionalidades de segurança incorporada do seu browser poderão impedir os valores destes cabeçalhos a aceder.

Para resolver este problema, pode efetuar o pedido de API de sugestão automática do Bing através de um proxy CORS. A resposta de um proxy essa tem um `Access-Control-Expose-Headers` cabeçalho que cabeçalhos de resposta whitelists e disponibiliza-los JavaScript.

É fácil instalar um proxy CORS para permitir a nossa [aplicação tutorial](tutorials/autosuggest.md) para aceder os cabeçalhos de cliente opcional. Primeiro, se ainda não tiver, [instale o Node.js](https://nodejs.org/en/download/). Em seguida, introduza o seguinte comando numa linha de comandos.

    npm install -g cors-proxy-server

Em seguida, altere o ponto final de API de sugestão automática do Bing no ficheiro HTML para:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/Suggestions

Por fim, inicie o proxy CORS com o seguinte comando:

    cors-proxy-server

Deixe a janela de comandos aberta e, ao utilizar a aplicação tutorial; fechar a janela interrompe o proxy. Na secção de cabeçalhos de HTTP expansível abaixo os resultados da pesquisa, agora, pode ver o `X-MSEdge-ClientID` cabeçalho (entre outras) e certifique-se de que é o mesmo para cada pedido.

## <a name="next-steps"></a>Passos Seguintes

É a sua pergunta sobre uma funcionalidade ou funcionalidade em falta? Considere solicitar ou votá-lo nosso [voz do utilizador web site](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Consulte também

- [Capacidade excedida da pilha: Serviços cognitivos](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
