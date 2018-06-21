---
title: Criar, alterar ou eliminar um grupo de segurança de rede do Azure | Microsoft Docs
description: Saiba como criar, alterar ou eliminar um grupo de segurança de rede.
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
ms.date: 04/05/2018
ms.author: jdial
ms.openlocfilehash: f2fe02a6e7e696fa2c0ab301e7469060d6bd4ab6
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295674"
---
# <a name="create-change-or-delete-a-network-security-group"></a>Criar, alterar ou eliminar um grupo de segurança de rede

Regras de segurança nos grupos de segurança de rede permitem-lhe filtrar o tipo de tráfego de rede que possam circular que entra e sai sub-redes da rede virtual e interfaces de rede. Se não estiver familiarizado com grupos de segurança de rede, consulte [descrição geral de grupo de segurança de rede](security-overview.md) para saber mais sobre os mesmos e concluir o [filtrar o tráfego de rede](tutorial-filter-network-traffic.md) tutorial para ter alguma experiência com a rede grupos de segurança.

## <a name="before-you-begin"></a>Antes de começar

Conclua as seguintes tarefas antes de concluir os passos em qualquer secção deste artigo:

- Se ainda não tiver uma conta do Azure, inscreva-se um [conta de avaliação gratuita](https://azure.microsoft.com/free).
- Se utilizar o portal, abra https://portal.azure.come inicie sessão com a sua conta do Azure.
- Se utilizar comandos do PowerShell para concluir tarefas neste artigo, quer executar os comandos [Shell de nuvem do Azure](https://shell.azure.com/powershell), ou através da execução do PowerShell do seu computador. O Azure Cloud Shell é um shell interativo gratuito que pode utilizar para executar os passos neste artigo. Tem as ferramentas comuns do Azure pré-instaladas e configuradas para utilização com a sua conta. Este tutorial requer o Azure PowerShell versão do módulo 5.4.1 ou posterior. Execute `Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Connect-AzureRmAccount` para criar uma ligação com o Azure.
- Se utilizar comandos de interface de linha de comandos (CLI) do Azure para concluir tarefas neste artigo, quer executar os comandos [Shell de nuvem do Azure](https://shell.azure.com/bash), ou executando a CLI do seu computador. Este tutorial requer a CLI do Azure versão 2.0.28 ou posterior. Execute `az --version` para localizar a versão instalada. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli). Se estiver a executar localmente a CLI do Azure, também terá de executar `az login` para criar uma ligação com o Azure.

A conta iniciar sessão ou ligar para o Azure com deve ser atribuída ao [contribuinte de rede](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) função ou a um [função personalizada](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) atribuída as ações adequadas listadas na [permissões ](#permissions).

## <a name="work-with-network-security-groups"></a>Trabalhar com grupos de segurança de rede

Pode criar, [ver todos os](#view-all-network-security-groups), [ver os detalhes das](#view-details-of-a-network-security-group), [alterar](#change-a-network-security-group), e [eliminar](#delete-a-network-security-group) um grupo de segurança de rede. Também pode [associar ou desassociar](#associate-or-dissociate-a-network-security-group-to-or-from-a-subnet-or-network-interface) um grupo de segurança de rede de uma interface de rede ou de sub-rede.

### <a name="create-a-network-security-group"></a>Criar um grupo de segurança de rede

Não há um limite para quantas pode criar por localização do Azure e subscrição de grupos de segurança de rede. Para obter mais detalhes, veja [Limites do Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

1. No canto superior esquerdo do portal, selecione **+ criar um recurso**.
2. Selecione **redes**, em seguida, selecione **grupo de segurança de rede**.
3. Introduza um **nome** para o grupo de segurança de rede, selecione o **subscrição**, crie um novo **grupo de recursos**, ou selecione um grupo de recursos existente, selecione um **Localização**e, em seguida, selecione **criar**.

**Comandos**

- CLI do Azure: [criar az rede nsg](/cli/azure/network/nsg#az-network-nsg-create)
- PowerShell: [novo AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup)

### <a name="view-all-network-security-groups"></a>Ver todos os grupos de segurança de rede

Na caixa de pesquisa na parte superior do portal, introduza *grupos de segurança de rede*. Quando **grupos de segurança de rede** aparecer nos resultados da pesquisa, selecione-o. Os grupos de segurança de rede que existem na sua subscrição são listados.

**Comandos**

- CLI do Azure: [lista de nsg az rede](/cli/azure/network/nsg#az-network-nsg-list)
- PowerShell: [Get AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermnetworksecuritygroup)

### <a name="view-details-of-a-network-security-group"></a>Ver detalhes de um grupo de segurança de rede

1. Na caixa de pesquisa na parte superior do portal, introduza *grupos de segurança de rede*. Quando **grupos de segurança de rede** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione o grupo de segurança de rede na lista que pretende ver detalhes. Em **definições** pode ver o **regras de segurança de entrada** e **regras de segurança de saída**, a **interfaces de rede** e  **Sub-redes** o grupo de segurança de rede se encontra associado a. Também pode ativar ou desativar **registos de diagnóstico** e vista **regras de segurança eficaz**. Para obter mais informações, consulte [registos de diagnóstico](virtual-network-nsg-manage-log.md) e [ver regras de segurança eficaz](diagnose-network-traffic-filter-problem.md).
3. Para saber mais sobre as definições do Azure comuns listadas, consulte os artigos seguintes:
    *   [Registo de atividades](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
    *   [Controlo de acesso (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
    *   [Etiquetas](../azure-resource-manager/resource-group-using-tags.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Bloqueia](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
    *   [Script de automatização](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Comandos**

- CLI do Azure: [mostrar de nsg az rede](/cli/azure/network/nsg#az-network-nsg-show)
- PowerShell: [Get AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermnetworksecuritygroup)

### <a name="change-a-network-security-group"></a>Alterar um grupo de segurança de rede

1. Na caixa de pesquisa na parte superior do portal, introduza *grupos de segurança de rede* na caixa de pesquisa. Quando **grupos de segurança de rede** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione o grupo de segurança de rede que pretende alterar. As alterações mais comuns são [adicionar](#create-a-security-rule) ou [remover](#delete-a-security-rule) regras de segurança e [Associating ou desassociar um grupo de segurança de rede para ou a partir de uma interface de rede ou de sub-rede](#associate-or-dissociate-a-network-security-group-to-or-from-a-subnet-or-network-interface).

**Comandos**

- CLI do Azure: [atualização de nsg az de rede](/cli/azure/network/nsg#az-network-nsg-update)
- PowerShell: [AzureRmNetworkSecurityGroup conjunto](/powershell/module/azurerm.network/set-azurermnetworksecuritygroup)

### <a name="associate-or-dissociate-a-network-security-group-to-or-from-a-subnet-or-network-interface"></a>Associar ou desassociar a um grupo de segurança de rede para ou a partir de uma interface de rede ou de sub-rede

Para associar um grupo de segurança de rede ou desassociar a um grupo de segurança de rede de uma interface de rede, consulte [associar um grupo de segurança de rede ou desassociar a um grupo de segurança de rede de uma interface de rede](virtual-network-network-interface.md#associate-or-dissociate-a-network-security-group). Para associar um grupo de segurança de rede ou desassociar a um grupo de segurança de rede de sub-rede, consulte [alterar definições da sub-rede](virtual-network-manage-subnet.md#change-subnet-settings).

### <a name="delete-a-network-security-group"></a>Eliminar um grupo de segurança de rede

Se um grupo de segurança de rede se encontra associado a quaisquer sub-redes ou interfaces de rede, não pode ser eliminada. [Desassociar](#associate-or-dissociate-a-network-security-group-to-or-from-a-resource) um grupo de segurança de rede de todas as sub-redes e interfaces de rede antes de tentar eliminá-lo.

1. Na caixa de pesquisa na parte superior do portal, introduza *grupos de segurança de rede* na caixa de pesquisa. Quando **grupos de segurança de rede** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione o grupo de segurança de rede que pretende eliminar a partir da lista.
3. Selecione **eliminar**e, em seguida, selecione **Sim**.

**Comandos**

- CLI do Azure: [az rede nsg eliminar](/cli/azure/network/nsg#az-network-nsg-delete)
- PowerShell: [AzureRmNetworkSecurityGroup remover](/powershell/module/azurerm.network/remove-azurermnetworksecuritygroupp) 

## <a name="work-with-security-rules"></a>Trabalhar com regras de segurança

Um grupo de segurança de rede contém zero ou mais regras de segurança. Pode criar, [ver todos os](#view-all-security-rules), [ver os detalhes das](#view-details-of-a-security-rule), [alterar](#change-a-security-rule), e [eliminar](#delete-a-security-rule) uma regra de segurança.

### <a name="create-a-security-rule"></a>Criar uma regra de segurança

Não há um limite para quantas regras por grupo de segurança de rede podem criar por localização do Azure e subscrição. Para obter mais detalhes, veja [Limites do Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

1. Na caixa de pesquisa na parte superior do portal, introduza *grupos de segurança de rede* na caixa de pesquisa. Quando **grupos de segurança de rede** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione o grupo de segurança de rede na lista que pretende adicionar uma regra de segurança para.
3. Selecione **regras de segurança de entrada** em **definições**. São apresentadas várias regras existentes. Algumas das regras que não possa ter adicionado. Quando é criado um grupo de segurança de rede, várias regras de segurança predefinidas são criadas no mesmo. Para obter mais informações, consulte [predefinido regras de segurança](security-overview.md#default-security-rules).  Não é possível eliminar regras de segurança predefinidas, mas pode substitui-los com regras que tenham uma prioridade mais alta.
4. <a name = "security-rule-settings"></a>Selecione **+ adicionar**.  Selecionar ou adicionar valores para as seguintes definições e, em seguida, selecione **OK**:
    
    |Definição  |Valor  |Detalhes  |
    |---------|---------|---------|
    |Origem     | Selecione **qualquer**, **endereços IP**, ou **Service Tag**.        | Se selecionar **endereços IP**, em seguida, tem de especificar **intervalos de endereços/CIDR de IP de origem**. Pode especificar um valor único ou uma lista de valores separados por vírgulas de vários valores. Um exemplo de vários valores é 10.0.0.0/16, 192.188.1.1. Existem limites para o número de valores que pode especificar. Consulte [Azure limita](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) para obter mais detalhes. Se selecionar **etiquetas de serviço**, em seguida, tem de selecionar uma tag de serviço. Uma tag de serviço é um identificador predefinido para uma categoria de endereços IP. Para obter mais informações sobre etiquetas de serviço disponível e que cada etiqueta representa, consulte [etiquetas de serviço](security-overview.md#service-tags). Se o endereço IP que especificou é atribuído a uma máquina virtual do Azure, certifique-se de que especifica o endereço IP privado, não o endereço IP público, se estiver atribuído um endereço IP público para a máquina virtual. As regras de segurança são processadas depois Azure traduz o endereço IP público para um endereço IP privado para regras de segurança de entrada e antes de Azure traduz um endereço IP privado para um endereço IP público para regras de saída. Para obter mais informações sobre endereços IP públicos e privados no Azure, consulte o artigo [tipos de endereços IP](virtual-network-ip-addresses-overview-arm.md).        |
    |Intervalo de portas de origem     | Especifique uma porta única, como 80, um intervalo de portas, por exemplo 1024-65535 ou uma lista separada por vírgulas de portas único e/ou intervalos de portas, tais como 80, 1024-65535. Introduza um asterisco para permitir tráfego em qualquer porta. | As portas e intervalos de especificam o tráfego de portas é permitido ou negado pela regra. Existem limites para o número de portas, que pode especificar. Consulte [Azure limita](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) para obter mais detalhes.  |
    |Destino     | Selecione **qualquer**, **endereços IP**, ou **rede Virtual**.        | Se selecionar **endereços IP**, em seguida, tem de especificar **intervalos de endereços/CIDR de destino IP**. Semelhante ao **origem** e **intervalos de endereços/CIDR de IP de origem**, pode especificar um único ou vários endereços ou intervalos e existem limites para o número especificar. Selecionar **rede Virtual**, que é uma etiqueta de serviço, significa que o tráfego é permitida para todos os endereços IP num espaço de endereços da rede virtual. Se o endereço IP que especificou é atribuído a uma máquina virtual do Azure, certifique-se de que especifica o endereço IP privado, não o endereço IP público, se estiver atribuído um endereço IP público para a máquina virtual. As regras de segurança são processadas depois Azure traduz o endereço IP público para um endereço IP privado para regras de segurança de entrada e antes de Azure traduz um endereço IP privado para um endereço IP público para regras de saída. Para obter mais informações sobre endereços IP públicos e privados no Azure, consulte o artigo [tipos de endereços IP](virtual-network-ip-addresses-overview-arm.md).        |
    |Intervalos de portas de destino     | Especifique um valor único ou a lista de valores separados por vírgulas. | Semelhante ao **intervalos de portas de origem**, pode especificar uma única ou várias portas e intervalos e existem limites para o número especificar. |
    |Protocolo     | Selecione **qualquer**, **TCP**, ou **UDP**.        |         |
    |Ação     | Selecione **permitir** ou **negar**.        |         |
    |Prioridade     | Introduza um valor entre 100-4096 que seja exclusivo para todas as regras de segurança dentro do grupo de segurança de rede. |As regras são processadas pela ordem de prioridade. Menor número, maior prioridade. É recomendado que mantenha um intervalo entre os números de prioridade quando criar regras, tais como 100, 200, 300. Inclusão de intervalos torna mais fácil adicionar no futuro regras que poderá ser necessário efetuar superior ou inferior a regras existentes.         |
    |Nome     | Um nome exclusivo para a regra dentro do grupo de segurança de rede.        |  O nome pode ter até 80 carateres. Tem de começar com uma letra ou um número, terminar com uma letra, um número ou um caráter de sublinhado e pode conter apenas letras, números, carateres de sublinhado, pontos ou hífenes.       |
    |Descrição     | Uma descrição opcional.        |         |

    Não é possível especificar um [grupo de segurança da aplicação](#work-with-application-security-groups) para o **origem** ou **destino** definições através do portal. No entanto, pode utilizar a CLI do Azure ou o PowerShell. As definições para **regras de segurança de saída** são semelhantes, pelo que não são abrangidos separadamente.

**Comandos**

- CLI do Azure: [criar regras de nsg az rede](/cli/azure/network/nsg/rule#az-network-nsg-rule-create)
- PowerShell: [novo AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig)

### <a name="view-all-security-rules"></a>Ver todas as regras de segurança

Um grupo de segurança de rede contém zero ou várias regras. Para saber mais sobre as informações apresentadas quando visualizar as regras, consulte [descrição geral de grupo de segurança de rede](security-overview.md).

1. Na caixa de pesquisa na parte superior do portal, introduza *grupos de segurança de rede*. Quando **grupos de segurança de rede** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione o grupo de segurança de rede na lista que pretende ver regras para.
3. Selecione **regras de segurança de entrada** ou **regras de segurança de saída** em **definições**.

A lista contém quaisquer regras que criou e o grupo de segurança de rede [predefinido regras de segurança](security-overview.md#default-security-rules).

**Comandos**

- CLI do Azure: [lista de regras de nsg de rede az](/cli/azure/network/nsg/rule#az-network-nsg-rule-list)
- PowerShell: [Get AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/get-azurermnetworksecurityruleconfig)

### <a name="view-details-of-a-security-rule"></a>Ver detalhes de uma regra de segurança

1. Na caixa de pesquisa na parte superior do portal, introduza *grupos de segurança de rede*. Quando **grupos de segurança de rede** aparecer nos resultados da pesquisa, selecione-o.
2. Selecione o grupo de segurança de rede que pretende ver os detalhes de uma regra de segurança para.
3. Selecione **regras de segurança de entrada** ou **regras de segurança de saída** em **definições**.
4. Selecione a regra que pretende ver detalhes. Para obter uma explicação detalhada de todas as definições, consulte [as definições da regra de segurança](#security-rule-settings).

**Comandos**

- CLI do Azure: [mostrar de regras de nsg de rede az](/cli/azure/network/nsg/rule#az-network-nsg-rule-show)
- PowerShell: [Get AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/get-azurermnetworksecurityruleconfig)

### <a name="change-a-security-rule"></a>Alterar uma regra de segurança

1. Execute os passos do [ver os detalhes de uma regra de segurança](#view-details-of-a-security-rule).
2. Alterar as definições conforme pretendido e, em seguida, selecione **guardar**. Para obter uma explicação detalhada de todas as definições, consulte [as definições da regra de segurança](#security-rule-settings).

**Comandos**

- CLI do Azure: [actualização da regra az rede nsg](/cli/azure/network/nsg/rule#az-network-nsg-rule-update)
- PowerShell: [AzureRmSecurityRuleConfig conjunto](/powershell/module/azurerm.network/set-azurermnetworksecurityruleconfig)

### <a name="delete-a-security-rule"></a>Eliminar uma regra de segurança

1. Execute os passos do [ver os detalhes de uma regra de segurança](#view-details-of-a-security-rule).
2. Selecione **eliminar**e, em seguida, selecione **Sim**.

**Comandos**

- CLI do Azure: [az rede nsg regra elimine](/cli/azure/network/nsg/rule#az-network-nsg-rule-delete)
- PowerShell: [AzureRmSecurityRuleConfig remover](/powershell/module/azurerm.network/remove-azurermnetworksecurityruleconfig)


## <a name="work-with-application-security-groups"></a>Trabalhar com grupos de segurança de aplicações

Um grupo de segurança da aplicação contém zero ou mais interfaces de rede. Para obter mais informações, consulte [grupos de segurança de aplicações](security-overview.md#application-security-groups). Não é possível trabalhar com grupos de segurança da aplicação no portal, mas pode utilizar o PowerShell ou a CLI do Azure. Todas as interfaces de rede num grupo de segurança da aplicação tem de existir na mesma rede virtual. A primeira interface de rede adicionada a um grupo de segurança da aplicação determina qual todas as interfaces de rede subsequentes tem de constar de rede virtual. Para saber como adicionar uma interface de rede a um grupo de segurança de aplicações, consulte [adicionar uma interface de rede a um grupo de segurança de aplicação](virtual-network-network-interface.md#add-to-or-remove-from-application-security-groups).

### <a name="create-an-application-security-group"></a>Criar um grupo de segurança de aplicações

- CLI do Azure: [az rede asg criar](/cli/azure/network/asg#az-network-asg-create)
- PowerShell: [novo AzureRmApplicationSecurityGroup](/powershell/module/azurerm.network/new-azurermapplicationsecuritygroup)

### <a name="view-all-application-security-groups"></a>Ver todos os grupos de segurança de aplicações

- CLI do Azure: [lista de asg az rede](/cli/azure/network/asg#az-network-asg-list)
- PowerShell: [Get AzureRmApplicationSecurityGroup](/powershell/module/azurerm.network/get-azurermapplicationsecuritygroup)

### <a name="view-details-of-a-specific-application-security-group"></a>Ver detalhes de um grupo de segurança de aplicação específica

- CLI do Azure: [mostrar de asg az rede](/cli/azure/network/asg#az-network-asg-show)
- PowerShell: [Get AzureRmApplicationSecurityGroup](/powershell/module/azurerm.network/get-azurermapplicationsecuritygroup)

### <a name="change-an-application-security-group"></a>Alterar um grupo de segurança da aplicação

Enquanto pode alterar algumas definições, tais como as etiquetas e permissões para um grupo de segurança de aplicação existente, não é possível alterar o respetivo nome ou localização.

- CLI do Azure: [atualização de asg az de rede](/cli/azure/network/asg#az-network-asg-update)
- Do PowerShell: Cmdlet nenhum PowerShell.

### <a name="delete-an-application-security-group"></a>Eliminar um grupo de segurança da aplicação

Não é possível eliminar um grupo de segurança da aplicação, se tiver quaisquer interfaces de rede no mesmo. Remova todas as interfaces de rede do grupo de segurança de aplicação ao alterar as definições de interface de rede, ou eliminar as interfaces de rede. Para obter mais informações, consulte [adicionar a ou remover uma interface de rede de grupos de segurança da aplicação](virtual-network-network-interface.md#add-to-or-remove-from-application-security-groups) ou [eliminar uma interface de rede](virtual-network-network-interface.md#delete-a-network-interface).

**Comandos**

- CLI do Azure: [az rede asg eliminar](/cli/azure/network/asg#az-network-asg-delete)
- PowerShell: [AzureRmApplicationSecurityGroup remover](/powershell/module/azurerm.network/remove-azurermapplicationsecuritygroup)

## <a name="permissions"></a>Permissões

Para efetuar tarefas em grupos de segurança de rede, as regras de segurança e grupos de segurança de aplicações, tem de ser atribuída à conta para o [contribuinte de rede](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) função ou a um [função personalizada](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) que é atribuído a permissões adequadas listadas nas tabelas seguintes:

### <a name="network-security-groups"></a>Grupos de segurança de rede

| Ação                                                        |   Nome                                                                |
|-------------------------------------------------------------- |   -------------------------------------------                         |
| Microsoft.Network/ruleTables/read                             |   Obter grupo de segurança de rede                                          |
| Microsoft.Network/ruleTables/write                            |   Criar ou atualizar o grupo de segurança de rede                             |
| Microsoft.Network/ruleTables/delete                           |   Eliminar grupo de segurança de rede                                       |
| Microsoft.Network/ruleTables/join/action                      |   Associar um grupo de segurança de rede a uma interface de rede ou de sub-rede |
| Microsoft.Network/ruleTables/rules/read                       |   Obter regra                                                            |
| Microsoft.Network/ruleTables/rules/write                      |   Criar ou atualizar regra                                               |
| Microsoft.Network/ruleTables/rules/delete                     |   Eliminar a regra                                                         |
| Microsoft.Network/networkInterfaces/effectiveruleTable/action |   Obter grupo de segurança de rede eficiente de Interface de rede              |
| Microsoft.Network/networkWatchers/nextHop/action              |   Obtém o salto seguinte a partir de uma VM                                         |

### <a name="application-security-groups"></a>Grupos de segurança de aplicações

| Ação                                                                     | Nome                                                     |
| --------------------------------------------------------------             | -------------------------------------------              |
| Microsoft.Network/applicationSecurityGroups/joinIpConfiguration/action     | Associar uma configuração de IP a um grupo de segurança de aplicação|
| Microsoft.Network/applicationSecurityGroups/joinNetworkSecurityRule/action | Associar uma regra de segurança a um grupo de segurança de aplicação    |
| Microsoft.Network/applicationSecurityGroups/read                           | Obter um grupo de segurança da aplicação                        |
| Microsoft.Network/applicationSecurityGroups/write                          | Criar ou atualizar um grupo de segurança da aplicação           |
| Microsoft.Network/applicationSecurityGroups/delete                         | Eliminar um grupo de segurança da aplicação                     |

## <a name="next-steps"></a>Passos Seguintes

- Criar uma rede ou o grupo de segurança da aplicação utilizando [PowerShell](powershell-samples.md) ou [CLI do Azure](cli-samples.md) scripts ou utilizar o Azure de exemplo [modelos do Resource Manager](template-samples.md)
- Criar e aplicar [política do Azure](policy-samples.md) para redes virtuais
