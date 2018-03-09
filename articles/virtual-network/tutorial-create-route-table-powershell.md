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
ms.date: 03/05/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: f91b143c75a82aa6760796770b3ae4d0e4ec53dd
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="route-network-traffic-with-a-route-table-using-powershell"></a>Encaminhar o tráfego de rede com uma tabela de rota com o PowerShell

Azure automaticamente as rotas de tráfego entre todas as sub-redes na rede virtual, por predefinição. Pode criar as seus próprios rotas para substituir do Azure encaminhamento predefinido. A capacidade de criar rotas personalizadas é útil se, por exemplo, pretender encaminhar o tráfego entre sub-redes através de uma firewall. Neste artigo, saiba como:

> [!div class="checklist"]
> * Criar uma tabela de rota
> * Criar uma rota
> * Associar uma tabela de rota para uma sub-rede de rede virtual
> * Encaminhamento de teste
> * Resolver problemas de encaminhamento

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este artigo requer o Azure PowerShell versão do módulo 5.4.1 ou posterior. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure. 

## <a name="create-a-route-table"></a>Criar uma tabela de rota

Tráfego de rotas do Azure entre todas as sub-redes na rede virtual, por predefinição. Para saber mais sobre as rotas predefinidas do Azure, consulte o artigo [rotas de sistema](virtual-networks-udr-overview.md). Substituir predefinição do Azure encaminhamento, pode criar uma tabela de rota que contém rotas e associar a tabela de rotas para uma sub-rede de rede virtual.

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

Uma tabela de rota contém zero ou mais rotas. Criar uma rota obtendo o objeto de tabela de rotas com [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable), criar uma rota com [adicionar AzureRmRouteConfig](/powershell/module/azurerm.network/add-azurermrouteconfig), em seguida, escrever a configuração de rota para a tabela de rotas com [Conjunto AzureRmRouteTable](/powershell/module/azurerm.network/set-azurermroutetable). 

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

A rota irá direcionar todo o tráfego destinado ao prefixo do endereço 10.0.1.0/24 através de uma aplicação virtual de rede com o endereço IP 10.0.2.4. O dispositivo de rede virtual e a sub-rede com o prefixo de endereço especificado são criados em passos posteriores. A rota substitui a predefinição do Azure encaminhamento, que encaminha o tráfego entre sub-redes diretamente. Cada rota especifica um tipo de salto seguinte. O tipo de próximo salto dá instruções ao Azure como encaminhar o tráfego. Neste exemplo, o tipo de próximo salto é *VirtualAppliance*. Para saber mais sobre os tipos de salto seguintes disponíveis no Azure e, quando a utilizá-los, consulte [próximo salto tipos](virtual-networks-udr-overview.md#custom-routes).

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

Os prefixos de endereço tem de estar dentro do prefixo de endereço definido para a rede virtual. Os prefixos de endereço sub-rede não podem sobrepor-se entre si.

Escrever as configurações de sub-rede da rede virtual com [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), que cria as sub-redes na rede virtual:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

Pode associar uma tabela de rota para zero ou mais sub-redes. Uma sub-rede pode ter zero ou uma tabela de rotas associada ao mesmo. Tráfego de saída de uma sub-rede é encaminhado com base nas rotas predefinidas do Azure e as rotas personalizadas que adicionou a uma tabela de rota que associar a uma sub-rede. Associar o *myRouteTablePublic* tabela de rotas para o *pública* sub-rede com [conjunto AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) e, em seguida, escrever a configuração de sub-rede para o rede virtual com [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).

```azurepowershell-interactive
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $virtualNetwork `
  -Name 'Public' `
  -AddressPrefix 10.0.0.0/24 `
  -RouteTable $routeTablePublic | `
Set-AzureRmVirtualNetwork
```

Antes de implementar as tabelas de rotas para utilização em produção, recomenda-se que lhe exaustivamente familiarizar-se com [encaminhamento no Azure](virtual-networks-udr-overview.md) e [limites do Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="test-routing"></a>Encaminhamento de teste

Para testar o encaminhamento, irá criar uma máquina virtual que funciona como a aplicação virtual de rede que a rota que criou no passo anterior encaminha através de. Depois de criar a aplicação virtual de rede, irá implementar uma máquina virtual para o *pública* e *privada* sub-redes. Em seguida, irá encaminhar o tráfego do *pública* sub-rede para o *privada* sub-rede através da aplicação virtual de rede.

### <a name="create-a-network-virtual-appliance"></a>Criar uma aplicação virtual de rede

Uma máquina virtual com uma aplicação de rede é frequentemente referida como uma aplicação virtual de rede. Os dispositivos de rede virtuais normalmente recebem tráfego de rede, efetuar qualquer ação, em seguida, reencaminhar ou remover o tráfego de rede com base na lógica configurada na aplicação de rede. 

#### <a name="create-a-network-interface"></a>Criar uma interface de rede

No passo anterior, criou uma rota que especificada uma aplicação virtual de rede como o tipo de salto seguinte. Uma máquina virtual com uma aplicação de rede é frequentemente referida como uma aplicação virtual de rede. Em ambientes de produção, a aplicação virtual de rede que implementa é frequentemente uma máquina virtual previamente configurada. Várias aplicações virtuais de rede estão disponíveis a partir de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1). Neste artigo, é criada uma máquina virtual básica.

Uma máquina virtual tem um ou mais interfaces de rede ligadas ao mesmo, que permitem a máquina virtual para comunicar com a rede. Para uma interface de rede conseguir encaminhar o tráfego de rede enviado, que não é destinado a entidades para o seu próprio endereço IP, o reencaminhamento IP tem de estar ativado para a interface de rede. Antes de criar uma interface de rede, tem de obter virtual Id com rede [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork), em seguida, o Id de sub-rede com [Get-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig). Criar uma interface de rede com [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) no *DMZ* sub-rede com o reencaminhamento IP está ativado:

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

#### <a name="create-a-virtual-machine"></a>Criar uma máquina virtual

Para criar uma máquina virtual e anexar uma interface de rede existente, primeiro tem de criar uma configuração de máquina virtual com [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). A configuração inclui a interface de rede que criou no passo anterior. Quando lhe for pedido um nome de utilizador e palavra-passe, selecione o nome de utilizador e palavra-passe que pretende iniciar sessão na máquina virtual com o. 

```azurepowershell-interactive
# Create a credential object.
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create a virtual machine configuration.
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

Criar a máquina virtual utilizando a configuração de máquina virtual com o [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). O exemplo seguinte cria uma máquina virtual denominada *myVmNva*. 

```azurepowershell-interactive
$vmNva = New-AzureRmVM `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -VM $vmConfig `
  -AsJob
```

O `-AsJob` opção cria a máquina virtual em segundo plano, para poder continuar para o passo seguinte. Quando lhe for pedido, introduza o nome de utilizador e palavra-passe que pretende iniciar sessão na máquina virtual com o. Em ambientes de produção, a aplicação virtual de rede que implementa é frequentemente uma máquina virtual previamente configurada. Várias aplicações virtuais de rede estão disponíveis a partir de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1).

Azure atribuída 10.0.2.4 como o endereço IP privado da máquina virtual, porque 10.0.2.4 é o primeiro endereço IP disponível na *DMZ* sub-rede de *myVirtualNetwork*.

### <a name="create-virtual-machines"></a>Criar máquinas virtuais

Criar duas máquinas virtuais na rede virtual para que possa validar que o tráfego de entrada a *pública* sub-rede é encaminhada para o *privada* sub-rede através da aplicação virtual de rede num passo posterior. 

Criar uma máquina virtual no *pública* sub-rede com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). O exemplo seguinte cria uma máquina virtual denominada *myVmWeb* no *pública* sub-rede do *myVirtualNetwork* rede virtual. 

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Public" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmWeb" `
  -AsJob
```

Azure atribuída 10.0.0.4 como o endereço IP privado da máquina virtual, porque 10.0.1.4 é o primeiro endereço IP disponível na *pública* sub-rede de *myVirtualNetwork*.

Criar uma máquina virtual no *privada* sub-rede.

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Private" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmMgmt"
```

A máquina virtual demora alguns minutos a criar. Azure atribuída 10.0.1.4 como o endereço IP privado da máquina virtual, porque 10.0.1.4 é o primeiro endereço IP disponível na *privada* sub-rede de *myVirtualNetwork*. 

Não continue com o passo seguinte até que a máquina virtual está criada e Azure devolve resultado para o PowerShell.

### <a name="route-traffic-through-a-network-virtual-appliance"></a>Encaminhar o tráfego através de uma aplicação virtual de rede

Utilize [conjunto AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) para ativar o ICMP entrada para o *myVmWeb* e *myVmMgmt* máquinas virtuais através da firewall do Windows para utilizar tracert para testar comunicação entre as máquinas virtuais num passo posterior:

```powershell-interactive
Set-AzureRmVMExtension `
  -ResourceGroupName myResourceGroup `
  -VMName myVmWeb `
  -ExtensionName AllowICMP `
  -Publisher Microsoft.Compute `
  -ExtensionType CustomScriptExtension `
  -TypeHandlerVersion 1.8 `
  -SettingString '{"commandToExecute": "netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}' `
  -Location EastUS

Set-AzureRmVMExtension `
  -ResourceGroupName myResourceGroup `
  -VMName myVmMgmt `
  -ExtensionName AllowICMP `
  -Publisher Microsoft.Compute `
  -ExtensionType CustomScriptExtension `
  -TypeHandlerVersion 1.8 `
  -SettingString '{"commandToExecute": "netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow"}' `
  -Location EastUS
```

Os comandos anteriores podem demorar alguns minutos a concluir. Não continue com o passo seguinte até concluir os comandos e o resultado é devolvido para o PowerShell. Embora tracert é utilizado para testar o encaminhamento neste artigo, permitir ICMP através da Firewall do Windows para implementações de produção não é recomendada.

Ligar ao endereço IP público de uma máquina virtual a partir da Internet. Utilize [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) para devolver o endereço IP público de uma máquina virtual. O exemplo seguinte devolve o endereço IP público do *myVmMgmt* máquina virtual:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Utilize o seguinte comando para criar uma sessão de ambiente de trabalho remota com o *myVmMgmt* máquina virtual do seu computador local. Substitua `<publicIpAddress>` com o endereço IP devolvido do comando anterior.

```
mstsc /v:<publicIpAddress>
```

Um ficheiro de protocolo de ambiente de trabalho remoto (RDP) é criado, transferido para o computador e aberto. Introduza o nome de utilizador e palavra-passe que especificou ao criar a máquina virtual (poderá ter de selecionar **mais opções**, em seguida, **utilizar uma conta diferente**, para especificar as credenciais introduzidas quando a criar a máquina virtual) e, em seguida, selecione **OK**. Poderá receber um aviso de certificado durante o processo de início de sessão. Clique em **Sim** ou **Continuar** para continuar com a ligação. 

Ativar o reencaminhamento IP no Azure para a interface de rede da máquina virtual no [ativar IP fowarding](#enable-ip-forwarding). Na máquina virtual, o sistema operativo ou uma aplicação em execução numa máquina virtual, deve também conseguir encaminhar o tráfego de rede. Quando implementa uma aplicação virtual de rede num ambiente de produção, o dispositivo normalmente filtra, regista ou efetua algumas outra função antes do reencaminhamento de tráfego. No entanto, neste artigo o sistema operativo simplesmente reencaminham todo o tráfego que recebe. Ativar o reencaminhamento IP no sistema operativo do *myVmNva*:

Numa linha de comandos no *myVmMgmt* máquina virtual, o ambiente de trabalho remoto para o *myVmNva* máquina virtual:

``` 
mstsc /v:myVmNva
```
    
Para ativar o reencaminhamento IP no sistema operativo do *myVmNva* máquina virtual, introduza o seguinte comando do PowerShell *myVmNva* máquina virtual:

```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
```
    
Reinicie o *myVmNva* máquina virtual, que também irá desligar a sessão de ambiente de trabalho remota, deixando-dentro da sessão de ambiente de trabalho remota aberta para o *myVmMgmt* máquina virtual.

Depois do *myVmNva* reinícios da máquina virtual, utilize o seguinte comando para testar o encaminhamento de tráfego de rede para o *myVmWeb* máquina virtual a partir de *myVmMgmt* virtual máquina.

```
tracert myvmweb
```

A resposta é semelhante ao seguinte exemplo:

```
Tracing route to myvmweb.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
over a maximum of 30 hops:
    
1     1 ms     1 ms     1 ms  10.0.0.4
  
Trace complete.
```

Pode ver que o tráfego é encaminhado diretamente a partir de *myVmMgmt* máquina virtual para o *myVmWeb* máquina virtual. Rotas predefinidas do Azure, encaminhar o tráfego diretamente entre sub-redes.

Utilize o seguinte comando para ambiente de trabalho remoto para o *myVmWeb* máquina virtual a partir de *myVmMgmt* máquina virtual:

``` 
mstsc /v:myVmWeb
```

Para testar o encaminhamento de tráfego de rede para o *myVmMgmt* máquina virtual a partir de *myVmWeb* máquina virtual, introduza o comando seguinte numa linha de comandos:

```
tracert myvmmgmt
```

A resposta é semelhante ao seguinte exemplo:

```
Tracing route to myvmmgmt.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
over a maximum of 30 hops:
        
1    <1 ms     *        1 ms  10.0.2.4
2     1 ms     1 ms     1 ms  10.0.1.4
        
Trace complete.
```

Pode ver que o primeiro salto é 10.0.2.4, que é o endereço IP privado a rede aplicação virtual. O segundo hop é 10.0.1.4, o endereço IP privado do *myVmMgmt* máquina virtual. A rota à *myRouteTablePublic* tabela de rotas e associado ao *pública* sub-rede causado do Azure encaminhar o tráfego através de NVA, em vez de diretamente para o *privada* sub-rede.

Fechar as sessões de ambiente de trabalho remotas para ambos os *myVmWeb* e *myVmMgmt* máquinas virtuais.

## <a name="troubleshoot-routing"></a>Resolver problemas de encaminhamento

Conforme aprendido nos passos anteriores, Azure aplica-se rotas predefinidas, que pode, opcionalmente, substituir com as seus próprios rotas. Por vezes, o tráfego poderão não ser encaminhado conforme esperado que seja. Utilize [New-AzureRmNetworkWatcher](/powershell/module/azurerm.network/new-azurermnetworkwatcher) para ativar um observador de rede na região EastUS, se ainda não tiver um observador de rede nessa região:

```azurepowershell-interactive
# Enable network watcher for east region, if you don't already have a network watcher enabled for the region:
$nw = New-AzureRmNetworkWatcher `
 -Location eastus `
 -Name myNetworkWatcher_eastus `
 -ResourceGroupName myResourceGroup
```

Utilize [Get-AzureRmNetworkWatcherNextHop](/powershell/module/azurerm.network/get-azurermnetworkwatchernexthop) para determinar a forma como o tráfego é encaminhado entre duas máquinas virtuais. Por exemplo, o seguinte comando os testes de encaminhamento de tráfego do *myVmWeb* (10.0.0.4) máquina virtual para o *myVmMgmt* (10.0.1.4) máquina virtual:

```azurepowershell-interactive
$vmWeb = Get-AzureRmVM `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup

Get-AzureRmNetworkWatcherNextHop `
  -DestinationIPAddress 10.0.1.4 `
  -NetworkWatcherName myNetworkWatcher_eastus `
  -ResourceGroupName myResourceGroup `
  -SourceIPAddress 10.0.0.4 `
  -TargetVirtualMachineId $vmWeb.Id
```
O seguinte resultado é devolvido após uma curta espera:

```azurepowershell
NextHopIpAddress    NextHopType       RouteTableId
------------------  ---------------- ---------------------------------------------------------------------------------------------------------------------------
10.0.2.4            VirtualAppliance  /subscriptions/<Subscription-Id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/routeTables/myRouteTablePublic
```

A saída informa que o próximo salto endereço IP para o tráfego de *myVmWeb* para *myVmMgmt* é 10.0.2.4 (o *myVmNva* máquina virtual), que o tipo de próximo salto é  *VirtualAppliance*, sendo que a tabela de rota que faz com que o encaminhamento *myRouteTablePublic*.

As rotas em vigor para cada interface de rede são uma combinação de rotas predefinidas do Azure e as rotas que define. Para ver todas as rotas eficazes para uma interface de rede numa máquina virtual com [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable). Por exemplo, para mostrar as rotas efetivas para o *myVmWeb* interface de rede no *myVmWeb* máquina virtual, introduza o seguinte comando:

```azurepowershell-interactive
Get-AzureRmEffectiveRouteTable `
  -NetworkInterfaceName myVmWeb `
  -ResourceGroupName myResourceGroup `
  | Format-Table
```

Todas as rotas predefinidas e a rota adicionado no passo anterior, são devolvidos.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário utilizar [Remove-AzureRmResourcegroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o grupo de recursos e todos os recursos nele contidos.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, criou uma tabela de rota e associados-la a uma sub-rede. Criar uma aplicação virtual de rede que encaminhados tráfego de sub-rede público para uma sub-rede privada. Enquanto pode implementar vários recursos do Azure dentro de uma rede virtual, os recursos para alguns serviços do Azure PaaS não não possível implementar numa rede virtual. Pode ainda restringir o acesso aos recursos de alguns serviços do Azure PaaS para o tráfego apenas a partir de uma sub-rede de rede virtual apesar. Avançar para o artigo seguinte para saber como restringir o acesso de rede para recursos do Azure PaaS.

> [!div class="nextstepaction"]
> [Restringir o acesso de rede para recursos de PaaS](virtual-network-service-endpoints-configure.md#azure-powershell)