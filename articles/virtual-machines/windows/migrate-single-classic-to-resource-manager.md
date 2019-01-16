---
title: Migrar uma VM clássica para um disco gerido ARM de VM | Documentos da Microsoft
description: Migre uma única VM do Azure do modelo de implementação clássica para os Managed Disks no modelo de implementação do Resource Manager.
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
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: a662a61d737dbb620d07fa6d114649e70c082796
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2019
ms.locfileid: "54329774"
---
# <a name="migrate-a-classic-vm-to-use-a-managed-disk"></a>Migrar uma VM clássica para utilizar um disco gerido 


Esta secção ajuda-o a migrar as VMs do Azure existente a partir de modelo de implementação clássica para [Managed Disks](managed-disks-overview.md) no modelo de implementação do Resource Manager.


## <a name="plan-for-the-migration-to-managed-disks"></a>Planear a migração para os Managed Disks

Esta secção ajuda-o a tomar a melhor decisão em tipos VM e disco.


### <a name="location"></a>Localização

Escolha uma localização onde o Managed Disks estão disponíveis. Se estiver a migrar para o disco gerido apoiadas pelo armazenamento Premium, certifique-se também que o armazenamento Premium está disponível na região. Ver [dos serviços do Azure byRegion](https://azure.microsoft.com/regions/#services) para obter informações atualizadas sobre localizações disponíveis.

### <a name="vm-sizes"></a>Tamanhos de VM

Se estiver a migrar para um discos geridos com o armazenamento Premium, terá de atualizar o tamanho da VM para o armazenamento Premium com capacidade de tamanho disponível na região onde está localizada a VM. Reveja os tamanhos VM que são compatíveis com o armazenamento Premium. As especificações de tamanho de VM do Azure estão listadas na [tamanhos de máquinas virtuais](sizes.md).
Reveja as características de desempenho de máquinas virtuais que funcionam com o armazenamento Premium e escolha o tamanho VM mais adequado que melhor se adequa aos sua carga de trabalho. Certifique-se de que existe largura de banda suficiente disponível na sua VM para direcionar o tráfego de disco.

### <a name="disk-sizes"></a>Tamanhos de disco

**Premium**

Existem sete tipos de armazenamento Premium que pode ser utilizado com a VM e cada uma tem específicos IOPs e débito limites. Ao escolher o tipo de disco Premium para a sua VM com base nas necessidades da sua aplicação em termos de capacidade, desempenho, escalabilidade, considere estes limites e carrega de pico.

| Tipo de discos Premium  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Tamanho do disco           | 128 GB| 512 GB| 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOPs por disco       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Débito por disco | 25 MB por segundo  | 50 MB por segundo  | 100 MB por segundo | 150 MB por segundo | 200 MB por segundo | 250 MB por segundo | 250 MB por segundo | 

**Standard**

Existem sete tipos de discos padrão que podem ser utilizados com a sua VM. Cada um deles tem capacidade diferente, mas têm a mesma IOPS e limites de débito. Escolha o tipo de discos geridos Standard com base nas necessidades de capacidade do seu aplicativo.

| Tipo de Disco Standard  | S4               | S6               | S10              | S20              | S30              | S40              | S50              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| Tamanho do disco           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   | 2048 GB (2 TB)    | 4095 GB (4 TB)   | 
| IOPs por disco       | 500              | 500              | 500              | 500              | 500              | 500             | 500              | 
| Débito por disco | 60 MB por segundo | 60 MB por segundo | 60 MB por segundo | 60 MB por segundo | 60 MB por segundo | 60 MB por segundo | 60 MB por segundo | 


### <a name="disk-caching-policy"></a>Política de colocação em cache do disco 

**Premium Managed Disks**

Por predefinição, é a política de colocação em cache *só de leitura* em todos os discos de dados de Premium, e *leitura-escrita* para o disco de sistema operativo Premium ligados à VM. Esta definição de configuração é recomendada para alcançar o desempenho ideal para a IOs sua aplicação. Para discos de dados de escrita intensiva ou só de escrita (por exemplo, ficheiros de registo do SQL Server), desative o cache em disco para que pode obter um melhor desempenho do aplicativo.

### <a name="pricing"></a>Preços

Reveja os [os preços dos discos geridos](https://azure.microsoft.com/pricing/details/managed-disks/). Preços dos discos geridos Premium são os mesmos discos não geridos premium. Mas os preços dos discos geridos de padrão são diferente de discos standard não geridos.


## <a name="checklist"></a>Lista de verificação

1.  Se estiver a migrar para Premium Managed Disks, certifique-se de que está disponível na região que está a migrar.

2.  Decida a nova série VM que irá utilizar. Deve ser um compatível com o armazenamento Premium se estiver a migrar para Premium Managed Disks.

3.  Decida o VM tamanho exato que irá utilizar que estão disponíveis na região que está a migrar. Tamanho da VM tem de ser suficientemente grande para suportar o número de discos de dados que tiver. Por exemplo, se tiver quatro discos de dados, a VM tem de ter dois ou mais núcleos. Além disso, considere o processamento, memória e necessidades de largura de banda de rede.

4.  Ter os detalhes da VM atuais úteis, incluindo a lista de discos e blobs de VHD correspondentes.

Prepare a sua aplicação para o período de indisponibilidade. Para fazer uma migração limpa, terá de parar a todo o processamento do sistema atual. Só que, em seguida, pode obtê-lo para o estado consistente, que pode migrar para a nova plataforma. Duração de tempo de inatividade depende da quantidade de dados nos discos para migrar.


## <a name="migrate-the-vm"></a>Migrar a VM

Prepare a sua aplicação para o período de indisponibilidade. Para fazer uma migração limpa, terá de parar a todo o processamento do sistema atual. Só que, em seguida, pode obtê-lo para um estado consistente que pode migrar para a nova plataforma. Duração de tempo de inatividade depende da quantidade de dados nos discos para migrar.

Esta parte requer o Azure PowerShell versão do módulo 6.0.0 ou posterior. Executar ` Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Também precisa de executar `Connect-AzureRmAccount` para criar uma ligação ao Azure.


Crie variáveis para os parâmetros comuns.

```powershell
$resourceGroupName = 'yourResourceGroupName'

$location = 'your location' 

$virtualNetworkName = 'yourExistingVirtualNetworkName'

$virtualMachineName = 'yourVMName'

$virtualMachineSize = 'Standard_DS3'

$adminUserName = "youradminusername"

$adminPassword = "yourpassword" | ConvertTo-SecureString -AsPlainText -Force

$imageName = 'yourImageName'

$osVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd'

$dataVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk1.vhd'

$dataDiskName = 'dataDisk1'
```

Crie um disco de SO gerido com o VHD da VM clássica. Certifique-se de que forneceu o URI completo do VHD do SO para o parâmetro $osVhdUri. Além disso, introduza **- AccountType** como **Premium_LRS** ou **Standard_LRS** com base no tipo de discos (premium ou standard) estiver a migrar para.

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName '
   -Disk (New-AzureRmDiskConfig '
   -AccountType Premium_LRS '
   -Location $location '
   -CreateOption Import '
   -SourceUri $osVhdUri) '
   -ResourceGroupName $resourceGroupName
```

Anexe o disco do SO para a nova VM.

```powershell
$VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize
$VirtualMachine = Set-AzureRmVMOSDisk '
   -VM $VirtualMachine '
   -ManagedDiskId $osDisk.Id '
   -StorageAccountType Premium_LRS '
   -DiskSizeInGB 128 '
   -CreateOption Attach -Windows
```

Criar um disco de dados geridos a partir do ficheiro VHD de dados e adicioná-lo para a nova VM.

```powershell
$dataDisk1 = New-AzureRmDisk '
   -DiskName $dataDiskName '
   -Disk (New-AzureRmDiskConfig '
   -AccountType Premium_LRS '
   -Location $location '
   -CreationOption Import '
   -SourceUri $dataVhdUri ) '
   -ResourceGroupName $resourceGroupName
    
$VirtualMachine = Add-AzureRmVMDataDisk '
   -VM $VirtualMachine '
   -Name $dataDiskName '
   -CreateOption Attach '
   -ManagedDiskId $dataDisk1.Id '
   -Lun 1
```

Criar a nova VM ao definir o IP público, a rede virtual e a NIC.

```powershell
$publicIp = New-AzureRmPublicIpAddress '
   -Name ($VirtualMachineName.ToLower()+'_ip') '
   -ResourceGroupName $resourceGroupName '
   -Location $location '
   -AllocationMethod Dynamic
    
$vnet = Get-AzureRmVirtualNetwork '
   -Name $virtualNetworkName '
   -ResourceGroupName $resourceGroupName
    
$nic = New-AzureRmNetworkInterface '
   -Name ($VirtualMachineName.ToLower()+'_nic') '
   -ResourceGroupName $resourceGroupName '
   -Location $location '
   -SubnetId $vnet.Subnets[0].Id '
   -PublicIpAddressId $publicIp.Id
    
$VirtualMachine = Add-AzureRmVMNetworkInterface '
   -VM $VirtualMachine '
   -Id $nic.Id
    
New-AzureRmVM -VM $VirtualMachine '
   -ResourceGroupName $resourceGroupName '
   -Location $location
```

> [!NOTE]
>Pode haver passos adicionais necessários para suportar a sua aplicação que não é são cobertas neste guia.
>
>

## <a name="next-steps"></a>Passos Seguintes

- Ligue à máquina virtual. Para obter instruções, consulte [como ligar e iniciar sessão a uma máquina virtual do Azure a executar o Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

