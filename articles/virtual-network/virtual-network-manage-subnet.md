---
title: Adicionar, alterar ou eliminar uma sub-rede de rede virtual do Azure | Microsoft Docs
description: Saiba como adicionar, alterar ou eliminar uma sub-rede de rede virtual no Azure.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/09/2018
ms.author: jdial
ms.openlocfilehash: 27918e1d0b335613ea578a815fb3ae00df73ebaa
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="add-change-or-delete-a-virtual-network-subnet"></a>Adicionar, alterar ou eliminar uma sub-rede de rede virtual

Saiba como adicionar, alterar ou eliminar uma sub-rede de rede virtual. Se não estiver familiarizado com redes virtuais, antes de adicionar, alterar ou eliminar uma sub-rede, recomendamos que leia [descrição geral da rede Virtual do Azure](virtual-networks-overview.md) e [criar, alterar ou eliminar uma rede virtual](virtual-network-manage-network.md). Todos os recursos do Azure implementados numa rede virtual são implementados para uma sub-rede dentro de uma rede virtual.
 
## <a name="before-you-begin"></a>Antes de começar

Conclua as seguintes tarefas antes de concluir os passos em qualquer secção deste artigo:

- Se ainda não tiver uma conta do Azure, inscreva-se um [conta de avaliação gratuita](https://azure.microsoft.com/free).
- Se utilizar o portal, Abra https://portal.azure.com e iniciar sessão com a sua conta do Azure.
- Se utilizar comandos do PowerShell para concluir tarefas neste artigo, quer executar os comandos [Shell de nuvem do Azure](https://shell.azure.com/powershell), ou através da execução do PowerShell do seu computador. O Azure Cloud Shell é um shell interativo gratuito que pode utilizar para executar os passos neste artigo. Tem as ferramentas comuns do Azure pré-instaladas e configuradas para utilização com a sua conta. Este tutorial requer o Azure PowerShell versão do módulo 5.2.0 ou posterior. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.
- Se utilizar comandos de interface de linha de comandos (CLI) do Azure para concluir tarefas neste artigo, quer executar os comandos [Shell de nuvem do Azure](https://shell.azure.com/bash), ou executando a CLI do seu computador. Este tutorial requer a CLI do Azure versão 2.0.26 ou posterior. Executar `az --version` para localizar a versão instalada. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). Se estiver a executar localmente a CLI do Azure, também terá de executar `az login` para criar uma ligação com o Azure.

## <a name="add-a-subnet"></a>Adicionar uma sub-rede

1. Na caixa de pesquisa na parte superior do portal, introduza *redes virtuais* na caixa de pesquisa. Quando **redes virtuais** aparece nos resultados da pesquisa, selecionados-lo.
2. Na lista de redes virtuais, selecione a rede virtual que pretende adicionar uma sub-rede para.
3. Em **definições**, selecione **sub-redes**.
4. Selecione **+ sub-rede**.
5. Introduza valores para os seguintes parâmetros:
    - **Nome**: O nome tem de ser exclusivo dentro da rede virtual.
    - **Intervalo de endereços**: O intervalo deve ser exclusivo num espaço de endereços da rede virtual. O intervalo não pode sobrepor-se com outros intervalos de endereços de sub-rede na rede virtual. O espaço de endereços tem de ser especificado utilizando a notação de Classless entre domínios encaminhamento CIDR (). Por exemplo, numa rede virtual com endereço espaço 10.0.0.0/16, poderá definir um espaço de endereços da sub-rede do 10.0.0.0/24. O intervalo mais pequeno, que pode especificar é /29, que fornece oito endereços IP da sub-rede. O primeiro e último endereço em cada sub-rede para a conformidade de protocolo de reservas de Azure. Três endereços adicionais estão reservados para utilização do serviço do Azure. Como resultado, que define uma sub-rede com/29 resultados de intervalo três utilizáveis endereços de IP na sub-rede de endereços. Se planear ligar uma rede virtual para um gateway de VPN, tem de criar uma sub-rede de gateway. Saiba mais sobre [considerações de intervalo de endereço específico para sub-redes de gateway](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Pode alterar o intervalo de endereços depois da sub-rede é adicionada, sob condições específicas. Para saber como alterar um intervalo de endereços da sub-rede, consulte [alterar definições da sub-rede](#change-subnet-settings).
    - **Grupo de segurança de rede**: pode associar zero ou uma rede grupo de segurança existente para uma sub-rede para filtrar o tráfego de rede de entrada e saída para a sub-rede. O grupo de segurança de rede tem de existir na mesma subscrição e localização da rede virtual. Saiba mais sobre [grupos de segurança de rede](security-overview.md) e [como criar um grupo de segurança de rede](virtual-networks-create-nsg-arm-pportal.md).
    - **Tabela de rotas**: pode associar zero ou uma existente tabela de rotas para uma sub-rede para controlar o encaminhamento de tráfego de rede a outras redes. A tabela de rota tem de existir na mesma subscrição e localização da rede virtual. Saiba mais sobre [encaminhamento Azure](virtual-networks-udr-overview.md) e [como criar uma tabela de rota](tutorial-create-route-table-portal.md)
    - **Pontos finais de serviço:** uma sub-rede pode ter zero ou vários pontos finais de serviço ativados para o mesmo. Para ativar um ponto final de serviço para um serviço, selecione o serviço ou serviços que pretende ativar pontos finais de serviço para a partir de **serviços** lista. Para remover um ponto final de serviço, anule a seleção de serviço que pretende remover o ponto final de serviço para. Para obter mais informações sobre pontos finais de serviço, consulte o artigo [descrição geral de pontos finais de serviço de rede Virtual](virtual-network-service-endpoints-overview.md). Depois de ativar um ponto final de serviço para um serviço, tem também de ativar o acesso de rede para a sub-rede para um recurso que criou com o serviço. Por exemplo, se ativar o ponto final de serviço para *Microsoft*, tem também de ativar acesso à rede para todas as contas de armazenamento do Azure que pretende conceder acesso à rede. Para obter detalhes sobre como ativar o acesso de rede para sub-redes ativado para um ponto final de serviço, consulte a documentação para o serviço individuais ativou o ponto final do serviço para.
6. Para adicionar a sub-rede à rede virtual que selecionou, selecione **OK**.

**Comandos**

- CLI do Azure: [az rede vnet sub-rede](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create)
- PowerShell: [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig)

## <a name="change-subnet-settings"></a>Alterar definições da sub-rede

1. Na caixa de pesquisa na parte superior do portal, introduza *redes virtuais* na caixa de pesquisa. Quando **redes virtuais** aparece nos resultados da pesquisa, selecionados-lo.
2. Na lista de redes virtuais, selecione a rede virtual que contém a sub-rede que pretende alterar as definições.
3. Em **definições**, selecione **sub-redes**.
4. Na lista de sub-redes, selecione a sub-rede que pretende alterar as definições. Pode alterar as seguintes definições:

    - **Intervalo de endereços:** se não existem recursos são implementados dentro da sub-rede, pode alterar o intervalo de endereços. Se existirem quaisquer recursos na sub-rede, deve mover os recursos para outra sub-rede ou eliminá-los primeiro da sub-rede. Os passos que efetuar para mover ou eliminar um recurso variam consoante o recurso. Para saber como mover ou eliminar os recursos que estão em sub-redes, leia a documentação para cada tipo de recurso que pretende mover ou eliminar. Consulte as restrições de **intervalo de endereços** no passo 5 [adicionar uma sub-rede](#add-a-subnet).
    - **Os utilizadores**: pode controlar o acesso à sub-rede através da utilização de funções incorporadas ou as suas próprias funções personalizadas. Para saber mais sobre a atribuição de funções e os utilizadores acedam a sub-rede, consulte o artigo [utilizar atribuição de função para gerir o acesso aos seus recursos do Azure](../active-directory/role-based-access-control-configure.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-access).
    - Para obter informações sobre como alterar **grupo de segurança de rede**, **tabela de rotas**, **utilizadores**, e **pontos finais de serviço**, consulte o passo 5 no [ Adicionar uma sub-rede](#add-a-subnet).
5. Selecione **guardar**.

**Comandos**

- CLI do Azure: [atualização de sub-rede da vnet de rede de az](/cli/azure/network/vnet#az_network_vnet_update)
- PowerShell: [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)

## <a name="delete-a-subnet"></a>Eliminar uma sub-rede

Pode eliminar uma sub-rede apenas se existem não existem recursos na sub-rede. Se existirem recursos na sub-rede, tem de eliminar os recursos que estão na sub-rede antes de poder eliminar a sub-rede. Os passos que efetuar para eliminar um recurso variam consoante o recurso. Para saber como eliminar os recursos que estão em sub-redes, leia a documentação para cada tipo de recurso que pretende eliminar.

1. Na caixa de pesquisa na parte superior do portal, introduza *redes virtuais* na caixa de pesquisa. Quando **redes virtuais** aparece nos resultados da pesquisa, selecionados-lo.
2. Na lista de redes virtuais, selecione a rede virtual que contém a sub-rede que pretende eliminar.
3. Em **definições**, selecione **sub-redes**.
4. Na lista de sub-redes, selecione **...** , à direita para a sub-rede que pretende eliminar
5. Selecione **eliminar**e, em seguida, selecione **Sim**.

**Comandos**

- CLI do Azure: [eliminar az rede vnet](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#az_network_vnet_delete)
- PowerShell: [Remove-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/remove-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)

## <a name="permissions"></a>Permissões

Para executar tarefas em sub-redes, deve ser atribuída à conta para o [contribuinte de rede](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) função ou a um [personalizado](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) função atribuída as permissões adequadas listadas na seguinte tabela:

|Operação                                                                |   Nome da operação                               |
|-----------------------------------------------------------------------  |   -------------------------------------------  |
|Microsoft.Network/virtualNetworks/subnets/read                           |   Obter a sub-rede da rede Virtual                   |
|Microsoft.Network/virtualNetworks/subnets/write                          |   Criar ou atualizar a sub-rede da rede Virtual      |
|Microsoft.Network/virtualNetworks/subnets/delete                         |   Eliminar a sub-rede de rede Virtual                |
|Microsoft.Network/virtualNetworks/subnets/join/action                    |   Associar a rede Virtual                         |
|Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action  |   Aderir ao serviço a uma sub-rede                     |
|Microsoft.Network/virtualNetworks/subnets/virtualMachines/read           |   Obter máquinas de virtuais de sub-rede de rede Virtual  |
