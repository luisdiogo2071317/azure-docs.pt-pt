---
title: incluir ficheiro
description: incluir ficheiro
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: 46b588d6977aa6ffeb9d473e6bfe149cde7147ba
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55478763"
---
O ficheiro de expressões de exemplo, **utterances.json**, segue um formato específico. 

O campo `text` contém o texto da expressão de exemplo. O campo `intentName` tem de corresponder ao nome de uma intenção existente na aplicação LUIS. O campo `entityLabels` é obrigatório. Se não quiser etiquetar entidades, forneça uma matriz vazia.

Se a matriz entityLabels não estiver vazia, `startCharIndex` e `endCharIndex` têm de marcar a entidade referida no campo `entityName`. O índice é baseado em zero, o que significa que o 6 no exemplo anterior se refere ao "S" de Seattle e não ao espaço antes do S maiúsculo. Se começar ou terminar a etiqueta num espaço no texto, a chamada à API para adicionar as expressões falha.

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
