---
title: Criar, alterar ou eliminar uma interface de rede do Azure | Microsoft Docs
description: Saiba que uma interface de rede é como criar, alterar as definições e eliminar um.
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
ms.date: 07/24/2017
ms.author: jdial
ms.openlocfilehash: 49685d5b0e30356cab7f1f530bcc97e193d7fd90
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2018
---
# <a name="create-change-or-delete-a-network-interface"></a>Criar, alterar ou eliminar uma interface de rede

Saiba como criar, alterar as definições e eliminar uma interface de rede. Uma interface de rede permite que uma Máquina Virtual do Azure para comunicar com a internet, do Azure e recursos no local. Ao criar uma máquina virtual utilizando o portal do Azure, o portal cria uma interface de rede com as predefinições para si. Em vez disso, pode optar por criar interfaces de rede com definições personalizadas e adicione uma ou mais interfaces de rede a uma máquina virtual quando a criar. Também poderá pretender alterar as definições de interface de rede predefinido para uma interface de rede existente. Este artigo explica como criar uma interface de rede com definições personalizadas, alterar definições existentes, tais como a atribuição de (grupo de segurança de rede) do filtro de rede, atribuição de sub-rede, definições do servidor DNS e reencaminhamento IP e eliminar uma interface de rede.

Se necessário para adicionar, alterar ou remover os endereços IP para uma interface de rede, consulte [endereços IP gerir](virtual-network-network-interface-addresses.md). Se precisar de adicionar as interfaces de rede para ou remova as interfaces de rede de máquinas virtuais, consulte [interfaces de rede de adicionar ou remover](virtual-network-network-interface-vm.md).


## <a name="before-you-begin"></a>Antes de começar

Conclua as seguintes tarefas antes de concluir os passos em qualquer secção deste artigo:

- Se ainda não tiver uma conta do Azure, inscreva-se um [conta de avaliação gratuita](https://azure.microsoft.com/free).
- Se utilizar o portal, abra https://portal.azure.come inicie sessão com a sua conta do Azure.
- Se utilizar comandos do PowerShell para concluir tarefas neste artigo, quer executar os comandos [Shell de nuvem do Azure](https://shell.azure.com/powershell), ou através da execução do PowerShell do seu computador. O Azure Cloud Shell é um shell interativo gratuito que pode utilizar para executar os passos neste artigo. Tem as ferramentas comuns do Azure pré-instaladas e configuradas para utilização com a sua conta. Este tutorial requer o Azure PowerShell versão do módulo 5.2.0 ou posterior. Execute `Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.
- Se utilizar comandos de interface de linha de comandos (CLI) do Azure para concluir tarefas neste artigo, quer executar os comandos [Shell de nuvem do Azure](https://shell.azure.com/bash), ou executando a CLI do seu computador. Este tutorial requer a CLI do Azure versão 2.0.26 ou posterior. Execute `az --version` para localizar a versão instalada. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). Se estiver a executar localmente a CLI do Azure, também terá de executar `az login` para criar uma ligação com o Azure.

A conta, inicie sessão no Azure com deve ser atribuída a um permissões mínimas, para a função de contribuinte de rede para a sua subscrição. Para saber mais sobre a atribuição de funções e permissões a contas, consulte o artigo [funções incorporadas para controlo de acesso baseado em funções do Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).

## <a name="create-a-network-interface"></a>Criar uma interface de rede

Ao criar uma máquina virtual utilizando o portal do Azure, o portal cria uma interface de rede com as predefinições para si. Se tem em vez de especificar todas as definições de interface de rede, pode criar uma interface de rede com definições personalizadas e anexar a interface de rede a uma máquina virtual, ao criar a máquina virtual (utilizando o PowerShell ou a CLI do Azure). Também pode criar uma interface de rede e adicioná-lo a uma máquina virtual existente (utilizando o PowerShell ou a CLI do Azure). Para saber como criar uma máquina virtual com uma interface de rede existente ou adicionar ou remover as interfaces de rede de máquinas virtuais existentes, consulte [interfaces de rede de adicionar ou remover](virtual-network-network-interface-vm.md). Antes de criar uma interface de rede, tem de ter um existente [rede virtual](manage-virtual-network.md#create-a-virtual-network) a mesma localização e subscrição, criar uma interface de rede no.

1. Na caixa que contém o texto *procurar recursos* na parte superior do portal do Azure, escreva *interfaces de rede*. Quando **interfaces de rede** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione **+ adicionar** em **interfaces de rede**.
3. Introduza, ou selecione os valores para as seguintes definições e selecione **criar**:

    |Definição|Necessário?|Detalhes|
    |---|---|---|
    |Nome|Sim|O nome tem de ser exclusivo no grupo de recursos que selecionar. Ao longo do tempo, provavelmente terá de várias interfaces de rede na sua subscrição do Azure. Para sugestões ao criar uma convenção de nomenclatura gerir várias interfaces de rede mais fácil, consulte [convenções de nomenclatura](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions). O nome não pode ser alterado depois da interface de rede é criada.|
    |Rede virtual|Sim|Selecione a rede virtual para a interface de rede. Apenas pode atribuir uma interface de rede a uma rede virtual que existe na mesma subscrição e localização como a interface de rede. Quando é criada uma interface de rede, não é possível alterar a rede virtual que está atribuída a. A máquina virtual, que adicione a interface de rede também tem de existir na mesma localização e subscrição enquanto a interface de rede.|
    |Subrede|Sim|Selecione uma sub-rede dentro da rede virtual que selecionou. Pode alterar a sub-rede que a interface de rede está atribuída a depois de criado.|
    |Atribuição de endereços IP privados|Sim| Esta definição estiver a escolher o método de atribuição para o endereço IPv4. Escolha entre os seguintes métodos de atribuição: **dinâmico:** quando selecionar esta opção, o Azure atribui automaticamente o endereço disponível seguinte do espaço de endereços da sub-rede que selecionou. **Estática:** quando selecionar esta opção, tem de atribuir manualmente um endereço IP disponível a partir do espaço de endereços da sub-rede que selecionou. Endereços estáticos e dinâmicos não alterar até que altere-as ou a interface de rede é eliminada. Pode alterar o método de atribuição depois de criar a interface de rede. O servidor DHCP do Azure atribui este endereço para a interface de rede no sistema operativo da máquina virtual.|
    |Grupo de segurança de rede|Não| Deixe definido como **nenhum**, selecione um existente [grupo de segurança de rede](virtual-networks-nsg.md), ou [criar um grupo de segurança de rede](virtual-networks-create-nsg-arm-pportal.md). Grupos de segurança de rede permitem-lhe para filtrar o tráfego de rede que entra e sai de uma interface de rede. Pode aplicar zero ou um grupo de segurança de rede a uma interface de rede. Também pode ser aplicado a zero ou um grupo de segurança de rede para a sub-rede que está atribuída a interface de rede. Quando um grupo de segurança de rede é aplicado a uma interface de rede e a sub-rede que está atribuída a interface de rede, resultados inesperados, por vezes, ocorrerem. Para resolver problemas relacionados com grupos de segurança de rede aplicados a interfaces de rede e sub-redes, consulte o artigo [resolver problemas relacionados com grupos de segurança de rede](virtual-network-nsg-troubleshoot-portal.md#nsg).|
    |Subscrição|Sim|Selecione uma das suas Azure [subscrições](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). A máquina virtual, anexar uma interface de rede para e a rede virtual que liga-o para tem de existir na mesma subscrição.|
    |Endereço IP privado (IPv6)|Não| Se selecionar esta caixa de verificação, é atribuído um endereço IPv6 para a interface de rede, para além do endereço de IPv4 atribuído à interface de rede. Consulte o [IPv6](#IPv6) secção deste artigo para obter informações importantes sobre a utilização do IPv6 com interfaces de rede. Não é possível selecionar um método de atribuição para o endereço IPv6. Se optar por atribuir um endereço IPv6, que lhe é atribuída com o método dinâmico.
    |Nome de IPv6 (apenas aparece quando o **endereço IP privado (IPv6)** caixa de verificação está selecionada) |Sim, se o **endereço IP privado (IPv6)** caixa de verificação está selecionada.| Este nome é atribuído a uma configuração de IP secundária para a interface de rede. Para saber mais sobre as configurações de IP, consulte o artigo [ver as definições de interface de rede](#view-network-interface-settings).|
    |Grupo de recursos|Sim|Selecione um existente [grupo de recursos](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) ou criar um. Pode existir uma interface de rede no grupo de recursos idêntica ou diferente, a máquina virtual que o anexa, ou a rede virtual, ligue-o a.|
    |Localização|Sim|A máquina virtual, anexa uma interface de rede para e a rede virtual estabelecer a ligação que tem de existir na mesma [localização](https://azure.microsoft.com/regions), também referido como uma região.|

O portal não fornece a opção de atribuir um endereço IP público para a interface de rede quando cria, embora o portal de criar um endereço IP público e atribua-a uma interface de rede, ao criar uma máquina virtual utilizando o portal. Para saber como adicionar um endereço IP público para a interface de rede após a criação, consulte [endereços IP gerir](virtual-network-network-interface-addresses.md). Se pretender criar uma interface de rede com um endereço IP público, tem de utilizar a CLI ou o PowerShell para criar a interface de rede.

>[!Note]
> Azure atribui um endereço MAC para a interface de rede apenas depois da interface de rede está ligada a uma máquina virtual e a máquina virtual é iniciada pela primeira vez. Não é possível especificar o endereço de MAC Azure atribui à interface de rede. O endereço MAC permanecerá atribuído à interface de rede até que a interface de rede é eliminada ou o endereço IP privado atribuído à configuração de IP primária da interface de rede primária é alterado. Para saber mais sobre as configurações de IP e endereços IP, consulte o artigo [endereços IP gerir](virtual-network-network-interface-addresses.md)

**Comandos**

|Ferramenta|Comando|
|---|---|
|CLI|[az network nic create](/cli/azure/network/nic#az_network_nic_create)|
|PowerShell|[New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface#create)|

## <a name="view-network-interface-settings"></a>Ver definições de interface de rede

Pode ver e alterar a maioria das definições para uma interface de rede depois de criado. O portal não apresentar o sufixo DNS para a interface de rede. Pode utilizar o PowerShell ou a CLI do Azure [comandos](#view-settings-commands) para ver o sufixo DNS.

1. Na caixa que contém o texto *procurar recursos* na parte superior do portal do Azure, escreva *interfaces de rede*. Quando **interfaces de rede** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione a interface de rede que pretende ver ou alterar as definições da lista.
3. Os seguintes itens são listados para a interface de rede que selecionou:
    - **Descrição geral:** fornece informações sobre a interface de rede, tais como os endereços IP atribuídos, rede/sub-rede virtual que está atribuída a interface de rede e a máquina virtual, a interface de rede está ligada a (se está ligado a um). A imagem seguinte mostra as definições de descrição geral de uma interface de rede com o nome **mywebserver256**: ![descrição geral da interface de rede](./media/virtual-network-network-interface/nic-overview.png) pode mover uma interface de rede para um grupo de recursos diferente ou subscrição selecionando (**alterar**) junto a **grupo de recursos** ou **nome da subscrição**. Se mover a interface de rede, tem de mover todos os recursos relacionados com a interface de rede com o mesmo. Se a interface de rede está ligada a uma máquina virtual, por exemplo, tem também de mover a máquina virtual e outros recursos relacionados com a máquina virtual. Para mover uma interface de rede, consulte [mover recursos para um novo grupo de recursos ou subscrição](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json#use-portal). O artigo apresenta uma lista de pré-requisitos e como mover recursos com o portal do Azure, PowerShell e a CLI do Azure.
    - **Configurações de IP:** públicas e privados endereços IPv4 e IPv6 atribuídos para as configurações de IP estão listados aqui. Se um endereço IPv6 for atribuído a uma configuração de IP, o endereço não é apresentado. Para saber mais sobre as configurações de IP e como adicionar e remover endereços IP, consulte [endereços IP de configurar para uma interface de rede do Azure](virtual-network-network-interface-addresses.md). Reencaminhamento IP e a atribuição de sub-rede também são configuradas nesta secção. Para saber mais sobre estas definições, consulte [ativar ou desativar o reencaminhamento IP](#enable-or-disable-ip-forwarding) e [alterar a atribuição de sub-rede](#change-subnet-assignment).
    - **Servidores DNS:** pode especificar que servidor DNS uma interface de rede é atribuída pelos servidores DHCP do Azure. A interface de rede pode herdar a definição de rede virtual que a interface de rede está atribuída a ou ter uma definição personalizada que substitui a definição para a rede virtual que está atribuída a. Para modificar o que é apresentado, consulte [servidores DNS de alteração](#change-dns-servers).
    - **O grupo de segurança de rede (NSG):** mostra que NSG é associado à interface de rede (se aplicável). Um NSG contém regras de entrada e saídas para filtrar o tráfego de rede para a interface de rede. Se um NSG é associado à interface de rede, é apresentado o nome do NSG associado. Para modificar o que é apresentado, concluir os passos [gerir as associações de grupo de segurança de rede](virtual-network-manage-nsg-arm-portal.md#manage-associations).
    - **Propriedades:** apresenta definições sobre a interface de rede, incluindo o seu endereço de MAC (em branco se estiver a interface de rede não está ligada a uma máquina virtual) e a subscrição que existe da chave.
    - **Regras de segurança eficaz:** regras de segurança estão listadas se a interface de rede está ligada a uma máquina virtual em execução e um NSG é associado à interface de rede, a sub-rede está atribuído à ou ambos. Para saber mais sobre o que é apresentado, consulte o artigo [resolver problemas relacionados com grupos de segurança de rede](virtual-network-nsg-troubleshoot-portal.md#nsg). Para saber mais sobre NSGs, consulte [grupos de segurança de rede](virtual-networks-nsg.md).
    - **Rotas efetivas:** rotas listadas se a interface de rede está ligada a uma máquina virtual em execução. As rotas são uma combinação das rotas predefinidas do Azure, as rotas definidas pelo utilizador (UDR) e as rotas BGP que possam existir para a sub-rede que a interface de rede está atribuída a. Para saber mais sobre o que é apresentado, consulte o artigo [resolver rotas](virtual-network-routes-troubleshoot-portal.md#view-effective-routes-for-a-network-interface). Para saber mais sobre UDRs e predefinidos do Azure, consulte o artigo [rotas definidas pelo utilizador](virtual-networks-udr-overview.md).
    - **Definições comuns de Gestor de recursos do Azure:** para saber mais sobre definições comuns de Azure Resource Manager, consulte o artigo [registo de atividade](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs), [(IAM) do controlo de acesso](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control), [etiquetas](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags), [Bloqueia](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json), e [scripts de automatização](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group).

<a name="view-settings-commands"></a>**Comandos**

Se um endereço IPv6 for atribuído a uma interface de rede, a saída do PowerShell devolve o facto de que o endereço está atribuído, mas não devolve o endereço atribuído. Da mesma forma, a CLI devolve o facto de que o endereço está atribuído, mas devolve *nulo* no respetivo resultado para o endereço.

|Ferramenta|Comando|
|---|---|
|CLI|[lista de nic de redes AZ](/cli/azure/network/nic#az_network_nic_list) para ver as interfaces de rede na subscrição; [mostrar de nic de rede az](/cli/azure/network/nic#az_network_nic_show) para ver as definições para uma interface de rede|
|PowerShell|[Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) para ver as interfaces de rede nas definições de subscrição ou vista para uma interface de rede|

## <a name="change-dns-servers"></a>Alterar os servidores DNS

O servidor DNS é atribuído pelo servidor DHCP do Azure para a interface de rede dentro do sistema de operativo da máquina virtual. O servidor DNS atribuído é que a definição do servidor DNS é uma interface de rede. Para saber mais sobre as definições de resolução de nome de uma interface de rede, consulte o artigo [a resolução de nomes para máquinas virtuais](virtual-networks-name-resolution-for-vms-and-role-instances.md). A interface de rede pode herdar as definições de rede virtual, ou utilizar as suas próprias definições exclusivas que substituem a definição para a rede virtual.

1. Na caixa que contém o texto *procurar recursos* na parte superior do portal do Azure, escreva *interfaces de rede*. Quando **interfaces de rede** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione a interface de rede que pretende alterar um servidor DNS para a partir da lista.
3. Selecione **servidores DNS** em **definições**.
4. Selecione:
    - **Herdar a partir da rede virtual**: Escolha esta opção para herdar a definição do servidor DNS definida para a rede virtual que está atribuída a interface de rede. Ao nível da rede virtual, um servidor DNS personalizado ou o servidor DNS fornecidos pelo Azure está definido. O servidor DNS fornecidos pelo Azure pode resolver os nomes de anfitrião para recursos atribuídos à mesma rede virtual. FQDN deve ser utilizado para resolver para os recursos atribuídos a redes virtuais diferentes.
    - **Personalizada**: pode configurar o próprio servidor DNS para resolver nomes através de várias redes virtuais. Introduza o endereço IP do servidor que pretende utilizar como um servidor DNS. O endereço do servidor DNS que especificar é atribuído apenas para esta interface de rede e substitui qualquer definição de DNS para a rede virtual, que a interface de rede está atribuída.
5. Selecione **Guardar**.

**Comandos**

|Ferramenta|Comando|
|---|---|
|CLI|[atualização de nic de rede AZ](/cli/azure/network/nic#az_network_nic_update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface)|

## <a name="enable-or-disable-ip-forwarding"></a>Ativar ou desativar o reencaminhamento IP

Reencaminhamento IP permite que a máquina virtual de que uma interface de rede está ligada a:
- Receba o tráfego de rede não destinado a um dos endereços IP atribuídos a qualquer uma das configurações de IP atribuídas à interface de rede.
- Envie o tráfego de rede com um endereço IP de origem diferente daquela atribuídos a uma das configurações de IP de uma interface de rede.

A definição deve estar ativada para cada interface de rede que está ligado à máquina virtual que recebe o tráfego que a máquina virtual necessita de reencaminhar. Uma máquina virtual pode reencaminhar tráfego se possui várias interfaces de rede ou uma única interface de rede ligados ao mesmo. Enquanto o reencaminhamento IP é uma definição do Azure, a máquina virtual tem também de executar uma aplicação capaz de reencaminhar o tráfego, como a firewall, otimização de WAN e aplicações de balanceamento de carga. Quando uma máquina virtual está a executar aplicações de rede, a máquina virtual é frequentemente referida como uma aplicação virtual de rede. Pode ver uma lista de pronto para implementar aplicações virtuais de rede no [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). Reencaminhamento IP é normalmente utilizado com rotas definidas pelo utilizador. Para saber mais sobre as rotas definidas pelo utilizador, consulte o artigo [rotas definidas pelo utilizador](virtual-networks-udr-overview.md).

1. Na caixa que contém o texto *procurar recursos* na parte superior do portal do Azure, escreva *interfaces de rede*. Quando **interfaces de rede** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione a interface de rede que pretende ativar ou desativar o reencaminhamento para IP.
3. Selecione **configurações de IP** no **definições** secção.
4. Selecione **ativado** ou **desativado** (predefinição) para alterar a definição.
5. Selecione **Guardar**.

**Comandos**

|Ferramenta|Comando|
|---|---|
|CLI|[atualização de nic de rede AZ](/cli/azure/network/nic#az_network_nic_update)|
|PowerShell|[Set-AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface)|

## <a name="change-subnet-assignment"></a>Alterar a atribuição de sub-rede

Pode alterar a sub-rede, mas não a rede virtual, que está atribuída uma interface de rede.

1. Na caixa que contém o texto *procurar recursos* na parte superior do portal do Azure, escreva *interfaces de rede*. Quando **interfaces de rede** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione a interface de rede que pretende alterar a atribuição de sub-rede para.
3. Selecione **configurações de IP** em **definições**. Se quaisquer endereços IP privados para as configurações de IP listado ter **(estáticas)** junto-los, tem de alterar o método de atribuição de endereço IP dinâmico para seguindo os passos que se seguem. Todos os endereços IP privados devem ser atribuídos com o método de atribuição dinâmico para alterar a atribuição de sub-rede para a interface de rede. Se os endereços são atribuídos com o método dinâmico, continue a quinto passo. Se forem atribuídos os endereços IPv4 com o método de atribuição estática, conclua os passos seguintes para alterar o método de atribuição para dinâmica:
    - Selecione a configuração de IP que pretende alterar o método de atribuição de endereços IPv4 para da lista de configurações de IP.
    - Selecione **dinâmica** para o endereço IP privado **atribuição** método. Não é possível atribuir um endereço IPv6 com o método de atribuição estática.
    - Selecione **Guardar**.
4. Selecione a sub-rede que pretende mover a interface de rede a partir de **sub-rede** na lista pendente.
5. Selecione **Guardar**. Os novos endereços dinâmicos são atribuídos a partir do intervalo de endereços de sub-rede para a sub-rede de novo. Depois de atribuir a interface de rede para uma nova sub-rede, pode atribuir um endereço IPv4 estático a partir do novo intervalo de endereços de sub-rede se escolher. Para saber mais sobre adicionar, alterar e remover endereços IP para uma interface de rede, consulte [endereços IP gerir](virtual-network-network-interface-addresses.md).

**Comandos**

|Ferramenta|Comando|
|---|---|
|CLI|[az network nic ip-config update](/cli/azure/network/nic/ip-config#az_network_nic_ip_config_update)|
|PowerShell|[Set-AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/set-azurermnetworkinterfaceipconfig)|


## <a name="delete-a-network-interface"></a>Eliminar uma interface de rede

Pode eliminar uma interface de rede, desde que não está ligado a uma máquina virtual. Se está ligado a uma máquina virtual, deve primeiro coloque a máquina virtual no estado parado (desalocado) e desanexar a interface de rede da máquina virtual, antes de poder eliminar a interface de rede. Para anular a exposição de uma interface de rede de uma máquina virtual, conclua os passos no [desanexar uma interface de rede de uma máquina virtual](virtual-network-network-interface-vm.md#remove-a-network-interface-from-a-vm). Eliminar uma máquina virtual Desanexa todas as interfaces de rede ligadas ao mesmo, mas não eliminar as interfaces de rede.

1. Na caixa que contém o texto *procurar recursos* na parte superior do portal do Azure, escreva *interfaces de rede*. Quando **interfaces de rede** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione **...**  no lado direito da interface de rede que pretende eliminar a partir da lista de interfaces de rede.
3. Selecione **Eliminar**.
4. Selecione **Sim** para confirmar a eliminação da interface de rede.

Quando elimina uma interface de rede, são lançados a qualquer endereços MAC ou IP atribuídos ao mesmo.

**Comandos**

|Ferramenta|Comando|
|---|---|
|CLI|[az network nic delete](/cli/azure/network/nic#az_network_nic_delete)|
|PowerShell|[Remove-AzureRmNetworkInterface](/powershell/module/azurerm.network/remove-azurermnetworkinterface)|

## <a name="next-steps"></a>Passos Seguintes
Para criar uma máquina virtual com várias interfaces de rede ou endereços IP, consulte os artigos seguintes:

**Comandos**

|Tarefa|Ferramenta|
|---|---|
|Criar uma VM com vários NICs|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Criar uma VM de NIC único com vários endereços de IPv4|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Criar uma VM de NIC único com um endereço IPv6 privado (atrás de um balanceador de carga do Azure)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [modelo Azure Resource Manager](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
