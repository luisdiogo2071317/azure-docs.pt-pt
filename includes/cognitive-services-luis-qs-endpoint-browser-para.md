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
ms.openlocfilehash: 1b70c5e71c585524579171782dfdeda308592b30
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55478693"
---
Para compreender o que devolve um ponto final de predição do LUIS, veja um resultado de predição num browser. Para consultar uma aplicação pública, precisa da sua própria chave e do ID da aplicação. O ID da aplicação IoT pública, `df67dcdb-c37d-46af-88e1-8b97951ca1c2`, é fornecido como parte do URL no primeiro passo.

O formato do URL para um pedido de ponto final **GET** é:

```JSON
https://<region>.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<YOUR-KEY>&q=<user-utterance>
```

1. O ponto final da aplicação IoT pública está neste formato: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=<YOUR_KEY>&q=turn on the bedroom light`

    Copie o URL e substitua a sua chave pelo valor de `<YOUR_KEY>`.

2. Cole o URL numa janela do browser e prima Enter. O browser apresenta um resultado JSON que indica que o LUIS deteta a intenção `HomeAutomation.TurnOn` como intenção principal e a entidade `HomeAutomation.Room` com o valor `bedroom`.

    ```JSON
    {
      "query": "turn on the bedroom light",
      "topScoringIntent": {
        "intent": "HomeAutomation.TurnOn",
        "score": 0.809439957
      },
      "entities": [
        {
          "entity": "bedroom",
          "type": "HomeAutomation.Room",
          "startIndex": 12,
          "endIndex": 18,
          "score": 0.8065475
        }
      ]
    }
    ```

3. Altere o valor do parâmetro `q=` no URL para `turn off the living room light` e prima Enter. O resultado indica agora que o LUIS detetou a intenção `HomeAutomation.TurnOff` como intenção principal e a entidade `HomeAutomation.Room` com o valor `living room`. 

    ```JSON
    {
      "query": "turn off the living room light",
      "topScoringIntent": {
        "intent": "HomeAutomation.TurnOff",
        "score": 0.984057844
      },
      "entities": [
        {
          "entity": "living room",
          "type": "HomeAutomation.Room",
          "startIndex": 13,
          "endIndex": 23,
          "score": 0.9619945
        }
      ]
    }
    ```
