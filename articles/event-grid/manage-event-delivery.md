---
title: Entregues e as políticas de repetição para subscrições do Azure Event Grid
description: Descreve como personalizar as opções de entrega de eventos do Event Grid. Definir um destino de mensagens não entregues e o tempo que especificar para repetir a entrega.
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: tomfitz
ms.openlocfilehash: 5db53567b1df9e726fb0c507f0302536ea45b7f4
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45603775"
---
# <a name="dead-letter-and-retry-policies"></a>Entregues e as políticas de repetição

Ao criar uma subscrição de evento, pode personalizar as definições de entrega de eventos. Este artigo mostra-lhe como configurar uma localização entregues e personalizar as definições de repetição. Para obter informações sobre estas funcionalidades, consulte [entrega de mensagens do Event Grid e volte a tentar](delivery-and-retry.md).

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="set-dead-letter-location"></a>Definir localização de mensagens não entregues

Para definir uma localização entregues, precisa de uma conta de armazenamento para conter eventos que não podem ser entregue a um ponto de extremidade. O script seguinte obtém o ID de recurso de uma conta de armazenamento existente e cria uma subscrição de evento que utiliza um contentor nessa conta de armazenamento para o ponto final de mensagens não entregues.

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
