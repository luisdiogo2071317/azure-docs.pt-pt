---
title: Diagnosticar um máquina virtual encaminhamento problema de rede - Azure PowerShell | Microsoft Docs
description: Neste artigo, irá aprender a diagnosticar um problema encaminhamento de rede de máquina virtual utilizando a capacidade de salto seguinte do observador de rede do Azure.
services: network-watcher
documentationcenter: network-watcher
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I need to diagnose virtual machine (VM) network routing problem that prevents communication to different destinations.
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: network-watcher
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: f793a201b3fbf57ac2f420c4f4e57a230bc11468
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="diagnose-a-virtual-machine-network-routing-problem---azure-powershell"></a>Diagnosticar um máquina virtual encaminhamento problema de rede - Azure PowerShell

Neste artigo, implementar uma máquina virtual (VM) e, em seguida, verifique as comunicações com um endereço IP e o URL. Determinar a causa de uma falha de comunicação e como resolvê-lo.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este artigo requer o AzureRM PowerShell versão do módulo 5.4.1 ou posterior. Para localizar a versão instalada, execute `Get-Module -ListAvailable AzureRM`. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="create-a-vm"></a>Criar uma VM

Antes de poder criar uma VM, tem de criar um grupo de recursos que contém a VM. Crie um grupo de recursos com [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

Criar a VM com [novo-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Ao executar este passo, serão pedidas credenciais. Os valores que introduzir são configurados, como o nome de utilizador e a palavra-passe para a VM.

```azurepowershell-interactive
$vM = New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVm" `
    -Location "East US"
```

A criação da VM demora alguns minutos. Não continue com os restantes passos até que a VM é criada e PowerShell devolve um resultado.

## <a name="test-network-communication"></a>Comunicação de rede de teste

Para testar a comunicação de rede com o observador de rede, tem de ativar primeiro um observador de rede na região a VM que pretende testar está a ser e, em seguida, utilizar a capacidade de salto seguinte do observador de rede para testar a comunicação.

## <a name="enable-network-watcher"></a>Ativar o observador de rede

Se já tiver um observador de rede ativado na região EUA leste, utilize [Get-AzureRmNetworkWatcher](/powershell/module/azurerm.network/get-azurermnetworkwatcher) para obter o observador de rede. O exemplo seguinte obtém um observador de rede existente com o nome *NetworkWatcher_eastus* que está a ser o *NetworkWatcherRG* grupo de recursos:

```azurepowershell-interactive
$networkWatcher = Get-AzureRmNetworkWatcher `
  -Name NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG
```

Se ainda não tiver um observador de rede ativado na região EUA leste, utilize [New-AzureRmNetworkWatcher](/powershell/module/azurerm.network/new-azurermnetworkwatcher) para criar um observador de rede na região EUA Leste:

```azurepowershell-interactive
$networkWatcher = New-AzureRmNetworkWatcher `
  -Name "NetworkWatcher_eastus" `
  -ResourceGroupName "NetworkWatcherRG" `
  -Location "East US"
```

### <a name="use-next-hop"></a>Utilizar o próximo salto

Azure cria automaticamente as rotas para destinos de predefinição. Pode criar rotas personalizadas que substituem as rotas predefinidas. Por vezes, as rotas personalizadas podem fazer com comunicação falhar. Para testar o encaminhamento de uma VM, utilize o [Get-AzureRmNetworkWatcherNextHop](/powershell/module/azurerm.network/get-azurermnetworkwatchernexthop) o comando para determinar o próximo salto de encaminhamento ao tráfego é destinado a um endereço específico.

Comunicações de saída da VM para um dos endereços IP para www.bing.com de teste:

```azurepowershell-interactive
Get-AzureRmNetworkWatcherNextHop `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $VM.Id `
  -SourceIPAddress 192.168.1.4 `
  -DestinationIPAddress 13.107.21.200
```

Após alguns segundos, o resultado informa-o que o **NextHopType** é **Internet**e que o **RouteTableId** é **rota de sistema**. Este resultado permite-lhe saber que existe uma rota válida para o destino.

Comunicações de saída da VM para 172.31.0.100 de teste:

```azurepowershell-interactive
Get-AzureRmNetworkWatcherNextHop `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $VM.Id `
  -SourceIPAddress 192.168.1.4 `
  -DestinationIPAddress 172.31.0.100
```

Informa-o resultado devolvido que **nenhum** é o **NextHopType**e que o **RouteTableId** também é **rota de sistema**. Este resultado permite-lhe saber que, enquanto existir uma rota de sistema válido para o destino, não há nenhum salto seguinte para encaminhar o tráfego para o destino.

## <a name="view-details-of-a-route"></a>Ver detalhes de uma rota

Para analisar ainda mais a encaminhamento, reveja as rotas efetivas para a interface de rede com o [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable) comando:

```azurepowershell-interactive
Get-AzureRmEffectiveRouteTable `
  -NetworkInterfaceName myVm `
  -ResourceGroupName myResourceGroup |
  Format-table
```

É devolvido o resultado que inclui o seguinte texto:

```powershell
Name State  Source  AddressPrefix           NextHopType NextHopIpAddress
---- -----  ------  -------------           ----------- ----------------
     Active Default {192.168.0.0/16}        VnetLocal   {}              
     Active Default {0.0.0.0/0}             Internet    {}              
     Active Default {10.0.0.0/8}            None        {}              
     Active Default {100.64.0.0/10}         None        {}              
     Active Default {172.16.0.0/12}         None        {}              
```

Como pode ver no resultado do anterior, a rota com o **AaddressPrefix** de **0.0.0.0/0** encaminha todo o tráfego não destinado a endereços prefixos de endereços de outras rota com um salto seguinte do **Internet**. Como pode também ver na saída, embora há uma rota predefinida para o prefixo 172.16.0.0/12, que inclui o 172.31.0.100 endereço, a **nextHopType** é **nenhum**. Azure cria uma rota predefinida para 172.16.0.0/12, mas não especifica um tipo de próximo salto até um motivo para. Se, por exemplo, o intervalo de endereços 172.16.0.0/12 tiver adicionado ao espaço de endereços da rede virtual, o Azure altera o **nextHopType** para **rede Virtual** da rota. Uma verificação de, em seguida, seria mostrar **rede Virtual** como o **nextHopType**.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, pode utilizar [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o grupo de recursos e todos os recursos que contém:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, criou uma VM e diagnosticado encaminhamento de rede da VM. Aprendeu a que o Azure cria várias rotas predefinidas e testado encaminhamento para duas destinos diferentes. Saiba mais sobre [encaminhamento no Azure](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) e como [criar rotas personalizadas](../virtual-network/manage-route-table.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-route).

Para ligações de saída VM, também é possível determinar a latência e e rejeições tráfego de rede entre a VM e um ponto final com o observador de rede [resolver problemas de ligação](network-watcher-connectivity-powershell.md) capacidade. Pode monitorizar a comunicação entre uma VM e um ponto final, tais como um endereço IP ou o URL, ao longo do tempo, utilizar a capacidade de monitor de ligação do observador de rede. Para saber como, consulte [monitorizar uma ligação de rede](connection-monitor.md).