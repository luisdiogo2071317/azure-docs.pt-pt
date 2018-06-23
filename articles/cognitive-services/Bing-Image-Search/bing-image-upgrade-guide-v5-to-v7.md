---
title: Atualizar Bing imagem pesquisa API v5 para v7 | Microsoft Docs
description: Identifica as partes da sua aplicação que tem de atualizar para utilizar a versão 7.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 7F78B91F-F13B-40A4-B8A7-770FDB793F0F
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: b4d785eafe9ced6fb12e2dac3282dfd286849eb6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351829"
---
# <a name="image-search-api-upgrade-guide"></a>Guia de atualização de API de pesquisa de imagem

Este guia de atualização identifica as alterações entre a versão 5 e a versão 7 da API de pesquisa do Bing imagem. Utilize este guia para ajudar a identificar as partes da sua aplicação que tem de atualizar para utilizar a versão 7.

## <a name="breaking-changes"></a>Alterações interruptivas

### <a name="endpoints"></a>Pontos Finais

- Número de versão do ponto final alterado de v5 para v7. Por exemplo, https://api.cognitive.microsoft.com/bing/\ * \*v7.0**/images/search.

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



### <a name="query-parameters"></a>Parâmetros de consulta

- Mudar o nome de `modulesRequested` parâmetro de consulta [módulos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modules).  
  
- Mudar o nome as anotações para etiquetas. Consulte [módulos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modules) parâmetro para as etiquetas de consulta.  

- Alterar a lista de mercados suportados do valor de filtro ShoppingSources en-US apenas. Consulte [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype).  
 

### <a name="image-insights-changes"></a>Alterações das informações de imagem
  
- Mudar o nome de `annotations` campo [ImagesInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse) para `imageTags`.  
  
- Mudar o nome de `AnnotationModule` object [ImageTagsModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetagsmodule).  
  
- Mudar o nome de `Annotation` object [Tag](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#tag)e remover o `confidence` campo.  
  
- Mudar o nome de `insightsSourcesSummary` campo a [imagem](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) object `insightsMetadata`.  
  
- Mudar o nome de `InsightsSourcesSummary` object [InsightsMetadata](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightsmetadata).  
  
- Adicionar o `https://api.cognitive.microsoft.com/bing/v7.0/images/details` ponto final. Utilize este ponto final às informações de imagem de pedido em vez do / imagens/pesquisa ponto final. Consulte [imagem Insights](./image-insights.md). 
  
- Os seguintes parâmetros de consulta são agora válidos apenas com o `/images/details` ponto final.  
  
    -   [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken)  
    -   [módulos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modules)  
    -   [imgUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imgurl)  
    -   [ficheiro CAB](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cab)  
    -   [CAL](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cal)  
    -   [carro](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#car)  
    -   [cat](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cat)  
    -   [cionar](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#ct)  
  
- Mudar o nome de `ImageInsightsResponse` object [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsights).  
  
- Alterar os tipos de dados dos campos seguintes no [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsights) objeto.  
  
    -   Alterar o tipo do `relatedCollections` campo do `ImageGallery[]` para [RelatedCollectionsModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#relatedcollectionsmodule).  
  
    -   Alterar o tipo do `pagesIncluding` campo do `Image[]` para [ImagesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagesmodule).  
  
    -   Alterar o tipo do `relatedSearches` campo do `Query[]` para [RelatedSearchesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#relatedsearchesmodule).  
  
    -   Alterar o tipo do `recipes` campo do `Recipe[]` para [RecipesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#recipesmodule).  
  
    -   Alterar o tipo do `visuallySimilarImages` campo do `Image[]` para [ImagesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagesmodule).  
  
    -   Alterar o tipo do `visuallySimilarProducts` campo do `ProductSummaryImage[]` para [ImagesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagesmodule).  
  
    -   Remover o `ProductSummaryImage` de objeto e mover os campos relacionados com o produto para o [imagem](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) objeto. O `Image` objeto inclui os campos relacionados com o produto apenas quando a imagem é incluída como parte dos produtos visualmente semelhantes numa resposta de conhecimentos aprofundados imagem.  
  
    -   Alterar o tipo do `recognizedEntityGroups` campo do `RecognizedEntityGroup[]` para [RecognizedEntitiesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#recognizedentitiesmodule).  
  
-   Mudar o nome de `categoryClassification` campo [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse) para `annotations`e alterar o tipo para `AnnotationsModule`.  

### <a name="images-answer"></a>Resposta de imagens

-   Remover os campos displayShoppingSourcesBadges e displayRecipeSourcesBadges de [imagens](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images).  
  
-   Mudar o nome de `nextOffsetAddCount` campo [imagens](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) para `nextOffset`. O forma como utiliza o deslocamento também foi alterada. Anteriormente, defina o [desvio](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offset) consultar parâmetro para o `nextOffsetAddCount` valor e o valor de deslocamento anterior e o número de imagens nos resultados. Agora, defina `offset` para o `nextOffset` valor.  
    
  
## <a name="non-breaking-changes"></a>Não-alterações

### <a name="query-parameters"></a>Parâmetros de consulta
  
- Adicionar transparente uma possível [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype) Filtrar valor. O filtro transparente devolve apenas imagens com um fundo transparente.

- Adicionar quaisquer uma possível [licença](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#license) Filtrar valor. O filtro de qualquer devolve apenas imagens com licença.
  
- Adicionar o [maxFileSize](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#maxfilesize) e [minFileSize](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#minfilesize) parâmetros de consulta. Utilize estes filtros para devolver imagens dentro de um intervalo de tamanhos de ficheiro.  
  
- Adicionar o [maxHeight](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#maxheight), [minHeight](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#minheight), [maxWidth](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#maxwidth), [minWidth](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#minwidth) parâmetros de consulta. Utilize estes filtros para devolver imagens dentro de um intervalo de heights e larguras.  

### <a name="object-changes"></a>Alterações de objetos
  
- Adicionar o `description` e `lastUpdated` campos para o [oferecem](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offer) objeto.  
  
- Adicionar o `name` campo para a [ImageGallery](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagegallery) objeto.  
  
- Adicionar `similarTerms` para o [imagens](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) objeto. Este campo contém uma lista dos termos que são semelhantes na significado a cadeia de consulta do utilizador.  
