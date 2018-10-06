---
title: Perguntas mais frequentes (FAQ) - perguntas de sugestão automática do Bing API
titlesuffix: Azure Cognitive Services
description: Obtenha respostas a perguntas comuns sobre a API de sugestão automática do Bing.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: conceptual
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: 84f1b0555922119e9de4addc3d51ac233e7bae65
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/06/2018
ms.locfileid: "48831369"
---
# <a name="frequently-asked-questions-faq-about-bing-autosuggest-api"></a>Perguntas mais frequentes (FAQ) de perguntas sobre o Bing API de sugestão automática
 
 Encontre respostas para perguntas freqüentes sobre conceitos, código e cenários relacionados com a API de sugestão automática para os serviços cognitivos do Azure.

### <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-autosuggest-api-from-javascript"></a>Como posso obter os cabeçalhos de cliente opcional ao chamar a API de sugestão automática do Bing a partir de JavaScript?

Os seguintes cabeçalhos são opcionais, mas recomendamos tratá-los conforme necessário. Esses cabeçalhos ajudam a API de sugestão automática do Bing devolver resultados mais precisos.

- Localização de pesquisa de X
- X-MSEdge-ID de cliente
- X-MSEdge-ClientIP

No entanto, quando chama a API de sugestão automática do Bing do JavaScript, funcionalidades de segurança incorporadas do seu browser podem impedir que os valores desses cabeçalhos a aceder.

Para resolver este problema, pode fazer o pedido de API de sugestão automática do Bing através de um proxy CORS. A resposta de um proxy deste tipo tem um `Access-Control-Expose-Headers` cabeçalho que cabeçalhos de resposta de listas de permissões e disponibiliza-os para o JavaScript.

É fácil de instalar um proxy CORS para permitir que nossos [tutorial de aplicação](tutorials/autosuggest.md) para acessar os cabeçalhos de opcional do cliente. Primeiro, se ainda não o tiver, [instalar node. js](https://nodejs.org/en/download/). Em seguida, introduza o seguinte comando no prompt de comando.

    npm install -g cors-proxy-server

Em seguida, altere o ponto final de API de sugestão automática do Bing no arquivo HTML para:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/Suggestions

Por fim, inicie o proxy CORS com o seguinte comando:

    cors-proxy-server

Deixe a janela de comando aberta e, embora usar o tutorial de aplicação; fechar a janela será interrompido o proxy. Na secção de cabeçalhos HTTP expansível abaixo os resultados da pesquisa, pode agora ver o `X-MSEdge-ClientID` cabeçalho (entre outras) e certifique-se de que é o mesmo para cada solicitação.

## <a name="next-steps"></a>Passos Seguintes

É sua pergunta sobre um recurso ou uma funcionalidade em falta? Considere solicitar ou Vote para ele em nosso [web site do Uservoice](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Consulte também

- [Estouro de pilha: Serviços cognitivos](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
