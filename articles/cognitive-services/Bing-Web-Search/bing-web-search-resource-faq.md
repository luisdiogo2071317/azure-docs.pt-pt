---
title: Perguntas mais frequentes (FAQ) - API de pesquisa Web Bing
titleSuffix: Azure Cognitive Services
description: Obtenha respostas a perguntas comuns sobre como utilizar a API de pesquisa Web Bing.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: troubleshooting
ms.date: 10/06/2017
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 9098ddc780e6adefa8430fe601e3f4d06634972a
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2018
ms.locfileid: "53252988"
---
# <a name="frequently-asked-questions-faq"></a>Perguntas Mais Frequentes (FAQ)

 Encontre respostas para perguntas freqüentes sobre conceitos, código e cenários relacionados à API de pesquisa Web do Bing nos serviços cognitivos da Microsoft no Azure.

## <a name="response-headers-in-javascript"></a>Cabeçalhos de resposta em JavaScript

Os cabeçalhos seguintes podem ocorrer nas respostas da API de pesquisa Web do Bing.

|||
|-|-|
|`X-MSEdge-ClientID`|O ID exclusivo que Bing atribuída ao utilizador|
|`BingAPIs-Market`|O mercado que foi utilizado para satisfazer o pedido|
|`BingAPIs-TraceId`|A entrada de log no servidor de API do Bing para este pedido (para obter suporte)|

É particularmente importante manter o ID de cliente e retorná-lo com as solicitações subseqüentes. Ao fazê-lo, a pesquisa irá utilizar em contexto na classificação os resultados da pesquisa e também proporcionar uma experiência de usuário consistente.

No entanto, quando chama a API de pesquisa Web Bing a partir de JavaScript, funcionalidades de segurança incorporadas do seu browser (CORS) podem impedir que os valores desses cabeçalhos a aceder.

Para obter acesso aos cabeçalhos, pode fazer o pedido de API de pesquisa Web Bing através de um proxy CORS. A resposta de um proxy deste tipo inclui um cabeçalho `Access-Control-Expose-Headers`, que adiciona os cabeçalhos das respostas à lista de permissões e os disponibiliza para o JavaScript.

É fácil de instalar um proxy CORS para permitir que nossos [tutorial de aplicação](tutorial-bing-web-search-single-page-app.md) para acessar os cabeçalhos de opcional do cliente. Em primeiro lugar, se ainda não o tiver, [instale Node.js](https://nodejs.org/en/download/). Em seguida, introduza o seguinte comando no prompt de comando.

    npm install -g cors-proxy-server

Em seguida, altere o ponto final de API de pesquisa Web Bing no arquivo HTML para:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Por fim, inicie o proxy do CORS com o comando seguinte:

    cors-proxy-server

Deixe a janela de comando aberta enquanto utiliza a aplicação de tutorial. Se a janela for fechada, o proxy é interrompido. Na secção Cabeçalhos HTTP expansíveis, abaixo dos resultados da pesquisa, pode agora ver o cabeçalho `X-MSEdge-ClientID` (entre outros) e confirmar se é o mesmo em todos os pedidos.

## <a name="response-headers-in-production"></a>Cabeçalhos de resposta em produção

A abordagem de proxy CORS descrita na resposta anterior é adequada para o desenvolvimento, teste e aprendizado.

No entanto, num ambiente de produção, deve hospedar um script do lado do servidor no mesmo domínio que a página da Web que utiliza a API de pesquisa Web Bing. Este script, na verdade, deve fazer as chamadas à API mediante a solicitação de página da Web JavaScript e passar todos os resultados, incluindo os cabeçalhos, volta ao cliente. Como os dois recursos (página e script) compartilham uma origem, CORS não entram em cena e os cabeçalhos especiais estão acessíveis para o JavaScript na página da Web.

Essa abordagem também protege a chave de API de exposição ao público, uma vez que somente o script do lado do servidor precise dele. O script pode usar outro método para certificar-se de que a solicitação é autorizada.

## <a name="next-steps"></a>Passos Seguintes

É sua pergunta sobre um recurso ou uma funcionalidade em falta? Considere solicitar ou Vote para ele em nosso [web site do Uservoice](https://cognitive.uservoice.com/forums/555907-bing-search).

## <a name="see-also"></a>Consulte também

 [Estouro de pilha: Serviços cognitivos](http://stackoverflow.com/questions/tagged/bing-api)
