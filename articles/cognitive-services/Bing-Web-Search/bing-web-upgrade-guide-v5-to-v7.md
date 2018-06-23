---
title: Atualizar a partir do Bing Web pesquisa API v5 para v7 | Microsoft Docs
description: Identifica as partes da sua aplicação que tem de atualizar para utilizar a versão 7.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: E8827BEB-4379-47CE-B67B-6C81AD7DAEB1
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 01/15/2017
ms.author: scottwhi
ms.openlocfilehash: 155297f230c0ee02d6fa49d6d35eb24d9941f29b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354530"
---
# <a name="web-search-api-upgrade-guide"></a>Guia de atualização de pesquisa API Web

Este guia de atualização identifica as alterações entre a versão 5 e a versão 7 da API de pesquisa de Web Bing. Utilize este guia para ajudar a identificar as partes da sua aplicação que tem de atualizar para utilizar a versão 7.

## <a name="breaking-changes"></a>Alterações interruptivas

### <a name="endpoints"></a>Pontos Finais

- Número de versão do ponto final alterado de v5 para v7. Por exemplo, https:\/\/api.cognitive.microsoft.com/bing/**v 7.0**  /procurar.

### <a name="error-response-objects-and-error-codes"></a>Objetos de resposta de erro e códigos de erro

- Todos os pedidos falhados agora devem incluir um `ErrorResponse` objeto no corpo da resposta.

- Adicionar os seguintes campos ao `Error` objeto.  
  - `subCode`&mdash;Partições o código de erro para registos discretos, se possível
  - `moreDetails`&mdash;Informações adicionais sobre o erro descrita o `message` campo
   

- Substituir os códigos de erro v5 com os seguintes possíveis `code` e `subCode` valores.

|Código|SubCode|Descrição
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>NãoImplementado|Bing devolve ServerError sempre que ocorrer qualquer uma das condições código secundárias. A resposta irá incluir estes erros se o código de estado HTTP 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Bloqueado|Bing devolve InvalidRequest sempre que qualquer parte do pedido não é válido. Por exemplo, um parâmetro necessário está em falta ou um valor de parâmetro não é válido.<br/><br/>Se o erro é ParameterMissing ou ParameterInvalidValue, o código de estado HTTP é 400.<br/><br/>Se o erro é HttpNotAllowed, o código de estado HTTP 410.
|RateLimitExceeded||Bing devolve RateLimitExceeded sempre que excedem o consultas por segundo (QPS) ou consultas por quota do mês (QPM).<br/><br/>Bing devolve o código de estado HTTP 429 se excedido QPS e 403 se excedido QPM.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing devolve InvalidAuthorization quando Bing não é possível autenticar o autor da chamada. Por exemplo, o `Ocp-Apim-Subscription-Key` cabeçalho está em falta ou a chave de subscrição não é válida.<br/><br/>Redundância ocorre se especificar mais do que um método de autenticação.<br/><br/>Se o erro é InvalidAuthorization, o código de estado HTTP é 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing devolve InsufficientAuthorization quando o autor da chamada não tem permissões para aceder ao recurso. Este erro pode ocorrer se a chave de subscrição foi desativada ou não expirou. <br/><br/>Se o erro é InsufficientAuthorization, o código de estado HTTP é 403.

- O seguinte mapeia os códigos de erro anterior para os novos códigos de erro. Se tiver criado uma dependência no v5 códigos de erro, atualize o código em conformidade.

|Código de versão 5|Versão 7 code.subCode
|-|-
|RequestParameterMissing|InvalidRequest.ParameterMissing
RequestParameterInvalidValue|InvalidRequest.ParameterInvalidValue
ResourceAccessDenied|InsufficientAuthorization
ExceededVolume|RateLimitExceeded
ExceededQpsLimit|RateLimitExceeded
Desativado|InsufficientAuthorization.AuthorizationDisabled
UnexpectedError|ServerError.UnexpectedError
DataSourceErrors|ServerError.ResourceError
AuthorizationMissing|InvalidAuthorization.AuthorizationMissing
HttpNotAllowed|InvalidRequest.HttpNotAllowed
UserAgentMissing|InvalidRequest.ParameterMissing
NãoImplementado|ServerError.NotImplemented
InvalidAuthorization|InvalidAuthorization
InvalidAuthorizationMethod|InvalidAuthorization
MultipleAuthorizationMethod|InvalidAuthorization.AuthorizationRedundancy
ExpiredAuthorizationToken|InsufficientAuthorization.AuthorizationExpired
InsufficientScope|InsufficientAuthorization
Bloqueado|InvalidRequest.Blocked


## <a name="non-breaking-changes"></a>Não-alterações  

### <a name="headers"></a>Cabeçalhos

- Adicionar o opcional [Pragma](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#pragma) cabeçalho do pedido. Por predefinição, o Bing devolve os conteúdos em cache, se disponível. Para impedir que o Bing devolver o conteúdo em cache, defina o cabeçalho de Pragma para no-cache (por exemplo, Pragma: não cache).

### <a name="query-parameters"></a>Parâmetros de consulta

- Adicionar o [answerCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#answercount) parâmetro de consulta. Utilize este parâmetro para especificar o número de respostas pretendidas a resposta a incluir. As respostas são escolhidas com base na classificação. Por exemplo, se definir este parâmetro para três (3), a resposta inclui as respostas de classificados três principais.  
  
- Adicionar o [promover](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#promote) parâmetro de consulta. Utilize este parâmetro juntamente com `answerCount` explicitamente incluir um ou mais tipos de resposta, independentemente da respetiva classificação. Por exemplo, para promover vídeos e imagens na resposta, iria definir promover a *vídeos, imagens*. A lista de respostas que pretende promover não contagem contra o `answerCount` limite. Por exemplo, se `answerCount` é 2 e `promote` está definido como *vídeos, imagens*, a resposta pode incluir páginas Web, notícias de última hora, vídeos e imagens.

### <a name="object-changes"></a>Alterações de objetos

- Adicionar o `someResultsRemoved` campo para a [WebAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#webanswer) objeto. O campo contém um valor booleano que indica se a resposta excluídos alguns resultados a resposta da web.  

