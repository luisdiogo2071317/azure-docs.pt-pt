---
title: Tutorial – Gerir discos do Azure com o Azure PowerShell | Microsoft Docs
description: Neste tutorial, vai aprender a utilizar Azure PowerShell para criar e gerir discos do Azure para máquinas virtuais
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/09/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: ea9d89b7dd94c38b326b83ff1fbf51595d67599a
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32190633"
---
# <a name="tutorial---manage-azure-disks-with-azure-powershell"></a>Tutorial – Gerir discos do Azure com o Azure PowerShell

As máquinas virtuais do Azure utilizam discos para armazenar o sistema operativo, as aplicações e os dados das VMs. Ao criar uma VM, é importante escolher um tamanho de disco e a configuração adequados para a carga de trabalho esperada. Este tutorial abrange a implementação e gestão de discos de VM. Vai obter informações sobre:

> [!div class="checklist"]
> * Discos de SO e discos temporários
> * Discos de dados
> * Discos Standard e Premium
> * Desempenho do disco
> * Expor e preparar os discos de dados

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer o módulo do Azure PowerShell versão 5.7.0 ou posterior. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Connect-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="default-azure-disks"></a>Discos do Azure predefinidos

Quando uma máquina virtual do Azure é criada, dois discos são automaticamente expostos à máquina virtual. 

**Disco do sistema operativo** - os discos do sistema operativo podem ter o tamanho máximo de 4 terabytes e alojam o sistema operativo das VMs.  O disco do SO é atribuído a uma letra de unidade de *c:* por predefinição. A configuração da colocação em cache do disco do SO está otimizada para desempenho do SO. O disco do SO **não deve** alojar aplicações ou dados. Para aplicações e dados, utilize um disco de dados, que se encontra detalhado posteriormente neste artigo.

**Disco temporário** - os discos temporários utilizam uma unidade de estado sólido que está localizada no mesmo anfitrião da VM do Azure. Os discos temporários são de elevado desempenho e servem para operações como o processamento de dados temporários. No entanto, se a VM for movida para um novo anfitrião, todos os dados armazenados num disco temporário são removidos. O tamanho do disco temporário é determinado pelo tamanho da VM. Os discos temporários estão atribuídos a uma letra de unidade de *d:* por predefinição.

### <a name="temporary-disk-sizes"></a>Tamanhos dos discos temporários

| Tipo | Tamanhos comuns | Tamanho máximo de disco temporário (GiB) |
|----|----|----|
| [Fins gerais](sizes-general.md) | Séries A, B e D | 1600 |
| [Com otimização de computação](sizes-compute.md) | Série F | 576 |
| [Com otimização de memória](sizes-memory.md) | Séries D, E, G e M | 6144 |
| [Com otimização de armazenamento](sizes-storage.md) | Série L | 5630 |
| [GPU](sizes-gpu.md) | Série N | 1440 |
| [Elevado desempenho](sizes-hpc.md) | Séries A e H | 2000 |

## <a name="azure-data-disks"></a>Discos de dados do Azure

Podem ser adicionados mais discos para instalar aplicações e armazenar dados. Os discos de dados devem ser utilizados em qualquer situação em que se pretenda armazenamento de dados duradouro e reativo. Cada disco de dados tem a capacidade máxima de 4 terabytes. O tamanho da máquina virtual determina quantos discos de dados podem ser expostos a uma VM. Para cada vCPU de VM, podem ser expostos dois discos de dados. 

### <a name="max-data-disks-per-vm"></a>Discos de dados máximos por VM

| Tipo | Tamanhos comuns | Discos de dados máximos por VM |
|----|----|----|
| [Fins gerais](sizes-general.md) | Séries A, B e D | 64 |
| [Com otimização de computação](sizes-compute.md) | Série F | 64 |
| [Com otimização de memória](sizes-memory.md) | Séries D, E, G e M | 64 |
| [Com otimização de armazenamento](sizes-storage.md) | Série L | 64 |
| [GPU](sizes-gpu.md) | Série N | 64 |
| [Elevado desempenho](sizes-hpc.md) | Séries A e H | 64 |

## <a name="vm-disk-types"></a>Tipos de disco de VM

O Azure oferece dois tipos de disco.

### <a name="standard-disk"></a>Disco Standard

O Armazenamento Standard está protegido por HDDs e fornece armazenamento económico, mantendo o desempenho. Os discos Standard são ideais para uma carga de trabalho de desenvolvimento e teste económica.

### <a name="premium-disk"></a>Disco Premium

Os discos Premium são apoiados por um disco de elevado desempenho baseado em SSD e de baixa latência. São perfeitos para as VMs com carga de trabalho de produção. O Armazenamento Premium suporta VMs da série DS, série DSv2, série GS e série FS. Os discos Premium são fornecidos em cinco tipos (P10, P20, P30, P40, P50), e o tamanho do disco determina o tipo de disco. Ao selecionar um tamanho de disco, o valor é arredondado para o tipo seguinte. Por exemplo, se o tamanho for inferior a 128 GB, o tipo de disco é P10 ou entre 129 GB e 512 GB, o disco é P20.

### <a name="premium-disk-performance"></a>Desempenho do disco Premium

|Tipo de disco de armazenamento Premium | P4 | P6 | P10 | P20 | P30 | P40 | P50 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Tamanho do disco (arredondado) | 32 GB | 64 GB | 128 GB | 512 GB | 1.024 GB (1 TB) | 2.048 GB (2 TB) | 4.095 GB (4 TB) |
| IOPs Máx por disco | 120 | 240 | 500 | 2.300 | 5.000 | 7.500 | 7.500 |
Débito por disco | 25 MB/s | 50 MB/s | 100 MB/s | 150 MB/s | 200 MB/s | 250 MB/s | 250 MB/s |

Enquanto a tabela acima identifica o IOPS máximo por disco, um nível mais elevado de desempenho pode ser alcançado ao repartir vários discos de dados. Por exemplo, podem ser anexados 64 discos de dados à VM Standard_GS5. Se cada um destes discos for dimensionado como um P30, pode ser alcançado um máximo de 80.000 IOPS. Para obter informações detalhadas sobre o IOPS máximo por VM, veja [Tipos e tamanhos de VMs](./sizes.md).

## <a name="create-and-attach-disks"></a>Criar e anexar discos

Para concluir o exemplo neste tutorial, tem de ter uma máquina virtual existente. Se for preciso, crie uma máquina virtual com os seguintes comandos.

Defina o nome de utilizador e a palavra-passe necessários para a conta de administrador na máquina virtual com [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Criar a máquina virtual com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupDisk" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred `
    -AsJob
```

O parâmetro `-AsJob` cria a VM como uma tarefa em segundo plano, para que os pedidos do PowerShell voltem para si. Pode ver os detalhes das tarefas em segundo plano com o cmdlet `Job`.

Crie a configuração inicial com [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig). O exemplo a seguir configura um disco com 128 gigabytes.

```azurepowershell-interactive
$diskConfig = New-AzureRmDiskConfig `
    -Location "EastUS" `
    -CreateOption Empty `
    -DiskSizeGB 128
```

Crie o disco de dados com o comando [New-AzureRmDisk](/powershell/module/azurerm.compute/new-azurermdisk).

```azurepowershell-interactive
$dataDisk = New-AzureRmDisk `
    -ResourceGroupName "myResourceGroupDisk" `
    -DiskName "myDataDisk" `
    -Disk $diskConfig
```

Obtenha a máquina virtual que pretende para adicionar o disco de dados com o comando [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm).

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroupDisk" -Name "myVM"
```

Adicione o disco de dados à configuração da máquina virtual com o comando [Add-AzureRmVMDataDisk](/powershell/module/azurerm.compute/add-azurermvmdatadisk).

```azurepowershell-interactive
$vm = Add-AzureRmVMDataDisk `
    -VM $vm `
    -Name "myDataDisk" `
    -CreateOption Attach `
    -ManagedDiskId $dataDisk.Id `
    -Lun 1
```

Atualize a máquina virtual com o comando [Update-AzureRmVM](/powershell/module/azurerm.compute/add-azurermvmdatadisk).

```azurepowershell-interactive
Update-AzureRmVM -ResourceGroupName "myResourceGroupDisk" -VM $vm
```

## <a name="prepare-data-disks"></a>Preparar discos de dados

Depois de um disco ser exposto à máquina virtual, o sistema operativo tem de ser configurado para utilizar o disco. O exemplo seguinte mostra como configurar manualmente o primeiro disco adicionado à VM. Também pode utilizar a [extensão de script personalizada](./tutorial-automate-vm-deployment.md) para automatizar este processo.

### <a name="manual-configuration"></a>Configuração manual

Crie uma ligação RDP com a máquina virtual. Abra o PowerShell e execute este script.

```azurepowershell
Get-Disk | Where partitionstyle -eq 'raw' | `
Initialize-Disk -PartitionStyle MBR -PassThru | `
New-Partition -AssignDriveLetter -UseMaximumSize | `
Format-Volume -FileSystem NTFS -NewFileSystemLabel "myDataDisk" -Confirm:$false
```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu sobre os tópicos de discos de VM, como:

> [!div class="checklist"]
> * Discos de SO e discos temporários
> * Discos de dados
> * Discos Standard e Premium
> * Desempenho do disco
> * Expor e preparar os discos de dados

Avance para o próximo tutorial para saber mais sobre como automatizar a configuração da VM.

> [!div class="nextstepaction"]
> [Automatizar a configuração da VM](./tutorial-automate-vm-deployment.md)
