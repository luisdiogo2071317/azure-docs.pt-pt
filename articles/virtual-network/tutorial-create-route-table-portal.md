---
title: Encaminhar tráfego de rede - tutorial - portal do Azure | Microsoft Docs
description: Neste tutorial, saiba como encaminhar o tráfego de rede com uma tabela de rotas através do portal do Azure.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: 7254e9336fca14daee2021d5bde4c5538509fe35
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
ms.locfileid: "30842327"
---
# <a name="tutorial-route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Tutorial: Encaminhar o tráfego de rede com uma tabela de rotas através do portal do Azure

O Azure encaminha automaticamente o tráfego entre todas as sub-redes numa rede virtual, por predefinição. Pode criar as sua próprias rotas para substituir o encaminhamento predefinido do Azure. A capacidade de criar rotas personalizadas é útil se, por exemplo, pretender encaminhar o tráfego entre sub-redes através de uma aplicação virtual de rede (NVA). Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma tabela de rotas
> * Criar uma rota
> * Criar uma rede virtual com várias sub-redes
> * Associar uma tabela de rotas a uma sub-rede
> * Criar uma NVA que encaminha o tráfego
> * Implementar máquinas virtuais (VMs) em sub-redes diferentes
> * Encaminhar o tráfego de uma sub-rede para outra através de uma NVA

Se preferir, pode concluir este tutorial com a [CLI do Azure](tutorial-create-route-table-cli.md) ou o [Azure PowerShell](tutorial-create-route-table-powershell.md).

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="create-a-route-table"></a>Criar uma tabela de rotas

1. Selecione **+ Criar um recurso**, no canto superior esquerdo do Portal do Azure.
2. Selecione **Redes** e, em seguida, selecione **Tabela de rotas**.
3. Introduza ou selecione as seguintes informações, aceite as predefinições para as restantes definições e, em seguida, selecione **Criar**:

    |Definição|Valor|
    |---|---|
    |Nome|myRouteTablePublic|
    |Subscrição| Selecione a sua subscrição.|
    |Grupo de recursos | Selecione **Criar novo** e introduza *myResourceGroup*.|
    |Localização|EUA Leste|
 
    ![Criar tabela de rotas](./media/tutorial-create-route-table-portal/create-route-table.png) 

## <a name="create-a-route"></a>Criar uma rota

1. Na caixa *Procurar recursos, serviços e documentos*, na parte superior do portal, comece a escrever *myRouteTablePublic*. Quando **myRouteTablePublic** aparecer nos resultados da pesquisa, selecione-a.
2. Em **DEFINIÇÕES**, selecione **Rotas** e selecione **+ Adicionar**, conforme mostrado na imagem seguinte:

    ![Adicionar rota](./media/tutorial-create-route-table-portal/add-route.png) 
 
3. Em **Adicionar rota**, introduza ou selecione as seguintes informações, aceite as predefinições para as restantes definições e, em seguida, selecione **Criar**:

    |Definição|Valor|
    |---|---|
    |Nome da rota|ToPrivateSubnet|
    |Prefixo de endereço| 10.0.1.0/24|
    |Tipo de salto seguinte | Selecione **Aplicação virtual**.|
    |Endereço do próximo salto| 10.0.2.4|

## <a name="associate-a-route-table-to-a-subnet"></a>Associar uma tabela de rotas a uma sub-rede

Antes de poder associar uma tabela de rotas a uma sub-rede, tem de criar uma rede virtual e uma sub-rede. Depois, pode associar a tabela de rotas a uma sub-rede.

1. Selecione **+ Criar um recurso**, no canto superior esquerdo do Portal do Azure.
2. Selecione **Redes** e, em seguida, selecione **Rede virtual**.
3. Em **Criar rede virtual**, introduza ou selecione as seguintes informações, aceite as predefinições para as restantes definições e, em seguida, selecione **Criar**:

    |Definição|Valor|
    |---|---|
    |Nome|myVirtualNetwork|
    |Espaço de endereços| 10.0.0.0/16|
    |Subscrição | Selecione a sua subscrição.|
    |Grupo de recursos|Selecione **Utilizar existente** e, em seguida, selecione **myResourceGroup**.|
    |Localização|Selecione *E.U.A. Leste*.|
    |Nome da sub-rede|Público|
    |Intervalo de endereços|10.0.0.0/24|
    
4. Na caixa **Procurar recursos, serviços e documentos**, na parte superior do portal, comece a escrever *myVirtualNetwork*. Quando **myVirtualNetwork** aparecer nos resultados da pesquisa, selecione-a.
5. Em **DEFINIÇÕES**, selecione **Sub-redes** e selecione **+ Sub-rede**, conforme mostrado na imagem seguinte:

    ![Adicionar sub-rede](./media/tutorial-create-route-table-portal/add-subnet.png) 

6. Selecione ou introduza as seguintes informações e selecione **OK**:

    |Definição|Valor|
    |---|---|
    |Nome|Privado|
    |Espaço de endereços| 10.0.1.0/24|

7. Conclua os passos 5 e 6 novamente, indicando as seguintes informações:

    |Definição|Valor|
    |---|---|
    |Nome|Rede de Perímetro|
    |Espaço de endereços| 10.0.2.0/24|

8. A caixa **myVirtualNetwork - Sub-redes** é apresentada depois de concluir o passo anterior. Em **DEFINIÇÕES**, selecione **Sub-redes** e selecione **Pública**.
9. Conforme mostrado na imagem seguinte, selecione **Tabela de rotas**, selecione **MyRouteTablePublic** e selecione **Guardar**:

    ![Associar a tabela de rotas](./media/tutorial-create-route-table-portal/associate-route-table.png) 

## <a name="create-an-nva"></a>Criar uma NVA

Uma NVA é uma VM que executa uma função de rede, como encaminhamento, proteção com firewalls ou otimização de WAN.

1. Selecione **+ Criar um recurso**, no canto superior esquerdo do Portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. Pode selecionar um sistema operativo diferente, mas os restantes passos partem do princípio de que selecionou **Windows Server 2016 Datacenter**. 
3. Selecione ou introduza as seguintes informações em **Informações básicas** e selecione **OK**:

    |Definição|Valor|
    |---|---|
    |Nome|myVmNva|
    |Nome de utilizador|Introduza um nome de utilizador à sua escolha.|
    |Palavra-passe|Introduza uma palavra-passe à sua escolha. A palavra-passe tem de ter, pelo menos, 12 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Grupo de recursos| Selecione **Utilizar existente** e, em seguida, selecione *myResourceGroup*.|
    |Localização|Selecione **E.U.A. Leste**.|
4. Selecione um tamanho de VM em **Escolher um tamanho**.
5. Selecione ou introduza as seguintes informações em **Definições** e selecione **OK**:

    |Definição|Valor|
    |---|---|
    |Rede virtual|myVirtualNetwork - se não estiver selecionada, selecione **Rede virtual** e selecione **myVirtualNetwork**, em **Escolher rede virtual**.|
    |Subrede|Selecione **Sub-rede** e selecione **DMZ**, em **Escolher sub-rede**. |
    |Endereço IP público| Selecione **Endereço IP público** e selecione **Nenhum**, em **Escolher endereço IP público**. Não vai ser atribuído nenhum endereço IP público a esta VM, porque a mesma não vai ser ligada a partir da Internet.
6. Em **Criar** no **Resumo**, selecione **Criar** para iniciar a implementação da VM.

    A criação da VM demora alguns minutos. Não continue para o passo seguinte enquanto o Azure não acabar de criar a VM e não for aberta uma caixa com informações sobre a VM.

7. Na caixa que foi aberta após a criação da VM, em **DEFINIÇÕES**, selecione **Redes** e selecione **myvmnva158** (a interface de rede que o Azure criou para a VM tem um número diferente a seguir a **myvmnva**), conforme mostrado na imagem seguinte.

    ![Redes de VM](./media/tutorial-create-route-table-portal/virtual-machine-networking.png) 

8. Para que as interfaces de rede possam reencaminhar o tráfego de rede que lhes é enviado e que não seja destinado para o seu próprio endereço IP, o reencaminhamento de IP tem de estar ativado nas mesmas. Em **DEFINIÇÕES**, selecione **Configurações de IP**, selecione **Ativado** em **Reencaminhamento de IP** e selecione **Guardar**, conforme mostrado na imagem seguinte:

    ![Ativar o reencaminhamento de IP](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png) 

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Crie duas VMs na rede virtual, para poder confirmar que o tráfego da sub-rede *Pública* é encaminhado para a *Privada* através da NVA num passo posterior. Conclua os passos 1 a 6 em [Criar uma NVA](#create-a-network-virtual-appliance). Utilize as mesmas definições dos passos 3 e 5, exceto as seguintes alterações:

|Nome da máquina virtual      |Subrede      | Endereço IP público     |
|--------- | -----------|---------              |
| myVmPublic  | Público     | Aceitar a predefinição do portal |
| myVmPrivate | Privado    | Aceitar a predefinição do portal |

Pode criar a VM *myVmPrivate* enquanto o Azure cria a VM *myVmPublic*. Não avance para os passos seguintes enquanto o Azure não acabar de criar ambas as VMs.

## <a name="route-traffic-through-an-nva"></a>Encaminhar o tráfego através de uma NVA

1. Na caixa *Pesquisar*, na parte superior do portal, comece a escrever *myVmPrivate*. Quando a VM **myVmPrivate** aparecer nos resultados da pesquisa, selecione-a.
2. Crie uma ligação de ambiente de trabalho remoto à VM *myVmPrivate* ao **Ligar**, conforme mostrado na imagem seguinte:

    ![Ligar à VM ](./media/tutorial-create-route-table-portal/connect-to-virtual-machine.png)  

3. Para ligar à VM, abra o ficheiro RDP transferido. Se lhe for pedido, selecione **Ligar**.
4. Introduza o nome de utilizador e a palavra-passe que especificou ao criar a VM (poderá ter de selecionar **Mais opções** e **Utilizar uma conta diferente** para especificar as credenciais que introduziu quando criou a VM) e, em seguida, selecione **OK**.
5. Poderá receber um aviso de certificado durante o processo de início de sessão. Selecione **Sim** para continuar a ligação.
6. Num passo posterior, vai ser utilizada a ferramenta de encaminhamento de rastreio para testar o encaminhamento. O encaminhamento de rastreio utiliza o protocolo ICMP (Internet Control Message Protocol), que é negado através da firewall do Windows. Introduza o seguinte comando do PowerShell na VM *myVmPrivate* para ativar o ICMP através da Firewall do Windows:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

    Apesar de o encaminhamento de rastreio ser utilizado para testar o encaminhamento neste tutorial, não é recomendado permitir o ICMP através da Firewall do Windows para implementações de produção.
7. Ativou o reencaminhamento de IP dentro do Azure na interface de rede da VM em [Ativar o encaminhamento de IP](#enable-ip-forwarding). Dentro da VM, é necessário que o sistema operativo, ou uma aplicação em execução na mesma, consiga também reencaminhar o tráfego de rede. Ative o reencaminhamento de IP no sistema operativo da VM *myVmNva*:

    Numa linha de comandos na VM *myVmPrivate*, aceda através de ambiente de trabalho remoto à VM *myVmNva*:

    ``` 
    mstsc /v:myvmnva
    ```
    
    Para ativar o reencaminhamento de IP no sistema operativo, introduza o seguinte comando no PowerShell da VM *myVmNva*:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```
    
    Reinicie a VM *myVmNva*, o que também desliga a sessão de ambiente de trabalho remoto.
8. Ainda ligado à VM *myVmPrivate*, crie uma sessão de ambiente de trabalho remoto para a VM *myVmPublic*, após a VM *myVmNva* ser reiniciada:

    ``` 
    mstsc /v:myVmPublic
    ```
    
    Introduza o seguinte comando do PowerShell na VM *myVmPublic* para ativar o ICMP através da Firewall do Windows:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

9. Para testar o encaminhamento do tráfego de rede para a VM *myVmPrivate* a partir da VM *myVmPublic*, introduza o seguinte comando do PowerShell na VM *myVmPublic*:

    ```
    tracert myVmPrivate
    ```

    A resposta é semelhante ao seguinte exemplo:
    
    ```
    Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
    over a maximum of 30 hops:
        
    1    <1 ms     *        1 ms  10.0.2.4
    2     1 ms     1 ms     1 ms  10.0.1.4
        
    Trace complete.
    ```
      
    Pode ver que o primeiro salto é 10.0.2.4, que é o endereço IP privado da NVA. O segundo salto é 10.0.1.4, o endereço IP privado da VM *myVmPrivate*. A rota adicionada à tabela de rotas *myRouteTablePublic* e associada à sub-rede *Pública* fez com que o Azure encaminhe o tráfego através da NVA, em vez de diretamente para a sub-rede *Privada*.
10.  Feche a sessão de ambiente de trabalho remoto para a VM *myVmPublic*, o que mantém a ligação à VM *myVmPrivate* ativa.
11. Para testar o encaminhamento do tráfego de rede para a VM *myVmPublic* a partir da VM *myVmPrivate*, introduza o seguinte comando numa linha de comandos na VM *myVmPrivate*:

    ```
    tracert myVmPublic
    ```

    A resposta é semelhante ao seguinte exemplo:

    ```
    Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:
    
    1     1 ms     1 ms     1 ms  10.0.0.4
    
    Trace complete.
    ```

    Pode ver que o tráfego é encaminhado diretamente da VM *myVmPrivate* para a VM *myVmPublic*. Por predefinição, o Azure encaminha o tráfego diretamente entre sub-redes.
12. Feche a sessão de ambiente de trabalho remoto para a VM *myVmPrivate*.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine o grupo de recursos e todos os recursos contidos no mesmo: 

1. Introduza *myResourceGroup* na caixa **Pesquisar**, na parte superior do portal. Quando vir **myResourceGroup** nos resultados da pesquisa, selecione-o.
2. Selecione **Eliminar grupo de recursos**.
3. Introduza *myResourceGroup* em **ESCREVER O NOME DO GRUPO DE RECURSOS:** e selecione **Eliminar**.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou uma tabela de rotas e associou-a a uma sub-rede. Criou uma NVA simples que encaminhou o tráfego de uma sub-rede pública para uma privada. Implementou uma variedade de NVAs pré-configuradas que realizam funções de rede, como proteção com firewall e otimização de WAN, a partir do [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Para saber mais sobre o encaminhamento, veja [Descrição geral do encaminhamento](virtual-networks-udr-overview.md) e [Manage a route table](manage-route-table.md) (Gerir uma tabela de rotas).


Embora possa implementar muitos recursos do Azure dentro de uma rede virtual, não é possível implementar recursos de alguns serviços PaaS do Azure nas redes virtuais. Contudo, pode restringir o acesso aos recursos de alguns serviços PaaS do Azure ao tráfego apenas a partir de uma sub-rede de rede virtual. Para saber como restringir o acesso de rede a recursos de PaaS do Azure, avance para o próximo tutorial.

> [!div class="nextstepaction"]
> [Restrict network access to PaaS resources](tutorial-restrict-network-access-to-resources.md) (Restringir o acesso de rede a recursos de PaaS)
