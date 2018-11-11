---
title: Tutorial - Criar imagens de VM personalizadas com o Azure PowerShell | Microsoft Docs
description: Neste tutorial, vai aprender a utilizar o Azure PowerShell para criar uma imagem de máquina virtual personalizada no Azure
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
ms.date: 03/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 1481c32512266f494c03f9ffdc1ce67ef503b4a8
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51255205"
---
# <a name="tutorial-create-a-custom-image-of-an-azure-vm-with-azure-powershell"></a>Tutorial: Criar uma imagem personalizada de uma VM do Azure com o Azure PowerShell

As imagens personalizadas são como imagens do marketplace, mas são criadas por si. As imagens personalizadas podem ser utilizadas para configurações do programa de arranque do sistema, como o pré-carregamento de aplicações, configurações de aplicação e outras configurações do SO. Neste tutorial, vai criar a sua imagem personalizada de uma máquina virtual do Azure. Saiba como:

> [!div class="checklist"]
> * Executar o Sysprep e generalizar VMs
> * Criar uma imagem personalizada
> * Criar uma VM a partir de uma imagem personalizada
> * Listar todas as imagens na sua subscrição
> * Eliminar uma imagem

## <a name="before-you-begin"></a>Antes de começar

Os passos abaixo detalham como tornar uma VM existente numa imagem personalizada reutilizável que pode utilizar para criar novas instâncias da VM.

Para concluir o exemplo neste tutorial, tem de ter uma máquina virtual existente. Se for preciso, este [script de exemplo](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) pode criar um para si. Quando trabalhar no tutorial, substitua o grupo de recursos e os nomes da VM sempre que preciso.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer a versão 5.7.0 ou posterior do módulo Azure AzureRM. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps).

## <a name="prepare-vm"></a>Preparar a VM

Para criar uma imagem de uma máquina virtual, tem de preparar a VM ao generalizar a VM, desalocar e, em seguida, marcar a VM de origem como generalizada no Azure.

### <a name="generalize-the-windows-vm-using-sysprep"></a>Generalizar a VM do Windows com o Sysprep

O Sysprep remove todas as suas informações de conta pessoal, entre outras coisas, e prepara a máquina para ser utilizada como uma imagem. Para mais detalhes sobre o Sysprep, veja [Como utilizar o Sysprep: uma Introdução](https://technet.microsoft.com/library/bb457073.aspx).


1. Ligue à máquina virtual.
2. Abra a janela da Linha de Comandos como administrador. Altere o diretório para *%windir%\system32\sysprep* e, em seguida, execute *sysprep.exe*.
3. Na caixa de diálogo **Ferramenta de Preparação do Sistema**, selecione *Entrar na Experiência 1ª Execução (OOBE) do Sistema* e certifique-se de que a caixa de verificação *Generalizar* está selecionada.
4. Em **Opções de Encerramento**, selecione *Encerramento* e, em seguida, clique em **OK**.
5. Quando o Sysprep estiver concluído, encerra a máquina virtual. **Não reinicie a VM**.

### <a name="deallocate-and-mark-the-vm-as-generalized"></a>Desaloque e marque a VM como generalizada

Para criar uma imagem, a VM tem de ser desalocada e marcada como generalizada no Azure.

Desalocou a VM com [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).

```azurepowershell-interactive
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Force
```

Defina o estado da máquina virtual para `-Generalized`, com [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm). 
   
```azurepowershell-interactive
Set-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM -Generalized
```


## <a name="create-the-image"></a>Criar a imagem

Agora, pode criar uma imagem da VM com [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) e [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage). O exemplo seguinte cria uma imagem designada *myImage* a partir de uma VM designada *myVM*.

Obtenha a máquina virtual. 

```azurepowershell-interactive
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroup
```

Crie a configuração da imagem.

```azurepowershell-interactive
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

Crie a imagem.

```azurepowershell-interactive
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroup
``` 

 
## <a name="create-vms-from-the-image"></a>Criar VMs a partir da imagem

Agora que tem uma imagem, pode criar uma ou mais VMs novas a partir da imagem. Criar uma VM a partir de uma imagem personalizada é semelhante à criação de uma VM com uma imagem do Marketplace. Quando utiliza uma imagem do Marketplace, tem de fornecer as informações sobre a imagem, o fornecedor da imagem, a oferta, o SKU e a versão. Com o parâmetro simplificado definido para o cmdlet [New-AzureRMVM](), apenas terá de fornecer o nome da imagem personalizada, desde que esteja no mesmo grupo de recursos. 

Este exemplo cria uma VM com o nome *myVMfromImage* de *myImage*, no *myResourceGroup*.


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

Seguem-se alguns exemplos de tarefas comuns de imagens geridas e como concluí-las com o PowerShell.

Liste todas as imagens por nome.

```azurepowershell-interactive
$images = Get-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

Elimine uma imagem. Este exemplo elimina a imagem denominada *myOldImage* do *myResourceGroup*.

```azurepowershell-interactive
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou uma imagem de VM personalizada. Aprendeu a:

> [!div class="checklist"]
> * Executar o Sysprep e generalizar VMs
> * Criar uma imagem personalizada
> * Criar uma VM a partir de uma imagem personalizada
> * Listar todas as imagens na sua subscrição
> * Eliminar uma imagem

Avance para o próximo tutorial, para saber mais sobre máquinas virtuais de elevada disponibilidade.

> [!div class="nextstepaction"]
> [Criar VMs de elevada disponibilidade](tutorial-availability-sets.md)



