---
title: O que é a Pesquisa de Imagens do Bing? | Microsoft Docs
description: Saiba como utilizar a API de Pesquisa de Imagens do Bing para pesquisar imagens na Web.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 1446AD8B-A685-4F5F-B4AA-74C8E9A40BE9
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: overview
ms.date: 10/11/2017
ms.author: scottwhi
ms.openlocfilehash: 457707065059b5cb83c9d2b81f5d073cf1c89b84
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36336524"
---
# <a name="what-is-bing-image-search"></a>O que é a Pesquisa de Imagens do Bing?

A API de Pesquisa de Imagens do Bing proporciona uma experiência semelhante a [Imagens do Bing](https://www.bing.com/images) ao permitir-lhe enviar uma consulta de pesquisa de utilizador para o Bing e obter uma lista de imagens relevantes.

Se estiver a criar uma página de resultados de pesquisa só de imagens para localizar as imagens que são relevantes para a consulta de pesquisa do utilizador, chame esta API em vez de chamar a [API de Pesquisa na Web](../bing-web-search/search-the-web.md) mais geral. Se pretender imagens e outros tipos de conteúdo, como páginas Web, notícias e vídeos, chame a API de Pesquisa na Web.

## <a name="suggesting--using-search-terms"></a>Sugerir e utilizar termos de pesquisa

Se disponibilizar uma caixa de pesquisa na qual o utilizador introduz o seu termo de pesquisa, utilize a [API de Sugestão Automática do Bing](../bing-autosuggest/get-suggested-search-terms.md) para melhorar a experiência. A API devolve cadeias de consulta sugerida com base em termos de pesquisa parcial à medida que o utilizador escreve.

Depois de o utilizador introduzir o seu termo de pesquisa, o URL codifica o termo antes de definir o parâmetro de consulta [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query). Por exemplo, se o utilizador introduzir *sailing dinghies*, defina `q` como `sailing+dinghies` ou `sailing%20dinghies`.

## <a name="getting-images"></a>Obter imagens

Para obter imagens relacionadas com o termo de pesquisa do utilizador a partir da Web, envie o seguinte pedido GET:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Todos os pedidos têm de ser feitos a partir de um servidor. Não poderá realizar chamadas a partir de um cliente.

Se for a primeira vez que está a chamar qualquer uma das APIs do Bing, não inclua o cabeçalho de ID de cliente. Inclua apenas o ID de cliente se tiver chamado anteriormente uma API do Bing e o Bing tiver devolvido um ID de cliente para o utilizador e a combinação de dispositivo.

Para obter imagens de um domínio específico, utilize o operador de consulta [site:](http://msdn.microsoft.com/library/ff795613.aspx).

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

A resposta contém uma resposta [Imagens](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) que contém uma lista de imagens que o Bing considerou relevantes para a consulta. Cada objeto [Imagem](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) na lista inclui o URL da imagem, o respetivo tamanho, dimensões e formato de codificação. O objeto de imagem também inclui o URL de uma miniatura da imagem e as dimensões da miniatura.

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

Pode apresentar um colagem de todas as miniaturas de imagem ou pode apresentar um subconjunto das miniaturas. Se for apresentado um subconjunto, dê ao utilizador a opção de ver as imagens restantes. Tem de apresentar as imagens pela ordem fornecida na resposta.

Também pode expandir a miniatura à medida que o utilizador paira o cursor sobre a mesma. Lembre-se de atribuir a imagem se a expandir. Por exemplo, extraindo o anfitrião de [hostPageDisplayUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-hostpagedisplayurl) e apresentando-o abaixo da imagem. Para obter informações sobre o redimensionamento da miniatura, veja [Redimensionar e Recortar Miniaturas](./resize-and-crop-thumbnails.md).

<!-- Removing image until we can replace it with a sanatized version.
![Expanded view of thumbnail image](../bing-web-search/media/cognitive-services-bing-web-api/bing-web-image-thumbnail-expansion.PNG)
-->

Se o utilizador clicar na miniatura, pode utilizar [contentUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-contenturl) para apresentar a imagem em tamanho completo ao utilizador. Lembre-se de atribuir a imagem.

Se `shoppingSourcesCount` ou `recipeSourcesCount` for maior que zero, adicione a criação de distintivos, tal como um carrinho de compras, à miniatura para indicar que existem compras ou receitas para o item na imagem.

<!-- this is a sanitized version but we're removing all images for now until everything is sanitized.
![Shopping sources badge](./media/cognitive-services-bing-images-api/bing-images-shopping-source.PNG)
-->

Para obter informações sobre a imagem, tais como as páginas Web que incluem a imagem ou as pessoas que foram reconhecidas na imagem, utilize [imageInsightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-imageinsightstoken). Para obter mais informações, veja [Informações sobre uma imagem](./image-insights.md).

## <a name="filtering-images"></a>Filtrar imagens

 Por predefinição, a API de Pesquisa de Imagens devolve todas as imagens que são relevantes para a consulta. Mas se pretender apenas imagens com um fundo transparente ou imagens de um tamanho específico, teria de utilizar os seguintes parâmetros de consulta para filtrar as imagens devolvidas pelo Bing.  

- [aspect](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#aspect) — Filtrar imagens por proporção (por exemplo, imagens padrão ou em ecrã panorâmico)
- [color](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#color) — Filtrar imagens por cor dominante ou preto e branco
- [freshness](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#freshness) — Filtrar imagens por antiguidade (por exemplo, imagens descobertas pelo Bing na semana anterior)
- [height](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#height), [width](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#width) — Filtrar imagens por largura e altura
- [imageContent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagecontent) — Filtrar imagens por conteúdo (por exemplo, imagens que mostram apenas o rosto de uma pessoa)
- [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype) — Filtrar imagens por tipo (por exemplo, ClipArt, GIFs animados ou fundos transparentes)
- [license](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#license) — Filtrar imagens pelo tipo de licença associada ao site
- [size](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#size) — Filtrar imagens por tamanho, como, por exemplo, imagens pequenas até 200x200 pixéis

Para obter imagens de um domínio específico, utilize o operador de consulta [site:](http://msdn.microsoft.com/library/ff795613.aspx).

> [!NOTE]
> Dependendo da consulta, se utilizar o operador `site:`, existe a possibilidade de a resposta incluir conteúdo para adultos, independentemente da definição [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#safesearch). Deve utilizar `site:` apenas se tiver conhecimento do conteúdo do site e se o seu cenário suportar a possibilidade de conteúdo para adultos.

O exemplo seguinte mostra como obter imagens pequenas a partir de ContosoSailing.com que o Bing descobriu na semana anterior.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="pivoting-the-query"></a>Dinamizar a consulta

Se o Bing puder segmentar a consulta de pesquisa original, o objeto [Imagens](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) contém o campo `pivotSuggestions`. Por exemplo, se a consulta original era *Microsoft Surface*, o Bing poderá segmentar a consulta em *Microsoft* e *Surface*.  

O exemplo seguinte mostra as sugestões de dinamização para *Microsoft Surface*.  

```json
{
    "_type": "Images",
    "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=microsoft%20surface&FORM=OIIARP",
    "totalEstimatedMatches": 1000,
    "value": [...],
    "queryExpansions": [...],
    "pivotSuggestions": [{
        "pivot": "microsoft",
        "suggestions": [{
            "text": "Contoso Surface",
            "displayText": "Contoso",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=OtterBox+Surface&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=Contoso...",
                    "searchLink": "https:\/\/api.cognitive.microsoft.com\/api...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Contoso+Surface..."
            }
        },
        {
            "text": "Adatum Surface",
            "displayText": "Adatum",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Adatum+Surface&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Adatum+Surface&pid=Ap..."
            }
        },
        ...
        ]
    },
    {
        "pivot": "surface",
        "suggestions": [{
            "text": "Microsoft Surface4",
            "displayText": "Surface4",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface...",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft..."
            }
        },
        {
            "text": "Microsoft Tablet",
            "displayText": "Tablet",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Tablet&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Microsoft+Tablet..."
            }
        },
        ...
    ],
    "nextOffsetAddCount": 0
}
```

Em seguida, pode apresentar ao utilizador os termos de consulta opcionais no caso de terem interesse para o utilizador.

O campo `pivotSuggestions` contém a lista de segmentos (pivôs) em que a consulta original foi dividida. Para cada pivô, a resposta contém uma lista de objetos de [Consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query_obj) que contêm as consultas sugeridas. O campo `text` contém a consulta sugerida e o campo `displayText` contém o termo que substitui o pivô na consulta original. Por exemplo, Data de Lançamento do Surface.

Pode utilizar os campos `text` e `thumbnail` para apresentar as cadeias de consulta pivô ao utilizador no caso de a cadeia de consulta pivô ser realmente aquilo que procura. Torne a miniatura e o texto clicáveis com o URL `webSearchUrl` ou o URL `searchLink`. Utilize `webSearchUrl` para encaminhar o utilizador para os resultados de pesquisa do Bing ou `searchLink` se fornecer a sua própria página de resultados.

<!-- Need a sanitized version of the image
The following shows an example of the pivot queries.

![Pivot suggestions](./media/cognitive-services-bing-images-api/bing-image-pivotsuggestion.GIF)
-->

## <a name="expanding-the-query"></a>Expandir a consulta

Se o Bing puder expandir a consulta para restringir a pesquisa original, o objeto [Imagens](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) contém o campo `queryExpansions`. Por exemplo, se a consulta era *Microsoft Surface*, as consultas expandidas poderão ser: Microsoft Surface **Pro 3**, Microsoft Surface **RT**, Microsoft Surface **Phone** e Microsoft Surface **Hub**.

O exemplo seguinte mostra as consultas expandidas para *Microsoft Surface*.

```json
{
    "_type": "Images",
    "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=microsoft%20surface...",
    "totalEstimatedMatches": 1000,
    "value": [...],
    "queryExpansions":  [{
        "text": "Microsoft Surface Pro 3",
        "displayText": "Pro 3",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+Pro+3...",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=Microsoft...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+Pro+3..."
        }
    },
    {
        "text": "Microsoft Surface RT",
        "displayText": "RT",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+RT...",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+RT..."
        }
    },
    {
        "text": "Microsoft Surface Phone",
        "displayText": "Phone",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+Phone",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+Phone..."
        }
    }],
    "pivotSuggestions": [...],
    "nextOffsetAddCount": 0
}
```

O campo `queryExpansions` contém uma lista de objetos de [Consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query_obj). O campo `text` contém a consulta expandida e o campo `displayText` contém o termo de expansão. Pode utilizar os campos `text` e `thumbnail` para apresentar as cadeias de consulta expandidas ao utilizador no caso de a cadeia de consulta expandida ser realmente aquilo que procura. Torne a miniatura e o texto clicáveis com o URL `webSearchUrl` ou o URL `searchLink`. Utilize `webSearchUrl` para encaminhar o utilizador para os resultados de pesquisa do Bing ou `searchLink` se fornecer a sua própria página de resultados.

<!-- Removing until we can replace with a sanitized image.
The following shows an example Bing implementation that uses expanded queries. If the user clicks the Microsoft Surface Pro 3 link, they're taken to the Bing search results page, which shows them images of the Pro 3.

![Query expansion suggestions](./media/cognitive-services-bing-images-api/bing-image-queryexpansion.GIF)
-->

## <a name="throttling-requests"></a>Limitar pedidos

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Passos seguintes

Para começar a trabalhar rapidamente com o seu primeiro pedido, veja o [início rápido de C#](quickstarts/csharp.md).

Familiarize-se com a referência da [API de Pesquisa de Imagens do Bing v7 ](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference). A referência contém a lista de pontos finais, cabeçalhos e parâmetros de consulta que teria de utilizar para solicitar os resultados da pesquisa. Também inclui as definições dos objetos de resposta.

Para melhorar a sua experiência de utilizador da caixa de pesquisa, veja o artigo [API de Sugestão Automática do Bing](../bing-autosuggest/get-suggested-search-terms.md). À medida que o utilizador introduz o termo de consulta, pode chamar esta API para obter os termos de consulta relevantes que foram utilizados por outras pessoas.

Não se esqueça de ler o artigo [Requisitos de Utilização e Apresentação do Bing](./useanddisplayrequirements.md) para que não infrinja nenhuma das regras relativas à utilização dos resultados da pesquisa.

Quando chamar a API de Pesquisa de Imagens do Bing, o Bing devolve uma lista de resultados. A lista é um subconjunto do número total de resultados que são relevantes para a consulta. O campo `totalEstimatedMatches` da resposta contém uma estimativa do número de imagens que estão disponíveis para visualização. Para obter detalhes sobre como percorrer as imagens restantes, veja [Paginar Imagens](./paging-images.md).