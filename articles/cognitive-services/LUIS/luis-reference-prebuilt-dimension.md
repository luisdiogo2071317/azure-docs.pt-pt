---
title: Entidades previamente concebidas de dimensão
titleSuffix: Azure
description: Este artigo contém informações de entidade pré-criados na compreensão de idiomas (LUIS) de dimensão.
services: cognitive-services
ms.custom: seodec18
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: 775c7800c2ea9abe4b960a2e82cebb85e3c74d3b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53098777"
---
# <a name="dimension-entity"></a>Entidade de dimensão
A entidade de dimensão pré-criados detecta vários tipos de dimensões, independentemente da cultura de aplicação do LUIS. Uma vez que já está preparada esta entidade, não é necessário adicionar expressões de exemplo que contém as dimensões para os objetivos do aplicativo. Entidade de dimensão é suportada no [várias culturas](luis-reference-prebuilt-entities.md). 

## <a name="types-of-dimension"></a>Tipos de dimensão

Dimensão é gerenciada a partir da [reconhecedores texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml) repositório do Github


## <a name="resolution-for-dimension-entity"></a>Resolução de entidades de dimensão
O exemplo seguinte mostra a resolução do **builtin.dimension** entidade.

```json
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