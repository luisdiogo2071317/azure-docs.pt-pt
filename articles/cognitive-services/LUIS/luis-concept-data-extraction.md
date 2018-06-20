---
title: Compreender os conceitos de extração de dados do LUIS - Azure | Microsoft Docs
description: Saiba que tipo de dados pode ser extraído de compreensão de idiomas (LUIS)
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: 28fde09fa9291fbcd64ce4542a008f48dd0018d1
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265257"
---
# <a name="data-extraction"></a>Extração de dados
LUIS dá-lhe a capacidade de obter as informações da utterances de linguagem natural de um utilizador. As informações são extraídas de forma a que pode ser utilizado por um programa, aplicação ou chatbot qualquer ação.

Nas secções seguintes, saiba que dados são devolvidos de entidades com exemplos de JSON e pendentes. Os dados hardest, a extrair são os dados adquiridos por máquina porque não é uma correspondência exata de texto. Extração de dados do aprendidas máquina [entidades](luis-concept-entity-types.md) deve fazer parte do [criação ciclo](luis-concept-app-iteration.md) até que tiver a certeza de que recebe os dados esperados. 

## <a name="data-location-and-key-usage"></a>Utilização de localização e a chave de dados
LUIS fornece os dados da publicado [endpoint](luis-glossary.md#endpoint). O **pedido HTTPS** (POST ou GET) contém o utterance, bem como algumas configurações opcionais, tais como a ambientes de teste ou de produção. 

`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<subscription-key>&verbose=true&timezoneOffset=0&q=book 2 tickets to paris`

O `appID` está disponível na **definições** página do seu LUIS aplicações, bem como parte do URL (depois de `/apps/`) quando estiver a editar essa aplicação LUIS. O `subscription-key` é a chave de ponto final utilizada para consultar a sua aplicação. Apesar de poder utilizar a chave de criação livre/starter. enquanto estiver a aprender LUIS, é importante alterar a chave de subscrição para uma chave que suporte o [esperado utilização LUIS](luis-boundaries.md#key-limits). O `timezoneOffset` unidade é minutos.

O **resposta HTTPS** contém todas as informações de intenção e entidade LUIS pode determinar, com base no modelo publicado atual do ponto final do teste ou de produção. O ponto final do URL for encontrado no [LUIS] [ LUIS] Web site **publicar** página. 

## <a name="data-from-intents"></a>Dados de pendentes
Os dados primários são parte superior da classificação **nome intenção**. Utilizar o `MyStore` [início rápido](luis-quickstart-intents-only.md), a resposta do ponto final é:

```JSON
{
  "query": "when do you open next?",
  "topScoringIntent": {
    "intent": "GetStoreInfo",
    "score": 0.984749258
  },
  "entities": []
}
```

|Objeto de dados|Tipo de Dados|Localização dos Dados|Valor|
|--|--|--|--|
|Objetivo|Cadeia|topScoringIntent.intent|"GetStoreInfo"|

Se a aplicação de chamar LUIS ou chatbot fizer uma decisão com base em mais do que uma pontuação intenção, devolver pontuações dos pendentes, definindo o parâmetro de cadeia de consulta, `verbose=true`. A resposta do ponto final é:

```JSON
{
  "query": "when do you open next?",
  "topScoringIntent": {
    "intent": "GetStoreInfo",
    "score": 0.984749258
  },
  "intents": [
    {
      "intent": "GetStoreInfo",
      "score": 0.984749258
    },
    {
      "intent": "None",
      "score": 0.2040639
    }
  ],
  "entities": []
}
```

Os pendentes são ordenadas do valor mais alto a classificação mais baixa.

|Objeto de dados|Tipo de Dados|Localização dos Dados|Valor|Classificação|
|--|--|--|--|:--|
|Objetivo|Cadeia|.intent pendentes [0]|"GetStoreInfo"|0.984749258|
|Objetivo|Cadeia|.intent pendentes [1]|"None"|0.0168218873|

Se adicionar domínios prebuilt, o nome da intenção indica o domínio, tal como `Utilties` ou `Communication` , bem como o objetivo:

```JSON
{
  "query": "Turn on the lights next monday at 9am",
  "topScoringIntent": {
    "intent": "Utilities.ShowNext",
    "score": 0.07842206
  },
  "intents": [
    {
      "intent": "Utilities.ShowNext",
      "score": 0.07842206
    },
    {
      "intent": "Communication.StartOver",
      "score": 0.0239675418
    },
    {
      "intent": "None",
      "score": 0.0168218873
    }],
  "entities": []
}
```
    
|Domínio|Objeto de dados|Tipo de Dados|Localização dos Dados|Valor|
|--|--|--|--|--|
|Serviços Públicos|Objetivo|Cadeia|.intent pendentes [0]|"<b>Utilitários</b>. ShowNext"|
|Comunicação|Objetivo|Cadeia|.intent pendentes [1]|<b>Comunicação</b>. StartOver"|
||Objetivo|Cadeia|.intent pendentes [2]|"None"|


## <a name="data-from-entities"></a>Dados de entidades
A maioria das aplicações e chatbots tem mais do que o nome de intenção. Estes dados adicionais, optional provém de entidades detetadas no utterance. Cada tipo de entidade devolve diferentes informações sobre a correspondência. 

Uma única palavra ou frase num utterance pode corresponder a mais de uma entidade. Nesse caso, é devolvida cada entidade correspondente com o respetivo modelo de pontuação. 

Todas as entidades são devolvidas no **entidades** matriz da resposta do ponto final:

```JSON
"entities": [
  {
    "entity": "bob jones",
    "type": "Name",
    "startIndex": 0,
    "endIndex": 8,
    "score": 0.473899543
  },
  {
    "entity": "3",
    "type": "builtin.number",
    "startIndex": 16,
    "endIndex": 16,
    "resolution": {
      "value": "3"
    }
  }
]
```

## <a name="tokenized-entity-returned"></a>Entidade com token devolvida
Vários [culturas](luis-supported-languages.md#tokenization) devolver o objeto de entidade com o `entity` valor [atomizada](luis-glossary.md#token). O startIndex e endIndex devolvido pelo LUIS no objeto de entidade não mapeiam para o valor novo, com token, mas em vez disso, para a consulta original para que, a extrair a entidade não processada através de programação. 

Por exemplo, em alemão, a palavra `das Bauernbrot` é atomizada para `das bauern brot`. O valor tokenized, `das bauern brot`, é devolvido e o valor original pode ser determinado através de programação do startIndex e endIndex da consulta original, dando-lhe `das Bauernbrot`.

## <a name="simple-entity-data"></a>Dados de entidade simples

A [entidade simple](luis-concept-entity-types.md) é um valor adquiridos por máquina. Pode ser uma palavra ou expressão. 

`Bob Jones wants 3 meatball pho`

No utterance anterior, `Bob Jones` assinalada como como uma simples `Customer` entidade.

Os dados devolvidos do ponto final incluem o nome da entidade, o texto do utterance detetado, a localização do texto detetado e a classificação:

```JSON
"entities": [
  {
  "entity": "bob jones",
  "type": "Customer",
  "startIndex": 0,
  "endIndex": 8,
  "score": 0.473899543
  }
]
```

|Objeto de dados|Nome da entidade|Valor|
|--|--|--|
|Entidade Simple|"Cliente"|"bob jones"|

## <a name="hierarchical-entity-data"></a>Dados de entidade hierárquica

[Hierárquica](luis-concept-entity-types.md) entidades são aprendidas de máquina e pode incluir uma palavra ou expressão. Elementos subordinados são identificados pelo contexto. Se estiver à procura de uma relação principal-subordinado com correspondência exata de texto, utilize um [lista](#list-entity-data) entidade. 

`book 2 tickets to paris`

No utterance anterior, `paris` assinalada como um `Location::ToLocation` subordinado do `Location` entidade hierárquica. 

Os dados devolvidos do ponto final incluem o nome da entidade e nome de elemento subordinado, o texto do utterance detetado, a localização do texto detetado e a classificação: 

```JSON
"entities": [
  {
    "entity": "paris",
    "type": "Location::ToLocation",
    "startIndex": 18,
    "endIndex": 22,
    "score": 0.6866132
  }
]
```

|Objeto de dados|Principal|Subordinado|Valor|
|--|--|--|--|--|
|Entidade hierárquica|Localização|ToLocation|"paris"|

## <a name="composite-entity-data"></a>Dados de entidade composto
[Composto](luis-concept-entity-types.md) entidades são aprendidas de máquina e pode incluir uma palavra ou expressão. Por exemplo, considere uma entidade composta de prebuilt `number` e `Location::ToLocation` com utterance o seguinte:

`book 2 tickets to paris`

Repare que `2`, o número e `paris`, o ToLocation ter palavras entre os mesmos que não fazem parte de qualquer das entidades. O sublinhado verde, utilizado num utterance identificado no [LUIS] [ LUIS] Web site, indica uma entidade composta.

![Entidade composta](./media/luis-concept-data-extraction/composite-entity.png)

Entidades compostas são devolvidas num `compositeEntities` matriz e todas as entidades dentro de compostos também são devolvidas no `entities` matriz:

```JSON
  "entities": [
    {
      "entity": "paris",
      "type": "Location::ToLocation",
      "startIndex": 18,
      "endIndex": 22,
      "score": 0.956998169
    },
    {
      "entity": "2",
      "type": "builtin.number",
      "startIndex": 5,
      "endIndex": 5,
      "resolution": {
        "value": "2"
      }
    },
    {
      "entity": "2 tickets to paris",
      "type": "Order",
      "startIndex": 5,
      "endIndex": 22,
      "score": 0.7714499
    }
  ],
  "compositeEntities": [
    {
      "parentType": "Order",
      "value": "2 tickets to paris",
      "children": [
        {
          "type": "builtin.number",
          "value": "2"
        },
        {
          "type": "Location::ToLocation",
          "value": "paris"
        }
      ]
    }
  ]
```    

|Objeto de dados|Nome da entidade|Valor|
|--|--|--|
|Entidade prebuilt - número|"builtin.number"|"2"|
|Entidade hierárquica - localização|"Location::ToLocation"|"paris"|

## <a name="list-entity-data"></a>Dados de entidades de lista

A [lista](luis-concept-entity-types.md) entidade não é aprendidas máquina. É uma correspondência exata de texto. Uma lista representa os itens na lista, juntamente com os sinónimos para esses itens. LUIS marca qualquer correspondência para um item na lista de quaisquer como uma entidade na resposta. Pode ser um sinónimo na lista de mais do que um. 

Suponha que a aplicação tem uma lista, com o nome `Cities`, que permite variações dos nomes de cidade incluindo cidade de airport (Sea tac), o código de airport (SEA), o código postal de zip (98101) e o indicativo de telefone (206). 

|Item da lista|Item sinónimos|
|---|---|
|Seattle|SEA tac, sea, 98101, 206, + 1 |
|Paris|cdg, roissy, ory, 75001, 1, +33|

`book 2 tickets to paris`

No utterance anterior, a palavra `paris` está mapeado para o item de paris como parte do `Cities` lista entidade. A entidade de lista corresponde de nome normalizado do item, bem como os sinónimos do item. 

```JSON
"entities": [
  {
    "entity": "paris",
    "type": "Cities",
    "startIndex": 18,
    "endIndex": 22,
    "resolution": {
      "values": [
        "Paris"
      ]
    }
  }
]
```

Outro utterance de exemplo, utilizando um sinónimo para Paris:

`book 2 tickets to roissy`

```JSON
"entities": [
  {
    "entity": "roissy",
    "type": "Cities",
    "startIndex": 18,
    "endIndex": 23,
    "resolution": {
      "values": [
        "Paris"
      ]
    }
  }
]
```

## <a name="prebuilt-entity-data"></a>Dados de entidade prebuilt
[Prebuilt](luis-concept-entity-types.md) entidades são detetadas com base numa correspondência de expressão regular com a open source [Recognizers texto](https://github.com/Microsoft/Recognizers-Text) projeto. Entidades prebuilt são devolvidas na matriz de entidades e utilize o nome de tipo com o prefixo `builtin::`. O texto seguinte é um utterance de exemplo com as entidades prebuilt devolvidas:

`Dec 5th send to +1 360-555-1212`

```JSON
"entities": [
    {
      "entity": "dec 5th",
      "type": "builtin.datetimeV2.date",
      "startIndex": 0,
      "endIndex": 6,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-12-05",
            "type": "date",
            "value": "2017-12-05"
          },
          {
            "timex": "XXXX-12-05",
            "type": "date",
            "value": "2018-12-05"
          }
        ]
      }
    },
    {
      "entity": "1",
      "type": "builtin.number",
      "startIndex": 18,
      "endIndex": 18,
      "resolution": {
        "value": "1"
      }
    },
    {
      "entity": "360",
      "type": "builtin.number",
      "startIndex": 20,
      "endIndex": 22,
      "resolution": {
        "value": "360"
      }
    },
    {
      "entity": "555",
      "type": "builtin.number",
      "startIndex": 26,
      "endIndex": 28,
      "resolution": {
        "value": "555"
      }
    },
    {
      "entity": "1212",
      "type": "builtin.number",
      "startIndex": 32,
      "endIndex": 35,
      "resolution": {
        "value": "1212"
      }
    },
    {
      "entity": "5th",
      "type": "builtin.ordinal",
      "startIndex": 4,
      "endIndex": 6,
      "resolution": {
        "value": "5"
      }
    },
    {
      "entity": "1 360 - 555 - 1212",
      "type": "builtin.phonenumber",
      "startIndex": 18,
      "endIndex": 35,
      "resolution": {
        "value": "1 360 - 555 - 1212"
      }
    }
  ]
``` 

## <a name="regular-expression-entity-data"></a>Dados de entidades de expressão regular
[Expressão regular](luis-concept-entity-types.md) entidades são detetadas com base numa correspondência de expressão regular a utilização de uma expressão que fornece ao criar a entidade. Quando utilizar `kb[0-9]{6}` como a definição de entidade de expressão regular, a seguinte resposta JSON é uma utterance de exemplo com as entidades de expressão regular devolvido para a consulta `When was kb123456 published?`:

```JSON
{
  "query": "when was kb123456 published?",
  "topScoringIntent": {
    "intent": "FindKBArticle",
    "score": 0.933641255
  },
  "intents": [
    {
      "intent": "FindKBArticle",
      "score": 0.933641255
    },
    {
      "intent": "None",
      "score": 0.04397359
    }
  ],
  "entities": [
    {
      "entity": "kb123456",
      "type": "KB number",
      "startIndex": 9,
      "endIndex": 16
    }
  ]
}
```

## <a name="extracting-names"></a>Nomes de extração
É difícil obter nomes de um utterance porque um nome de pode ter praticamente qualquer combinação de letras e palavras. Consoante o tipo de nome está a extrair, tem várias opções. Não são regras mas mais diretrizes. 

### <a name="names-of-people"></a>Nomes das pessoas
Nome das pessoas pode ter algumas ligeiras formato consoante o idioma e cultura. Utilize uma entidade de hierárquica com nomes primeiro e últimos como elementos subordinados ou uma entidade simple com funções do nome próprio e apelido. Certifique-se de que lhe exemplos que utilizam o nome próprio e apelido em diferentes partes de utterance, utterances de comprimentos diferentes e utterances em todos os pendentes, incluindo a intenção. [Reveja](label-suggested-utterances.md) utterances de ponto final regularmente Etiquetar todos os nomes de que não foram corretamente prever. 

### <a name="names-of-places"></a>Nomes dos locais
Os nomes de localização são definidos e conhecidos como cidades, counties, Estados, provinces e países. Se a sua aplicação utiliza um conjunto de know das localizações, considere uma entidade de lista. Se precisar de localizar a que colocar todos os nomes, criar uma entidade simple e fornecer uma variedade de exemplos. Adicione uma lista de expressão de nomes local para impor que local nomes aspeto na sua aplicação. [Reveja](label-suggested-utterances.md) utterances de ponto final regularmente Etiquetar todos os nomes de que não foram corretamente prever. 

### <a name="new-and-emerging-names"></a>Nomes de novo e emergentes
Algumas aplicações necessitam de ser capazes de localizar os nomes de novo e emergentes, como produtos ou as empresas. Este é o tipo mais difícil de extração de dados. Começar com uma entidade simple e adicionam uma lista de expressão. [Reveja](label-suggested-utterances.md) utterances de ponto final regularmente Etiquetar todos os nomes de que não foram corretamente prever. 

## <a name="pattern-roles-data"></a>Dados de funções padrão
As funções são a nível contextual das diferenças de entidades. 

```JSON
{
  "query": "move bob jones from seattle to redmond",
  "topScoringIntent": {
    "intent": "MoveAssetsOrPeople",
    "score": 0.9999998
  },
  "intents": [
    {
      "intent": "MoveAssetsOrPeople",
      "score": 0.9999998
    },
    {
      "intent": "None",
      "score": 1.02040713E-06
    },
    {
      "intent": "GetEmployeeBenefits",
      "score": 6.12244548E-07
    },
    {
      "intent": "GetEmployeeOrgChart",
      "score": 6.12244548E-07
    },
    {
      "intent": "FindForm",
      "score": 1.1E-09
    }
  ],
  "entities": [
    {
      "entity": "bob jones",
      "type": "Employee",
      "startIndex": 5,
      "endIndex": 13,
      "score": 0.922820568,
      "role": ""
    },
    {
      "entity": "seattle",
      "type": "Location",
      "startIndex": 20,
      "endIndex": 26,
      "score": 0.948008537,
      "role": "Origin"
    },
    {
      "entity": "redmond",
      "type": "Location",
      "startIndex": 31,
      "endIndex": 37,
      "score": 0.7047979,
      "role": "Destination"
    }
  ]
}
```

## <a name="patternany-entity-data"></a>Dados de entidade Pattern.any
Pattern.any entidades são entidades de comprimento variável utilizadas na utterances de modelo de um [padrão](luis-concept-patterns.md). 

```JSON
{
  "query": "where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?",
  "topScoringIntent": {
    "intent": "FindForm",
    "score": 0.999999464
  },
  "intents": [
    {
      "intent": "FindForm",
      "score": 0.999999464
    },
    {
      "intent": "GetEmployeeBenefits",
      "score": 4.883697E-06
    },
    {
      "intent": "None",
      "score": 1.02040713E-06
    },
    {
      "intent": "GetEmployeeOrgChart",
      "score": 9.278342E-07
    },
    {
      "intent": "MoveAssetsOrPeople",
      "score": 9.278342E-07
    }
  ],
  "entities": [
    {
      "entity": "understand your responsibilities as a member of the community",
      "type": "FormName",
      "startIndex": 18,
      "endIndex": 78,
      "role": ""
    }
  ]
}
```


## <a name="sentiment-analysis"></a>Análise de sentimentos
Se a análise de dados de sentimento estiver configurada, a resposta de json LUIS inclui a análise de dados de sentimento. Saiba mais sobre a análise de dados de sentimento no [análise de texto](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) documentação.

### <a name="sentiment-data"></a>Dados de sentimento
Dados de sentimento são uma pontuação entre 1 e 0 que indica o positivo (próximo de 1) ou negativa (próximo de 0) sentimento dos dados.

Quando estiver a cultura `en-us`, a resposta é:

```JSON
"sentimentAnalysis": {
  "label": "positive",
  "score": 0.9163064
}
```

Para todos os outras culturas, a resposta é:

```JSON
"sentimentAnalysis": {
  "score": 0.9163064
}
```


### <a name="key-phrase-extraction-entity-data"></a>Dados de expressão de chave de entidade de extração
A entidade de extração chave frase devolve expressões chaves utterance, fornecido pelo [análise de texto](https://docs.microsoft.com/azure/cognitive-services/text-analytics/).

<!-- TBD: verify JSON-->
```JSON
"keyPhrases": [
    "places",
    "beautiful views",
    "favorite trail"
]
```

## <a name="data-matching-multiple-entities"></a>Várias entidades de correspondência de dados
LUIS devolve todas as entidades detetadas no utterance. Como resultado, o seu chatbot poderá ter de tomar decisão baseada nos resultados. Um utterance pode ter várias entidades num utterance:

`book me 2 adult business tickets to paris tomorrow on air france`

O ponto final LUIS pode detetar os mesmos dados em entidades diferentes: 

```JSON
{
  "query": "book me 2 adult business tickets to paris tomorrow on air france",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 1.0
  },
  "intents": [
    {
      "intent": "BookFlight",
      "score": 1.0
    },
    {
      "intent": "Concierge",
      "score": 0.04216196
    },
    {
      "intent": "None",
      "score": 0.03610297
    }
  ],
  "entities": [
    {
      "entity": "air france",
      "type": "Airline",
      "startIndex": 54,
      "endIndex": 63,
      "score": 0.8291798
    },
    {
      "entity": "adult",
      "type": "Category",
      "startIndex": 10,
      "endIndex": 14,
      "resolution": {
        "values": [
          "adult"
        ]
      }
    },
    {
      "entity": "paris",
      "type": "Cities",
      "startIndex": 36,
      "endIndex": 40,
      "resolution": {
        "values": [
          "Paris"
        ]
      }
    },
    {
      "entity": "tomorrow",
      "type": "builtin.datetimeV2.date",
      "startIndex": 42,
      "endIndex": 49,
      "resolution": {
        "values": [
          {
            "timex": "2018-02-21",
            "type": "date",
            "value": "2018-02-21"
          }
        ]
      }
    },
    {
      "entity": "paris",
      "type": "Location::ToLocation",
      "startIndex": 36,
      "endIndex": 40,
      "score": 0.9730773
    },
    {
      "entity": "2",
      "type": "builtin.number",
      "startIndex": 8,
      "endIndex": 8,
      "resolution": {
        "value": "2"
      }
    },
    {
      "entity": "business",
      "type": "Seat",
      "startIndex": 16,
      "endIndex": 23,
      "resolution": {
        "values": [
          "business"
        ]
      }
    },
    {
      "entity": "2 adult business",
      "type": "TicketSeatOrder",
      "startIndex": 8,
      "endIndex": 23,
      "score": 0.8784727
    }
  ],
  "compositeEntities": [
    {
      "parentType": "TicketSeatOrder",
      "value": "2 adult business",
      "children": [
        {
          "type": "Category",
          "value": "adult"
        },
        {
          "type": "builtin.number",
          "value": "2"
        },
        {
          "type": "Seat",
          "value": "business"
        }
      ]
    }
  ]
}
```

## <a name="next-steps"></a>Passos Seguintes

Consulte [adicionar entidades](luis-how-to-add-entities.md) para saber mais sobre como adicionar entidades à sua aplicação LUIS.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions