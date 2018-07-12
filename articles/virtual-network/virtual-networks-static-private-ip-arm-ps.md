---
title: Configurar endereços IP privados para as VMs - Azure PowerShell | Documentos da Microsoft
description: Saiba como configurar os endereços IP privados para as máquinas virtuais com o PowerShell.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: d5f18929-15e3-40a2-9ee3-8188bc248ed8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b0e8153f1d0cecd4efe66dc7cce64addd6ed62aa
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38307675"
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-powershell"></a>Configurar endereços IP privados para uma máquina virtual com o PowerShell

[!INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

O Azure tem dois modelos de implementação: a implementação do Azure Resource Manager e a implementação clássica. A Microsoft recomenda a criação de recursos com o modelo de implementação Resource Manager. Para saber mais sobre as diferenças entre os dois modelos, veja o artigo [Understand Azure deployment models (Compreender os modelos de implementação do Azure)](../azure-resource-manager/resource-manager-deployment-model.md). Este artigo abrange o modelo de implementação do Resource Manager. Também pode [gerir o endereço IP privado estático no modelo de implementação clássica](virtual-networks-static-private-ip-classic-ps.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

O exemplo de PowerShell comandos abaixo esperam um ambiente simples já criado com base no cenário acima. Se quiser executar os comandos à medida que são apresentadas neste documento, primeiro crie o ambiente de teste descrito em [criar uma rede virtual](quick-create-powershell.md).

## <a name="create-a-vm-with-a-static-private-ip-address"></a>Criar uma VM com um endereço IP privado estático
Para criar uma VM com o nome *DNS01* no *front-end* sub-rede de uma VNet com o nome *TestVNet* com um IP privado estático da *192.168.1.101*, siga os passos abaixo:

1. Defina variáveis para a conta de armazenamento, localização, grupo de recursos e as credenciais a utilizar. Terá de introduzir um nome de utilizador e palavra-passe para a VM. O grupo de recursos e conta de armazenamento tem de existir.

    ```powershell
    $stName  = "vnetstorage"
    $locName = "Central US"
    $rgName  = "TestRG"
    $cred    = Get-Credential -Message "Type the name and password of the local administrator account."
    ```

2. Obter a rede virtual e sub-rede que pretende criar a VM na.

    ```powershell
    $vnet   = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    $subnet = $vnet.Subnets[0].Id
    ```

3. Se necessário, crie um endereço IP público para aceder à VM a partir da Internet.

    ```powershell
    $pip = New-AzureRmPublicIpAddress -Name TestPIP -ResourceGroupName $rgName `
    -Location $locName -AllocationMethod Dynamic
    ```

4. Crie um NIC com o endereço IP privado estático que pretende atribuir à VM. Certifique-se o IP do intervalo de sub-rede que está a adicionar a VM. Essa é a etapa principal deste artigo, onde definir o IP privado estático.

    ```powershell
    $nic = New-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName $rgName `
    -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id `
    -PrivateIpAddress 192.168.1.101
    ```

5. Crie a VM com a NIC:

    ```powershell
    $vm = New-AzureRmVMConfig -VMName DNS01 -VMSize "Standard_A1"
    $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName DNS01 `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/WindowsVMosDisk.vhd"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name "windowsvmosdisk" -VhdUri $osDiskUri `
    -CreateOption fromImage
    New-AzureRmVM -ResourceGroupName $rgName -Location $locName -VM $vm 
    ```

É recomendável que não atribuir estaticamente IP privado atribuído à máquina virtual do Azure no sistema operativo de uma VM, a menos que necessário, como quando [atribuição de IP de vários endereços para uma VM do Windows](virtual-network-multiple-ip-addresses-powershell.md). Se definir manualmente o endereço IP privado no sistema operativo, certifique-se de que é o mesmo endereço como o endereço IP privado atribuído para o Azure [interface de rede](virtual-network-network-interface-addresses.md#change-ip-address-settings), ou pode perder a conectividade para a máquina virtual. Saiba mais sobre [endereço IP privado](virtual-network-network-interface-addresses.md#private) definições. Nunca manualmente deve atribuir o endereço IP público atribuído a uma máquina virtual do Azure no sistema de operativo da máquina virtual.

## <a name="retrieve-static-private-ip-address-information-for-a-network-interface"></a>Obter estáticas informações de endereço IP privadas para uma interface de rede
Para ver as informações de endereço IP privadas estáticas para a VM criada com o script acima, execute o seguinte comando do PowerShell e observe os valores para *PrivateIpAddress* e *PrivateIpAllocationMethod*:

```powershell
Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
```

Resultado esperado:

    Name                 : TestNIC
    ResourceGroupName    : TestRG
    Location             : centralus
    Id                   : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    Etag                 : W/"[Id]"
    ProvisioningState    : Succeeded
    Tags                 : 
    VirtualMachine       : {
                             "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01"
                           }
    IpConfigurations     : [
                             {
                               "Name": "ipconfig1",
                               "Etag": "W/\"[Id]\"",
                               "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                               "PrivateIpAddress": "192.168.1.101",
                               "PrivateIpAllocationMethod": "Static",
                               "Subnet": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                               },
                               "PublicIpAddress": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP"
                               },
                               "LoadBalancerBackendAddressPools": [],
                               "LoadBalancerInboundNatRules": [],
                               "ProvisioningState": "Succeeded"
                             }
                           ]
    DnsSettings          : {
                             "DnsServers": [],
                             "AppliedDnsServers": [],
                             "InternalDnsNameLabel": null,
                             "InternalFqdn": null
                           }
    EnableIPForwarding   : False
    NetworkSecurityGroup : null
    Primary              : True

## <a name="remove-a-static-private-ip-address-from-a-network-interface"></a>Remover um endereço IP privado estático a partir de uma interface de rede
Para remover o endereço IP privado estático adicionado à VM no script acima, execute os seguintes comandos do PowerShell:

```powershell
$nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Dynamic"
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Resultado esperado:

    Name                 : TestNIC
    ResourceGroupName    : TestRG
    Location             : centralus
    Id                   : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    Etag                 : W/"[Id]"
    ProvisioningState    : Succeeded
    Tags                 : 
    VirtualMachine       : {
                             "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/WindowsVM"
                           }
    IpConfigurations     : [
                             {
                               "Name": "ipconfig1",
                               "Etag": "W/\"[Id]\"",
                               "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                               "PrivateIpAddress": "192.168.1.101",
                               "PrivateIpAllocationMethod": "Dynamic",
                               "Subnet": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
                               },
                               "PublicIpAddress": {
                                 "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP"
                               },
                               "LoadBalancerBackendAddressPools": [],
                               "LoadBalancerInboundNatRules": [],
                               "ProvisioningState": "Succeeded"
                             }
                           ]
    DnsSettings          : {
                             "DnsServers": [],
                             "AppliedDnsServers": [],
                             "InternalDnsNameLabel": null,
                             "InternalFqdn": null
                           }
    EnableIPForwarding   : False
    NetworkSecurityGroup : null
    Primary              : True

## <a name="add-a-static-private-ip-address-to-a-network-interface"></a>Adicionar um endereço IP privado estático a uma interface de rede
Para adicionar um endereço IP privado estático para a VM criada utilizando o script acima, execute os seguintes comandos:

```powershell
$nic=Get-AzureRmNetworkInterface -Name TestNIC -ResourceGroupName TestRG
$nic.IpConfigurations[0].PrivateIpAllocationMethod = "Static"
$nic.IpConfigurations[0].PrivateIpAddress = "192.168.1.101"
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

É recomendável que não atribuir estaticamente IP privado atribuído à máquina virtual do Azure no sistema operativo de uma VM, a menos que necessário, como quando [atribuição de IP de vários endereços para uma VM do Windows](virtual-network-multiple-ip-addresses-powershell.md). Se definir manualmente o endereço IP privado no sistema operativo, certifique-se de que é o mesmo endereço como o endereço IP privado atribuído para o Azure [interface de rede](virtual-network-network-interface-addresses.md#change-ip-address-settings), ou pode perder a conectividade para a máquina virtual. Saiba mais sobre [endereço IP privado](virtual-network-network-interface-addresses.md#private) definições. Nunca manualmente deve atribuir o endereço IP público atribuído a uma máquina virtual do Azure no sistema de operativo da máquina virtual.

## <a name="change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface"></a>Alterar o método de alocação para um endereço IP privado atribuído a uma interface de rede

Um endereço IP privado é atribuído a um NIC com o método de alocação estático ou dinâmico. Endereços IP dinâmicos, podem alterar depois de iniciar uma VM que estava anteriormente no estado parado (desalocada). Isso pode causar potencialmente problemas se a VM está a alojar um serviço que requer o mesmo endereço IP, mesmo após a reinicialização de um estado parado (desalocada). Endereços IP estáticos são mantidos até que a VM é eliminada. Para alterar o método de alocação de um endereço IP, execute o seguinte script, que altera o método de alocação de dinâmico para estático. Se o método de alocação para o endereço IP privado atual for estático, altere *estático* ao *dinâmico* antes de executar o script.

```powershell
$RG = "TestRG"
$NIC_name = "testnic1"

$nic = Get-AzureRmNetworkInterface -ResourceGroupName $RG -Name $NIC_name
$nic.IpConfigurations[0].PrivateIpAllocationMethod = 'Static'
Set-AzureRmNetworkInterface -NetworkInterface $nic 
$IP = $nic.IpConfigurations[0].PrivateIpAddress

Write-Host "The allocation method is now set to"$nic.IpConfigurations[0].PrivateIpAllocationMethod"for the IP address" $IP"." -NoNewline
```

Se não souber o nome do NIC, pode ver uma lista das NICs dentro de um grupo de recursos, introduzindo o seguinte comando:

```powershell
Get-AzureRmNetworkInterface -ResourceGroupName $RG | Where-Object {$_.ProvisioningState -eq 'Succeeded'} 
```

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre como gerir [definições do endereço IP](virtual-network-network-interface-addresses.md).