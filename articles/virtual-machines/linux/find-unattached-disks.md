---
title: Localizar e eliminar desanexados discos geridos e do Azure | Microsoft Docs
description: Como localizar e eliminar desanexados discos do Azure geridos e não geridas (blobs de páginas/VHDs) ao utilizar a CLI do Azure.
services: virtual-machines-linux
documentationcenter: ''
author: ramankumarlive
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: ramankum
ms.openlocfilehash: 1718b35aa68937cad4b3ae7e677e300506820bd0
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
ms.locfileid: "30323258"
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Localizar e eliminar desanexados discos geridos e do Azure
Quando elimina uma máquina virtual (VM) no Azure, por predefinição, não são eliminados todos os discos que estão ligados à VM. Esta funcionalidade ajuda a evitar perda de dados devido a eliminação não intencional de VMs. Depois de uma VM é eliminada, continuará a pagar para os discos não ligados. Este artigo mostra como encontrar e eliminar quaisquer discos desanexados e reduzir os custos desnecessários. 


## <a name="managed-disks-find-and-delete-unattached-disks"></a>Discos geridos pelo: localizar e eliminar discos desanexados 

O script seguinte procura desanexados [discos geridos pelo](managed-disks-overview.md) , examinando o valor da **está** propriedade. Quando um disco gerido está ligado a uma VM, o **está** propriedade contém o ID de recurso da VM. Quando um disco gerido é desanexado, a **está** propriedade é nula. O script analisa todos os discos geridos uma subscrição do Azure. Quando o script localiza um disco gerido com o **está** propriedade definida como nulo, o script determina que o disco não ligado.

>[!IMPORTANT]
>Em primeiro lugar, execute o script, definindo o **deleteUnattachedDisks** variável como 0. Esta ação permite-lhe localizar e ver todos os discos geridos não ligados.
>
>Depois de analisar todos os discos desanexados, execute novamente o script e defina o **deleteUnattachedDisks** variável para 1. Esta ação permite eliminar todos os discos geridos não ligados.
>

```azurecli

# Set deleteUnattachedDisks=1 if you want to delete unattached Managed Disks
# Set deleteUnattachedDisks=0 if you want to see the Id of the unattached Managed Disks
deleteUnattachedDisks=0

unattachedDiskIds=$(az disk list --query '[?managedBy==`null`].[id]' -o tsv)
for id in ${unattachedDiskIds[@]}
do
    if (( $deleteUnattachedDisks == 1 ))
    then

        echo "Deleting unattached Managed Disk with Id: "$id
        az disk delete --ids $id --yes
        echo "Deleted unattached Managed Disk with Id: "$id

    else
        echo $id
    fi
done
```

## <a name="unmanaged-disks-find-and-delete-unattached-disks"></a>Não gerido discos: localizar e eliminar discos desanexados 

Os discos não geridos são ficheiros VHD que são armazenados como [blobs de páginas](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) no [contas do storage do Azure](../../storage/common/storage-create-storage-account.md). O script seguinte procura desanexados discos não geridos (os blobs de páginas), examinando o valor da **LeaseStatus** propriedade. Quando um disco não gerido está ligado a uma VM, o **LeaseStatus** propriedade está definida como **está bloqueado**. Quando um disco não gerido é desanexado, a **LeaseStatus** propriedade está definida como **Unlocked**. O script analisa todos os discos não geridos todas as contas de armazenamento do Azure uma subscrição do Azure. Quando o script localiza um disco não gerido com um **LeaseStatus** propriedade definida como **Unlocked**, o script determina que o disco não ligado.

>[!IMPORTANT]
>Em primeiro lugar, execute o script, definindo o **deleteUnattachedVHDs** variável como 0. Esta ação permite-lhe localizar e ver todos os VHDs não geridos não ligados.
>
>Depois de analisar todos os discos desanexados, execute novamente o script e defina o **deleteUnattachedVHDs** variável para 1. Esta ação permite eliminar todos os VHDs não geridos não ligados.
>

```azurecli
   
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the details of the unattached VHDs
deleteUnattachedVHDs=1

storageAccountIds=$(az storage account list --query [].[id] -o tsv)

for id in ${storageAccountIds[@]}
do
    connectionString=$(az storage account show-connection-string --ids $id --query connectionString -o tsv)
    containers=$(az storage container list --connection-string $connectionString --query [].[name] -o tsv)

    for container in ${containers[@]}
    do 
        
        blobs=$(az storage blob list -c $container --connection-string $connectionString --query "[?properties.blobType=='PageBlob' && ends_with(name,'.vhd')].[name]" -o tsv)
        
        for blob in ${blobs[@]}
        do
            leaseStatus=$(az storage blob show -n $blob -c $container --connection-string $connectionString --query "properties.lease.status" -o tsv)
            
            if [ "$leaseStatus" == "unlocked" ]
            then 

                if (( $deleteUnattachedVHDs == 1 ))
                then 

                    echo "Deleting VHD: "$blob" in container: "$container" in storage account: "$id

                    az storage blob delete --delete-snapshots include  -n $blob -c $container --connection-string $connectionString

                    echo "Deleted VHD: "$blob" in container: "$container" in storage account: "$id
                else
                    echo "StorageAccountId: "$id" container: "$container" VHD: "$blob
                fi

            fi
        done
    done
done 
```

## <a name="next-steps"></a>Passos Seguintes

[Eliminar conta do storage](../../storage/common/storage-create-storage-account.md)



