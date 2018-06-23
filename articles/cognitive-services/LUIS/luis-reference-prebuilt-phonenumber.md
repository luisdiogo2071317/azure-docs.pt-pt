---
title: Entidades LUIS Prebuilt phone referência número - Azure | Microsoft Docs
titleSuffix: Azure
description: Este artigo contém informações de entidade prebuilt número na compreensão de idiomas (LUIS) do telefone.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 0f72b807b9b0ec110a80d67babb1c45902b8c810
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321884"
---
# <a name="phonenumber-entity"></a>PhoneNumber entidade
O `phonenumber` entidade extrai uma variedade de números de telefone, incluindo o código de país. Porque esta entidade já está preparada, não terá de adicionar utterances de exemplo para a aplicação. O `phonenumber` entidade é suportada no `en-us` idioma apenas. 

## <a name="types-of-phonenumber"></a>Tipos de phonenumber
PhoneNumber é gerida a partir de [Recognizers texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml) repositório do Github

## <a name="resolution-for-prebuilt-phonenumber-entity"></a>Resolução de entidades prebuilt phonenumber
O exemplo seguinte mostra a resolução de **builtin.phonenumber** entidade.

```JSON
{
  "query": "my mobile is 00 44 161 1234567",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8448457
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.8448457
    }
  ],
  "entities": [
    {
      "entity": "00 44 161 1234567",
      "type": "builtin.phonenumber",
      "startIndex": 13,
      "endIndex": 29,
      "resolution": {
        "value": "00 44 161 1234567"
      }
    }
  ]
}
```


## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o [percentagem](luis-reference-prebuilt-percentage.md), [número](luis-reference-prebuilt-number.md), e [temperatura](luis-reference-prebuilt-temperature.md) entidades. 