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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38739632"
---
Quando o anfitrião de funções é executada localmente, escreve os registos para o seguinte caminho:

```
<DefaultTempDirectory>\LogFiles\Application\Functions
```

No Windows, `<DefaultTempDirectory>` é o primeiro valor encontrado do perfil do USUÁRIO TMP, TEMP, variáveis de ambiente ou o diretório do Windows.
No MacOS ou Linux, `<DefaultTempDirectory>` é a variável de ambiente de TMPDIR.

> [!NOTE]
> Quando o anfitrião de funções é iniciado, ele substitui a estrutura de ficheiros existentes no diretório.