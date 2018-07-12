---
title: incluir ficheiro
description: incluir ficheiro
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 07/05/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: a7ed21d8246b618149aa0d116070a14b033d5370
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37869649"
---
## <a name="enable-event-grid-resource-provider"></a>Ativar o fornecedor de recursos do Event Grid

Se ainda não tiver utilizado anteriormente o Event Grid na sua subscrição do Azure, precisa de registar o fornecedor de recursos do Event Grid. Execute o seguinte comando:

```azurecli-interactive
az provider register --namespace Microsoft.EventGrid
```

Pode demorar algum tempo para o registo terminar. Para ver o estado, execute:

```azurecli-interactive
az provider show -n Microsoft.EventGrid --query "registrationState"
```

Quando `registrationState` está `Registered`, está pronto para continuar.