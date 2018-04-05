---
title: Criar uma Azure Virtual Network - PowerShell | Microsoft Docs
description: Saiba mais rapidamente criar uma rede virtual com o PowerShell. Uma rede virtual permite que os recursos do Azure, tais como as máquinas virtuais, em privado comunicar entre si e com a internet.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: ''
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/09/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: fe171000f83c27f23972569b93e351340f4426ad
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="create-a-virtual-network-using-powershell"></a>Criar uma rede virtual com o PowerShell

Uma rede virtual permite que os recursos do Azure, como as máquinas virtuais (VM), em privado comunicar entre si e com a internet. Neste artigo, irá aprender a criar uma rede virtual. Depois de criar uma rede virtual, implementar duas VMs para a rede virtual. Em seguida, ligar a uma VM a partir da internet e comunicar em privado entre as duas VMs.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este artigo requer o AzureRM PowerShell versão do módulo 5.4.1 ou posterior. Para localizar a versão instalada, execute ` Get-Module -ListAvailable AzureRM`. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

Antes de poder criar uma rede virtual, terá de criar um grupo de recursos que contém a rede virtual. Criar um grupo de recursos com [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

Crie uma rede virtual com [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). O exemplo seguinte cria uma rede virtual da predefinição denominada *myVirtualNetwork* no *EastUS* localização:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Recursos do Azure são implementados para uma sub-rede dentro de uma rede virtual, por isso terá de criar uma sub-rede. Criar uma configuração de sub-rede com [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). 

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name default `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

A configuração de sub-rede de escrita para a rede virtual com [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), que cria a sub-rede na rede virtual:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Crie duas VMs na rede virtual:

### <a name="create-the-first-vm"></a>Criar a primeira VM

Criar uma VM com [novo-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Quando executar o comando que se segue, lhe forem pedidas as credenciais. Os valores que introduziu estão configurados como nome de utilizador e palavra-passe para a VM. O `-AsJob` opção cria a VM em segundo plano, para que possa continuar para o passo seguinte.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "default" `
    -Name "myVm1" `
    -AsJob
```

Resultado semelhante ao seguinte exemplo de saída é devolvido e Azure começa a criar a VM em segundo plano.

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command                  
--     ----            -------------   -----         -----------     --------             -------                  
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM     
```

### <a name="create-the-second-vm"></a>Criar a VM segundo 

Introduza o seguinte comando:

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "default" `
  -Name "myVm2"
```

A VM demora alguns minutos a criar. Não continue com o passo seguinte até que executa o comando anterior e o resultado é devolvido para o PowerShell.

## <a name="connect-to-a-vm-from-the-internet"></a>Ligar a uma VM a partir da internet

Utilize [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) para devolver o endereço IP público de uma VM. O exemplo seguinte devolve o endereço IP público do *myVm1* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Substitua `<publicIpAddress>` no comando seguinte, com o IP público endereço devolvido pelo comando anterior e, em seguida, introduza o seguinte comando: 

```
mstsc /v:<publicIpAddress>
```

Um ficheiro de protocolo de ambiente de trabalho remoto (RDP) é criado e transferido para o seu computador. Abra o ficheiro rdp transferido. Se lhe for pedido, selecione **Connect**. Introduza o nome de utilizador e palavra-passe que especificou ao criar a VM. Poderá ter de selecionar **mais opções**, em seguida, **utilizar uma conta diferente**, para especificar as credenciais que introduziu quando criou a VM. Selecione **OK**. Poderá receber um aviso de certificado durante o processo de início de sessão. Se receber o aviso, selecione **Sim** ou **continuar**, prossiga com a ligação.

## <a name="communicate-privately-between-vms"></a>Comunicar modo privado entre VMs

A partir do PowerShell no *myVm1* VM, introduza `ping myvm2`. Ping falhar, porque o ping utiliza o protocolo de mensagem de controlo de internet (ICMP) e ICMP não é permitido através da firewall do Windows, por predefinição.

Para permitir *myVm2* para enviar um ping *myVm1* num passo posterior, introduza o seguinte comando do PowerShell, que permite ICMP entrada através da firewall do Windows:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Fechar a ligação de ambiente de trabalho remota para *myVm1*. 

Execute os passos do [ligar a uma VM a partir da internet](#connect-to-a-vm-from-the-internet) novamente, mas a ligação à *myVm2*. 

Numa linha de comandos no *myVm2* VM, introduza `ping myvm1`.

Receber respostas do *myVm1*porque permitido ICMP através da firewall do Windows no *myVm1* VM num passo anterior.

Fechar a ligação de ambiente de trabalho remota para *myVm2*.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, pode utilizar [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o grupo de recursos e todos os recursos que contém:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, criou uma rede virtual predefinido e duas VMs. Ligado a uma VM a partir da Internet e comunidades de forma clara modo privado entre a VM e outra VM. Para saber mais sobre as definições de rede virtual, consulte [gerir uma rede virtual](manage-virtual-network.md). 

Por predefinição, o Azure permite a comunicação privada sem restrições entre máquinas virtuais, mas só permite ligações de ambiente de trabalho remotas entrada para VMs do Windows através da Internet. Para saber como permitir ou restringir a diferentes tipos de comunicação de rede de e para as VMs, avançar para o próximo tutorial.

> [!div class="nextstepaction"]
> [Filtrar o tráfego de rede](tutorial-filter-network-traffic.md)
