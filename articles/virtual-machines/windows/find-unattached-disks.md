---
title: Localizar e eliminar desanexados discos geridos e do Azure | Microsoft Docs
description: "Como localizar e eliminar desanexados discos do Azure geridos e não geridas (blobs de páginas/VHDs), utilizando o Azure PowerShell."
services: virtual-machines-windows
documentationcenter: 
author: ramankumarlive
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: ramankum
ms.openlocfilehash: a846d3578d40b19762f185381c92bdf8e225b185
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/20/2018
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks"></a>Localizar e eliminar desanexados discos geridos e do Azure
Quando elimina uma máquina virtual no Azure, os discos ligados ao mesmo não são eliminados por predefinição. Impede a perda de dados devido a máquinas virtuais eliminado por engano, mas continua a pagar para os discos desanexados desnecessariamente. Utilize este artigo para localizar e eliminar todos os discos desanexados e guarde o custo. 


## <a name="find-and-delete-unattached-managed-disks"></a>Localizar e eliminar desanexados discos geridos 

O script seguinte mostra como encontrar desanexados [discos geridos](managed-disks-overview.md) utilizando *está* propriedade. -Repetido ao longo de todos os geridos discos uma subscrição e verifica a *está* propriedade é nula para localizar os discos gerido. *Está* propriedade armazena o ID de recurso da máquina virtual ao qual está ligado um disco gerido.

Recomendamos vivamente para ser executado primeiro o script, definindo o *deleteUnattachedDisks* variável como 0 para ver todos os discos não ligados. Depois de rever os discos desanexados, execute o script através da definição *deleteUnattachedDisks* como 1 para eliminar todos os discos não ligados.

```azurepowershell-interactive

# Set deleteUnattachedDisks=1 if you want to delete unattached Managed Disks
# Set deleteUnattachedDisks=0 if you want to see the Id of the unattached Managed Disks
$deleteUnattachedDisks=0

$managedDisks = Get-AzureRmDisk

foreach ($md in $managedDisks) {
    
    # ManagedBy property stores the Id of the VM to which Managed Disk is attached to
    # If ManagedBy property is $null then it means that the Managed Disk is not attached to a VM
    if($md.ManagedBy -eq $null){

        if($deleteUnattachedDisks -eq 1){
            
            Write-Host "Deleting unattached Managed Disk with Id: $($md.Id)"

            $md | Remove-AzureRmDisk -Force

            Write-Host "Deleted unattached Managed Disk with Id: $($md.Id) "

        }else{

            $md.Id

        }
           
    }
     
 } 
```
## <a name="find-and-delete-unattached-unmanaged-disks"></a>Localizar e eliminar desanexados discos não geridos 

Os discos não geridos são ficheiros VHD armazenados como [blobs de páginas] (/ rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) no [contas de armazenamento do Azure](../../storage/common/storage-create-storage-account.md). O script seguinte mostra como encontrar desanexados discos não geridos (os blobs de páginas) utilizando o *LeaseStatus* propriedade. -Repetido ao longo de todos os discos não geridos em todas as contas numa subscrição de armazenamento e verifica se o *LeaseStatus* propriedade está desbloqueada encontrar desanexados discos não geridos. *LeaseStatus* propriedade está definida como bloqueada se se encontra ligado a uma máquina virtual um disco não gerido.

Recomendamos vivamente para ser executado primeiro o script, definindo o *deleteUnattachedVHDs* variável como 0 para ver todos os discos não ligados. Depois de rever os discos desanexados, execute o script através da definição *deleteUnattachedVHDs* como 1 para eliminar todos os discos não ligados.


```azurepowershell-interactive
   
# Set deleteUnattachedVHDs=1 if you want to delete unattached VHDs
# Set deleteUnattachedVHDs=0 if you want to see the Uri of the unattached VHDs
$deleteUnattachedVHDs=1

$storageAccounts = Get-AzureRmStorageAccount

foreach($storageAccount in $storageAccounts){

    $storageKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccount.ResourceGroupName -Name $storageAccount.StorageAccountName)[0].Value

    $context = New-AzureStorageContext -StorageAccountName $storageAccount.StorageAccountName -StorageAccountKey $storageKey

    $containers = Get-AzureStorageContainer -Context $context

    foreach($container in $containers){

        $blobs = Get-AzureStorageBlob -Container $container.Name -Context $context

        #Fetch all the Page blobs with extension .vhd as only Page blobs can be attached as disk to Azure VMs
        $blobs | Where-Object {$_.BlobType -eq 'PageBlob' -and $_.Name.EndsWith('.vhd')} | ForEach-Object { 
        
            #If a Page blob is not attached as disk then LeaseStatus will be unlocked
            if($_.ICloudBlob.Properties.LeaseStatus -eq 'Unlocked'){
              
                  if($deleteUnattachedVHDs -eq 1){

                        Write-Host "Deleting unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"

                        $_ | Remove-AzureStorageBlob -Force

                        Write-Host "Deleted unattached VHD with Uri: $($_.ICloudBlob.Uri.AbsoluteUri)"
                  }
                  else{

                        $_.ICloudBlob.Uri.AbsoluteUri

                  }

            }
        
        }

    }

}

```

## <a name="next-steps"></a>Passos Seguintes

[Eliminar conta do Storage](../../storage/common/storage-create-storage-account.md)




