---
title: Referência de keyphrase de entidades de LUIS pré-criados - Azure | Documentos da Microsoft
titleSuffix: Azure
description: Este artigo contém keyphrase informações da entidade pré-criados na compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/09/2018
ms.author: v-geberr
ms.openlocfilehash: f7c71c409a1d32eb7ca0984e9fdfe4d5d0ed0cef
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37932925"
---
# <a name="keyphrase-entity"></a>entidade de keyPhrase
keyPhrase extrai uma variedade de expressões-chave de uma expressão. Não é necessário adicionar expressões de exemplo que contém keyPhrase à aplicação. entidade keyPhrase é suportada no [várias culturas](luis-supported-languages.md#languages-supported) como parte do [análise de texto](../text-analytics/overview.md) funcionalidades. 

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>Resolução de entidade keyPhrase pré-criados
O exemplo seguinte mostra a resolução do **builtin.keyPhrase** entidade.

```JSON
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