---
title: Filtrar o tráfego de rede - Azure PowerShell | Microsoft Docs
description: Neste artigo, irá aprender a filtrar o tráfego de rede a uma sub-rede, com um grupo de segurança de rede, utilizando o PowerShell.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to filter network traffic to virtual machines that perform similar functions, such as web servers.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/30/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 53406150cfc2ec4229ecd9cf3356ad03d60f8e7e
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="filter-network-traffic-with-a-network-security-group-using-powershell"></a>Filtrar o tráfego de rede com um grupo de segurança de rede com o PowerShell

Pode filtrar o tráfego de rede de entrada para e de saída de uma sub-rede de rede virtual com um grupo de segurança de rede. Grupos de segurança de rede contêm regras de segurança que filtrar o tráfego de rede pelo endereço IP, porta e protocolo. Regras de segurança são aplicadas a recursos implementados numa sub-rede. Neste artigo, saiba como:

> [!div class="checklist"]
> * Criar regras de grupo e de segurança de um segurança de rede
> * Criar uma rede virtual e associar um grupo de segurança de rede a uma sub-rede
> * Implementar máquinas virtuais (VM) a uma sub-rede
> * Filtros de tráfego de teste

Se preferir, pode concluir este artigo utilizando o [CLI do Azure](tutorial-filter-network-traffic-cli.md).

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este artigo requer o Azure PowerShell versão do módulo 5.4.1 ou posterior. Execute ` Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure. 

## <a name="create-a-network-security-group"></a>Criar um grupo de segurança de rede

Um grupo de segurança de rede contém regras de segurança. Especificam regras de segurança de uma origem e de destino. As origens e destinos podem ser grupos de segurança de aplicações.

### <a name="create-application-security-groups"></a>Criar grupos de segurança de aplicações

Em primeiro lugar, crie um grupo de recursos para todos os recursos criados neste artigo com [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). O exemplo seguinte cria um grupo de recursos a *eastus* localização: 


```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Criar um grupo de segurança de aplicações com [New-AzureRmApplicationSecurityGroup](/powershell/module/azurerm.network/new-azurermapplicationsecuritygroup). Um grupo de segurança de aplicações permite-lhe para servidores de grupo com os requisitos de filtragem de portas semelhantes. O exemplo seguinte cria dois grupos de segurança da aplicação.

```azurepowershell-interactive
$webAsg = New-AzureRmApplicationSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Name myAsgWebServers `
  -Location eastus

$mgmtAsg = New-AzureRmApplicationSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Name myAsgMgmtServers `
  -Location eastus
```

### <a name="create-security-rules"></a>Criar regras de segurança

Criar uma regra de segurança com [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). O exemplo seguinte cria uma regra que permite o tráfego de entrada a partir da internet para o *myWebServers* grupo de segurança de aplicações através das portas 80 e 443:

```azurepowershell-interactive
$webRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name "Allow-Web-All" `
  -Access Allow `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 100 `
  -SourceAddressPrefix Internet `
  -SourcePortRange * `
  -DestinationApplicationSecurityGroupId $webAsg.id `
  -DestinationPortRange 80,443

The following example creates a rule that allows traffic inbound from the internet to the *myMgmtServers* application security group over port 3389:

$mgmtRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name "Allow-RDP-All" `
  -Access Allow `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 110 `
  -SourceAddressPrefix Internet `
  -SourcePortRange * `
  -DestinationApplicationSecurityGroupId $mgmtAsg.id `
  -DestinationPortRange 3389
```

Neste artigo, RDP (porta 3389) está exposto à internet para o *myAsgMgmtServers* VM. Para ambientes de produção, em vez de exposição porta 3389 para a internet, recomenda-se que o se ligar aos recursos do Azure que pretende gerir com um [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou [privada](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ligação de rede.

### <a name="create-a-network-security-group"></a>Criar um grupo de segurança de rede

Criar um grupo de segurança de rede com [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). O exemplo seguinte cria um grupo de segurança de rede com o nome *myNsg*: 

```powershell-interactive
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -Name myNsg `
  -SecurityRules $webRule,$mgmtRule
```

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

Crie uma rede virtual com [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). O exemplo seguinte cria um virtual com o nome *myVirtualNetwork*:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Criar uma configuração de sub-rede com [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig)e, em seguida, escrever a configuração de sub-rede da rede virtual com [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork). O exemplo seguinte adiciona uma sub-rede designada *mySubnet* para a rede virtual e, em seguida, associa a *myNsg* grupo de segurança de rede ao mesmo:

```azurepowershell-interactive
Add-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -VirtualNetwork $virtualNetwork `
  -AddressPrefix "10.0.2.0/24" `
  -NetworkSecurityGroup $nsg
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Antes de criar as VMs, obter o objeto de rede virtual com a sub-rede com [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):

```powershell-interactive
$virtualNetwork = Get-AzureRmVirtualNetwork `
 -Name myVirtualNetwork `
 -Resourcegroupname myResourceGroup
```
Criar um endereço IP público para cada VM com [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

$publicIpWeb = New-AzureRmPublicIpAddress `
  -AllocationMethod Dynamic ` -ResourceGroupName myResourceGroup `
  -Location eastus ` -Name myVmWeb

$publicIpMgmt = New-AzureRmPublicIpAddress `
  -AllocationMethod Dynamic ` -ResourceGroupName myResourceGroup `
  -Location eastus ` -Name myVmMgmt


Criar duas interfaces de rede com [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface)e atribuir um endereço IP público para a interface de rede. O exemplo seguinte cria uma interface de rede, associa a *myVmWeb* endereço IP público e faz com que um membro do *myAsgWebServers* grupo de segurança da aplicação:

```powershell-interactive
$webNic = New-AzureRmNetworkInterface `
  -Location eastus `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup `
  -SubnetId $virtualNetwork.Subnets[0].Id `
  -ApplicationSecurityGroupId $webAsg.Id `
  -PublicIpAddressId $publicIpWeb.Id
```

O exemplo seguinte cria uma interface de rede, associa a *myVmMgmt* endereço IP público e faz com que um membro do *myAsgMgmtServers* grupo de segurança da aplicação:

```powershell-interactive
$mgmtNic = New-AzureRmNetworkInterface `
  -Location eastus `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup `
  -SubnetId $virtualNetwork.Subnets[0].Id `
  -ApplicationSecurityGroupId $mgmtAsg.Id `
  -PublicIpAddressId $publicIpMgmt.Id
```

Crie duas VMs na rede virtual para que possa validar tráfego filtragem num passo posterior. 

Criar uma configuração de VM com [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig), em seguida, criar a VM com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). O exemplo seguinte cria uma VM que irá servir como um servidor web. O `-AsJob` opção cria a VM em segundo plano, para poder continuar para o passo seguinte: 

```azurepowershell-interactive
# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

$webVmConfig = New-AzureRmVMConfig `
  -VMName myVmWeb `
  -VMSize Standard_DS1_V2 | `
Set-AzureRmVMOperatingSystem -Windows `
  -ComputerName myVmWeb `
  -Credential $cred | `
Set-AzureRmVMSourceImage `
  -PublisherName MicrosoftWindowsServer `
  -Offer WindowsServer `
  -Skus 2016-Datacenter `
  -Version latest | `
Add-AzureRmVMNetworkInterface `
  -Id $webNic.Id
New-AzureRmVM `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -VM $webVmConfig `
  -AsJob
```

Crie uma VM para servir como um servidor de gestão:

```azurepowershell-interactive
# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create the web server virtual machine configuration and virtual machine.
$mgmtVmConfig = New-AzureRmVMConfig `
  -VMName myVmMgmt `
  -VMSize Standard_DS1_V2 | `
Set-AzureRmVMOperatingSystem -Windows `
  -ComputerName myVmMgmt `
  -Credential $cred | `
Set-AzureRmVMSourceImage `
  -PublisherName MicrosoftWindowsServer `
  -Offer WindowsServer `
  -Skus 2016-Datacenter `
  -Version latest | `
Add-AzureRmVMNetworkInterface `
  -Id $mgmtNic.Id
New-AzureRmVM `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -VM $mgmtVmConfig
```

A máquina virtual demora alguns minutos a criar. Não continue com o passo seguinte até que o Azure acaba de criar a VM.

## <a name="test-traffic-filters"></a>Filtros de tráfego de teste

Utilize [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) para devolver o endereço IP público de uma VM. O exemplo seguinte devolve o endereço IP público do *myVmMgmt* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Utilize o seguinte comando para criar uma sessão de ambiente de trabalho remota com o *myVmMgmt* VM a partir do seu computador local. Substitua `<publicIpAddress>` com o endereço IP devolvido do comando anterior.

```
mstsc /v:<publicIpAddress>
```

Abra o ficheiro RDP transferido. Se lhe for pedido, selecione **Connect**.

Introduza o nome de utilizador e palavra-passe que especificou ao criar a VM (poderá ter de selecionar **mais opções**, em seguida, **utilizar uma conta diferente**, para especificar as credenciais que introduziu quando criou a VM), em seguida, selecione **OK**. Poderá receber um aviso de certificado durante o processo de início de sessão. Selecione **Sim** para continuar com a ligação. 
   
A ligação tiver êxito, porque a porta 3389 é permitida a entrada da internet para o *myAsgMgmtServers* grupo de segurança de aplicações que a interface de rede ligada à *myVmMgmt* a VM está na.

Utilize o seguinte comando para criar uma ligação de ambiente de trabalho remota para o *myVmWeb* VM, do *myVmMgmt* VM, com o seguinte comando, a partir do PowerShell:

``` 
mstsc /v:myvmWeb
```

A ligação é bem-sucedida porque uma regra de segurança predefinida de cada grupo de segurança de rede permite que o tráfego através de todas as portas entre todos os endereços IP numa rede virtual. Não é possível criar uma ligação de ambiente de trabalho remoto para o *myVmWeb* VM a partir da internet porque a regra de segurança para o *myAsgWebServers* não permite a porta 3389 de entrada da internet.

Utilize o seguinte comando para instalar o IIS da Microsoft no *myVmWeb* VM a partir do PowerShell:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Depois de concluída a instalação do IIS, desligue o *myVmWeb* VM, deixando-na *myVmMgmt* ligação de ambiente de trabalho remota de VM. Para ver o ecrã de boas-vindas do IIS, abra um browser da internet e navegue até à http://myVmWeb.

Desligue o *myVmMgmt* VM.

No seu computador, introduza o seguinte comando do PowerShell para obter o endereço IP público do *myVmWeb* servidor:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Para confirmar se consegue aceder a *myVmWeb* web server a partir de fora do Azure, abra um browser de internet no seu computador e navegue para `http://<public-ip-address-from-previous-step>`. A ligação tiver êxito, porque a porta 80 é permitida a entrada da internet para o *myAsgWebServers* grupo de segurança de aplicações que a interface de rede ligada à *myVmWeb* a VM está na.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, pode utilizar [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o grupo de recursos e todos os recursos que contém:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, criou um grupo de segurança de rede e associados-la a uma sub-rede de rede virtual. Para obter mais informações sobre grupos de segurança de rede, consulte [descrição geral de grupo de segurança de rede](security-overview.md) e [gerir um grupo de segurança de rede](virtual-network-manage-nsg-arm-ps.md).

Tráfego de rotas do Azure entre sub-redes por predefinição. Em vez disso, pode optar por encaminhar o tráfego entre sub-redes através de uma VM, que serve como uma firewall, por exemplo. Para saber como criar uma tabela de rota, avançar para o seguinte artigo.

> [!div class="nextstepaction"]
> [Criar uma tabela de rota](./tutorial-create-route-table-portal.md)