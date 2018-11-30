---
title: Referência de url de entidades de LUIS pré-criados - Azure | Documentos da Microsoft
titleSuffix: Azure
description: Este artigo contém o url de informações da entidade pré-criados na compreensão de idiomas (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 62872a3fd3e6e830e25fc4cdba01d1f32b80db91
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52446539"
---
# <a name="url-entity"></a>Entidade de URL
Entidade de URL extrai URLs com nomes de domínio ou endereços IP. Uma vez que já está preparada esta entidade, não é necessário adicionar expressões de exemplo que contém os URLs para a aplicação. Entidade de URL é suportada no `en-us` apenas a cultura. 

## <a name="types-of-urls"></a>Tipos de URLs
URL é gerido a partir da [reconhecedores texto](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml) repositório do Github

## <a name="resolution-for-prebuilt-url-entity"></a>Resolução de entidade pré-criados do URL
O exemplo seguinte mostra a resolução do **builtin.url** entidade.

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