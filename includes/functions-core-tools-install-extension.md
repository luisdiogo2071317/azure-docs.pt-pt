---
title: incluir ficheiro
description: incluir ficheiro
services: functions
author: ggailey777
ms.service: functions
ms.topic: include
ms.date: 09/21/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: f1b53c53b1e5fb089eb9b8a9b816b11a1eea126d
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47044514"
---
Ao desenvolver funções localmente, pode instalar as extensões que necessita, utilizando as ferramentas de núcleo de funções do Azure a partir do Terminal ou a partir de um prompt de comando.

Depois de atualizar seus *Function* arquivo para incluir todas as ligações que precisa de sua função, execute o seguinte comando na pasta de projeto.

```bash
func extensions install
```

O comando lê a *Function* arquivo para ver os pacotes que precisa, instala-os e recria o projeto de extensões. Ele adiciona todos os enlaces novo na versão atual, mas não atualiza "BIND" existente. Utilize o `--force` opção para atualizar "BIND" existente para a versão mais recente ao instalar novas etiquetas.

Se pretender instalar uma versão específica de um pacote ou se quiser instalar pacotes antes de editar a *Function* do ficheiro, utilize o `func extensions install` comando com o nome do pacote, conforme mostrado no exemplo a seguir:

```bash
func extensions install --package Microsoft.Azure.WebJobs.ServiceBus --version <target_version>
```

Substitua `<target_version>` com uma versão específica do pacote, tais como `3.0.0-beta5`. Versões válidas estão listadas nas páginas individuais do pacote na [NuGet.org](https://nuget.org).
