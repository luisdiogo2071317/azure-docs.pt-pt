---
title: Criar uma rede virtual no Azure - PowerShell | Microsoft Docs
description: "Saiba mais rapidamente criar uma rede virtual com o PowerShell. Uma rede virtual permite que vários tipos de recursos do Azure em privado comuniquem entre si."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: 
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 01/25/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 22fcdbda85f3ea336c3926e04d408935ed069c25
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="create-a-virtual-network-using-powershell"></a>Criar uma rede virtual com o PowerShell

Neste artigo, irá aprender a criar uma rede virtual. Depois de criar uma rede virtual, implementar duas máquinas virtuais para a rede virtual para testar a comunicação de rede privada entre eles.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este artigo requer o AzureRM PowerShell versão do módulo 5.1.1 ou posterior. Para localizar a versão instalada, execute ` Get-Module -ListAvailable AzureRM`. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos do Azure com [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos. O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*. Todos os recursos do Azure são criados dentro de uma localização do Azure (ou região).

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

Crie uma rede virtual com [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). O exemplo seguinte cria uma rede virtual da predefinição denominada *myVirtualNetwork* no *EastUS* localização:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/24
```

Todas as redes virtuais têm um ou mais os prefixos de endereço atribuídos aos mesmos. Foi especificado o espaço de endereços em notação CIDR. O endereço espaço 10.0.0.0/24 abrange 10.0.0.0-10.0.0.254. Redes virtuais tem zero ou mais sub-redes dentro delas. Recursos são implementados para uma sub-rede numa rede virtual. 

Criar uma configuração de sub-rede com [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Todas as sub-redes que têm um prefixo de endereço que existe no prefixo do endereço de rede virtual. Neste exemplo, é criada uma configuração de sub-rede com o mesmo prefixo de endereço como prefixo de endereço a rede virtual:

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name default `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

Embora o prefixo de endereço de sub-rede abrange 10.0.0.0-10.0.0.254, apenas o 10.0.0.4-10.0.0.254 endereços estão disponíveis, porque o Azure reserva-se os endereços de quatro primeiras (0-3) e o último endereço em cada sub-rede. Uma vez que o prefixo de endereço de sub-rede é o mesmo que o prefixo de endereço de rede virtual, pode existir apenas uma sub-rede nesta rede virtual.

A configuração de sub-rede de escrita para a rede virtual com [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork), que cria a sub-rede:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="test-network-communication"></a>Comunicação de rede de teste

Uma rede virtual permite que vários tipos de recursos do Azure em privado comuniquem entre si. Um tipo de recurso que pode implementar numa rede virtual é uma máquina virtual. Crie duas máquinas virtuais na rede virtual para que possa validar privada comunicação entre os mesmos num passo posterior.

### <a name="create-virtual-machines"></a>Criar máquinas virtuais

Criar uma máquina virtual com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Ao executar este passo, serão pedidas credenciais. Os valores que introduzir são configurados, como o nome de utilizador e a palavra-passe para a máquina virtual. A localização em que uma máquina virtual é criada na tem de ser a mesma localização que da rede virtual existe. A máquina virtual não é necessário para estar no mesmo grupo de recursos que a máquina virtual, embora seja neste artigo. O `-AsJob` parâmetro permite que o comando a executar em segundo plano, para poder continuar com a seguinte tarefa.

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "default" `
    -Name "myVm1" `
    -AsJob
```

Resultado semelhante ao seguinte exemplo de saída é devolvido e Azure começa a criação da máquina virtual em segundo plano.

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command                  
--     ----            -------------   -----         -----------     --------             -------                  
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM     
```

Azure DHCP atribui automaticamente 10.0.0.4 à máquina virtual durante a criação, porque é o primeiro endereço disponível no *predefinido* sub-rede.

Crie uma segunda máquina virtual. 

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "default" `
  -Name "myVm2"
```
A máquina virtual demora alguns minutos a criar. Depois de criado, o Azure devolve resultado sobre a máquina virtual criada. Embora não se encontra na saída devolvida, Azure atribuído *10.0.0.5* para o *myVm2* máquina virtual, porque foi o seguinte endereço disponível na sub-rede.

### <a name="connect-to-a-virtual-machine"></a>Ligar a uma máquina virtual

Utilize o [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) comando para devolver o endereço IP público de uma máquina virtual. Azure atribui um endereço público, para o Internet encaminhável IP para cada máquina virtual, por predefinição. O endereço IP público é atribuído à máquina virtual de um [conjunto de endereços atribuídos a cada região do Azure](https://www.microsoft.com/download/details.aspx?id=41653). Enquanto o Azure sabe qual o endereço IP público é atribuído a uma máquina virtual, o sistema operativo executado numa máquina virtual tem não conhecimento de qualquer endereço IP público atribuído. O exemplo seguinte devolve o endereço IP público do *myVm1* máquina virtual:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress -Name myVm1 -ResourceGroupName myResourceGroup | Select IpAddress
```

Utilize o seguinte comando para criar uma sessão de ambiente de trabalho remota com o *myVm1* máquina virtual do seu computador local. Substitua `<publicIpAddress>` com o endereço IP devolvido do comando anterior.

```
mstsc /v:<publicIpAddress>
```

Um ficheiro de protocolo de ambiente de trabalho remoto (RDP) é criado, transferido para o computador e aberto. Introduza o nome de utilizador e palavra-passe que especificou ao criar a máquina virtual e, em seguida, clique em **OK**. Poderá receber um aviso de certificado durante o processo de início de sessão. Clique em **Sim** ou **Continuar** para continuar com a ligação.

### <a name="validate-communication"></a>Validar a comunicação

A tentar enviar um ping uma falha de máquina virtual do Windows, porque não é permitido ping através da firewall do Windows, por predefinição. Para permitir que um ping para *myVm1*, introduza o seguinte comando numa linha de comandos:

```
netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
```

Para validar a comunicação com *myVm2*, introduza o seguinte comando numa linha de comandos *myVm1* máquina virtual. Forneça as credenciais que utilizou quando criou a máquina virtual e, em seguida, conclua a ligação:

```
mstsc /v:myVm2
```

A ligação de ambiente de trabalho remota é efetuada com êxito porque as duas máquinas virtuais têm endereços IP privados atribuídos a partir do *predefinido* sub-rede e porque o ambiente de trabalho remoto está aberta através da firewall do Windows, por predefinição. É possível ligar ao *myVm2* pelo nome de anfitrião porque o Azure oferece automaticamente resolução do nome DNS de todos os anfitriões numa rede virtual. Numa linha de comandos, um ping a minha *myVm1*, de *myVm2*.

```
ping myvm1
```

Ping for bem sucedido porque permitido através da firewall do Windows no *myVm1* máquina virtual num passo anterior. Para confirmar a comunicação de saída à Internet, introduza o seguinte comando:

```
ping bing.com
```

Receber quatro respostas bing.com. Por predefinição, qualquer máquina virtual numa rede virtual podem comunicar saída à Internet.

Sair da sessão de ambiente de trabalho remota. 

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, pode utilizar o [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) comando para remover o grupo de recursos e todos os recursos que contém:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, implementou uma predefinição a rede virtual com uma sub-rede. Para saber como criar uma rede virtual personalizada com várias sub-redes, avance para o tutorial para criar uma rede virtual personalizada.

> [!div class="nextstepaction"]
> [Criar uma rede virtual personalizada](virtual-networks-create-vnet-arm-ps.md)
