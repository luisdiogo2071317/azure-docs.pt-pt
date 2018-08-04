---
title: Entregues e as políticas de repetição para subscrições do Azure Event Grid
description: Descreve como personalizar as opções de entrega de eventos do Event Grid. Definir um destino de mensagens não entregues e o tempo que especificar para repetir a entrega.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: tomfitz
ms.openlocfilehash: 5a37fadc179157ba590b31a79fcd98f223cb1869
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39501954"
---
# <a name="dead-letter-and-retry-policies"></a>Entregues e as políticas de repetição

Ao criar uma subscrição de evento, pode personalizar as definições de entrega de eventos. Pode definir o tempo que o Event Grid tenta entregar a mensagem. Pode definir uma conta de armazenamento a utilizar para armazenar eventos que não podem ser entregue a um ponto de extremidade.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="set-dead-letter-location"></a>Definir localização de mensagens não entregues

Quando o Event Grid não é possível entregar um evento, ele pode enviar o evento não serão entregues para uma conta de armazenamento. Este processo é conhecido como mensagens não entregues. Por predefinição, não ative Event Grid mensagens não entregues. Para ativá-la, tem de especificar uma conta de armazenamento para armazenar eventos não serão entregues ao criar a subscrição de evento. Obter eventos a partir desta conta de armazenamento para resolver entregas.

Grelha de eventos envia um evento para a localização de mensagens não entregues, se ele tiver tentado todas suas tentativas de repetição, ou se for recebida uma mensagem de erro que indica a entrega nunca terá êxito. Por exemplo, se grelha de eventos receber um erro de formato incorrecto ao fornecer um evento, ele envia esse evento para a localização de mensagens não entregues. Existe um atraso de cinco minutos entre a última tentativa de fornecer um evento e quando ela é entregue para a localização de mensagens não entregues. Este atraso destina-se para reduzir o número de operações de armazenamento Blob. Se a localização de mensagens não entregues não está disponível para quatro horas, o evento foi removido.

Antes de definir a localização de mensagens não entregues, tem de ter uma conta de armazenamento com um contentor. Forneça o ponto final para esse contentor ao criar a subscrição de evento. O ponto final está no formato de: `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>/blobServices/default/containers/<container-name>`

O script seguinte obtém o ID de recurso de uma conta de armazenamento existente e cria uma subscrição de evento que utiliza um contentor nessa conta de armazenamento para o ponto final de mensagens não entregues.

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
  --endpoint <endpoint_URL> \
  --deadletter-endpoint $storageid/blobServices/default/containers/$containername
```

Para utilizar o Event Grid para responder a eventos não serão entregues, [criar uma subscrição de evento](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) para o armazenamento de BLOBs de mensagens não entregues. Sempre que o seu armazenamento de BLOBs de mensagens não entregues recebe um evento não serão entregues, o Event Grid notifica o manipulador. O manipulador responde com as ações que deseja assumir de reconciliar os eventos não serão entregues. 

Para desativar as mensagens não entregues, execute novamente o comando para criar a subscrição de evento, mas não fornecer um valor para `deadletter-endpoint`. Não precisa de eliminar a subscrição de evento.

## <a name="set-retry-policy"></a>Definir política de repetição

Ao criar uma subscrição do Event Grid, pode definir valores para o tempo que o Event Grid deve tentar entregar o evento. Por predefinição, o Event Grid tentativas durante 24 horas (1440 minutos) e tenta um máximo de 30 vezes. Pode definir qualquer um destes valores para a sua subscrição do event grid. O valor de tempo de vida de evento tem de ser um número inteiro entre 1 a 1440. O valor para tentativas de entrega máxima tem de ser um número inteiro de 1 para 30.

Não é possível configurar o [intervalo de repetição](delivery-and-retry.md#retry-intervals-and-duration).

Para definir o evento time-to-live com um valor diferente 1440 minutos, utilize:

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --event-ttl 720
```

Para definir o máximo de tentativas de tentativas para um valor diferente de 30, utilize:

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --max-delivery-attempts 18
```

Se definir ambos `event-ttl` e `max-deliver-attempts`, Event Grid utiliza o primeiro a expirar para tentativas de repetição.

## <a name="next-steps"></a>Passos Seguintes

* Para uma aplicação de exemplo que utiliza uma aplicação de função do Azure para processar entregues eventos, consulte [do Azure Event Grid entregues exemplos para .NET](https://azure.microsoft.com/resources/samples/event-grid-dotnet-handle-deadlettered-events/).
* Para obter informações sobre a entrega de eventos e repetições, [entrega de mensagens do Event Grid e volte a tentar](delivery-and-retry.md).
* Para obter uma introdução ao Event Grid, veja [Sobre o Azure Event Grid](overview.md).
* Para começar rapidamente com o Event Grid, veja [criar e encaminhar eventos personalizados com o Azure Event Grid](custom-event-quickstart.md).
