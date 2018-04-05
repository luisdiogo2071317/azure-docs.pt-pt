---
title: Ligar redes virtuais com o peering de rede virtual - portal do Azure | Microsoft Docs
description: Neste artigo, irá aprender a ligar redes virtuais com a rede virtual peering, utilizando o portal do Azure.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to connect two virtual networks so that virtual machines in one virtual network can communicate with virtual machines in the other virtual network.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: b864c71a62289b3abef13a98b52683f7d928b8e1
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-the-azure-portal"></a>Ligar redes virtuais com o peering de rede virtual com o portal do Azure

Pode ligar redes virtuais entre si ao peering de rede virtual. Depois de redes virtuais em modo de peering, recursos em ambas as redes virtuais conseguem comunicar entre si, com a mesma latência e largura de banda, como se os recursos foram na mesma rede virtual. Neste artigo, saiba como:

> [!div class="checklist"]
> * Criar duas redes virtuais
> * Ligar duas redes virtuais com um peering de rede virtual
> * Implementar uma máquina virtual (VM) em cada rede virtual
> * Comunicar entre VMs

Se preferir, pode concluir este artigo utilizando o [CLI do Azure](tutorial-connect-virtual-networks-cli.md) ou [Azure PowerShell](tutorial-connect-virtual-networks-powershell.md).

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no portal do Azure em https://portal.azure.com.

## <a name="create-virtual-networks"></a>Criar redes virtuais

1. Selecione **+ criar um recurso** no canto superior, esquerda canto do portal do Azure.
2. Selecione **redes**e, em seguida, selecione **rede Virtual**.
3. Introduza, ou selecione as seguintes informações, aceite as predefinições para as restantes definições e, em seguida, selecione **criar**:

    |Definição|Valor|
    |---|---|
    |Nome|myVirtualNetwork1|
    |Espaço de endereços|10.0.0.0/16|
    |Subscrição| Selecione a sua subscrição.|
    |Grupo de recursos| Selecione **criar nova** e introduza *myResourceGroup*.|
    |Localização| Selecione **EUA Leste**.|
    |Nome da sub-rede|Subnet1|
    |Intervalo de endereços de sub-rede|10.0.0.0/24|

      ![Criar uma rede virtual](./media/tutorial-connect-virtual-networks-portal/create-virtual-network.png)

4. Conclua os passos 1 a 3 novamente, com as seguintes alterações:

    |Definição|Valor|
    |---|---|
    |Nome|myVirtualNetwork2|
    |Espaço de endereços|10.1.0.0/16|
    |Grupo de recursos| Selecione **utilizar existente** e, em seguida, selecione **myResourceGroup**.|
    |Intervalo de endereços de sub-rede|10.1.0.0/24|

## <a name="peer-virtual-networks"></a>Redes virtuais do elemento de rede

1. Na caixa de pesquisa na parte superior do portal do Azure, começa a escrever *MyVirtualNetwork1*. Quando **myVirtualNetwork1** aparece nos resultados da pesquisa, selecionados-lo.
2. Selecione **Peerings**, em **definições**e, em seguida, selecione **+ adicionar**, conforme mostrado na imagem seguinte:

    ![Criar peering](./media/tutorial-connect-virtual-networks-portal/create-peering.png)

3. Introduza, ou selecione as seguintes informações, aceite as predefinições para as restantes definições e, em seguida, selecione **OK**.

    |Definição|Valor|
    |---|---|
    |Nome|myVirtualNetwork1-myVirtualNetwork2|
    |Subscrição| Selecione a sua subscrição.|
    |Rede virtual|myVirtualNetwork2 - para selecionar o *myVirtualNetwork2* rede virtual, selecione **rede Virtual**, em seguida, selecione **myVirtualNetwork2**.|

    ![Definições de peering](./media/tutorial-connect-virtual-networks-portal/peering-settings.png)

    O **PEERING estado** é *iniciado*, conforme mostrado na imagem seguinte:

    ![Estado de peering](./media/tutorial-connect-virtual-networks-portal/peering-status.png)

    Se não vir o estado, atualize o browser.

4. No **pesquisa** caixa na parte superior do portal do Azure, começa a escrever *MyVirtualNetwork2*. Quando **myVirtualNetwork2** aparece nos resultados da pesquisa, selecionados-lo.
5. Conclua os passos 2 a 3 novamente, com as seguintes alterações e, em seguida, selecione **OK**:

    |Definição|Valor|
    |---|---|
    |Nome|myVirtualNetwork2-myVirtualNetwork1|
    |Rede virtual|myVirtualNetwork1|

    O **PEERING estado** é *ligado*. Azure também alterou o estado de peering para o *myVirtualNetwork2 myVirtualNetwork1* peering do *iniciado* para *ligado.* Peering de rede virtual não for totalmente estabelecida enquanto o estado de peering para ambas as redes virtuais for *ligado.* 

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Crie uma VM em cada rede virtual para que possa comunicar entre eles num passo posterior.

### <a name="create-the-first-vm"></a>Criar a primeira VM

1. Selecione **+ criar um recurso** no canto superior, esquerda canto do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. Pode selecionar um sistema operativo diferente, mas os restantes passos partem do princípio de que selecionou **Datacenter do Windows Server 2016**. 
3. Introduza ou selecione as seguintes informações para **Noções básicas**, aceite as predefinições para as restantes definições e, em seguida, selecione **criar**:

    |Definição|Valor|
    |---|---|
    |Nome|myVm1|
    |Nome de utilizador| Introduza um nome de utilizador à sua escolha.|
    |Palavra-passe| Introduza uma palavra-passe da sua escolha. A palavra-passe tem de ter, pelo menos, 12 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Grupo de recursos| Selecione **utilizar existente** e, em seguida, selecione **myResourceGroup**.|
    |Localização| Selecione **EUA Leste**.|
4. Selecione um tamanho VM em **escolher um tamanho**.
5. Selecione os seguintes valores para **definições**, em seguida, selecione **OK**:
    |Definição|Valor|
    |---|---|
    |Rede virtual| myVirtualNetwork1 - se ainda não estiver selecionada, selecione **rede Virtual** e, em seguida, selecione **myVirtualNetwork1** em **escolha de rede virtual**.|
    |Subrede| Subnet1 - se ainda não estiver selecionada, selecione **sub-rede** e, em seguida, selecione **Subnet1** em **escolha subrede**.|
    
    ![Definições da máquina virtual](./media/tutorial-connect-virtual-networks-portal/virtual-machine-settings.png)
 
6. Em **criar** no **resumo**, selecione **criar** para iniciar a implementação de VM.

### <a name="create-the-second-vm"></a>Criar a VM segundo

Conclua os passos 1-6 novamente, com as seguintes alterações:

|Definição|Valor|
|---|---|
|Nome | myVm2|
|Rede virtual | myVirtualNetwork2|

As VMs demorar alguns minutos a criar. Não continue com os restantes passos até que ambas as VMs que são criadas.

## <a name="communicate-between-vms"></a>Comunicar entre VMs

1. No *pesquisa* caixa na parte superior do portal, começa a escrever *myVm1*. Quando **myVm1** aparece nos resultados da pesquisa, selecionados-lo.
2. Criar uma ligação de ambiente de trabalho remota para o *myVm1* VM selecionando **Connect**, conforme mostrado na imagem seguinte:

    ![Conectar à máquina virtual](./media/tutorial-connect-virtual-networks-portal/connect-to-virtual-machine.png)  

3. Para ligar à VM, abra o ficheiro RDP transferido. Se lhe for pedido, selecione **Connect**.
4. Introduza o nome de utilizador e palavra-passe que especificou ao criar a VM (poderá ter de selecionar **mais opções**, em seguida, **utilizar uma conta diferente**, para especificar as credenciais que introduziu quando criou a VM), em seguida, selecione **OK**.
5. Poderá receber um aviso de certificado durante o processo de início de sessão. Selecione **Sim** para continuar com a ligação.
6. Num passo posterior, ping é utilizado para comunicar com o *myVm2* VM a partir de *myVm1* VM. Ping utiliza o controlo de mensagem de ICMP (Internet Protocol), que é negado através da Firewall do Windows, por predefinição. No *myVm1* VM, ativar o ICMP através da firewall do Windows, para que consegue enviar pings para esta VM *myVm2* num passo posterior, com o PowerShell:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```
    
    Apesar de ping é utilizado para comunicar entre VMs neste artigo, permitir ICMP através da Firewall do Windows para implementações de produção não é recomendada.

7. Para estabelecer ligação com o *myVm2* VM, introduza o seguinte comando numa linha de comandos *myVm1* VM:

    ```
    mstsc /v:10.1.0.4
    ```
    
8. Uma vez que ativou o ping no *myVm1*, pode agora executar um ping-lo por endereço IP:

    ```
    ping 10.0.0.4
    ```
    
9. Desligar as sessões RDP para ambos *myVm1* e *myVm2*.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, elimine o grupo de recursos e todos os recursos que contém: 

1. Introduza *myResourceGroup* no **pesquisa** caixa na parte superior do portal. Quando vir **myResourceGroup** nos resultados da pesquisa, selecione-o.
2. Selecione **Eliminar grupo de recursos**.
3. Introduza *myResourceGroup* para **tipo o nome de grupo de recursos:** e selecione **eliminar**.

## <a name="next-steps"></a>Passos seguintes

Neste artigo, aprendeu a ligar duas redes na mesma região do Azure, com o peering de rede virtual. Também pode elemento redes virtuais em diferentes [regiões suportadas](virtual-network-manage-peering.md#cross-region) e na [diferentes subscrições do Azure](create-peering-different-subscriptions.md#portal), bem como criar [hub- and -spoke designs de rede](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) com peering. Para obter mais informações sobre peering de rede virtual, consulte o artigo [descrição geral de peering da rede Virtual](virtual-network-peering-overview.md) e [gerir peerings de rede virtual](virtual-network-manage-peering.md).

Para ligar o seu próprio computador a uma rede virtual através de uma VPN e interagir com recursos numa rede virtual, ou em redes virtuais em modo de peering, consulte [ligar o computador a uma rede virtual](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
