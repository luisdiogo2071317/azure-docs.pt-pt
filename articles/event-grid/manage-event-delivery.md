---
title: Entregues e as políticas de repetição para subscrições do Azure Event Grid
description: Descreve como personalizar as opções de entrega de eventos do Event Grid. Definir um destino de mensagens não entregues e o tempo que especificar para repetir a entrega.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/06/2019
ms.author: spelluru
ms.openlocfilehash: 8660f09c41cf6226f2ffb173508d37c260522b80
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54474256"
---
# <a name="dead-letter-and-retry-policies"></a>Entregues e as políticas de repetição

Ao criar uma subscrição de evento, pode personalizar as definições de entrega de eventos. Este artigo mostra-lhe como configurar uma localização entregues e personalizar as definições de repetição. Para obter informações sobre estas funcionalidades, consulte [entrega de mensagens do Event Grid e volte a tentar](delivery-and-retry.md).

## <a name="install-preview-feature"></a>Instale a funcionalidade de pré-visualização

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="set-dead-letter-location"></a>Definir localização de mensagens não entregues

Para definir uma localização entregues, precisa de uma conta de armazenamento para conter eventos que não podem ser entregue a um ponto de extremidade. Os exemplos de obtém o ID de recurso de uma conta de armazenamento existente. Eles criam uma subscrição de evento que utiliza um contentor nessa conta de armazenamento para o ponto final de mensagens não entregues.

### <a name="azure-cli"></a>CLI do Azure

```azurecli-interactive
# If you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

containername=testcontainer

topicid=$(az eventgrid topic show --name demoTopic -g gridResourceGroup --query id --output tsv)
storageid=$(az storage account show --name demoStorage --resource-group gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --source-resource-id $topicid \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --deadletter-endpoint $storageid/blobServices/default/containers/$containername
```

Para desativar as mensagens não entregues, execute novamente o comando para criar a subscrição de evento, mas não fornecer um valor para `deadletter-endpoint`. Não precisa de eliminar a subscrição de evento.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

$containername = "testcontainer"

$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id
$storageid = (Get-AzureRmStorageAccount -ResourceGroupName gridResourceGroup -Name demostorage).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -DeadLetterEndpoint "$storageid/blobServices/default/containers/$containername"
```

Para desativar as mensagens não entregues, execute novamente o comando para criar a subscrição de evento, mas não fornecer um valor para `DeadLetterEndpoint`. Não precisa de eliminar a subscrição de evento.

## <a name="set-retry-policy"></a>Definir política de repetição

Ao criar uma subscrição do Event Grid, pode definir valores para o tempo que o Event Grid deve tentar entregar o evento. Por predefinição, o Event Grid tenta durante 24 horas (1440 minutos) ou 30 vezes. Pode definir qualquer um destes valores para a sua subscrição do event grid. O valor de tempo de vida de evento tem de ser um número inteiro entre 1 a 1440. O valor de repetições máximas tem de ser um número inteiro de 1 para 30.

Não é possível configurar o [Repita agenda](delivery-and-retry.md#retry-schedule-and-duration).

### <a name="azure-cli"></a>CLI do Azure

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

Para definir as repetições máximas para um valor diferente de 30, utilize:

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --max-delivery-attempts 18
```

Se definir ambos `event-ttl` e `max-deliver-attempts`, Event Grid utiliza o primeiro a expirar para determinar quando parar a entrega de eventos.

### <a name="powershell"></a>PowerShell

Para definir o evento time-to-live com um valor diferente 1440 minutos, utilize:

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -EventTtl 720
```

Para definir as repetições máximas para um valor diferente de 30, utilize:

```azurepowershell-interactive
$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name demoTopic).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -MaxDeliveryAttempt 18
```

Se definir ambos `EventTtl` e `MaxDeliveryAttempt`, Event Grid utiliza o primeiro a expirar para determinar quando parar a entrega de eventos.

## <a name="next-steps"></a>Passos Seguintes

* Para uma aplicação de exemplo que utiliza uma aplicação de função do Azure para processar entregues eventos, consulte [do Azure Event Grid entregues exemplos para .NET](https://azure.microsoft.com/resources/samples/event-grid-dotnet-handle-deadlettered-events/).
* Para obter informações sobre a entrega de eventos e repetições, [entrega de mensagens do Event Grid e volte a tentar](delivery-and-retry.md).
* Para obter uma introdução ao Event Grid, veja [Sobre o Azure Event Grid](overview.md).
* Para começar rapidamente com o Event Grid, veja [criar e encaminhar eventos personalizados com o Azure Event Grid](custom-event-quickstart.md).
