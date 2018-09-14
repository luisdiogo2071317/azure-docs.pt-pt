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
ms.date: 09/12/2018
ms.author: cynthn
ms.openlocfilehash: 1daf04e3f878d0748bfa0904259c7b7187481843
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45580494"
---
# <a name="move-a-windows-vm-to-another-azure-subscription-or-resource-group"></a>Mover uma VM do Windows para outro grupo de subscrição ou ao recurso do Azure
Este artigo explica como mover uma máquina virtual de Windows (VM) entre grupos de recursos ou subscrições. A movimentação entre subscrições, pode ser útil se tiver criado originalmente uma VM numa subscrição de pessoal e agora quero movê-lo para a subscrição da sua empresa para continuar o seu trabalho.

> [!IMPORTANT]
>Não é possível mover o Managed Disks do Azure neste momento. 
>
>Novos IDs de recurso são criados como parte da transição. Depois da VM foi movida, terá de atualizar as suas ferramentas e scripts para usar os novos IDs de recurso. 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-to-move-a-vm"></a>Utilize o Powershell para mover uma VM

Para mover uma máquina virtual para outro grupo de recursos, terá de certificar-se de que também mova todos os recursos dependentes. Para obter uma lista com o ID de recurso de cada um destes recursos, utilize o [Get-AzureRMResource](/powershell/module/azurerm.resources/get-azurermresource) cmdlet.

```azurepowershell-interactive
 Get-AzureRMResource -ResourceGroupName <sourceResourceGroupName> | Format-table -Property ResourceId 
```

Pode utilizar a saída do comando anterior como uma lista separada por vírgulas de IDs de recurso para [Move-AzureRMResource](/powershell/module/azurerm.resources/move-azurermresource) para mover cada recurso para o destino. 

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


Quando lhe for pedido que confirme que pretende mover os recursos especificados, introduza **Y** para confirmar.

## <a name="next-steps"></a>Passos Seguintes
Pode mover vários tipos de recursos entre grupos de recursos e subscrições. Para obter mais informações, consulte [mover recursos para um novo grupo de recursos ou subscrição](../../resource-group-move-resources.md).    

