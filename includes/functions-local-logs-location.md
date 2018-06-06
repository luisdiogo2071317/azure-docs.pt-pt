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
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "33814633"
---
Quando o anfitrião de funções é executada localmente, escreve os registos para o seguinte caminho:

```
<DefaultTempDirectory>\LogFiles\Application\Functions
```

No Windows, `<DefaultTempDirectory>` primeiro valor encontrado o USERPROFILE TMP, TEMP, variáveis de ambiente ou diretório do Windows.
No MacOS ou Linux, `<DefaultTempDirectory>` é a variável de ambiente de TMPDIR.

> [!NOTE]
> Quando o anfitrião de funções é iniciado, substitui a estrutura de ficheiros existentes no diretório.