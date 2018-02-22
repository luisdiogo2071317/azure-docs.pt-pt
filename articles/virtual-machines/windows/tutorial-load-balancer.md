---
title: "Como fazer o balanceamento de carga das máquinas virtuais do Windows no Azure | Microsoft Docs"
description: "Saiba como utilizar o balanceador de carga do Azure para criar uma aplicação de elevada disponibilidade e segura entre três VMs do Windows"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/09/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: f0e154d0ac917d2ef2799431a72969a96415e0c0
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2018
---
# <a name="how-to-load-balance-windows-virtual-machines-in-azure-to-create-a-highly-available-application"></a>Como fazer o balanceamento de carga das máquinas virtuais do Windows no Azure para criar uma aplicação de elevada disponibilidade
O balanceamento de carga oferece um nível mais elevado de disponibilidade ao propagar os pedidos recebidos por várias máquinas virtuais. Neste tutorial, vai conhecer os diferentes componentes do balanceador de carga do Azure que distribuem o tráfego e oferecem elevada disponibilidade. Saiba como:

> [!div class="checklist"]
> * Criar um balanceador de carga do Azure
> * Criar uma sonda de estado de funcionamento do balanceador de carga
> * Criar regras de tráfego do balanceador de carga
> * Utilizar a extensão de Script Personalizado para criar um site do IIS básico
> * Criar máquinas virtuais e anexar a um balanceador de carga
> * Ver um balanceador de carga em ação
> * Adicionar e remover VMs de um balanceador de carga

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer a versão 5.3 ou posterior do módulo Azure PowerShell. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure. 


## <a name="azure-load-balancer-overview"></a>Descrição geral do balanceador de carga do Azure
Um balanceador de carga do Azure é um balanceador de carga de Camada 4 (TCP, UDP) que fornece elevada disponibilidade ao distribuir o tráfego de entrada entre VMs em bom estado de funcionamento. Uma sonda de estado de funcionamento do balanceador de carga monitoriza uma porta especificada em cada VM e apenas distribui o tráfego para uma VM operacional.

Pode definir uma configuração de IP de front-end com um ou mais endereços IP públicos. Esta configuração de IP de front-end permite ao balanceador de carga e às aplicações estarem acessíveis através da Internet. 

As máquinas virtuais ligam a um balanceador de carga através da respetiva placa de interface de rede virtual (NIC). Para distribuir o tráfego pelas VMs, um conjunto de endereços de back-end contém os endereços IP das placas virtuais (NICs) ligadas ao balanceador de carga.

Para controlar o fluxo de tráfego, pode definir regras de balanceador de carga para portas específicas e protocolos que mapeiam para as suas VMs.


## <a name="create-azure-load-balancer"></a>Criar um balanceador de carga do Azure
Esta secção descreve como pode criar e configurar cada componente do balanceador de carga. Para poder criar o seu balanceador de carga, crie primeiro um grupo de recursos com [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). O exemplo seguinte cria um grupo de recursos designado *myResourceGroupLoadBalancer* na localização *EastUS*:

```azurepowershell-interactive
New-AzureRmResourceGroup `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Location "EastUS"
```

### <a name="create-a-public-ip-address"></a>Crie um endereço IP público
Para aceder à sua aplicação na Internet, precisa de um endereço IP público para o balanceador de carga. Crie um endereço IP público com [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). O exemplo seguinte cria um endereço IP público designado *myPublicIP* no grupo de recursos *myResourceGroupLoadBalancer*:

```azurepowershell-interactive
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Location "EastUS" `
  -AllocationMethod "Static" `
  -Name "myPublicIP"
```

### <a name="create-a-load-balancer"></a>Criar um balanceador de carga
Crie um conjunto IP de front-end com [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig). O exemplo seguinte cria um conjunto IP de front-end designado *myFrontEndPool* e anexa o endereço *myPublicIP*: 

```azurepowershell-interactive
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name "myFrontEndPool" `
  -PublicIpAddress $publicIP
```

Crie um conjunto de endereços de back-end com [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig). As VMs são anexadas a este conjunto de back-end nos restantes passos. O exemplo seguinte cria um conjunto de endereços de back-end designado *myBackEndPool*:

```azurepowershell-interactive
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "myBackEndPool"
```

Agora, cria o balanceador de carga com [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer). O exemplo seguinte cria um balanceador de carga designado *myLoadBalancer* com os conjuntos de IP de front-end e back-end criados nos passos anteriores:

```azurepowershell-interactive
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Name "myLoadBalancer" `
  -Location "EastUS" `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-a-health-probe"></a>Criar uma sonda de estado de funcionamento
Para permitir ao balanceador de carga monitorizar o estado da aplicação, pode utilizar uma sonda de estado de funcionamento. A sonda de estado de funcionamento adiciona ou remove dinamicamente VMs da rotação do balanceador de carga com base na respetiva resposta às verificações de estado de funcionamento. Por predefinição, uma VM é removida da distribuição do balanceador de carga após duas falhas consecutivas em intervalos de 15 segundos. Pode criar uma sonda de estado de funcionamento com base num protocolo ou numa página de verificação de estado de funcionamento específica da aplicação. 

O exemplo seguinte cria uma sonda TCP. Também pode criar sondas HTTP personalizadas para obter verificações de estado de funcionamento mais detalhadas. Quando utilizar uma sonda HTTP personalizada, tem de criar a página de verificação de estado de funcionamento, tal como *healthcheck.aspx*. A sonda tem de devolver uma resposta **HTTP 200 OK** para o balanceador de carga manter o anfitrião na rotação.

Para criar uma sonda do estado de funcionamento TCP, utilize [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig). O exemplo seguinte cria uma sonda de estado de funcionamento designado *myHealthProbe* que monitoriza cada VM no *TCP* porta *80*:

```azurepowershell-interactive
Add-AzureRmLoadBalancerProbeConfig `
  -Name "myHealthProbe" `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

Para aplicar a sonda de estado de funcionamento, atualize o balanceador de carga com [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```azurepowershell-interactive
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

### <a name="create-a-load-balancer-rule"></a>Crie uma regra de balanceador de carga
É utilizada uma regra de balanceador de carga para definir a forma como o tráfego é distribuído pelas VMs. Pode definir a configuração de IP de front-end do tráfego de entrada e o conjunto de IPs de back-end para receber o tráfego, juntamente com a porta de origem e de destino necessárias. Para garantir que apenas as VMs em bom estado de funcionamento recebem o tráfego, também pode definir a sonda de estado de funcionamento a utilizar.

Crie uma regra de balanceador de carga com [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig). O exemplo seguinte cria uma regra de balanceador de carga designada *myLoadBalancerRule* e faz o balanceamento de carga do tráfego no *TCP* porta *80*:

```azurepowershell-interactive
$probe = Get-AzureRmLoadBalancerProbeConfig -LoadBalancer $lb -Name "myHealthProbe"

Add-AzureRmLoadBalancerRuleConfig `
  -Name "myLoadBalancerRule" `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80 `
  -Probe $probe
```

Atualize o balanceador de carga com [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```azurepowershell-interactive
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="configure-virtual-network"></a>Configurar uma rede virtual
Antes de implementar algumas VMs e testar o balanceador, crie os recursos de rede virtual. Para obter mais informações sobre redes virtuais, veja o tutorial [Gerir Redes Virtuais do Azure](tutorial-virtual-network.md).

### <a name="create-network-resources"></a>Criar recursos de rede
Crie uma rede virtual com [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). O exemplo seguinte cria uma rede virtual designada *myVnet* com *mySubnet*:

```azurepowershell-interactive
# Create subnet config
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 192.168.1.0/24

# Create the virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Location "EastUS" `
  -Name "myVnet" `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

As NICs virtuais são criadas com [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). O exemplo seguinte cria três NICs virtuais. (Uma NIC virtual para cada VM que criar para a aplicação nos passos seguintes). Pode criar NICs virtuais e VMs adicionais em qualquer altura e adicioná-las ao balanceador de carga:

```azurepowershell-interactive
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface `
     -ResourceGroupName "myResourceGroupLoadBalancer" `
     -Name myVM$i `
     -Location "EastUS" `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}
```


## <a name="create-virtual-machines"></a>Criar máquinas virtuais
Para melhorar a elevada disponibilidade da aplicação, coloque as VMs num conjunto de disponibilidade.

Crie um conjunto de disponibilidade com [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). O exemplo seguinte cria um conjunto de disponibilidade designado *myAvailabilitySet*:

```azurepowershell-interactive
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Name "myAvailabilitySet" `
  -Location "EastUS" `
  -Sku aligned `
  -PlatformFaultDomainCount 2 `
  -PlatformUpdateDomainCount 2
```

Defina um nome de utilizador e palavra-passe para as VMs com [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = Get-Credential
```

Agora, pode criar as VMs com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). O exemplo seguinte cria três VMs e os componentes de rede virtual necessários, caso ainda não existam:

```azurepowershell-interactive
for ($i=1; $i -le 3; $i++)
{
    New-AzureRmVm `
        -ResourceGroupName "myResourceGroupLoadBalancer" `
        -Name "myVM$i" `
        -Location "East US" `
        -VirtualNetworkName "myVnet" `
        -SubnetName "mySubnet" `
        -SecurityGroupName "myNetworkSecurityGroup" `
        -OpenPorts 80 `
        -AvailabilitySetName "myAvailabilitySet" `
        -Credential $cred `
        -AsJob
}
```

O parâmetro `-AsJob` cria a VM como uma tarefa em segundo plano, para que os pedidos do PowerShell voltem para si. Pode ver os detalhes das tarefas em segundo plano com o cmdlet `Job`. Demora alguns minutos para criar e configurar as três VMs.


### <a name="install-iis-with-custom-script-extension"></a>Instalar o IIS com a Extensão de Script Personalizado
Num tutorial anterior sobre [Como personalizar uma máquina virtual do Windows](tutorial-automate-vm-deployment.md), aprendeu a automatizar a personalização de VMs com a Extensão de Script Personalizado para Windows. Pode utilizar a mesma abordagem para instalar e configurar o IIS nas suas VMs.

Utilize [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) para instalar a Extensão de Script Personalizado. A extensão executa o `powershell Add-WindowsFeature Web-Server` para instalar o servidor Web IIS e, em seguida, atualiza a página *Default.htm* para mostrar o nome do anfitrião da VM:

```azurepowershell-interactive
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension `
     -ResourceGroupName "myResourceGroupLoadBalancer" `
     -ExtensionName "IIS" `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.8 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
     -Location EastUS
}
```

## <a name="test-load-balancer"></a>Testar o balanceador de carga
Obtenha o endereço IP público do seu balanceador de carga com [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). O exemplo seguinte obtém o endereço IP para *myPublicIP* criado anteriormente:

```azurepowershell-interactive
Get-AzureRmPublicIPAddress `
  -ResourceGroupName "myResourceGroupLoadBalancer" `
  -Name "myPublicIP" | select IpAddress
```

Em seguida, pode introduzir o endereço IP público num browser. O site é apresentado, incluindo o nome do anfitrião da VM para a qual o balanceador de carga distribuiu tráfego, como no seguinte exemplo:

![Site do IIS em execução](./media/tutorial-load-balancer/running-iis-website.png)

Para ver o balanceador de carga a distribuir tráfego pelas três VMs que estão a executar a aplicação, pode forçar a atualização do browser.


## <a name="add-and-remove-vms"></a>Adicionar e remover VMs
Pode ter de realizar a manutenção nas VMs que estão a executar a aplicação, como instalar atualizações do SO. Para lidar com maior volume de tráfego para a aplicação, pode ter de adicionar mais VMs. Esta secção mostra como adicionar ou remover uma VM do balanceador de carga.

### <a name="remove-a-vm-from-the-load-balancer"></a>Remover uma VM do balanceador de carga
Obtenha o cartão de interface de rede com [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) e, em seguida, defina a propriedade *LoadBalancerBackendAddressPools* da NIC virtual para *$null*. Por fim, atualize a NIC virtual.:

```azurepowershell-interactive
$nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName "myResourceGroupLoadBalancer" `
    -Name "myVM2"
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Para ver o balanceador de carga a distribuir tráfego pelas duas VMs restantes que estão a executar a aplicação, pode forçar a atualização do browser. Pode agora realizar a manutenção da VM, como instalar atualizações do SO ou reiniciar.

### <a name="add-a-vm-to-the-load-balancer"></a>Adicionar uma VM ao balanceador de carga
Após realizar a manutenção da VM, ou se precisar de expandir a capacidade, defina a propriedade *LoadBalancerBackendAddressPools* da NIC virtual para o *BackendAddressPool* de [ Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):

Obtenha o balanceador de carga:

```azurepowershell-interactive
$lb = Get-AzureRMLoadBalancer `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myLoadBalancer 
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou um balanceador de carga e anexou VMs ao mesmo. Aprendeu a:

> [!div class="checklist"]
> * Criar um balanceador de carga do Azure
> * Criar uma sonda de estado de funcionamento do balanceador de carga
> * Criar regras de tráfego do balanceador de carga
> * Utilizar a extensão de Script Personalizado para criar um site do IIS básico
> * Criar máquinas virtuais e anexar a um balanceador de carga
> * Ver um balanceador de carga em ação
> * Adicionar e remover VMs de um balanceador de carga

Avance para o tutorial seguinte para aprender a gerir a rede de VMs.

> [!div class="nextstepaction"]
> [Gerir VMs e redes virtuais](./tutorial-virtual-network.md)
