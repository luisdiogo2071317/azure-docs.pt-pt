---
title: Tipos de estrutura e a resposta de resposta de API de pesquisa Web Bing
titleSuffix: Azure Cognitive Services
description: Saiba mais sobre os tipos de resposta e as respostas utilizadas pela API de pesquisa Web do Bing.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: conceptual
ms.date: 8/13/2018
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 1d47d8e35a1be28b5610961c1b1c7b5d1492e871
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2018
ms.locfileid: "53250506"
---
# <a name="bing-web-search-api-response-structure-and-answer-types"></a>Tipos de estrutura e a resposta de resposta de API de pesquisa Web Bing  

Quando envia um pedido de pesquisa a pesquisa Web Bing, ele retorna um [ `SearchResponse` ](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse) objeto no corpo da resposta. O objeto inclui um campo para cada resposta que Bing determinadas foi relevante para consulta. Este exemplo ilustra um objeto de resposta se todas as respostas de devolvido do Bing:

```json
{
    "_type": "SearchResponse",
    "queryContext": {...},
    "webPages": {...},
    "images": {...},
    "relatedSearches": {...},
    "videos": {...},
    "news": {...},
    "spellSuggestion": {...},
    "computation": {...},
    "timeZone": {...},
    "rankingResponse": {...}
}, ...
```

Normalmente, a pesquisa Web Bing devolve um subconjunto de respostas. Por exemplo, se o termo de consulta foi *velejar dinghies*, a resposta pode incluir `webPages`, `images`, e `rankingResponse`. A menos que já usou [responseFilter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#responsefilter) para filtrar páginas da Web, a resposta inclui sempre a `webpages` e `rankingResponse` respostas.

## <a name="webpages-answer"></a>Resposta de páginas da Web

O [páginas Web](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#webanswer) resposta contém uma lista de links para páginas Web que pesquisa Web Bing determinado foram relevantes para a consulta. Cada [página da web](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#webpage) na lista inclui o URL de apresentação de nome, url, da página, uma descrição breve de conteúdo e a data do Bing foi encontrado o conteúdo.

```json
{
    "id": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0",
    "name": "Dinghy sailing",
    "url": "https:\/\/www.bing.com\/cr?IG=3A43CA5...",
    "displayUrl": "https:\/\/en.contoso.com\/wiki\/Dinghy_sailing",
    "snippet": "Dinghy sailing is the activity of sailing small boats...",
    "dateLastCrawled": "2017-04-05T16:25:00"
}, ...
```

Uso `name` e `url` para criar um hiperlink que direciona o utilizador para a página Web.

<!-- Remove until this can be replaced with a sanitized version.
The following shows an example of how you might display the webpage in a search results page.

![Rendered webpage example](./media/cognitive-services-bing-web-api/bing-rendered-webpage-example.PNG)
-->

## <a name="images-answer"></a>Resposta de imagens

O [imagens](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) resposta contém uma lista de imagens Bing achava que foram relevantes para a consulta. Cada [imagem](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) na lista inclui o URL da imagem, seu tamanho, suas dimensões e seu formato de codificação. O objeto de imagem também inclui o URL de uma miniatura da imagem e as dimensões da miniatura.

```json
{
    "name": "Rich Passage Sailing Dinghy",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=3A43CA5CA64...",
    "thumbnailUrl": "https:\/\/tse1.mm.bing.net\/th?id=OIP....",
    "datePublished": "2011-10-29T11:26:00",
    "contentUrl": "http:\/\/upload.contoso.com\/sailing\/...",
    "hostPageUrl": "http:\/\/www.bing.com\/cr?IG=3A43CA5CA6464....",
    "contentSize": "79239 B",
    "encodingFormat": "jpeg",
    "hostPageDisplayUrl": "http:\/\/en.contoso.com\/wiki\/File...",
    "width": 526,
    "height": 688,
    "thumbnail": {
        "width": 229,
        "height": 300
    },
    "insightsSourcesSummary": {
        "shoppingSourcesCount": 0,
        "recipeSourcesCount": 0
    }
}, ...
```

Consoante o dispositivo do utilizador, normalmente seria exibe um subconjunto das miniaturas com uma opção para o utilizador ver as imagens restantes.

<!-- Remove until this can be replaced with a sanitized version.
![List of thumbnail images](./media/cognitive-services-bing-web-api/bing-web-image-thumbnails.PNG)
-->

Também pode expandir a miniatura à medida que o utilizador paira o cursor sobre a mesma. Lembre-se de atribuir a imagem se a expandir. Por exemplo, extraindo o anfitrião `hostPageDisplayUrl` e exibi-lo abaixo da imagem. Para obter informações sobre o redimensionamento da miniatura, veja [Redimensionar e Recortar Miniaturas](./resize-and-crop-thumbnails.md).

<!-- Remove until this can be replaced with a sanitized version.
![Expanded view of thumbnail image](./media/cognitive-services-bing-web-api/bing-web-image-thumbnail-expansion.PNG)
-->

Se o usuário clica na miniatura, utilize `webSearchUrl` para levar o usuário a search página de resultados do Bing para imagens, que contém um collage das imagens.

Para obter detalhes sobre a resposta de imagem e imagens, consulte [API de pesquisa de imagens](../bing-image-search/search-the-web.md).

## <a name="related-searches-answer"></a>Resposta de pesquisas relacionadas

O [relatedSearches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse-relatedsearches) resposta contém uma lista das consultas relacionadas mais populares feitas por outros usuários. Cada [consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#query_obj) na lista inclui uma cadeia de consulta (`text`), uma cadeia de consulta com o detetor de ocorrências de carateres (`displayText`) e um URL (`webSearchUrl`) à página de resultados de pesquisa do Bing para essa consulta.

```json
{
    "text": "dinghy racing teams",
    "displayText": "dinghy racing teams",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=96C4CF214A0..."
}, ...
```

Utilize o `displayText` cadeia de consulta e o `webSearchUrl` página para a consulta relacionada com os resultados de URL para criar um hiperlink que direciona o utilizador para a pesquisa do Bing. Também pode usar o `text` consultar a cadeia de caracteres em sua própria consulta de API de pesquisa na Web e exibir os resultados por conta própria.

Para obter informações sobre como lidar com os marcadores de realce na `displayText`, consulte [pressionar realce](./hit-highlighting.md).

A seguir mostra um exemplo da utilização da consultas relacionados no Bing.com.

![Exemplo de pesquisas relacionadas no Bing](./media/cognitive-services-bing-web-api/bing-web-rendered-relatedsearches.GIF)

## <a name="videos-answer"></a>Resposta de vídeos

O [vídeos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos) resposta contém uma lista de vídeos do Bing achava que foram relevantes para a consulta. Cada [vídeo](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video) na lista incluem o URL do vídeo, sua duração, suas dimensões e seu formato de codificação. O objeto de vídeo também inclui o URL de uma miniatura do vídeo e as dimensões da miniatura.

```json
{
    "name": "Sailing dinghy",
    "description": "Northwind Traders is a 12 foot gunter rigged...",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=1CAE739681D84...",
    "thumbnailUrl": "https:\/\/tse2.mm.bing.net\/th?id=OVP.wsKiL...",
    "datePublished": "2013-11-06T01:56:28",
    "publisher": [{
        "name": "Fabrikam"
    }],
    "contentUrl": "https:\/\/www.fabrikam.com\/watch?v=MrVBWZpJjX",
    "hostPageUrl": "https:\/\/www.bing.com\/cr?IG=1CAE739681D8400DB...",
    "encodingFormat": "mp4",
    "hostPageDisplayUrl": "https:\/\/www.fabrikam.com\/watch?v=MrBWZpJjXo",
    "width": 1280,
    "height": 720,
    "duration": "PT3M47S",
    "motionThumbnailUrl": "https:\/\/tse2.mm.bing.net\/th?id=OM.oa...",
    "embedHtml": "<iframe width=\"1280\" height=\"720\" src=\"http:\/\/www....><\/iframe>",
    "allowHttpsEmbed": true,
    "viewCount": 19089,
    "thumbnail": {
        "width": 300,
        "height": 168
    },
    "allowMobileEmbed": true,
    "isSuperfresh": false
}, ...
```

Consoante o dispositivo do utilizador, normalmente seria exibe um subconjunto dos vídeos com uma opção para o utilizador ver os vídeos restantes. Poderia exibir uma miniatura de vídeo com a duração do vídeo, description (nome) e atribuição (Editor).

<!-- Remove until this can be replaced with a sanitized version.
![List of video thumbnails](./media/cognitive-services-bing-web-api/bing-web-video-thumbnails.PNG)
-->

Conforme o usuário focalizar a miniatura é possível usar `motionThumbnailUrl` para reproduzir uma versão em miniatura do vídeo. Certifique-se de que adiciona um atributo à miniatura de movimento quando a apresentar.

<!-- Remove until this can be replaced with a sanitized version.
![Motion thumbnail of a video](./media/cognitive-services-bing-web-api/bing-web-video-motion-thumbnail.PNG)
-->

Se o utilizador clicar na miniatura, seguem-se as opções de visualização do vídeo:

- Utilize `hostPageUrl` para ver o vídeo no site do anfitrião (por exemplo, YouTube)
- Utilize `webSearchUrl` para ver o vídeo no navegador de vídeo Bing
- Utilize `embedHtml` para incorporar o vídeo na sua própria experiência

Para obter detalhes sobre os vídeos e a resposta de vídeo, consulte [API de pesquisa de vídeo](../bing-video-search/search-the-web.md).

## <a name="news-answer"></a>Resposta de notícias

O [notícias](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news) resposta contém uma lista de artigos de notícias Bing achava que foram relevantes para a consulta. Cada [artigo](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) na lista inclui o nome, a descrição e o URL do artigo no site do anfitrião. Se o artigo contiver uma imagem, o objeto inclui uma miniatura da imagem.

```json
{
    "name": "WC Sailing Qualifies for America Trophy with...",
    "url": "http:\/\/www.bing.com\/cr?IG=3445EEF15DAF4FFFBF7...",
    "image": {
        "contentUrl": "http:\/\/www.contoso.com\/sports\/sail...",
        "thumbnail": {
            "contentUrl": "https:\/\/www.bing.com\/th?id=ON.1...",
            "width": 400,
            "height": 272
        }
    },
    "description": "The WC sailing team qualified for a...",
    "provider": [{
        "_type": "Organization",
        "name": "contoso.com"
    }],
    "datePublished": "2017-04-16T21:56:00"
}, ...
```

Consoante o dispositivo do utilizador, seria exibida um subconjunto dos artigos de notícias com uma opção para o utilizador veja os artigos restantes. Utilize `name` e `url` para criar uma hiperligação que direcione o utilizador para o artigo no site do anfitrião. Se o artigo inclui uma imagem, fazer a imagem clicáveis com `url`. Certifique-se de que utiliza `provider` para atribuir o artigo.

<!-- Remove until this can be replaced with a sanitized version.
The following shows an example of how you might display articles in a search results page.

![List of news articles](./media/cognitive-services-bing-web-api/bing-web-news-list.PNG)
-->

Para obter detalhes sobre a resposta de notícias e artigos de notícias, consulte [API de pesquisa de notícias](../bing-news-search/search-the-web.md).

## <a name="computation-answer"></a>Resposta de computação

Se o usuário inserir uma expressão de matemática ou uma consulta de conversão de unidade, a resposta pode conter uma [computação](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#computation) resposta. O `computation` resposta contém a expressão normalizada e seu resultado.

Uma consulta de conversão de unidade é uma consulta que converte uma unidade para outro. Por exemplo, *quantos pés em 10 metros?* ou *tablespoons quantos numa xícara de 1/4?*

A seguir mostra a `computation` responder para *pés quantos em 10 metros?*

```json
"computation": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#Computation",
    "expression": "10 meters",
    "value": "32.808399 feet"
}, ...
```

O seguinte mostra exemplos de consultas de matemáticas e seus correspondentes `computation` respostas.

```
Query: (5+3)(10/2)+8
Encoded query: %285%2B3%29%2810%2F2%29%2B8
```

```json
"computation": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#Computation",
    "expression": "((5+3)*(10\/2))+8",
    "value": "48"
}
```

```
Query: sqrt(4^2+8^2)
Encoded query: sqrt%284^2%2B8^2%29
```

```json
"computation": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#Computation",
    "expression": "sqrt((4^2)+(8^2))",
    "value": "8.94427191"
}
```

```
Query: 30 6/8 - 18 8/16
Encoded query: 30%206%2F8%20-%2018%208%2F16
```

```json
"computation": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#WolframAlpha",
    "expression": "30 6\/8-18 8\/16",
    "value": "12.25"
}
```

```
Query: 8^2+11^2-2*8*11*cos(37)
Encoded query: 8^2%2B11^2-2*8*11*cos%2837%29
```

```json
"computation": {
        "id": "https:\/\/www.bing.com\/api\/v7\/#Computation",
        "expression": "(8^2)+(11^2)-(2*8*11*cos(37))",
        "value": "44.4401502"
}
```

Uma expressão matemática pode conter os seguintes símbolos:

|Símbolo|Descrição|
|------------|-----------------|
|+|Adição|
|-|Subtração|
|/|divisão|
|*|Multiplicação|
|^|Power|
|!|Fatorial|
|.|decimal|
|()|Agrupamento de precedência|
|[]|Função|

Uma expressão matemática pode conter as seguintes constantes:

|Símbolo|Descrição|
|------------|-----------------|
|Instalador de plataforma|3.14159...|
|Grau|Grau|
|Eu|Número de imaginery|
|i|e, do 2.71828...|
|GoldenRatio|Rácio dourado, 1.61803...|

Uma expressão matemática pode conter as seguintes funções:

|Símbolo|Descrição|
|------------|-----------------|
|Sqrt|Raiz quadrada|
|Sin [x], Cos [x], Tan [x]<br />CSC [x], seg [x], Cot [x]|Funções trigonométricas (com argumentos em radianos)|
|ArcSin [x], [x], de ArcCos ArcTan [x]<br />ArcCsc [x], [x], de ArcSec ArcCot [x]|Inversa funções trigonométricas (dar resultados em radianos)|
|EXP [x], E ^ x|Função exponencial|
|Registo [x]|Logaritmo natural|
|Sinh [x], [x], de Cosh Tanh [x]<br />Csch [x], [x], de Sech Coth [x]|Funções de hyperbolic|
|ArcSinh [x], [x], de ArcCosh ArcTanh [x]<br />ArcCsch [x], [x], de ArcSech ArcCoth [x]|Funções de hyperbolic inversa|

Não são suportadas expressões matemáticas que contenham variáveis (por exemplo, 4 x + 6 = 18, onde x é a variável).

## <a name="timezone-answer"></a>Resposta de fuso horário

Se o usuário inserir uma consulta de hora ou data, a resposta pode conter uma [fuso horário](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#timezone) resposta. Esta resposta suporta consultas implícitas ou explícitas. Uma consulta implícita, tal como *que altura é isso?*, devolve a hora local com base na localização do utilizador. Uma consulta explícita, por exemplo, *que altura é em Seattle?*, devolve a hora local para Seattle, WA.

O `timeZone` resposta fornece o nome da localização, a atual data e hora UTC na localização especificada e o UTC de deslocamento. Se o limite da localização é dentro de vários fusos horários, a resposta contém a atual data e hora UTC de todos os fusos horários, dentro do limite. Por exemplo, porque o estado da Flórida se encontrem dentro de dois fusos horários, a resposta contém a data local e a hora de ambos os fusos horários.  

Se a consulta solicitar o tempo de um Estado ou país, Bing determina a cidade principal dentro do limite da localização geográfica e devolve-na `primaryCityTime` campo. Se o limite contém vários fusos horários, os fusos horários restantes são retornados no `otherCityTimes` campo.

O exemplo a seguir mostra consultas que devolvem o `timeZone` resposta.

```
Query: What time is it?

"timeZone": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#TimeZone",
    "primaryCityTime": {
        "location": "Redmond, Washington, United States",
        "time": "2015-10-27T08:38:12.1189231Z",
        "utcOffset": "UTC-7"
    }
}

Query: What time is it in the Pacific time zone?

"timeZone": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#TimeZone",
    "primaryCityTime": {
        "location": "Pacific Time Zone",
        "time": "2015-10-23T12:33:19.0728146Z",
        "utcOffset": "UTC-7"
    }
}

Query: Time in Florida?

"timeZone": {
        "id": "https:\/\/www.bing.com\/api\/v7\/#TimeZone",
        "primaryCityTime": {
            "location": "Tallahassee, Florida, United States",
            "time": "2015-10-23T13:04:56.6774389Z",
            "utcOffset": "UTC-4"
        },
        "otherCityTimes": [{
            "location": "Pensacola",
            "time": "2015-10-23T12:04:56.6664294Z",
            "utcOffset": "UTC-5"
        }]
}

Query: What time is it in the U.S.

"timeZone": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#TimeZone",
    "primaryCityTime": {
        "location": "Washington, D.C., United States",
        "time": "2015-10-23T15:27:59.8892745Z",
        "utcOffset": "UTC-4"
    },
    "otherCityTimes": [{
        "location": "Honolulu",
        "time": "2015-10-23T09:27:59.8892745Z",
        "utcOffset": "UTC-10"
    },
    {
        "location": "Anchorage",
        "time": "2015-10-23T11:27:59.8892745Z",
        "utcOffset": "UTC-8"
    },
    {
        "location": "Phoenix",
        "time": "2015-10-23T12:27:59.8892745Z",
        "utcOffset": "UTC-7"
    },
    {
        "location": "Los Angeles",
        "time": "2015-10-23T12:27:59.8942788Z",
        "utcOffset": "UTC-7"
    },
    {
        "location": "Denver",
        "time": "2015-10-23T13:27:59.8812681Z",
        "utcOffset": "UTC-6"
    },
    {
        "location": "Chicago",
        "time": "2015-10-23T14:27:59.8892745Z",
        "utcOffset": "UTC-5"
    }]
}
```

## <a name="spellsuggestion-answer"></a>Resposta de SpellSuggestion

Se o Bing determina que o utilizador pode ter se destina a procurar algo diferente, a resposta inclui uma [SpellSuggestions](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#spellsuggestions) objeto. Por exemplo, se o utilizador procura *caneta de carlos*, Bing pode determinar que o usuário provavelmente se destina a procurar Carlos Pena (com base em pesquisas nos últimos por outras pessoas de *caneta de carlos*). O código a seguir mostra uma resposta de ortografia de exemplo.

```json
"spellSuggestions": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#SpellSuggestions",
    "value": [{
        "text": "carlos pena",
        "displayText": "carlos pena"
    }]
}, ...
```

A seguir mostra como o Bing utiliza as sugestões de ortografia.

![Exemplo de sugestões de ortografia do Bing](./media/cognitive-services-bing-web-api/bing-web-spellingsuggestion.GIF)  

## <a name="next-steps"></a>Passos Seguintes  

* Revisão [limitação de pedidos](throttling-requests.md) documentação.  

## <a name="see-also"></a>Consulte também  

* [Referência da API de pesquisa Web Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
