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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38941238"
---
Ao desenvolver funções localmente, pode instalar as extensões que necessita, utilizando as ferramentas de núcleo de funções do Azure a partir do Terminal ou a partir de um prompt de comando. 

Depois de atualizar seus *Function* arquivo para incluir todas as ligações que sua função, tem de executar o `func extensions install` comando na pasta de projeto. O comando lê a *Function* arquivo para ver quais os pacotes que precisa e, em seguida, instala-os.

Se pretender instalar uma versão específica de um pacote ou se quiser instalar pacotes antes de editar a *Function* do ficheiro, utilize o `func extensions install` comando com o nome do pacote, conforme mostrado no exemplo a seguir:

```
func extensions install --package Microsoft.Azure.WebJobs.ServiceBus --version <target_version>
```

Substitua `<target_version>` com uma versão específica do pacote, tais como `3.0.0-beta5`. Versões válidas estão listadas nas páginas individuais do pacote na [NuGet.org](https://nuget.org).
