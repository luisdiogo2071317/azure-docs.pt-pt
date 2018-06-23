---
title: Referência de url de entidades de LUIS Prebuilt - Azure | Microsoft Docs
titleSuffix: Azure
description: Este artigo contém um url informações de entidade prebuilt na compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 4eacf564a295a568a3e2c8d2f44ad0af3fbbe258
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321961"
---
# <a name="url-entity"></a>Entidade de URL
Entidade de URL extrai os URLs com nomes de domínio ou endereços IP. Porque esta entidade já está preparada, não terá de adicionar utterances de exemplo que contém os URLs para a aplicação. Entidade de URL é suportada no `en-us` idioma apenas. 

## <a name="types-of-urls"></a>Tipos de URLs
Gerido do URL de [Recognizers texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml) repositório do Github

## <a name="resolution-for-prebuilt-url-entity"></a>Resolução de entidades de URL prebuilt
O exemplo seguinte mostra a resolução de **builtin.url** entidade.

```JSON
{
  "query": "http://www.luis.ai is a great cognitive services example of artificial intelligence",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.781975448
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.781975448
    }
  ],
  "entities": [
    {
      "entity": "http://www.luis.ai",
      "type": "builtin.url",
      "startIndex": 0,
      "endIndex": 17
    }
  ]
}
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o [ordinal](luis-reference-prebuilt-ordinal.md), [número](luis-reference-prebuilt-number.md), e [temperatura](luis-reference-prebuilt-temperature.md) entidades.