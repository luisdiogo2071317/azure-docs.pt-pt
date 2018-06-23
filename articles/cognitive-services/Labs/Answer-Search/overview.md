---
title: Descrição geral de pesquisa de resposta - serviços cognitivos Microsoft do projeto | Microsoft Docs
description: Introdução à pesquisa de resposta de projeto.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: d87cf1390970d2c815b94bcaee7e07c19bc03cce
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354116"
---
# <a name="what-is-project-answer-search"></a>O que é a pesquisa de resposta de projeto?
API de pesquisa de resposta de projeto utiliza o ponto final do Bing v7 para obter respostas a interrogative consultas. A pergunta, tal como "Qual é o circumference terrestre?" Devolve uma resposta com informações factual.  Uma consulta para uma pessoa, local ou coisa devolve informações sobre a entidade identificada pela consulta. Estes cenários podem ser úteis para aplicações, tais como bots conversational, mensagens de aplicações, leitores, etc.  

As respostas que dependem do cenário de consulta de retorno de consultas: páginas Web é sempre devolvido, enquanto [factos](fact-queries.md) e/ou [entidades](entity-queries.md) são devolvidos se forem relevantes.

## <a name="endpoint"></a>Ponto Final
Para obter respostas a uma pergunta ou informações sobre uma pessoa, local ou coisa, envie um pedido para o ponto final da API de pesquisa de resposta. Utilize os cabeçalhos e os parâmetros de URL para vários especificações.  Incluir *Ocp-Apim-Subscription-Key* cabeçalho com um token válido.  É necessário o parâmetro de mercado. Apenas `en-us` mercado é atualmente suportado.

A consulta seguinte obtém as respostas à pergunta: "O que é o circumference terrestre?"

GET:
````
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=what+is+circumference+of+the=earth?&mkt=en-us

````

O parâmetro de URL `q=` é necessário especificar o objeto da pesquisa.

## <a name="response-object"></a>Objeto de resposta

A resposta inclui HTTP cabeçalhos, páginas Web, factos e/ou entidades.

````
BingAPIs-TraceId: AB2E75C998614ADB8EBF5110DF648298
X-MSEdge-ClientID: 1E48FC4F7B8768C80B14F7997A106906
BingAPIs-SessionId: 0504DDD6DAE84861A4842306F8DA7A58
BingAPIs-Market: en-US
X-MSEdge-Ref: Ref A: AB2E75C998614ADB8EBF5110DF648298 Ref B: CO1EDGE0322 Ref C: 2018-04-19T19:57:13Z

JSON Response:

{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "what is the circumference of earth"
  },
  "webPages": {
    "webSearchUrl": "https://www.bing.com/search?q\u003dwhat+is+the+circumference+of+earth",
    "totalEstimatedMatches": 217000,
    "value": [
      {
        "id": "https://www.bingapis.com/api/v7/#WebPages.0",
        "name": "Circumference of the Earth - Universe Today",
        "url": "https://www.universetoday.com/26461/circumference-of-the-earth/",
        "isFamilyFriendly": true,
        "displayUrl": "https://www.universetoday.com/26461/circumference-of-the-earth",
        "snippet": "The circumference of the Earth in kilometers is 40,075 km, and the circumference of the Earth in miles is 24,901. In other words, if you could drive your car around the equator of the Earth (yes, even over the oceans), youâ€™d put on an extra 40,075 km on the odometer.",
        "deepLinks": [
          {
            "name": "About Earth",
            "url": "https://www.universetoday.com/14382/10-interesting-facts-about-planet-earth/"
          }
        ],
        "dateLastCrawled": "2018-04-12T14:13:00.0000000Z",
        "language": "en"
      },
      {
        "id": "https://www.bingapis.com/api/v7/#WebPages.1",
        "name": "Earth - Wikipedia",
        "url": "https://en.wikipedia.org/wiki/Earth",
        "about": [
          {
            "name": "Earth"
          },
          {
            "name": "Earth"
          }
        ],
        "isFamilyFriendly": true,
        "displayUrl": "https://en.wikipedia.org/wiki/Earth",
        "snippet": "Circumference: 40 075.017 km equatorial (24 901.461 mi) ... Earth is the third planet from the Sun and the only object in the Universe known to harbor life.",
        "deepLinks": [
          {
            "name": "Moon",
            "url": "https://en.wikipedia.org/wiki/Moon"
          },
          {
            "name": "Planet",
            "url": "https://en.wikipedia.org/wiki/Planet"
          },
          {
            "name": "Quasi-Satellites",
            "url": "https://en.wikipedia.org/wiki/Quasi-satellite"
          },
          {
            "name": "World Population",
            "url": "https://en.wikipedia.org/wiki/World_population"
          },
   . . .

    ]
  },
  "entities": {
    "value": [
      {
        "id": "https://www.bingapis.com/api/v7/#Entities.0",
        "contractualRules": [
          {
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
            "text": "Wikipedia",
            "url": "http://en.wikipedia.org/wiki/Earth"
          },
          {
            "_type": "ContractualRules/MediaAttribution",
            "targetPropertyName": "image",
            "mustBeCloseToContent": true,
            "url": "http://en.wikipedia.org/wiki/Earth"
          }
        ],
        "webSearchUrl": "https://www.bing.com/entityexplore?q\u003dEarth\u0026filters\u003dsid:%226ddb3372-4801-5567-321e-e8a53bd774a4%22\u0026elv\u003dAXXfrEiqqD9r3GuelwApulpmymQx!ODfuQu*veOQHkvP0!Zbvi5F5tVcMSDJvDEWiQWwrdueYTtIszgj03oFQHykYYLYgq3q5!Sf00QxXGIS",
        "name": "Earth",
        "image": {
          "name": "Earth",
          "thumbnailUrl": "https://www.bing.com/th?id\u003dA3ab623665ab412f386c162bd29f0683a\u0026w\u003d110\u0026h\u003d110\u0026c\u003d7\u0026rs\u003d1\u0026qlt\u003d80\u0026cdv\u003d1\u0026pid\u003d16.1",
          "provider": [
            {
              "_type": "Organization",
              "url": "http://en.wikipedia.org/wiki/Earth"
            }
          ],
          "hostPageUrl": "http://upload.wikimedia.org/wikipedia/commons/9/97/The_Earth_seen_from_Apollo_17.jpg",
          "width": 110,
          "height": 110,
          "sourceWidth": 799,
          "sourceHeight": 800
        },
        "description": "Earth is the third planet from the Sun and the only object in the Universe known to harbor life. According to radiometric dating and other sources of evidence, Earth formed over 4.5 billion years ago. Earth\u0027s gravity interacts with other objects in space, especially the Sun and the Moon, Earth\u0027s only natural satellite. Earth revolves around the Sun in 365.26 days, a period known as an Earth year. During this time, Earth rotates about its axis about 366.26 times.",
        "entityPresentationInfo": {
          "entityScenario": "DominantEntity",
          "entityTypeHints": [
            "Generic"
          ]
        },
        "bingId": "6ddb3372-4801-5567-321e-e8a53bd774a4"
      }
    ]
  },
  "facts": {
    "id": "https://www.bingapis.com/api/v7/#Facts",
    "contractualRules": [
      {
        "_type": "ContractualRules/LinkAttribution",
        "text": "www.universetoday.com/26461/circumference-of-the-earth/",
        "url": "http://www.universetoday.com/26461/circumference-of-the-earth/"
      }
    ],
    "attributions": [
      {
        "providerDisplayName": "www.universetoday.com/26461/circumference-of-the-earth/",
        "seeMoreUrl": "http://www.universetoday.com/26461/circumference-of-the-earth/"
      }
    ],
    "value": [
      {
        "description": "The circumference of the Earth in kilometers is 40,075 km, and the circumference of the Earth in miles is 24,901. In other words, if you could drive your car around the equator of the Earth (yes, even over the oceans), youâ€™d put on an extra 40,075 km on the odometer.",
        "subjectName": ""
      }
    ]
  },
  "rankingResponse": {
    "mainline": {
      "items": [
        {
          "answerType": "Facts",
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Facts"
          }
        },
        {
          "answerType": "WebPages",
          "resultIndex": 0,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#WebPages.0"
          }
        },
        {
          "answerType": "WebPages",
          "resultIndex": 1,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#WebPages.1"
          }
        },
        {
          "answerType": "WebPages",
          "resultIndex": 2,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#WebPages.2"
          }
        },
        
        . . . 
      ]
    },
    "sidebar": {
      "items": [
        {
          "answerType": "Entities",
          "resultIndex": 0,
          "value": {
            "id": "https://www.bingapis.com/api/v7/#Entities.0"
          }
        }
      ]
    }
  }
}

````

## <a name="terms-of-use"></a>Termos de utilização
Pesquisa de resposta do projeto e as tendências de vídeo do projeto estão sujeitas ao [utilizar de pesquisa do Bing e requisitos de apresentação](use-display-requirements.md).

Ou uma linha de terceiros em seu nome, poderá não utilizar, manter, armazenar, colocar em cache, partilha, ou distribuir quaisquer dados a partir da API de pré-visualização do URL para fins de teste, desenvolver, formação, distribuir ou disponibilizar-se de que qualquer serviço de terceiros ou funcionalidade. 

## <a name="throttling-requests"></a>Limitar pedidos

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]


## <a name="data-attribution"></a>Atribuição de dados  

Respostas de pesquisa de resposta de projeto contêm informações de propriedade de terceiros. É responsável para garantir a sua utilização adequada, por exemplo por complying com qualquer licença commons criativos que poderá dependem da sua experiência de utilizador.  
  
Se uma resposta ou o resultado inclui o `contractualRules`, `attributions`, ou `provider` campos, tem de atributos de dados. Se a resposta não inclui qualquer um destes campos, sem atribuição é necessária. Se a resposta inclui o `contractualRules` campo e o `attributions` e/ou `provider` campos, tem de utilizar as regras contractual para os dados de atributos.  
  
O exemplo seguinte mostra uma entidade que inclui uma regra de contractual MediaAttribution e uma imagem que inclua um `provider` campo. A regra de MediaAttribution identifica a imagem como o destino da regra, pelo que pretende ignorar a imagem `provider` campo em vez disso, a regra de MediaAttribution para fornecer a atribuição.  
  
```  
        "value" : [{
            "contractualRules" : [
                . . .
                {
                    "_type" : "ContractualRules\/MediaAttribution",
                    "targetPropertyName" : "image",
                    "mustBeCloseToContent" : true,
                    "url" : "http:\/\/en.wikipedia.org\/wiki\/Space_Needle"
                }
            ],
            . . .
            "image" : {
                "name" : "Space Needle",
                "thumbnailUrl" : "https:\/\/www.bing.com\/th?id=A46378861201...",
                "provider" : [{
                    "_type" : "Organization",
                    "url" : "http:\/\/en.wikipedia.org\/wiki\/Space_Needle"
                }],
                "hostPageUrl" : "http:\/\/www.citydictionary.com\/Uploaded...",
                "width" : 110,
                "height" : 110
            },
            . . .
        }]
```  
  
Se uma regra contractual inclui o `targetPropertyName` campo, a regra aplica-se apenas para o campo de destino. Caso contrário, a regra se aplica ao objeto de principal que contém o `contractualRules` campo.  
  
  
No exemplo seguinte, o `LinkAttribution` regra inclui o `targetPropertyName` campo, pelo que se aplica a regra para o `description` campo. Para as regras que se aplicam a campos específicos, tem de incluir uma linha imediatamente a seguir os dados de destino que contém uma hiperligação para o site do fornecedor. Por exemplo, para a descrição de atributos, inclua uma linha imediatamente a seguir o texto de descrição que contém uma hiperligação para os dados no site do fornecedor, neste caso, criar uma ligação para en.wikipedia.org.  
  
```  
"entities" : {  
    "value" : [{  
            . . .  
            "description" : "Peyton Williams Manning is a former American....",  
            . . .  
            "contractualRules" : [{  
                    "_type" : "ContractualRules\/LinkAttribution",  
                    "targetPropertyName" : "description",  
                    "mustBeCloseToContent" : true,  
                    "text" : "en.wikipedia.org",  
                    "url" : "http:\/\/www.bing.com\/cr?IG=B8AD73..."  
                 },  
            . . .  
  
```  

### <a name="license-attribution"></a>Atribuição de licença  

Se a lista de regras contractual inclui um [LicenseAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#licenseattribution) regra, tem de apresentar o aviso na linha imediatamente a seguir o conteúdo que se aplica a licença. O `LicenseAttribution` regra utiliza o `targetPropertyName` campo para identificar a propriedade que se aplica a licença.  
  
O seguinte mostra um exemplo que inclua um `LicenseAttribution` regra.  
  
![Atribuição de licença](./media/licenseattribution.png)  
  
O aviso de licença que visualiza tem de incluir uma hiperligação para o Web site que contém informações sobre a licença. Normalmente, se o nome da licença de um hyperlink. Por exemplo, se o aviso é **texto com licença de CC por SA** e CC por SA é o nome da licença, iria tornar CC por SA uma hiperligação.  
  
### <a name="link-and-text-attribution"></a>Ligação e a atribuição de texto  

O [LinkAttribution](reference.md#linkattribution) e [TextAttribution](reference.md#textattribution) regras são normalmente utilizadas para identificar o fornecedor de dados. O `targetPropertyName` campo identifica o campo que se aplica a regra.  
  
Para os fornecedores de atributos, inclua uma linha imediatamente a seguir o conteúdo que se aplicam a autoria (por exemplo, no campo destino). A linha deve ser claramente com a etiqueta para indicar que os fornecedores são a origem de dados. Por exemplo, "dados a partir de: en.wikipedia.org". Para `LinkAttribution` regras, tem de criar uma hiperligação para o site do fornecedor.  
  
O seguinte mostra um exemplo que inclui `LinkAttribution` e `TextAttribution` regras.  
  
![Atribuição de texto de ligação](./media/linktextattribution.png)  

### <a name="media-attribution"></a>Atribuição de suporte de dados  

Se a entidade inclui uma imagem e apresentá-lo, tem de fornecer uma ligação através de clique para no site do fornecedor. Se a entidade inclui um [MediaAttribution](reference.md#mediaattribution) regra, utilize o URL da regra para criar a ligação através de clique. Caso contrário, utilize o URL incluído na imagem do `provider` campo para criar a ligação através de clique.  
  
O seguinte mostra um exemplo que inclui uma imagem `provider` campo e contractual regras. Porque o exemplo inclui a regra contractual, irão ignorar a imagem `provider` campo e aplique o `MediaAttribution` regra.  
  
![Atribuição de suporte de dados](./media/mediaattribution.png)  

## <a name="next-steps"></a>Passos Seguintes
- [Início rápido c#](c-sharp-quickstart.md)
- [Guia de introdução do Java](java-quickstart.md)
- [Guia de introdução do nó](node-quickstart.md)
- [Guia de introdução do Python](python-quickstart.md)
