---
title: Entidade pré-criados de idade
titleSuffix: Azure
description: Este artigo contém idade informações da entidade pré-criados na compreensão de idiomas (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 88d2633a107f36c7c0eab8803a3b6ea10e067506
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/10/2018
ms.locfileid: "53166545"
---
# <a name="age-prebuilt-entity-for-a-luis-app"></a>Idade entidade criados previamente para uma aplicação LUIS
A entidade de idade pré-criados captura o valor de idade tanto numericamente e em termos de dias, semanas, meses e anos. Uma vez que já está preparada esta entidade, não é necessário adicionar expressões de exemplo que contém a idade para os objetivos do aplicativo. Entidade de idade é suportada no [várias culturas](luis-reference-prebuilt-entities.md). 

## <a name="types-of-age"></a>Tipos de idade
Idade é gerenciada a partir da [reconhecedores texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L3) repositório do GitHub

## <a name="resolution-for-prebuilt-age-entity"></a>Resolução de entidades de idade pré-criados
O exemplo seguinte mostra a resolução do **builtin.age** entidade.

```json
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