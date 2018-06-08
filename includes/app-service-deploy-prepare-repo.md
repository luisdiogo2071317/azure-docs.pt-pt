---
title: incluir ficheiro
description: incluir ficheiro
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/05/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 1c2a4f1e463fff278981de2297662a94cca8944e
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "34850813"
---
## <a name="prepare-your-repository"></a>Preparar o seu repositório

Para obter compilações automáticas do servidor de compilação do Kudu do serviço de aplicações, certifique-se de que a raiz do repositório tem os ficheiros corretos no seu projeto.

| Runtime | Ficheiros do diretório de raiz |
|-|-|
| ASP.NET (apenas Windows) | _*.sln_, _*.csproj_, ou _default.aspx_ |
| Núcleo de ASP.NET | _*.sln_ ou _*.csproj_ |
| PHP | _index.php_ |
| Ruby (apenas Linux) | _Gemfile_ |
| Node.js | _Server.js_, _app.js_, ou _Package. JSON_ com um script de início |
| Python (apenas Windows) | _\*. PY_, _requirements.txt_, ou _runtime.txt_ |
| HTML | _default.htm_, _default.html_, _default.asp_, _index.htm_, _index.html_, ou  _iisstart.htm_ |
| Trabalhos Web | _\<job_name > / executar. \<extensão >_ em _aplicação\_dados/tarefas/contínua_ (para WebJobs contínuos) ou _aplicação\_dados/tarefas/acionada_ (para acionada WebJobs). Para obter mais informações, consulte [documentação de WebJobs do Kudu](https://github.com/projectkudu/kudu/wiki/WebJobs) |
| Funções | Consulte [a implementação contínua para as funções do Azure](../articles/azure-functions/functions-continuous-deployment.md#continuous-deployment-requirements). |

Para personalizar a sua implementação, incluir uma _.deployment_ ficheiro na raiz do repositório. Para obter mais informações, consulte [personalizar implementações](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) e [script de implementação personalizada](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).

> [!NOTE]
> Permitir que o se desenvolver no Visual Studio, [Visual Studio criar um repositório para si](/vsts/git/tutorial/creatingrepo?view=vsts&tabs=visual-studio). O projeto é imediatamente pronto para ser implementada utilizando o Git.
>
>

