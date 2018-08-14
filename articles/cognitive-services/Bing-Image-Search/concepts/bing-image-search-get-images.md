---
title: Obter imagens da web com a API de pesquisa de imagens do Bing | Documentos da Microsoft
description: Use a API de pesquisa de imagens do Bing para pesquisar e obter imagens relevantes da web.
services: cognitive-services
author: aahill
manager: cgronlun
ms.assetid: AB1B9898-C94A-4B59-91A1-8623C94BA3D4
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: conceptual
ms.date: 8/8/2018
ms.author: aahi
ms.openlocfilehash: c379cc2dfbe53f83e4d79500cd947879407c8d55
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/13/2018
ms.locfileid: "40082614"
---
# <a name="getting-images-from-the-web-with-the-bing-image-search-api"></a>Obter imagens da web com a API de pesquisa de imagens do Bing

Utilizar a API de REST de pesquisa de imagens Bing, pode obter imagens da web que estão relacionados com o termo de pesquisa do usuário ao enviar o pedido GET seguinte:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

> [!IMPORTANT]
> * Todos os pedidos devem ser feitos a partir de um servidor e não a partir de um cliente.
> * Se for a primeira vez que chamar qualquer uma das APIs de pesquisa da Bing, não inclua o cabeçalho de ID de cliente. Inclua o ID de cliente apenas se já tenha chamado uma API do Bing que devolveu um ID de cliente para o utilizador e a combinação de dispositivo.
> * Imagens devem ser exibidas na ordem disposta na resposta.

## <a name="getting-images-from-a-specific-web-domain"></a>Obter imagens de um domínio web específico

Para obter imagens de um domínio específico, utilize o operador de consulta [site:](http://msdn.microsoft.com/library/ff795613.aspx).

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

> [!NOTE]
> As respostas para consultas com o `site:` operador pode incluir conteúdo para adultos, independentemente do [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#safesearch) definição. Utilize apenas `site:` se está atento o conteúdo no domínio.

O exemplo seguinte mostra como obter imagens pequenas a partir de ContosoSailing.com que o Bing descobriu na semana anterior.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="filtering-images"></a>Filtrar imagens

 Por predefinição, a API de pesquisa de imagens irá devolver todas as imagens que são relevantes para a consulta. Se pretender filtrar as imagens do que Bing devolve (por exemplo, para devolver apenas as imagens com um plano de fundo transparant ou tamanho específico), pode utilizar os seguintes parâmetros de consulta:

* [aspect](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#aspect) — Filtrar imagens por proporção (por exemplo, imagens padrão ou em ecrã panorâmico)
* [color](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#color) — Filtrar imagens por cor dominante ou preto e branco
* [freshness](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#freshness) — Filtrar imagens por antiguidade (por exemplo, imagens descobertas pelo Bing na semana anterior)
* [height](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#height), [width](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#width) — Filtrar imagens por largura e altura
* [imageContent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagecontent) — Filtrar imagens por conteúdo (por exemplo, imagens que mostram apenas o rosto de uma pessoa)
* [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype) — Filtrar imagens por tipo (por exemplo, ClipArt, GIFs animados ou fundos transparentes)
* [license](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#license) — Filtrar imagens pelo tipo de licença associada ao site
* [size](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#size) — Filtrar imagens por tamanho, como, por exemplo, imagens pequenas até 200x200 pixéis

Para obter imagens de um domínio específico, utilize o operador de consulta [site:](http://msdn.microsoft.com/library/ff795613.aspx).

    > [!NOTE]
    > Responses to queries using the `site:` operator may include adult content regardless of the [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#safesearch) setting. Only use `site:` if you are aware of the content on the domain.

O exemplo seguinte mostra como obter imagens pequenas a partir de ContosoSailing.com que o Bing descobriu na semana anterior.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="bing-image-search-response-format"></a>Formato de resposta de pesquisa de imagens Bing

A mensagem de resposta do Bing contém um [imagens](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) resposta que contém uma lista de imagens que os serviços cognitivos do Azure identificado como sendo relevantes para a consulta. Cada [imagem](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) objeto na lista inclui as seguintes informações sobre a imagem: o URL, seu tamanho, suas dimensões, seu formato de codificação, um URL para uma miniatura da imagem e dimensões a miniatura.

```json
{
    "name": "Rich Passage Sailing Dinghy",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=73118C8B4E3...",
    "thumbnailUrl": "https:\/\/tse1.mm.bing.net\/th?id=OIP.GNarK7m...",
    "datePublished": "2011-10-29T11:26:00",
    "contentUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3...",
    "hostPageUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3687...",
    "contentSize": "79239 B",
    "encodingFormat": "jpeg",
    "hostPageDisplayUrl": "en.contoso.org\/wiki\/File:Rich_Passage...",
    "width": 526,
    "height": 688,
    "thumbnail": {
        "width": 229,
        "height": 300
    },
    "imageInsightsToken": "ccid_GNarK7ma*mid_CCF85447ADA6...",
    "insightsSourcesSummary": {
        "shoppingSourcesCount": 0,
        "recipeSourcesCount": 0
    },
    "imageId": "CCF85447ADA6FFF9E96E7DF0B796F7A86E34593",
    "accentColor": "376094"
},
```

Quando chamar a API de Pesquisa de Imagens do Bing, o Bing devolve uma lista de resultados. A lista é um subconjunto do número total de resultados que são relevantes para a consulta. O campo `totalEstimatedMatches` da resposta contém uma estimativa do número de imagens que estão disponíveis para visualização. Para obter detalhes sobre como percorrer as imagens restantes, veja [Paginar Imagens](../paging-images.md).

## <a name="next-steps"></a>Passos Seguintes

Se ainda não tiver tentado a API de pesquisa de imagens do Bing antes, experimente um [guia de introdução](../quickstarts/csharp.md). Se estiver procurando por algo mais complexo, experimente o tutorial para criar uma [aplicação web de página única](../tutorial-bing-image-search-single-page-app.md).
