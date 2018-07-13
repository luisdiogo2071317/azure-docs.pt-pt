---
title: Criar uma VM com um endereço IP público estático - Azure PowerShell | Documentos da Microsoft
description: Saiba como criar uma VM com um endereço IP público estático através do PowerShell.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ad975ab9-d69f-45c1-9e45-0d3f0f51e87e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 68656db0b76a29e7ab36fd6fa9ad4647712233ee
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38696588"
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-powershell"></a>Criar uma VM com um endereço IP público estático através do PowerShell

> [!div class="op_single_selector"]
> * [Portal do Azure](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [CLI do Azure](virtual-network-deploy-static-pip-arm-cli.md)
> * [PowerShell (Clássico)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com os recursos: [Resource Manager e clássico](../resource-manager-deployment-model.md). Este artigo explica como utilizar o modelo de implementação do Resource Manager, que é recomendado pela Microsoft para as implementações mais novas em vez do modelo de implementação clássica.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="start-your-script"></a>Iniciar o seu script
Pode transferir o script do PowerShell completo utilizado [aqui](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/virtual-network-deploy-static-pip-arm-ps.ps1). Siga os passos abaixo para alterar o script funcione no seu ambiente.

Altere os valores das variáveis abaixo com base nos valores que pretende utilizar para a sua implementação. Os seguintes valores mapeiam para o cenário usado neste artigo:

```powershell
# Set variables resource group
$rgName                = "IaaSStory"
$location              = "West US"

# Set variables for VNet
$vnetName              = "WTestVNet"
$vnetPrefix            = "192.168.0.0/16"
$subnetName            = "FrontEnd"
$subnetPrefix          = "192.168.1.0/24"

# Set variables for storage
$stdStorageAccountName = "iaasstorystorage"

# Set variables for VM
$vmSize                = "Standard_A1"
$diskSize              = 127
$publisher             = "MicrosoftWindowsServer"
$offer                 = "WindowsServer"
$sku                   = "2012-R2-Datacenter"
$version               = "latest"
$vmName                = "WEB1"
$osDiskName            = "osdisk"
$nicName               = "NICWEB1"
$privateIPAddress      = "192.168.1.101"
$pipName               = "PIPWEB1"
$dnsName               = "iaasstoryws1"
```

## <a name="create-the-necessary-resources-for-your-vm"></a>Criar os recursos necessários para a sua VM
Antes de criar uma VM, terá de um grupo de recursos, VNet, IP público e NIC a ser utilizado pela VM.

1. Crie um novo grupo de recursos.

    ```powershell
    New-AzureRmResourceGroup -Name $rgName -Location $location
    ```

2. Crie a VNet e sub-rede.

    ```powershell
    $vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName `
        -AddressPrefix $vnetPrefix -Location $location

    Add-AzureRmVirtualNetworkSubnetConfig -Name $subnetName `
        -VirtualNetwork $vnet -AddressPrefix $subnetPrefix

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

3. Crie o recurso IP público. 

    ```powershell
    $pip = New-AzureRmPublicIpAddress -Name $pipName -ResourceGroupName $rgName `
        -AllocationMethod Static -DomainNameLabel $dnsName -Location $location
    ```

4. Crie a interface de rede (NIC) para a VM na sub-rede criada acima, com o IP público. Tenha em atenção o primeiro cmdlet ao obter a VNet do Azure, isto é necessário desde um `Set-AzureRmVirtualNetwork` foi executado para alterar a VNet existente.

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
        -Subnet $subnet -Location $location -PrivateIpAddress $privateIPAddress `
        -PublicIpAddress $pip
    ```

5. Crie uma conta de armazenamento para alojar a unidade do SO da VM.

    ```powershell
    $stdStorageAccount = New-AzureRmStorageAccount -Name $stdStorageAccountName `
    -ResourceGroupName $rgName -Type Standard_LRS -Location $location
    ```

## <a name="create-the-vm"></a>Crie a VM
Agora que todos os recursos necessários estão em vigor, pode criar uma nova VM.

1. Crie o objeto de configuração para a VM.

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
    ```

2. Obter as credenciais para a conta de administrador local de VM.

    ```powershell
    $cred = Get-Credential -Message "Type the name and password for the local administrator account."
    ```

3. Crie um objeto de configuração de VM.

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $vmName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    ```

4. Defina a imagem do sistema operativo para a VM.

    ```powershell
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisher `
        -Offer $offer -Skus $sku -Version $version
    ```

5. Configure o disco do SO.

    ```powershell
    $osVhdUri = $stdStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $osDiskName + ".vhd"
    $vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $osDiskName -VhdUri $osVhdUri -CreateOption fromImage
    ```

6. Adicione a NIC à VM.

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id -Primary
    ```

7. Crie a VM.

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName $rgName -Location $location
    ```

8. Guarde o ficheiro de script.

## <a name="run-the-script"></a>Execute o script

Depois de fazer as alterações necessárias, execute o script anterior. A máquina virtual é criada após alguns minutos.

## <a name="set-ip-addresses-within-the-operating-system"></a>Conjunto de endereços IP no sistema operativo

Nunca manualmente deve atribuir o endereço IP público atribuído a uma máquina virtual do Azure no sistema de operativo da máquina virtual. É recomendável que não atribuir estaticamente IP privado atribuído à máquina virtual do Azure no sistema operativo de uma VM, a menos que necessário, como quando [atribuição de IP de vários endereços para uma VM do Windows](virtual-network-multiple-ip-addresses-powershell.md). Se definir manualmente o endereço IP privado no sistema operativo, certifique-se de que é o mesmo endereço como o endereço IP privado atribuído para o Azure [interface de rede](virtual-network-network-interface-addresses.md#change-ip-address-settings), ou pode perder a conectividade para a máquina virtual. Saiba mais sobre [endereço IP privado](virtual-network-network-interface-addresses.md#private) definições.

## <a name="next-steps"></a>Passos Seguintes

Qualquer tráfego de rede pode fluir de e para a VM que criou neste artigo. Pode definir regras de segurança de entrada e saída dentro de um grupo de segurança de rede que limitam o tráfego que pode fluir de e para a interface de rede, a sub-rede ou ambos. Para saber mais sobre os grupos de segurança de rede, veja [descrição geral de grupo de segurança de rede](security-overview.md).