---
title: Localizar e eliminar desanexados discos geridos e do Azure | Microsoft Docs
description: "Como localizar e eliminar desanexados discos do Azure geridos e não geridas (blobs de páginas/VHDs) por utilizar o Azure PowerShell."
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
ms.openlocfilehash: 15c2550472156d5c1f680af77df2fe771edf3444
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
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

## <a name="unmanaged-disks-find-and-delete-unattached-disks"></a>Não gerido discos: localizar e eliminar discos desanexados 

Os discos não geridos são ficheiros VHD que são armazenados como [blobs de páginas](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) no [contas do storage do Azure](../../storage/common/storage-create-storage-account.md). O script seguinte procura desanexados discos não geridos (os blobs de páginas), examinando o valor da **LeaseStatus** propriedade. Quando um disco não gerido está ligado a uma VM, o **LeaseStatus** propriedade está definida como **está bloqueado**. Quando um disco não gerido é desanexado, a **LeaseStatus** propriedade está definida como **Unlocked**. O script analisa todos os discos não geridos todas as contas de armazenamento do Azure uma subscrição do Azure. Quando o script localiza um disco não gerido com um **LeaseStatus** propriedade definida como **Unlocked**, o script determina que o disco não ligado.

>[!IMPORTANT]
>Em primeiro lugar, execute o script, definindo o **deleteUnattachedVHDs** variável como 0. Esta ação permite-lhe localizar e ver todos os VHDs não geridos não ligados.
>
>Depois de analisar todos os discos desanexados, execute novamente o script e defina o **deleteUnattachedVHDs** variável para 1. Esta ação permite eliminar todos os VHDs não geridos não ligados.
>

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

[Eliminar conta do storage](../../storage/common/storage-create-storage-account.md)




