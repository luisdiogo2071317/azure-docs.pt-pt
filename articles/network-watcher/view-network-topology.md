---
title: Visualizar a topologia de rede virtual do Azure | Microsoft Docs
description: Saiba como ver os recursos numa rede virtual e as relações entre os recursos.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2018
ms.author: jdial
ms.openlocfilehash: 6ef165ddc481bf84c6189635e36b97eb9518261e
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="view-the-topology-of-an-azure-virtual-network"></a>Visualizar a topologia de uma rede virtual do Azure

Neste artigo, irá aprender a ver recursos na rede virtual do Microsoft Azure e as relações entre os recursos. Por exemplo, uma rede virtual contém sub-redes. Sub-redes contenham recursos, tais como máquinas virtuais do Azure (VM). VMs de ter uma ou mais interfaces de rede. Cada sub-rede pode ter um grupo de segurança de rede e uma tabela de rotas associada ao mesmo. A capacidade de topologia do observador de rede do Azure permite-lhe ver todos os recursos numa rede virtual, os recursos associados a recursos na rede virtual e as relações entre os recursos.

Pode utilizar o [portal do Azure](#azure-portal), a [CLI do Azure](#azure-cli), ou [PowerShell](#powershell) para ver uma topologia.

## <a name = "azure-portal"></a>Topologia de vista - portal do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com) com uma conta que tenha o necessário [permissões](required-rbac-permissions.md).
2. Na parte superior, esquerda canto do portal, selecione **todos os serviços**.
3. No **todos os serviços** filtrar caixa, introduza *observador de rede*. Quando **observador de rede** aparece nos resultados, selecionados-lo.
4. Selecione **topologia**. Gerar uma topologia requer um observador de rede na mesma região que a rede virtual que pretende gerar a topologia para existe. Se não tiver um observador de rede ativado na região que é a rede virtual que pretende gerar uma topologia numa, observadores de rede são automaticamente criados por si em todas as regiões. Os observadores de rede são criados num grupo de recursos com o nome **NetworkWatcherRG**.
5. Selecione uma subscrição, o grupo de recursos de uma rede virtual que pretende visualizar a topologia para, e, em seguida, selecione a rede virtual. Na imagem seguinte, é mostrada uma topologia para uma rede virtual denominada *MyVnet*, no grupo de recursos denominado *MyResourceGroup*:

    ![Ver topologia](./media/view-network-topology/view-topology.png)

    Como pode ver na imagem anterior, a rede virtual contém três sub-redes. Uma sub-rede tem uma VM implementada no mesmo. A VM possui uma interface de rede ligados ao mesmo e um endereço IP público associados ao mesmo. As outras duas sub-redes tem uma tabela de rotas associada aos mesmos. Cada tabela de rotas contém duas rotas. Uma sub-rede tem um grupo de segurança de rede associado. Informações de topologia só são mostradas para recursos que são: - dentro do mesmo grupo de recursos e região que o *myVnet* rede virtual. Por exemplo, um grupo de segurança de rede existe num grupo de recursos diferente de *MyResourceGroup*, não é apresentado, mesmo que o grupo de segurança de rede se encontra associado a uma sub-rede a *MyVnet* rede virtual .
        -No, ou associados a recursos na, o *myVnet* rede virtual. Por exemplo, um grupo de segurança de rede não está associado a uma interface de rede ou de sub-rede no *myVnet* rede virtual não é apresentada, mesmo que o grupo de segurança de rede está a ser o *MyResourceGroup* grupo de recursos.

    A topologia mostrada na imagem destina-se a rede virtual criada depois de implementar o **encaminhar o tráfego através de um script de exemplo de aplicação virtual da rede**, que pode implementar utilizando o [CLI do Azure](../virtual-network/scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json), ou [PowerShell](../virtual-network/scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json).

6. Selecione **transferir topologia** para transferir a imagem como um ficheiro editável, no formato svg.

Os recursos mostrados no diagrama são um subconjunto dos componentes de rede na rede virtual. Por exemplo, enquanto é apresentado um grupo de segurança de rede, as regras de segurança dentro da mesma não são apresentadas no diagrama. Apesar de não diferenciadas no diagrama, as linhas representam uma das duas relações: *contenção* ou *associados*. Para obter uma lista completa de recursos na rede virtual e o tipo de relação entre os recursos, gerar a topologia com [PowerShell](#powershell) ou [CLI do Azure](#azure-cli).

## <a name = "azure-cli"></a>Visualizar a topologia - CLI do Azure

Pode executar os comandos nos passos que se seguem:
- Na Shell de nuvem do Azure, selecionando **tente-** na parte superior direita do qualquer comando. A Shell de nuvem do Azure é uma shell interativa livre que tem as ferramentas do Azure comuns pré-instalado e configurado para utilizar com a sua conta.
- Ao executar a CLI do seu computador. Se utilizar a CLI do seu computador, os passos neste artigo requerem a CLI do Azure versão 2.0.31 ou posterior. Execute `az --version` para localizar a versão instalada. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure](/cli/azure/install-azure-cli). Se estiver a executar localmente a CLI do Azure, também terá de executar `az login` para criar uma ligação com o Azure.

A conta que utilizar tem de ter o necessário [permissões](required-rbac-permissions.md).

1. Se já tiver um observador de rede na mesma região que a rede virtual que pretende criar uma topologia para, avance para o passo 3. Criar um grupo de recursos que contém um observador de rede com [criar grupo az](/cli/azure/group#az_group_create). O exemplo seguinte cria o grupo de recursos no *eastus* região:

    ```azurecli-interactive
    az group create --name NetworkWatcherRG --location eastus
    ```

2. Criar um observador de rede com [observador de rede az configurar](/cli/azure/network/watcher#az-network-watcher-configure). O exemplo seguinte cria um observador de rede no *eastus* região:

    ```azurecli-interactive
    az network watcher configure \
      --resource-group NetworkWatcherRG \
      --location eastus \
      --enabled true
    ```

3. Visualizar a topologia com [observador Mostrar-topologia de rede de az](/cli/azure/network/watcher#az-network-watcher-show-topology). O exemplo seguinte visualiza a topologia para um grupo de recursos denominado *MyResourceGroup*:

    ```azurecli-interactive
    az network watcher show-topology --resource-group MyResourceGroup
    ```

    Informações de topologia só são devolvidas para recursos que estão dentro do mesmo grupo de recursos como o *MyResourceGroup* grupo de recursos e a mesma região que o observador de rede. Por exemplo, um grupo de segurança de rede existe num grupo de recursos diferente de *MyResourceGroup*, não é apresentado, mesmo que o grupo de segurança de rede se encontra associado a uma sub-rede a *MyVnet* rede virtual .

  Saiba mais sobre o [relações](#relationhips) e [propriedades](#properties) na saída devolvida. Se não tiver uma rede virtual existente para ver uma topologia para, pode criar um utilizando o [encaminhar o tráfego através de uma aplicação virtual de rede](../virtual-network/scripts/virtual-network-cli-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) script de exemplo. Para ver um diagrama da topologia e transferi-la num ficheiro editável, utilize o [portal](#azure-portal).

## <a name = "powershell"></a>Topologia de vista - PowerShell

Pode executar os comandos nos passos que se seguem:
- Na Shell de nuvem do Azure, selecionando **tente-** na parte superior direita do qualquer comando. A Shell de nuvem do Azure é uma shell interativa livre que tem as ferramentas do Azure comuns pré-instalado e configurado para utilizar com a sua conta.
- Através da execução do PowerShell do seu computador. Se executar o PowerShell a partir do seu computador, os passos descritos neste artigo requerem versão 5.7.0 ou posterior do módulo AzureRm. Execute `Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

A conta que utilizar tem de ter o necessário [permissões](required-rbac-permissions.md).

1. Se já tiver um observador de rede na mesma região que a rede virtual que pretende criar uma topologia para, avance para o passo 3. Criar um grupo de recursos que contém um observador de rede com [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). O exemplo seguinte cria o grupo de recursos no *eastus* região:

    ```azurepowershell-interactive
    New-AzureRmResourceGroup -Name NetworkWatcherRG -Location EastUS
    ```

2. Criar um observador de rede com [New-AzureRmNetworkWatcher](/powershell/module/azurerm.network/new-azurermnetworkwatcher). O exemplo seguinte cria um observador de rede na região eastus:

    ```azurepowershell-interactive
    New-AzureRmNetworkWatcher `
      -Name NetworkWatcher_eastus `
      -ResourceGroupName NetworkWatcherRG
    ```

3. Obter uma instância de observador de rede com [Get-AzureRmNetworkWatcher](/powershell/module/azurerm.network/get-azurermnetworkwatcher). O exemplo seguinte obtém um observador de rede na região EUA Leste:

    ```azurepowershell-interactive
    $nw = Get-AzurermResource `
      | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "EastUS" }
    $networkWatcher = Get-AzureRmNetworkWatcher `
      -Name $nw.Name `
      -ResourceGroupName $nw.ResourceGroupName
    ```

4. Obter uma topologia com [Get-AzureRmNetworkWatcherTopology](/powershell/module/azurerm.network/get-azurermnetworkwatchertopology). O exemplo seguinte obtém uma topologia para uma rede virtual no grupo de recursos com o nome *MyResourceGroup*:

    ```azurepowershell-interactive
    Get-AzureRmNetworkWatcherTopology `
      -NetworkWatcher $networkWatcher `
      -TargetResourceGroupName MyResourceGroup
    ```

   Informações de topologia só são devolvidas para recursos que estão dentro do mesmo grupo de recursos como o *MyResourceGroup* grupo de recursos e a mesma região que o observador de rede. Por exemplo, um grupo de segurança de rede existe num grupo de recursos diferente de *MyResourceGroup*, não é apresentado, mesmo que o grupo de segurança de rede se encontra associado a uma sub-rede a *MyVnet* rede virtual .

  Saiba mais sobre o [relações](#relationhips) e [propriedades](#properties) na saída devolvida. Se não tiver uma rede virtual existente para ver uma topologia para, pode criar um utilizando o [encaminhar o tráfego através de uma aplicação virtual de rede](../virtual-network/scripts/virtual-network-powershell-sample-route-traffic-through-nva.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) script de exemplo. Para ver um diagrama da topologia e transferi-la num ficheiro editável, utilize o [portal](#azure-portal).

## <a name="relationships"></a>Relações

Todos os recursos devolvidos numa topologia de tem um dos seguintes tipos de relação a outro recurso:

| Tipo de relação | Exemplo                                                                                                |
| ---               | ---                                                                                                    |
| Contenção       | Uma rede virtual contém uma sub-rede. Uma sub-rede contém uma interface de rede.                            |
| Associados        | Uma interface de rede está associada uma VM. Um endereço IP público está associado a uma interface de rede. |

## <a name="properties"></a>Propriedades

Todos os recursos devolvidos numa topologia de possuir as seguintes propriedades:

- **Nome**: O nome do recurso
- **ID**: O URI do recurso.
- **Localização**: região do Azure que o recurso existe.
- **Associações**: uma lista de associações para o objeto referenciado. Cada associação tem as seguintes propriedades:
    - **AssociationType**: referencia a relação entre o objeto subordinado e o elemento principal. Os valores válidos são *contém* ou *associados*.
    - **Nome**: O nome do recurso referenciado.
    - **ResourceId**:-o URI do recurso referenciado na associação.

## <a name="next-steps"></a>Passos Seguintes

- Saiba como [diagnosticar um problema de filtro de tráfego rede para ou a partir de uma VM](diagnose-vm-network-traffic-filtering-problem.md) utilizando o fluxo do observador de rede IP Certifique-se a capacidade
- Saiba como [diagnosticar um problema de encaminhamento de tráfego de rede a partir de uma VM](diagnose-vm-network-routing-problem.md) com capacidade de salto seguinte do observador de rede