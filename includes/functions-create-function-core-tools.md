---
title: incluir ficheiro
description: incluir ficheiro
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 10/20/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 26789a12053fa6275b09836e706c391e181c8efd
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53430017"
---
## <a name="create-a-function"></a>Criar uma função

O seguinte comando cria uma função acionada por HTTP com o nome `MyHttpTrigger`.

```bash
func new --name MyHttpTrigger --template "HttpTrigger"
```

Quando o comando for executado, verá algo parecido com o seguinte resultado:

```output
The function "MyHttpTrigger" was created successfully from the "HttpTrigger" template.
```
