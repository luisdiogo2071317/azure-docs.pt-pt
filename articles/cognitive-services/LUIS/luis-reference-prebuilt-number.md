---
title: Entidade pré-criados numérica
titleSuffix: Azure
description: Este artigo contém informações de entidade pré-criados numéricas na compreensão de idiomas (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: ead544400f97f9a49296cc43552bea3a6f3def98
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53095324"
---
# <a name="number-entity"></a>Entidade de número
Existem várias formas em que os valores numéricos são utilizados para quantificar, express além de descrever partes de informações. Este artigo abrange apenas alguns dos exemplos possíveis. LUIS interpreta as variações em expressões de utilizador e devolve valores numéricos consistentes. Uma vez que já está preparada esta entidade, não é necessário adicionar expressões de exemplo que contém um número para os objetivos do aplicativo. 

## <a name="types-of-number"></a>Tipos de número
Número é gerido a partir da [reconhecedores texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml) repositório do Github

## <a name="examples-of-number-resolution"></a>Exemplos de resolução de número

| Expressão        | Entidade   | Resolução |
| ------------- |:----------------:| --------------:|
| ```one thousand times```  | ```"one thousand"``` |   ```"1000"```      | 
| ```1,000 people```        | ```"1,000"```    |   ```"1000"```      |
| ```1/2 cup```         | ```"1 / 2"```    |    ```"0.5"```      |
|  ```one half the amount```     | ```"one half"```     |    ```"0.5"```      |
| ```one hundred fifty orders``` | ```"one hundred fifty"``` | ```"150"``` |
| ```one hundred and fifty books``` | ```"one hundred and fifty"``` | ```"150"```|
| ```a grade of one point five```| ```"one point five"``` |  ```"1.5"``` |
| ```buy two dozen eggs```    | ```"two dozen"``` | ```"24"``` |


LUIS inclui o valor reconhecido de um **`builtin.number`** entidade no `resolution` campo da resposta JSON retorna.

## <a name="resolution-for-prebuilt-number"></a>Resolução de número pré-criados
O exemplo seguinte mostra uma resposta JSON do LUIS, que inclui a resolução do valor 24, para a expressão "duas dezenas".

```json
{
  "query": "order two dozen eggs",
  "topScoringIntent": {
    "intent": "OrderFood",
    "score": 0.105443209
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.105443209
    },
    {
      "intent": "OrderFood",
      "score": 0.9468431361
    },
    {
      "intent": "Help",
      "score": 0.000399122015
    },
  ],
  "entities": [
    {
      "entity": "two dozen",
      "type": "builtin.number",
      "startIndex": 6,
      "endIndex": 14,
      "resolution": {
        "value": "24"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o [moeda](luis-reference-prebuilt-currency.md), [ordinal](luis-reference-prebuilt-ordinal.md), e [percentagem](luis-reference-prebuilt-percentage.md). 