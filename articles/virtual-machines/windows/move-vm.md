---
title: Mover um recurso de VM do Windows no Azure | Microsoft Docs
description: Mova uma VM do Windows para outro Azure subscrição ou grupo de recursos no modelo de implementação Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2017
ms.author: cynthn
ms.openlocfilehash: b98b8c947fb34b60c7bd27b006672e0e9d923d3b
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Mover uma VM do Windows para outro grupo de subscrição ou recurso do Azure
Este artigo explica como mover uma VM do Windows entre grupos de recursos ou subscrições. Mover entre subscrições pode ser útil se tiver uma VM originalmente criado numa subscrição pessoal e agora pretende movê-la para a subscrição da sua empresa para continuar o trabalho.

> [!IMPORTANT]
>Não é possível mover discos geridos neste momento. 
>
>Novo IDs de recurso são criados como parte da transição. Assim que a VM foi movida, terá de atualizar as suas ferramentas e scripts para utilizar o novo IDs de recurso. 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a>Utilize o Powershell para mover uma VM

Para mover uma máquina virtual para outro grupo de recursos, tem de certificar-se de que também mova todos os recursos dependentes. Para utilizar o cmdlet Move-AzureRMResource, terá do ResourceId de cada um dos recursos. Pode obter uma lista de utilizar o ResourceId do [localizar AzureRMResource](/powershell/module/azurerm.resources/find-azurermresource) cmdlet.

```azurepowershell-interactive
 Find-AzureRMResource -ResourceGroupNameContains <sourceResourceGroupName> | Format-table -Property ResourceId 
```

Para mover uma VM, é necessário mover vários recursos. Podemos utilizar o resultado de localizar AzureRMResource para criar uma lista separada por vírgulas do ResourceIds e passar que para [mover AzureRMResource](/powershell/module/azurerm.resources/move-azurermresource) ao movê-los para o destino. 

```azurepowershell-interactive

Move-AzureRmResource -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```
    
Para mover os recursos para uma subscrição diferente, inclua o **- DestinationSubscriptionId** parâmetro. 

```azurepowershell-interactive
Move-AzureRmResource -DestinationSubscriptionId "<myDestinationSubscriptionID>" `
    -DestinationResourceGroupName "<myDestinationResourceGroup>" `
    -ResourceId <myResourceId,myResourceId,myResourceId>
```


Será pedido para confirmar que pretende mover os recursos especificados. 

## <a name="next-steps"></a>Passos Seguintes
Pode mover vários tipos de recursos entre grupos de recursos e as subscrições. Para obter mais informações, consulte [Mover recursos para um novo grupo de recursos ou subscrição](../../resource-group-move-resources.md).    

