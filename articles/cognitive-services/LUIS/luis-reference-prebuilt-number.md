---
title: Número de entidades LUIS Prebuilt referência - Azure | Microsoft Docs
titleSuffix: Azure
description: Este artigo contém entidade prebuilt informações sobre o número na compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: aa0b389a0694a3b742259fd42bed08055fbbadbe
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321863"
---
# <a name="number-entity"></a>Número entidade
Existem várias formas, na qual os valores numéricos são utilizados quantificar, rápidas e descrevem as informações. Este artigo abrange apenas alguns exemplos de possíveis. LUIS interpreta variações no utterances de utilizador e devolve os valores numéricos consistentes. Porque esta entidade já está preparada, não terá de adicionar utterances de exemplo que contém o número e os pendentes de aplicação. 

## <a name="types-of-number"></a>Tipos de número
Gerido do número de [Recognizers texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml) repositório do Github

## <a name="examples-of-number-resolution"></a>Exemplos de resolução de número

| utterance        | Entidade   | Resolução |
| ------------- |:----------------:| --------------:|
| ```one thousand times```  | ```"one thousand"``` |   ```"1000"```      | 
| ```1,000 people```        | ```"1,000"```    |   ```"1000"```      |
| ```1/2 cup```         | ```"1 / 2"```    |    ```"0.5"```      |
|  ```one half the amount```     | ```"one half"```     |    ```"0.5"```      |
| ```one hundred fifty orders``` | ```"one hundred fifty"``` | ```"150"``` |
| ```one hundred and fifty books``` | ```"one hundred and fifty"``` | ```"150"```|
| ```a grade of one point five```| ```"one point five"``` |  ```"1.5"``` |
| ```buy two dozen eggs```    | ```"two dozen"``` | ```"24"``` |


LUIS inclui o valor reconhecido um **`builtin.number`** entidade no `resolution` campo da resposta JSON devolve.

## <a name="resolution-for-prebuilt-number"></a>Resolução de número prebuilt
O exemplo seguinte mostra uma resposta JSON de LUIS, que inclui a resolução do valor 24, para o utterance "dúzia de dois".

```JSON
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