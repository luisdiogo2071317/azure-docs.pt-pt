---
title: Gerir discos de VM no Azure Stack | Documentos da Microsoft
description: Discos de aprovisionamento de máquinas virtuais no Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 4e5833cf-4790-4146-82d6-737975fb06ba
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/15/2018
ms.author: mabrigg
ms.reviewer: jiahan
ms.openlocfilehash: fc17ce0ebd13fb7e89405fcf4d6633551f340a27
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "42139547"
---
# <a name="provision-virtual-machine-disk-storage-in-azure-stack"></a>Aprovisionar o armazenamento de disco da máquina virtual no Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Este artigo descreve como aprovisionar o armazenamento de disco da máquina virtual com o portal do Azure Stack ou com o PowerShell.

## <a name="overview"></a>Descrição geral

O Azure Stack suporta a utilização de [discos não geridos](https://docs.microsoft.com/azure/virtual-machines/windows/about-disks-and-vhds#unmanaged-disks) nas máquinas virtuais, como um sistema operativo (SO) e um disco de dados.

Para utilizar discos não geridos, crie uma [conta de armazenamento](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) para armazenar os discos. Os discos que cria são conhecidos como discos de VM e são armazenados em contentores na conta de armazenamento.

### <a name="best-practice-guidelines"></a>Diretrizes de práticas recomendadas

Para melhorar o desempenho e reduzir os custos gerais, recomendamos que coloque cada disco VM num contentor separado. Um contentor deve conter um disco de SO ou um disco de dados, mas não ambos simultaneamente. (No entanto, há nada para o impedir de colocar os dois tipos de disco no mesmo contentor.)

Se adicionar um ou mais discos de dados a uma VM, utilize contentores adicionais como uma localização para armazenar estes discos. O disco do SO para VMs adicionais também deve estar em seus próprios contentores.

Quando cria várias VMs, pode reutilizar a mesma conta de armazenamento para cada nova máquina virtual. Apenas os contentores que cria devem ser exclusivos.

### <a name="adding-new-disks"></a>Adição de novos discos

A tabela seguinte resume como adicionar discos ao utilizar o portal e com o PowerShell.

| Método | Opções
|-|-|
|[Portal de utilizador](#use-the-portal-to-add-additional-disks-to-a-vm)|-Adicione novos discos de dados a uma VM existente. Novos discos são criados através do Azure Stack. </br> </br>-Adicione um ficheiro de disco (. vhd) existente para uma VM previamente aprovisionada. Para tal, tem de preparar o VHD e, em seguida, carregue o ficheiro para o Azure Stack. |
|[PowerShell](#use-powershell-to-add-multiple-unmanaged-disks-to-a-vm) | -Criar uma nova VM com um disco de SO e, ao mesmo tempo, adicione um ou mais discos de dados para essa VM. |

## <a name="use-the-portal-to-add-disks-to-a-vm"></a>Utilizar o portal para adicionar discos a uma VM

Por predefinição, quando utilizar o portal para criar uma VM para a maioria dos itens do marketplace, apenas o disco do SO é criado.

Depois de criar uma VM, pode utilizar o portal para:
* Criar um novo disco de dados e anexá-lo para a VM.
* Carregar um disco de dados existente e anexá-lo para a VM.

Cada disco não gerido, que adiciona deve ser colocado num contentor separado.

>[!NOTE]
>Os discos criados e geridos pelo Azure são chamados [discos geridos](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview).

### <a name="use-the-portal-to-create-and-attach-a-new-data-disk"></a>Utilizar o portal para criar e anexar um disco de dados novo

1.  No portal, selecione **máquinas virtuais**.    
    ![Exemplo: Dashboard VM](media/azure-stack-manage-vm-disks/vm-dashboard.png)

2.  Selecione uma máquina virtual que se anteriormente tiver sido aprovisionada.   
    ![Exemplo: Selecione uma VM no dashboard](media/azure-stack-manage-vm-disks/select-a-vm.png)

3.  Para a máquina virtual, selecione **discos** > **anexar novo**.       
    ![Exemplo: Anexar um disco novo à vm](media/azure-stack-manage-vm-disks/Attach-disks.png)    

4.  Na **anexar novo disco** painel, selecione **localização**. Por predefinição, a localização é definida para o mesmo contentor que contém o disco do SO.      
    ![Exemplo: Definir a localização do disco](media/azure-stack-manage-vm-disks/disk-location.png)

5.  Selecione o **conta de armazenamento** a utilizar. Em seguida, selecione o **contentor** onde pretende colocar o disco de dados. Partir do **contentores** página, pode criar um novo contentor se desejar. Em seguida, pode alterar a localização para o novo disco para o seu próprio contentor. Quando utiliza um contentor separado para cada disco, distribuir o posicionamento do disco de dados que pode melhorar o desempenho. Escolher **selecione** para guardar a seleção.     
    ![Exemplo: Selecione um contentor](media/azure-stack-manage-vm-disks/select-container.png)

6.  Na **anexar novo disco** página, atualize o **nome**, **tipo**, **tamanho**, e **alojar a colocação em cache** definições de disco. Em seguida, selecione **OK** para guardar a nova configuração de disco para a VM.  
    ![Exemplo: Anexo de disco completa](media/azure-stack-manage-vm-disks/complete-disk-attach.png)  

7.  Depois do Azure Stack cria o disco e anexa-o para a máquina virtual, o novo disco está listado nas definições de disco da máquina virtual em **discos de dados**.   
    ![Exemplo: Disco de vista](media/azure-stack-manage-vm-disks/view-data-disk.png)


### <a name="attach-an-existing-data-disk-to-a-vm"></a>Anexar um disco de dados existente a uma VM

1.  [Preparar um ficheiro. vhd](https://docs.microsoft.com/azure/virtual-machines/windows/classic/createupload-vhd) para utilização como disco de dados para uma VM. Carregue esse ficheiro. vhd para uma conta de armazenamento que utilizar com a VM que pretende anexar o ficheiro. vhd para.

  Recomendamos que utilize um contentor diferente para armazenar o ficheiro. vhd que o contentor que retém o disco do SO.   
  ![Exemplo: Carregar um ficheiro VHD](media/azure-stack-manage-vm-disks/upload-vhd.png)

2.  Depois do ficheiro. vhd é carregado, está pronto para anexar o VHD para uma VM. No menu à esquerda, selecione **máquinas virtuais**.  
 ![Exemplo: Selecione uma VM no dashboard](media/azure-stack-manage-vm-disks/vm-dashboard.png)

3.  Escolha a máquina virtual a partir da lista.    
  ![Exemplo: Selecione uma VM no dashboard](media/azure-stack-manage-vm-disks/select-a-vm.png)

4.  Na página para a máquina virtual, selecione **discos** > **Attach existente**.   
  ![Exemplo: Anexar um disco existente](media/azure-stack-manage-vm-disks/attach-disks2.png)

5.  Na **anexar disco existente** página, selecione **ficheiro VHD**. O **contas de armazenamento** é aberta a página.    
  ![Exemplo: Selecione um ficheiro VHD](media/azure-stack-manage-vm-disks/select-vhd.png)

6.  Sob **contas de armazenamento**, selecione a conta a utilizar e, em seguida, selecione um contentor que retém o ficheiro. vhd carregado anteriormente. Selecione o ficheiro. vhd e, em seguida, escolha **selecione** para guardar a seleção.    
  ![Exemplo: Selecione um contentor](media/azure-stack-manage-vm-disks/select-container2.png)

7.  Sob **anexar disco existente**, o ficheiro selecionado está listado em **ficheiro VHD**. Atualização do **colocação em cache do anfitrião** configuração do disco e, em seguida, selecione **OK** para guardar a nova configuração de disco para a VM.    
  ![Exemplo: Anexar o ficheiro VHD](media/azure-stack-manage-vm-disks/attach-vhd.png)

8.  Depois do Azure Stack cria o disco e anexa-o para a máquina virtual, o novo disco está listado nas definições de disco da máquina virtual em **discos de dados**.   
  ![Exemplo: Concluir o disco anexar](media/azure-stack-manage-vm-disks/complete-disk-attach.png)


## <a name="use-powershell-to-add-multiple-unmanaged-disks-to-a-vm"></a>Utilize o PowerShell para adicionar vários discos não geridos para uma VM
Pode utilizar o PowerShell para Aprovisionar uma VM e adicione um novo disco de dados ou anexar pré-existente **. vhd** ficheiro como um disco de dados.

O **Add-AzureRmVMDataDisk** cmdlet adiciona um disco de dados a uma máquina virtual. Pode adicionar um disco de dados quando criar uma máquina virtual, ou pode adicionar um disco de dados para uma máquina virtual existente. Especifique a **VhdUri** parâmetro para distribuir os discos para diferentes recipientes.

### <a name="add-data-disks-to-a-new-virtual-machine"></a>Adicionar discos de dados para uma nova máquina virtual
Os exemplos seguintes utilizam comandos do PowerShell para criar uma VM com discos de dados de três, cada colocado num contentor diferente.

O primeiro comando cria um objeto de máquina virtual e, em seguida, armazena-a na *$VirtualMachine* variável. O comando atribui um nome e o tamanho para a máquina virtual.
  ```
  $VirtualMachine = New-AzureRmVMConfig -VMName "VirtualMachine" `
                                      -VMSize "Standard_A2"
  ```

Os próximos três comandos atribuam caminhos de três discos de dados para o *us $DataDiskVhdUri01*, *us $DataDiskVhdUri02*, e *us $DataDiskVhdUri03* variáveis. Defina um nome de caminho diferente no URL para distribuir os discos para diferentes recipientes.     
  ```
  $DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
  ```

  ```
  $DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
  ```

  ```
  $DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
  ```

Os três comandos finais adicionam discos de dados para a máquina virtual armazenada na *$VirtualMachine*. Cada comando Especifica o nome, localização e propriedades adicionais do disco. O URI de cada disco é armazenado em *us $DataDiskVhdUri01*, *us $DataDiskVhdUri02*, e *us $DataDiskVhdUri03*.
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

Utilize os seguintes comandos do PowerShell para adicionar a configuração de rede e disco do SO para a VM e, em seguida, inicie a nova VM.
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
Os exemplos seguintes utilizam comandos do PowerShell para adicionar os três discos de dados a uma VM existente.
O primeiro comando obtém a máquina virtual com o nome VirtualMachine utilizando o **Get-AzureRmVM** cmdlet. O comando armazena a máquina virtual na *$VirtualMachine* variável.
  ```
  $VirtualMachine = Get-AzureRmVM -ResourceGroupName "myResourceGroup" `
                                  -Name "VirtualMachine"
  ```
Os próximos três comandos atribuam caminhos de três discos de dados para as variáveis $DataDiskVhdUri01, US $DataDiskVhdUri02 e US $DataDiskVhdUri03.  Os nomes de caminho diferente do vhduri indicam contentores diferentes para a colocação de disco.
  ```
  $DataDiskVhdUri01 = "https://contoso.blob.local.azurestack.external/test1/data1.vhd"
  ```
  ```
  $DataDiskVhdUri02 = "https://contoso.blob.local.azurestack.external/test2/data2.vhd"
  ```
  ```
  $DataDiskVhdUri03 = "https://contoso.blob.local.azurestack.external/test3/data3.vhd"
  ```


  Os próximos três comandos adicionam os discos de dados para a máquina virtual armazenada na *$VirtualMachine* variável. Cada comando Especifica o nome, localização e propriedades adicionais do disco. O URI de cada disco é armazenado em *us $DataDiskVhdUri01*, *us $DataDiskVhdUri02*, e *us $DataDiskVhdUri03*.
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


  O comando final atualiza o estado da máquina virtual armazenado na *$VirtualMachine* além -*ResourceGroupName*.
  ```
  Update-AzureRmVM -ResourceGroupName "myResourceGroup" -VM $VirtualMachine
  ```
<!-- Pending scripts  

## Distribute the data disks of an existing VM
If you have a VM with more than one disk in the same container, the service operator of the Azure Stack deployment might ask you to redistribute the disks into individual containers.

To do so, use the scripts from the following location in GitHub. These scripts can be used to move the data disks to different containers.
-->

## <a name="next-steps"></a>Próximos Passos
Para saber mais sobre as máquinas virtuais do Azure Stack, veja [considerações para máquinas virtuais no Azure Stack](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-considerations).
