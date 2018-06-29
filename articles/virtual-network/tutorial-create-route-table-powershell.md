---
title: Encaminhar o tráfego de rede do Azure PowerShell | Microsoft Docs
description: Neste artigo, saiba como encaminhar o tráfego de rede com uma tabela de rota com o PowerShell.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 2aca1de567dbd4d37daf7f9dd7c407b669396a47
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "31603713"
---
# <a name="route-network-traffic-with-a-route-table-using-powershell"></a>Encaminhar o tráfego de rede com uma tabela de rota com o PowerShell

O Azure encaminha automaticamente o tráfego entre todas as sub-redes numa rede virtual, por predefinição. Pode criar as sua próprias rotas para substituir o encaminhamento predefinido do Azure. A capacidade de criar rotas personalizadas é útil se, por exemplo, pretender encaminhar o tráfego entre sub-redes através de uma aplicação virtual de rede (NVA). Neste artigo, vai aprender a:

* Criar uma tabela de rotas
* Criar uma rota
* Criar uma rede virtual com várias sub-redes
* Associar uma tabela de rotas a uma sub-rede
* Criar uma NVA que encaminha o tráfego
* Implementar máquinas virtuais (VMs) em sub-redes diferentes
* Encaminhar o tráfego de uma sub-rede para outra através de uma NVA

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este artigo requer a versão 5.4.1 ou posterior do módulo Azure PowerShell. Execute `Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Connect-AzureRmAccount` para criar uma ligação com o Azure. 

## <a name="create-a-route-table"></a>Criar uma tabela de rotas

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

## <a name="associate-a-route-table-to-a-subnet"></a>Associar uma tabela de rotas a uma sub-rede

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

Uma NVA é uma VM que executa uma função de rede, como encaminhamento, proteção com firewalls ou otimização de WAN.

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

A criação da VM demora alguns minutos. Não continue com o passo seguinte até que a VM é criada e Azure devolve resultado para o PowerShell.

## <a name="route-traffic-through-an-nva"></a>Encaminhar o tráfego através de uma NVA

Utilize [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) para devolver o endereço IP público do *myVmPrivate* VM. O exemplo seguinte devolve o endereço IP público do *myVmPrivate* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Utilize o seguinte comando para criar uma sessão de ambiente de trabalho remota com o *myVmPrivate* VM a partir do seu computador local. Substitua `<publicIpAddress>` pelo endereço IP devolvido no comando anterior.

```
mstsc /v:<publicIpAddress>
```

Abra o ficheiro RDP transferido. Se lhe for pedido, selecione **Ligar**.

Introduza o nome de utilizador e a palavra-passe que especificou ao criar a VM (poderá ter de selecionar **Mais opções** e **Utilizar uma conta diferente** para especificar as credenciais que introduziu quando criou a VM) e, em seguida, selecione **OK**. Poderá receber um aviso de certificado durante o processo de início de sessão. Selecione **Sim** para continuar a ligação. 

Num passo posterior, o comando tracert.exe é utilizado para testar o encaminhamento. Tracert utiliza o controlo de mensagem de ICMP (Internet Protocol), que é negado através da Firewall do Windows. Introduza o seguinte comando do PowerShell na VM *myVmPrivate* para ativar o ICMP através da Firewall do Windows:

```powershell
New-NetFirewallRule -DisplayName "Allow ICMPv4-In" -Protocol ICMPv4
```

Apesar de rota de rastreio é utilizada para testar o encaminhamento neste artigo, permitir ICMP através da Firewall do Windows para implementações de produção não é recomendada.

Ativou o reencaminhamento de IP dentro do Azure na interface de rede da VM em [Ativar o encaminhamento de IP](#enable-ip-forwarding). Dentro da VM, é necessário que o sistema operativo, ou uma aplicação em execução na mesma, consiga também reencaminhar o tráfego de rede. Ativar o reencaminhamento IP no sistema operativo do *myVmNva*.

Numa linha de comandos no *myVmPrivate* VM, ambiente de trabalho remoto para o *myVmNva*:

``` 
mstsc /v:myvmnva
```
    
Para ativar o reencaminhamento de IP no sistema operativo, introduza o seguinte comando no PowerShell da VM *myVmNva*:

```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
```
    
Reinicie a VM *myVmNva*, o que também desliga a sessão de ambiente de trabalho remoto.

Ainda ligado à VM *myVmPrivate*, crie uma sessão de ambiente de trabalho remoto para a VM *myVmPublic*, após a VM *myVmNva* ser reiniciada:

``` 
mstsc /v:myVmPublic
```
    
Introduza o seguinte comando do PowerShell na VM *myVmPublic* para ativar o ICMP através da Firewall do Windows:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Para testar o encaminhamento do tráfego de rede para a VM *myVmPrivate* a partir da VM *myVmPublic*, introduza o seguinte comando do PowerShell na VM *myVmPublic*:

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
      
Pode ver que o primeiro salto é 10.0.2.4, que é o endereço IP privado da NVA. O segundo salto é 10.0.1.4, o endereço IP privado da VM *myVmPrivate*. A rota adicionada à tabela de rotas *myRouteTablePublic* e associada à sub-rede *Pública* fez com que o Azure encaminhe o tráfego através da NVA, em vez de diretamente para a sub-rede *Privada*.

Feche a sessão de ambiente de trabalho remoto para a VM *myVmPublic*, o que mantém a ligação à VM *myVmPrivate* ativa.

Para testar o encaminhamento do tráfego de rede para a VM *myVmPublic* a partir da VM *myVmPrivate*, introduza o seguinte comando numa linha de comandos na VM *myVmPrivate*:

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

Pode ver que o tráfego é encaminhado diretamente da VM *myVmPrivate* para a VM *myVmPublic*. Por predefinição, o Azure encaminha o tráfego diretamente entre sub-redes.

Feche a sessão de ambiente de trabalho remoto para a VM *myVmPrivate*.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário utilizar [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o grupo de recursos e todos os recursos nele contidos.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, criou uma tabela de rota e associados-la a uma sub-rede. Criar uma aplicação virtual de rede simples que encaminhados tráfego de sub-rede público para uma sub-rede privada. Implementar uma variedade de aplicações virtuais de rede previamente configuradas que efetuam funções de rede, tais como a firewall e a otimização de WAN do [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Para saber mais sobre o encaminhamento, veja [Descrição geral do encaminhamento](virtual-networks-udr-overview.md) e [Manage a route table](manage-route-table.md) (Gerir uma tabela de rotas).

Embora possa implementar muitos recursos do Azure dentro de uma rede virtual, não é possível implementar recursos de alguns serviços PaaS do Azure nas redes virtuais. Contudo, pode restringir o acesso aos recursos de alguns serviços PaaS do Azure ao tráfego apenas a partir de uma sub-rede de rede virtual. Para saber como, consulte [restringir o acesso de rede para recursos de PaaS](tutorial-restrict-network-access-to-resources-powershell.md).
