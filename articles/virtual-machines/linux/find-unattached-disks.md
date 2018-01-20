---
title: Localizar e eliminar desanexados discos geridos e do Azure | Microsoft Docs
description: "Como localizar e eliminar desanexados discos do Azure geridos e não geridas (blobs de páginas/VHDs), utilizando a CLI do Azure"
services: virtual-machines-linux
documentationcenter: 
author: ramankumarlive
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: ramankum
ms.openlocfilehash: 9ada768cd4128b9dd6949b5a96c557496c6bb11c
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/20/2018
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Localizar e eliminar desanexados discos geridos e do Azure
Quando elimina uma máquina virtual no Azure, os discos ligados ao mesmo não são eliminados por predefinição. Impede a perda de dados devido a máquinas virtuais eliminado por engano, mas continua a pagar para os discos desanexados desnecessariamente. Utilize este artigo para localizar e eliminar todos os discos desanexados e guarde o custo. 


## <a name="find-and-delete-unattached-managed-disks"></a>Localizar e eliminar desanexados discos geridos 

O script seguinte mostra como encontrar desanexados discos geridos utilizando a propriedade está.  Repete através de todos os geridos discos numa subscrição e verifica se o *está* propriedade é nula para localizar os discos gerido. *Está* propriedade armazena o ID de recurso da máquina virtual ao qual está ligado um disco gerido. 

Recomendamos vivamente para ser executado primeiro o script, definindo o *deleteUnattachedDisks* variável como 0 para ver todos os discos não ligados. Depois de rever os discos desanexados, execute o script através da definição *deleteUnattachedDisks* como 1 para eliminar todos os discos não ligados.

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
## <a name="find-and-delete-unattached-unmanaged-disks"></a>Localizar e eliminar desanexados discos não geridos 

Os discos não geridos são ficheiros VHD armazenados como [blobs de páginas](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) no [contas de armazenamento do Azure](../../storage/common/storage-create-storage-account.md). O script seguinte mostra como encontrar desanexados discos não geridos (os blobs de páginas) utilizando LeaseStatus propriedade. -Repetido ao longo de todos os discos não geridos em todas as contas numa subscrição de armazenamento e verifica se o *LeaseStatus* propriedade está desbloqueada encontrar desanexados discos não geridos. O *LeaseStatus* propriedade está definida como bloqueada se se encontra ligado a uma máquina virtual um disco não gerido. 

Recomendamos vivamente para ser executado primeiro o script, definindo o *deleteUnattachedVHDs* variável como 0 para ver todos os discos não ligados. Depois de rever os discos desanexados, execute o script através da definição *deleteUnattachedVHDs* como 1 para eliminar todos os discos não ligados.


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

[Eliminar conta do Storage](../../storage/common/storage-create-storage-account.md)



