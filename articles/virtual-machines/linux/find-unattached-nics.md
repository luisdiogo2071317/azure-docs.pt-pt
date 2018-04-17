---
title: Localizar e eliminar desanexados Azure NICs | Microsoft Docs
description: Como encontrar e eliminar as NICs do Azure que não estão anexados às VMs com o 2.0 CLI do Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/10/2018
ms.author: iainfou
ms.openlocfilehash: c730866fe73305a37b37038699a7f729085a16aa
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="how-to-find-and-delete-unattached-network-interface-cards-nics-for-azure-vms"></a>Como encontrar e eliminar a interface de rede desanexados placas (NICs) para as VMs do Azure
Quando elimina uma máquina virtual (VM) no Azure, as placas de interface de rede (NICs) não são eliminadas por predefinição. Se criar e eliminar várias VMs, os NICs dos continuam a utilizar os concessões de endereços IP internos. Como criar outros NICs de VM, poderá ser não é possível obter uma concessão IP no espaço de endereços da sub-rede. Este artigo mostra como encontrar e eliminar desanexados NICs.

## <a name="find-and-delete-unattached-nics"></a>Localizar e eliminar NICs desanexados

O *virtualMachine* propriedade para um NIC armazena o grupo de recursos e ID de VM a NIC está ligada a. O script seguinte repetido ao longo de todos os NICs numa subscrição e verifica se o *virtualMachine* propriedade é nula. Se esta propriedade for nula, o NIC não está ligado a uma VM.

Para ver todos os NICs desanexados, tem vivamente recomendado para ser executado primeiro o script com o *deleteUnattachedNics* variável à *0*. Para eliminar todos os NICs desanexados depois de analisar o resultado da lista, execute o script com *deleteUnattachedNics* para *1*.

```azurecli
# Set deleteUnattachedNics=1 if you want to delete unattached NICs
# Set deleteUnattachedNics=0 if you want to see the Id(s) of the unattached NICs
deleteUnattachedNics=0

unattachedNicsIds=$(az network nic list --query '[?virtualMachine==`null`].[id]' -o tsv)
for id in ${unattachedNicsIds[@]}
do
   if (( $deleteUnattachedNics == 1 ))
   then

       echo "Deleting unattached NIC with Id: "$id
       az network nic delete --ids $id
       echo "Deleted unattached NIC with Id: "$id
   else
       echo $id
   fi
done
```

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre como criar e gerir redes virtuais no Azure, consulte [criar e gerir redes VM](tutorial-virtual-network.md).
