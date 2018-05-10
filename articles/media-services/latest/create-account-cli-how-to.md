---
title: Criar uma conta de Media Services do Azure com o CLI 2.0 | Microsoft Docs
description: Siga os passos para este início rápido para criar uma conta de Media Services do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: cflower
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 03/27/2018
ms.author: juliako
ms.openlocfilehash: a9660ac61bab9f8b9eb9563aab4cc584786b25ae
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="create-an-azure-media-services-account"></a>Criar uma conta dos Azure Media Services

Para começar a encriptar, codificar, análise, gerir e a transmissão em fluxo conteúdo de multimédia do Azure, terá de criar uma conta de Media Services. No momento em que cria uma conta de Media Services, também cria uma conta do Storage associada (ou utilize uma já existente) na mesma região geográfica da conta de Media Services.

Este tópico descreve os passos para criar uma nova conta de Media Services do Azure utilizando a CLI 2.0.  

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no [portal do Azure](http://portal.azure.com) e iniciar **CloudShell** para executar comandos da CLI, conforme mostrado nos passos.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Se optar por instalar e usar a CLI localmente, este tópico requer a execução da versão 2.0 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli). 

## <a name="set-the-azure-subscription"></a>Definir a subscrição do Azure

O comando seguinte, forneça o ID de subscrição do Azure que pretende utilizar para a conta de Media Services. Pode ver uma lista de subscrições que tem acesso ao navegar até [subscrições](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

```azurecli-interactive
az account set --subscription mySubscriptionId
```
 
[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]
 
## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Um ficheiro de fluxo](stream-files-dotnet-quickstart.md)
