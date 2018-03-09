---
title: Ligar redes virtuais com o peering de rede virtual - portal do Azure | Microsoft Docs
description: Saiba como ligar redes virtuais com o peering de rede virtual.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/06/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: ac0c1033546758a77b43298a5fa8cba5f5204650
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-the-azure-portal"></a>Ligar redes virtuais com o peering de rede virtual com o portal do Azure

Pode ligar redes virtuais entre si ao peering de rede virtual. Depois de redes virtuais em modo de peering, recursos em ambas as redes virtuais conseguem comunicar entre si, com a mesma latência e largura de banda, como se os recursos foram na mesma rede virtual. Este artigo abrange a criação e o peering de duas redes virtuais. Saiba como:

> [!div class="checklist"]
> * Criar duas redes virtuais
> * Criar um peering entre redes virtuais
> * Peering de teste

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="create-virtual-networks"></a>Criar redes virtuais

1. Selecione **+ criar um recurso** no canto superior, esquerda canto do portal do Azure.
2. Selecione **redes**e, em seguida, selecione **rede Virtual**.
3. Como é mostrado na imagem seguinte, introduza *myVirtualNetwork1* para **nome**, *10.0.0.0/16* para **espaço de endereços**,  **myResourceGroup** para **grupo de recursos**, *Subnet1* para sub-rede **nome**, 10.0.0.0/24 para sub-rede **intervalo de endereços** , selecione um **localização** e os seus **subscrição**, aceite as restantes predefinições e, em seguida, selecione **criar**:

    ![Criar uma rede virtual](./media/tutorial-connect-virtual-networks-portal/create-virtual-network.png)

4. Conclua os passos 1 a 3 novamente, com as seguintes alterações:
    - **Name**: *myVirtualNetwork2*
    - **Grupo de recursos**: selecione **utilizar existente** e, em seguida, selecione **myResourceGroup**.
    - **Espaço de endereços**: *10.1.0.0/16*
    - **Intervalo de endereços da sub-rede**: *10.1.0.0/24*

    O prefixo de endereço para o *myVirtualNetwork2* rede virtual não se sobreponha a com o espaço de endereços do *myVirtualNetwork1* rede virtual. Não é possível elemento redes virtuais com espaços de endereços sobrepostos.

## <a name="peer-virtual-networks"></a>Redes virtuais do elemento de rede

1. Na caixa de pesquisa na parte superior do portal do Azure, começa a escrever *MyVirtualNetwork1*. Quando **myVirtualNetwork1** aparece nos resultados da pesquisa, selecionados-lo.
2. Selecione **Peerings**, em **definições**e, em seguida, selecione **+ adicionar**, conforme mostrado na imagem seguinte:

    ![Criar peering](./media/tutorial-connect-virtual-networks-portal/create-peering.png)

3. Introduza ou selecione as informações mostradas na imagem seguinte, em seguida, selecione **OK**. Para selecionar o *myVirtualNetwork2* rede virtual, selecione **rede Virtual**, em seguida, selecione *myVirtualNetwork2*.

    ![Definições de peering](./media/tutorial-connect-virtual-networks-portal/peering-settings.png)

    O **PEERING estado** é *iniciado*, conforme mostrado na imagem seguinte:

    ![Estado de peering](./media/tutorial-connect-virtual-networks-portal/peering-status.png)

    Se não vir o estado, atualize o browser.

4. Procure o *myVirtualNetwork2* rede virtual. Quando este é devolvido nos resultados da pesquisa, selecione-a.
5. Conclua os passos 1 a 3 novamente, com as seguintes alterações e, em seguida, selecione **OK**:
    - **Name**: *myVirtualNetwork2-myVirtualNetwork1*
    - **Rede virtual**: *myVirtualNetwork1*

    O **PEERING estado** é *ligado*. Azure também alterou o estado de peering para o *myVirtualNetwork2 myVirtualNetwork1* peering do *iniciado* para *ligado.* Peering de rede virtual não for totalmente estabelecida enquanto o estado de peering para ambas as redes virtuais for *ligado.* 

Peerings estão entre duas redes virtuais, mas não estão transitivas. Sim, por exemplo, se pretendesse também a ponto *myVirtualNetwork2* para *myVirtualNetwork3*, terá de criar um peering entre redes virtuais adicionais *myVirtualNetwork2* e *myVirtualNetwork3*. Apesar de *myVirtualNetwork1* está em modo de peering com *myVirtualNetwork2*, recursos *myVirtualNetwork1* só foi possível aceder a recursos em  *myVirtualNetwork3* se *myVirtualNetwork1* foi também em modo de peering com *myVirtualNetwork3*. 

Antes de peering redes virtuais de produção, recomenda-se que exaustivamente familiarizar-se com o [descrição geral do peering](virtual-network-peering-overview.md), [gerir peering](virtual-network-manage-peering.md), e [limites de rede virtual ](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Embora este artigo ilustra um peering entre duas redes virtuais na mesma subscrição e localização, também pode elemento redes virtuais no [regiões diferentes](#register) e [diferentes subscrições do Azure](create-peering-different-subscriptions.md#portal). Também pode criar [hub- and -spoke designs de rede](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) com peering.

## <a name="test-peering"></a>Peering de teste

Para testar a comunicação de rede entre máquinas virtuais nas redes virtuais diferentes através de um peering, implementar uma máquina virtual para cada sub-rede e, em seguida, comunicar entre as máquinas virtuais. 

### <a name="create-virtual-machines"></a>Criar máquinas virtuais

Crie uma máquina virtual em cada rede virtual, de modo pode validar a comunicação entre elas num passo posterior.

### <a name="create-virtual-machines"></a>Criar máquinas virtuais

1. Selecione **+ criar um recurso** no canto superior, esquerda canto do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. Pode selecionar um sistema operativo diferente, mas os restantes passos partem do princípio de que selecionou **Datacenter do Windows Server 2016**. 
3. Selecione ou introduza as seguintes informações para **Noções básicas**, em seguida, selecione **OK**:
    - **Name**: *myVm1*
    - **Grupo de recursos**: selecione **utilizar existente** e, em seguida, selecione *myResourceGroup*.
    - **Localização**: selecione *EUA Leste*.

    O **nome de utilizador** e **palavra-passe** introduzir são utilizadas num passo posterior. A palavra-passe tem de ter, pelo menos, 12 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm). O **localização** e **subscrição** selecionada tem de ser os mesmos como a localização e a subscrição da rede virtual está a ser. Não é necessário que seleciona o mesmo grupo de recursos que a rede virtual foi criada, mas o mesmo grupo de recursos está selecionado para este artigo.
4. Selecione um tamanho VM em **escolher um tamanho**.
5. Selecione ou introduza as seguintes informações para **definições**, em seguida, selecione **OK**:
    - **Rede virtual**: Certifique-se de que **myVirtualNetwork1** está selecionada. Se não, selecione **rede Virtual** e, em seguida, selecione **myVirtualNetwork1** em **escolha de rede virtual**.
    - **Sub-rede**: Certifique-se de que **Subnet1** está selecionada. Se não, selecione **sub-rede** e, em seguida, selecione **Subnet1** em **escolha subrede**, conforme mostrado na imagem seguinte:
    
        ![Definições da máquina virtual](./media/tutorial-connect-virtual-networks-portal/virtual-machine-settings.png)
 
6. Em **criar** no **resumo**, selecione **criar** para iniciar a implementação da máquina virtual.
7. Execute novamente os passos 1-6, mas introduzir *myVm2* para o **nome** da máquina virtual e selecione *myVirtualNetwork2* para **rede Virtual**.

Azure atribuído *10.0.0.4* como o endereço IP privado do *myVm1* a máquina virtual e 10.1.0.4 para o *myVm2* máquina virtual, porque estavam primeiro IP disponível endereços nas *Subnet1* do *myVirtualNetwork1* e *myVirtualNetwork2* virtual redes, respetivamente.

As máquinas virtuais demorar alguns minutos a criar. Não continue com os restantes passos até que ambas as máquinas virtuais são criadas.

### <a name="test-virtual-machine-communication"></a>Comunicação de máquina virtual de teste

1. No *pesquisa* caixa na parte superior do portal, começa a escrever *myVm1*. Quando **myVm1** aparece nos resultados da pesquisa, selecionados-lo.
2. Criar uma ligação de ambiente de trabalho remota para o *myVm1* máquina virtual, selecionando **Connect**, conforme mostrado na imagem seguinte:

    ![Conectar à máquina virtual](./media/tutorial-connect-virtual-networks-portal/connect-to-virtual-machine.png)  

3. Para ligar à VM, abra o ficheiro RDP transferido. Se lhe for pedido, selecione **Connect**.
4. Introduza o nome de utilizador e palavra-passe que especificou ao criar a máquina virtual (poderá ter de selecionar **mais opções**, em seguida, **utilizar uma conta diferente**, para especificar as credenciais introduzidas quando a criar a máquina virtual), em seguida, selecione **OK**.
5. Poderá receber um aviso de certificado durante o processo de início de sessão. Selecione **Sim** para continuar com a ligação.
6. Num passo posterior, ping é utilizado para comunicar com o *myVm2* máquina virtual a partir de *myVmWeb* máquina virtual. Utiliza o ping ICMP, que é negado através da Firewall do Windows, por predefinição. Ative o ICMP através da firewall do Windows, introduzindo o seguinte comando numa linha de comandos:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

    Apesar de ping é utilizado para fins de teste neste artigo, permitir ICMP através da Firewall do Windows para implementações de produção não é recomendada.

7. Para estabelecer ligação com o *myVm2* máquina virtual, introduza o seguinte comando numa linha de comandos *myVm1* máquina virtual:

    ```
    mstsc /v:10.1.0.4
    ```
    
8. Uma vez que ativou o ping no *myVm1*, pode agora executar um ping-lo por endereço IP:

    ```
    ping 10.0.0.4
    ```
    
    Receber quatro respostas. Se executar o ping pelo nome da máquina virtual (*myVm1*), em vez do endereço IP, ping falhar, porque *myVm1* é um nome de anfitrião desconhecido. Resolução de nomes predefinido do Azure funciona entre as máquinas virtuais na mesma rede virtual, mas não entre máquinas virtuais nas redes virtuais diferentes. Para resolver nomes através de redes virtuais, terá [implementar o seu próprio servidor DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md) ou utilize [domínios privados do DNS do Azure](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

9. Desligar as sessões RDP para ambos *myVm1* e *myVm2*.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, elimine o grupo de recursos e todos os recursos que contém: 

1. Introduza *myResourceGroup* no **pesquisa** caixa na parte superior do portal. Quando vir **myResourceGroup** nos resultados da pesquisa, selecione-o.
2. Selecione **Eliminar grupo de recursos**.
3. Introduza *myResourceGroup* para **tipo o nome de grupo de recursos:** e selecione **eliminar**.

**<a name="register"></a>Registar-se para a pré-visualização de peering de rede virtual global**

O peering de redes virtuais na mesma região encontra-se em disponibilidade geral. Peering redes virtuais em diferentes regiões está atualmente em pré-visualização. Consulte [atualizações da rede Virtual](https://azure.microsoft.com/updates/?product=virtual-network) de regiões disponíveis. A ponto redes virtuais em regiões, primeiro tem de registar para a pré-visualização. Não é possível registar a utilizar o portal, mas pode registar utilizando [PowerShell](tutorial-connect-virtual-networks-powershell.md#register) ou [CLI do Azure](tutorial-connect-virtual-networks-cli.md#register). Se tentar elemento redes virtuais em diferentes regiões antes de registar para a capacidade de peering falhar.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu a ligar duas redes com o peering de rede virtual.

Continue para ligar o seu próprio computador a uma rede virtual através de uma VPN e interagir com recursos numa rede virtual, ou em redes virtuais em modo de peering.

> [!div class="nextstepaction"]
> [Ligar o computador a uma rede virtual](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
