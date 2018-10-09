---
title: 'Início rápido: Consulta de entidade de pesquisa de resposta de projeto'
titlesuffix: Azure Cognitive Services
description: Consultas para entidades com a pesquisa de resposta de projeto
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: project-answer-search
ms.topic: article
ms.date: 04/16/2018
ms.author: rosh
ms.openlocfilehash: efb46fc7064bcad69b5ea84f9bdfe923d95ccbe6
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867583"
---
# <a name="quickstart-query-for-entities"></a>Início rápido: Consultar entidades

Se a consulta solicita as informações sobre uma pessoa, local ou coisa, a resposta pode conter um `entities` resposta.  Páginas da Web, as consultas retornam sempre [fatos](fact-queries.md) e/ou [entidades](entity-queries.md) são consulta dependentes.

Entidades de oferecer suporte a três cenários de consulta: 
-   DominantEntity — há apenas uma entidade que corresponde à consulta e a intenção do utilizador. Por exemplo, a consulta, o Space Needle, é um cenário de DominantEntity. 
-   Desambiguação — há mais de uma entidade que corresponde à consulta e a intenção do usuário e cabe ao utilizador selecionar a entidade correta. Por exemplo, a consulta de jogo de Thrones é um cenário de Desambiguação que retorna o programa de televisão e a série de livros. 
-   Lista — existem várias entidades que correspondem à consulta e a intenção do utilizador. Por exemplo, a consulta "Lista de endangered species" é um cenário de lista que retorna valores de tabela formatadas para apresentar em linhas e células. 
 
Para determinar o cenário de consulta, utilize o `queryScenario` campo o `entities` objeto. Os dados que inclui a entidade dependem do tipo da entidade. Embora as entidades incluem as mesmas informações básicas, algumas entidades como tourist attractions ou livros incluem propriedades adicionais. As entidades que incluem propriedades adicionais incluem a `_type` campo que contém uma sugestão utilizada pelo serializador. As seguintes entidades incluem propriedades adicionais: 
-   Livro 
-   MusicRecording 
-   Pessoa 
-   Attraction 
 
Para determinar o tipo de entidade que contém a resposta, utilize o `entityTypeHints` campo conforme mostrado na consulta para Bill Gates.
````
        },
        "description": "Bill Gates is an American business man and philanthropist, co-founder of Microsoft",
        "entityPresentationInfo": {
          "entityScenario": "DominantEntity",
          "entityTypeHints": [
            "Person"
          ]
        },
        "bingId": "6d7d66a7-2cb8-0ae9-637c-f81fd749dc9a"
      }
````
Segue-se uma consulta para o Space Needle:
````
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=space+needle&mkt=en-us
````
A resposta inclui a `entities` resposta. Tenha em atenção a `entityScenario` e `entityTypeHints` campos. 
````
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
            "url": "http://en.wikipedia.org/wiki/Space_Needle"
          },
          {
            "_type": "ContractualRules/MediaAttribution",
            "targetPropertyName": "image",
            "mustBeCloseToContent": true,
            "url": "http://en.wikipedia.org/wiki/Space_Needle"
          }
        ],
        "webSearchUrl": "https://www.bing.com/entityexplore?q\u003dSpace+Needle\u0026filters\u003dsid:%22f8dd5b08-206d-2554-6e4a-893f51f4de7e%22\u0026elv\u003dAXXfrEiqqD9r3GuelwApulpmymQx!ODfuQu*veOQHkvP0!Zbvi5F5tVcMSDJvDEWiQWwrdueYTtIszgj03oFQHykYYLYgq3q5!Sf00QxXGIS",
        "name": "Space Needle",
        "image": {
          "name": "Space Needle",
          "thumbnailUrl": "https://www.bing.com/th?id\u003dA15d336cf119b9b5c7e0ab37e271421d3\u0026w\u003d110\u0026h\u003d110\u0026c\u003d7\u0026rs\u003d1\u0026qlt\u003d80\u0026cdv\u003d1\u0026pid\u003d16.1",
          "provider": [
            {
              "_type": "Organization",
              "url": "http://en.wikipedia.org/wiki/Space_Needle"
            }
          ],
          "hostPageUrl": "http://upload.wikimedia.org/wikipedia/commons/2/23/Space_Needle_2011-07-04.jpg",
          "width": 110,
          "height": 110,
          "sourceWidth": 152,
          "sourceHeight": 300
        },
        "description": "The Space Needle is an observation tower in Seattle, Washington, a landmark of the Pacific Northwest, and an icon of Seattle. It was built in the Seattle Center for the 1962 World\u0027s Fair, which drew over 2.3 million visitors, when nearly 20,000 people a day used its elevators.",
        "entityPresentationInfo": {
          "entityScenario": "DominantEntity",
          "entityTypeHints": [
            "Attraction"
          ]
        },
        "bingId": "f8dd5b08-206d-2554-6e4a-893f51f4de7e"
      }
    ]
  },
````

Uma consulta pode devolver uma lista, se é relevante.

**Consulta:** a consulta seguinte localiza uma lista de endangered espécie:

````
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=list+of+endangered+species

````

**Resposta:** a resposta inclui uma lista formatada para apresentar como valores de tabela:
````
  "facts": {
    "id": "https://www.bingapis.com/api/v7/#Facts",
    "contractualRules": [
      {
        "_type": "ContractualRules/LinkAttribution",
        "text": "www.worldwildlife.org/species/directory?direction=desc&sort=e…",
        "url": "https://www.worldwildlife.org/species/directory?direction=desc&sort=extinction_status"
      }
    ],
    "attributions": [
      {
        "providerDisplayName": "www.worldwildlife.org/species/directory?direction=desc&sort=e…",
        "seeMoreUrl": "https://www.worldwildlife.org/species/directory?direction=desc&sort=extinction_status"
      }
    ],
    "value": [
      {
        "subjectName": "Species Directory",
        "richCaption": {
          "_type": "StructuredValue/TabularData",
          "header": [
            "Common name",
            "Scientific name",
            "Conservation status ↓"
          ],
          "rows": [
            {
              "cells": [
                {
                  "_type": "Properties/Link",
                  "text": "Amur Leopard",
                  "url": "https://www.bing.com/https//www.worldwildlife.org/species/amur-leopard"
                },
                {
                  "text": "Panthera pardus orientalis"
                },
                {
                  "text": "Critically Endangered"
                }
              ]
            },
            {
              "cells": [
                {
                  "_type": "Properties/Link",
                  "text": "Black Rhino",
                  "url": "https://www.bing.com/https//www.worldwildlife.org/species/black-rhino"
                },
                {
                  "text": "Diceros bicornis"
                },
                {
                  "text": "Critically Endangered"
                }
              ]
            },
            {
              "cells": [
                {
                  "_type": "Properties/Link",
                  "text": "Bornean Orangutan",
                  "url": "https://www.bing.com/https//www.worldwildlife.org/species/bornean-orangutan"
                },
                {
                  "text": "Pongo pygmaeus"
                },
                {
                  "text": "Critically Endangered"
                }
              ]
            },
            {
              "cells": [
                {
                  "_type": "Properties/Link",
                  "text": "Cross River Gorilla",
                  "url": "https://www.bing.com/https//www.worldwildlife.org/species/cross-river-gorilla"
                },
                {
                  "text": "Gorilla gorilla diehli"
                },
                {
                  "text": "Critically Endangered"
                }
              ]
            }
          ],
          "seeMoreUrl": {
            "text": "46 more rows",
            "url": "https://www.worldwildlife.org/species/directory?direction=desc&sort=extinction_status"
          }
        }
      }
    ]
  },

````


## <a name="next-steps"></a>Passos Seguintes
- [Início rápido em C#](c-sharp-quickstart.md)
- [Início rápido de Java](java-quickstart.md)
- [Guia de introdução do nó](node-quickstart.md)
- [Guia de introdução do Python](python-quickstart.md)