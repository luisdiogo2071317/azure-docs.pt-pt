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
ms.openlocfilehash: 6fb497a5b6da00dece43c7f41ea3c411f385a2ba
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726892"
---
Quando desenvolver funções localmente, pode instalar as extensões que precisa, utilizando as ferramentas de núcleos de funções do Azure no Terminal ou numa linha de comandos. 

Depois de atualizar o *function.json* ficheiro para incluir todos os enlaces da função, tem de executar o `func extensions install` comando na pasta do projeto. O comando lê o *function.json* ficheiro para ver quais os pacotes que precisa e, em seguida, instala-los.

Se quiser instalar uma versão específica de um pacote ou de que pretende instalar pacotes antes de editar o *function.json* de ficheiros, utilize o `func extensions install` comando com o nome do pacote, conforme mostrado no exemplo seguinte:

```
func extensions install --package Microsoft.Azure.WebJobs.Extensions.CosmosDB --version <target_version>
```

Substitua `<target_version>` com uma versão específica do pacote. Versões válidas estão listadas nas páginas individuais pacote em [NuGet.org](https://nuget.org).
