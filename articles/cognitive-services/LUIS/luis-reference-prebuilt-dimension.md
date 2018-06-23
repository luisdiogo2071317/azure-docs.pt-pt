---
title: Entidades LUIS Prebuilt dimensão de referência - Azure | Microsoft Docs
titleSuffix: Azure
description: Este artigo contém informações de entidade prebuilt na compreensão de idiomas (LUIS) de dimensão.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 3b2758f1d68ae3659f2e43cad555d327b21f8732
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321982"
---
# <a name="dimension-entity"></a>Entidade de dimensão
A entidade de dimensão prebuilt Deteta vários tipos de dimensões, independentemente do idioma de aplicação LUIS. Porque esta entidade já está preparada, não terá de adicionar utterances de exemplo que contenha dimensões para pendentes a aplicação. Entidade de dimensão é suportada no [culturas muitos](luis-reference-prebuilt-entities.md). 

## <a name="types-of-dimension"></a>Tipos de dimensão

Dimensão é gerida a partir de [Recognizers texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml) repositório do Github


## <a name="resolution-for-dimension-entity"></a>Resolução de entidades de dimensão
O exemplo seguinte mostra a resolução de **builtin.dimension** entidade.

```JSON
{
  "query": "it takes more than 10 1/2 miles of cable and wire to hook it all up , and 23 computers.",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.762141049
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.762141049
    }
  ],
  "entities": [
    {
      "entity": "10 1/2 miles",
      "type": "builtin.dimension",
      "startIndex": 19,
      "endIndex": 30,
      "resolution": {
        "unit": "Mile",
        "value": "10.5"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o [e-mail](luis-reference-prebuilt-email.md), [número](luis-reference-prebuilt-number.md), e [ordinal](luis-reference-prebuilt-ordinal.md) entidades. 