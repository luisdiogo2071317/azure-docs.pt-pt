---
title: Consultar subscrições do Azure Event Grid
description: Descreve como listar as subscrições do Azure Event Grid.
services: event-grid
author: spelluru
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: spelluru
ms.openlocfilehash: ac43b85858451149ceabf87c77b42d40fbd4eac4
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54470989"
---
# <a name="query-event-grid-subscriptions"></a>Consultar subscrições do Event Grid 

Este artigo descreve como listar as subscrições do Event Grid na sua subscrição do Azure. Ao consultar as suas subscrições do Event Grid existentes, é importante compreender os diferentes tipos de subscrições. Fornecer parâmetros diferentes com base no tipo de subscrição que pretende obter.

## <a name="resource-groups-and-azure-subscriptions"></a>Grupos de recursos e subscrições do Azure

As subscrições do Azure e grupos de recursos não são recursos do Azure. Por conseguinte, subscrições do event grid para grupos de recursos ou subscrições do Azure não tem as mesmas propriedades de subscrições do event grid para recursos do Azure. Subscrições do Event grid para grupos de recursos ou subscrições do Azure são consideradas global.

Para obter as subscrições do event grid para uma subscrição do Azure e os respetivos grupos de recursos, não precisa fornecer quaisquer parâmetros. Certifique-se de que selecionou a subscrição do Azure que pretende consultar. Os exemplos a seguir não obtém as subscrições do event grid para tópicos personalizados ou de recursos do Azure.

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

Para obter as subscrições do event grid para uma subscrição do Azure, forneça o tipo de tópico **Microsoft.Resources.Subscriptions**.

Para a CLI do Azure, utilize:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.Resources.Subscriptions"
```

Para o PowerShell, utilize:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -TopicTypeName "Microsoft.Resources.Subscriptions"
```

Para obter as subscrições do event grid para todos os grupos de recursos dentro de uma subscrição do Azure, forneça o tipo de tópico **Microsoft.Resources.ResourceGroups**.

Para a CLI do Azure, utilize:

```azurecli-interactive
az eventgrid event-subscription list --topic-type-name "Microsoft.Resources.ResourceGroups"
```

Para o PowerShell, utilize:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -TopicTypeName "Microsoft.Resources.ResourceGroups"
```

Para obter as subscrições do event grid para um grupo de recursos especificado, forneça o nome do grupo de recursos como um parâmetro.

Para a CLI do Azure, utilize:

```azurecli-interactive
az eventgrid event-subscription list --resource-group myResourceGroup
```

Para o PowerShell, utilize:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -ResourceGroupName myResourceGroup
```

## <a name="custom-topics-and-azure-resources"></a>Tópicos personalizados e recursos do Azure

Tópicos de personalizado do Event grid são recursos do Azure. Portanto, consultar subscrições do event grid para tópicos personalizados e outros recursos, como conta de armazenamento de BLOBs, da mesma forma. Para obter as subscrições do event grid para tópicos personalizados, tem de fornecer os parâmetros que identificar o recurso ou identificar a localização do recurso. Não é possível amplamente consulta subscrições do event grid para os recursos na sua subscrição do Azure.

Para obter as subscrições do event grid para tópicos personalizados e outros recursos num local, forneça o nome da localização.

Para a CLI do Azure, utilize:

```azurecli-interactive
az eventgrid event-subscription list --location westus2
```

Para o PowerShell, utilize:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -Location westus2
```

Para obter as subscrições de tópicos personalizados para uma localização, forneça a localização e o tipo de tópico **Microsoft.EventGrid.Topics**.

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

Para obter as subscrições do event grid para um tópico personalizado, forneça o nome do tópico personalizado e o nome do seu grupo de recursos.

Para a CLI do Azure, utilize:

```azurecli-interactive
az eventgrid event-subscription list --topic-name myCustomTopic --resource-group myResourceGroup
```

Para o PowerShell, utilize:

```azurepowershell-interactive
Get-AzureRmEventGridSubscription -TopicName myCustomTopic -ResourceGroupName myResourceGroup
```

Para obter as subscrições do event grid para um recurso em particular, forneça o ID de recurso.

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

* Para obter informações sobre a entrega de eventos e repetições, [entrega de mensagens do Event Grid e volte a tentar](delivery-and-retry.md).
* Para obter uma introdução ao Event Grid, veja [Sobre o Azure Event Grid](overview.md).
* Para começar rapidamente com o Event Grid, veja [criar e encaminhar eventos personalizados com o Azure Event Grid](custom-event-quickstart.md).
