---
title: Tutorial - Utilizar uma imagem de VM personalizada num conjunto de dimensionamento com o Azure PowerShell | Microsoft Docs
description: Saiba como utilizar o Azure PowerShell para criar uma imagem de VM personalizada que pode utilizar para implementar um conjunto de dimensionamento de máquinas virtuais
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: a70bb09edb5ccaf4e216785933af80e2fcb2153a
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49363611"
---
# <a name="tutorial-create-and-use-a-custom-image-for-virtual-machine-scale-sets-with-azure-powershell"></a>Tutorial: Criar e utilizar uma imagem personalizada para conjuntos de dimensionamento de máquinas virtuais com o Azure PowerShell
Quando cria um conjunto de dimensionamento, tem de especificar uma imagem a ser utilizada quando as instâncias de VM são implementadas. Para reduzir o número de tarefas após as instâncias de VM serem implementadas, pode utilizar uma imagem de VM personalizada. Esta imagem de VM personalizada inclui instalações ou configurações de aplicações obrigatórias. Quaisquer instâncias de VM criadas no conjunto de dimensionamento utilizam a imagem de VM personalizada e estão prontas para apresentar o seu tráfego de aplicações. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar e personalizar uma VM
> * Desaprovisionar e generalizar a VM
> * Criar uma imagem de VM personalizada a partir da VM de origem
> * Implementar um conjunto de dimensionamento que utiliza a imagem de VM personalizada

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial precisará da versão 6.0.0 ou posterior do módulo do Azure PowerShell. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Connect-AzureRmAccount` para criar uma ligação com o Azure. 


## <a name="create-and-configure-a-source-vm"></a>Criar e configurar uma VM de origem

>[!NOTE]
> Este tutorial explica o processo de criar e utilizar uma imagem de VM generalizada. Não é suportado criar um conjunto de dimensionamento a partir de um VHD especializado.

Crie primeiro um grupo de recursos com [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) e, em seguida, crie uma VM com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Esta VM é depois utilizada como a origem de uma imagem de VM personalizada. O exemplo seguinte cria uma VM com o nome *myCustomVM* no grupo de recursos com o nome *myResourceGroup*. Quando lhe for pedido, introduza um nome de utilizador e uma palavra-passe a utilizar como credenciais de início de sessão para a VM:

```azurepowershell-interactive
# Create a resource a group
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"

# Create a Windows Server 2016 Datacenter VM
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Name "myCustomVM" `
  -ImageName "Win2016Datacenter" `
  -OpenPorts 3389
```

Para se ligar à sua VM, liste o endereço IP público através de [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress), da seguinte forma:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Crie uma ligação remota à VM. Se utiliza o Azure Cloud Shell, efetue este passo a partir de um pedido local do PowerShell ou do Cliente de Ambiente de Trabalho Remoto. Indique o seu endereço IP do comando anterior. Quando tal lhe for pedido, introduza as credenciais utilizadas quando criou a VM no primeiro passo:

```powershell
mstsc /v:<IpAddress>
```

Para personalizar a sua VM, vamos instalar um servidor Web básico. Quando a instância de VM no conjunto de dimensionamento for implementada, terá todos os pacotes necessários para executar uma aplicação Web. Abra um pedido local do PowerShell na VM e instale o servidor Web IIS com [Install-WindowsFeature](/powershell/module/servermanager/install-windowsfeature), da seguinte forma:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

O último passo para preparar a sua VM para utilização como imagem personalizada é gerar a VM. O Sysprep remove todas as suas informações de conta e configurações pessoais e repõe a VM para um estado limpo para futuras implementações. Para obter mais informações, consulte [Como Utilizar o Sysprep: Uma Introdução](http://technet.microsoft.com/library/bb457073.aspx).

Para generalizar a VM, execute o Sysprep e defina a VM para uma experiência fora da caixa. Quando terminar, indique ao Sysprep que encerre a VM:

```powershell
C:\Windows\system32\sysprep\sysprep.exe /oobe /generalize /shutdown
```

A ligação remota à VM é automaticamente fechada quando o Sysprep conclui o processo e a VM é encerrada.


## <a name="create-a-custom-vm-image-from-the-source-vm"></a>Criar uma imagem de VM personalizada a partir da VM de origem
A VM de origem está agora personalizada com o servidor Web IIS instalado. Vamos criar a imagem de VM personalizada para utilizar com um conjunto de dimensionamento.

Para criar uma imagem, a VM tem de ser desalocada. Desaloque a VM com [Stop-AzureRmVm](/powershell/module/azurerm.compute/stop-azurermvm). Em seguida, defina o estado da VM como generalizado com [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm), para que a plataforma do Azure saiba que a VM está pronta para utilizar uma imagem personalizada. Pode criar apenas uma imagem a partir de uma VM generalizada:

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myCustomVM" -Force
Set-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myCustomVM" -Generalized
```

Poderão ser necessários alguns minutos para desalocar e generalizar a VM.

Agora, crie uma imagem da VM com [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) e [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage). O exemplo seguinte cria uma imagem designada *myImage* a partir da sua VM:

```azurepowershell-interactive
# Get VM object
$vm = Get-AzureRmVM -Name "myCustomVM" -ResourceGroupName "myResourceGroup"

# Create the VM image configuration based on the source VM
$image = New-AzureRmImageConfig -Location "EastUS" -SourceVirtualMachineId $vm.ID 

# Create the custom VM image
New-AzureRmImage -Image $image -ImageName "myImage" -ResourceGroupName "myResourceGroup"
```


## <a name="create-a-scale-set-from-the-custom-vm-image"></a>Criar um conjunto de dimensionamento a partir da imagem de VM personalizada
Agora, crie um conjunto de dimensionamento com [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) que utilize o parâmetro `-ImageName` para definir a imagem de VM personalizada criada no passo anterior. Para distribuir o tráfego para instâncias de VM individuais, é também criado um balanceador de carga. O balanceador de carga inclui regras para distribuir o tráfego na porta TCP 80, bem como permitir o tráfego de ambiente de trabalho remoto na porta TCP 3389 e a comunicação remota do PowerShell na porta TCP 5985. Quando solicitado, forneça as suas próprias credenciais administrativas pretendidas para as instâncias de VM no conjunto de dimensionamento:

```azurepowershell-interactive
New-AzureRmVmss `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicyMode "Automatic" `
  -ImageName "myImage"
```

A criação e configuração de todas as VMs e recursos do conjunto de dimensionamento demora alguns minutos.


## <a name="test-your-scale-set"></a>Testar o seu conjunto de dimensionamento
Para ver o seu conjunto de dimensionamento em ação, obtenha o endereço IP público do seu balanceador de carga com [Get-AzureRmPublicIpAddress](/powershell/module/AzureRM.Network/Get-AzureRmPublicIpAddress), da seguinte forma:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -ResourceGroupName "myResourceGroup" `
  -Name "myPublicIPAddress" | Select IpAddress
```

Escreva o endereço IP público no seu browser. A página Web IIS predefinida é apresentada, conforme mostrado no seguinte exemplo:

![IIS executado da imagem de VM personalizada](media/tutorial-use-custom-image-powershell/default-iis-website.png)


## <a name="clean-up-resources"></a>Limpar recursos
Para remover o seu conjunto de dimensionamento e recursos adicionais, elimine o grupo de recursos e todos os respetivos recursos com [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup). O parâmetro `-Force` confirma que pretende eliminar os recursos sem uma linha de comandos adicional para fazê-lo. O parâmetro `-AsJob` devolve o controlo à linha de comandos, sem aguardar a conclusão da operação.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name "myResourceGroup" -Force -AsJob
```


## <a name="next-steps"></a>Passos seguintes
Neste tutorial, aprendeu a criar e utilizar uma imagem de VM personalizada nos seus conjuntos de dimensionamento com o Azure PowerShell:

> [!div class="checklist"]
> * Criar e personalizar uma VM
> * Desaprovisionar e generalizar a VM
> * Criar uma imagem de VM personalizada
> * Implementar um conjunto de dimensionamento que utiliza a imagem de VM personalizada

Prossiga para o próximo tutorial para saber como implementar aplicações no seu conjunto de dimensionamento.

> [!div class="nextstepaction"]
> [Implementar aplicações nos seus conjuntos de dimensionamento](tutorial-install-apps-powershell.md)
