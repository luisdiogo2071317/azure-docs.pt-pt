---
title: Criar imagens VM personalizadas com o Azure PowerShell | Microsoft Docs
description: Tutorial - criar uma imagem VM personalizada com o Azure PowerShell.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 443f47b98ea063c6fe1f0b3517c00b6cf3692161
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-custom-image-of-an-azure-vm-using-powershell"></a>Criar uma imagem personalizada da VM do Azure com o PowerShell

As imagens personalizadas são como imagens do marketplace, mas são criadas por si. As imagens personalizadas podem ser utilizadas para configurações do programa de arranque do sistema, como o pré-carregamento de aplicações, configurações de aplicação e outras configurações do SO. Neste tutorial, vai criar a sua imagem personalizada de uma máquina virtual do Azure. Saiba como:

> [!div class="checklist"]
> * Sysprep e generalizar VMs
> * Criar uma imagem personalizada
> * Criar uma VM a partir de uma imagem personalizada
> * Listar todas as imagens na sua subscrição
> * Eliminar uma imagem


## <a name="before-you-begin"></a>Antes de começar

Os passos abaixo detalham como tornar uma VM existente numa imagem personalizada reutilizável que pode utilizar para criar novas instâncias da VM.

Para concluir o exemplo neste tutorial, tem de ter uma máquina virtual existente. Se for preciso, este [script de exemplo](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) pode criar um para si. Quando trabalhar no tutorial, substitua o grupo de recursos e os nomes da VM sempre que preciso.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer a versão do módulo 5.6.0 de AzureRM ou posterior. Executar ` Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps).

## <a name="prepare-vm"></a>Preparar a VM

Para criar uma imagem de uma máquina virtual, terá de preparar a VM ao generalizar a VM, Desalocação e, em seguida, marcar a origem de VM como generalizado no Azure.

### <a name="generalize-the-windows-vm-using-sysprep"></a>Generalize a VM do Windows com o Sysprep

Sysprep remove todas as informações pessoais da conta, entre outras coisas e prepara a máquina para ser utilizado como uma imagem. Para obter detalhes sobre o Sysprep, consulte [como Sysprep de utilização: uma introdução](http://technet.microsoft.com/library/bb457073.aspx).


1. Ligar à máquina virtual.
2. Abra a janela de linha de comandos como administrador. Altere o diretório para *%windir%\system32\sysprep*e, em seguida, execute *sysprep.exe*.
3. No **ferramenta de preparação de sistema** caixa de diálogo, selecione *introduza sistema Out-of-Box experiência (OOBE)*e certifique-se de que o *Generalize* caixa de verificação está selecionada.
4. No **as opções de encerramento**, selecione *encerramento* e, em seguida, clique em **OK**.
5. Quando tiver concluído o Sysprep, encerrar a máquina virtual. **Não reiniciar a VM**.

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>Desaloque e marque a VM como generalizada

Para criar uma imagem, a VM tem de ser anulada e marcada como generalizado no Azure.

Desalocar VM utilizando [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Force
```

Definir o estado da máquina virtual para `-Generalized` utilizando [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm). 
   
```azurepowershell-interactive
Set-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Generalized
```


## <a name="create-the-image"></a>Criar a imagem

Agora pode criar uma imagem da VM utilizando [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) e [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage). O exemplo seguinte cria uma imagem designada *myImage* a partir de uma VM designada *myVM*.

Coloque a máquina virtual. 

```azurepowershell-interactive
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroup
```

Crie a configuração de imagem.

```azurepowershell-interactive
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

Crie a imagem.

```azurepowershell-interactive
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroup
``` 

 
## <a name="create-vms-from-the-image"></a>Criar VMs a partir da imagem

Agora que tem uma imagem, pode criar uma ou mais VMs novas partir da imagem. Criar uma VM a partir de uma imagem personalizada é muito semelhante à criação de uma VM com uma imagem do Marketplace. Quando utiliza uma imagem do Marketplace, terá de fornecer as informações sobre a imagem, o fornecedor de imagem, oferta, SKU e versão. Utilizando o parâmetro simplificado definido para o [New-AzureRMVM]() cmdlet, apenas terá de fornecer o nome da imagem personalizada, desde que está no mesmo grupo de recursos. 

Este exemplo cria uma VM chamada *myVMfromImage* do *myImage*, no *myResourceGroup*.


```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVMfromImage" `
    -ImageName "myImage" `
    -Location "East US" `
    -VirtualNetworkName "myImageVnet" `
    -SubnetName "myImageSubnet" `
    -SecurityGroupName "myImageNSG" `
    -PublicIpAddressName "myImagePIP" `
    -OpenPorts 3389
```

## <a name="image-management"></a>Gestão das imagens 

Seguem-se alguns exemplos de tarefas de imagem de gestão comuns e como conclui-los através do PowerShell.

Liste todas as imagens de por nome.

```azurepowershell-interactive
$images = Find-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

Elimine uma imagem. Este exemplo elimina a imagem denominada *myOldImage* do *myResourceGroup*.

```azurepowershell-interactive
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, criou uma imagem de VM personalizada. Aprendeu a:

> [!div class="checklist"]
> * Sysprep e generalizar VMs
> * Criar uma imagem personalizada
> * Criar uma VM a partir de uma imagem personalizada
> * Listar todas as imagens na sua subscrição
> * Eliminar uma imagem

Avançar para o próximo tutorial para saber mais sobre como elevadas máquinas virtuais.

> [!div class="nextstepaction"]
> [Criar VMs de elevada disponibilidade](tutorial-availability-sets.md)



