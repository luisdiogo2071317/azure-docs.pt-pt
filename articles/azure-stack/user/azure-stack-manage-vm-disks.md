---
title: Gerir discos VM na pilha do Azure | Microsoft Docs
description: "Aprovisionar discos para máquinas virtuais para a pilha do Azure."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 4e5833cf-4790-4146-82d6-737975fb06ba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/14/2017
ms.author: brenduns
ms.reviewer: jiahan
ms.openlocfilehash: 0c36e2eaaf2d266842b2b7de0b0c8dc0ed1e0145
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/15/2017
---
# <a name="virtual-machine-disk-storage-for-azure-stack"></a>Armazenamento de disco da máquina virtual para a pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pilha do Azure suporta a utilização de [não gerido discos](https://docs.microsoft.com/azure/virtual-machines/windows/about-disks-and-vhds#unmanaged-disks) numa máquina Virtual como um disco de sistema operativo (SO) e um disco de dados. Para utilizar discos não geridos, crie um [conta de armazenamento](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) e, em seguida, armazenar os discos que os blobs de página nos contentores na conta de armazenamento. Estes discos, em seguida, são referidos como discos VM.

Para melhorar o desempenho e reduzir o custo de gestão do sistema de pilha do Azure, recomendamos que coloque cada disco da VM num contentor separado. Um contentor deve conter um disco de SO ou um disco de dados, mas não ambos simultaneamente. No entanto, não há nenhuma restrição para o que impede a colocar ambos no mesmo contentor.

Se adicionar um ou mais discos de dados para uma VM, planeie utilizar contentores adicionais como uma localização para armazenar os discos. Como discos de dados, o disco de SO para VMs adicionais também deve ter os seus próprios contentores separados.

Quando cria várias VMs, pode reutilizar a mesma conta de armazenamento para cada nova VM. Apenas os contentores que cria devem ser exclusivos.  

Para adicionar discos para uma VM, utilize o portal de utilizador ou o PowerShell.

| Método | Opções
|-|-|
|[Portal de utilizador](#use-the-portal-to-add-additional-disks-to-a-vm)|-Adicione novos discos de dados para uma VM que foi aprovisionada anteriormente. Novos discos são criados pela pilha de Azure. </br> </br>-Adicione um ficheiro. vhd existente como um disco a uma VM que foi aprovisionada anteriormente. Para fazê-lo primeiro tem de preparar e carregar o ficheiro. vhd para a pilha do Azure. |
|[PowerShell](#use-powershell-to-add-multiple-unmanaged-disks-to-a-vm) | -Criar uma nova VM com um disco de SO e, ao mesmo tempo, adicione um ou mais discos de dados para essa VM. |


## <a name="use-the-portal-to-add-additional-disks-to-a-vm"></a>Utilizar o portal para adicionar mais discos para uma VM
Por predefinição, quando utilizar o portal para criar uma VM para a maioria dos itens do marketplace, é criado um disco de SO. Discos criados pelo Azure são denominados discos geridos.

Depois de aprovisionar uma VM, pode utilizar o portal para adicionar um novo disco de dados ou um disco de dados existente para essa VM. Cada disco adicional deve ser colocado num contentor separado. Os discos que adicionar a uma VM são denominados discos não geridos.

### <a name="use-the-portal-to-attach-a-new-data-disk-to-a-vm"></a>Utilizar o portal para anexar um disco de dados de novo a uma VM

1.  No portal, clique em **máquinas virtuais**.    
    ![Exemplo: Dashboard VM](media/azure-stack-manage-vm-disks/vm-dashboard.png)

2.  Selecione uma máquina virtual que tenha sido aprovisionada anteriormente.   
    ![Exemplo: Selecione uma VM no dashboard](media/azure-stack-manage-vm-disks/select-a-vm.png)

3.  Para a máquina virtual, clique em **discos** > **anexar novo**.       
    ![Exemplo: Anexe um novo disco à vm](media/azure-stack-manage-vm-disks/Attach-disks.png)    

4.  No **anexar novo disco** painel, clique em **localização**. Por predefinição, a localização é definida ao mesmo contentor que detém o disco do SO.      
    ![Exemplo: Defina a localização do disco](media/azure-stack-manage-vm-disks/disk-location.png)

5.  Selecione o **conta de armazenamento** a utilizar. Em seguida, selecione o **contentor** onde pretende colocar o disco de dados. Do **contentores** página, pode criar um novo contentor se pretender. Em seguida, pode alterar a localização para o novo disco para a suas próprias contentor. Quando utilizar um contentor separado para cada disco, distribuir o posicionamento do disco de dados que pode melhorar o desempenho. Clique em **selecione** para guardar a seleção.     
    ![Exemplo: Selecione um contentor](media/azure-stack-manage-vm-disks/select-container.png)

6.  No **anexar novo disco** página, atualize o **nome**, **tipo**, **tamanho**, e **alojar a colocação em cache** definições de disco. Em seguida, clique em **OK** para guardar a nova configuração de disco para a VM.  
    ![Exemplo: Anexo de discos completos](media/azure-stack-manage-vm-disks/complete-disk-attach.png)  

7.  Depois de pilha do Azure cria o disco e anexa-lo para a máquina virtual, o novo disco está listado nas definições de disco da máquina virtual em **discos de dados**.   
    ![Exemplo: Disco de vista](media/azure-stack-manage-vm-disks/view-data-disk.png)


### <a name="attach-an-existing-data-disk-to-a-vm"></a>Anexar um disco de dados existente para uma VM
1.  [Preparar um ficheiro. vhd](https://docs.microsoft.com/azure/virtual-machines/windows/classic/createupload-vhd) para utilização como disco de dados para uma VM. Carregar esse ficheiro. vhd para uma conta de armazenamento que utiliza com a VM que pretende anexar o ficheiro. vhd para.

  Recomendamos que utilize um contentor diferente para conter o ficheiro. vhd que o contentor que detém o disco do SO.   
  ![Exemplo: Carregar um ficheiro VHD](media/azure-stack-manage-vm-disks/upload-vhd.png)

2.  Depois do ficheiro. vhd é carregado, está pronto para anexar o VHD para uma VM. No menu à esquerda, clique em **máquinas virtuais**.  
 ![Exemplo: Selecione uma VM no dashboard](media/azure-stack-manage-vm-disks/vm-dashboard.png)

3.  Selecione a máquina virtual na lista.    
  ![Exemplo: Selecione uma VM no dashboard](media/azure-stack-manage-vm-disks/select-a-vm.png)

4.  Na página para a máquina virtual, clique em **discos** > **anexar existente**.   
  ![Exemplo: Anexar um disco existente](media/azure-stack-manage-vm-disks/attach-disks2.png)

5.  No **anexar o disco existente** página, clique em **ficheiro VHD**. O **contas do Storage** é aberta a página.    
  ![Exemplo: Selecione um ficheiro VHD](media/azure-stack-manage-vm-disks/select-vhd.png)

6.  Em **contas do Storage**, selecione a conta a utilizar e, em seguida, selecione um contentor que retém o ficheiro. vhd que carregou anteriormente. Selecione o ficheiro. vhd e, em seguida, clique em **selecione** para guardar a seleção.    
  ![Exemplo: Selecione um contentor](media/azure-stack-manage-vm-disks/select-container2.png)

7.  Em **anexar o disco existente**, o ficheiro que selecionou é apresentado em **ficheiro VHD**. Atualização do **alojar a colocação em cache** definição do disco e, em seguida, clique em **OK** para guardar a nova configuração de disco para a VM.    
  ![Exemplo: Anexar o ficheiro VHD](media/azure-stack-manage-vm-disks/attach-vhd.png)

8.  Depois de pilha do Azure cria o disco e anexa-lo para a máquina virtual, o novo disco está listado nas definições de disco da máquina virtual em **discos de dados**.   
  ![Exemplo: Concluir o disco anexar](media/azure-stack-manage-vm-disks/complete-disk-attach.png)


## <a name="use-powershell-to-add-multiple-unmanaged-disks-to-a-vm"></a>Utilize o PowerShell para adicionar vários discos não geridos para uma VM
Pode utilizar o PowerShell para Aprovisionar uma VM e adicionar um novo disco de dados ou a anexar pré-existente **. vhd** ficheiro como um disco de dados.

O **adicionar AzureRmVMDataDisk** cmdlet adiciona um disco de dados para uma máquina virtual. Pode adicionar um disco de dados quando criar uma máquina virtual ou pode adicionar um disco de dados para uma máquina virtual existente. Especifique o **VhdUri** parâmetro para distribuir os discos para contentores diferentes.

### <a name="add-data-disks-to-a-new-virtual-machine"></a>Adicionar discos de dados para uma nova máquina virtual
Os exemplos seguintes utilizam comandos do PowerShell para criar uma VM com discos de três dados, cada colocado num contentor diferente.

O primeiro comando cria um objeto de máquina virtual e, em seguida, armazena-na *$VirtualMachine* variável. O comando atribui um nome e o tamanho para a máquina virtual.
  ```
  $VirtualMachine = New-AzureRmVMConfig -VMName "VirtualMachine" `
                                      -VMSize "Standard_A2"
  ```

Os três comandos atribuam caminhos três de discos de dados para o *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, e *$DataDiskVhdUri03* variáveis. Defina um nome de outro caminho no URL para distribuir os discos para contentores diferentes.     
  ```
  $DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
  ```

  ```
  $DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
  ```

  ```
  $DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
  ```

Os três comandos finais adicionar discos de dados para a máquina virtual armazenada na *$VirtualMachine*. Cada comando Especifica o nome, localização e propriedades adicionais do disco. O URI de cada disco é armazenado em *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, e *$DataDiskVhdUri03*.
  ```
  $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk1' `
                  -Caching 'ReadOnly' -DiskSizeInGB 10 -Lun 0 `
                  -VhdUri $DataDiskVhdUri01 -CreateOption Empty
  ```

  ```
  $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk2' `
                 -Caching 'ReadOnly' -DiskSizeInGB 11 -Lun 1 `
                 -VhdUri $DataDiskVhdUri02 -CreateOption Empty
  ```

  ```
  $VirtualMachine = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name 'DataDisk3' `
                  -Caching 'ReadOnly' -DiskSizeInGB 12 -Lun 2 `
                  -VhdUri $DataDiskVhdUri03 -CreateOption Empty
  ```

Utilize os seguintes comandos do PowerShell para adicionar a configuração de disco e rede de SO para a VM e, em seguida, iniciar a nova VM.
  ```
  #set variables
  $rgName = "myResourceGroup"
  $location = "local"
  #Set OS Disk
  $osDiskUri = "https://contoso.blob.local.azurestack.external/vhds/osDisk.vhd"
  $osDiskName = "osDisk"

  $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $osDiskName -VhdUri $osDiskUri `
      -CreateOption FromImage -Windows

  # Create a subnet configuration
  $subnetName = "mySubNet"
  $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24

  # Create a vnet configuration
  $vnetName = "myVnetName"
  $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
      -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet

  # Create a public IP
  $ipName = "myIP"
  $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
      -AllocationMethod Dynamic

  # Create a network security group cnfiguration
  $nsgName = "myNsg"
  $rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
      -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
      -SourceAddressPrefix Internet -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 3389
  $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
      -Name $nsgName -SecurityRules $rdpRule

  # Create a NIC configuration
  $nicName = "myNicName"
  $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
  -Location $location -SubnetId $vnet.Subnets[0].Id -NetworkSecurityGroupId $nsg.Id -PublicIpAddressId $pip.Id

  #Create the new VM
  $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName VirtualMachine | `
      Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
      -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id
  New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $VirtualMachine
  ```



### <a name="add-data-disks-to-an-existing-virtual-machine"></a>Adicionar discos de dados para uma máquina virtual existente
Os exemplos seguintes utilizam comandos do PowerShell para adicionar três discos de dados para uma VM existente.
O primeiro comando obtém a máquina virtual com o nome máquina virtual utilizando o **Get-AzureRmVM** cmdlet. O comando armazena a máquina virtual no *$VirtualMachine* variável.
  ```
  $VirtualMachine = Get-AzureRmVM -ResourceGroupName "myResourceGroup" `
                                  -Name "VirtualMachine"
  ```
Os três comandos atribuam caminhos dos discos de três dados para as variáveis $DataDiskVhdUri01, $DataDiskVhdUri02 e $DataDiskVhdUri03.  Os nomes de caminho diferente de vhduri indicam contentores diferentes para o posicionamento de disco.
  ```
  $DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
  ```
  ```
  $DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
  ```
  ```
  $DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
  ```


  Os três comandos adicionar os discos de dados para a máquina virtual armazenada no *$VirtualMachine* variável. Cada comando Especifica o nome, localização e propriedades adicionais do disco. O URI de cada disco é armazenado em *$DataDiskVhdUri01*, *$DataDiskVhdUri02*, e *$DataDiskVhdUri03*.
  ```
  Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk1" `
                        -VhdUri $DataDiskVhdUri01 -LUN 0 `
                        -Caching ReadOnly -DiskSizeinGB 10 -CreateOption Empty
  ```
  ```
  Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk2" `
                        -VhdUri $DataDiskVhdUri02 -LUN 1 `
                        -Caching ReadOnly -DiskSizeinGB 11 -CreateOption Empty
  ```
  ```
  Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk3" `
                        -VhdUri $DataDiskVhdUri03 -LUN 2 `
                        -Caching ReadOnly -DiskSizeinGB 12 -CreateOption Empty
  ```


  O comando final atualiza o estado da máquina virtual armazenado na *$VirtualMachine* no -*ResourceGroupName*.
  ```
  Update-AzureRmVM -ResourceGroupName "myResourceGroup" -VM $VirtualMachine
  ```
<!-- Pending scripts  

## Distribute the data disks of an existing VM
If you have a VM with more than one disk in the same container, the service operator of the Azure Stack deployment might ask you to redistribute the disks into individual containers.

To do so, use the scripts from the following location in GitHub. These scripts can be used to move the data disks to different containers.
-->

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre as máquinas virtuais de pilha do Azure, consulte o artigo [considerações para máquinas virtuais no Azure pilha](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-considerations).
