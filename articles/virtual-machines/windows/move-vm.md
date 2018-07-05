---
title: Mover um recurso de VM do Windows no Azure | Documentos da Microsoft
description: Mova uma VM do Windows para outro Azure subscrição ou grupo de recursos no modelo de implementação do Resource Manager.
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
ms.openlocfilehash: 168ba57399b2649af29820f7321dd0151618346e
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436485"
---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Mover uma VM do Windows para outro grupo de subscrição ou ao recurso do Azure
Este artigo explica como mover uma VM do Windows entre grupos de recursos ou subscrições. A movimentação entre subscrições, pode ser útil se tiver criado originalmente uma VM numa subscrição de pessoal e agora quero movê-lo para a subscrição da sua empresa para continuar o seu trabalho.

> [!IMPORTANT]
>Não é possível mover o Managed Disks neste momento. 
>
>Novos IDs de recurso são criados como parte da transição. Assim que a VM foi movida, tem de atualizar as suas ferramentas e scripts para usar os novos IDs de recurso. 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a>Utilize o Powershell para mover uma VM

Para mover uma máquina virtual para outro grupo de recursos, terá de certificar-se de que também mova todos os recursos dependentes. Para utilizar o cmdlet Move-AzureRMResource, terá do ResourceId de cada um dos recursos. Pode obter uma lista de usar o ResourceId a [Get-AzureRMResource](/powershell/module/azurerm.resources/get-azurermresource) cmdlet.

```azurepowershell-interactive
 Get-AzureRMResource -ResourceGroupName <sourceResourceGroupName> | Format-table -Property ResourceId 
```

Para mover uma VM, precisamos de mover vários recursos. Podemos usar a saída de Get-AzureRMResource para criar uma lista separada por vírgulas a ResourceIds e transmiti-lo para [Move-AzureRMResource](/powershell/module/azurerm.resources/move-azurermresource) para movê-los para o destino. 

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


Será solicitado a confirmar que pretende mover os recursos especificados. 

## <a name="next-steps"></a>Passos Seguintes
Pode mover vários tipos de recursos entre grupos de recursos e subscrições. Para obter mais informações, consulte [Mover recursos para um novo grupo de recursos ou subscrição](../../resource-group-move-resources.md).    

