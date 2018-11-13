---
title: Exemplo de script da CLI do Azure - Subscrever uma conta de armazenamento de Blobs | Microsoft Docs
description: Exemplo de script da CLI do Azure - Subscrever uma conta de armazenamento de Blobs
services: event-grid
documentationcenter: na
author: tfitzmac
ms.service: event-grid
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2018
ms.author: tomfitz
ms.openlocfilehash: 3ecf5cd0906e04625f44584d0b668f764665075e
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51037870"
---
# <a name="subscribe-to-events-for-a-blob-storage-account-with-azure-cli"></a>Subscrever eventos para uma conta de armazenamento de Blobs com a CLI do Azure

Este script cria uma subscrição do Event Grid para os eventos para uma conta de armazenamento de Blobs. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

O script de exemplo de pré-visualização exige a extensão do Event Grid. Para instalar, execute `az extension add --name eventgrid`.

## <a name="sample-script---stable"></a>Script de exemplo - estável

[!code-azurecli[main](../../../cli_scripts/event-grid/subscribe-to-blob-storage/subscribe-to-blob-storage.sh "Subscribe to Blob storage")]

## <a name="sample-script---preview-extension"></a>Script de exemplo - extensão de pré-visualização

[!code-azurecli[main](../../../cli_scripts/event-grid/subscribe-to-blob-storage-preview/subscribe-to-blob-storage-preview.sh "Subscribe to Blob storage")]

## <a name="script-explanation"></a>Explicação do script

Este script utiliza o seguinte comando para criar a subscrição de eventos. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [az eventgrid event-subscription create](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription#az-eventgrid-event-subscription-create) | Crie uma subscrição do Event Grid. |
| [az eventgrid event-subscription create](/cli/azure/ext/eventgrid/eventgrid/event-subscription#ext-eventgrid-az-eventgrid-event-subscription-create) - versão de extensão | Crie uma subscrição do Event Grid. |

## <a name="next-steps"></a>Passos seguintes

* Para obter informações sobre a consulta de subscrições, veja [Consultar subscrições do Event Grid](../query-event-subscriptions.md).
* Para obter mais informações sobre a CLI do Azure, veja [Documentação da CLI do Azure](https://docs.microsoft.com/cli/azure).
