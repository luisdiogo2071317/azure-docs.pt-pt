---
title: incluir ficheiro
description: incluir ficheiro
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 04/06/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 4c9b579534d9a7f2c55e9c589b1738fe060b1cf2
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
Quando desenvolver funções localmente, pode instalar as extensões que precisa, utilizando as ferramentas de núcleos de funções do Azure no Terminal ou numa linha de comandos. O seguinte `func extensions install` comando instala a extensão de enlace de BD do Cosmos do Azure:

```
func extensions install --package Microsoft.Azure.WebJobs.Extensions.CosmosDB --version <target_version>
```

Substitua `<taget_version>` com uma versão específica do pacote. Versões válidas estão listadas nas páginas individuais pacote em [NuGet.org](https://nuget.org).
