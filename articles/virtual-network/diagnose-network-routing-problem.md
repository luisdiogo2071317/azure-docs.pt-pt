---
title: Diagnosticar um problema de encaminhamento de máquina virtual do Azure | Microsoft Docs
description: Saiba como diagnosticar um problema de encaminhamento de máquina virtual visualizando as rotas eficazes para uma máquina virtual.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/30/2018
ms.author: jdial
ms.openlocfilehash: 07352a5d7c8b465440efab17c654979662a95f8e
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34702393"
---
# <a name="diagnose-a-virtual-machine-routing-problem"></a>Diagnosticar um problema de encaminhamento de máquina virtual

Neste artigo, irá aprender a diagnosticar um problema de encaminhamento visualizando as rotas que são eficazes para uma interface de rede numa máquina virtual (VM). O Azure cria várias rotas predefinidas para cada sub-rede de rede virtual. Pode substituir as rotas predefinidas do Azure, definir rotas numa tabela de rota e, em seguida, associar a tabela de rotas para uma sub-rede. A combinação de rotas criar, as rotas predefinidas do Azure e as rotas propagadas da sua rede no local através de um gateway de VPN do Azure (se a rede virtual está ligada à sua rede no local) através do protocolo border gateway protocol (BGP) estão a rotas efetivas para todas as interfaces de rede numa sub-rede. Se não estiver familiarizado com a rede virtual, a interface de rede ou o encaminhamento conceitos, consulte [descrição geral de rede Virtual](virtual-networks-overview.md), [interface de rede](virtual-network-network-interface.md), e [descrição geral de encaminhamento](virtual-networks-udr-overview.md).

## <a name="scenario"></a>Cenário

Se tenta estabelecer ligação a uma VM, mas a ligação falha. Para determinar por que motivo não é possível ligar à VM, pode ver as rotas eficazes para uma interface de rede através do Azure [portal](#diagnose-using-azure-portal), [PowerShell](#diagnose-using-powershell), ou o [CLI do Azure](#diagnose-using-azure-cli).

Os passos que se seguem partem do princípio de que tem uma VM existente para ver as rotas efetivas de. Se não tiver uma VM existente, primeiro implementar um [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM para concluir as tarefas neste artigo com. Os exemplos neste artigo são para uma VM chamada *myVM* com uma interface de rede com o nome *myVMVMNic*. A interface de rede e VM estão num grupo de recursos com o nome *myResourceGroup*e que estão no *EUA Leste* região. Altere os valores nos passos, conforme adequado, para a VM diagnosticar o problema para.

## <a name="diagnose-using-azure-portal"></a>Diagnosticar através do portal do Azure

1. Iniciar sessão do Azure [portal](https://portal.azure.com) com uma conta do Azure que tenha o [as permissões necessárias](virtual-network-network-interface.md#permissions).
2. Na parte superior do portal do Azure, introduza o nome de uma VM que está no estado de execução, na caixa de pesquisa. Quando o nome da VM for apresentada nos resultados da pesquisa, selecione-a.
3. Selecione **diagnosticar e resolver problemas**e, em **recomendado passos**, selecione **rotas efetivas** no item 7, conforme mostrado na imagem seguinte:

    ![Vista de rotas efetivas](./media/diagnose-network-routing-problem/view-effective-routes.png)

4. O eficaz encaminha para uma interface de rede com o nome **myVMVMNic** são apresentados na imagem seguinte:

     ![Vista de rotas efetivas](./media/diagnose-network-routing-problem/effective-routes.png)

    Se existirem várias interfaces de rede ligadas à VM, pode ver as rotas efetivas qualquer interface de rede ao selecioná-la. Uma vez que cada interface de rede pode estar numa sub-rede diferente, cada interface de rede pode ter diferentes rotas efetivas.

    No exemplo mostrado na imagem anterior, as rotas listadas são as rotas predefinidas que o Azure cria para cada sub-rede. A lista tem, pelo menos, estas rotas, mas pode ter rotas adicionais, dependendo das capacidades que pode ter ativada para a rede virtual, tais como a ser executado o peering com outra rede virtual ou ligados à sua rede no local através de um gateway de VPN do Azure. Para obter mais informações sobre cada um das rotas e a outras rotas de vir para a interface de rede, consulte [encaminhamento de tráfego de rede Virtual](virtual-networks-udr-overview.md). Se a sua lista tiver um grande número de rotas, poderá considerar mais fácil selecionar **transferir**, para transferir um ficheiro. csv com a lista de rotas.

Apesar de rotas efetivas foram visualizadas através da VM nos passos anteriores, também pode ver as rotas efetivas através de uma:
- **Interface de rede individuais**: Saiba como [ver uma interface de rede](virtual-network-network-interface.md#view-network-interface-settings).
- **Tabela de rotas individuais**: Saiba como [ver uma tabela de rota](manage-route-table.md#view-details-of-a-route-table).

## <a name="diagnose-using-powershell"></a>Diagnosticar através do PowerShell

Pode executar os comandos que se seguem na [Shell de nuvem do Azure](https://shell.azure.com/powershell), ou através da execução do PowerShell do seu computador. A Shell de nuvem do Azure é uma shell interativa livre. Tem as ferramentas comuns do Azure pré-instaladas e configuradas para utilização com a sua conta. Se executar o PowerShell a partir do seu computador, terá do *AzureRM* módulo do PowerShell, versão 6.0.1 ou posterior. Executar `Get-Module -ListAvailable AzureRM` no seu computador, para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente do PowerShell, também terá de executar `Login-AzureRmAccount` iniciar sessão no Azure com uma conta que tenha o [as permissões necessárias](virtual-network-network-interface.md#permissions).

Obter as rotas eficazes para uma interface de rede com [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable). O exemplo seguinte obtém as rotas eficazes para uma interface de rede com o nome *myVMVMNic*, que é um grupo de recursos denominado *myResourceGroup*:

```azurepowershell-interactive
Get-AzureRmEffectiveRouteTable `
  -NetworkInterfaceName myVMVMNic `
  -ResourceGroupName myResourceGroup `
  | Format-Table
```

Para compreender as informações devolvidas na saída, consulte [descrição geral de encaminhamento](virtual-networks-udr-overview.md). Resultado é devolvido apenas se a VM se encontra no estado de execução. Se existirem várias interfaces de rede ligadas à VM, pode rever as rotas em vigor para cada interface de rede. Uma vez que cada interface de rede pode estar numa sub-rede diferente, cada interface de rede pode ter diferentes rotas efetivas. Se ainda está a ter um problema de comunicação, consulte o artigo [diagnóstico adicional](#additional-diagnosis) e [considerações](#considerations).

Se não souber o nome de uma interface de rede, mas souber o nome da interface de rede está ligada à VM, os seguintes comandos devolvem os IDs de todas as interfaces de rede ligados a uma VM:

```azurepowershell-interactive
$VM = Get-AzureRmVM -Name myVM `
  -ResourceGroupName myResourceGroup
$VM.NetworkProfile
```

Poderá recebe um resultado semelhante ao seguinte exemplo:

```powershell
NetworkInterfaces
-----------------
{/subscriptions/<ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic
```

Na saída anterior, o nome de interface de rede está *myVMVMNic*.

## <a name="diagnose-using-azure-cli"></a>Diagnosticar utilizando a CLI do Azure

Pode executar os comandos que se seguem na [Shell de nuvem do Azure](https://shell.azure.com/bash), ou executando a CLI do seu computador. Este artigo requer a CLI do Azure versão 2.0.32 ou posterior. Execute `az --version` para localizar a versão instalada. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli). Se estiver a executar localmente a CLI do Azure, também terá de executar `az login` e iniciar sessão no Azure com uma conta que tenha o [as permissões necessárias](virtual-network-network-interface.md#permissions).

Obter as rotas eficazes para uma interface de rede com [nic Mostrar-eficaz--a tabela de rotas de rede az](/cli/azure/network/nic#az-network-nic-show-effective-route-table). O exemplo seguinte obtém as rotas eficazes para uma interface de rede com o nome *myVMVMNic* que está a ser um grupo de recursos denominado *myResourceGroup*:

```azurecli-interactive
az network nic show-effective-route-table \
  --name myVMVMNic \
  --resource-group myResourceGroup
```

Para compreender as informações devolvidas na saída, consulte [descrição geral de encaminhamento](virtual-networks-udr-overview.md). Resultado é devolvido apenas se a VM se encontra no estado de execução. Se existirem várias interfaces de rede ligadas à VM, pode rever as rotas em vigor para cada interface de rede. Uma vez que cada interface de rede pode estar numa sub-rede diferente, cada interface de rede pode ter diferentes rotas efetivas. Se ainda está a ter um problema de comunicação, consulte o artigo [diagnóstico adicional](#additional-diagnosis) e [considerações](#considerations).

Se não souber o nome de uma interface de rede, mas souber o nome da interface de rede está ligada à VM, os seguintes comandos devolvem os IDs de todas as interfaces de rede ligados a uma VM:

```azurecli-interactive
az vm show \
  --name myVM \
  --resource-group myResourceGroup
```

## <a name="resolve-a-problem"></a>Resolver um problema

Resolver problemas de encaminhamento, normalmente, é composta por:

- Adicionar uma rota personalizada para substituir uma das rotas predefinidas do Azure. Saiba como [adicione uma rota personalizada](manage-route-table.md#create-a-route).
- Alterar ou remover uma rota personalizada que pode causar encaminhamento para uma localização indesejada. Saiba como [alterar](manage-route-table.md#change-a-route) ou [eliminar](manage-route-table.md#delete-a-route) uma rota personalizada.
- Garantir que a tabela de rota que contém as rotas personalizadas que definiu está associado à sub-rede da que interface de rede está a ser. Saiba como [associar uma tabela de rota para uma sub-rede](manage-route-table.md#associate-a-route-table-to-a-subnet).
- Garantir que os dispositivos, como de VPN do Azure rede ou de gateway de aplicações virtuais implementadas estão operacional. Utilize o [diagnóstico VPN](../network-watcher/diagnose-communication-problem-between-networks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) capacidade do observador de rede para determinar a quaisquer problemas com um gateway de VPN do Azure.

Se ainda está a ter problemas de comunicação, consulte o artigo [considerações](#considerations) e [diagnóstico adicional](#additional-dignosis).

## <a name="considerations"></a>Considerações

Quando a resolução de problemas de comunicação, considere os seguintes pontos:

- Encaminhamento baseia-se na maior correspondência de prefixo (LPM) entre as rotas que já definiu, as rotas border gateway protocol (BGP) e do sistema. Se houver mais do que uma rota com a mesma correspondência LPM, em seguida, uma rota é selecionada com base na origem pela ordem listada no [descrição geral de encaminhamento](virtual-networks-udr-overview.md#how-azure-selects-a-route). Com rotas efetivas, pode ver apenas as rotas efetivas que sejam uma correspondência LPM, com base em todas as rotas disponíveis. Ver a forma como as rotas são avaliadas para uma interface de rede torna muito mais fácil solucionar rotas específicas que poderão estar a afetar a comunicação de VM.
- Se tiver definido as rotas personalizadas para uma aplicação virtual de rede (NVA), com *aplicação Virtual* como o tipo de salto seguinte, certifique-se de que o reencaminhamento IP está ativado no NVA receber o tráfego de ou pacotes serem largados. Saiba mais sobre [ativar o reencaminhamento IP para uma interface de rede](virtual-network-network-interface.md#enable-or-disable-ip-forwarding). Além disso, o sistema operativo ou aplicação dentro a NVA deve também conseguir encaminhar o tráfego de rede e de ser configurado para fazê-lo.
- Se tiver criado uma rota para o 0.0.0.0/0, todo o tráfego de saída da internet é encaminhado para o salto seguinte especificado, tal como um gateway NVA ou VPN. Criar uma rota deste tipo é frequentemente referido como imposição do túnel. Ligações remotas utilizando os protocolos RDP ou SSH através da internet para a VM podem não funcionar com esta rota, dependendo de como o próximo salto processa o tráfego. Túnel forçado pode ser ativado:
    - Ao utilizar VPN de site a site, criando uma rota com um tipo de próximo salto de *Gateway de VPN*. Saiba mais sobre [configurar imposição do túnel](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    - Se um 0.0.0.0/0 (rota predefinida) é anunciada através de BGP através de um gateway de rede virtual ao utilizar uma VPN de site para site ou do circuito ExpressRoute. Saiba mais sobre como utilizar o BGP com um [VPN site a site](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou [ExpressRoute](../expressroute/expressroute-routing.md?toc=%2fazure%2fvirtual-network%2ftoc.json#ip-addresses-used-for-azure-private-peering).
- Para tráfego peering de rede virtual funcionar corretamente, uma rota de sistema com um tipo de próximo salto de *VNet Peering* tem de existir durante o intervalo de prefixo da rede virtual em modo de peering. Se não existe uma rota deste tipo, não sendo a ligação de peering de rede virtual **ligado**:
    - Aguarde alguns segundos e tente novamente. Se se tratar de uma ligação de peering recentemente estabelecida, ocasionalmente demora mais para propagar rotas para todas as interfaces de rede numa sub-rede. Para obter mais informações sobre peering de rede virtual, consulte o artigo [descrição geral de peering da rede Virtual](virtual-network-peering-overview.md) e [gerir peering de rede virtual](virtual-network-manage-peering.md).
    - Regras do grupo de segurança de rede podem estar a afetar comunicação. Para obter mais informações, consulte [diagnosticar um problema de filtro de tráfego de rede de máquina virtual](diagnose-network-traffic-filter-problem.md).
- Embora o Azure atribui rotas predefinidas para cada interface de rede do Azure, se tiver várias interfaces de rede ligadas à VM, a interface de rede primária é atribuído uma rota predefinida (0.0.0.0/0) ou o gateway, no sistema de operativo da VM. Saiba como criar uma rota predefinida para interfaces de rede secundárias ligado a um [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) ou [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) VM. Saiba mais sobre [interfaces de rede principal e secundário](virtual-network-network-interface-vm.md#constraints).

## <a name="additional-diagnosis"></a>Diagnóstico adicional

* Para executar um teste rápido para determinar o tipo de salto seguinte para o tráfego destinado a uma localização, utilize o [próximo salto](../network-watcher/diagnose-vm-network-routing-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json) capacidade do observador de rede do Azure. Próximo salto indica que o tipo de salto seguinte é para o tráfego destinado à localização especificada.
* Se não houver nenhuma rotas fazendo com que a comunicação de rede de uma VM falhar, o problema pode dever-se o software de firewall em execução no sistema de operativo da VM
* Se estiver [imposição de túnel](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tráfego para um dispositivo no local através de um gateway de VPN ou NVA, poderá não conseguir ligar a uma VM a partir da internet, dependendo de como tiver configurado o encaminhamento para os dispositivos. Certifique-se de que o que configurou para o dispositivo de encaminhamento encaminha o tráfego para qualquer um endereço IP público ou privado para a VM.
* Utilize o [resolver problemas de ligação](../network-watcher/network-watcher-connectivity-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) capacidade do observador de rede para determinar o encaminhamento, filtragem e no SO causas dos problemas de comunicação de saída.

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre todas as tarefas, propriedades e definições para um [rotas e tabela de rotas](manage-route-table.md).
- Saiba mais sobre todos os [próximo salto tipos, rotas de sistema e como o Azure seleciona uma rota](virtual-networks-udr-overview.md).
