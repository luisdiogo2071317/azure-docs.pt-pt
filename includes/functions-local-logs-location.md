---
title: incluir ficheiro
description: incluir ficheiro
services: functions
author: ggailey777
manager: cfowler
ms.service: functions
ms.topic: include
ms.date: 05/01/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 88c01e8e57d4a92478b8b1ca0689ff0f8e499b39
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
Quando o anfitrião de funções é executada localmente, escreve os registos para o seguinte caminho:

```
<DefaultTempDirectory>\LogFiles\Application\Functions
```

No Windows, `<DefaultTempDirectory>` primeiro valor encontrado o USERPROFILE TMP, TEMP, variáveis de ambiente ou diretório do Windows.
No MacOS ou Linux, `<DefaultTempDirectory>` é a variável de ambiente de TMPDIR.

> [!NOTE]
> Quando o anfitrião de funções é iniciado, substitui a estrutura de ficheiros existentes no diretório.