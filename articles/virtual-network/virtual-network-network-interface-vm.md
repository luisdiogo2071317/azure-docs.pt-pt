---
title: Interfaces de rede para adicionar ou remover máquinas virtuais do Azure | Microsoft Docs
description: Saiba como interfaces de rede para adicionar ou remover as interfaces de rede de máquinas virtuais.
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
ms.date: 12/15/2017
ms.author: jdial
ms.openlocfilehash: abccfed6f335ed7febb4fb2f17ad71fa153e3ed6
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="add-network-interfaces-to-or-remove-network-interfaces-from-virtual-machines"></a>Interfaces de rede para adicionar ou remover as interfaces de rede de máquinas virtuais

Saiba como adicionar uma interface de rede existente, quando criar uma máquina virtual do Azure (VM), ou adicionar ou remover as interfaces de rede de VM existente no Estado parada (desalocada). Uma interface de rede permite que uma máquina virtual do Azure comunicar com a internet, do Azure e recursos no local. Uma VM pode ter uma ou mais interfaces de rede. 

Se necessário para adicionar, alterar ou remover os endereços IP para uma interface de rede, consulte [gerir endereços IP da interface de rede](virtual-network-network-interface-addresses.md). Se precisar de criar, alterar, ou elimine as interfaces de rede, consulte [gerir interfaces de rede](virtual-network-network-interface.md).

## <a name="before-you-begin"></a>Antes de começar

Conclua as seguintes tarefas antes de concluir os passos em qualquer secção deste artigo:

- Se ainda não tiver uma conta do Azure, inscreva-se um [conta de avaliação gratuita](https://azure.microsoft.com/free).
- Se utilizar o portal, abra https://portal.azure.come inicie sessão com a sua conta do Azure.
- Se utilizar comandos do PowerShell para concluir tarefas neste artigo, quer executar os comandos [Shell de nuvem do Azure](https://shell.azure.com/powershell), ou através da execução do PowerShell do seu computador. O Azure Cloud Shell é um shell interativo gratuito que pode utilizar para executar os passos neste artigo. Tem as ferramentas comuns do Azure pré-instaladas e configuradas para utilização com a sua conta. Este tutorial requer o Azure PowerShell versão do módulo 5.2.0 ou posterior. Execute `Get-Module -ListAvailable AzureRM` para localizar a versão instalada. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.
- Se utilizar comandos de interface de linha de comandos (CLI) do Azure para concluir tarefas neste artigo, quer executar os comandos [Shell de nuvem do Azure](https://shell.azure.com/bash), ou executando a CLI do seu computador. Este tutorial requer a CLI do Azure versão 2.0.26 ou posterior. Execute `az --version` para localizar a versão instalada. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0](/cli/azure/install-azure-cli). Se estiver a executar localmente a CLI do Azure, também terá de executar `az login` para criar uma ligação com o Azure.

## <a name="add-existing-network-interfaces-to-a-new-vm"></a>Adicione as interfaces de rede existente para uma nova VM

Quando cria uma máquina virtual através do portal, o portal cria uma interface de rede com as predefinições e anexa-lo para a VM para si. Não é possível adicionar as interfaces de rede existente para uma nova VM, nem para criar uma VM com várias interfaces de rede, utilizando o portal do Azure. Pode fazer a ambos utilizando a CLI ou PowerShell, mas certifique-se para se familiarizar com o [restrições](#constraints). Se criar uma VM com várias interfaces de rede, tem também de configurar o sistema operativo para utilizá-los corretamente depois de criar a VM. Saiba como configurar [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) ou [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) para várias interfaces de rede.

### <a name="commands"></a>Comandos

Antes de criar a VM, crie uma interface de rede, utilizando os passos no [criar uma interface de rede](virtual-network-network-interface.md#create-a-network-interface).

|Ferramenta|Comando|
|---|---|
|CLI|[az vm create](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_create)|
|PowerShell|[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Adicione uma interface de rede para uma VM existente

1. Inicie sessão no Portal do Azure.
2. Na caixa de pesquisa na parte superior do portal, escreva o nome da VM para o qual pretende adicionar a interface de rede ou navegue para a VM selecionando **todos os serviços**e, em seguida, **máquinas virtuais**. Após encontrar o a VM, selecione-a. A VM tem de suportar o número de interfaces de rede que pretende adicionar. Para saber cada tamanho da VM das interfaces de rede quantos suporta, consulte [tamanhos de máquinas virtuais do Linux no Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou [máquinas de virtuais de tamanhos para Windows no Azure](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  
3. Selecione **descrição geral**, em **definições**. Selecione **parar**e, em seguida, aguardar até o **estado** da VM é alterado para **parado (desalocado)**. 
4. Selecione **redes**, em **definições**.
5. Selecione **interface de rede de anexar**. Na lista de interfaces de rede que não são atualmente ligado a outra VM, selecione aquele que pretende ligar. 

    >[!NOTE]
    A interface de rede que selecionou não é possível ter acelerados rede ativada, não pode ter um endereço de IPv6 atribuído e tem de existir na mesma rede virtual que contém a interface de rede atualmente ligada à VM. 

    Se não tiver uma interface de rede existente, primeiro tem de criar um. Para tal, selecione **interface de rede criar**. Para obter mais informações sobre como criar uma interface de rede, consulte [criar uma interface de rede](virtual-network-network-interface.md#create-a-network-interface). Para obter mais informações sobre restrições adicionais ao adicionar as interfaces de rede para máquinas virtuais, consulte o artigo [restrições](#constraints).

6. Selecione **OK**.
7. Selecione **descrição geral**, em **definições**e, em seguida, **iniciar** para iniciar a máquina virtual.
8. Configure o sistema de operativo VM para utilizar várias interfaces de rede corretamente. Saiba como configurar [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) ou [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) para várias interfaces de rede.

|Ferramenta|Comando|
|---|---|
|CLI|[Adicionar AZ vm nic](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_nic_add) (referência) ou [detalhadas passos](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[AzureRmVMNetworkInterface adicionar](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (referência) ou [detalhadas passos](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="view-network-interfaces-for-a-vm"></a>Vista de interfaces de rede para uma VM

Pode ver as interfaces de rede atualmente ligadas a uma VM para saber mais sobre a configuração de cada interface de rede e os endereços IP atribuídos a cada interface de rede. 

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) com uma conta que está atribuída a função de proprietário, Contribuidor ou contribuinte de rede para a sua subscrição. Para obter mais informações sobre como atribuir funções de contas, consulte o artigo [funções incorporadas para controlo de acesso baseado em funções do Azure](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Na caixa que contém o texto **procurar recursos** na parte superior do portal do Azure, escreva **máquinas virtuais**. Quando **máquinas virtuais** aparece nos resultados da pesquisa, selecionados-lo.
3. Selecione o nome da VM para o qual pretende ver interfaces de rede.
4. No **definições** secção para a VM que selecionou, e selecione **redes**. Para saber mais sobre as definições da interface de rede e como alterá-los, consulte [gerir interfaces de rede](virtual-network-network-interface.md). Para saber mais sobre como adicionar, alterar ou remover os endereços IP atribuídos a uma interface de rede, consulte [gerir endereços IP da interface de rede](virtual-network-network-interface-addresses.md).

### <a name="commands"></a>Comandos

|Ferramenta|Comando|
|---|---|
|CLI|[az vm show](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_show)|
|PowerShell|[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="remove-a-network-interface-from-a-vm"></a>Remover uma interface de rede a partir de uma VM

1. Inicie sessão no Portal do Azure.
2. Na caixa de pesquisa na parte superior do portal, procure o nome da VM que pretende remover (desanexar) de interface de rede da ou navegue para a VM selecionando **todos os serviços**e, em seguida, **máquinas virtuais**. Após encontrar o a VM, selecione-a.
3. Selecione **descrição geral**, em **definições**e, em seguida, **parar**. Aguarde até que o **estado** da VM é alterado para **parado (desalocado)**. 
4. Selecione **redes**, em **definições**.
5. Selecione **interface de rede de anulação de exposições**. Na lista de interfaces de rede atualmente ligado à máquina virtual, selecione a interface de rede que pretende desligar. 

    >[!NOTE]
    Se apenas uma interface de rede estiver listada, não é possível desanexar, porque uma máquina virtual tem sempre de ter, pelo menos, uma interface de rede ligada ao mesmo.
6. Selecione **OK**.

### <a name="commands"></a>Comandos

|Ferramenta|Comando|
|---|---|
|CLI|[remover AZ vm nic](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_nic_remove) (referência) ou [detalhadas passos](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Remover AzureRMVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (referência) ou [detalhadas passos](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="constraints"></a>Restrições

- Uma VM tem de ter, pelo menos, uma interface de rede ligada ao mesmo.
- Uma VM só pode ter como muitas anexadas ao mesmo como o suporta de tamanho VM de interfaces de rede. Para mais informações sobre o número de interfaces de rede suporta cada tamanho da VM, consulte [tamanhos de máquinas virtuais do Linux no Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou [máquinas de virtuais de tamanhos para Windows no Azure](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Todos os tamanhos de suportam, pelo menos, duas interfaces de rede.
- Atualmente não não possível expor interfaces de rede, que adicionar a uma VM para outra VM. Para obter mais informações sobre como criar interfaces de rede, consulte [criar uma interface de rede](virtual-network-network-interface.md#create-a-network-interface).
- No passado, interfaces de rede só podem ser adicionadas para VMs que são suportadas várias interfaces de rede e criadas com, pelo menos, duas interfaces de rede. Não foi possível adicionar uma interface de rede a uma VM que foi criada com uma interface de rede, mesmo que o tamanho da VM suportado várias interfaces de rede. Por outro lado, só foi possível remover as interfaces de rede de uma VM com, pelo menos, três interfaces de rede, porque as VMs criadas com a rede, pelo menos, duas interfaces sempre tinham de ter, pelo menos, duas interfaces de rede. Nenhuma destas restrições aplicam-se delas. Agora, pode criar uma VM com qualquer número de interfaces de rede (até ao número suportados pelo tamanho da VM).
- Por predefinição, a primeira interface de rede ligada a uma VM está definida como o *primário* interface de rede. Todas as outras interfaces de rede VM são *secundário* interfaces de rede.
- Embora pode controlar o interface de rede enviado, o tráfego de saída por predefinição, todo o tráfego de saída da VM é enviado o endereço IP atribuído à configuração de IP primária da interface de rede principal.
- No passado, eram necessárias para ter interfaces de rede único ou vários, todas as VMs no mesmo conjunto de disponibilidade. As VMs com qualquer número de interfaces de rede podem agora existir no mesmo conjunto de disponibilidade, até ao número suportado pelo tamanho da VM. Só é possível adicionar uma VM para um conjunto quando é criado de disponibilidade. Para obter mais informações sobre conjuntos de disponibilidade, consulte o artigo [gerir a disponibilidade das VMs no Azure](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy).
- Enquanto as interfaces de rede na mesma VM podem ser ligadas a sub-redes diferentes dentro de uma rede virtual, as interfaces de rede devem todos estar ligadas à mesma rede virtual.
- Pode adicionar qualquer endereço IP de qualquer configuração de IP de qualquer interface de rede primária ou secundária para um conjunto de back-end de Balanceador de carga do Azure. No passado, apenas o endereço IP primário para a interface de rede principal foi possível adicionar a um conjunto de back-end. Para obter mais informações sobre endereços IP e as configurações, consulte [adicionar, alterar ou remover endereços IP](virtual-network-network-interface-addresses.md).
- Eliminar uma VM não elimina as interfaces de rede que estão anexadas ao mesmo. Quando elimina uma VM, as interfaces de rede estiverem desligadas da VM. Pode adicionar as interfaces de rede para diferentes VMs ou eliminá-los.
- Se uma interface de rede tem um endereço IPv6 privado atribuído, tem de adicionar (ligar) para uma VM ao criar a VM. Não é possível adicionar uma interface de rede com um endereço de IPv6 atribuído a uma VM, depois de criar a VM. Se adicionar uma interface de rede com um endereço IPv6 privada atribuído quando criar uma máquina virtual, só é possível adicionar essa interface de rede à máquina virtual, independentemente de quantas interfaces de rede suporta o tamanho da VM. Consulte [gerir endereços IP da interface de rede](virtual-network-network-interface-addresses.md) para saber mais sobre como atribuir endereços IP a interfaces de rede.
- Tal como acontece com o IPv6, não é possível anexar uma interface de rede com redes na melhoria ativada para uma VM, depois de a criar. Além disso, para tirar partido do funcionamento em rede na melhoria, também tem de concluir os passos no sistema de operativo VM. Saiba mais sobre redes na melhoria e outras restrições quando utilizá-la, para [Windows](create-vm-accelerated-networking-powershell.md) ou [Linux](create-vm-accelerated-networking-cli.md) máquinas virtuais.

## <a name="next-steps"></a>Passos Seguintes
Para criar uma VM com várias interfaces de rede ou endereços IP, leia os artigos seguintes:

### <a name="commands"></a>Comandos

|Tarefa|Ferramenta|
|---|---|
|Criar uma VM com vários NICs|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Criar uma VM de NIC único com vários endereços de IPv4|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Criar uma VM de NIC único com um endereço IPv6 privado (atrás de um balanceador de carga do Azure)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [modelo Azure Resource Manager](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|


