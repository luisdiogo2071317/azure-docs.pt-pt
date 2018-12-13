---
title: PersonName pré-criados de entidade
titleSuffix: Azure Cognitive Services
description: Este artigo contém personName informações da entidade pré-criados na compreensão de idiomas (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: 31dce870e99cbe74e2795a3ba661b0caf92dd48e
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/10/2018
ms.locfileid: "53140239"
---
# <a name="personname-prebuilt-entity-for-a-luis-app"></a>PersonName entidade criados previamente para uma aplicação LUIS
A entidade de personName pré-criados Deteta os nomes de pessoas. Uma vez que já está preparada esta entidade, não é necessário adicionar expressões de exemplo que contém personName para os objetivos do aplicativo. entidade de personName é suportada em inglês e chinês [culturas](luis-reference-prebuilt-entities.md).

## <a name="resolution-for-personname-entity"></a>Resolução de entidades de personName
O exemplo seguinte mostra a resolução do **builtin.personName** entidade.

```json
{
  "query": "Is Jill Jones in Cairo?",
  "topScoringIntent": {
    "intent": "WhereIsEmployee",
    "score": 0.762141049
  }
  "entities": [
    {
      "entity": "Jill Jones",
      "type": "builtin.personName",
      "startIndex": 3,
      "endIndex": 12
    }
  ]
}
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o [e-mail](luis-reference-prebuilt-email.md), [número](luis-reference-prebuilt-number.md), e [ordinal](luis-reference-prebuilt-ordinal.md) entidades. 