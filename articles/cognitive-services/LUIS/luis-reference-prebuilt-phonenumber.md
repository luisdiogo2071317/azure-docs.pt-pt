---
title: Entidades previamente concebidas LUIS phone referência número - Azure | Documentos da Microsoft
titleSuffix: Azure
description: Este artigo contém informações de entidade pré-criados numéricas na compreensão de idiomas (LUIS) do telefone.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: diberry
ms.openlocfilehash: 1ae14f72f0dc610b9399e675f49ef5fff51a6965
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39238996"
---
# <a name="phonenumber-entity"></a>Entidade Phonenumber
O `phonenumber` entidade extrai uma variedade de números de telefone, incluindo o indicativo de país. Uma vez que já está preparada esta entidade, não é necessário adicionar expressões de exemplo para a aplicação. O `phonenumber` entidade é suportada no `en-us` apenas a cultura. 

## <a name="types-of-phonenumber"></a>Tipos de phonenumber
PhoneNumber é gerido a partir da [reconhecedores texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml) repositório do Github

## <a name="resolution-for-prebuilt-phonenumber-entity"></a>Resolução de entidade phonenumber pré-criados
O exemplo seguinte mostra a resolução do **builtin.phonenumber** entidade.

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