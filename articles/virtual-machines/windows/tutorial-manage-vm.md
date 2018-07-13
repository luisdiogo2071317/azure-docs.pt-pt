---
title: Tutorial – Criar e gerir VMs do Windows com o Azure PowerShell | Microsoft Docs
description: Neste tutorial, vai aprender a utilizar o Azure PowerShell para criar e gerir VMs do Windows no Azure
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/23/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: d47981042fc13a96bdf5cb9690e4dc83a6aa0162
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37932556"
---
# <a name="tutorial-create-and-manage-windows-vms-with-azure-powershell"></a>Tutorial: Criar e gerir VMs do Windows com o Azure PowerShell

As máquinas virtuais do Azure proporcionam um ambiente informático totalmente configurável e flexível. Este tutorial abrange itens de implementação básicos de máquinas virtuais do Azure, como selecionar um tamanho de VM, selecionar uma imagem de VM e implementar uma VM. Saiba como:

> [!div class="checklist"]
> * Criar e ligar a uma VM
> * Selecionar e utilizar imagens de VM
> * Ver e utilizar tamanhos específicos de VM
> * Redimensionar uma VM
> * Visualizar e compreender o estado de uma VM

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer o módulo do Azure PowerShell versão 5.7.0 ou posterior. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Connect-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="create-resource-group"></a>Criar grupo de recursos

Crie um grupo de recursos com o comando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). 

Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. Um grupo de recursos tem de ser criado antes de uma máquina virtual. No exemplo seguinte, é criado um grupo de recursos designado *myResourceGroupVM* na região *EastUS*:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName "myResourceGroupVM" -Location "EastUS"
```

O grupo de recursos é especificado ao criar ou modificar uma VM, o que pode ser visto ao longo deste tutorial.

## <a name="create-virtual-machine"></a>Criar a máquina virtual

Ao criar uma máquina virtual, estão disponíveis várias opções, como a imagem do sistema operativo, a configuração da rede as credenciais administrativas. Neste exemplo, é criada uma máquina virtual designada *myVM* com a versão mais recente predefinida do Windows Server 2016 Datacenter.

Defina o nome de utilizador e a palavra-passe necessários para a conta de administrador na máquina virtual com [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Criar a máquina virtual com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupVM" `
    -Name "myVM" `
    -Location "EastUS" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -Credential $cred
```

## <a name="connect-to-vm"></a>Ligar à VM

Depois de a implementação estar concluída, crie uma ligação de ambiente de trabalho remoto à máquina virtual.

Execute os seguintes comandos para devolver o endereço IP público da máquina virtual. Tome nota deste endereço IP, para que se possa ligar ao mesmo com o seu browser e testar a conectividade Web num passo posterior.

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName "myResourceGroupVM"  | Select IpAddress
```

Utilize o seguinte comando na sua máquina virtual para criar uma sessão de ambiente de trabalho remoto com a máquina virtual. Substitua o endereço IP com o *publicIPAddress* da máquina virtual. Quando lhe for pedido, introduza as credenciais utilizadas ao criar a máquina virtual.

```powershell
mstsc /v:<publicIpAddress>
```

Na janela **Segurança do Windows**, selecione **Mais escolhas** e **Utilizar uma conta diferente**. Escreva o nome de utilizador e a palavra-passe que criou para a máquina virtual e clique em **OK**.

## <a name="understand-vm-images"></a>Compreender as imagens de VM

O Azure marketplace inclui várias imagens de máquina virtual que podem ser utilizadas para criar uma nova máquina virtual. Nos passos anteriores, foi criada uma máquina virtual com uma imagem do Windows Server 2016 Datacenter. Neste passo, o módulo do PowerShell serve para pesquisar outras imagens do Windows no marketplace, que pode também ser utilizado como base para novas VMs. Este processo consiste em encontrar o publicador, a oferta, o SKU e, opcionalmente, um número de versão para [identificar](cli-ps-findimage.md#terminology) a imagem. 

Utilize o comando [Get-AzureRmVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher) para devolver uma lista de editores de imagem:

```powershell
Get-AzureRmVMImagePublisher -Location "EastUS"
```

Utilize o [Get-AzureRmVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer) para devolver uma lista de ofertas de imagem. Com este comando, a lista devolvida é filtrada no publicador especificado:

```azurepowershell-interactive
Get-AzureRmVMImageOffer -Location "EastUS" -PublisherName "MicrosoftWindowsServer"
```

```azurepowershell-interactive
Offer             PublisherName          Location
-----             -------------          -------- 
Windows-HUB       MicrosoftWindowsServer EastUS 
WindowsServer     MicrosoftWindowsServer EastUS   
WindowsServer-HUB MicrosoftWindowsServer EastUS   
```

O comando [Get-AzureRmVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) irá, em seguida, filtrar no nome do publicador e da oferta para devolver uma lista de nomes de imagem.

```azurepowershell-interactive
Get-AzureRmVMImageSku -Location "EastUS" -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer"
```

```azurepowershell-interactive
Skus                                      Offer         PublisherName          Location
----                                      -----         -------------          --------
2008-R2-SP1                               WindowsServer MicrosoftWindowsServer EastUS  
2008-R2-SP1-smalldisk                     WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter                           WindowsServer MicrosoftWindowsServer EastUS  
2012-Datacenter-smalldisk                 WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter                        WindowsServer MicrosoftWindowsServer EastUS  
2012-R2-Datacenter-smalldisk              WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter                           WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core               WindowsServer MicrosoftWindowsServer EastUS  
2016-Datacenter-Server-Core-smalldisk     WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-smalldisk                 WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-Containers           WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-Containers-smalldisk WindowsServer MicrosoftWindowsServer EastUS
2016-Datacenter-with-RDSH                 WindowsServer MicrosoftWindowsServer EastUS
2016-Nano-Server                          WindowsServer MicrosoftWindowsServer EastUS
```

Estas informações podem ser utilizadas para implementar uma VM com uma imagem específica. Este exemplo implementa uma máquina virtual com a versão mais recente de uma imagem de Contentores do Windows Server 2016.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroupVM" `
    -Name "myVM2" `
    -Location "EastUS" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress2" `
    -ImageName "MicrosoftWindowsServer:WindowsServer:2016-Datacenter-with-Containers:latest" `
    -Credential $cred `
    -AsJob
```

O parâmetro `-AsJob` cria a VM como uma tarefa em segundo plano, para que os pedidos do PowerShell voltem para si. Pode ver os detalhes das tarefas em segundo plano com o cmdlet `Get-Job`.


## <a name="understand-vm-sizes"></a>Compreender os tamanhos de VM

Um tamanho de máquina virtual determina a quantidade de recursos de computação, como a CPU, memória e GPU que ficam disponíveis para a máquina virtual. As máquinas virtuais têm de ser criadas com um tamanho adequado para a carga de trabalho esperada. Se a carga de trabalho aumentar, uma máquina virtual existente pode ser redimensionada.

### <a name="vm-sizes"></a>Tamanhos de VMs

A tabela seguinte categoriza tamanhos em casos de utilização.  
| Tipo                     | Tamanhos comuns           |    Descrição       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Fins gerais](sizes-general.md)         |Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7| CPU-para-memória equilibrada. Ideal para desenvolvimento/teste e aplicações e soluções de dados pequenas a médias.  |
| [Com otimização de computação](sizes-compute.md)   | Fs, F             | CPU-para-memória elevada. É adequado para aplicações de tráfego médio, dispositivos de rede e processos em lote.        |
| [Com otimização de memória](sizes-memory.md)    | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Memória-para-núcleo elevada. É ideal para bases de dados relacionais, caches médias a grandes e análise dentro da memória.                 |
| [Com otimização de armazenamento](sizes-storage.md)      | Ls                | Débito e E/S de disco elevados. Ideal para bases de dados de Macrodados, SQL e NoSQL.                                                         |
| [GPU](sizes-gpu.md)          | NV, NC            | VMs especializadas destinadas a composição gráfica e edição de vídeo exigentes.       |
| [Elevado desempenho](sizes-hpc.md) | H, A8-11          | As nossas mais poderosas VMs com CPU, com interfaces de rede de alto débito (RDMA) opcionais. 


### <a name="find-available-vm-sizes"></a>Encontrar todos os tamanhos de VM disponíveis

Para ver uma lista de tamanhos de VM disponíveis numa região específica, utilize o comando [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize).

```azurepowershell-interactive
Get-AzureRmVMSize -Location "EastUS"
```

## <a name="resize-a-vm"></a>Redimensionar uma VM

Depois de implementar uma VM, esta pode ser redimensionada para aumentar ou diminuir a alocação de recursos.

Antes de redimensionar uma VM, verifique se o tamanho pretendido está disponível no cluster da VM atual. O comando [Get-AzureRmVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) devolve uma lista de tamanhos. 

```azurepowershell-interactive
Get-AzureRmVMSize -ResourceGroupName "myResourceGroupVM" -VMName "myVM"
```

Se o tamanho pretendido estiver disponível, a VM pode ser redimensionada de um estado ligado, no entanto, é reiniciada durante a operação.

```azurepowershell-interactive
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroupVM"  -VMName "myVM"
$vm.HardwareProfile.VmSize = "Standard_D4"
Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroupVM"
```

Se o tamanho pretendido não estiver no cluster atual, a VM tem de ser desalocada antes de a operação de redimensionamento poder ocorrer. Tenha em atenção que quando a VM é novamente ativada, são removidos os dados no disco temporário e os endereços IP públicos, a menos que esteja a ser utilizado um endereço IP estático. 

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName "myResourceGroupVM" -Name "myVM" -Force
$vm = Get-AzureRmVM -ResourceGroupName "myResourceGroupVM"  -VMName "myVM"
$vm.HardwareProfile.VmSize = "Standard_F4s"
Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroupVM"
Start-AzureRmVM -ResourceGroupName "myResourceGroupVM"  -Name $vm.name
```

## <a name="vm-power-states"></a>Estados de energia da VM

Uma VM do Azure pode ter um de vários estados de energia. Este estado representa o estado atual da VM do ponto de vista do hipervisor. 

### <a name="power-states"></a>Estados de energia

| Estado de Energia | Descrição
|----|----|
| A iniciar | Indica que a máquina virtual está a iniciar. |
| A executar | Indica que a máquina virtual está em execução. |
| A parar | Indica que a máquina virtual está a ser parada. | 
| Parada | Indica que a máquina virtual está parada. Note que as máquinas virtuais no estado de paragem continuam a incorrer em custos de computação.  |
| A desalocar | Indica que a máquina virtual está a ser desalocada. |
| Desalocada | Indica que a máquina virtual foi completamente removida do hipervisor, mas continua disponível no painel de controlo. As máquinas virtuais no estado Desalocada não incorrem em custos de computação. |
| - | Indica que o estado de energia da máquina virtual é desconhecido. |

### <a name="find-power-state"></a>Encontrar estado de energia

Para obter o estado de uma VM específica, utilize o comando [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm). Certifique-se de que especifica um nome válido para a máquina virtual e o grupo de recursos. 

```azurepowershell-interactive
Get-AzureRmVM `
    -ResourceGroupName "myResourceGroupVM" `
    -Name "myVM" `
    -Status | Select @{n="Status"; e={$_.Statuses[1].Code}}
```

Saída:

```azurepowershell-interactive
Status
------
PowerState/running
```

## <a name="management-tasks"></a>Tarefas de gestão

Durante o ciclo de vida de uma máquina virtual, poderá querer executar tarefas de gestão, como iniciar, parar ou eliminar uma máquina virtual. Além disso, pode querer criar scripts para automatizar tarefas repetitivas ou complexas. Ao utilizar o Azure PowerShell, muitas tarefas comuns de gestão podem ser executadas na linha de comandos ou em scripts.

### <a name="stop-virtual-machine"></a>Parar a máquina virtual

Pare e anule a atribuição de uma máquina virtual com [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm):

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName "myResourceGroupVM" -Name "myVM" -Force
```

Se pretender manter a máquina virtual num estado aprovisionado, utilize o parâmetro -StayProvisioned.

### <a name="start-virtual-machine"></a>Iniciar a máquina virtual

```azurepowershell-interactive
Start-AzureRmVM -ResourceGroupName "myResourceGroupVM" -Name "myVM"
```

### <a name="delete-resource-group"></a>Eliminar grupo de recursos

Eliminar um grupo de recursos também elimina todos os recursos contidos no grupo.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name "myResourceGroupVM" -Force
```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, aprendeu sobre a criação e gestão básica de VMs, como:

> [!div class="checklist"]
> * Criar e ligar a uma VM
> * Selecionar e utilizar imagens de VM
> * Ver e utilizar tamanhos específicos de VM
> * Redimensionar uma VM
> * Visualizar e compreender o estado de uma VM

Avance para o próximo tutorial para saber mais sobre os discos de VM.  

> [!div class="nextstepaction"]
> [Criar e Gerir discos de VM](./tutorial-manage-data-disk.md)
