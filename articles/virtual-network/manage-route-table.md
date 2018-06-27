---
title: Criar, alterar ou eliminar uma tabela de rota do Azure | Microsoft Docs
description: Saiba como criar, alterar ou eliminar uma tabela de rota.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: jdial
ms.openlocfilehash: cd97b00a522ff41a74f46195da5d8b1a0d92d344
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960013"
---
# <a name="create-change-or-delete-a-route-table"></a>Criar, alterar ou eliminar uma tabela de rota

Azure automaticamente encaminha o tráfego entre sub-redes do Azure, redes virtuais e redes no local. Se pretender alterar qualquer uma das predefinido do Azure encaminhamento, fazê-lo através da criação de uma tabela de rota. Se estiver familiarizado com o encaminhamento na redes virtuais, pode saber mais acerca do mesmo no [descrição geral do encaminhamento](virtual-networks-udr-overview.md) ou concluindo um [tutorial](tutorial-create-route-table-portal.md).

## <a name="before-you-begin"></a>Antes de começar

Conclua as seguintes tarefas antes de concluir os passos em qualquer secção deste artigo:

- Se ainda não tiver uma conta do Azure, inscreva-se um [conta de avaliação gratuita](https://azure.microsoft.com/free).
- Se utilizar o portal, abra https://portal.azure.come inicie sessão com a sua conta do Azure.
- Se utilizar comandos do PowerShell para concluir tarefas neste artigo, quer executar os comandos [Shell de nuvem do Azure](https://shell.azure.com/powershell), ou através da execução do PowerShell do seu computador. O Azure Cloud Shell é um shell interativo gratuito que pode utilizar para executar os passos neste artigo. Tem as ferramentas comuns do Azure pré-instaladas e configuradas para utilização com a sua conta. Este tutorial requer a versão do módulo 5.7.0 ou posterior do Azure PowerShell. Execute `Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Connect-AzureRmAccount` para criar uma ligação com o Azure.
- Se utilizar comandos de interface de linha de comandos (CLI) do Azure para concluir tarefas neste artigo, quer executar os comandos [Shell de nuvem do Azure](https://shell.azure.com/bash), ou executando a CLI do seu computador. Este tutorial requer a CLI do Azure versão 2.0.31 ou posterior. Execute `az --version` para localizar a versão instalada. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli). Se estiver a executar localmente a CLI do Azure, também terá de executar `az login` para criar uma ligação com o Azure.

A conta iniciar sessão ou ligar ao Azure, tem de ser atribuída para a [contribuinte de rede](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) função ou a um [função personalizada](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atribuída as ações adequadas listadas na [permissões ](#permissions).

## <a name="create-a-route-table"></a>Criar uma tabela de rotas

Não há um limite para quantas tabelas de rota, pode criar por localização do Azure e subscrição. Para obter mais detalhes, veja [Limites do Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

1. No canto superior esquerdo do portal, selecione **+ criar um recurso**.
2. Selecione **redes**, em seguida, selecione **tabela de rotas**.
3. Introduza um **nome** para a tabela de rotas, selecione o **subscrição**, crie um novo **grupo de recursos**, ou selecione um grupo de recursos existente, selecione um **localização** , em seguida, selecione **criar**. O **propagação de rota BGP desativar** opção impede rotas no local, de que está a ser propagados por BGP para as interfaces de rede em nenhuma sub-rede que está associada a tabela de rotas. Se a rede virtual não está ligada a um gateway de rede do Azure (VPN ou ExpressRoute), deixe a opção *desativado*.

**Comandos**

- CLI do Azure: [criar tabela de rotas do rede az](/cli/azure/network/route-table/route#az_network_route_table_create)
- PowerShell: [New-AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable)

## <a name="view-route-tables"></a>Tabelas de rotas de vista

Na caixa de pesquisa na parte superior do portal, introduza *tabelas de rotas* na caixa de pesquisa. Quando **tabelas de rotas** aparecer nos resultados da pesquisa, selecione-o. São listadas entre as tabelas de rota que existem na sua subscrição.

**Comandos**

- CLI do Azure: [lista de tabela de rotas de rede az](/cli/azure/network/route-table/route#az_network_route_table_list)
- PowerShell: [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable)

## <a name="view-details-of-a-route-table"></a>Ver detalhes de uma tabela de rota

1. Na caixa de pesquisa na parte superior do portal, introduza *tabelas de rotas* na caixa de pesquisa. Quando **tabelas de rotas** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione a tabela de rotas na lista que pretende ver detalhes. Em **definições**, pode ver o **rotas** na tabela de rota e a **sub-redes** a tabela de rotas é associada.
3. Para saber mais sobre as definições do Azure comuns, consulte as seguintes informações:
    *   [Registo de atividades](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
    *   [Controlo de acesso (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
    *   [Etiquetas](../azure-resource-manager/resource-group-using-tags.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Bloqueia](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Script de automatização](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Comandos**

- CLI do Azure: [mostrar de tabela de rotas de rede az](/cli/azure/network/route-table/route#az_network_route_table_show)
- PowerShell: [Get-AzureRmRouteTable](/powershell/module/azurerm.network/get-azurermroutetable)

## <a name="change-a-route-table"></a>Alterar uma tabela de rota

1. Na caixa de pesquisa na parte superior do portal, introduza *tabelas de rotas* na caixa de pesquisa. Quando **tabelas de rotas** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione a tabela de rota que pretende alterar. As alterações mais comuns são [adicionar](#create-a-route) ou [remover](#delete-a-route) rotas e [associar](#associate-a-route-table-to-a-subnet) tabelas, de rotas ou [desassociar](#dissociate-a-route-table-from-a-subnet) encaminhar tabelas a partir da sub-redes.

**Comandos**

- CLI do Azure: [atualização de tabela de rotas de rede az](/cli/azure/network/route-table/route#az_network_route_table_update)
- PowerShell: [Set-AzureRmRouteTable](/powershell/module/azurerm.network/set-azurermroutetable)

## <a name="associate-a-route-table-to-a-subnet"></a>Associar uma tabela de rotas a uma sub-rede

Uma sub-rede pode ter zero ou uma tabela de rotas associada ao mesmo. Uma tabela de rota pode ser associada a zero ou várias sub-redes. Uma vez que as tabelas de rotas não estão associadas às redes virtuais, tem de associar uma tabela de rota para cada sub-rede que pretende que a tabela de rotas associada. Todo o tráfego que sai da sub-rede é encaminhado com base nas rotas criou dentro de tabelas de rota, [predefinido rotas](virtual-networks-udr-overview.md#default), e as rotas propagadas a partir de uma rede no local, se a rede virtual está ligada a um (gateway de rede virtual do Azure ExpressRoute, ou uma VPN, se utilizar o BGP com um gateway de VPN). Só pode associar uma tabela de rotas para sub-redes em redes virtuais que existem na mesma localização do Azure e subscrição enquanto a tabela de rotas.

1. Na caixa de pesquisa na parte superior do portal, introduza *redes virtuais* na caixa de pesquisa. Quando **redes virtuais** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione a rede virtual na lista que contém a sub-rede que pretende associar uma tabela de rota para.
3. Selecione **sub-redes** em **definições**.
4. Selecione a sub-rede que pretende associar a tabela de rotas para.
5. Selecione **tabela de rotas**, selecione de que pretende associar à sub-rede, em seguida, selecione a tabela de rotas **guardar**.

Se a rede virtual esiver ligada a um gateway VPN do Azure, não associe nenhuma tabela de rota à [sub-rede do gateway](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub) que incluia uma rota com um destino 0.0.0.0/0. Se o fizer, poderá impedir que o gateway funcione corretamente. Para obter mais informações sobre como utilizar 0.0.0.0/0 numa rota, consulte [encaminhamento de tráfego de rede Virtual](virtual-networks-udr-overview.md#default-route).

**Comandos**

- CLI do Azure: [atualização de sub-rede da vnet de rede de az](/cli/azure/network/vnet/subnet?view=azure-cli-latest#az_network_vnet_subnet_update)
- PowerShell: [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)

## <a name="dissociate-a-route-table-from-a-subnet"></a>Desassociar a uma tabela de rota de sub-rede

Ao desassociar a uma tabela de rota de sub-rede, Azure encaminha o tráfego com base no respetivo [predefinido rotas](virtual-networks-udr-overview.md#default).

1. Na caixa de pesquisa na parte superior do portal, introduza *redes virtuais* na caixa de pesquisa. Quando **redes virtuais** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione a rede virtual que contém a sub-rede que pretende desassociar a partir de uma tabela de rota.
3. Selecione **sub-redes** em **definições**.
4. Selecione a sub-rede que pretende desassociar a tabela de rota do.
5. Selecione **tabela de rotas**, selecione **nenhum**, em seguida, selecione **guardar**.

**Comandos**

- CLI do Azure: [atualização de sub-rede da vnet de rede de az](/cli/azure/network/vnet/subnet?view=azure-cli-latest#az_network_vnet_subnet_update)
- PowerShell: [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) 

## <a name="delete-a-route-table"></a>Eliminar uma tabela de rota

Se uma tabela de rota está associada a quaisquer sub-redes, não pode ser eliminada. [Desassociar](#dissociate-a-route-table-from-a-subnet) uma tabela de rota de todas as sub-redes antes de tentar eliminá-lo.

1. Na caixa de pesquisa na parte superior do portal, introduza *tabelas de rotas* na caixa de pesquisa. Quando **tabelas de rotas** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione **...**  no lado direito da tabela de rota que pretende eliminar.
3. Selecione **eliminar**e, em seguida, selecione **Sim**.

**Comandos**

- CLI do Azure: [eliminar a tabela de rotas do rede az](/cli/azure/network/route-table/route#az_network_route_table_delete)
- PowerShell: [Delete-AzureRmRouteTable](/powershell/module/azurerm.network/delete-azurermroutetable) 

## <a name="create-a-route"></a>Criar uma rota

Não há um limite para quantas rotas por tabela de rotas podem criar por localização do Azure e subscrição. Para obter mais detalhes, veja [Limites do Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

1. Na caixa de pesquisa na parte superior do portal, introduza *tabelas de rotas* na caixa de pesquisa. Quando **tabelas de rotas** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione a tabela de rota da lista que pretende adicionar uma rota para.
3. Selecione **rotas**, em **definições**.
4. Selecione **+ Adicionar**.
5. Introduza um único **nome** da rota na tabela de rota.
6. Introduza o **prefixo de endereço**, na notação CIDR, o que pretende para encaminhar o tráfego para. O prefixo não pode ser duplicado na rota mais do que uma tabela de rotas, embora possam ser o prefixo dentro de outro prefixo. Por exemplo, se definidas 10.0.0.0/16 como um prefixo numa rota, pode definir outra rota com o prefixo de endereço 10.0.0.0/24. Azure seleciona uma rota para o tráfego com base na correspondência de prefixo mais longo. Para obter mais informações sobre como o Azure seleciona rotas, consulte o artigo [descrição geral de encaminhamento](virtual-networks-udr-overview.md#how-azure-selects-a-route).
7. Selecione um **próximo salto tipo**. Para obter uma descrição detalhada de todos os tipos de salto seguintes, consulte [descrição geral de encaminhamento](virtual-networks-udr-overview.md).
8. Introduza um endereço IP para **endereço do próximo salto**. Apenas pode introduzir um endereço se tiver selecionado *aplicação Virtual* para **próximo salto tipo**.
9. Selecione **OK**.

**Comandos**

- CLI do Azure: [criar rota de tabela de rotas de rede az](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_create)
- PowerShell: [New-AzureRmRouteConfig](/powershell/module/azurerm.network/new-azurermrouteconfig)

## <a name="view-routes"></a>Rotas de vista

Uma tabela de rota contém zero ou várias rotas. Para saber mais sobre as informações apresentadas quando visualizar as rotas, consulte [descrição geral de encaminhamento](virtual-networks-udr-overview.md).

1. Na caixa de pesquisa na parte superior do portal, introduza *tabelas de rotas* na caixa de pesquisa. Quando **tabelas de rotas** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione a tabela de rota da lista que pretende ver rotas para.
3. Selecione **rotas** em **definições**.

**Comandos**

- CLI do Azure: [lista de rota da tabela de rotas de rede de az](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_list)
- PowerShell: [Get-AzureRmRouteConfig](/powershell/module/azurerm.network/get-azurermrouteconfig)

## <a name="view-details-of-a-route"></a>Ver detalhes de uma rota

1. Na caixa de pesquisa na parte superior do portal, introduza *tabelas de rotas* na caixa de pesquisa. Quando **tabelas de rotas** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione a tabela de rota que pretende ver os detalhes de uma rota para.
3. Selecione **rotas**.
4. Selecione a rota de que pretende ver os detalhes da.

**Comandos**

- CLI do Azure: [mostrar de rota de tabela de rotas de rede az](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_show)
- PowerShell: [Get-AzureRmRouteConfig](/powershell/module/azurerm.network/get-azurermrouteconfig)

## <a name="change-a-route"></a>Alterar uma rota

1. Na caixa de pesquisa na parte superior do portal, introduza *tabelas de rotas* na caixa de pesquisa. Quando **tabelas de rotas** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione a tabela de rota que pretende alterar uma rota para.
3. Selecione **rotas**.
4. Selecione a rota de que pretende alterar.
5. Alterar as definições existentes para as novas definições, em seguida, selecione **guardar**.

**Comandos**

- CLI do Azure: [atualização de rota da tabela de rotas de rede de az](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_update)
- PowerShell: [Set-AzureRmRouteConfig](/powershell/module/azurerm.network/set-azurermrouteconfig)

## <a name="delete-a-route"></a>Eliminar uma rota

1. Na caixa de pesquisa na parte superior do portal, introduza *tabelas de rotas* na caixa de pesquisa. Quando **tabelas de rotas** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione a tabela de rota que pretende eliminar uma rota para.
3. Selecione **rotas**.
4. Na lista de rotas, selecione **...**  no lado direito da rota que pretende eliminar.
5. Selecione **eliminar**, em seguida, selecione **Sim**.

**Comandos**

- CLI do Azure: [delete de rota de tabela de rotas de rede az](/cli/azure/network/route-table/route?view=azure-cli-latest#az_network_route_table_route_delete)
- PowerShell: [Remove-AzureRmRouteConfig](/powershell/module/azurerm.network/remove-azurermrouteconfig)

## <a name="view-effective-routes"></a>Vista de rotas efetivas

As rotas em vigor para cada interface de rede ligado a uma máquina virtual são uma combinação de tabelas de rota que criou, as rotas predefinidas do Azure e as rotas propagadas da redes no local através do BGP através de um gateway de rede virtual do Azure. Compreender as rotas efetivas de uma interface de rede é útil quando a resolução de problemas de encaminhamento. Pode ver as rotas efetivas qualquer interface de rede que está ligado a uma máquina virtual em execução.

1. Na caixa de pesquisa na parte superior do portal, introduza o nome de uma máquina virtual que pretende visualizar as rotas efetivas para. Se não souber o nome de uma máquina virtual, introduza *máquinas virtuais* na caixa de pesquisa. Quando **máquinas virtuais** aparecer nos resultados da pesquisa, selecione-o e selecione uma máquina virtual a partir da lista.
2. Selecione **redes** em **definições**.
3. Selecione o nome de uma interface de rede.
4. Selecione **rotas efetivas** em **suporte + resolução de problemas**.
5. Reveja a lista de rotas efetivas para determinar se a rota correta existe para encaminhar o tráfego para onde pretende. Saiba mais sobre os tipos de salto seguintes que vê nesta lista no [descrição geral de encaminhamento](virtual-networks-udr-overview.md).

**Comandos**

- CLI do Azure: [az nic Mostrar-eficaz--a tabela de rotas de rede](/cli/azure/network/nic?view=azure-cli-latest#az_network_nic_show_effective_route_table)
- PowerShell: [Get-AzureRmEffectiveRouteTable](/powershell/module/azurerm.network/get-azurermeffectiveroutetable) 

## <a name="validate-routing-between-two-endpoints"></a>Validar o encaminhamento entre dois pontos finais

Pode determinar o tipo de próximo salto entre uma máquina virtual e o endereço IP de outro recurso do Azure, um recurso no local ou um recurso na Internet. O do Azure ao determinar encaminhamento é útil quando a resolução de problemas de encaminhamento. Para concluir esta tarefa, tem de ter um observador de rede existente. Se não tiver um observador de rede existente, crie um seguindo os passos no [criar uma instância de observador de rede](../network-watcher/network-watcher-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

1. Na caixa de pesquisa na parte superior do portal, introduza *observador de rede* na caixa de pesquisa. Quando a opção **Observador de Rede** aparecer nos resultados de pesquisa, selecione-a.
2. Selecione **próximo salto** em **as ferramentas de diagnóstico de rede**.
3. Selecione o **subscrição** e **grupo de recursos** da máquina virtual de origem que pretende validar o encaminhamento do.
4. Selecione o **Máquina Virtual**, **interface de rede** anexado à máquina virtual, e **endereço IP de origem** atribuído à interface de rede que pretende validar encaminhamento do.
5. Introduza o **endereço IP de destino** que pretende validar o encaminhamento para.
6. Selecione **próximo salto**.
7. Após uma espera curta, são devolvidas informações que indica o tipo de salto seguinte e o ID da rota que encaminhados o tráfego. Saiba mais sobre próximos salto tipos que visualiza devolvidos em [descrição geral de encaminhamento](virtual-networks-udr-overview.md).

**Comandos**

- CLI do Azure: [az Mostrar-next-hop do observador de rede](/cli/azure/network/watcher?view=azure-cli-latest#az_network_watcher_show_next_hop)
- PowerShell: [Get-AzureRmNetworkWatcherNextHop](/powershell/module/azurerm.network/get-azurermnetworkwatchernexthop) 

## <a name="permissions"></a>Permissões

Para efetuar tarefas em tabelas de rota e as rotas, deve ser atribuída à conta para o [contribuinte de rede](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) função ou a um [personalizado](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) função atribuída as ações adequadas listadas na seguinte tabela:

| Ação                                                          |   Nome                                                  |
|--------------------------------------------------------------   |   -------------------------------------------           |
| Microsoft.Network/routeTables/read                              |   Ler uma tabela de rota                                    |
| Microsoft.Network/routeTables/write                             |   Criar ou atualizar uma tabela de rota                        |
| Microsoft.Network/routeTables/delete                            |   Eliminar uma tabela de rota                                  |
| Microsoft.Network/routeTables/join/action                       |   Associar uma tabela de rotas a uma sub-rede                   |
| Microsoft.Network/routeTables/routes/read                       |   Ler uma rota                                          |
| Microsoft.Network/routeTables/routes/write                      |   Criar ou atualizar uma rota                              |
| Microsoft.Network/routeTables/routes/delete                     |   Eliminar uma rota                                        |
| Microsoft.Network/networkInterfaces/effectiveRouteTable/action  |   Obter a tabela de rotas efetivas para uma interface de rede |
| Microsoft.Network/networkWatchers/nextHop/action                |   Obtém o salto seguinte a partir de uma VM                           |

## <a name="next-steps"></a>Passos Seguintes

- Criar uma tabela de rota utilizando [PowerShell](powershell-samples.md) ou [CLI do Azure](cli-samples.md) scripts ou utilizar o Azure de exemplo [modelos do Resource Manager](template-samples.md)
- Criar e aplicar [política do Azure](policy-samples.md) para redes virtuais
