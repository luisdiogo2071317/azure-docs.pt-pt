---
title: Referência de percentagem de entidades de LUIS Prebuilt - Azure | Microsoft Docs
titleSuffix: Azure
description: Este artigo contém percentagem informações de entidade prebuilt na compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 4d502e93f8b6bf3af2da05a499c359faeabb51da
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321912"
---
# <a name="percentage-entity"></a>Percentagem de entidade
Números de percentagem podem aparecer como frações, `3 1/2`, ou como percentagem, `2%`. Porque esta entidade já está preparada, não terá de adicionar utterances de exemplo que contém a percentagem de pendentes a aplicação. Percentagem de entidade é suportada no [culturas muitos](luis-reference-prebuilt-entities.md). 

## <a name="types-of-percentage"></a>Tipos de percentagem
Percentagem é gerida a partir de [Recognizers texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L114) repositório do Github

## <a name="resolution-for-prebuilt-percentage-entity"></a>Resolução de percentagem prebuilt entidade
O exemplo seguinte mostra a resolução de **builtin.percentage** entidade.

```JSON
{
  "query": "set a trigger when my stock goes up 2%",
  "topScoringIntent": {
    "intent": "SetTrigger",
    "score": 0.971157849
  },
  "intents": [
    {
      "intent": "SetTrigger",
      "score": 0.971157849
    }
  ],
  "entities": [
    {
      "entity": "2%",
      "type": "builtin.percentage",
      "startIndex": 36,
      "endIndex": 37,
      "resolution": {
        "value": "2%"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o [ordinal](luis-reference-prebuilt-ordinal.md), [número](luis-reference-prebuilt-number.md), e [temperatura](luis-reference-prebuilt-temperature.md) entidades. 