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
ms.openlocfilehash: dae56e05f01e83f05e75fdf378c0c50679d18728
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42820022"
---
Para compreender o que retorna um ponto de extremidade de predição do LUIS, ver um resultado de predição num navegador da web. Para consultar uma aplicações pública, terá sua própria chave e o ID da aplicação. O ID da aplicação IoT público, `df67dcdb-c37d-46af-88e1-8b97951ca1c2`, é fornecido como parte do URL no passo um.

O formato do URL para um **obter** pedido de ponto final é:

```JSON
https://<region>.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<YOUR-KEY>&q=<user-utterance>
```

1. O ponto final da aplicação IoT pública está neste formato: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=<YOUR_KEY>&q=turn on the bedroom light`

    Copie o URL e substitua a chave para o valor do `<YOUR_KEY>`.

2. Cole o URL numa janela do browser e prima Enter. O browser apresenta um resultado JSON, que indica que o LUIS Deteta os `HomeAutomation.TurnOn` intenção como a intenção de principal e o `HomeAutomation.Room` entidade com o valor `bedroom`.

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

3. Altere o valor do parâmetro `q=` no URL para `turn off the living room light` e prima Enter. O resultado indica agora que o LUIS detetado o `HomeAutomation.TurnOff` intenção como a intenção de principal e o `HomeAutomation.Room` entidade com o valor `living room`. 

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
