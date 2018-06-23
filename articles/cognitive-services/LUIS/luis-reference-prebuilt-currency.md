---
title: Referência de moeda de entidades de LUIS Prebuilt - Azure | Microsoft Docs
titleSuffix: Azure
description: Este artigo contém moeda informações de entidade prebuilt na compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 3e20642f6734b0247d23db1a63317eb8b4a96b5e
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321849"
---
# <a name="currency-entity"></a>Entidade de moeda
A entidade de moeda prebuilt Deteta moeda em muitos denominations e países, independentemente do idioma de aplicação LUIS. Porque esta entidade já está preparada, não terá de adicionar utterances de exemplo que contém a moeda para pendentes a aplicação. Entidade de moeda é suportada no [culturas muitos](luis-reference-prebuilt-entities.md). 

## <a name="types-of-currency"></a>Tipos de moeda
Moeda é gerida a partir de [Recognizers texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L26) repositório do Github

## <a name="resolution-for-currency-entity"></a>Resolução de entidades de moeda
O exemplo seguinte mostra a resolução de **builtin.currency** entidade.

```JSON
{
  "query": "search for items under $10.99",
  "topScoringIntent": {
    "intent": "SearchForItems",
    "score": 0.926173568
  },
  "intents": [
    {
      "intent": "SearchForItems",
      "score": 0.926173568
    },
    {
      "intent": "None",
      "score": 0.07376878
    }
  ],
  "entities": [
    {
      "entity": "$10.99",
      "type": "builtin.currency",
      "startIndex": 23,
      "endIndex": 28,
      "resolution": {
        "unit": "Dollar",
        "value": "10.99"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o [datetimeV2](luis-reference-prebuilt-datetimev2.md), [dimensão](luis-reference-prebuilt-dimension.md), e [e-mail](luis-reference-prebuilt-email.md) entidades. 