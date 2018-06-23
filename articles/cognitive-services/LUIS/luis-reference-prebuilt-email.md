---
title: Entidades LUIS Prebuilt de e-mail de referência - Azure | Microsoft Docs
titleSuffix: Azure
description: Este artigo contém e-mail informações de entidade prebuilt na compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 517e92202bdd5bc8d970306b9e24999fa62a8e43
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321870"
---
# <a name="email-entity"></a>Entidade de e-mail
Extração de correio eletrónico inclui o endereço de e-mail completo de um utterance. Porque esta entidade já está preparada, não terá de adicionar utterances de exemplo que contém os pendentes de aplicação do correio eletrónico. Entidade de correio eletrónico é suportada no `en-us` idioma apenas. 

## <a name="resolution-for-prebuilt-email"></a>Resolução de correio eletrónico prebuilt
O exemplo seguinte mostra a resolução de **builtin.email** entidade.

```JSON
{
  "query": "please send the information to patti.owens@microsoft.com",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.811592042
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.811592042
    }
  ],
  "entities": [
    {
      "entity": "patti.owens@microsoft.com",
      "type": "builtin.email",
      "startIndex": 31,
      "endIndex": 55
    }
  ]
}
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o [número](luis-reference-prebuilt-number.md), [ordinal](luis-reference-prebuilt-ordinal.md), e [percentagem](luis-reference-prebuilt-percentage.md). 