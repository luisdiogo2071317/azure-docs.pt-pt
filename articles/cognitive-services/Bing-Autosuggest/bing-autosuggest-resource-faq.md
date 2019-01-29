---
title: Perguntas mais frequentes (FAQ) - perguntas de sugestão automática do Bing API
titlesuffix: Azure Cognitive Services
description: Obtenha respostas a perguntas comuns sobre a API de sugestão automática do Bing.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: conceptual
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: b0ec10bbf03e8a8d005eece4b6496b74b2943233
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55173263"
---
# <a name="frequently-asked-questions-faq-about-bing-autosuggest-api"></a>Perguntas mais frequentes (FAQ) de perguntas sobre o Bing API de sugestão automática
 
 Encontre respostas para perguntas freqüentes sobre conceitos, código e cenários relacionados com a API de sugestão automática para os serviços cognitivos do Azure.

### <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-autosuggest-api-from-javascript"></a>Como posso obter os cabeçalhos de cliente opcional ao chamar a API de sugestão automática do Bing a partir de JavaScript?

Os seguintes cabeçalhos são opcionais, mas recomendamos tratá-los conforme necessário. Esses cabeçalhos ajudam a API de sugestão automática do Bing devolver resultados mais precisos.

- X-Search-Location
- X-MSEdge-ClientID
- X-MSEdge-ClientIP

No entanto, quando chama a API de sugestão automática do Bing do JavaScript, funcionalidades de segurança incorporadas do seu browser podem impedir que os valores desses cabeçalhos a aceder.

Para resolver este problema, pode fazer o pedido de API de sugestão automática do Bing através de um proxy CORS. A resposta de um proxy deste tipo tem um `Access-Control-Expose-Headers` cabeçalho que cabeçalhos de resposta de listas de permissões e disponibiliza-os para o JavaScript.

É fácil de instalar um proxy CORS para permitir que nossos [tutorial de aplicação](tutorials/autosuggest.md) para acessar os cabeçalhos de opcional do cliente. Em primeiro lugar, se ainda não o tiver, [instale Node.js](https://nodejs.org/en/download/). Em seguida, introduza o seguinte comando no prompt de comando.

    npm install -g cors-proxy-server

Em seguida, altere o ponto final de API de sugestão automática do Bing no arquivo HTML para:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/Suggestions

Por fim, inicie o proxy do CORS com o comando seguinte:

    cors-proxy-server

Deixe a janela de comando aberta enquanto utiliza a aplicação de tutorial. Se a janela for fechada, o proxy é interrompido. Na secção Cabeçalhos HTTP expansíveis, abaixo dos resultados da pesquisa, pode agora ver o cabeçalho `X-MSEdge-ClientID` (entre outros) e confirmar se é o mesmo em todos os pedidos.

## <a name="next-steps"></a>Passos Seguintes

É sua pergunta sobre um recurso ou uma funcionalidade em falta? Considere solicitar ou Vote para ele em nosso [web site do Uservoice](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Consulte também

- [Estouro de pilha: Serviços cognitivos](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
