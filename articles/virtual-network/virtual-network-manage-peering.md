---
title: Criar, alterar ou eliminar um peering de rede virtual do Azure | Microsoft Docs
description: Saiba como criar, alterar ou eliminar um peering de rede virtual.
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
ms.author: jdial;anavin
ms.openlocfilehash: 773bfd3c4106bead85542970b3d27fd5dad25049
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/01/2018
---
# <a name="create-change-or-delete-a-virtual-network-peering"></a>Criar, alterar ou eliminar um peering de rede virtual

Saiba como criar, alterar ou eliminar um peering de rede virtual. Peering de rede virtual permite-lhe ligar redes virtuais através da rede principal do Azure. Uma vez executado o peering, as redes virtuais ainda são geridas como recursos separados. Se não estiver familiarizado com o peering de rede virtual, recomendamos a leitura a [descrição geral de peering da rede Virtual](virtual-network-peering-overview.md) e concluir o [criar um tutorial de peering de rede virtual](tutorial-connect-virtual-networks-portal.md), antes de concluir o tarefas neste artigo.

## <a name="before-you-begin"></a>Antes de começar

Conclua as seguintes tarefas antes de concluir os passos em qualquer secção deste artigo:

- Se ainda não tiver uma conta do Azure, inscreva-se um [conta de avaliação gratuita](https://azure.microsoft.com/free).
- Se utilizar o portal, abra https://portal.azure.come inicie sessão com uma conta que tenha o [as permissões necessárias](#permissions) para trabalhar com peerings.
- Se utilizar comandos do PowerShell para concluir tarefas neste artigo, quer executar os comandos [Shell de nuvem do Azure](https://shell.azure.com/powershell), ou através da execução do PowerShell do seu computador. O Azure Cloud Shell é um shell interativo gratuito que pode utilizar para executar os passos neste artigo. Tem as ferramentas comuns do Azure pré-instaladas e configuradas para utilização com a sua conta. Este tutorial requer o Azure PowerShell versão do módulo 5.5.0 ou posterior. Execute `Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente do PowerShell, também terá de executar `Connect-AzureRmAccount` com uma conta que tenha o [as permissões necessárias](#permissions) para trabalhar com o peering, para criar uma ligação com o Azure.
- Se utilizar comandos de interface de linha de comandos (CLI) do Azure para concluir tarefas neste artigo, quer executar os comandos [Shell de nuvem do Azure](https://shell.azure.com/bash), ou executando a CLI do seu computador. Este tutorial requer a CLI do Azure versão 2.0.29 ou posterior. Execute `az --version` para localizar a versão instalada. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli). Se estiver a executar localmente a CLI do Azure, também terá de executar `az login` com uma conta que tenha o [as permissões necessárias](#permissions) para trabalhar com o peering, para criar uma ligação com o Azure.

## <a name="create-a-peering"></a>Criar um peering

Antes de criar um peering, familiarize-se com o [requisitos e restrições](#requirements-and-contstraints) e [as permissões necessárias](#permissions).

1. Na caixa de pesquisa na parte superior do portal do Azure, introduza *redes virtuais* na caixa de pesquisa. Quando **redes virtuais** aparece nos resultados da pesquisa, selecionados-lo. Não selecione **redes virtuais (clássicas)** se aparece na lista, como não é possível criar um peering de uma rede virtual implementada através do modelo de implementação clássica.
2. Selecione a rede virtual na lista que pretende criar um peering para.
3. Na lista de redes virtuais, selecione a rede virtual que pretende criar um peering para.
4. Em **definições**, selecione **Peerings**.
5. Selecione **+ Adicionar**. 
6. <a name="add-peering"></a>Introduza ou selecione os valores para as seguintes definições:
    - **Nome:** o nome para o peering tem de ser exclusivo dentro da rede virtual.
    - **Modelo de implementação de rede virtual:** selecione a rede virtual que pretende peering com de modelo de implementação de que foi implementada através de.
    - **Sei o ID de recurso:** se tiver acesso de leitura para a rede virtual que pretende peering com, deixe esta caixa de verificação desmarcada. Se não tiver acesso de leitura para a rede virtual ou uma subscrição que pretende peering com, esta caixa de verificação. Introduza o ID de recurso completo da rede virtual que pretende peering com no **ID de recurso** caixa que antes eram quando tiver selecionado a caixa. O recurso tem de ser ID que introduziu para uma rede virtual que exista no mesmo, ou [suportado diferentes](#requirements-and-constraints) Azure [região](https://azure.microsoft.com/regions) como esta rede virtual. O ID de recurso completo semelhante /subscriptions/{targetsubscriptionid}/resourcegroups/{targetresourcegroupname}<Id>/providers/Microsoft.Network/virtualNetworks/ /resourceGroups/ < nome de grupo de recursos > <--nome de rede virtual >. Pode obter o ID de recurso para uma rede virtual visualizando as propriedades de uma rede virtual. Para saber como visualizar as propriedades de uma rede virtual, consulte o artigo [gerir redes virtuais](manage-virtual-network.md#view-virtual-networks-and-settings).
    - **Subscrição:** selecionar o [subscrição](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) da rede virtual que pretende peering com. Uma ou mais subscrições são listadas, dependendo de quantos subscrições a conta tem acesso de leitura para. Se tiver selecionado o **ID de recurso** caixa de verificação, esta definição não está disponível.
    - **Rede virtual:** selecione a rede virtual que pretende peering com. Pode selecionar uma rede virtual criadas através de um modelo de implementação do Azure. Se pretender selecionar uma rede virtual numa região diferente, tem de selecionar uma rede virtual num [suportado região](#cross-region). Tem de ter acesso de leitura para a rede virtual para que seja visível na lista. Se uma rede virtual é listada, mas a cinzento, poderá ser porque o espaço de endereços da rede virtual sobrepõe-se o espaço de endereços para esta rede virtual. Se a sobreposição de espaços de endereços de rede virtual, não pode ser emparelhados. Se tiver selecionado o **ID de recurso** caixa de verificação, esta definição não está disponível.
    - **Permitir o acesso de rede virtual:** selecione **ativado** (predefinição), se pretender ativar a comunicação entre as duas redes virtuais. Ativar a comunicação entre redes virtuais permite que os recursos ligados a uma rede virtual para comunicar entre si com o mesmo largura de banda e latência, como se ligados à mesma rede virtual. Todas as comunicações entre os recursos as duas redes virtuais é através da rede privada do Azure. O **VirtualNetwork** etiqueta predefinida para grupos de segurança de rede abrange a rede virtual e a rede virtual em modo de peering. Para obter mais informações sobre etiquetas de predefinição de grupo de segurança de rede, leia o [descrição geral de grupos de segurança de rede](virtual-networks-nsg.md#default-tags) artigo.  Selecione **desativado** se não quiser que o tráfego que irão transitar para a rede virtual em modo de peering. Poderá selecionar **desativado** se tiver uma rede virtual com a outra rede virtual em modo de peering, mas ocasionalmente pretende desativar o fluxo de tráfego entre as duas redes virtuais. Pode considerar a ativação/desativação é mais conveniente de eliminar e voltar a criar peerings. Quando esta definição estiver desativada, o tráfego não fluir entre as redes virtuais em modo de peering.
    - **Permitir tráfego reencaminhado:** marcar esta caixa para permitir tráfego *reencaminhados* por uma aplicação virtual de rede numa rede virtual (que não provenientes da rede virtual) para o fluxo para esta rede virtual através de um peering . Por exemplo, considere as três redes virtuais com o nome Spoke1, Spoke2 e Hub. Um peering existe entre cada rede virtual spoke e a rede virtual do Hub, mas não existem peerings entre as redes virtuais spoke. Uma aplicação virtual de rede é implementada na rede virtual Hub e rotas definidas pelo utilizador são aplicadas a cada rede virtual spoke que encaminhar o tráfego entre as sub-redes através da aplicação virtual de rede. Se esta caixa de verificação não estiver marcada para o peering entre cada rede virtual spoke e a rede virtual do hub, o tráfego não fluir entre as redes virtuais spoke porque o hub é reencaminhar o tráfego entre as redes virtuais. Ao ativar esta capacidade permite o tráfego reencaminhado através do peering, não cria as rotas definidas pelo utilizador ou dispositivos de rede virtual. Rotas definidas pelo utilizador e os dispositivos de rede virtual são criados em separado. Saiba mais sobre [rotas definidas pelo utilizador](virtual-networks-udr-overview.md#user-defined). Não tem de verificar esta definição se é reencaminhar o tráfego entre redes virtuais através de um Gateway de VPN do Azure.
    - **Permitir que o trânsito do gateway:** esta caixa de verificação se tiver um gateway de rede virtual ligado a esta rede virtual e pretende permitir o tráfego de rede virtual em modo de peering para fluir pelo gateway. Por exemplo, esta rede virtual pode ser ligada a uma rede no local através de um gateway de rede virtual. O gateway pode ser um gateway ExpressRoute ou VPN. Ao selecionar esta caixa permite que o tráfego de rede virtual em modo de peering para fluir pelo gateway ligado a esta rede virtual à rede no local. Se selecionar esta caixa, a rede virtual em modo de peering não pode ter um gateway configurado. A rede virtual em modo de peering tem de ter o **utilizar gateways remotos** caixa de verificação selecionado quando configurar o peering da outra rede virtual para esta rede virtual. Se deixar esta caixa desmarcada (predefinição), o tráfego dos fluxos ainda em modo de peering de rede virtual para esta rede virtual, mas não é possível fluxo através de um gateway de rede virtual ligado a esta rede virtual. Se o peering entre uma rede virtual (Resource Manager) e uma rede virtual (clássica), o gateway tem de ser na rede virtual (Resource Manager). Não é possível ativar esta opção se estiver a peering redes virtuais em diferentes regiões.
    
        Para além das que reencaminham o tráfego para uma rede no local, um gateway de VPN pode reencaminhar o tráfego de rede entre redes virtuais que estão em modo de peering com a rede virtual, que o gateway está, sem as redes virtuais que necessitam ser executado o peering entre si. Isto é útil quando pretender utilizar um gateway de VPN num hub (veja o exemplo de hub- and -spoke descrito para **permitir tráfego reencaminhado**) rede virtual para encaminhar o tráfego entre redes virtuais spoke que não estão em modo de peering entre si. Para mais informações sobre a utilização de um gateway de trânsito, consulte o artigo [configurar um gateway de VPN para trânsito no peering de rede virtual](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Este cenário requer a implementação de rotas definidas pelo utilizador que especificam o gateway de rede virtual porque o tipo de salto seguinte. Saiba mais sobre [rotas definidas pelo utilizador](virtual-networks-udr-overview.md#user-defined). Pode especificar apenas um gateway de VPN como um tipo de próximo salto numa rota definida pelo utilizador, não é possível especificar um gateway ExpressRoute como o tipo de próximo salto numa rota definida pelo utilizador. Não é possível ativar esta opção se estiver a peering redes virtuais em diferentes regiões.

    - **Utilizar gateways remotos:** marcar esta caixa para permitir tráfego desta rede virtual para o fluxo através de um gateway de rede virtual ligado à rede virtual está a ser peering com. Por exemplo, a rede virtual que está a ser peering com tem um gateway de VPN ligado que permite a comunicação a uma rede no local.  Ao selecionar esta caixa permite tráfego desta rede virtual para o fluxo através do gateway VPN ligado à rede virtual em modo de peering. Se selecionar esta caixa, a rede virtual em modo de peering tem de ter um gateway de rede virtual ligado à mesma e tem de ter o **permitir trânsito do gateway** caixa de verificação marcada. Se deixar esta caixa desmarcada (predefinição), o tráfego de rede virtual em modo de peering ainda possam circular para esta rede virtual, mas não pode fluxo através de um gateway de rede virtual ligado a virtual nesta rede. 
    Apenas um peering para esta rede virtual pode ter esta definição ativada.

        Não é possível utilizar gateways remotos se já tiver um gateway configurado na sua rede virtual. Não é possível ativar esta opção se estiver a peering redes virtuais em diferentes regiões. Para obter mais informações sobre como utilizar um gateway para trânsito, consulte [configurar um gateway de VPN para trânsito no peering de rede virtual](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json)

7. Selecione **OK** para adicionar o peering para a rede virtual que selecionou.

Para obter instruções passo a passo para implementar o peering entre redes virtuais em diferentes subscrições e modelos de implementação, consulte [passos](#next-steps). 


### <a name="commands"></a>Comandos

- **CLI do Azure**: [az rede vnet peering criar](/cli/azure/network/vnet/peering#create)
- **PowerShell**: [Add-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering)

## <a name="view-or-change-peering-settings"></a>Ver ou alterar as definições de peering

Antes de alterar um peering, familiarize-se com o [requisitos e restrições](#requirements-and-contstraints) e [as permissões necessárias](#permissions).

1. Na caixa de pesquisa na parte superior do portal, introduza *redes virtuais* na caixa de pesquisa. Quando **redes virtuais** aparece nos resultados da pesquisa, selecionados-lo. Não selecione **redes virtuais (clássicas)** se aparece na lista, como não é possível criar um peering de uma rede virtual implementada através do modelo de implementação clássica.
2. Selecione a rede virtual na lista que pretende alterar as definições peering.
3. Na lista de redes virtuais, selecione a rede virtual que pretende alterar as definições de peering para.
4. Em **definições**, selecione **Peerings**.
5. Selecione o peering que pretende ver ou alterar as definições.
6. Altere a definição adequada. Leia sobre as opções para cada definição na [passo 6](#add-peering) de criar um peering. 
7. Selecione **Guardar**.

**Comandos**

- **CLI do Azure**: [lista do az rede vnet peering](/cli/azure/network/vnet/peering#az_network_vnet_peering_list) para peerings de lista para uma rede virtual, [az rede Mostrar peering vnet](/cli/azure/network/vnet/peering#az_network_vnet_peering_show) para mostrar as definições para um peering específica, e [rede az atualização de vnet peering](/cli/azure/network/vnet/peering#az_network_vnet_peering_update) para alterar as definições de peering. |
- **PowerShell**: [Get-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering) para obter as definições de peering de vista e [Set-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/set-azurermvirtualnetworkpeering) para alterar as definições.

## <a name="delete-a-peering"></a>Eliminar um peering

Antes de eliminar um peering, certifique-se a conta tem o [as permissões necessárias](#permissions).

Quando um peering é eliminado, o tráfego de rede virtual já não flui de para a rede virtual em modo de peering. Quando as redes virtuais implementadas através do Gestor de recursos em modo de peering, a cada rede virtual tem um peering à rede virtual. Embora a eliminar o peering de uma rede virtual desativa a comunicação entre as redes virtuais, não eliminar o peering da outra rede virtual. O estado de peering para o peering que existe na rede virtual é **desligado**. Não é possível recriar o peering até voltar a criar o peering na primeira rede virtual e o estado de peering para ambas as redes virtuais que as alterações *ligado*. 

Se pretender que as redes virtuais para comunicar por vezes, mas nem sempre, em vez de eliminar um peering, pode definir o **permitir o acesso de rede virtual** definição **desativado** em vez disso. Para saber como, leia o passo 6 do [criar um peering](#create-peering) secção deste artigo. Pode encontrar desativar e ativar o acesso de rede fácil eliminar e recriar peerings.

1. Na caixa de pesquisa na parte superior do portal, introduza *redes virtuais* na caixa de pesquisa. Quando **redes virtuais** aparece nos resultados da pesquisa, selecionados-lo. Não selecione **redes virtuais (clássicas)** se aparece na lista, como não é possível criar um peering de uma rede virtual implementada através do modelo de implementação clássica.
2. Selecione a rede virtual na lista que pretende eliminar um peering para.
3. Na lista de redes virtuais, selecione a rede virtual que pretende eliminar um peering para.
4. Em **definições**, selecione **Peerings**.
5. No lado direito de peering que pretende eliminar, selecione **...** , selecione **eliminar**, em seguida, selecione **Sim** para eliminar o peering da primeira rede virtual.
6. Conclua os passos anteriores para eliminar o peering da outra rede virtual no peering de.

**Comandos**

- **CLI do Azure**: [eliminar do az rede vnet peering](/cli/azure/network/vnet/peering#az_network_vnet_peering_delete)
- **PowerShell**: [Remove-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/remove-azurermvirtualnetworkpeering)

## <a name="requirements-and-constraints"></a>Requisitos e limitações 

- As redes virtuais que o elemento tem de ter espaços de endereços IP sem sobreposição.
- Não é possível adicionar intervalos de endereços para ou eliminar intervalos de endereços do espaço de endereços da rede virtual depois de uma rede virtual é executado o peering com outra rede virtual. Para adicionar ou remover intervalos de endereços, eliminar o peering, adicionar ou remover os intervalos de endereços, em seguida, recrie o peering. Para intervalos de endereços para adicionar ou remover intervalos de endereços das redes virtuais, consulte o artigo [gerir redes virtuais](manage-virtual-network.md).
- Pode elemento duas redes virtuais implementadas através do Gestor de recursos ou uma rede virtual implementado através do Resource Manager com uma rede virtual implementada através do modelo de implementação clássica. Não é possível elemento duas redes virtuais criadas através do modelo de implementação clássica. Se não estiver familiarizado com os modelos de implementação do Azure, leia o [modelos de implementação do Azure compreender](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artigo. Pode utilizar um [Gateway de VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) para ligar duas redes virtuais criadas através do modelo de implementação clássica.
- Ao configurar o peering de duas redes virtuais criadas através do Resource Manager, tem de ser configurado um peering para cada rede virtual no peering. Consulte um dos seguintes tipos para o estado do peering: 
    - *Foi iniciada:* quando cria o peering de modo a segunda rede virtual da primeira rede virtual, o estado de peering é *iniciado*. 
    - *Ligado:* quando cria o peering da segunda rede virtual para a primeira rede virtual, o respetivo estado peering é *ligado*. Se visualizar o estado de peering para a primeira rede virtual, consulte o estado mudou de *iniciado* para *ligado*. O peering não é estabelecido êxito até que o estado de peering para ambos os peerings de rede virtual é *ligado*.
- Quando o peering de uma rede virtual criadas através do Resource Manager com uma rede virtual criadas através do modelo de implementação clássica, apenas configurar um peering da rede virtual implementado através do Resource Manager. Não é possível configurar o peering para uma rede virtual (clássica) ou entre duas redes virtuais implementadas através do modelo de implementação clássica. Quando cria o peering da rede virtual (Resource Manager) para a rede virtual (clássica), o estado de peering é *atualização*, em seguida, em breve alterações *ligado*.
- Um peering é estabelecido entre duas redes virtuais. Não são transitivas Peerings. Se criar peerings entre:
    - VirtualNetwork1 & VirtualNetwork2
    - VirtualNetwork2 & VirtualNetwork3

  Não há nenhum peering entre VirtualNetwork1 e VirtualNetwork3 através de VirtualNetwork2. Se pretender criar uma rede virtual peering entre VirtualNetwork1 e VirtualNetwork3, terá de criar um peering entre VirtualNetwork1 e VirtualNetwork3.
- Não é possível resolver os nomes de redes virtuais em modo de peering com a resolução do nome do Azure de predefinição. Para resolver os nomes de outras redes virtuais, tem de utilizar [DNS do Azure para domínios privados](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou um servidor DNS personalizado. Para saber como configurar o próprio servidor DNS, consulte [resolução de nomes utilizando o seu próprio servidor DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
- Recursos na redes virtuais em modo de peering na mesma região podem comunicar entre si com o mesmo largura de banda e latência como se estivessem na mesma rede virtual. Cada tamanho da máquina virtual tem no entanto a sua própria largura de banda de rede máxima. Para saber mais sobre a largura de banda de rede máxima para tamanhos de outra máquina virtual, consulte [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tamanhos de máquina virtual.
- As subscrições que ambas as redes virtuais que pretende elemento são pol, tem de estar associado ao mesmo inquilino do Azure Active Directory. Se ainda não tiver um inquilino do AD, pode rapidamente [criar um](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-new-azure-ad-tenant). Pode utilizar um [Gateway de VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) para ligar duas redes virtuais que existam em diferentes subscrições associadas aos inquilinos diferentes do Active Directory.
- Uma rede virtual pode estar em modo de peering para outra rede virtual e também de estar ligada a outra rede virtual com um gateway de rede virtual do Azure. Quando estão ligados a redes virtuais através de peering e um gateway, o tráfego entre as redes virtuais flui através da configuração do peering, em vez do gateway.
- Existe uma cobrança nominal para o tráfego de entrada e de saída que utilize um peering de rede virtual. Para obter mais informações, veja a [página de preços](https://azure.microsoft.com/pricing/details/virtual-network).
* <a name="cross-region"></a>Pode elemento redes virtuais na mesma região ou regiões diferentes. As seguintes restrições não se aplicam quando ambas as redes virtuais estiverem a ser o *mesmo* região, mas aplicam quando as redes virtuais global em modo de peering: 
    - As redes virtuais podem existir em qualquer região de nuvem pública do Azure, mas não em nuvens nacionais do Azure.
    - Recursos numa rede virtual não é possível comunicar com o endereço IP de um balanceador de carga interno do Azure na rede virtual em modo de peering. O Balanceador de carga e os recursos que comunicam com a mesma tem de ser na mesma rede virtual.
    - Não é possível utilizar gateways remotos ou permitir que o trânsito do gateway. Para utilizar gateways remotos ou permitir que o trânsito do gateway, ambas as redes virtuais no peering de tem de existir na mesma região. 

## <a name="permissions"></a>Permissões

As contas que utilizar para criar um peering de rede virtual tem de ter a função necessária ou permissões. Por exemplo, se são peering duas redes virtuais com o nome *myVnetA* e *myVnetB*, deve ser atribuída à conta a seguinte função mínima ou as permissões para cada rede virtual:
    
|Rede virtual|Modelo de implementação|Função|Permissões|
|---|---|---|---|
|myVnetA|Resource Manager|[Contribuidor de Rede](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Clássica|[Contribuidor de Rede Clássica](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|N/A|
|myVnetB|Resource Manager|[Contribuidor de Rede](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Clássica|[Contribuidor de Rede Clássica](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Saiba mais sobre [funções incorporadas](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) e atribuir permissões específicas para [funções personalizadas](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (apenas para o Resource Manager).

## <a name="next-steps"></a>Passos Seguintes

* É criado um peering de rede virtual entre redes virtuais criadas com o mesmo ou com diferentes modelos de implementação que existam na mesma ou em diferentes subscrições. Conclua um tutorial para um dos cenários seguintes:

    |Modelo de implementação do Azure             | Subscrição  |
    |---------                          |---------|
    |Ambas com Resource Manager              |[Mesma](tutorial-connect-virtual-networks-portal.md)|
    |                                   |[Diferente](create-peering-different-subscriptions.md)|
    |Uma com Resource Manager, outra com clássica  |[Mesma](create-peering-different-deployment-models.md)|
    |                                   |[Diferente](create-peering-different-deployment-models-subscriptions.md)|

* Saiba como criar uma [topologia de rede hub-and-spoke](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual network-peering).
