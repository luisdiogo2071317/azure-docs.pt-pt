---
title: Entidades previamente concebidas LUIS idade referência - Azure | Documentos da Microsoft
titleSuffix: Azure
description: Este artigo contém idade informações da entidade pré-criados na compreensão de idiomas (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: diberry
ms.openlocfilehash: 851e658c68c845c900aee9a4c4c4780f72e83725
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033027"
---
# <a name="age-entity"></a>Entidade de idade
A entidade de idade pré-criados captura o valor de idade tanto numericamente e em termos de dias, semanas, meses e anos. Uma vez que já está preparada esta entidade, não é necessário adicionar expressões de exemplo que contém a idade para os objetivos do aplicativo. Entidade de idade é suportada no [várias culturas](luis-reference-prebuilt-entities.md). 

## <a name="types-of-age"></a>Tipos de idade
Idade é gerenciada a partir da [reconhecedores texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L3) repositório do Github

## <a name="resolution-for-prebuilt-age-entity"></a>Resolução de entidades de idade pré-criados
O exemplo seguinte mostra a resolução do **builtin.age** entidade.

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