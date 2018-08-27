---
title: incluir ficheiro
description: incluir ficheiro
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: 419f15901b665b43b850922f77bd32d7aac8d3a2
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42920521"
---
O ficheiro de expressões de exemplo **utterances.json**, segue um formato específico. 

O `text` campo contém o texto da expressão de exemplo. O `intentName` campo tem de corresponder ao nome de um objetivo existente na aplicação do LUIS. O campo `entityLabels` é obrigatório. Se não quiser que identifique qualquer entidades, forneça uma matriz vazia.

Se a matriz de entityLabels não está vazia, o `startCharIndex` e `endCharIndex` tem de marcar a entidade referida no `entityName` campo. O índice é baseado em zero, que significa que a 6 no exemplo principal refere-se para o "S" de Seattle e não o espaço antes da capital S. Se começar ou terminar a etiqueta num espaço no texto, a chamada de API para adicionar as expressões falha.

```JSON
[
  {
    "text": "go to Seattle today",
    "intentName": "BookFlight",
    "entityLabels": [
      {
        "entityName": "Location::LocationTo",
        "startCharIndex": 6,
        "endCharIndex": 12
      }
    ]
  },
  {
    "text": "purple dogs are difficult to work with",
    "intentName": "BookFlight",
    "entityLabels": []
  }
]
```