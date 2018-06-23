---
title: Atualizar o Bing notícias pesquisa API v5 para v7 | Microsoft Docs
description: Identifica as partes da sua aplicação que tem de atualizar para utilizar a versão 7.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 5334C475-4841-4736-A66E-DC1E07CBCEC9
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: baed6f0091ddad40b4802c0fb52dc2ca1818cd03
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351763"
---
# <a name="news-search-api-upgrade-guide"></a>Guia de atualização de API de pesquisa de notícias de última hora

Este guia de atualização identifica as alterações entre a versão 5 e a versão 7 da API de pesquisa do Bing notícias de última hora. Utilize este guia para ajudar a identificar as partes da sua aplicação que tem de atualizar para utilizar a versão 7.

## <a name="breaking-changes"></a>Alterações interruptivas

### <a name="endpoints"></a>Pontos Finais

- Número de versão do ponto final alterado de v5 para v7. Por exemplo, https://api.cognitive.microsoft.com/bing/\ * \*v7.0**/news/search.

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

### <a name="object-changes"></a>Alterações de objetos

- Adicionar o `contractualRules` campo para a [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) objeto. O `contractualRules` campo contém uma lista de regras tem de seguir (por exemplo, a atribuição de artigo). Tem de aplicar a atribuição fornecida `contractualRules` em vez de utilizar `provider`. O artigo inclui `contractualRules` apenas quando o [Web pesquisa API](../bing-web-search/search-the-web.md) resposta contém uma resposta de notícias de última hora.

## <a name="non-breaking-changes"></a>Sem quebra alterações

### <a name="query-parameters"></a>Parâmetros de consulta

- Adicionar produtos como um valor possível que pode definir o [categoria](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#category) parâmetro de consulta. Consulte [categorias por mercados](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#categories-by-market).  
    
- Adicionar o [SortBy](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#sortby) parâmetro de consulta, que devolve tópicos tendências ordenados por data, com o mais recente primeiro.  
  
- Adicionar o [, uma vez que](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#since) parâmetro de consulta, que devolve tópicos tendências que foram detetados pelo Bing nesta ou após o carimbo de época Unix especificado.

### <a name="object-changes"></a>Alterações de objetos

- Adicionar o `mentions` campo para a [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) objeto. O `mentions` campo contém uma lista de entidades (pessoas ou locais) que foram encontradas no artigo.  
  
- Adicionar o `video` campo para a [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) objeto. O `video` campo contém um vídeo que está relacionado com o artigo de notícias de última hora. O vídeo é um \<iframe\> que pode incorporar ou uma miniatura de movimento.   
  
- Adicionar o `sort` campo para a [notícias](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news) objeto. O `sort` campo mostra a sequência de ordenação dos artigos. Por exemplo, os artigos são ordenados por data ou relevância (predefinição).  
  
- Adicionar o [SortValue](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#sortvalue) objeto, que define uma sequência de ordenação. O `isSelected` campo indica se a resposta utilizado a sequência de ordenação. Se **verdadeiro**, a resposta utilizado a sequência de ordenação. Se `isSelected` é **falso**, pode utilizar o URL no `url` campo para pedir uma sequência de ordenação diferente.
