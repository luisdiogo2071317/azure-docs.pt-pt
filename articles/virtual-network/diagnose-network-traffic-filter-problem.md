---
title: Diagnosticar um problema de filtro de tráfego de rede de máquina virtual | Microsoft Docs
description: Saiba como diagnosticar um problema de filtro de tráfego de rede de máquina virtual visualizando as regras de segurança eficaz para uma máquina virtual.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: a54feccf-0123-4e49-a743-eb8d0bdd1ebc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2018
ms.author: jdial
ms.openlocfilehash: 1c33a75363eec2b4e338ba64e3d1ad877d8b1610
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757232"
---
# <a name="diagnose-a-virtual-machine-network-traffic-filter-problem"></a>Diagnosticar um problema de filtro de tráfego de rede de máquina virtual

Neste artigo, irá aprender a diagnosticar o problema de filtro de tráfego de rede visualizando as regras de segurança de grupo (NSG) de segurança de rede que são eficazes para uma máquina virtual (VM).

Os NSGs permitem-lhe controlar os tipos de tráfego que fluxo que entra e sai de uma VM. Pode associar um NSG a uma sub-rede numa rede virtual do Azure, uma interface de rede ligada a uma VM ou ambos. As regras de segurança eficaz aplicadas a uma interface de rede são uma agregação das regras que existe no NSG associado a uma interface de rede e a sub-rede a interface de rede está a ser. As regras no NSGs diferentes, por vezes, podem entrar em conflito entre si e afetar a conectividade de rede da VM. Pode ver todas as regras de segurança eficaz de NSGs que são aplicadas em interfaces de rede da VM. Se não estiver familiarizado com conceitos NSG, de interface de rede ou de rede virtual, consulte [descrição geral de rede Virtual](virtual-networks-overview.md), [interface de rede](virtual-network-network-interface.md), e [descrição geraldegruposdesegurançaderede](security-overview.md).

## <a name="scenario"></a>Cenário

Se tenta estabelecer ligação a uma VM através da porta 80 a partir da internet, mas a ligação falha. Para determinar por que motivo não pode aceder a porta 80 da Internet, pode ver as regras de segurança eficaz para uma interface de rede através do Azure [portal](#diagnose-using-azure-portal), [PowerShell](#diagnose-using-powershell), ou o [CLI do Azure](#diagnose-using-azure-cli).

Os passos que se seguem partem do princípio de que tem uma VM existente para ver as regras de segurança eficaz para. Se não tiver uma VM existente, primeiro implementar um [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou [Windows](../virtual-machines/windows/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM para concluir as tarefas neste artigo com. Os exemplos neste artigo são para uma VM chamada *myVM* com uma interface de rede com o nome *myVMVMNic*. A interface de rede e VM estão num grupo de recursos com o nome *myResourceGroup*e que estão no *EUA Leste* região. Altere os valores nos passos, conforme adequado, para a VM diagnosticar o problema para.

## <a name="diagnose-using-azure-portal"></a>Diagnosticar através do portal do Azure

1. Iniciar sessão do Azure [portal](https://portal.azure.com) com uma conta do Azure que tenha o [as permissões necessárias](virtual-network-network-interface.md#permissions).
2. Na parte superior do portal do Azure, introduza o nome da VM na caixa de pesquisa. Quando o nome da VM for apresentada nos resultados da pesquisa, selecione-a.
3. Em **definições**, selecione **redes**, conforme mostrado na imagem seguinte:

    ![Regras de segurança de vista](./media/diagnose-network-traffic-filter-problem/view-security-rules.png)

    As regras que vê listadas na imagem anterior são para uma interface de rede com o nome **myVMVMNic**. Verá que existem **regras de porta de entrada** para a interface de rede de dois grupos de segurança de rede diferente:- **mySubnetNSG**: associados à sub-rede da interface de rede está a ser.
        - **myVMNSG**: associado à interface de rede na VM com o nome **myVMVMNic**.

    A regra com o nome **DenyAllInBound** é o que está a impedir comunicação de entrada para a VM através da porta 80, a internet, conforme descrito no [cenário](#scenario). As listas de regra *0.0.0.0/0* para **origem**, que inclui a internet. Nenhuma outra regra com uma prioridade mais alta (número) permite que a porta 80 entrada. Para permitir que a porta 80 entrada para a VM a partir da internet, consulte [resolver um problema](#resolve-a-problem). Para obter mais informações sobre regras de segurança e a forma como o Azure aplica-as, consulte [grupos de segurança de rede](security-overview.md).

    Na parte inferior da imagem, verá também **regras de porta de saída**. Em que são as regras de porta de saída para a interface de rede. Apesar da imagem apenas mostra quatro regras de entrada para cada NSG, NSGs podem ter muitos mais de quatro regras. A imagem, é apresentado o **VirtualNetwork** em **origem** e **destino** e **AzureLoadBalancer** em  **ORIGEM**. **VirtualNetwork** e **AzureLoadBalancer** são [etiquetas de serviço](security-overview.md#service-tags). Serviço etiquetas representam um grupo de prefixos de endereços IP para ajudar a minimizar a complexidade para a criação de regra de segurança.

4. Certifique-se de que a VM se encontra em execução estada e, em seguida, selecione **regras de segurança eficaz**, conforme mostrado na imagem anterior, para ver as regras de segurança eficaz, mostradas na imagem seguinte:

    ![Regras de segurança eficaz de vista](./media/diagnose-network-traffic-filter-problem/view-effective-security-rules.png)

    As regras listadas são que as mesmas conforme mostrado no passo 3, apesar de existirem diferentes separadores para o NSG associado à interface de rede e a sub-rede. Como pode ver na imagem, apenas as primeiro 50 regras são apresentadas. Para transferir um ficheiro. csv que contém todas as regras, selecione **transferir**.

    Para ver os prefixos que cada etiqueta do serviço representa, selecione uma regra, tal como a regra com o nome **AllowAzureLoadBalancerInbound**. A imagem seguinte mostra os prefixos do **AzureLoadBalancer** etiqueta do serviço:

    ![Regras de segurança eficaz de vista](./media/diagnose-network-traffic-filter-problem/address-prefixes.png)

    Embora o **AzureLoadBalancer** tag de serviço só representa um prefixo, outras etiquetas de serviço representam vários prefixos.

4. Os passos anteriores mostrou as regras de segurança para uma interface de rede com o nome **myVMVMNic**, mas também viu uma interface de rede com o nome **myVMVMNic2** em algumas das imagens anteriores. A VM neste exemplo tem duas interfaces de rede ligadas ao mesmo. As regras de segurança eficaz podem ser diferentes para cada interface de rede.

    Para ver as regras para o **myVMVMNic2** interface de rede, selecione-o. Como é mostrado na imagem que se segue, a interface de rede tem as mesmas regras associadas à sua sub-rede que o **myVMVMNic** interface, de rede porque ambas as interfaces de rede estão na mesma sub-rede. Quando associa um NSG a uma sub-rede, as respetivas regras são aplicadas a todas as interfaces de rede na sub-rede.

    ![Regras de segurança de vista](./media/diagnose-network-traffic-filter-problem/view-security-rules2.png)

    Ao contrário do **myVMVMNic** interface de rede, o **myVMVMNic2** interface de rede não tem um grupo de segurança de rede associado. Cada interface de rede e sub-rede podem ter zero ou um NSG associado ao mesmo. O NSG associado a cada interface de rede ou a sub-rede pode ser o mesmo, ou de outro. Pode associar o mesmo grupo de segurança de rede para o número de interfaces de rede e sub-redes que escolher.

Apesar de regras de segurança eficaz foram visualizadas através da VM, também pode ver as regras de segurança eficaz através de uma:
- **Interface de rede individuais**: Saiba como [ver uma interface de rede](virtual-network-network-interface.md#view-network-interface-settings).
- **Individual NSG**: Saiba como [ver um NSG](manage-network-security-group.md#view-details-of-a-network-security-group).

## <a name="diagnose-using-powershell"></a>Diagnosticar através do PowerShell

Pode executar os comandos que se seguem na [Shell de nuvem do Azure](https://shell.azure.com/powershell), ou através da execução do PowerShell do seu computador. A Shell de nuvem do Azure é uma shell interativa livre. Tem as ferramentas comuns do Azure pré-instaladas e configuradas para utilização com a sua conta. Se executar o PowerShell a partir do seu computador, terá do *AzureRM* módulo do PowerShell, versão 6.0.1 ou posterior. Executar `Get-Module -ListAvailable AzureRM` no seu computador, para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente do PowerShell, também terá de executar `Login-AzureRmAccount` iniciar sessão no Azure com uma conta que tenha o [as permissões necessárias](virtual-network-network-interface.md#permissions)].

Obter as regras de segurança eficaz para uma interface de rede com [Get-AzureRmEffectiveNetworkSecurityGroup](/powershell/module/azurerm.network/get-azurermeffectivenetworksecuritygroup). O exemplo seguinte obtém as regras de segurança eficaz para uma interface de rede com o nome *myVMVMNic*, que é um grupo de recursos denominado *myResourceGroup*:

```azurepowershell-interactive
Get-AzureRmEffectiveNetworkSecurityGroup `
  -NetworkInterfaceName myVMVMNic interface `
  -ResourceGroupName myResourceGroup
```

Resultado é devolvido no formato json. Para compreender a saída, consulte [interpretar a saída do comando](#interpret-command-output).
Resultado é devolvido apenas se um NSG é associado a interface de rede, a sub-rede que a interface de rede está a ser ou ambos. A VM tem de estar no estado de execução. Uma VM pode ter várias interfaces de rede com NSGs diferentes aplicados. Quando a resolução de problemas, execute o comando para cada interface de rede.

Se ainda está a ter um problema de conectividade, consulte o artigo [diagnóstico adicional](#additional-diagnosis) e [considerações](#considerations).

Se não souber o nome de uma interface de rede, mas souber o nome da interface de rede está ligada à VM, os seguintes comandos devolvem os IDs de todas as interfaces de rede ligados a uma VM:

```azurepowershell-interactive
$VM = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroup
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

Se utilizar comandos de interface de linha de comandos (CLI) do Azure para concluir tarefas neste artigo, quer executar os comandos [Shell de nuvem do Azure](https://shell.azure.com/bash), ou executando a CLI do seu computador. Este artigo requer a CLI do Azure versão 2.0.32 ou posterior. Execute `az --version` para localizar a versão instalada. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli). Se estiver a executar localmente a CLI do Azure, também terá de executar `az login` e iniciar sessão no Azure com uma conta que tenha o [as permissões necessárias](virtual-network-network-interface.md#permissions).

Obter as regras de segurança eficaz para uma interface de rede com [az rede nic lista-eficaz-nsg](/cli/azure/network/nic#az-network-nic-list-effective-nsg). O exemplo seguinte obtém as regras de segurança eficaz para uma interface de rede com o nome *myVMVMNic* que está a ser um grupo de recursos denominado *myResourceGroup*:

```azurecli-interactive
az network nic list-effective-nsg \
  --name myVMVMNic \
  --resource-group myResourceGroup
```

Resultado é devolvido no formato json. Para compreender a saída, consulte [interpretar a saída do comando](#interpret-command-output).
Resultado é devolvido apenas se um NSG é associado a interface de rede, a sub-rede que a interface de rede está a ser ou ambos. A VM tem de estar no estado de execução. Uma VM pode ter várias interfaces de rede com NSGs diferentes aplicados. Quando a resolução de problemas, execute o comando para cada interface de rede.

Se ainda está a ter um problema de conectividade, consulte o artigo [diagnóstico adicional](#additional-diagnosis) e [considerações](#considerations).

Se não souber o nome de uma interface de rede, mas souber o nome da interface de rede está ligada à VM, os seguintes comandos devolvem os IDs de todas as interfaces de rede ligados a uma VM:

```azurecli-interactive
az vm show \
  --name myVM \
  --resource-group myResourceGroup
```

Na saída devolvida, consulte informações semelhantes ao seguinte exemplo:

```azurecli
"networkProfile": {
    "additionalProperties": {},
    "networkInterfaces": [
      {
        "additionalProperties": {},
        "id": "/subscriptions/<ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic",
        "primary": true,
        "resourceGroup": "myResourceGroup"
      },
```

Na saída anterior, o nome de interface de rede está *myVMVMNic interface*.

## <a name="interpret-command-output"></a>Interpretar a saída do comando

Independentemente se utilizou o [PowerShell](#diangose-using-powershell), ou o [CLI do Azure](#diagnose-using-azure-cli) para diagnosticar o problema, poderá recebe um resultado que contém as seguintes informações:

- **NetworkSecurityGroup**: O ID do grupo de segurança de rede.
- **Associação**: se o grupo de segurança de rede se encontra associado a um *NetworkInterface* ou *sub-rede*. Se um NSG é associado a ambos, o resultado é devolvido com **NetworkSecurityGroup**, **associação**, e **EffectiveSecurityRules**, para cada NSG. Se o NSG é associado ou desassociar imediatamente antes de executar o comando para ver as regras de segurança eficaz, poderá ter de aguardar alguns segundos para que a alteração para refletir no resultado do comando.
- **EffectiveSecurityRules**: uma explicação de cada propriedade está detalhada na [criar uma regra de segurança](manage-network-security-group.md#create-a-security-rule). Regra de nomes precedidos pela *defaultSecurityRules /* é a predefinição das regras de segurança que existem em cada NSG. Regra de nomes precedidos pela *securityRules /* são regras que criou. As regras que especificam um [service tag](security-overview.md#service-tags), tais como **Internet**, **VirtualNetwork**, e **AzureLoadBalancer** para o  **destinationAddressPrefix** ou **sourceAddressPrefix** propriedades, também de ter valores para o **expandedDestinationAddressPrefix** propriedade. O **expandedDestinationAddressPrefix** propriedade apresenta uma lista de todos os prefixos de endereço representados pela tag de serviço.

Se vir regras duplicadas listadas no resultado, é porque um NSG é associado à interface de rede e a sub-rede. Os NSGs têm as mesmas regras de predefinidas e podem ter regras duplicadas adicionais, se tiver criado as suas próprias regras que são os mesmos em ambos os NSGs.

A regra com o nome **defaultSecurityRules/DenyAllInBound** é o que está a impedir comunicação de entrada para a VM através da porta 80, a internet, conforme descrito no [cenário](#scenario). Nenhuma outra regra com uma prioridade mais alta (número) permite que a porta 80 de entrada da internet.

## <a name="resolve-a-problem"></a>Resolver um problema

Se utilizar o Azure [portal](#diagnose-using-azure-portal), [PowerShell](#diagnose-using-powershell), ou o [CLI do Azure](#diagnose-using-azure-cli) para diagnosticar o problema apresentado a [cenário](#scenario) deste artigo, a solução é criar uma regra de segurança de rede com as seguintes propriedades:

| Propriedade                | Valor                                                                              |
|---------                |---------                                                                           |
| Origem                  | Qualquer                                                                                |
| Intervalo de portas de origem      | Qualquer                                                                                |
| Destino             | O endereço IP da VM, um intervalo de endereços IP ou todos os endereços de sub-rede. |
| Intervalos de portas de destino | 80                                                                                 |
| Protocolo                | TCP                                                                                |
| Ação                  | Permitir                                                                              |
| Prioridade                | 100                                                                                |
| Nome                    | Permitir-HTTP-All                                                                     |

Depois de criar a regra, a porta 80 está permitido de entrada da internet, porque a prioridade da regra é superior à regra de segurança predefinida denominada *DenyAllInBound*, que nega o tráfego. Saiba como [criar uma regra de segurança](manage-network-security-group.md#create-a-security-rule). Se estiverem associados a interface de rede e a sub-rede NSGs diferentes, tem de criar a mesma regra em ambos os NSGs.

Quando o tráfego de entrada de processos do Azure, processa as regras de NSG associado à sub-rede (se existir um NSG associado) e, em seguida, processa regras do NSG associado à interface de rede. Se existir um NSG associado à interface de rede e a sub-rede, a porta tem de ser aberta em ambos os NSGs, para o tráfego alcançar a VM. Para facilitar a administração e comunicação com problemas, recomendamos que associa um NSG a uma sub-rede, em vez de interfaces de rede individuais. Se as VMs numa sub-rede precisam de regras de segurança diferentes, pode tornar a rede de membros de interfaces de um grupo de segurança da aplicação (ASG) e especifique um ASG como origem e de destino de uma regra de segurança. Saiba mais sobre [grupos de segurança de aplicações](security-overview.md#application-security-groups).

Se ainda está a ter problemas de comunicação, consulte o artigo [considerações](#considerations) e [diagnóstico adicional](#additional-dignosis).

## <a name="considerations"></a>Considerações

Quando a resolução de problemas de conectividade, considere os seguintes pontos:

* Regras de segurança predefinidas bloquear o acesso de entrada da internet e permitem apenas o tráfego de entrada da rede virtual. Para permitir tráfego de entrada a partir da Internet, adicione regras de segurança com uma prioridade superior à regras predefinidas. Saiba mais sobre [predefinido regras de segurança](security-overview.md#default-security-rules), ou como [adicionar uma regra de segurança](manage-network-security-group.md#create-a-security-rule).
* Se tiver executado o peering redes virtuais, por predefinição, o **VIRTUAL_NETWORK** etiquetas de serviço expande automaticamente para incluir os prefixos para redes virtuais em modo de peering. Para resolver quaisquer problemas relacionados com a rede virtual peering, pode ver os prefixos no **ExpandedAddressPrefix** lista. Saiba mais sobre [peering de rede virtual](virtual-network-peering-overview.md) e [etiquetas de serviço](security-overview.md#service-tags).
* Regras de segurança eficaz só são mostradas para uma interface de rede se houver um NSG associado à interface de rede da VM e, em alternativa, sub-rede, e se a VM se encontra no estado de execução.
* Se não existem nenhum NSGs associados a interface de rede ou a sub-rede e tiver um [endereço IP público](virtual-network-public-ip-address.md) atribuída a uma VM, todas as portas são abertas para acesso de entrada do e acesso de saída para qualquer local. Se a VM tem um endereço IP público, é recomendável aplicar um NSG para sub-rede a interface de rede.

## <a name="additional-diagnosis"></a>Diagnóstico adicional

* Para executar um teste rápido para determinar se o tráfego é permitido para ou a partir de uma VM, utilize o [Certifique-se de fluxo IP](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md) capacidade do observador de rede do Azure. Fluxo IP verificar indica se o tráfego é permitido ou negado. Se negado, o fluxo IP verificar indica que regra de segurança está a negar o tráfego.
* Se não existem quaisquer regras de segurança, fazendo com que a conectividade de rede de uma VM falhar, o problema pode ser devido a:
  * Software de firewall em execução no sistema de operativo da VM
  * Rotas configuradas para aplicações virtuais ou tráfego no local. Tráfego de Internet pode ser redirecionado para a sua rede no local através de [forçada do túnel](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Se forçar o tráfego de internet de túnel para uma aplicação virtual ou no local, pode não conseguir ligar à VM a partir da internet. Para saber como diagnosticar problemas de rota que poderão impedir o fluxo de tráfego da VM, consulte o artigo [diagnosticar um problema de encaminhamento de tráfego de rede virtual máquina](diagnose-network-routing-problem.md).

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre todas as tarefas, propriedades e definições para um [grupo de segurança de rede](manage-network-security-group.md#work-with-network-security-groups) e [regras de segurança](manage-network-security-group.md#work-with-security-rules).
- Saiba mais sobre [predefinido regras de segurança](security-overview.md#default-security-rules), [etiquetas de serviço](security-overview.md#service-tags), e [como o Azure processa as regras de segurança para o tráfego de entrada e saída](security-overview.md#network-security-groups) para uma VM.
