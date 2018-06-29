---
title: Gerir as definições de entrega para subscrições de grelha de eventos do Azure
description: Descreve como personalizar opções de entrega de eventos de grelha de eventos.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: tomfitz
ms.openlocfilehash: 65e79f492e96c418502e096b60992ba992868dd7
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37036505"
---
# <a name="manage-event-grid-delivery-settings"></a>Gerir as definições de entrega de eventos de grelha

Ao criar uma subscrição de evento, poderá personalizar as definições para a entrega de eventos. Pode definir quanto a eventos grelha tentar entregar a mensagem. Pode definir uma conta de armazenamento a utilizar para armazenar eventos que não não possível entregar para um ponto final.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="set-retry-policy"></a>Política de repetição de conjunto

Ao criar uma subscrição de evento grelha, pode definir valores para o período de tempo eventos grelha deve tentar entregar o evento. Por predefinição, o evento grelha tentativas durante 24 horas (1440 minutos) e tenta um máximo de 30 vezes. Pode definir um destes valores para a sua subscrição da grelha de eventos.

Para definir o evento time-to-live para um valor diferente de 1440 minutos, utilize:

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
  --event-ttl 720
```

Para definir as máximo de tentativas para um valor diferente de 30, utilize:

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
  --max-delivery-attempts 18
```

Se definir ambos `event-ttl` e `max-deliver-attempts`, grelha de evento utiliza o primeiro para expirar para tentativas de repetição.

## <a name="set-dead-letter-location"></a>Definir localização do entregues

Quando a grelha de eventos não é possível fornecer um evento, qual pode enviar o evento undelivered para uma conta de armazenamento. Este processo é conhecido como mensagens não entregues. Por predefinição, não ative a grelha de evento de mensagens não entregues. Para ativá-lo, tem de especificar uma conta de armazenamento para armazenar eventos undelivered ao criar a subscrição de evento. Solicitar eventos a partir desta conta de armazenamento para resolver entregas.

Grelha de evento enviará um evento para a localização de entregues se atingir todos os respetivos de tentativas de repetição ou, se receber uma mensagem de erro que indica o entrega nunca será bem sucedida. Por exemplo, se a grelha de evento recebe um erro de formato incorrecto quando distribui um evento, imediatamente envia esse evento para a localização de entregues.

Antes de definir a localização de mensagens não entregues, tem de ter uma conta de armazenamento com um contentor. Forneça o ponto final para este contentor ao criar a subscrição de evento. O ponto final está no formato: `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>/blobServices/default/containers/<container-name>`

O script seguinte obtém o ID de recurso de uma conta de armazenamento existente e cria uma subscrição de evento que utiliza um contentor nessa conta de armazenamento para o ponto final entregues.

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

storagename=demostorage
containername=testcontainer

storageid=$(az storage account show --name $storagename --resource-group gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL>
  --deadletter-endpoint $storageid/blobServices/default/containers/$containername
```

Para utilizar a grelha de eventos para responder a eventos undelivered, [, crie uma subscrição de evento](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) para o armazenamento de BLOBs de entregues. Sempre que o armazenamento de BLOBs de entregues recebe um evento undelivered, o evento grelha notifica o processador. O processador responde com as ações que pretende tirar para reconciliar undelivered eventos. 

## <a name="next-steps"></a>Passos Seguintes

* Para obter informações sobre a entrega de eventos e de tentativas, [entrega de mensagens de evento grelha e volte a tentar](delivery-and-retry.md).
* Para obter uma introdução ao Event Grid, veja [Sobre o Azure Event Grid](overview.md).
* Para rapidamente começar a utilizar a grelha de eventos, consulte o artigo [criar e rota eventos personalizados com o Azure eventos grelha](custom-event-quickstart.md).
