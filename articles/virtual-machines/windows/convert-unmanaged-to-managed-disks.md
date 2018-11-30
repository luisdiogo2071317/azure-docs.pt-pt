---
title: Converter uma máquina virtual do Windows de discos não geridos para managed disks – Managed Disks do Azure | Documentos da Microsoft
description: Como converter uma VM do Windows de discos não geridos para discos geridos com o PowerShell no modelo de implementação do Resource Manager
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: cynthn
ms.openlocfilehash: 26ddc1df8353a8a8b0e0c54a0c906c8d8b6b45b9
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52495329"
---
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-to-managed-disks"></a>Converter uma máquina virtual do Windows de discos não geridos para managed disks

Se tiver existentes máquinas de virtuais de Windows (VMs) que utilizam discos não geridos, pode converter as VMs para utilizar discos geridos através da [Managed Disks do Azure](managed-disks-overview.md) serviço. Este processo converte o disco do SO e qualquer discos de dados anexados.

Este artigo mostra-lhe como converter VMs com o Azure PowerShell. Se precisar de instalar ou atualizá-lo, veja [instalar e configurar o Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="before-you-begin"></a>Antes de começar


* Revisão [planear a migração para os Managed Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).

* Revisão [as FAQ sobre a migração para os Managed Disks](faq-for-disks.md#migrate-to-managed-disks).

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]




## <a name="convert-single-instance-vms"></a>Converter VMs de instância única
Esta secção abrange como converter VMs do Azure de instância única de discos não geridos para discos geridos. (Se forem as suas VMs num conjunto de disponibilidade, consulte a secção seguinte.) 

1. Desaloque a VM ao utilizar o [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet. O exemplo seguinte desaloca a VM com o nome `myVM` no grupo de recursos com o nome `myResourceGroup`: 

  ```azurepowershell-interactive
  $rgName = "myResourceGroup"
  $vmName = "myVM"
  Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
  ```

2. Converter VM para managed disks, utilizando o [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) cmdlet. O processo a seguir converte a VM anterior, incluindo o disco do SO e qualquer discos de dados e inicia a Máquina Virtual:

  ```azurepowershell-interactive
  ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
  ```



## <a name="convert-vms-in-an-availability-set"></a>Converter VMs num conjunto de disponibilidade

Se as VMs que pretende converter para gerido discos estão num conjunto de disponibilidade, tem primeiro de converter o conjunto de disponibilidade para um conjunto de disponibilidade gerido.

1. Converter a conjunto de disponibilidade, utilizando o [Update-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) cmdlet. O exemplo seguinte atualiza a conjunto de disponibilidade designado `myAvailabilitySet` no grupo de recursos com o nome `myResourceGroup`:

  ```azurepowershell-interactive
  $rgName = 'myResourceGroup'
  $avSetName = 'myAvailabilitySet'

  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
  ```

  Se a região onde definir a sua disponibilidade está localizada tem apenas 2 domínios de falha gerido, mas o número de domínios de falha não gerenciado é 3, este comando mostra um erro semelhante a "a contagem de domínios de falhas especificado 3 deve estar no intervalo de 1 a 2." Para resolver o erro, atualize o domínio de falhas para 2 e de atualização `Sku` para `Aligned` da seguinte forma:

  ```azurepowershell-interactive
  $avSet.PlatformFaultDomainCount = 2
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
  ```

2. Desalocar e converter as VMs no conjunto de disponibilidade. O seguinte script desaloca cada VM utilizando o [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet, converte-o utilizando [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk)e reinicia automaticamente como outra parte do processo de conversão :

  ```azurepowershell-interactive
  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName

  foreach($vmInfo in $avSet.VirtualMachinesReferences)
  {
     $vm = Get-AzureRmVM -ResourceGroupName $rgName | Where-Object {$_.Id -eq $vmInfo.id}
     Stop-AzureRmVM -ResourceGroupName $rgName -Name $vm.Name -Force
     ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vm.Name
  }
  ```


## <a name="troubleshooting"></a>Resolução de problemas

Se existir um erro durante a conversão, ou se uma VM está num Estado de falha devido a problemas de uma conversão anterior, execute o `ConvertTo-AzureRmVMManagedDisk` cmdlet novamente. Uma repetição simple normalmente desbloqueia a situação.
Antes de converter, certificar-se de que todas as extensões VM estão no estado "A aprovisionar foi concluída com êxito" ou a conversão irá falhar com o código de erro 409.


## <a name="convert-using-the-azure-portal"></a>Converter utilizando o portal do Azure

Também pode converter discos não geridos para managed disks através do portal do Azure.

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione a VM a partir da lista de VMs no portal.
3. No painel da VM, selecione **discos** no menu.
4. Na parte superior a **discos** painel, selecione **para migrar para managed disks**.
5. Se a sua VM está num conjunto de disponibilidade, haverá um aviso sobre o **migrar para discos geridos** painel que tem de converter o primeiro conjunto de disponibilidade. O aviso deve ter uma ligação, pode clicar para converter o conjunto de disponibilidade. Depois do conjunto de disponibilidade é convertido, ou se a VM não está num conjunto de disponibilidade, clique em **migrar** para iniciar o processo de migrar os seus discos para discos geridos. 

A VM será parada e reiniciada após a conclusão da migração.

## <a name="next-steps"></a>Passos Seguintes

[Converter os discos geridos standard para premium](convert-disk-storage.md)

Ter uma cópia só de leitura de uma VM ao utilizar [instantâneos](snapshot-copy-managed-disk.md).

