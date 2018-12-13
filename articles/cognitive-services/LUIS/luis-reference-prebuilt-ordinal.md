---
title: Ordinal entidade pré-criados
titleSuffix: Azure
description: Este artigo contém informações de entidade pré-criados ordinal na compreensão de idiomas (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: 060314bd56d477bad3dcb3333ba02c5f4786b476
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53082769"
---
# <a name="ordinal-entity"></a>Entidade ordinal
Número de ordinal é uma representação numérica de um objeto dentro de um conjunto: `first`, `second`, `third`. Uma vez que já está preparada esta entidade, não é necessário adicionar expressões de exemplo que contém ordinal para os objetivos do aplicativo. Entidade ordinal é suportada no [várias culturas](luis-reference-prebuilt-entities.md). 

## <a name="types-of-ordinal"></a>Tipos de ordinal
Ordinal é gerido a partir da [reconhecedores texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L45) repositório do Github

## <a name="resolution-for-prebuilt-ordinal-entity"></a>Resolução de entidade ordinal pré-criados
O exemplo seguinte mostra a resolução do **builtin.ordinal** entidade.

```json
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