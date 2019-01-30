---
title: Entidade Keyphrase pré-criados
titleSuffix: Azure
description: Este artigo contém keyphrase informações da entidade pré-criados na compreensão de idiomas (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: a2bb486de0999ad61cf2fdfdfb57dd91d376f597
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55206602"
---
# <a name="keyphrase-prebuilt-entity-for-a-luis-app"></a>keyPhrase entidade criados previamente para uma aplicação LUIS
keyPhrase extrai uma variedade de expressões-chave de uma expressão. Não é necessário adicionar expressões de exemplo que contém keyPhrase à aplicação. entidade keyPhrase é suportada no [várias culturas](luis-language-support.md#languages-supported) como parte do [análise de texto](../text-analytics/overview.md) funcionalidades. 

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>Resolução de entidade keyPhrase pré-criados
O exemplo seguinte mostra a resolução do **builtin.keyPhrase** entidade.

```json
{
  "query": "where is the educational requirements form for the development and engineering group",
  "topScoringIntent": {
    "intent": "GetJobInformation",
    "score": 0.182757929
  },
  "entities": [
    {
      "entity": "development",
      "type": "builtin.keyPhrase",
      "startIndex": 51,
      "endIndex": 61
    },
    {
      "entity": "educational requirements",
      "type": "builtin.keyPhrase",
      "startIndex": 13,
      "endIndex": 36
    }
  ]
}
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o [percentagem](luis-reference-prebuilt-percentage.md), [número](luis-reference-prebuilt-number.md), e [idade](luis-reference-prebuilt-age.md) entidades.
