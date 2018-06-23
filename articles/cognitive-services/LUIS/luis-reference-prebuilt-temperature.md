---
title: Referência de temperatura de entidades de LUIS Prebuilt - Azure | Microsoft Docs
titleSuffix: Azure
description: Este artigo contém temperatura informações de entidade prebuilt na compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 3cc16e7ec87775407c4261655d8f680cc0903e81
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321842"
---
# <a name="temperature-entity"></a>Entidade de temperatura
Temperatura extrai uma variedade de tipos de temperatura. Porque esta entidade já está preparada, não terá de adicionar utterances de exemplo que contém a temperatura à aplicação. Entidade de temperatura é suportada no [culturas muitos](luis-reference-prebuilt-entities.md). 

## <a name="types-of-temperature"></a>Tipos de temperatura
Temperatura é gerida a partir de [Recognizers texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L819) repositório do Github

## <a name="resolution-for-prebuilt-temperature-entity"></a>Resolução de entidades de temperatura prebuilt
O exemplo seguinte mostra a resolução de **builtin.temperature** entidade.

```JSON
{
  "query": "set the temperature to 30 degrees",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.85310787
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.85310787
    }
  ],
  "entities": [
    {
      "entity": "30 degrees",
      "type": "builtin.temperature",
      "startIndex": 23,
      "endIndex": 32,
      "resolution": {
        "unit": "Degree",
        "value": "30"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o [percentagem](luis-reference-prebuilt-percentage.md), [número](luis-reference-prebuilt-number.md), e [idade](luis-reference-prebuilt-age.md) entidades. 