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
ms.openlocfilehash: d166a77a0636efea3b63660fde2187e3f2ec15c0
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063753"
---
Quando desenvolver funções localmente, pode instalar as extensões que precisa, utilizando as ferramentas de núcleos de funções do Azure no Terminal ou numa linha de comandos. 

Depois de atualizar o *function.json* ficheiro para incluir todos os enlaces da função, tem de executar o `func extensions install` comando na pasta do projeto. O comando lê o *function.json* ficheiro para ver quais os pacotes que precisa e, em seguida, instala-los.

Se quiser instalar uma versão específica de um pacote ou de que pretende instalar pacotes antes de editar o *function.json* de ficheiros, utilize o `func extensions install` comando com o nome do pacote, conforme mostrado no exemplo seguinte:

```
func extensions install --package Microsoft.Azure.WebJobs.ServiceBus --version <target_version>
```

Substitua `<target_version>` com uma versão específica do pacote, tais como `3.0.0-beta5`. Versões válidas estão listadas nas páginas individuais pacote em [NuGet.org](https://nuget.org).
