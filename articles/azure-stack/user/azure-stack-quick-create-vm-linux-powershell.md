---
title: Criar uma máquina virtual Linux utilizando o PowerShell na pilha do Azure | Microsoft Docs
description: Crie uma máquina virtual Linux com o PowerShell na pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 03EE5929-4F05-47D7-B246-EA93D6FC47CD
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 04/24/2018
ms.author: mabrigg
ms.custom: mvc
ms.openlocfilehash: 1e2dbc6020dd317e96c4116811f8e3bf87680bfb
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
ms.locfileid: "33204399"
---
# <a name="quickstart-create-a-linux-server-virtual-machine-by-using-powershell-in-azure-stack"></a>Início rápido: criar uma máquina virtual do Linux servidor utilizando o PowerShell na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode criar uma máquina virtual do Ubuntu Server 16.04 LTS através do PowerShell de pilha do Azure. Siga os passos neste artigo para criar e utilizar uma máquina virtual.  Este artigo também dá-lhe os passos para:

* Ligar à máquina virtual com um cliente remoto.
* Instalar o servidor de web NGINX e ver a home page predefinida.
* Limpe os recursos não utilizados.

## <a name="prerequisites"></a>Pré-requisitos

* **Uma imagem do Linux no marketplace pilha do Azure**

   O marketplace de pilha do Azure não contém uma imagem de Linux por predefinição. Obter o operador de pilha do Azure para fornecer o **Ubuntu Server 16.04 LTS** imagem precisa. O operador pode utilizar os passos descritos no [transferir itens do marketplace do Azure para Azure pilha](../azure-stack-download-azure-marketplace-item.md) artigo.

* Pilha do Azure requer uma versão específica do Azure PowerShell para criar e gerir os recursos. Se não tiver configurado para a pilha do Azure do PowerShell, inicie sessão no [kit de desenvolvimento](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), ou um baseados em Windows externo cliente se [ligado através de VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) e siga os passos para [ instalar](azure-stack-powershell-install.md) e [configurar](azure-stack-powershell-configure-user.md) PowerShell.

* Uma chave SSH pública com o id_rsa.pub nome guardado no diretório. SSH do seu perfil de utilizador do Windows. Para obter informações detalhadas sobre a criação de chaves SSH, consulte [criar SSH chaves no Windows](../../virtual-machines/linux/ssh-from-windows.md).

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um grupo de recursos é um contentor lógico onde pode implementar e gerir recursos de pilha do Azure. A partir da sua kit de desenvolvimento ou o sistema de pilha do Azure integrado, execute o seguinte bloco de código para criar um grupo de recursos. Os valores são atribuídos para todas as variáveis neste documento, pode utilizar estes valores ou atribuir os novos valores.

```powershell
# Create variables to store the location and resource group names.
$location = "local"
$ResourceGroupName = "myResourceGroup"

New-AzureRmResourceGroup `
  -Name $ResourceGroupName `
  -Location $location
```

## <a name="create-storage-resources"></a>Criar recursos de armazenamento

Criar uma conta de armazenamento e, em seguida, crie um contentor de armazenamento para a imagem de Ubuntu Server 16.04 LTS.

```powershell
# Create variables to store the storage account name and the storage account SKU information
$StorageAccountName = "mystorageaccount"
$SkuName = "Standard_LRS"

# Create a new storage account
$StorageAccount = New-AzureRMStorageAccount `
  -Location $location `
  -ResourceGroupName $ResourceGroupName `
  -Type $SkuName `
  -Name $StorageAccountName

Set-AzureRmCurrentStorageAccount `
  -StorageAccountName $storageAccountName `
  -ResourceGroupName $resourceGroupName

# Create a storage container to store the virtual machine image
$containerName = 'osdisks'
$container = New-AzureStorageContainer `
  -Name $containerName `
  -Permission Blob
```

## <a name="create-networking-resources"></a>Criar recursos de rede

Crie uma rede virtual, uma sub-rede e um endereço IP público. Estes recursos são utilizados para fornecer conectividade de rede para a máquina virtual.

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -Name MyVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -AllocationMethod Static `
  -IdleTimeoutInMinutes 4 `
  -Name "mypublicdns$(Get-Random)"

```

### <a name="create-a-network-security-group-and-a-network-security-group-rule"></a>Crie um grupo de segurança de rede e uma regra de grupo de segurança de rede

O grupo de segurança de rede protege a máquina virtual utilizando regras de entrada e saídas. Crie uma regra de entrada para a porta 3389 para permitir ligações de ambiente de trabalho remoto de entrada e de uma regra de entrada para a porta 80 para permitir tráfego de entrada web.

```powershell
# Create an inbound network security group rule for port 22
$nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleSSH  -Protocol Tcp `
-Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 22 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
-Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location local `
-Name myNetworkSecurityGroup -SecurityRules $nsgRuleSSH,$nsgRuleWeb
```

### <a name="create-a-network-card-for-the-virtual-machine"></a>Criar uma placa de rede para a máquina virtual

A placa de rede liga a máquina virtual a uma sub-rede, um grupo de segurança de rede e um endereço IP público.

```powershell
# Create a virtual network card and associate it with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
  -Name myNic `
  -ResourceGroupName $ResourceGroupName `
  -Location $location `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id `
  -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-a-virtual-machine"></a>Criar uma máquina virtual

Criar uma configuração da máquina virtual. Esta configuração inclui as definições utilizadas quando implementar a máquina virtual. Por exemplo: credenciais de utilizador, tamanho e a imagem de máquina virtual.

```powershell
# Define a credential object.
$UserName='demouser'
$securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ($UserName, $securePassword)

# Create the virtual machine configuration object
$VmName = "VirtualMachinelatest"
$VmSize = "Standard_D1"
$VirtualMachine = New-AzureRmVMConfig `
  -VMName $VmName `
  -VMSize $VmSize

$VirtualMachine = Set-AzureRmVMOperatingSystem `
  -VM $VirtualMachine `
  -Linux `
  -ComputerName "MainComputer" `
  -Credential $cred

$VirtualMachine = Set-AzureRmVMSourceImage `
  -VM $VirtualMachine `
  -PublisherName "Canonical" `
  -Offer "UbuntuServer" `
  -Skus "16.04-LTS" `
  -Version "latest"

$osDiskName = "OsDisk"
$osDiskUri = '{0}vhds/{1}-{2}.vhd' -f `
  $StorageAccount.PrimaryEndpoints.Blob.ToString(),`
  $vmName.ToLower(), `
  $osDiskName

# Sets the operating system disk properties on a virtual machine.
$VirtualMachine = Set-AzureRmVMOSDisk `
  -VM $VirtualMachine `
  -Name $osDiskName `
  -VhdUri $OsDiskUri `
  -CreateOption FromImage | `
  Add-AzureRmVMNetworkInterface -Id $nic.Id

# Configure SSH Keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"

# Adds the SSH Key to the virtual machine
Add-AzureRmVMSshPublicKey -VM $VirtualMachine `
 -KeyData $sshPublicKey `
 -Path "/home/azureuser/.ssh/authorized_keys"

# Create the virtual machine.
New-AzureRmVM `
  -ResourceGroupName $ResourceGroupName `
 -Location $location `
  -VM $VirtualMachine
```

## <a name="connect-to-the-virtual-machine"></a>Ligar à máquina virtual

Depois da máquina virtual é implementada, configure uma ligação SSH para a máquina virtual. Utilize o comando [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress?view=azurermps-4.3.1) para devolver o endereço IP público da máquina virtual.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

A partir de um sistema de cliente com SSH instalado, utilize o seguinte comando para ligar à máquina virtual. Se estiver a trabalhar no Windows, pode utilizar [Putty](http://www.putty.org/) para criar a ligação.

```
ssh <Public IP Address>
```

Quando lhe for pedido, introduza azureuser como o utilizador de início de sessão. Se utilizou uma frase de acesso ao criar as chaves SSH, terá de fornecer o frase de acesso.

## <a name="install-the-nginx-web-server"></a>Instalar o servidor de web NGINX

Para recursos do pacote de atualização e instalar o pacote NGINX mais recente, execute o seguinte script:

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-nginx-welcome-page"></a>Ver a página de boas-vindas do NGINX

Com NGINX instalado e a porta 80 aberto na sua máquina virtual, que pode aceder ao servidor de web utilizando o endereço IP público da máquina virtual. Abra um browser e navegue para ```http://<public IP address>```.

![Página de boas-vindas NGINX web server](./media/azure-stack-quick-create-vm-linux-cli/nginx.png)

## <a name="clean-up-resources"></a>Limpar recursos

Limpe os recursos que não precisa de já. Pode utilizar o [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup?view=azurermps-4.3.1) comando para remover estes recursos. Para eliminar o grupo de recursos e todos os respetivos recursos, execute o seguinte comando:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Passos Seguintes

Este guia de introdução implementada uma máquina virtual do servidor de Linux básica. Para saber mais sobre as máquinas virtuais de pilha do Azure, aceda a [considerações para máquinas virtuais no Azure pilha](azure-stack-vm-considerations.md).
