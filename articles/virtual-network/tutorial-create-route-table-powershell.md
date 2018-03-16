---
title: "Encaminhar o tráfego de rede - PowerShell | Microsoft Docs"
description: "Saiba como encaminhar o tráfego de rede com uma tabela de rota com o PowerShell."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 49c7b6158beee9d47ecd224e6a0750310d2b68c0
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="route-network-traffic-with-a-route-table-using-powershell"></a>Encaminhar o tráfego de rede com uma tabela de rota com o PowerShell

Azure automaticamente as rotas de tráfego entre todas as sub-redes na rede virtual, por predefinição. Pode criar as seus próprios rotas para substituir do Azure encaminhamento predefinido. A capacidade de criar rotas personalizadas é útil se, por exemplo, pretender encaminhar o tráfego entre sub-redes através de uma aplicação virtual de rede (NVA). Neste artigo, saiba como:

> [!div class="checklist"]
> * Criar uma tabela de rota
> * Criar uma rota
> * Criar uma rede virtual com várias sub-redes
> * Associar uma tabela de rota para uma sub-rede
> * Criar uma NVA que encaminha o tráfego
> * Implementar máquinas virtuais (VM) em sub-redes diferentes
> * Encaminhar o tráfego de uma sub-rede para outra através de uma NVA

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este artigo requer o Azure PowerShell versão do módulo 5.4.1 ou posterior. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure. 

## <a name="create-a-route-table"></a>Criar uma tabela de rota

Antes de poder criar uma tabela de rota, crie um grupo de recursos com [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). O exemplo seguinte cria um grupo de recursos denominado *myResourceGroup* para todos os recursos criados neste artigo. 

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Criar uma tabela de rotas com [New-AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable). O exemplo seguinte cria uma tabela de rota chamada *myRouteTablePublic*.

```azurepowershell-interactive
$routeTablePublic = New-AzureRmRouteTable `
  -Name 'myRouteTablePublic' `
  -ResourceGroupName myResourceGroup `
  -location EastUS
```

## <a name="create-a-route"></a>Criar uma rota

Criar uma rota obtendo o objeto de tabela de rotas com [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable), criar uma rota com [adicionar AzureRmRouteConfig](/powershell/module/azurerm.network/add-azurermrouteconfig), em seguida, escrever a configuração de rota para a tabela de rotas com [Conjunto AzureRmRouteTable](/powershell/module/azurerm.network/set-azurermroutetable). 

```azurepowershell-interactive
Get-AzureRmRouteTable `
  -ResourceGroupName "myResourceGroup" `
  -Name "myRouteTablePublic" `
  | Add-AzureRmRouteConfig `
  -Name "ToPrivateSubnet" `
  -AddressPrefix 10.0.1.0/24 `
  -NextHopType "VirtualAppliance" `
  -NextHopIpAddress 10.0.2.4 `
 | Set-AzureRmRouteTable
```

## <a name="associate-a-route-table-to-a-subnet"></a>Associar uma tabela de rota para uma sub-rede

Antes de poder associar uma tabela de rota para uma sub-rede, terá de criar uma rede virtual e a sub-rede. Crie uma rede virtual com [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). O exemplo seguinte cria uma rede virtual denominada *myVirtualNetwork* com o prefixo de endereço *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Criar três sub-redes através da criação de três configurações de sub-rede com [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). O exemplo seguinte cria três configurações de sub-rede para *pública*, *privada*, e *DMZ* sub-redes:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigPrivate = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigDmz = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name DMZ `
  -AddressPrefix 10.0.2.0/24 `
  -VirtualNetwork $virtualNetwork
```

Escrever as configurações de sub-rede da rede virtual com [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), que cria as sub-redes na rede virtual:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

Associar o *myRouteTablePublic* tabela de rotas para o *pública* sub-rede com [conjunto AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) e, em seguida, escrever a configuração de sub-rede para o rede virtual com [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $virtualNetwork `
  -Name 'Public' `
  -AddressPrefix 10.0.0.0/24 `
  -RouteTable $routeTablePublic | `
Set-AzureRmVirtualNetwork
```

## <a name="create-an-nva"></a>Criar uma NVA

Uma NVA, é uma VM que executa uma função de rede, tais como o encaminhamento de mensagens em fila, firewalling ou otimização de WAN.

Antes de criar uma VM, crie uma interface de rede.

### <a name="create-a-network-interface"></a>Criar uma interface de rede

Antes de criar uma interface de rede, tem de obter virtual Id com rede [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork), em seguida, o Id de sub-rede com [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig). Criar uma interface de rede com [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) no *DMZ* sub-rede com o reencaminhamento IP está ativado:

```azurepowershell-interactive
# Retrieve the virtual network object into a variable.
$virtualNetwork=Get-AzureRmVirtualNetwork `
  -Name myVirtualNetwork `
  -ResourceGroupName myResourceGroup

# Retrieve the subnet configuration into a variable.
$subnetConfigDmz = Get-AzureRmVirtualNetworkSubnetConfig `
  -Name DMZ `
  -VirtualNetwork $virtualNetwork

# Create the network interface.
$nic = New-AzureRmNetworkInterface `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name 'myVmNva' `
  -SubnetId $subnetConfigDmz.Id `
  -EnableIPForwarding
```

### <a name="create-a-vm"></a>Criar uma VM

Para criar uma VM e anexar uma interface de rede existente, primeiro tem de criar uma configuração de VM com [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). A configuração inclui a interface de rede que criou no passo anterior. Quando lhe for pedido um nome de utilizador e palavra-passe, selecione o nome de utilizador e palavra-passe que pretende iniciar sessão na VM com o. 

```azurepowershell-interactive
# Create a credential object.
$cred = Get-Credential -Message "Enter a username and password for the VM."

# Create a VM configuration.
$vmConfig = New-AzureRmVMConfig `
  -VMName 'myVmNva' `
  -VMSize Standard_DS2 | `
  Set-AzureRmVMOperatingSystem -Windows `
    -ComputerName 'myVmNva' `
    -Credential $cred | `
  Set-AzureRmVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
  Add-AzureRmVMNetworkInterface -Id $nic.Id
```

Criar a VM com a configuração de VM com o [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). O exemplo seguinte cria uma VM chamada *myVmNva*. 

```azurepowershell-interactive
$vmNva = New-AzureRmVM `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -VM $vmConfig `
  -AsJob
```

O `-AsJob` opção cria a VM em segundo plano, para poder continuar para o passo seguinte.

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Criar duas VMs na rede virtual para que possa validar que o tráfego de entrada a *pública* sub-rede é encaminhada para o *privada* sub-rede através da aplicação virtual de rede num passo posterior. 

Criar uma VM com o *pública* sub-rede com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). O exemplo seguinte cria uma VM chamada *myVmPublic* no *pública* sub-rede do *myVirtualNetwork* rede virtual. 

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Public" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPublic" `
  -AsJob
```

Criar uma VM com o *privada* sub-rede.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Private" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPrivate"
```

A VM demora alguns minutos a criar. Não continue com o passo seguinte até que a VM é criada e Azure devolve resultado para o PowerShell.

## <a name="route-traffic-through-an-nva"></a>Encaminhar o tráfego através de uma NVA

Utilize [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) para devolver o endereço IP público do *myVmPrivate* VM. O exemplo seguinte devolve o endereço IP público do *myVmPrivate* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Utilize o seguinte comando para criar uma sessão de ambiente de trabalho remota com o *myVmPrivate* VM a partir do seu computador local. Substitua `<publicIpAddress>` com o endereço IP devolvido do comando anterior.

```
mstsc /v:<publicIpAddress>
```

Abra o ficheiro RDP transferido. Se lhe for pedido, selecione **Connect**.

Introduza o nome de utilizador e palavra-passe que especificou ao criar a VM (poderá ter de selecionar **mais opções**, em seguida, **utilizar uma conta diferente**, para especificar as credenciais que introduziu quando criou a VM), em seguida, selecione **OK**. Poderá receber um aviso de certificado durante o processo de início de sessão. Selecione **Sim** para continuar com a ligação. 

Num passo posterior, o comando tracert.exe é utilizado para testar o encaminhamento. Tracert utiliza o controlo de mensagem de ICMP (Internet Protocol), que é negado através da Firewall do Windows. Ative o ICMP através da firewall do Windows, introduzindo o seguinte comando do PowerShell:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Embora tracert é utilizado para testar o encaminhamento neste artigo, permitir ICMP através da Firewall do Windows para implementações de produção não é recomendada.

Ativar o reencaminhamento IP no sistema operativo do *myVmNva* concluindo os seguintes passos do *myVmPrivate* VM:

Ambiente de trabalho remoto para o *myVmNva* VM com o seguinte comando do PowerShell:

``` 
mstsc /v:myvmnva
```
    
Para ativar o IP de reencaminhamento no sistema operativo, introduza o seguinte comando do PowerShell:

```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
```
    
Reinicie a VM, que também desliga a sessão de ambiente de trabalho remota.

Enquanto ainda ligada para o *myVmPrivate* VM, após o *myVmNva* VM reinícios, criar uma sessão de ambiente de trabalho remoto para o *myVmPublic* VM com o seguinte comando:

``` 
mstsc /v:myVmPublic
```
    
Ative o ICMP através da firewall do Windows, introduzindo o seguinte comando do PowerShell:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Para testar o encaminhamento de tráfego de rede para o *myVmPrivate* VM a partir de *myVmPublic* VM, introduza o seguinte comando do PowerShell:

```
tracert myVmPrivate
```

A resposta é semelhante ao seguinte exemplo:
    
```
Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
over a maximum of 30 hops:
        
1    <1 ms     *        1 ms  10.0.2.4
2     1 ms     1 ms     1 ms  10.0.1.4
        
Trace complete.
```
      
Pode ver que o primeiro salto é 10.0.2.4, que é o endereço IP privado a rede aplicação virtual. O segundo hop é 10.0.1.4, o endereço IP privado do *myVmPrivate* VM. A rota à *myRouteTablePublic* tabela de rotas e associado ao *pública* sub-rede causado do Azure encaminhar o tráfego através de NVA, em vez de diretamente para o *privada* sub-rede.

Fechar a sessão de ambiente de trabalho remoto para o *myVmPublic* VM, deixando-ainda ligado para o *myVmPrivate* VM.
Para testar o encaminhamento de tráfego de rede para o *myVmPublic* VM a partir de *myVmPrivate* VM, introduza o seguinte comando numa linha de comandos:

```
tracert myVmPublic
```

A resposta é semelhante ao seguinte exemplo:

```
Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
over a maximum of 30 hops:
    
1     1 ms     1 ms     1 ms  10.0.0.4
    
Trace complete.
```

Pode ver que o tráfego é encaminhado diretamente a partir de *myVmPrivate* VM para o *myVmPublic* VM. Por predefinição, o tráfego de rotas do Azure diretamente entre sub-redes.

Fechar a sessão de ambiente de trabalho remoto para o *myVmPrivate* VM.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário utilizar [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o grupo de recursos e todos os recursos nele contidos.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, criou uma tabela de rota e associados-la a uma sub-rede. Criar uma aplicação virtual de rede simples que encaminhados tráfego de sub-rede público para uma sub-rede privada. Implementar uma variedade de aplicações virtuais de rede previamente configuradas que efetuam funções de rede, tais como a firewall e a otimização de WAN do [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Antes de implementar as tabelas de rotas para utilização em produção, recomenda-se que lhe exaustivamente familiarizar-se com [encaminhamento no Azure](virtual-networks-udr-overview.md), [tabelas de rota de gerir](manage-route-table.md), e [Azure limita](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

Enquanto pode implementar vários recursos do Azure dentro de uma rede virtual, os recursos para alguns serviços do Azure PaaS não não possível implementar numa rede virtual. Pode ainda restringir o acesso aos recursos de alguns serviços do Azure PaaS para o tráfego apenas a partir de uma sub-rede de rede virtual apesar. Avançar para o próximo tutorial para saber como restringir o acesso de rede para recursos do Azure PaaS.

> [!div class="nextstepaction"]
> [Restringir o acesso de rede para recursos de PaaS](virtual-network-service-endpoints-configure.md#azure-powershell)