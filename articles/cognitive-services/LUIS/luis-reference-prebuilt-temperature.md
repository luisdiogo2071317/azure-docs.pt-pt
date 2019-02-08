---
title: Entidade pré-criados de temperatura
titleSuffix: Azure
description: Este artigo contém temperatura informações da entidade pré-criados na compreensão de idiomas (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 3629dc96fbb86236888014d9de9a7b7b3d86c298
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55867397"
---
# <a name="temperature-prebuilt-entity-for-a-luis-app"></a>Temperatura entidade criados previamente para uma aplicação LUIS
Temperatura extrai uma variedade de tipos de temperatura. Uma vez que esta entidade já está preparada, não é necessário adicionar expressões de exemplo que contém a temperatura à aplicação. Entidade de temperatura é suportada no [várias culturas](luis-reference-prebuilt-entities.md). 

## <a name="types-of-temperature"></a>Tipos de temperatura
Temperatura é gerenciada a partir da [reconhecedores texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L819) repositório do GitHub

## <a name="resolution-for-prebuilt-temperature-entity"></a>Resolução de entidades de temperatura pré-criados
O exemplo seguinte mostra a resolução do **builtin.temperature** entidade.

```json
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
