---
title: Criar uma VM do Windows a partir de um VHD especializado no Azure | Documentos da Microsoft
description: Crie uma nova VM do Windows ao anexar um disco gerido especializado como o disco do SO a utilizar no modelo de implementação do Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 3b7d3cd5-e3d7-4041-a2a7-0290447458ea
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: cynthn
ms.openlocfilehash: 34bfe7733c60337d6ab7d81c498d2fb0fd15e1fd
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43338491"
---
# <a name="create-a-windows-vm-from-a-specialized-disk-using-powershell"></a>Criar uma VM do Windows a partir de um disco especializado ao utilizar o PowerShell

Crie uma nova VM ao anexar um disco gerido especializado, como o disco do SO. Um disco especializado é uma cópia do disco rígido virtual (VHD) a partir de uma VM existente que mantém as contas de utilizador, aplicativos e outros dados de estado da original VM. 

Quando utilizar um VHD especializado para criar uma nova VM, a nova VM retém o nome do computador da original VM. Também é ser mantida outras informações específicas de computador e, em alguns casos, essas informações duplicadas podem causar problemas. Lembre-se de que tipos de informações específicas de computador seus aplicativos dependem ao copiar uma VM.

Tem várias opções:
* [Utilizar um disco gerido existente](#option-1-use-an-existing-disk). Isto é útil se tiver uma VM que não está a funcionar corretamente. É possível eliminar a reutilização VM de disco gerido para criar uma nova VM. 
* [Carregar um VHD](#option-2-upload-a-specialized-vhd) 
* [Copiar uma VM do Azure existente com os instantâneos](#option-3-copy-an-existing-azure-vm)

Também pode utilizar o portal do Azure para [criar uma nova VM a partir de um VHD especializado](create-vm-specialized-portal.md).

Este tópico mostra-lhe como utilizar discos geridos. Se tiver uma implementação legada que requer a utilização de uma conta de armazenamento, consulte [criar uma VM a partir de um VHD especializado numa conta de armazenamento](sa-create-vm-specialized.md)

## <a name="before-you-begin"></a>Antes de começar
Se utilizar o PowerShell, certifique-se de que tem a versão mais recente do módulo do Azurerm PowerShell. 

```powershell
Install-Module AzureRM -RequiredVersion 6.0.0
```
Para obter mais informações, consulte [controle de versão do Azure PowerShell](/powershell/azure/overview).

## <a name="option-1-use-an-existing-disk"></a>Opção 1: Utilizar um disco existente

Se tivesse uma VM que eliminou e quiser reutilizar o disco do SO para criar uma nova VM, utilize [Get-AzureRmDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermdisk?view=azurermps-6.8.1).

```powershell
$resourceGroupName = 'myResourceGroup'
$osDiskName = 'myOsDisk'
$osDisk = Get-AzureRmDisk `
-ResourceGroupName $resourceGroupName `
-DiskName $osDiskName
```
Agora pode anexar este disco como o disco do SO para uma [nova VM](#create-the-new-vm).

## <a name="option-2-upload-a-specialized-vhd"></a>Opção 2: Carregar um VHD especializado

Pode carregar o VHD a partir de uma VM especializada criado com uma ferramenta de virtualização no local, como o Hyper-V ou numa VM exportada de outra cloud.

### <a name="prepare-the-vm"></a>Preparar a VM
Se pretende usar o VHD como-consiste em criar uma nova VM, certifique-se de que os seguintes passos são concluídos. 
  
  * [Preparar um VHD do Windows para carregar para o Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). **Isso não é possível** generalizar a VM com o Sysprep.
  * Remova todos os agentes instalados na VM (como as ferramentas do VMware) e ferramentas de Virtualização do convidado.
  * Certifique-se de que a VM está configurada para solicitar o respetivo endereço IP e as definições de DNS através de DHCP. Isto garante que o servidor obtém o endereço IP na VNet em modo de arranque. 


### <a name="get-the-storage-account"></a>Obter a conta de armazenamento
Precisa de uma conta de armazenamento no Azure para armazenar o VHD carregado. Pode utilizar uma conta de armazenamento existente ou crie um novo. 

Para mostrar as contas de armazenamento disponíveis, escreva:

```powershell
Get-AzureRmStorageAccount
```

Se pretender utilizar uma conta de armazenamento existente, avance para o [carregar o VHD](#upload-the-vhd-to-your-storage-account) secção.

Se precisar de criar uma conta de armazenamento, siga estes passos:

1. É necessário o nome do grupo de recursos onde a conta de armazenamento deve ser criada. Para obter todos os grupos de recursos que estão na sua subscrição, escreva:
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    Para criar um grupo de recursos chamado *myResourceGroup* no *E.U.A. oeste* região, escreva:

    ```powershell
    New-AzureRmResourceGroup `
       -Name myResourceGroup `
       -Location "West US"
    ```

2. Criar uma conta de armazenamento com o nome *mystorageaccount* neste grupo de recursos, utilizando o [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:
   
    ```powershell
    New-AzureRmStorageAccount `
       -ResourceGroupName myResourceGroup `
       -Name mystorageaccount `
       -Location "West US" `
       -SkuName "Standard_LRS" `
       -Kind "Storage"
    ```

### <a name="upload-the-vhd-to-your-storage-account"></a>Carregar o VHD para a conta de armazenamento 
Utilize o [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet para carregar o VHD para um contentor na conta de armazenamento. Este exemplo carrega o ficheiro *myVHD.vhd* partir `"C:\Users\Public\Documents\Virtual hard disks\"` para uma conta de armazenamento com o nome *mystorageaccount* no *myResourceGroup* grupo de recursos. O ficheiro é armazenado num contentor com o nome *mycontainer* e o novo nome de ficheiro serão *myUploadedVHD.vhd*.

```powershell
$resourceGroupName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $resourceGroupName `
   -Destination $urlOfUploadedVhd `
   -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Se tiver êxito, receberá uma resposta que é semelhante a este:

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Dependendo da sua ligação de rede e o tamanho do ficheiro VHD, este comando pode demorar algum tempo a concluir

### <a name="create-a-managed-disk-from-the-vhd"></a>Criar um disco gerido a partir de VHD

Criar um disco gerido a partir de um VHD especializado na sua conta de armazenamento com [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). Este exemplo utiliza **myOSDisk1** para o nome do disco, coloca o disco num *Standard_LRS* armazenamento e utiliza *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* como o URI para o VHD de origem.

Crie um novo grupo de recursos para a nova VM.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location `
   -Name $destinationResourceGroup
```

Crie o novo disco de SO a partir do VHD carregado. 

```powershell
$sourceUri = (https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd)
$osDiskName = 'myOsDisk'
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig -AccountType Standard_LRS  `
    -Location $location -CreateOption Import `
    -SourceUri $sourceUri) `
    -ResourceGroupName $destinationResourceGroup
```

## <a name="option-3-copy-an-existing-azure-vm"></a>Opção 3: Copiar uma VM do Azure existente

Pode criar uma cópia de uma VM que utiliza discos geridos tirando um instantâneo da VM, em seguida, utilizar esse instantâneo para criar um novo gerido disco e uma nova VM.


### <a name="take-a-snapshot-of-the-os-disk"></a>Tirar um instantâneo do disco do SO

Pode tirar uma VM inteira e instantâneo de (incluindo todos os discos) ou de apenas um único disco. Os passos seguintes mostram-lhe como tirar um instantâneo de apenas o disco do SO da VM com o [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet. 

Defina alguns parâmetros. 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$vmName = 'myVM'
$location = 'westus' 
$snapshotName = 'mySnapshot'  
```

Obtenha o objeto VM.

```powershell
$vm = Get-AzureRmVM -Name $vmName `
   -ResourceGroupName $resourceGroupName
```
Obtenha o nome do disco de SO.

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName `
   -DiskName $vm.StorageProfile.OsDisk.Name
```

Crie a configuração de instantâneo. 

 ```powershell
$snapshotConfig =  New-AzureRmSnapshotConfig `
   -SourceUri $disk.Id `
   -OsType Windows `
   -CreateOption Copy `
   -Location $location 
```

Tire o instantâneo.

```powershell
$snapShot = New-AzureRmSnapshot `
   -Snapshot $snapshotConfig `
   -SnapshotName $snapshotName `
   -ResourceGroupName $resourceGroupName
```


Se planeja usar o instantâneo para criar uma VM que tem de ser de alto desempenho, utilize o parâmetro `-AccountType Premium_LRS` com o comando New-AzureRmSnapshot. O parâmetro cria o instantâneo para que ele é armazenado como disco gerido Premium. Premium Managed Disks são mais caros do que no plano padrão. Por isso, certifique-se de que realmente precisa Premium antes de utilizar o parâmetro.

### <a name="create-a-new-disk-from-the-snapshot"></a>Criar um novo disco a partir do instantâneo

Criar um disco gerido a partir do instantâneo com o [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). Este exemplo utiliza *myOSDisk* para o nome do disco.

Crie um novo grupo de recursos para a nova VM.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location `
   -Name $destinationResourceGroup
```

Defina o nome do disco de SO. 

```powershell
$osDiskName = 'myOsDisk'
```

Crie o disco gerido.

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-the-new-vm"></a>Criar a nova VM 

Crie a rede e outros recursos da VM a ser utilizado pela VM nova.

### <a name="create-the-subnet-and-vnet"></a>Criar a vNet e sub-rede

Criar a vNet e sub-rede do [rede virtual](../../virtual-network/virtual-networks-overview.md).

Crie a sub-rede. Este exemplo cria uma sub-rede denominada **mySubNet**, no grupo de recursos **myDestinationResourceGroup**e define o prefixo de endereço de sub-rede para **10.0.0.0/24**.
   
```powershell
$subnetName = 'mySubNet'
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig `
   -Name $subnetName `
   -AddressPrefix 10.0.0.0/24
```

Crie a vNet. Este exemplo define o nome de rede virtual para ser **myVnetName**, a localização para **E.U.A. oeste**e o prefixo de endereço para a rede virtual para **10.0.0.0/16**. 
   
```powershell
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork `
   -Name $vnetName -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -AddressPrefix 10.0.0.0/16 `
   -Subnet $singleSubnet
```    


### <a name="create-the-network-security-group-and-an-rdp-rule"></a>Criar o grupo de segurança de rede e uma regra RDP
Para poder iniciar sessão na sua VM através de RDP, terá de ter uma regra de segurança que permite o acesso RDP na porta 3389. Uma vez que o VHD para a nova VM foi criado a partir de uma VM especializada existente, pode utilizar uma conta da máquina virtual de origem para RDP.

Este exemplo define o nome do NSG **myNsg** e o nome da regra RDP para **myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup `
   -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -Name $nsgName -SecurityRules $rdpRule
    
```

Para obter mais informações sobre pontos de extremidade e as regras do NSG, consulte [abrir portas para uma VM no Azure com o PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="create-a-public-ip-address-and-nic"></a>Criar um endereço IP público e a NIC
Para ativar a comunicação com a máquina virtual na rede virtual, é necessário um [endereço IP público](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) e uma interface de rede.

Crie o IP público. Neste exemplo, o nome do endereço IP público é definido como **myIP**.
   
```powershell
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress `
   -Name $ipName -ResourceGroupName $destinationResourceGroup `
   -Location $location `
   -AllocationMethod Dynamic
```       

Criar a NIC. Neste exemplo, o nome do NIC está definido como **myNicName**.
   
```powershell
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName `
   -ResourceGroupName $destinationResourceGroup `
   -Location $location -SubnetId $vnet.Subnets[0].Id `
   -PublicIpAddressId $pip.Id `
   -NetworkSecurityGroupId $nsg.Id
```



### <a name="set-the-vm-name-and-size"></a>Defina o nome da VM e o tamanho

Este exemplo define o nome da VM como *myVM* e o tamanho da VM *Standard_A2*.

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-the-nic"></a>Adicionar a NIC
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-the-os-disk"></a>Adicionar o disco do SO 

Adicionar o disco do SO para a configuração através de [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk). Este exemplo define o tamanho do disco para *128 GB* e anexa o disco gerido como um *Windows* disco do SO.
 
```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType Standard_LRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-the-vm"></a>Concluir a VM 

Criar a VM com [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)as configurações que acabamos de criar.

```powershell
New-AzureRmVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

Se este comando foi concluída com êxito, verá a saída como esta:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-the-vm-was-created"></a>Certifique-se de que a VM foi criada
Deverá ver a VM criada recentemente ou na [portal do Azure](https://portal.azure.com), em **procurar** > **máquinas virtuais**, ou utilizando os seguintes comandos do PowerShell:

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a>Passos Seguintes
Inicie sessão na sua nova máquina virtual. Para obter mais informações, consulte [como ligar e iniciar sessão a uma máquina virtual do Azure a executar o Windows](connect-logon.md).

