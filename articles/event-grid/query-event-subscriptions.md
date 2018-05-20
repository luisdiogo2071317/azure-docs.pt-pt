---
title: Subscrições do Azure grelha de evento de consulta
description: Descreve como lista de subscrições de grelha de eventos do Azure.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 04/04/2018
ms.author: tomfitz
ms.openlocfilehash: 2b46cde4a352e647ee97669f116a6c1926879fa0
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="query-event-grid-subscriptions"></a>Subscrições de grelha de evento de consulta 

Este artigo descreve como listar as subscrições da grelha de evento na sua subscrição do Azure. Ao consultar as subscrições da grelha de evento existentes, é importante compreender os diferentes tipos de subscrições. Fornecer parâmetros diferentes com base no tipo de subscrição que pretende obter.

## <a name="resource-groups-and-azure-subscriptions"></a>Grupos de recursos e as subscrições do Azure

As subscrições do Azure e grupos de recursos não são recursos do Azure. Por conseguinte, as subscrições da grelha de eventos para grupos de recursos ou subscrições do Azure não têm as mesmas propriedades de subscrições de grelha de eventos nos recursos do Azure. Subscrições de grelha de eventos para grupos de recursos ou subscrições do Azure são consideradas global.

Para obter as subscrições de grelha de eventos para uma subscrição do Azure e os respetivos grupos de recursos, não precisa de fornecer os parâmetros. Certifique-se de que selecionou a subscrição do Azure que pretende consultar. Os exemplos seguintes não obter subscrições de grelha de eventos para tópicos personalizados ou de recursos do Azure.

Para a CLI do Azure, utilize:

```azurecli-interactive
az account set -s "My Azure Subscription"
az eventgrid event-subscription list
```

Para o PowerShell, utilize:

```azurepowershell-interactive
Set-AzureRmContext -Subscription "My Azure Subscription"
Get-AzureRmEventGridSubscription
```

Para obter as subscrições da grelha de eventos para uma subscrição do Azure, forneça o tipo de tópico **Microsoft.Resources.Subscriptions**.

Para a CLI do Azure, utilize:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.Resources.Subscriptions"
```

Para o PowerShell, utilize:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -TopicTypeName "Microsoft.Resources.Subscriptions"
```

Para obter as subscrições da grelha de eventos para todos os grupos de recursos dentro de uma subscrição do Azure, forneça o tipo de tópico **Microsoft.Resources.ResourceGroups**.

Para a CLI do Azure, utilize:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.Resources.ResourceGroups"
```

Para o PowerShell, utilize:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -TopicTypeName "Microsoft.Resources.ResourceGroups"
```

Para obter as subscrições da grelha de eventos para um grupo de recursos especificado, forneça o nome do grupo de recursos como um parâmetro.

Para a CLI do Azure, utilize:

```azurecli-interactive
az eventgrid event-subscription list --resource-group myResourceGroup
```

Para o PowerShell, utilize:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -ResourceGroupName myResourceGroup
```

## <a name="custom-topics-and-azure-resources"></a>Tópicos personalizados e de recursos do Azure

Tópicos de personalizada de grelha de evento são recursos do Azure. Por conseguinte, consultar o evento grelha subscrições tópicos personalizados e outros recursos, como a conta do Blob storage, da mesma forma. Para obter as subscrições de grelha de eventos para tópicos personalizados, tem de fornecer os parâmetros que identificam o recurso ou para identificam a localização do recurso. Não é possível amplamente consulta eventos grelha subscrições para os recursos na sua subscrição do Azure.

Para obter as subscrições de grelha de eventos para tópicos personalizados e outros recursos numa localização, forneça o nome da localização.

Para a CLI do Azure, utilize:

```azurecli-interactive
az eventgrid event-subscription list --location westus2
```

Para o PowerShell, utilize:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -Location westus2
```

Para obter subscrições de tópicos personalizadas para uma localização, forneça a localização e o tipo de tópico **Microsoft.EventGrid.Topics**.

Para a CLI do Azure, utilize:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.EventGrid.Topics" --location "westus2"
```

Para o PowerShell, utilize:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -TopicTypeName "Microsoft.EventGrid.Topics" -Location westus2
```

Para obter as subscrições para contas de armazenamento para uma localização, forneça a localização e o tipo de tópico **Microsoft.Storage.StorageAccounts**.

Para a CLI do Azure, utilize:

```azurecli-interactive
az eventgrid event-subscription list --topic-type "Microsoft.Storage.StorageAccounts" --location westus2
```

Para o PowerShell, utilize:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -TopicTypeName "Microsoft.Storage.StorageAccounts" -Location westus2
```

Para obter as subscrições da grelha de eventos para um tópico personalizado, forneça o nome do tópico personalizado e o nome do respetivo grupo de recursos.

Para a CLI do Azure, utilize:

```azurecli-interactive
az eventgrid event-subscription list --topic-name myCustomTopic --resource-group myResourceGroup
```

Para o PowerShell, utilize:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -TopicName myCustomTopic -ResourceGroupName myResourceGroup
```

Para obter as subscrições da grelha de eventos para um recurso específico, forneça o ID de recurso.

Para a CLI do Azure, utilize:

```azurecli-interactive
resourceid=$(az resource show -n mystorage -g myResourceGroup --resource-type "Microsoft.Storage/storageaccounts" --query id --output tsv)
az eventgrid event-subscription list --resource-id $resourceid
```

Para o PowerShell, utilize:

```azurepowershell-interactive
$resourceid = (Get-AzureRmResource -Name mystorage -ResourceGroupName myResourceGroup).ResourceId
Get-AzureRmEventGridSubscription -ResourceId $resourceid
```

## <a name="next-steps"></a>Passos Seguintes

* Para obter informações sobre a entrega de eventos e de tentativas, [entrega de mensagens de evento grelha e volte a tentar](delivery-and-retry.md).
* Para uma introdução à grelha de eventos, consulte [sobre eventos grelha](overview.md).
* Para rapidamente começar a utilizar a grelha de eventos, consulte o artigo [criar e rota eventos personalizados com o Azure eventos grelha](custom-event-quickstart.md).
