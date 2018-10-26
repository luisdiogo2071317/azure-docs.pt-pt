---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 0a3f739bae3ec758c8e25d581d3e2b9feb4af5b1
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133635"
---
```json
{
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    }
}
```

|Propriedade  |Predefinição | Descrição |
|---------|---------|---------| 
|maxConcurrentCalls|16|O número máximo de chamadas simultâneas para o retorno de chamada que deve iniciar o bombardeamento de mensagens. Por predefinição, o runtime das funções processa várias mensagens em simultâneo. Para direcionar o tempo de execução para processar apenas uma única fila ou uma mensagem de tópico ao mesmo tempo, defina `maxConcurrentCalls` como 1. | 
|prefetchCount|n/d|A predefinição PrefetchCount que será utilizada pelo MessageReceiver subjacente.| 
|autoRenewTimeout|00:05:00|A duração máxima em que o bloqueio da mensagem será renovado automaticamente.| 
