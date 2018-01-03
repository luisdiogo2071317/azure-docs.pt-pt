---
title: "Criar uma conta de armazenamento e de VM para uma aplicação dimensionável no Azure | Microsoft Docs"
description: "Saiba como implementar uma VM para ser utilizada para executar uma aplicação escalável utilizando o armazenamento de Blobs do Azure"
services: storage
documentationcenter: 
author: georgewallace
manager: jeconnoc
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 12/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 0fd1cd93ca6faabcbe0007136fe427028e722733
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/23/2017
---
# <a name="create-a-virtual-machine-and-storage-account-for-a-scalable-application"></a>Criar uma máquina virtual e a conta de armazenamento para uma aplicação dimensionável

Este tutorial faz parte de um de uma série. Este tutorial mostra que implementar uma aplicação que carrega e transferir grandes quantidades de dados aleatórios com uma conta de armazenamento do Azure. Quando tiver terminado, terá uma aplicação de consola em execução numa máquina virtual que carregar e transferir grandes quantidades de dados para uma conta de armazenamento.

Na parte de uma série, saiba como:

> [!div class="checklist"]
> * Criar uma conta do Storage
> * Criar uma máquina virtual
> * Configurar uma extensão de script personalizado

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer o módulo do Azure PowerShell versão 3.6 ou posterior. Executar ` Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos do Azure com [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-storage-account"></a>Criar uma conta do Storage
 
O exemplo carrega 50 ficheiros grandes para um contentor de BLOBs numa conta do Storage do Azure. Uma conta do storage fornece um espaço de nomes exclusivo para armazenar e aceder aos seus objetos de dados de armazenamento do Azure. Criar uma conta de armazenamento no grupo de recursos que criou utilizando o [New-AzureRmStorageAccount](/powershell/module/AzureRM.Storage/New-AzureRmStorageAccount) comando.

O seguinte comando, substitua o seus próprios nome globalmente exclusivo para a conta de armazenamento de BLOBs onde vir o `<blob_storage_account>` marcador de posição.

```powershell-interactive
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName myResourceGroup `
  -Name "<blob_storage_account>" `
  -Location EastUS `
  -SkuName Standard_LRS `
  -Kind Storage `
  -EnableEncryptionService Blob
```

## <a name="create-a-virtual-machine"></a>Criar uma máquina virtual

Criar uma configuração da máquina virtual. Esta configuração inclui as definições que são utilizadas ao implementar a máquina virtual, como uma imagem de máquina virtual, o tamanho e a configuração da autenticação. Ao executar este passo, serão pedidas credenciais. Os valores que introduzir são configurados, como o nome de utilizador e a palavra-passe para a máquina virtual.

Criar a máquina virtual com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```azurepowershell-interactive
# Variables for common values
$resourceGroup = "myResourceGroup"
$location = "eastus"
$vmName = "myVM"

# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Location $location `
  -Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup -Location $location `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4

# Create a virtual network card and associate with public IP address
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -Location $location `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS14_v2 | `
    Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
    Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer `
    -Skus 2016-Datacenter -Version latest | Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create a virtual machine
New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig

Write-host "Your public IP address is $($pip.IpAddress)"
```

## <a name="deploy-configuration"></a>Configuração de implementação

Para este tutorial, existem pré-requisitos que devem ser instalados na máquina virtual. A extensão de script personalizado é utilizada para executar um script do PowerShell que conclui as seguintes tarefas:

> [!div class="checklist"]
> * Instale .NET 2.0
> * Instalação chocolatey
> * Instalar o GIT
> * Clone o repositório de exemplo
> * Restaurar os pacotes NuGet
> * Cria 50 de 1 GB de ficheiros com dados aleatórios

Execute o seguinte cmdlet para finalizar a configuração da máquina virtual. Este passo demora 5-15 minutos a concluir.

```azurepowershell-interactive
# Start a CustomScript extension to use a simple PowerShell script to install .NET core, dependencies, and pre-create the files to upload.
Set-AzureRMVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location EastUS `
    -FileUri https://raw.githubusercontent.com/azure-samples/storage-dotnet-perf-scale-app/master/setup_env.ps1 `
    -Run 'setup_env.ps1' `
    -Name DemoScriptExtension
```

## <a name="next-steps"></a>Passos Seguintes

Parte de uma série, aprendeu sobre como criar uma conta de armazenamento, implementar uma máquina virtual e configurar a máquina virtual com os pré-requisitos necessários, tais como:

> [!div class="checklist"]
> * Criar uma conta do Storage
> * Criar uma máquina virtual
> * Configurar uma extensão de script personalizado

Avançar para a parte dois da série carregar grandes quantidades de dados para uma conta de armazenamento utilizando repetição exponencial e paralelismo.

> [!div class="nextstepaction"]
> [Carregar grandes quantidades de ficheiros grandes em paralelo com uma conta de armazenamento](storage-blob-scalable-app-upload-files.md)
