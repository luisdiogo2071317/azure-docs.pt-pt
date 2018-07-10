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
ms.openlocfilehash: 0946b703b45f69ede409167213f2629a796f18b5
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37869632"
---
## <a name="enable-event-grid-resource-provider"></a>Ativar o fornecedor de recursos do Event Grid

Se ainda não tiver utilizado anteriormente o Event Grid na sua subscrição do Azure, precisa de registar o fornecedor de recursos do Event Grid. Execute o seguinte comando:

```azurepowershell-interactive
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.EventGrid
```

Pode demorar algum tempo para o registo terminar. Para ver o estado, execute:

```azurepowershell-interactive
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.EventGrid
```

Quando `RegistrationStatus` está `Registered`, está pronto para continuar.