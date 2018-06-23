---
title: LUIS Prebuilt entidades ordinal referência - Azure | Microsoft Docs
titleSuffix: Azure
description: Este artigo contém informações de entidade prebuilt ordinal na compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 2ff9b083e6cabe455baea3ed777dd6cc00b6fbfe
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321968"
---
# <a name="ordinal-entity"></a>Entidade ordinal
Número de ordinal é uma representação numérica de um objeto dentro de um conjunto: `first`, `second`, `third`. Porque esta entidade já está preparada, não terá de adicionar utterances de exemplo que contém ordinal para pendentes a aplicação. Entidade ordinal é suportada no [culturas muitos](luis-reference-prebuilt-entities.md). 

## <a name="types-of-ordinal"></a>Tipos de ordinal
Gerido do ordinal de [Recognizers texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L45) repositório do Github

## <a name="resolution-for-prebuilt-ordinal-entity"></a>Resolução de entidades ordinal prebuilt
O exemplo seguinte mostra a resolução de **builtin.ordinal** entidade.

```JSON
{
  "query": "Order the second option",
  "topScoringIntent": {
    "intent": "OrderFood",
    "score": 0.9993253
  },
  "intents": [
    {
      "intent": "OrderFood",
      "score": 0.9993253
    },
    {
      "intent": "None",
      "score": 0.05046708
    }
  ],
  "entities": [
    {
      "entity": "second",
      "type": "builtin.ordinal",
      "startIndex": 10,
      "endIndex": 15,
      "resolution": {
        "value": "2"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o [percentagem](luis-reference-prebuilt-percentage.md), [phonenumber](luis-reference-prebuilt-phonenumber.md), e [temperatura](luis-reference-prebuilt-temperature.md) entidades. 