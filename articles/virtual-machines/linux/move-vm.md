---
title: Mover uma VM com Linux no Azure | Microsoft Docs
description: Mova uma VM com Linux para outro Azure subscrição ou grupo de recursos no modelo de implementação Resource Manager.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: d635f0a5-4458-4b95-a5f8-eed4f41eb4d4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 12/14/2017
ms.author: cynthn
ms.openlocfilehash: a4a7dd5541fe298675232ffa803f749e71f6a03f
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30907502"
---
# <a name="move-a-linux-vm-to-another-subscription-or-resource-group"></a>Mover uma VM com Linux para outro subscrição ou grupo de recursos
Este artigo explica como mover uma VM com Linux entre grupos de recursos ou subscrições. Mover uma VM entre subscrições pode ser útil se tiver criado uma VM numa subscrição pessoal e agora pretende movê-la para a subscrição da sua empresa.

> [!IMPORTANT]
>Não é possível mover discos geridos neste momento. 
>
>Novo IDs de recurso são criados como parte da transição. Assim que a VM foi movida, terá de atualizar as suas ferramentas e scripts para utilizar o novo IDs de recurso. 
> 
> 

## <a name="use-the-azure-cli-to-move-a-vm"></a>Utilizar a CLI do Azure para mover uma VM


Antes de pode mover VM utilizando a CLI, tem de certificar-se de que as subscrições de origem e de destino existem no mesmo inquilino. Para verificar que ambas as subscrições têm o mesmo ID de inquilino, utilize [mostrar de conta az](/cli/azure/account#az_account_show).

```azurecli-interactive
az account show --subscription mySourceSubscription --query tenantId
az account show --subscription myDestinationSubscription --query tenantId
```
Se o inquilino IDs para as subscrições de origem e de destino não forem iguais, tem de contactar o [suporta](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) para mover os recursos para um novo inquilino.

Para mover com êxito uma VM, tem de mover a VM e todos os respetivos recursos de suporte. Utilize o [lista de recursos de az](/cli/azure/resource#az_resource_list) comando para listar todos os recursos no grupo de recursos e os respetivos IDs. Ajuda a encaminhar a saída deste comando para um ficheiro para que possa copiar e colar os IDs para comandos posteriores.

```azurecli-interactive
az resource list --resource-group "mySourceResourceGroup" --query "[].{Id:id}" --output table
```

Para mover uma VM e os respetivos recursos para outro grupo de recursos, utilize [movimentação de recurso az](/cli/azure/resource#az_resource_move). O exemplo seguinte mostra como mover uma VM e os recursos mais comuns requer. Utilize o **-ids** parâmetro e passar numa lista separada por vírgulas (sem espaços) de IDs de para os recursos a mover.

```azurecli-interactive
vm=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
nic=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkInterfaces/myNIC
nsg=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNSG
pip=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIPAddress
vnet=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Network/virtualNetworks/myVNet
diag=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mydiagnosticstorageaccount
storage=/subscriptions/mySourceSubscriptionID/resourceGroups/mySourceResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageacountname    

az resource move \
    --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag \
    --destination-group "myDestinationResourceGroup"
```

Se pretender mover VM e os respetivos recursos para uma subscrição diferente, adicione o **– subscriptionId destino** parâmetro para especificar a subscrição de destino.

Se lhe for pedido que confirme que pretende mover o recurso especificado. Tipo **Y** para confirmar que pretende mover os recursos.

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Passos Seguintes
Pode mover vários tipos de recursos entre grupos de recursos e as subscrições. Para obter mais informações, consulte [Mover recursos para um novo grupo de recursos ou subscrição](../../resource-group-move-resources.md).    

