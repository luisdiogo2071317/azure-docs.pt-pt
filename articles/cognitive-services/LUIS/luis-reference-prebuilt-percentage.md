---
title: Entidade pré-criados de percentagem
titleSuffix: Azure
description: Este artigo contém a percentagem de informações da entidade pré-criados na compreensão de idiomas (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: 457de95fb4a16a3a43a0cad902b7fb1469d9db8b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53087002"
---
# <a name="percentage-entity"></a>Entidade de percentagem
Percentagem números podem aparecer como frações, `3 1/2`, ou como percentagem, `2%`. Uma vez que já está preparada esta entidade, não é necessário adicionar expressões de exemplo que contém a percentagem para os objetivos do aplicativo. Percentagem de entidade é suportada no [várias culturas](luis-reference-prebuilt-entities.md). 

## <a name="types-of-percentage"></a>Tipos de percentagem
Percentagem é gerenciada a partir da [reconhecedores texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L114) repositório do Github

## <a name="resolution-for-prebuilt-percentage-entity"></a>Resolução de entidade de percentagem pré-criados
O exemplo seguinte mostra a resolução do **builtin.percentage** entidade.

```json
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