---
title: incluir ficheiro
description: incluir ficheiro
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/27/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 1b553cbd720fcb76899844712ce5053af46f7ccb
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452960"
---
## <a name="deploy-the-function-app-project-to-azure"></a>Implementar o projeto da aplicação de funções no Azure

Depois da criação da aplicação de funções no Azure, pode utilizar o comando [`func azure functionapp publish`](../articles/azure-functions/functions-run-local.md#project-file-deployment) para implementar o código do projeto no Azure.

```bash
func azure functionapp publish <FunctionAppName>
```

Será apresentado algo semelhante ao resultado seguinte, que foi truncado para facilitar a leitura.

```output
Getting site publishing info...

...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
```

Agora, pode testar as suas funções no Azure.