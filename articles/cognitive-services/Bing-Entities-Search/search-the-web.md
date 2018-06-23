---
title: O que é a pesquisa do Bing entidade? | Microsoft Docs
description: Saiba como utilizar a API de pesquisa de entidades do Bing para pesquisar na web entidades e locais.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 0B54E747-61BF-42AA-8788-E25D63F625FC
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 07/06/2016
ms.author: scottwhi
ms.openlocfilehash: f1b87c07d5b56307fd6b3fc68999598aeab6eb82
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354896"
---
# <a name="what-is-bing-entity-search"></a>O que é a pesquisa do Bing entidade?

A API de pesquisa do Bing entidade envia uma consulta de pesquisa para o Bing e obtém os resultados que incluem entidades e locais. Resultados de local incluem restaurants, átrios ou outras empresas locais. Bing devolve locais se a consulta Especifica o nome do negócio local ou pede-lhe um tipo de negócio (por exemplo, restaurants quase-me). Bing devolve entidades, se a consulta Especifica conhecidas pessoas, locais (tourist attractions, Estados, países, etc.) ou coisas.

## <a name="suggesting--using-search-terms"></a>Sugerindo & utilizar os termos da procura

Se fornecer uma caixa de pesquisa em que o utilizador introduz as respetivas termo de pesquisa, utilize o [a API de sugestão automática do Bing](../bing-autosuggest/get-suggested-search-terms.md) para melhorar a experiência. A API devolve consulta sugerida cadeias com base em termos de pesquisa parcial dos tipos de utilizador.

Depois do utilizador introduz as respetivas termo de pesquisa, o URL codificar o termo antes de definir o [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#query) parâmetro de consulta. Por exemplo, se o utilizador introduz *Marcus Appel*, defina `q` para *Marcus + Appel* ou *Marcus % 20Appel*.

Se o termo de pesquisa contém um erro de ortografia, a resposta de pesquisa inclui um [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#querycontext) objeto. O objeto mostra a ortografia original e a ortografia corrigida que o Bing utilizado para a pesquisa.

```json
"queryContext": {
    "originalQuery": "hollo wrld",
    "alteredQuery": "hello world",
    "alterationOverrideQuery": "+hollo wrld",
    "adultIntent": false
}
```

## <a name="requesting-entities"></a>Pedir entidades

Para um exemplo de pedido, consulte [tornar o primeiro pedido](./quick-start.md).

## <a name="the-response"></a>A resposta

A resposta contém um [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#searchresponse) objeto. Se Bing encontrar uma entidade ou local que é relevante, o objeto inclui o `entities` campo, `places` campo, ou ambos. Caso contrário, o objeto de resposta não inclui um campo.

O `entities` campo é uma [EntityAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entityanswer) objeto que contém uma lista de [entidade](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity) objetos (consulte o `value` campo). A lista pode conter uma única entidade dominante, várias entidades disambiguation ou ambos. 

Uma entidade dominante é uma entidade que o Bing Deteta é a única entidade que satisfaça o pedido (não há nenhum ambiguidade relativamente a que entidade satisfaça o pedido). Se várias entidades foi possível satisfazer o pedido, a lista contém mais de uma entidade disambiguation. Por exemplo, se o pedido utiliza o título genérico de um franchise de filmes, a lista provável contém entidades disambiguation. No entanto, se o pedido especifica um título do franchise específico, a lista provável contém uma única entidade dominante.

Entidades incluem personalidades conhecidas como singers atores, athletes, modelos, etc.; locais e landmarks como montagem Rainier ou Lincoln Memorial; e coisas como um título banana, goldendoodle, livro ou filmes. O [entityPresentationInfo](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entitypresentationinfo) campo contém as sugestões que identificam o tipo da entidade. Por exemplo, se se tratar de uma pessoa, filmes, preferido ou attraction. Para obter uma lista de tipos possíveis, consulte [tipos de entidade](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity-types)

```json
"entityPresentationInfo": {
    "entityScenario": "DominantEntity",
    "entityTypeHints": ["Attraction"],
    "entityTypeDisplayHint": "Mountain"
}, ...
```

O seguinte mostra uma resposta que inclui uma entidade dominante e disambiguation.

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

A entidade inclui um `name`, `description`, e `image` campo. Quando visualiza estes campos na sua experiência de utilizador, tem de os atributos. O `contractualRules` campo contém uma lista de autoria que tem de aplicar. A regra contractual identifica o campo que se aplica a atribuição. Para obter informações sobre como aplicar atribuição, consulte [atribuição](#data-attribution).

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

Quando visualizar as informações de entidade (nome, descrição e imagem), também tem de utilizar o URL no `webSearchUrl` página que contém a entidade de resultados do campo de ligação para a pesquisa do Bing.


O `places` campo é uma [LocalEntityAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#localentityanswer) objeto que contém uma lista de [local](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#place) objetos (consulte o `value` campo). A lista contém uma ou mais entidades locais que satisfazem o pedido.

Locais incluem restaurante, hotéis ou empresas locais. O [entityPresentationInfo](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entitypresentationinfo) campo contém as sugestões que identificam o tipo da entidade local. A lista contém uma lista de sugestões, tais como o local, LocalBusiness, restaurante. Cada sugestão sucessivas na matriz restringe o tipo da entidade. Para obter uma lista de tipos possíveis, consulte [tipos de entidade](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity-types)

```json
"entityPresentationInfo": {
    "entityScenario": "ListItem",
    "entityTypeHints": ["Place",
    "LocalBusiness",
    "Restaurant"]
}, ...
```

Local entidade com suporte para consultas, tais como *restaurante quase-me* requerem a localização do utilizador para fornecer resultados precisos. Os pedidos de devem sempre utilizar a localização de pesquisa de X e X-MSEdge-ClientIP cabeçalhos para especificar a localização do utilizador. Se o Bing pensa a consulta seria beneficiar da localização do utilizador, define o `askUserForLocation` campo [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#querycontext) para **verdadeiro**. 

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

Um resultado local inclui o local nome, endereço, número de telefone e o URL para o site da entidade. Quando visualizar as informações de entidade, também tem de utilizar o URL no `webSearchUrl` página que contém a entidade de resultados do campo de ligação para a pesquisa do Bing.

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
> Ou uma linha de terceiros em seu nome, poderá não utilizar, manter, armazenar, colocar em cache, partilha, ou distribuir quaisquer dados a partir da API de entidades para fins de teste, desenvolver, formação, distribuir ou disponibilizar-se de que qualquer serviço de terceiros ou funcionalidade.  

## <a name="data-attribution"></a>Atribuição de dados

Respostas de API do Bing entidade contêm informações de propriedade de terceiros. É responsável para garantir a sua utilização adequada, por exemplo por complying com qualquer licença commons criativos que poderá dependem da sua experiência de utilizador.

Se uma resposta ou o resultado inclui o `contractualRules`, `attributions`, ou `provider` campos, tem de atributos de dados. Se a resposta não inclui qualquer um destes campos, sem atribuição é necessária. Se a resposta inclui o `contractualRules` campo e o `attributions` e/ou `provider` campos, tem de utilizar as regras contractual para os dados de atributos.

O exemplo seguinte mostra uma entidade que inclui uma regra de contractual MediaAttribution e uma imagem que inclua um `provider` campo. A regra de MediaAttribution identifica a imagem como o destino da regra, pelo que pretende ignorar a imagem `provider` campo em vez disso, a regra de MediaAttribution para fornecer a atribuição.  

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

Se uma regra contractual inclui o `targetPropertyName` campo, a regra aplica-se apenas para o campo de destino. Caso contrário, a regra se aplica ao objeto de principal que contém o `contractualRules` campo.

No exemplo seguinte, o `LinkAttribution` regra inclui o `targetPropertyName` campo, pelo que se aplica a regra para o `description` campo. Para as regras que se aplicam a campos específicos, tem de incluir uma linha imediatamente a seguir os dados de destino que contém uma hiperligação para o site do fornecedor. Por exemplo, para a descrição de atributos, inclua uma linha imediatamente a seguir o texto de descrição que contém uma hiperligação para os dados no site do fornecedor, neste caso, criar uma ligação para contoso.com.

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

Se a lista de regras contractual inclui um [LicenseAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#licenseattribution) regra, tem de apresentar o aviso na linha imediatamente a seguir o conteúdo que se aplica a licença. O `LicenseAttribution` regra utiliza o `targetPropertyName` campo para identificar a propriedade que se aplica a licença.

O seguinte mostra um exemplo que inclua um `LicenseAttribution` regra.

![Atribuição de licença](./media/cognitive-services-bing-entities-api/licenseattribution.png)

O aviso de licença que visualiza tem de incluir uma hiperligação para o Web site que contém informações sobre a licença. Normalmente, se o nome da licença de um hyperlink. Por exemplo, se o aviso é **texto com licença de CC por SA** e CC por SA é o nome da licença, iria tornar CC por SA uma hiperligação.

### <a name="link-and-text-attribution"></a>Atribuição de ligação e texto

O [LinkAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#linkattribution) e [TextAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#textattribution) regras são normalmente utilizadas para identificar o fornecedor de dados. O `targetPropertyName` campo identifica o campo que se aplica a regra.

Para os fornecedores de atributos, inclua uma linha imediatamente a seguir o conteúdo que se aplicam a autoria (por exemplo, no campo destino). A linha deve ser claramente com a etiqueta para indicar que os fornecedores são a origem de dados. Por exemplo, "dados a partir de: contoso.com". Para `LinkAttribution` regras, tem de criar uma hiperligação para o site do fornecedor.

O seguinte mostra um exemplo que inclui `LinkAttribution` e `TextAttribution` regras.

![Atribuição de texto de ligação](./media/cognitive-services-bing-entities-api/linktextattribution.png)

### <a name="media-attribution"></a>Atribuição de suporte de dados

Se a entidade inclui uma imagem e apresentá-lo, tem de fornecer uma ligação através de clique para no site do fornecedor. Se a entidade inclui um [MediaAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#mediaattribution) regra, utilize o URL da regra para criar a ligação através de clique. Caso contrário, utilize o URL incluído na imagem do `provider` campo para criar a ligação através de clique.

O seguinte mostra um exemplo que inclui uma imagem `provider` campo e contractual regras. Porque o exemplo inclui a regra contractual, ignorar a imagem `provider` campo e aplique o `MediaAttribution` regra.

![Atribuição de suporte de dados](./media/cognitive-services-bing-entities-api/mediaattribution.png)

### <a name="search-or-search-like-experience"></a>Pesquisa ou experiência semelhante de pesquisa

Tal como com a API de pesquisa do Bing Web, a API de pesquisa do Bing entidade só pode ser utilizada devido a uma consulta do utilizador direta ou a pesquisa ou, como resultado de uma ação dentro de uma aplicação ou a experiência que logicamente pode ser interpretada como pedidos de pesquisa de um utilizador. Para efeitos de ilustração, seguem-se alguns exemplos de pesquisa aceitável ou experiências semelhantes a pesquisa.

- Utilizador introduzir uma consulta diretamente para uma caixa de pesquisa numa aplicação
- O utilizador seleciona um texto específico ou imagem e pedidos "mais informações" ou "informações adicionais"
- Utilizador pede um bot pesquisa sobre um determinado tópico
- Utilizador dwells num objeto específico ou entidade num cenário de tipo de visual de pesquisa

Se não tiver a certeza se a sua experiência pode ser considerada uma experiência semelhante de pesquisa, recomenda-se que verifique se existem com a Microsoft.

## <a name="throttling-requests"></a>Limitar pedidos

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Passos Seguintes

Para começar a trabalhar rapidamente com o seu primeiro pedido, consulte o artigo [tornar o primeiro pedido](./quick-start.md).

Familiarize-se com o [v7 de API de pesquisa do Bing entidade](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) referência. A referência contém os cabeçalhos e os parâmetros de consulta que utiliza para pedir os resultados da pesquisa. Também inclui as definições dos objetos de resposta. 

Para melhorar a sua experiência de utilizador da caixa de pesquisa, consulte o artigo [a API de sugestão automática do Bing](../bing-autosuggest/get-suggested-search-terms.md). Como o utilizador introduz as respetivas termo de consulta, pode chamar esta API para obter os termos de consulta relevantes que foram utilizados por outras pessoas.

Não se esqueça de ler [utilizar do Bing e requisitos de apresentação](./use-display-requirements.md) , de modo não quebrar qualquer uma das regras sobre como utilizar os resultados da pesquisa.