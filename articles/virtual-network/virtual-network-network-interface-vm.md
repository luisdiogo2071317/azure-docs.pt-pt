---
title: "Interfaces de rede para adicionar ou remover máquinas virtuais do Azure | Microsoft Docs"
description: "Saiba como interfaces de rede para adicionar ou remover as interfaces de rede de máquinas virtuais."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/15/2017
ms.author: jdial
ms.openlocfilehash: abe6abb942d206330e809f3aef388b846d7d7c7f
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2018
---
# <a name="add-network-interfaces-to-or-remove-network-interfaces-from-virtual-machines"></a>Interfaces de rede para adicionar ou remover as interfaces de rede de máquinas virtuais

Saiba como adicionar uma interface de rede existente, quando criar uma VM ou adicionar ou remover as interfaces de rede de VM existente no Estado parada (desalocada). Uma interface de rede permite que uma Máquina Virtual do Azure (VM) a comunicar com a Internet, do Azure e recursos no local. Uma VM pode ter uma ou mais interfaces de rede. 

Se precisar de adicionar, alterar ou remover os endereços IP para uma interface de rede, leia o [gerir endereços IP da interface de rede](virtual-network-network-interface-addresses.md) artigo. Se é necessário para criar, alterar ou eliminar as interfaces de rede, leia o [gerir interfaces de rede](virtual-network-network-interface.md) artigo.

## <a name="before"></a>Antes de começar

Conclua as seguintes tarefas antes de concluir os passos em qualquer secção deste artigo:

- Inicie sessão para o Azure [portal](https://portal.azure.com), interface de linha de comandos (CLI) do Azure ou o Azure PowerShell com uma conta do Azure. Se ainda não tiver uma conta do Azure, inscreva-se um [conta de avaliação gratuita](https://azure.microsoft.com/free).
- Se utilizar comandos do PowerShell para concluir tarefas neste artigo, [instalar e configurar o Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Certifique-se de que dispõe da versão mais recente dos mini-comandos de Azure PowerShell instaladas. Para obter ajuda para comandos do PowerShell, com exemplos, digite `get-help <command> -full`. Em vez de instalar o Azure PowerShell, pode utilizar a Shell de nuvem do Azure. A Shell de nuvem do Azure é um livre PowerShell que pode executar diretamente no portal do Azure. Tem Azure PowerShell pré-instalado e configurado para utilizar com a sua conta. Para utilizar a Shell de nuvem, clique na Shell de nuvem **> _** na parte superior da parte a [portal](https://portal.azure.com) e selecione o PowerShell no canto superior esquerdo, quando for apresentada a janela de shell.
- Se utilizar comandos de interface de linha de comandos (CLI) do Azure para concluir tarefas neste artigo, [instalar e configurar a CLI do Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Certifique-se de que dispõe da versão mais recente da CLI do Azure instalados. Para obter ajuda para comandos da CLI, escreva `az <command> --help`. Em vez de instalar a CLI e respetivos pré-requisitos, pode utilizar a Shell de nuvem do Azure. O Azure Cloud Shell é um shell Bash gratuito que pode ser executado diretamente no portal do Azure. Tem a CLI do Azure pré-instalada e configurada para ser utilizada com a sua conta. Para utilizar a Shell de nuvem, clique na Shell de nuvem **> _** na parte superior da parte a [portal](https://portal.azure.com) e selecione Bash no canto superior esquerdo, quando for apresentada a janela de shell.

## <a name="vm-create"></a>Adicione as interfaces de rede existente para uma nova VM

Quando cria uma VM através do portal, o portal cria uma interface de rede com as predefinições e anexa-lo para a VM para si. Não é possível adicionar as interfaces de rede existente para uma nova VM ou crie uma VM com várias interfaces de rede, utilizando o portal do Azure. Pode efetuar ambas as utilizando a CLI ou PowerShell. Antes de utilizar o PowerShell ou a CLI para criar uma VM com uma interface de rede existente, no entanto, familiarize-se com o [restrições](#constraints). Se criar uma máquina virtual com várias interfaces de rede, também tem de configurar o sistema operativo para utilizá-los corretamente depois da VM ser criada. Para obter mais informações, consulte Configurar [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) ou [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) para várias interfaces de rede.

**Comandos** antes de criar a VM, crie uma interface de rede utilizando os passos no [criar uma interface de rede](virtual-network-network-interface.md#create-a-network-interface).

|Ferramenta|Comando|
|---|---|
|CLI|[Criar AZ vm](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Novo-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Adicione uma interface de rede para uma VM existente

1. Inicie sessão no portal do Azure.
2. Na caixa de pesquisa na parte superior do portal, procure o nome da VM que pretende adicionar a interface de rede ou navegue para a VM clicando **todos os serviços**, em seguida, **máquinas virtuais**. Assim que tiver encontrado VM, clique na mesma. A VM que pretende adicionar uma interface de rede tem de suportar o número de interfaces de rede que pretende adicionar. Para saber mais interfaces de rede quantos cada suporta de tamanho VM, leia o [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artigos de tamanhos de VM.  
3. Clique em **descrição geral**, em **definições**. Clique em **parar**e aguarde até que o **estado** da VM é alterado para *parado (desalocado)*. 
4. Clique em **redes**, em **definições**.
5. Clique em **interface de rede de anexar**. Na lista de interfaces de rede existentes que não são atualmente ligado a outra VM, clique em que pretende anexar a interface de rede. A interface de rede que selecionou não é possível ter acelerados redes ativada, não pode ter um endereço de IPv6 atribuído e tem de existir na mesma rede virtual porque a rede virtual atualmente ligada a interface de rede para a VM se encontra no. Se não tiver uma interface de rede existente, primeiro tem de criar um. Para criar uma interface de rede, clique em **interface de rede criar**. Para saber mais sobre a criação de uma interface de rede, consulte [criar uma interface de rede](virtual-network-network-interface.md#create-a-network-interface). Para obter mais informações sobre restrições adicionais ao adicionar as interfaces de rede para máquinas virtuais, consulte o artigo [restrições](#constraints).
6. Clique em **OK**.
7. Clique em **descrição geral**, em **definições**. Clique em **iniciar** para iniciar a máquina virtual.
8. Configure o sistema de operativo VM corretamente utilizar várias interfaces de rede. Para obter mais informações, consulte Configurar [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) ou [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) para várias interfaces de rede.

|Ferramenta|Comando|
|---|---|
|CLI|[Adicionar AZ vm nic](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#add) (referência) ou [detalhadas passos](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[AzureRmVMNetworkInterface adicionar](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (referência) ou [detalhadas passos](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="vm-view-nic"></a>Vista de interfaces de rede para uma VM

Pode ver as interfaces de rede atualmente ligadas a uma VM para saber mais sobre a configuração de cada interface de rede e os endereços IP atribuídos a cada interface de rede. 

1. Inicie sessão no [portal do Azure](https://portal.azure.com) com uma conta que está atribuída a função de proprietário, Contribuidor ou contribuinte de rede para a sua subscrição. Para obter mais informações sobre como atribuir funções de contas, consulte [funções incorporadas para controlo de acesso baseado em funções do Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Na caixa que contém o texto *procurar recursos* na parte superior do portal do Azure, escreva *máquinas virtuais*. Quando **máquinas virtuais** é apresentado nos resultados da pesquisa, clique no mesmo.
3. Clique no nome da VM que pretende visualizar as interfaces de rede.
4. No **definições** secção para a VM que selecionou, clique em **redes**. Para saber mais sobre as definições da interface de rede e como alterá-los, consulte [gerir interfaces de rede](virtual-network-network-interface.md). Para saber mais sobre como adicionar, alterar ou remover os endereços IP atribuídos a uma interface de rede, consulte o artigo [endereços IP gerir](virtual-network-network-interface-addresses.md).

**Comandos**

|Ferramenta|Comando|
|---|---|
|CLI|[Mostrar de vm AZ](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-remove-nic"></a>Remover uma interface de rede a partir de uma VM

1. Inicie sessão no portal do Azure.
2. Na caixa de pesquisa na parte superior do portal, procure o nome da VM que pretende remover (desanexar) de interface de rede da ou navegue para a VM, clicando em **todos os serviços**, em seguida, **máquinas virtuais**. Assim que tiver encontrado VM, clique na mesma.
3. Clique em **descrição geral**, em **definições**. Clique em **parar**e aguarde até que o **estado** da VM é alterado para *parado (desalocado)*. 
4. Clique em **redes**, em **definições**.
5. Clique em **interface de rede de anulação de exposições**. Na lista de interfaces de rede atualmente ligado à máquina virtual, clique em que pretende desanexar a interface de rede. Se apenas uma interface de rede estiver listada, não é possível desanexar, porque uma máquina virtual tem sempre de ter, pelo menos, uma interface de rede ligada ao mesmo.
6. Clique em **OK**.

**Comandos**

|Ferramenta|Comando|
|---|---|
|CLI|[remover AZ vm nic](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#remove) (referência) ou [detalhadas passos](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Remover AzureRMVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (referência) ou [detalhadas passos](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="next-steps"></a>Passos seguintes
Para criar uma VM com várias interfaces de rede ou endereços IP, leia os artigos seguintes:

**Comandos**

|Tarefa|Ferramenta|
|---|---|
|Criar uma VM com vários NICs|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Criar uma VM de NIC único com vários endereços de IPv4|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Criar uma VM de NIC único com um endereço IPv6 privado (atrás de um balanceador de carga do Azure)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [modelo Azure Resource Manager](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="constraints"></a>Restrições

- Uma VM tem de ter, pelo menos, uma interface de rede ligada ao mesmo.
- Uma VM só pode ter como muitas anexadas ao mesmo como o suporta de tamanho VM de interfaces de rede. Para mais informações sobre o número de interfaces de rede suporta cada tamanho da VM, consulte [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) e [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tamanhos de VM. Todos os tamanhos de suportam, pelo menos, duas interfaces de rede.
- Atualmente não não possível expor interfaces de rede, que adicionar a uma VM para outra VM. Para saber mais sobre a criação de interfaces de rede, consulte [criar uma interface de rede](virtual-network-network-interface.md#create-a-network-interface).
- No passado, interfaces de rede só podem ser adicionadas para VMs que são suportadas várias interfaces de rede e criadas com, pelo menos, duas interfaces de rede. Não foi possível adicionar uma interface de rede a uma VM que foi criada com uma interface de rede, mesmo que o tamanho da VM suportado várias interfaces de rede. Por outro lado, só foi possível remover as interfaces de rede de uma VM com, pelo menos, três interfaces de rede, porque as VMs criadas com a rede, pelo menos, duas interfaces sempre tinham de ter, pelo menos, duas interfaces de rede. Nenhuma destas restrições aplicam-se delas. Agora, pode criar uma VM com qualquer número de interfaces de rede (até ao número suportados pelo tamanho da VM).
- Por predefinição, a primeira interface de rede ligada a uma VM está definida como o *primário* interface de rede. Todas as outras interfaces de rede VM são *secundário* interfaces de rede.
- Embora pode controlar o interface de rede enviado, o tráfego de saída por predefinição, todo o tráfego de saída da VM é enviado o endereço IP atribuído à configuração de IP primária da interface de rede principal.
- No passado, eram necessárias para ter interfaces de rede único ou vários, todas as VMs no mesmo conjunto de disponibilidade. As VMs com qualquer número de interfaces de rede podem agora existir no mesmo conjunto de disponibilidade, até ao número suportado pelo tamanho da VM. Só é possível adicionar uma VM para um conjunto quando é criado apesar de disponibilidade. Para obter mais informações sobre conjuntos de disponibilidade, leia o [gerir a disponibilidade das VMs no Azure](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) artigo.
- Enquanto as interfaces de rede na mesma VM podem ser ligadas a sub-redes diferentes numa VNet, as interfaces de rede devem todos estar ligadas para a mesma VNet.
- Pode adicionar qualquer endereço IP de qualquer configuração de IP de qualquer interface de rede primária ou secundária para um conjunto de back-end de Balanceador de carga do Azure. No passado, apenas o endereço IP primário para a interface de rede principal foi possível adicionar a um conjunto de back-end. Para obter mais informações sobre endereços IP e as configurações, leia o [adicionar, alterar ou remover endereços IP](virtual-network-network-interface-addresses.md) artigo.
- Eliminar uma VM não elimina as interfaces de rede que estão anexadas ao mesmo. Quando uma VM é eliminada, as interfaces de rede estiverem desligadas da VM. Pode adicionar as interfaces de rede para diferentes VMs ou eliminá-los.
- Se uma interface de rede tem um endereço IPv6 privado atribuído, tem de adicionar (ligar) para uma VM ao criar a VM. Não é possível adicionar uma interface de rede com um endereço de IPv6 atribuído a uma VM, depois da VM é criada. Se adicionar uma interface de rede com um endereço IPv6 privada atribuído ao criar uma máquina virtual, só é possível adicionar essa interface de rede à máquina virtual, independentemente de quantas interfaces de rede suporta o tamanho da VM. Consulte [endereços IP da interface de rede](virtual-network-network-interface-addresses.md) para saber mais sobre a atribuição de endereços IP a interfaces de rede.
- Semelhante para o IPv6, não é possível anexar uma interface de rede com redes na melhoria ativada para uma VM após a VM ser criada. Além disso, para tirar partido do funcionamento em rede na melhoria, também tem de concluir os passos no sistema de operativo VM. Para obter mais informações sobre redes na melhoria e outras limitações quando a utilizar, consulte Accelerated redes para [Windows](create-vm-accelerated-networking-powershell.md) ou [Linux](create-vm-accelerated-networking-cli.md) máquinas virtuais.
