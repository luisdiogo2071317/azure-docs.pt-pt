---
title: O que é a Pesquisa de Entidades do Bing? | Microsoft Docs
description: Saiba como utilizar a API de pesquisa de entidades do Bing para pesquisar a web para entidades e locais.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 0B54E747-61BF-42AA-8788-E25D63F625FC
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 07/06/2016
ms.author: scottwhi
ms.openlocfilehash: 275430bc6ee8f935978243e61f68713974648189
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008115"
---
# <a name="what-is-bing-entity-search"></a>O que é a Pesquisa de Entidades do Bing?

A API de pesquisa de entidades do Bing envia uma consulta de pesquisa para o Bing e obtém resultados que incluem entidades e locais. Resultados de lugar incluem restaurantes, no hotel ou outros negócios locais. O Bing devolve lugares, se a consulta Especifica o nome da empresa local ou solicita um tipo de negócio (por exemplo, restaurantes perto de mim). O Bing devolve entidades, se a consulta Especifica bem conhecidas pessoas, lugares (tourist attractions, Estados, países, etc.) ou coisas.

## <a name="suggesting--using-search-terms"></a>Sugerir e utilizar termos de pesquisa

Se disponibilizar uma caixa de pesquisa na qual o utilizador introduz o seu termo de pesquisa, utilize a [API de Sugestão Automática do Bing](../bing-autosuggest/get-suggested-search-terms.md) para melhorar a experiência. A API devolve cadeias de consulta sugerida com base em termos de pesquisa parcial à medida que o utilizador escreve.

Depois de o utilizador introduzir o seu termo de pesquisa, o URL codifica o termo antes de definir o parâmetro de consulta [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#query). Por exemplo, se o usuário insere *Marcus Appel*, defina `q` para *Marcus + Appel* ou *Marcus % 20Appel*.

Se o termo de pesquisa contém um erro de ortografia, a resposta de pesquisa inclui um [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#querycontext) objeto. O objeto mostra a ortografia original e a ortografia corrigida que Bing utilizado para a pesquisa.

```json
"queryContext": {
    "originalQuery": "hollo wrld",
    "alteredQuery": "hello world",
    "alterationOverrideQuery": "+hollo wrld",
    "adultIntent": false
}
```

## <a name="requesting-entities"></a>Pedir entidades

Para um exemplo de solicitação, consulte [fazendo o primeiro pedido](./quick-start.md).

## <a name="the-response"></a>A resposta

A resposta contém um [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#searchresponse) objeto. Se o Bing encontrar uma entidade ou o que é relevante, o objeto inclui a `entities` campo, `places` campo, ou ambos. Caso contrário, o objeto de resposta não inclui qualquer um dos campos.
> [!NOTE]
> Respostas de entidade suportam vários mercados, mas a resposta de lugares suporta apenas as localizações nos negócios. 

O `entities` campo é um [EntityAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entityanswer) objeto que contém uma lista de [entidade](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity) objetos (consulte a `value` campo). A lista pode conter uma única entidade dominante, vários de desambiguação de entidades ou ambas. 

Uma entidade dominante é uma entidade que Bing acredita que é a única entidade que satisfaça a solicitação (não há ambiguidade em relação ao que a entidade satisfaça a solicitação). Se várias entidades podem satisfazer a solicitação, a lista contém mais de uma entidade de desambiguação. Por exemplo, se o pedido utiliza o título genérico de franquia um filme, a lista provavelmente contém entidades de desambiguação. No entanto, se o pedido especifica um título específico da franquia, a lista provavelmente contém uma única entidade dominante.

As entidades incluem personalidades bem conhecidas como singers, atores, atletas Favoritos, modelos, etc.; locais e de pontos de referência, como montagem Rainier ou Lincoln Memorial; e coisas como um título banana, goldendoodle, livro ou filme. O [entityPresentationInfo](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entitypresentationinfo) campo contém sugestões que identificam o tipo da entidade. Por exemplo, se se trata de uma pessoa, filmes, animal ou attraction. Para obter uma lista de tipos possíveis, consulte [tipos de entidade](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity-types)

```json
"entityPresentationInfo": {
    "entityScenario": "DominantEntity",
    "entityTypeHints": ["Attraction"],
    "entityTypeDisplayHint": "Mountain"
}, ...
```

O código a seguir mostra uma resposta que inclui uma entidade dominante e a desambiguação.

```json
{
    "_type": "SearchResponse",
    "queryContext": {
        "originalQuery": "Mount Rainier"
    },
    "entities": {
        "value": [{
            "contractualRules": [{
                "_type": "ContractualRules/LicenseAttribution",
                "targetPropertyName": "description",
                "mustBeCloseToContent": true,
                "license": {
                    "name": "CC-BY-SA",
                    "url": "http://creativecommons.org/licenses/by-sa/3.0/"
                },
                "licenseNotice": "Text under CC-BY-SA license"
            },
            {
                "_type": "ContractualRules/LinkAttribution",
                "targetPropertyName": "description",
                "mustBeCloseToContent": true,
                "text": "contoso.com",
                "url": "http://contoso.com/mount_rainier"
            },
            {
                "_type": "ContractualRules/MediaAttribution",
                "targetPropertyName": "image",
                "mustBeCloseToContent": true,
                "url": "http://contoso.com/mount-rainier"
            }],
            "webSearchUrl": "https://www.bing.com/search?q=Mount%20Rainier...",
            "name": "Mount Rainier",
            "url": "http://www.northwindtraders.com/",
            "image": {
                "name": "Mount Rainier",
                "thumbnailUrl": "https://www.bing.com/th?id=A4ae343983daa4...",
                "provider": [{
                    "_type": "Organization",
                    "url": "http://contoso.com/mount_rainier"
                }],
                "hostPageUrl": "http://contoso.com/commons/7/72/mount_rain...",
                "width": 110,
                "height": 110
            },
            "description": "Mount Rainier is 14,411 ft tall and the highest mountain...",
            "entityPresentationInfo": {
                "entityScenario": "DominantEntity",
                "entityTypeHints": ["Attraction"]
            },
            "bingId": "38b9431e-cf91-93be-0584-c42a3ecbfdc7"
        },
        {
            "contractualRules": [{
                "_type": "ContractualRules/MediaAttribution",
                "targetPropertyName": "image",
                "mustBeCloseToContent": true,
                "url": "http://contoso.com/mount_rainier_national_park"
            }],
            "webSearchUrl": "https://www.bing.com/search?q=Mount%20Rainier%20National...",
            "name": "Mount Rainier National Park",
            "url": "http://worldwideimporters.com/",
            "image": {
                "name": "Mount Rainier National Park",
                "thumbnailUrl": "https://www.bing.com/th?id=A91bdc5a1b648a695a39...",
                "provider": [{
                    "_type": "Organization",
                    "url": "http://contoso.com/mount_rainier_national_park"
                }],
                "hostPageUrl": "http://contoso.com/en/7/7a...",
                "width": 50,
                "height": 50
            },
            "description": "Mount Rainier National Park is a United States National Park...",
            "entityPresentationInfo": {
                "entityScenario": "DisambiguationItem",
                "entityTypeHints": ["Organization"]
            },
            "bingId": "29d4b681-227a-3924-7bb1-8a54e8666b8c"
        }]
    }
}
```

A entidade inclui um `name`, `description`, e `image` campo. Quando visualiza estes campos na sua experiência de utilizador, tem de os atributos. O `contractualRules` campo contém uma lista de atribuições tem de aplicar. A regra contratual identifica o campo que a atribuição aplica-se a. Para obter informações sobre como aplicar atribuição, consulte [atribuição](#data-attribution).

```json
"contractualRules": [{
    "_type": "ContractualRules/LicenseAttribution",
    "targetPropertyName": "description",
    "mustBeCloseToContent": true,
    "license": {
        "name": "CC-BY-SA",
        "url": "http://creativecommons.org/licenses/by-sa/3.0/"
    },
    "licenseNotice": "Text under CC-BY-SA license"
},
{
    "_type": "ContractualRules/LinkAttribution",
    "targetPropertyName": "description",
    "mustBeCloseToContent": true,
    "text": "contoso.com",
    "url": "http://contoso.com/wiki/Mount_Rainier"
},
{
    "_type": "ContractualRules/MediaAttribution",
    "targetPropertyName": "image",
    "mustBeCloseToContent": true,
    "url": "http://contoso.com/wiki/Mount_Rainier"
}], ...
```

Quando exibir as informações da entidade (nome, descrição e imagem), também tem de utilizar o URL no `webSearchUrl` página que contém a entidade de resultados de campo para ligar para a pesquisa do Bing.


O `places` campo é um [LocalEntityAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#localentityanswer) objeto que contém uma lista de [local](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#place) objetos (consulte a `value` campo). A lista contém um ou mais entidades locais que satisfazem o pedido.

Locais incluem restaurante, hotéis ou negócios locais. O [entityPresentationInfo](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entitypresentationinfo) campo contém sugestões que identificam o tipo da entidade local. A lista contém uma lista de sugestões, como local, LocalBusiness, restaurante. Cada sugestão sucessiva na matriz restringe o tipo da entidade. Para obter uma lista de tipos possíveis, consulte [tipos de entidade](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity-types)

```json
"entityPresentationInfo": {
    "entityScenario": "ListItem",
    "entityTypeHints": ["Place",
    "LocalBusiness",
    "Restaurant"]
}, ...
```
> [!NOTE]
> Respostas de entidade suportam vários mercados, mas a resposta de lugares suporta apenas as localizações nos negócios. 

Consultas de entidade com reconhecimento de local como *restaurante ao meu redor* requerem a localização do utilizador para fornecer resultados precisos. Os pedidos de devem sempre utilizar a localização de pesquisa de X e cabeçalhos X-MSEdge-ClientIP para especificar a localização do utilizador. Se achar que a Bing, a consulta beneficiariam com a localização do utilizador, ele define a `askUserForLocation` campo [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#querycontext) para **verdadeiro**. 

```json
{
    "_type": "SearchResponse",
    "queryContext": {
        "originalQuery": "Sinful Bakery and Cafe",
        "askUserForLocation": true
    },
    ...
}
```

Um resultado de lugar inclui o local onde nome, endereço, número de telefone e URL para o site da entidade. Quando exibir as informações da entidade, também tem de utilizar o URL no `webSearchUrl` página que contém a entidade de resultados de campo para ligar para a pesquisa do Bing.

```json
"places": {
    "value": [{
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Sinful%20Bakery...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "http://libertysdelightfulsinfulbakeryandcafe.com/",
        "entityPresentationInfo": {
            "entityScenario": "ListItem",
            "entityTypeHints": ["Place",
            "LocalBusiness",
            "Restaurant"]
        },
        "address": {
            "addressLocality": "Seattle",
            "addressRegion": "WA",
            "postalCode": "98112",
            "addressCountry": "US",
            "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
    }]
}
```

> [!NOTE]
> , Ou uma aplicação de terceiros em seu nome, poderá não utilizar, manter, armazenar, colocar em cache, partilhar, ou distribuir quaisquer dados da API de entidades para fins de teste, desenvolvimento, treinamento, distribuir ou ao disponibilizá-se de que qualquer serviço de terceiros ou de recursos.  

## <a name="data-attribution"></a>Atribuição de dados

Respostas de API entidades do Bing contêm informações de propriedade de terceiros. É responsável para garantir a que sua utilização está apropriada, por exemplo cumprimento de qualquer licença creative commons que possam recorrer a sua experiência de utilizador.

Se uma resposta ou o resultado inclui os `contractualRules`, `attributions`, ou `provider` campos, tem de atributo os dados. Se a resposta não inclui qualquer um desses campos, sem atribuição é necessária. Se a resposta inclui a `contractualRules` campo e o `attributions` e/ou `provider` campos, tem de utilizar as regras contratuais para os dados de atributo.

O exemplo seguinte mostra uma entidade que inclui uma regra de contratual MediaAttribution e uma imagem que inclua um `provider` campo. A regra de MediaAttribution identifica a imagem como o destino da regra, para que poderia ignorar a imagem `provider` campo em vez disso, a regra de MediaAttribution para fornecer a atribuição.  

```json
"value": [{
    "contractualRules": [
        ...
        {
            "_type": "ContractualRules/MediaAttribution",
            "targetPropertyName": "image",
            "mustBeCloseToContent": true,
            "url": "http://contoso.com/mount_rainier"
        }
    ],
    ...
    "image": {
        "name": "Mount Rainier",
        "thumbnailUrl": "https://www.bing.com/th?id=A46378861201...",
        "provider": [{
            "_type": "Organization",
            "url": "http://contoso.com/mount_rainier"
        }],
        "hostPageUrl": "http://www.graphicdesigninstitute.com/Uploaded...",
        "width": 110,
        "height": 110
    },
    ...
}]
```

Se uma regra contratual inclui o `targetPropertyName` campo, a regra aplica-se apenas para o campo de destino. Caso contrário, a regra se aplica ao objeto pai que contém o `contractualRules` campo.

No exemplo a seguir, o `LinkAttribution` regra inclui o `targetPropertyName` campo, para que a regra aplica-se para o `description` campo. Para regras que se aplicam a campos específicos, tem de incluir uma linha imediatamente após os dados de destinados que contém uma hiperligação para o site do fornecedor. Por exemplo, para a descrição do atributo, inclua uma linha imediatamente após o texto de descrição que contém uma hiperligação para os dados no site do fornecedor, neste caso, criar uma ligação para contoso.com.

```json
"entities": {
    "value": [{
            ...
            "description": "Marcus Appel is a former American....",
            ...
            "contractualRules": [{
                    "_type": "ContractualRules/LinkAttribution",
                    "targetPropertyName": "description",
                    "mustBeCloseToContent": true,
                    "text": "contoso.com",
                    "url": "http://contoso.com/cr?IG=B8AD73..."
                 },
            ...
  
```

### <a name="license-attribution"></a>Atribuição de licença

Se a lista de regras contratuais inclui um [LicenseAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#licenseattribution) regra, tem de apresentar o aviso na linha imediatamente após o conteúdo que a licença se aplica a. O `LicenseAttribution` regra utiliza o `targetPropertyName` campo para identificar a propriedade que se aplica a licença.

O seguinte mostra um exemplo que inclui um `LicenseAttribution` regra.

![Atribuição de licença](./media/cognitive-services-bing-entities-api/licenseattribution.png)

O aviso de licença que exibir tem de incluir um hiperlink para o site que contenha informações sobre a licença. Normalmente, verifique o nome da licença de um hiperlink. Por exemplo, se for o aviso **texto sob a licença de CC por SA** e CC por SA é o nome da licença, que faria CC por SA um hiperlink.

### <a name="link-and-text-attribution"></a>Atribuição de ligação e texto

O [LinkAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#linkattribution) e [TextAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#textattribution) regras são normalmente utilizadas para identificar o fornecedor de dados. O `targetPropertyName` campo identifica o campo que se aplica a regra.

Para os fornecedores de atributo, inclua uma linha imediatamente após o conteúdo que se aplicam as atribuições para (por exemplo, o campo de destino). A linha deve ser claramente nome para indicar que os fornecedores são a origem dos dados. Por exemplo, "dados a partir de: contoso.com". Para `LinkAttribution` regras, tem de criar uma hiperligação para o site do fornecedor.

O seguinte mostra um exemplo que inclui `LinkAttribution` e `TextAttribution` regras.

![Atribuição de texto do Link](./media/cognitive-services-bing-entities-api/linktextattribution.png)

### <a name="media-attribution"></a>Atribuição de suporte de dados

Se a entidade inclui uma imagem e apresentá-lo, tem de fornecer um link clicável para o site do fornecedor. Se a entidade inclui um [MediaAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#mediaattribution) de regra, utilize o URL da regra para criar a ligação clicável. Caso contrário, utilize o URL incluído na imagem do `provider` campo para criar a ligação clicável.

O seguinte mostra um exemplo que inclui uma imagem `provider` campo e regras contratuais. Uma vez que o exemplo inclui a regra contratual, ignorar a imagem `provider` campo e aplicar o `MediaAttribution` regra.

![Atribuição de suporte de dados](./media/cognitive-services-bing-entities-api/mediaattribution.png)

### <a name="search-or-search-like-experience"></a>Pesquisa ou experiência de pesquisa semelhantes

Tal como com a API de pesquisa Web Bing, a API de pesquisa de entidades do Bing só podem ser utilizada como resultado de uma consulta de utilizador direto ou a pesquisa, ou como resultado de uma ação num aplicativo ou a experiência que logicamente pode ser interpretada como a solicitação de pesquisa de um utilizador. Para efeitos de ilustração, seguem-se alguns exemplos de pesquisa aceitável ou experiências de pesquisa semelhantes.

- Usuário insere uma consulta diretamente na caixa de pesquisa numa aplicação
- Utilizador seleciona um texto específico ou imagem e pedidos "obter mais informações" ou "additional information"
- Utilizador pede um bot de pesquisa sobre um tópico específico
- Utilizador dwells num determinado objeto ou uma entidade num cenário do tipo de pesquisa visual

Se não tiver a certeza se a sua experiência pode ser considerada uma experiência de pesquisa semelhantes, recomenda-se que verifique com a Microsoft.

## <a name="throttling-requests"></a>Limitar pedidos

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Passos Seguintes

Para começar rapidamente com o primeiro pedido, consulte [tornando sua primeira solicitação](./quick-start.md).

Familiarize-se com o [API de pesquisa de entidades do Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) referência. A referência contém os cabeçalhos e os parâmetros de consulta que utiliza para solicitar os resultados da pesquisa. Também inclui as definições dos objetos de resposta. 

Para melhorar a sua experiência de utilizador da caixa de pesquisa, veja o artigo [API de Sugestão Automática do Bing](../bing-autosuggest/get-suggested-search-terms.md). À medida que o utilizador introduz o termo de consulta, pode chamar esta API para obter os termos de consulta relevantes que foram utilizados por outras pessoas.

Não se esqueça de ler o artigo [Requisitos de Utilização e Apresentação do Bing](./use-display-requirements.md) para que não infrinja nenhuma das regras relativas à utilização dos resultados da pesquisa.