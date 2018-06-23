---
title: Atualizar Bing ortográfica verificação API v5 para v7 | Microsoft Docs
description: Identifica as partes da sua aplicação que tem de atualizar para utilizar a versão 7.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 7DC8FB29-4732-47D8-824B-CF2D7AEBA07B
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: article
ms.date: 06/21/2016
ms.author: scottwhi
ms.openlocfilehash: 1b0406641053fac8a4b3f4721728ad3b6c313ba1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351452"
---
# <a name="spell-check-api-upgrade-guide"></a>Guia de atualização de API de verificação ortográfica

Este guia de atualização identifica as alterações entre a versão 5 e a versão 7 da API de verificação ortográfica do Bing. Utilize este guia para ajudar a identificar as partes da sua aplicação que tem de atualizar para utilizar a versão 7.

## <a name="breaking-changes"></a>Alterações interruptivas

### <a name="endpoints"></a>Pontos Finais

- Número de versão do ponto final alterado de v5 para v7. Por exemplo, https://api.cognitive.microsoft.com/bing/\ * \*v 7.0 * * / spellcheck.

### <a name="error-response-objects-and-error-codes"></a>Objetos de resposta de erro e códigos de erro

- Todos os pedidos falhados agora devem incluir um `ErrorResponse` objeto no corpo da resposta.

- Adicionar os seguintes campos ao `Error` objeto.  
  - `subCode`&mdash;Partições o código de erro para registos discretos, se possível
  - `moreDetails`&mdash;Informações adicionais sobre o erro descrita o `message` campo
   

- Substituir os códigos de erro v5 com os seguintes possíveis `code` e `subCode` valores.  
  
|Código|SubCode|Descrição
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>NãoImplementado|Bing devolve ServerError sempre que ocorrer qualquer uma das condições código secundárias. A resposta inclui estes erros se o código de estado HTTP 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Bloqueado|Bing devolve InvalidRequest sempre que qualquer parte do pedido não é válido. Por exemplo, um parâmetro necessário está em falta ou um valor de parâmetro não é válido.<br/><br/>Se o erro é ParameterMissing ou ParameterInvalidValue, o código de estado HTTP é 400.<br/><br/>Se o erro é HttpNotAllowed, o código de estado HTTP 410.
|RateLimitExceeded||Bing devolve RateLimitExceeded sempre que excedem o consultas por segundo (QPS) ou consultas por quota do mês (QPM).<br/><br/>Bing devolve o código de estado HTTP 429 se excedido QPS e 403 se excedido QPM.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing devolve InvalidAuthorization quando Bing não é possível autenticar o autor da chamada. Por exemplo, o `Ocp-Apim-Subscription-Key` cabeçalho está em falta ou a chave de subscrição não é válida.<br/><br/>Redundância ocorre se especificar mais do que um método de autenticação.<br/><br/>Se o erro é InvalidAuthorization, o código de estado HTTP é 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing devolve InsufficientAuthorization quando o autor da chamada não tem permissões para aceder ao recurso. Isto pode ocorrer se a chave de subscrição foi desativada ou não expirou. <br/><br/>Se o erro é InsufficientAuthorization, o código de estado HTTP é 403.

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

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Requisitos de utilização e apresentar](./UseAndDisplayRequirements.md)
