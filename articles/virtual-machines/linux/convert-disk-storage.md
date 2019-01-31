---
title: Converter os Azure managed armazenamento de discos de standard para premium e vice-versa | Documentos da Microsoft
description: Como converter do Azure gerido armazenamento de discos de standard para premium e vice-versa, ao utilizar a CLI do Azure.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: 42b0b7a1ca2767a7051a6c57ef2aeac8cf2bb64c
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55477408"
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a>Converter os Azure managed armazenamento de discos de standard para premium e vice-versa

Managed Disks oferece três opções de armazenamento: [Premium SSD](../windows/premium-storage.md), Standard SSD, e [HDD padrão](../windows/standard-storage.md). Permite-lhe alternar facilmente entre as opções com o tempo de inatividade mínimo, com base nas suas necessidades de desempenho. Isto não é suportado para discos não geridos. Mas pode facilmente [converter para discos geridos](convert-unmanaged-to-managed-disks.md) alternar facilmente entre os tipos de disco.

Este artigo mostra-lhe como converter discos geridos de standard para premium e vice-versa, utilizando a CLI do Azure. Se precisar de instalar ou atualizá-lo, veja [instalar a CLI do Azure](/cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Antes de começar

* A conversão necessita de um reinício da VM, por isso, agende a migração do armazenamento de discos durante uma janela de manutenção pré-existente. 
* Se estiver a utilizar discos não geridos, primeiro [converter para discos geridos](convert-unmanaged-to-managed-disks.md) para utilizar este artigo para alternar entre as opções de armazenamento. 


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a>Converter todos os discos geridos de uma VM de standard para premium e vice-versa

O exemplo seguinte mostra como mudar todos os discos de uma VM de standard para o armazenamento premium. Para utilizar discos geridos premium, a VM tem de utilizar um [tamanho da VM](sizes.md) que suporta o armazenamento premium. Este exemplo também muda para um tamanho que suporte o armazenamento premium.

 ```azurecli

#resource group that contains the virtual machine
rgName='yourResourceGroup'

#Name of the virtual machine
vmName='yourVM'

#Premium capable size 
#Required only if converting from standard to premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Deallocate the VM before changing the size of the VM
az vm deallocate --name $vmName --resource-group $rgName

#Change the VM size to a size that supports premium storage 
#Skip this step if converting storage from premium to standard
az vm resize --resource-group $rgName --name $vmName --size $size

#Update the sku of all the data disks 
az vm show -n $vmName -g $rgName --query storageProfile.dataDisks[*].managedDisk -o tsv \
 | awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

#Update the sku of the OS disk
az vm show -n $vmName -g $rgName --query storageProfile.osDisk.managedDisk -o tsv \
| awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

az vm start --name $vmName --resource-group $rgName

```
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a>Converter um disco gerido de standard para premium e vice-versa

Para sua carga de trabalho de programador/teste, pode querer ter a mistura de discos standard e premium para reduzir o custo. Poderá fazê-lo com a atualização para o armazenamento premium, apenas os discos que exigem um melhor desempenho. O exemplo seguinte mostra como alternar de um único disco de uma VM do plano padrão para o armazenamento premium e vice-versa. Para utilizar discos geridos premium, a VM tem de utilizar um [tamanho da VM](sizes.md) que suporta o armazenamento premium. Este exemplo também muda para um tamanho que suporte o armazenamento premium.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Premium capable size 
#Required only if converting from standard to premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the size of the VM
az vm deallocate --ids $vmId 

#Change the VM size to a size that supports premium storage 
#Skip this step if converting storage from premium to standard
az vm resize --ids $vmId --size $size

# Update the sku
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="convert-a-managed-disk-from-standard-hdd-to-standard-ssd-and-vice-versa"></a>Converter um disco gerido standard HDD para SSD standard e vice-versa

O exemplo seguinte mostra como alternar de um único disco de uma VM do padrão HDD para standard SSD.

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Choose between Standard_LRS and StandardSSD_LRS based on your scenario
sku='StandardSSD_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the disk type
az vm deallocate --ids $vmId 

# Update the sku
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="convert-using-the-azure-portal"></a>Converter utilizando o portal do Azure

Também pode converter discos não geridos para managed disks através do portal do Azure.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione a VM a partir da lista de VMs no portal.
3. No painel da VM, selecione **discos** no menu.
4. Na parte superior a **discos** painel, selecione **para migrar para managed disks**.
5. Se a sua VM está num conjunto de disponibilidade, haverá um aviso sobre o **migrar para discos geridos** painel que tem de converter o primeiro conjunto de disponibilidade. O aviso deve ter uma ligação, pode clicar para converter o conjunto de disponibilidade. Depois do conjunto de disponibilidade é convertido, ou se a VM não está num conjunto de disponibilidade, clique em **migrar** para iniciar o processo de migrar os seus discos para discos geridos. 

A VM será parada e reiniciada após a conclusão da migração.

## <a name="next-steps"></a>Passos Seguintes

Ter uma cópia só de leitura de uma VM ao utilizar [instantâneos](snapshot-copy-managed-disk.md).

