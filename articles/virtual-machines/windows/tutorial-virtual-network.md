---
title: "Redes virtuais do Azure e máquinas virtuais do Windows | Microsoft Docs"
description: "Tutorial - Gerir redes virtuais do Azure e máquinas virtuais do Windows com o Azure PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/27/2018
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 3a59d85ea19ba6670ffbb60aa9b764560a3567a0
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="manage-azure-virtual-networks-and-windows-virtual-machines-with-azure-powershell"></a>Gerir redes virtuais do Azure e máquinas virtuais do Windows com o Azure PowerShell

As máquinas virtuais do Azure utilizam a rede do Azure para a comunicação de rede interna e externa. Este tutorial mostra a implementação de duas máquinas virtuais e a configuração da rede do Azure para essas VMs Os exemplos neste tutorial partem do princípio de que as VMs alojam uma aplicação Web com um back-end de base de dados. No entanto, não é implementada nenhuma aplicação no tutorial. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma rede virtual e uma sub-rede
> * Crie um endereço IP público
> * Criar uma VM de front-end
> * Proteger o tráfego de rede
> * Criar a VM de back-end



Este tutorial requer AzureRM.Compute versão do módulo 4.3.1 ou posterior. Executar `Get-Module -ListAvailable AzureRM.Compute` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps).

## <a name="vm-networking-overview"></a>Descrição geral do funcionamento em rede de VMs

As redes virtuais do Azure permitem ligações de rede seguras entre as máquinas virtuais, a Internet e outros serviços do Azure, como a Base de Dados SQL do Azure. As redes virtuais são divididas em segmentos lógicos, chamados sub-redes. As sub-redes são utilizadas para controlar o fluxo de rede e como limite de segurança. Ao implementar uma VM, incluem, geralmente, uma interface de rede virtual, que é ligada a uma sub-rede.

Ao concluir este tutorial, pode ver estes recursos serem criados:

![Rede virtual com duas sub-redes](./media/tutorial-virtual-network/networktutorial.png)

- *myVNet* - a rede virtual que as VMs utilizam para comunicar entre si e com a Internet.
- *myFrontendSubnet* - a sub-rede em *myVNet* que os recursos de front-end utilizam.
- *myPublicIPAddress* - o endereço IP público utilizado para aceder a *myFrontendVM* a partir da Internet.
- *myFrontentNic* - a interface de rede utilizada por *myFrontendVM* para comunicar com *myBackendVM*.
- *myFrontendVM* - a VM utilizada para comunicar entre a Internet e *myBackendVM*.
- *myBackendNSG* - o grupo de segurança de rede que controla a comunicação entre *myFrontendVM* e *myBackendVM*.
- *myBackendSubnet* - a sub-rede associada a *myBackendNSG* e utilizada pelos recursos de back-end.
- *myBackendNic* - a interface de rede utilizada por *myBackendVM* para comunicar com *myFrontendVM*.
- *myBackendVM* -a VM que utiliza a porta 1433 para comunicar com *myFrontendVM*.


## <a name="create-a-virtual-network-and-subnet"></a>Criar uma rede virtual e uma sub-rede

Neste tutorial, é criada uma rede virtual individual com duas sub-redes. Uma sub-rede de front-end para alojar uma aplicação Web e uma sub-rede de back-end para alojar um servidor de bases de dados.

Antes de poder criar uma rede virtual, crie um grupo de recursos utilizando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). O exemplo seguinte cria um grupo de recursos denominado *myRGNetwork* no *EastUS* localização:

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName myRGNetwork -Location EastUS
```

### <a name="create-subnet-configurations"></a>Criar configurações de sub-rede

Criar uma configuração de sub-rede com o nome *myFrontendSubnet* utilizando [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myFrontendSubnet `
  -AddressPrefix 10.0.0.0/24
```

E, criar uma configuração de sub-rede com o nome *myBackendSubnet*:

```azurepowershell-interactive
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.1.0/24
```

### <a name="create-virtual-network"></a>Criar a rede virtual

Criar uma VNET com o nome *myVNet* utilizando *myFrontendSubnet* e *myBackendSubnet* utilizando [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork):

```azurepowershell-interactive
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $frontendSubnet, $backendSubnet
```

Nesta fase, já está criada uma rede e segmentada em duas sub-redes, uma para os serviços de front-end e outra para os de back-end. Na secção seguinte, são criadas máquinas virtuais e ligadas a estas sub-redes.

## <a name="create-a-public-ip-address"></a>Crie um endereço IP público

Os endereços IP públicos permitem que os recursos do Azure estejam acessíveis na Internet. O método de atribuição do endereço IP público pode ser configurado como dinâmico ou estático. Por predefinição, o endereço IP público é atribuído dinamicamente. Os endereços IP dinâmicos são libertados quando uma VM é desalocada. Este comportamento faz com que o endereço IP mude durante qualquer operação que inclua desalocar uma VM.

O método de alocação pode ser definido como estático, o que garante que o endereço IP permanece atribuído a uma VM, mesmo durante o estado desalocado. Quando é utilizado um endereço IP atribuído estaticamente, não é possível especificar o próprio endereço IP. Em vez disso, é alocado partir de um conjunto de endereços disponíveis.

Criar um endereço IP público com o nome *myPublicIPAddress* utilizando [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress):

```azurepowershell-interactive
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -AllocationMethod Dynamic `
  -Name myPublicIPAddress
```

Pode alterar o parâmetro - AllocationMethod para `Static` para atribuir um endereço IP público estático.

## <a name="create-a-front-end-vm"></a>Criar uma VM de front-end

Para uma VM comunicar numa rede virtual, necessita de uma interface de rede virtual (NIC). Criar uma NIC utilizando [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface):

```azurepowershell-interactive
$frontendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontend `
  -SubnetId $vnet.Subnets[0].Id `
  -PublicIpAddressId $pip.Id
```

Definir o nome de utilizador e palavra-passe necessária para a conta de administrador sobre a utilização de VM [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential). Utilizar estas credenciais para ligar à VM nos passos adicionais:

```azurepowershell-interactive
$cred = Get-Credential
```

Criar as VMs com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

```azurepowershell-interactive
New-AzureRmVM `
   -Credential $cred `
   -Name myFrontend `
   -PublicIpAddressName myPublicIPAddress `
   -ResourceGroupName myRGNetwork `
   -Location "EastUS" `
   -Size Standard_D1 `
   -SubnetName myFrontendSubnet `
   -VirtualNetworkName myVNet
```

## <a name="secure-network-traffic"></a>Proteger o tráfego de rede

Os grupos de segurança de rede (NSG) contêm uma lista de regras de segurança que permitem ou negam o tráfego de rede para recursos ligados a Redes Virtuais do Azure (VNet). Os NSGs podem ser associados a sub-redes ou a interfaces de rede individuais. Quando um NSG é associado uma interface de rede, aplica apenas a VM associada. Quando um NSG é associado a uma sub-rede, as regras são aplicadas a todos os recursos ligados à mesma.

### <a name="network-security-group-rules"></a>Regras do grupo de segurança de rede

As regras do NSG definem as portas de rede através das quais o tráfego é permitido ou recusado. As regras podem incluir intervalos de endereços IP de origem e de destino, para que o tráfego seja controlado entre sistemas ou sub-redes específicos. As regras do NSG também incluem uma prioridade (entre 1 e 4096). As regras são avaliadas por ordem de prioridade. Uma regra com uma prioridade de 100 é avaliada antes de uma regra com prioridade de 200.

Todos os NSGs contêm um conjunto de regras predefinidas. As regras predefinidas não podem ser eliminadas, mas como lhes é atribuída a prioridade mais baixa, podem ser substituídas pelas regras que criar.

- **Rede virtual** - o tráfego que tem origem e termina numa rede virtual é permitido nas direções de entrada e de saída.
- **Internet** - o tráfego de saída é permitido, mas o tráfego de entrada é bloqueado.
- **Balanceador de carga** - permitir que o balanceador de carga do Azure sonde o estado de funcionamento das VMs e das instâncias de função. Se não estiver a utilizar um conjunto com balanceamento de carga, pode substituir esta regra.

### <a name="create-network-security-groups"></a>Criar grupos de segurança de rede

Criar uma regra de entrada com o nome *myFrontendNSGRule* para permitir o tráfego web recebido em *myFrontendVM* utilizando [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig):

```azurepowershell-interactive
$nsgFrontendRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myFrontendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 200 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow
```

Pode limitar o tráfego interno para *myBackendVM* de apenas *myFrontendVM* criando um NSG para a sub-rede de back-end. O exemplo seguinte cria uma regra NSG designada *myBackendNSGRule*:

```azurepowershell-interactive
$nsgBackendRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myBackendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 100 `
  -SourceAddressPrefix 10.0.0.0/24 `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 1433 `
  -Access Allow
```

Adicionar um grupo de segurança de rede com o nome *myFrontendNSG* utilizando [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup):

```azurepowershell-interactive
$nsgFrontend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myFrontendNSG `
  -SecurityRules $nsgFrontendRule
```

Agora, adicione um grupo de segurança de rede com o nome *myBackendNSG* utilizando New-AzureRmNetworkSecurityGroup:

```azurepowershell-interactive
$nsgBackend = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackendNSG `
  -SecurityRules $nsgBackendRule
```

Adicione os grupos de segurança de rede para as sub-redes:

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myRGNetwork `
  -Name myVNet
$frontendSubnet = $vnet.Subnets[0]
$backendSubnet = $vnet.Subnets[1]
$frontendSubnetConfig = Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name myFrontendSubnet `
  -AddressPrefix $frontendSubnet.AddressPrefix `
  -NetworkSecurityGroup $nsgFrontend
$backendSubnetConfig = Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name myBackendSubnet `
  -AddressPrefix $backendSubnet.AddressPrefix `
  -NetworkSecurityGroup $nsgBackend
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

## <a name="create-a-back-end-vm"></a>Criar uma VM de back-end

É a forma mais fácil para criar a VM de back-end para este tutorial, utilizando uma imagem do SQL Server. Este tutorial só cria a VM com o servidor de base de dados, mas não fornece informações sobre como aceder à base de dados.

Criar *myBackendNic*:

```azurepowershell-interactive
$backendNic = New-AzureRmNetworkInterface `
  -ResourceGroupName myRGNetwork `
  -Location EastUS `
  -Name myBackend `
  -SubnetId $vnet.Subnets[1].Id
```

Defina o nome de utilizador e palavra-passe necessária para a conta de administrador na VM com o Get-Credential:

```azurepowershell-interactive
$cred = Get-Credential
```

Criar *myBackendVM*.

```azurepowershell-interactive
New-AzureRmVM `
   -Credential $cred `
   -Name myBackend `
   -ImageName "MicrosoftSQLServer:SQL2016SP1-WS2016:Enterprise:latest" `
   -ResourceGroupName myRGNetwork `
   -Location "EastUS" `
   -SubnetName myFrontendSubnet `
   -VirtualNetworkName myVNet
```

A imagem que é utilizada com o SQL Server instalada, mas não é utilizada neste tutorial. Está incluído para lhe mostrar como pode configurar uma VM para processar o tráfego web e uma VM processe a gestão de base de dados.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, criou e protegeu redes do Azure relacionadas com máquinas virtuais. 

> [!div class="checklist"]
> * Criar uma rede virtual e uma sub-rede
> * Crie um endereço IP público
> * Criar uma VM de front-end
> * Proteger o tráfego de rede
> * Criar uma VM de back-end

Avançar para o próximo tutorial para saber mais sobre a monitorização de proteger dados em máquinas virtuais utilizando cópia de segurança do Azure.

> [!div class="nextstepaction"]
> [Cópia de segurança de máquinas virtuais do Windows no Azure](./tutorial-backup-vms.md)
