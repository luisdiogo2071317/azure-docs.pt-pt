---
title: Criar uma rede virtual - início rápido - Azure PowerShell | Microsoft Docs
description: Neste início rápido, vai aprender a criar uma rede virtual com o Portal do Azure. Uma rede virtual permite que os recursos do Azure, como as máquinas virtuais, comuniquem em privado entre si e com a internet.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can communicate with privately with each other and with the internet.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 12/04/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: 4e2808df56684b257898f3e03f8e9ca36682063b
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53341920"
---
# <a name="quickstart-create-a-virtual-network-using-powershell"></a>Início rápido: Criar uma rede virtual com o PowerShell

Uma rede virtual permite que os recursos do Azure, como as máquinas virtuais (VMs), comuniquem em privado entre si e com a internet. Neste início rápido, vai aprender a criar uma rede virtual. Depois de criar uma rede virtual, vai implementar duas VMs na rede virtual. Em seguida, ligar às VMs a partir da internet e comunicar em privado através da rede virtual.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) agora.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e usar o PowerShell localmente, este início rápido requer a utilização AzureRM módulo do PowerShell versão 5.4.1 ou posterior. Para localizar a versão instalada, execute `Get-Module -ListAvailable AzureRM`. Ver [módulo de instalar o Azure PowerShell](/powershell/azure/install-azurerm-ps) para instalação e informações de atualização.

Por fim, se estiver executando o PowerShell localmente, terá também de executar `Connect-AzureRmAccount`. Esse comando cria uma ligação com o Azure.

## <a name="create-a-resource-group-and-a-virtual-network"></a>Criar um grupo de recursos e uma rede virtual

Existem diversos passos que terá de percorrer para obter o seu grupo de recursos e a rede virtual configurada.

### <a name="create-the-resource-group"></a>Criar o grupo de recursos

Antes de poder criar uma rede virtual, tem de criar um grupo de recursos para alojar a rede virtual. Crie um grupo de recursos com [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). Este exemplo cria um grupo de recursos chamado *myResourceGroup* no *eastus* localização:

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

### <a name="create-the-virtual-network"></a>Criar a rede virtual

Crie uma rede virtual com [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Este exemplo cria uma rede de virtual predefinida com o nome *myVirtualNetwork* no *EastUS* localização:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

### <a name="add-a-subnet"></a>Adicionar uma sub-rede

Azure implementa recursos a uma sub-rede numa rede virtual, por isso terá de criar uma sub-rede. Criar uma configuração de sub-rede com o nome *predefinição* com [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name default `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

### <a name="associate-the-subnet-to-the-virtual-network"></a>Associar a sub-rede à rede virtual

Pode escrever a configuração de sub-rede para a rede virtual com [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork). Este comando cria a sub-rede:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Crie duas VMs na rede virtual.

### <a name="create-the-first-vm"></a>Criar a primeira VM

Criar a primeira VM com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Ao executar o comando seguinte, lhe forem pedidas credenciais. Introduza um nome de utilizador e palavra-passe para a VM:

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "default" `
    -Name "myVm1" `
    -AsJob
```

O `-AsJob` opção cria a VM em segundo plano. Pode continuar para o passo seguinte.

Quando o Azure começa a criar a VM em segundo plano, obterá algo parecido com isto:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM
```

### <a name="create-the-second-vm"></a>Criar a segunda VM

Crie a segunda VM com este comando:

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "default" `
  -Name "myVm2"
```

Terá de criar outro utilizador e palavra-passe. Azure demora alguns minutos para criar a VM.

> [!IMPORTANT]
> Não continue para o passo seguinte até que o Azure foi concluída.  Sabe, é feito quando ele retorna o resultado para o PowerShell.

## <a name="connect-to-a-vm-from-the-internet"></a>Ligar a uma VM a partir da Internet

Utilize [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) para que o endereço IP público de uma VM seja devolvido. Este exemplo retorna o endereço IP público dos *myVm1* VM:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Abra uma linha de comandos no seu computador local. Execute o comando `mstsc`. Substitua `<publicIpAddress>` com o endereço IP público devolvido do último passo:

> [!NOTE]
> Se esteve executando estes comandos a partir de uma linha de comandos do PowerShell no seu computador local e estão no AzureRM módulo do PowerShell versão 5.4.1 ou posterior, pode continuar nessa interface.

```cmd
mstsc /v:<publicIpAddress>
```

Um protocolo de ambiente de trabalho remoto (*. rdp*) transferências de ficheiros para o seu computador e abre-se de um ambiente de trabalho remoto.

1. Se lhe for pedido, selecione **Ligar**.

1. Introduza o nome de utilizador e a palavra-passe que especificou ao criar a VM.

    > [!NOTE]
    > Poderá ter de selecionar **mais escolhas** > **utilizar uma conta diferente**, para especificar as credenciais que introduziu quando criou a VM.

1. Selecione **OK**.

1. Poderá receber um aviso de certificado. Se o fizer, selecione **Sim** ou **continuar**.

## <a name="communicate-between-vms"></a>Comunicar entre VMs

1. Na área de trabalho remota de *myVm1*, abra o PowerShell.

1. Introduza `ping myVm2`.

    Obterá algo parecido com este back:

    ```powershell
    PS C:\Users\myVm1> ping myVm2

    Pinging myVm2.ovvzzdcazhbu5iczfvonhg2zrb.bx.internal.cloudap
    Request timed out.
    Request timed out.
    Request timed out.
    Request timed out.

    Ping statistics for 10.0.0.5:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
    ```

    O ping falhar, porque utiliza o controle de mensagem ICMP (Internet Protocol). Por predefinição, o ICMP não é permitido na sua firewall do Windows.

1. Para permitir *myVm2* fazer ping *myVm1* num passo posterior, introduzir este comando:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

    Que comando permite que o ICMP de entrada através da firewall do Windows.

1. Feche a ligação de ambiente de trabalho remoto à *myVm1*.

1. Repita os passos [ligar a uma VM a partir da internet](#connect-to-a-vm-from-the-internet). Desta vez, ligue-se ao *myVm2*.

1. Numa linha de comandos na VM *myVm2*, introduza `ping myvm1`.

    Obterá algo parecido com este back:

    ```cmd
    C:\windows\system32>ping myVm1

    Pinging myVm1.e5p2dibbrqtejhq04lqrusvd4g.bx.internal.cloudapp.net [10.0.0.4] with 32 bytes of data:
    Reply from 10.0.0.4: bytes=32 time=2ms TTL=128
    Reply from 10.0.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.0.4: bytes=32 time<1ms TTL=128

    Ping statistics for 10.0.0.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 2ms, Average = 0ms
    ```

    Vai receber respostas da *myVm1*porque permitiu o ICMP através da firewall do Windows na VM *myVm1* num passo anterior.

1. Feche a ligação de ambiente de trabalho remoto à *myVm2*.

## <a name="clean-up-resources"></a>Limpar recursos

Quando tiver terminado com a rede virtual e as VMs, utilize [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para remover o grupo de recursos e todos os recursos de que tem:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Passos Seguintes

Neste início rápido, criou uma rede virtual predefinida e duas VMs. Ligou a uma VM a partir da Internet e comunicou em privado entre a VM e outra VM. Para saber mais sobre as definições de rede virtual, veja [Gerir uma rede virtual](manage-virtual-network.md).

Azure permite a comunicação privada sem restrições entre máquinas virtuais. Por predefinição, Azure só permite ligações de ambiente de trabalho remotas entrada às VMs do Windows da internet. Para saber mais sobre como configurar diferentes tipos de comunicações de rede VM, vá para o [filtrar o tráfego de rede](tutorial-filter-network-traffic.md) tutorial.
