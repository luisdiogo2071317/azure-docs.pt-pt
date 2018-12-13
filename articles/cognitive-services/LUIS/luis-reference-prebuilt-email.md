---
title: Entidades previamente concebidas LUIS de e-mail de referência - Azure | Documentos da Microsoft
titleSuffix: Azure
description: Este artigo contém e-mail informações da entidade pré-criados na compreensão de idiomas (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 1cb12bdc362955da907fb5a5ed64c2a1a43fdc32
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/10/2018
ms.locfileid: "53140868"
---
# <a name="email-prebuilt-entity-for-a-luis-app"></a>Enviar um e-mail entidade criados previamente para uma aplicação LUIS
Extração de e-mail inclui o endereço de e-mail completo de uma expressão. Uma vez que já está preparada esta entidade, não é necessário adicionar expressões de exemplo que contém o e-mail para os objetivos do aplicativo. Entidade de e-mail é suportada no `en-us` apenas a cultura. 

## <a name="resolution-for-prebuilt-email"></a>Resolução de correio eletrónico pré-criados
O exemplo seguinte mostra a resolução do **builtin.email** entidade.

```json
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