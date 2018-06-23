---
title: Entidades LUIS Prebuilt idade referência - Azure | Microsoft Docs
titleSuffix: Azure
description: Este artigo contém idade informações de entidade prebuilt na compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 59732469cf0d1e55643f3977958ec34a887130d3
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321989"
---
# <a name="age-entity"></a>Entidade de antiguidade
A entidade de idade prebuilt captura o valor de duração tanto numerically e em termos de dias, semanas, meses ou anos. Porque esta entidade já está preparada, não terá de adicionar utterances de exemplo que contém a idade para pendentes a aplicação. Entidade de idade é suportada no [culturas muitos](luis-reference-prebuilt-entities.md). 

## <a name="types-of-age"></a>Tipos de antiguidade
Idade é gerida a partir de [Recognizers texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L3) repositório do Github

## <a name="resolution-for-prebuilt-age-entity"></a>Resolução de entidades de idade prebuilt
O exemplo seguinte mostra a resolução de **builtin.age** entidade.

```JSON
{
  "query": "A 90 day old utilities bill is quite late.",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8236133
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.8236133
    }
  ],
  "entities": [
    {
      "entity": "90 day old",
      "type": "builtin.age",
      "startIndex": 2,
      "endIndex": 11,
      "resolution": {
        "unit": "Day",
        "value": "90"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o [moeda](luis-reference-prebuilt-currency.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md), e [dimensão](luis-reference-prebuilt-dimension.md) entidades. 