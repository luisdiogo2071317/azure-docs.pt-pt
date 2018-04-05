---
title: Criar, alterar ou eliminar uma Azure virtual network | Microsoft Docs
description: Saiba como criar, alterar ou eliminar uma rede virtual no Azure.
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
ms.date: 02/09/2018
ms.author: jdial
ms.openlocfilehash: ac0b15f120071093fd81de1d83cf2067ecbac269
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="create-change-or-delete-a-virtual-network"></a>Criar, alterar ou eliminar uma rede virtual

Saiba como criar e eliminar uma rede virtual e alterar as definições, como servidores DNS e espaços de endereços IP, para uma rede virtual existente.

Uma rede virtual é uma representação da sua própria rede na nuvem. Uma rede virtual é um isolamento lógico da nuvem do Azure que está dedicado à sua subscrição do Azure. Para cada rede virtual que criou, pode:
- Escolha um espaço de endereço para atribuir. Um espaço de endereços é composta por um ou mais intervalos de endereços que são definidos utilizando a notação de Classless entre domínios encaminhamento CIDR (), como 10.0.0.0/16.
- Optar por utilizar o servidor DNS fornecidos pelo Azure ou utilize o seu próprio servidor DNS. Todos os recursos que estão ligados à rede virtual são atribuídos este servidor DNS para resolver nomes dentro da rede virtual.
- Segmentar a rede virtual em sub-redes, cada um com o seu próprio intervalo de endereços, dentro do espaço de endereços da rede virtual. Para saber como criar, alterar e eliminar as sub-redes, consulte o artigo [adicionar, alterar ou eliminar sub-redes](virtual-network-manage-subnet.md).

## <a name="before-you-begin"></a>Antes de começar

Conclua as seguintes tarefas antes de concluir os passos em qualquer secção deste artigo:

- Se ainda não tiver uma conta do Azure, inscreva-se um [conta de avaliação gratuita](https://azure.microsoft.com/free).
- Se utilizar o portal, abra https://portal.azure.come inicie sessão com a sua conta do Azure.
- Se utilizar comandos do PowerShell para concluir tarefas neste artigo, quer executar os comandos [Shell de nuvem do Azure](https://shell.azure.com/powershell), ou através da execução do PowerShell do seu computador. O Azure Cloud Shell é um shell interativo gratuito que pode utilizar para executar os passos neste artigo. Tem as ferramentas comuns do Azure pré-instaladas e configuradas para utilização com a sua conta. Este tutorial requer o Azure PowerShell versão do módulo 5.2.0 ou posterior. Execute `Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.
- Se utilizar comandos de interface de linha de comandos (CLI) do Azure para concluir tarefas neste artigo, quer executar os comandos [Shell de nuvem do Azure](https://shell.azure.com/bash), ou executando a CLI do seu computador. Este tutorial requer a CLI do Azure versão 2.0.26 ou posterior. Execute `az --version` para localizar a versão instalada. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). Se estiver a executar localmente a CLI do Azure, também terá de executar `az login` para criar uma ligação com o Azure.

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

1. Selecione **+ criar um recurso** > **redes** > **rede Virtual**.
2. Introduza ou selecione os valores para as seguintes definições e selecione **criar**:
    - **Nome**: O nome tem de ser exclusivo no [grupo de recursos](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) que selecionar para criar a rede virtual. Não é possível alterar o nome depois da criação da rede virtual. Pode criar várias redes virtuais ao longo do tempo. Para sugestões de atribuição de nomes, consulte [convenções de nomenclatura](https://docs.microsoft.com/en-us/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions). Seguir uma convenção de nomenclatura pode ajudar a tornar mais fácil de gerir várias redes virtuais.
    - **Espaço de endereços**: O espaço de endereços para uma rede virtual é composto por um ou mais sem se sobreporem intervalos de endereços que estejam especificados na notação CIDR. O intervalo de endereços que define pode ser public ou private (RFC 1918). Se definir o intervalo de endereços como público ou privado, o intervalo de endereços é acessível apenas a partir de dentro da rede virtual, redes virtuais interligados e quaisquer redes no local que tiver estabelecido ligação à rede virtual. Não é possível adicionar os seguintes intervalos de endereços:
        - 224.0.0.0/4 (Multicast)
        - 255.255.255.255/32 (difusão)
        - 127.0.0.0/8 (Loopback)
        - 169.254.0.0/16 (locais)
        - 168.63.129.16/32 (DNS interno)

      Embora, pode definir o intervalo de endereços apenas um quando criar a rede virtual, pode adicionar mais intervalos de endereços ao espaço de endereços depois da criação da rede virtual. Para saber como adicionar um intervalo de endereços a uma rede virtual existente, consulte [adicionar ou remover um intervalo de endereços](#add-or-remove-an-address-range).

      >[!WARNING]
      >Se uma rede virtual tem de intervalos de endereços que se sobrepõe a outra rede virtual local ou na rede, as duas redes não podem ser ligadas. Antes de definir um intervalo de endereços, considere se pretenda ligar a rede virtual com outras redes virtuais ou redes no local no futuro.
      >
      >

    - **Nome da sub-rede**: O nome de sub-rede tem de ser exclusivo dentro da rede virtual. Não é possível alterar o nome de sub-rede depois de criar a sub-rede. O portal requer que defina uma sub-rede quando cria uma rede virtual, apesar de uma rede virtual não é necessária ter quaisquer sub-redes. No portal, pode definir apenas uma sub-rede quando criar uma rede virtual. Pode adicionar mais sub-redes para a rede virtual mais tarde, depois da criação da rede virtual. Para adicionar uma sub-rede para uma rede virtual, consulte [gerir sub-redes](virtual-network-manage-subnet.md). Pode criar uma rede virtual que tem várias sub-redes utilizando a CLI do Azure ou o PowerShell.

      >[!TIP]
      >Por vezes, os administradores criar sub-redes diferentes filtrar ou controlar o encaminhamento de tráfego entre as sub-redes. Antes de definir sub-redes, considere como poderá pretender filtrar e encaminhar o tráfego entre as sub-redes. Para saber mais sobre a filtragem de tráfego entre sub-redes, consulte [grupos de segurança de rede](security-overview.md). Azure automaticamente tráfego de rotas entre sub-redes, mas pode substituir as rotas predefinidas do Azure. Para saber mais sobre o encaminhamento do tráfego Azures predefinido sub-rede, consulte [descrição geral de encaminhamento](virtual-networks-udr-overview.md).
      >

    - **Intervalo de endereços da sub-rede**: O intervalo tem de estar no espaço de endereço que introduziu para a rede virtual. O intervalo mais pequeno, que pode especificar é /29, que fornece oito endereços IP da sub-rede. O primeiro e último endereço em cada sub-rede para a conformidade de protocolo de reservas de Azure. Três endereços adicionais estão reservados para utilização do serviço do Azure. Como resultado, uma rede virtual com um intervalo de endereços de sub-rede de /29 tem três apenas endereços IP utilizáveis. Se planear ligar uma rede virtual para um gateway de VPN, tem de criar uma sub-rede de gateway. Saiba mais sobre [considerações de intervalo de endereço específico para sub-redes de gateway](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Pode alterar o intervalo de endereços depois de criar a sub-rede, sob condições específicas. Para saber como alterar um intervalo de endereços da sub-rede, consulte [gerir sub-redes](virtual-network-manage-subnet.md).
    - **Subscrição**: selecione uma [subscrição](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). Não é possível utilizar a mesma rede virtual em mais do que uma subscrição do Azure. No entanto, pode ligar uma rede virtual numa subscrição a redes virtuais no outras subscrições com [peering de rede virtual](virtual-network-peering-overview.md). Qualquer recurso do Azure que ligam à rede virtual tem de ser na mesma subscrição que a rede virtual.
    - **Grupo de recursos**: selecione um existente [grupo de recursos](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) ou crie um novo. Um recurso do Azure que ligam à rede virtual pode estar no mesmo grupo de recursos de rede virtual ou num grupo de recursos diferente.
    - **Localização**: selecione um Azure [localização](https://azure.microsoft.com/regions/), também conhecido como uma região. Uma rede virtual pode ter apenas uma localização do Azure. No entanto, pode ligar uma rede virtual numa localização a uma rede virtual noutra localização, utilizando um gateway de VPN. Qualquer recurso do Azure que ligam à rede virtual tem de ser na mesma localização que a rede virtual.

**Comandos**

- CLI do Azure: [criar az rede vnet](/cli/azure/network/vnet)
- PowerShell: [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork)

## <a name="view-virtual-networks-and-settings"></a>Ver redes virtuais e definições

1. Na caixa de pesquisa na parte superior do portal, introduza *redes virtuais* na caixa de pesquisa. Quando **redes virtuais** aparece nos resultados da pesquisa, selecionados-lo.
2. Na lista de redes virtuais, selecione a rede virtual que pretende ver as definições para.
3. As seguintes definições estão listadas para a rede virtual que selecionou:
    - **Descrição geral**: fornece informações sobre a rede virtual, incluindo o espaço de endereços e servidores DNS. A seguinte captura de ecrã mostra as definições de descrição geral de uma rede virtual denominada **MyVNet**:

        ![Descrição geral da interface de rede](./media/manage-virtual-network/vnet-overview.png)

      Pode mover uma rede virtual para um grupo diferente da subscrição ou recurso, selecionando **alteração** junto a **grupo de recursos** ou **nome da subscrição**. Para saber como mover uma rede virtual, consulte [mover recursos para um grupo de recursos diferente ou uma subscrição](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json). O artigo apresenta uma lista de pré-requisitos e como mover recursos utilizando o portal do Azure, o PowerShell e a CLI do Azure. Tem de mover todos os recursos que estão ligados à rede virtual com a rede virtual.
    - **Espaço de endereços**: os espaços de endereços que estão atribuídos à rede virtual são listados. Para saber como adicionar e remover um intervalo de endereços para o espaço de endereços, concluir os passos [adicionar ou remover um intervalo de endereços](#add-or-remove-an-address-range).
    - **Dispositivos ligados**: são listados todos os recursos que estão ligados à rede virtual. Captura de ecrã anterior, três interfaces de rede e um balanceador de carga estão ligados à rede virtual. Quaisquer recursos novos que criam e ligar à rede virtual são listados. Se eliminar um recurso que foi ligado à rede virtual, já não aparece na lista.
    - **Sub-redes**: é apresentada uma lista de sub-redes existentes na rede virtual. Para saber como adicionar e remover uma sub-rede, consulte o artigo [gerir sub-redes](virtual-network-manage-subnet.md).
    - **Servidores DNS**: pode especificar se o servidor DNS interno do Azure ou um servidor DNS personalizado fornece resolução de nomes para os dispositivos que estão ligados à rede virtual. Quando cria uma rede virtual com o portal do Azure, os servidores DNS do Azure são utilizadas para a resolução do nome dentro de uma rede virtual, por predefinição. Para modificar os servidores DNS, execute os passos do [servidores DNS de alteração](#change-dns-servers) neste artigo.
    - **Peerings**: se existirem peerings existente na subscrição, estes são apresentados aqui. Pode ver as definições para peerings existente, ou criar, alterar ou eliminar peerings. Para saber mais sobre peerings, consulte o artigo [peering de rede Virtual](virtual-network-peering-overview.md).
    - **Propriedades**: apresenta as definições de sobre a rede virtual, incluindo o ID de recurso da rede virtual e a subscrição do Azure está a ser.
    - **Diagrama**: O diagrama proporciona uma representação visual de todos os dispositivos que estejam ligados à rede virtual. O diagrama tem algumas informações chaves sobre os dispositivos. Para gerir um dispositivo nesta vista, no diagrama, selecione o dispositivo.
    - **Definições comuns de Azure**: para saber mais sobre as definições do Azure comuns, consulte as seguintes informações:
        *   [Registo de atividades](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
        *   [Controlo de acesso (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
        *   [Etiquetas](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags)
        *   [Bloqueia](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
        *   [Script de automatização](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Comandos**

- CLI do Azure: [mostrar de vnet az rede](/cli/azure/network/vnet#az_network_vnet_show)
- PowerShell: [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork)

## <a name="add-or-remove-an-address-range"></a>Adicionar ou remover um intervalo de endereços

Pode adicionar e remover os intervalos de endereços para uma rede virtual. Um intervalo de endereços tem de ser especificado em notação CIDR e não pode sobrepor-se com outros intervalos de endereços na mesma rede virtual. Os intervalos de endereços que define podem ser public ou private (RFC 1918). Se definir o intervalo de endereços como público ou privado, o intervalo de endereços é acessível apenas a partir de dentro da rede virtual, redes virtuais interligados e quaisquer redes no local que tiver estabelecido ligação à rede virtual. Não é possível adicionar os seguintes intervalos de endereços:

- 224.0.0.0/4 (Multicast)
- 255.255.255.255/32 (difusão)
- 127.0.0.0/8 (Loopback)
- 169.254.0.0/16 (locais)
- 168.63.129.16/32 (DNS interno)

Para adicionar ou remover um intervalo de endereços:

1. Na caixa de pesquisa na parte superior do portal, introduza *redes virtuais* na caixa de pesquisa. Quando **redes virtuais** aparece nos resultados da pesquisa, selecionados-lo.
2. Na lista de redes virtuais, selecione a rede virtual para o qual pretende adicionar ou remover um intervalo de endereços.
3. Selecione **espaço de endereços**, em **definições**.
4. Conclua uma das seguintes opções:
    - **Adicionar um intervalo de endereços**: introduza o intervalo de endereços novo. O intervalo de endereços não pode sobrepor-se com um intervalo de endereços existente que está definido para a rede virtual.
    - **Remover um intervalo de endereços**: à direita do intervalo de endereços que pretende remover, selecione **...** , em seguida, selecione **remover**. Se existir uma sub-rede no intervalo de endereços, não é possível remover o intervalo de endereços. Para remover um intervalo de endereços, tem primeiro de eliminar quaisquer sub-redes (e quaisquer recursos nas sub-redes) que existe no intervalo de endereços.
5. Selecione **Guardar**.

**Comandos**

- CLI do Azure: [atualização do az rede vnet](/cli/azure/network/vnet#az_network_vnet_update)
- PowerShell: [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork)

## <a name="change-dns-servers"></a>Alterar os servidores DNS

Todas as VMs que estão ligadas ao registar a rede virtual com os servidores DNS que especificar para a rede virtual. Também utilizam o servidor DNS especificado para a resolução do nome. Cada interface de rede (NIC) numa VM pode ter as suas próprias definições de servidor DNS. Se um NIC tem as suas próprias definições de servidor DNS, elas substituirão as definições do servidor DNS para a rede virtual. Para saber mais sobre as definições de DNS de NIC, consulte o artigo [interface tarefas e definições de rede](virtual-network-network-interface.md#change-dns-servers). Para mais informações sobre resolução de nomes para VMs e instâncias de função nos serviços de nuvem do Azure, consulte o artigo [a resolução de nomes para VMs e instâncias de função](virtual-networks-name-resolution-for-vms-and-role-instances.md). Para adicionar, alterar ou remover um servidor DNS:

1. Na caixa de pesquisa na parte superior do portal, introduza *redes virtuais* na caixa de pesquisa. Quando **redes virtuais** aparece nos resultados da pesquisa, selecionados-lo.
2. Na lista de redes virtuais, selecione a rede virtual para o qual pretende alterar servidores DNS para.
3.  Selecione **servidores DNS**, em **definições**.
4. Selecione uma das seguintes opções:
    - **Predefinição (fornecidos pelo Azure)**: todos os nomes de recursos e endereços IP privados estão registados automaticamente para os servidores de DNS do Azure. Pode resolver nomes entre todos os recursos que estão ligados à mesma rede virtual. Não é possível utilizar esta opção para resolver nomes através de redes virtuais. Para resolver nomes através de redes virtuais, tem de utilizar um servidor DNS personalizado.
    - **Personalizada**: pode adicionar um ou mais servidores, até ao limite do Azure para uma rede virtual. Para saber mais sobre os limites do servidor DNS, consulte o artigo [limites do Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-networking-limits-classic). Existem as seguintes opções:
        - **Adicionar um endereço**: adiciona-à sua lista de servidores DNS de rede virtual. Esta opção também regista o servidor DNS com o Azure. Se já registou um servidor DNS com o Azure, pode selecionar esse servidor DNS na lista.
        - **Remover um endereço**: junto do servidor que pretende remover, selecione **...** , em seguida, **remover**. A eliminação do servidor remove o servidor apenas a partir desta lista de rede virtual. O servidor DNS permanece registado no Azure para as outras redes virtuais a utilizar.
        - **Reordenar os endereços do servidor DNS**: é importante verificar que lista os servidores DNS na ordem correta para o seu ambiente. Listas de servidor DNS são utilizadas pela ordem que estão especificadas. Não funcionam como uma configuração round robin. Se o primeiro servidor DNS na lista pode ser acedido, o cliente utiliza esse servidor DNS, independentemente se o servidor DNS está a funcionar corretamente. Remova todos os servidores DNS que estão listados e, em seguida, adicioná-las novamente na ordem em que pretende.
        - **Alterar um endereço**: realce o servidor DNS na lista e, em seguida, introduza o novo endereço.
5. Selecione **Guardar**.
6. Reinicie as VMs que estejam ligadas à rede virtual, pelo que são atribuídas as novas definições de servidor DNS. VMs continuam a utilizar as definições de DNS atuais até que o se forem reiniciados.

**Comandos**

- CLI do Azure: [atualização do az rede vnet](/cli/azure/network/vnet#az_network_vnet_update)
- PowerShell: [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork)

## <a name="delete-a-virtual-network"></a>Eliminar uma rede virtual

É possível eliminar uma rede virtual só se existem não existem recursos ligados ao mesmo. Se existirem recursos ligados a nenhuma sub-rede na rede virtual, tem primeiro de eliminar os recursos que estão ligados a todas as sub-redes na rede virtual. Os passos que efetuar para eliminar um recurso variam consoante o recurso. Para saber como eliminar os recursos que estão ligados a sub-redes, leia a documentação para cada tipo de recurso que pretende eliminar. Para eliminar uma rede virtual:

1. Na caixa de pesquisa na parte superior do portal, introduza *redes virtuais* na caixa de pesquisa. Quando **redes virtuais** aparece nos resultados da pesquisa, selecionados-lo.
2. Na lista de redes virtuais, selecione a rede virtual que pretende eliminar.
3. Confirme que existem não existem dispositivos ligados à rede virtual selecionando **dispositivos ligados**, em **definições**. Se existirem dispositivos ligados, tem de as eliminar antes de poder eliminar a rede virtual. Se não houver nenhuma dispositivos ligados, selecione **descrição geral**.
4. Selecione **Eliminar**.
5. Para confirmar a eliminação da rede virtual, selecione **Sim**.

**Comandos**

- CLI do Azure: [eliminar a rede azure vnet](/cli/azure/network/vnet#az_network_vnet_delete)
- PowerShell: [Remove-AzureRmVirtualNetwork](/powershell/module/azurerm.network/remove-azurermvirtualnetwork)

## <a name="permissions"></a>Permissões

Para efetuar tarefas em redes virtuais, deve ser atribuída à conta para o [contribuinte de rede](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) função ou a um [personalizado](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) função atribuída as permissões adequadas listadas na seguinte tabela:

|Operação                                    |   Nome da operação                    |
|-------------------------------------------  |   --------------------------------  |
|Microsoft.Network/virtualNetworks/read       |   Obter Rede Virtual               |
|Microsoft.Network/virtualNetworks/write      |   Criar ou atualizar a rede Virtual  |
|Microsoft.Network/virtualNetworks/delete     |   Eliminar a rede Virtual            |

## <a name="next-steps"></a>Passos Seguintes

- Para criar uma VM e, em seguida, ligue-o a uma rede virtual, consulte [criar uma rede virtual e ligar as VMs](quick-create-portal.md#create-virtual-machines).
- Para filtrar o tráfego de rede entre sub-redes dentro de uma rede virtual, consulte [criar grupos de segurança de rede](virtual-networks-create-nsg-arm-pportal.md).
- Para o elemento de uma rede virtual a outra rede virtual, consulte o artigo [criar um peering de rede virtual](tutorial-connect-virtual-networks-portal.md).
- Para saber mais sobre as opções para ligar uma rede virtual a uma rede no local, consulte [sobre o Gateway de VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams).
