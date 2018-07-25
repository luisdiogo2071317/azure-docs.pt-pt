---
title: Entidades previamente concebidas LUIS dimensão de referência - Azure | Documentos da Microsoft
titleSuffix: Azure
description: Este artigo contém informações de entidade pré-criados na compreensão de idiomas (LUIS) de dimensão.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: diberry
ms.openlocfilehash: 3c923e7791c58255690100b04700577eb5c3f5dd
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39237269"
---
# <a name="dimension-entity"></a>Entidade de dimensão
A entidade de dimensão pré-criados detecta vários tipos de dimensões, independentemente da cultura de aplicação do LUIS. Uma vez que já está preparada esta entidade, não é necessário adicionar expressões de exemplo que contém as dimensões para os objetivos do aplicativo. Entidade de dimensão é suportada no [várias culturas](luis-reference-prebuilt-entities.md). 

## <a name="types-of-dimension"></a>Tipos de dimensão

Dimensão é gerenciada a partir da [reconhecedores texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml) repositório do Github


## <a name="resolution-for-dimension-entity"></a>Resolução de entidades de dimensão
O exemplo seguinte mostra a resolução do **builtin.dimension** entidade.

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