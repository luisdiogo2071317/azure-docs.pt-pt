---
title: incluir ficheiro
description: incluir ficheiro
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 09/16/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: d264477693458ff4132c1f69704a480eed2756e0
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988021"
---
## <a name="update-the-function"></a>Atualizar a função

Por predefinição, o modelo cria uma função que requer uma tecla de função ao efetuar pedidos. Para que seja mais fácil testar a função no Azure, terá de atualizar a função para permitir acesso anónimo. A forma como efetua esta alteração depende da sua linguagem de projeto de funções.

### <a name="c"></a>C\#

Abra o ficheiro de código MyHttpTrigger.cs que é a sua nova função e atualize o atributo **AuthorizationLevel** na definição de função para um valor de `anonymous` e guarde as alterações.

```csharp
[FunctionName("MyHttpTrigger")]
        public static IActionResult Run([HttpTrigger(AuthorizationLevel.Anonymous, 
            "get", "post", Route = null)]HttpRequest req, ILogger log)
```

### <a name="javascript"></a>JavaScript

Abra o ficheiro function.json da sua nova função, abra-o num editor de texto, atualize a propriedade **authLevel** em **bindings.httpTrigger** para `anonymous` e guarde as alterações.

```json
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
```

Agora, pode chamar a função no Azure sem ter de indicar a tecla de função. A tecla de função nunca é necessária na execução local.
