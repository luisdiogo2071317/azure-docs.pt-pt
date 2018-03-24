---
title: Encaminhar o tráfego de rede - portal do Azure | Microsoft Docs
description: Saiba como encaminhar o tráfego de rede com uma tabela de rota com o portal do Azure.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 980cf7b59ed16778bbb6cd1b657e3522407c79c9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Encaminhar o tráfego de rede com uma tabela de rota com o portal do Azure

Azure automaticamente as rotas de tráfego entre todas as sub-redes na rede virtual, por predefinição. Pode criar as seus próprios rotas para substituir do Azure encaminhamento predefinido. A capacidade de criar rotas personalizadas é útil se, por exemplo, pretender encaminhar o tráfego entre sub-redes através de uma aplicação virtual de rede (NVA). Neste artigo, saiba como:

> [!div class="checklist"]
> * Criar uma tabela de rota
> * Criar uma rota
> * Criar uma rede virtual com várias sub-redes
> * Associar uma tabela de rota para uma sub-rede
> * Criar uma NVA que encaminha o tráfego
> * Implementar máquinas virtuais (VM) em sub-redes diferentes
> * Encaminhar o tráfego de uma sub-rede para outra através de uma NVA

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="create-a-route-table"></a>Criar uma tabela de rota

1. Selecione **+ criar um recurso** no canto superior, esquerda canto do portal do Azure.
2. Selecione **redes**e, em seguida, selecione **tabela de rotas**.
3. Introduza, ou selecione as seguintes informações, aceite a predefinição para a definição restantes e, em seguida, selecione **criar**:

    |Definição|Valor|
    |---|---|
    |Nome|myRouteTablePublic|
    |Subscrição| Selecione a sua subscrição.|
    |Grupo de recursos | Selecione **criar nova** e introduza *myResourceGroup*.|
    |Localização|EUA Leste|
 
    ![Criar a tabela de rotas](./media/tutorial-create-route-table-portal/create-route-table.png) 

## <a name="create-a-route"></a>Criar uma rota

1. No *procurar recursos, serviços e docs* caixa na parte superior do portal, começa a escrever *myRouteTablePublic*. Quando **myRouteTablePublic** aparece nos resultados da pesquisa, selecionados-lo.
2. Em **definições**, selecione **rotas** e, em seguida, selecione **+ adicionar**, conforme mostrado na imagem seguinte:

    ![Adicionar rota](./media/tutorial-create-route-table-portal/add-route.png) 
 
3. Em **adicionar rota**, introduza, ou selecione as seguintes informações, aceite a predefinição para as restantes definições e, em seguida, selecione **criar**:

    |Definição|Valor|
    |---|---|
    |Nome da rota|ToPrivateSubnet|
    |Prefixo de endereço| 10.0.1.0/24|
    |Tipo de salto seguinte | Selecione **aplicação Virtual**.|
    |Endereço do próximo salto| 10.0.2.4|

## <a name="associate-a-route-table-to-a-subnet"></a>Associar uma tabela de rota para uma sub-rede

Antes de poder associar uma tabela de rota para uma sub-rede, tem de criar uma rede virtual e uma sub-rede, em seguida, pode associar a tabela de rotas para uma sub-rede:

1. Selecione **+ criar um recurso** no canto superior, esquerda canto do portal do Azure.
2. Selecione **redes**e, em seguida, selecione **rede Virtual**.
3. Em **criar rede virtual**, introduza, ou selecione as seguintes informações, aceite a predefinição para as restantes definições e, em seguida, selecione **criar**:

    |Definição|Valor|
    |---|---|
    |Nome|myVirtualNetwork|
    |Espaço de endereços| 10.0.0.0/16|
    |Subscrição | Selecione a sua subscrição.|
    |Grupo de recursos|Selecione **utilizar existente** e, em seguida, selecione **myResourceGroup**.|
    |Localização|Selecione *EUA leste*|
    |Nome da sub-rede|Público|
    |Intervalo de endereços|10.0.0.0/24|
    
4. No **procurar recursos, serviços e docs** caixa na parte superior do portal, começa a escrever *myVirtualNetwork*. Quando **myVirtualNetwork** aparece nos resultados da pesquisa, selecionados-lo.
5. Em **definições**, selecione **sub-redes** e, em seguida, selecione **+ sub-rede**, conforme mostrado na imagem seguinte:

    ![Adicionar sub-rede](./media/tutorial-create-route-table-portal/add-subnet.png) 

6. Selecione ou introduza as seguintes informações, em seguida, selecione **OK**:

    |Definição|Valor|
    |---|---|
    |Nome|Privado|
    |Espaço de endereços| 10.0.1.0/24|

7. Conclua os passos 5 e 6 novamente, fornecendo as seguintes informações:

    |Definição|Valor|
    |---|---|
    |Nome|Rede de Perímetro|
    |Espaço de endereços| 10.0.2.0/24|

8. O **myVirtualNetwork - sub-redes** é apresentada a caixa depois de concluir o passo anterior. Em **definições**, selecione **sub-redes** e, em seguida, selecione **pública**.
9. Como é mostrado na imagem seguinte, selecione **tabela de rotas**, selecione **MyRouteTablePublic**e, em seguida, selecione **guardar**:

    ![Associar a tabela de rotas](./media/tutorial-create-route-table-portal/associate-route-table.png) 

## <a name="create-an-nva"></a>Criar uma NVA

Uma NVA, é uma VM que executa uma função de rede, tais como o encaminhamento de mensagens em fila, firewalling ou otimização de WAN.

1. Selecione **+ criar um recurso** no canto superior, esquerda canto do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. Pode selecionar um sistema operativo diferente, mas os restantes passos partem do princípio de que selecionou **Datacenter do Windows Server 2016**. 
3. Selecione ou introduza as seguintes informações para **Noções básicas**, em seguida, selecione **OK**:

    |Definição|Valor|
    |---|---|
    |Nome|myVmNva|
    |Nome de utilizador|Introduza um nome de utilizador à sua escolha.|
    |Palavra-passe|Introduza uma palavra-passe da sua escolha. A palavra-passe tem de ter, pelo menos, 12 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Grupo de recursos| Selecione **utilizar existente** e, em seguida, selecione *myResourceGroup*.|
    |Localização|Selecione **EUA Leste**.|
4. Selecione um tamanho VM em **escolher um tamanho**.
5. Selecione ou introduza as seguintes informações para **definições**, em seguida, selecione **OK**:

    |Definição|Valor|
    |---|---|
    |Rede virtual|myVirtualNetwork - se não estiver selecionada, selecione **rede Virtual**, em seguida, selecione **myVirtualNetwork** em **escolha de rede virtual**.|
    |Subrede|Selecione **sub-rede** e, em seguida, selecione **DMZ** em **escolha subrede**. |
    |Endereço IP público| Selecione **endereço IP público** e selecione **nenhum** em **escolher endereço IP público**. Nenhum endereço IP público é atribuído a esta VM, uma vez que não será ligada a partir da Internet.
6. Em **criar** no **resumo**, selecione **criar** para iniciar a implementação de VM.

    A VM demora alguns minutos a criar. Continue para o passo seguinte até que o Azure acaba de criar a VM e abre uma caixa com informações sobre a VM.

7. Na caixa que aberta para a VM depois foi criado, em **definições**, selecione **redes**e, em seguida, selecione **myvmnva158** (a interface de rede Azure criado para o A VM tem um número diferente após **myvmnva**), conforme mostrado na imagem seguinte:

    ![Redes de VM](./media/tutorial-create-route-table-portal/virtual-machine-networking.png) 

8. Para uma interface de rede conseguir encaminhar o tráfego de rede enviado, que não é destinado a entidades para o seu próprio endereço IP, o reencaminhamento IP tem de estar ativado para a interface de rede. Em **definições**, selecione **configurações de IP**, selecione **ativado** para **reencaminhamento IP**e, em seguida, selecione **guardar** , conforme mostrado na imagem seguinte:

    ![Ativar o reencaminhamento IP](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png) 

## <a name="create-virtual-machines"></a>Criar máquinas virtuais

Criar duas VMs na rede virtual, para que possa validar que o tráfego de entrada a *pública* sub-rede é encaminhada para o *privada* sub-rede através da NVA num passo posterior. Execute os passos 1-6 de [criar uma NVA](#create-a-network-virtual-appliance). Utilize as mesmas definições nos passos 3 e 5, exceto as seguintes alterações:

|Nome da máquina virtual      |Subrede      | Endereço IP público     |
|--------- | -----------|---------              |
| myVmPublic  | Público     | Aceite a predefinição de portal |
| myVmPrivate | Privado    | Aceite a predefinição de portal |

Pode criar o *myVmPrivate* VM enquanto o Azure cria a *myVmPublic* VM. Não continue com os seguintes passos até que o Azure acaba de criar ambas as VMs.

## <a name="route-traffic-through-an-nva"></a>Encaminhar o tráfego através de uma NVA

1. No *pesquisa* caixa na parte superior do portal, começa a escrever *myVmPrivate*. Quando o **myVmPrivate** VM aparece nos resultados da pesquisa, selecione-o.
2. Criar uma ligação de ambiente de trabalho remota para o *myVmPrivate* VM selecionando **Connect**, conforme mostrado na imagem seguinte:

    ![Ligar à VM ](./media/tutorial-create-route-table-portal/connect-to-virtual-machine.png)  

3. Para ligar à VM, abra o ficheiro RDP transferido. Se lhe for pedido, selecione **Connect**.
4. Introduza o nome de utilizador e palavra-passe que especificou ao criar a VM (poderá ter de selecionar **mais opções**, em seguida, **utilizar uma conta diferente**, para especificar as credenciais que introduziu quando criou a VM), em seguida, selecione **OK**.
5. Poderá receber um aviso de certificado durante o processo de início de sessão. Selecione **Sim** para continuar com a ligação.
6. Num passo posterior, o comando tracert.exe é utilizado para testar o encaminhamento. Tracert utiliza o controlo de mensagem de ICMP (Internet Protocol), que é negado através da Firewall do Windows. Ative o ICMP através da firewall do Windows, introduzindo o seguinte comando do PowerShell:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

    Embora tracert é utilizado para testar o encaminhamento neste artigo, permitir ICMP através da Firewall do Windows para implementações de produção não é recomendada.
7. Ativar o reencaminhamento IP no Azure para a interface de rede da VM no [ativar IP fowarding](#enable-ip-forwarding). Dentro da VM, o sistema operativo ou uma aplicação em execução dentro da VM, deve também conseguir encaminhar o tráfego de rede. Ativar o reencaminhamento IP no sistema operativo do *myVmNva* VM concluindo os seguintes passos do *myVmPrivate* VM:

    Ambiente de trabalho remoto para o *myVmNva* com o seguinte comando numa linha de comandos:

    ``` 
    mstsc /v:myvmnva
    ```
    
    Para ativar o IP de reencaminhamento no sistema operativo, introduza o seguinte comando do PowerShell:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```
    
    Reinicie a VM, que também desliga a sessão de ambiente de trabalho remota.
8. Enquanto ainda ligada à *myVmPrivate* VM, criar uma sessão de ambiente de trabalho remoto para o *myVmPublic* VM com o seguinte comando, após o *myVmNva* reinicia a VM:

    ``` 
    mstsc /v:myVmPublic
    ```
    
    Ative o ICMP através da firewall do Windows, introduzindo o seguinte comando do PowerShell:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

9. Para testar o encaminhamento de tráfego de rede para o *myVmPrivate* VM a partir de *myVmPublic* VM, introduza o seguinte comando do PowerShell:

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
      
    Pode ver que o primeiro salto é 10.0.2.4, que é o endereço IP privado a NVA. O segundo hop é 10.0.1.4, o endereço IP privado do *myVmPrivate* VM. A rota à *myRouteTablePublic* tabela de rotas e associado ao *pública* sub-rede causado do Azure encaminhar o tráfego através de NVA, em vez de diretamente para o *privada* sub-rede.
10.  Fechar a sessão de ambiente de trabalho remoto para o *myVmPublic* VM, deixando-ainda ligado para o *myVmPrivate* VM.
11. Para testar o encaminhamento de tráfego de rede para o *myVmPublic* VM a partir de *myVmPrivate* VM, introduza o seguinte comando numa linha de comandos:

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

    Pode ver que o tráfego é encaminhado diretamente a partir de *myVmPrivate* VM para o *myVmPublic* VM. Por predefinição, o tráfego de rotas do Azure diretamente entre sub-redes.
12. Fechar a sessão de ambiente de trabalho remoto para o *myVmPrivate* VM.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, elimine o grupo de recursos e todos os recursos que contém: 

1. Introduza *myResourceGroup* no **pesquisa** caixa na parte superior do portal. Quando vir **myResourceGroup** nos resultados da pesquisa, selecione-o.
2. Selecione **Eliminar grupo de recursos**.
3. Introduza *myResourceGroup* para **tipo o nome de grupo de recursos:** e selecione **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, criou uma tabela de rota e associados-la a uma sub-rede. Criou uma NVA simple que encaminhados tráfego de sub-rede público para uma sub-rede privada. Implementar uma variedade de NVAs pré-configurados que efetuam funções de rede, tais como a firewall e a otimização de WAN do [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Antes de implementar as tabelas de rotas para utilização em produção, recomenda-se que lhe exaustivamente familiarizar-se com [encaminhamento no Azure](virtual-networks-udr-overview.md), [tabelas de rota de gerir](manage-route-table.md), e [Azure limita](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).


Enquanto pode implementar vários recursos do Azure dentro de uma rede virtual, os recursos para alguns serviços do Azure PaaS não não possível implementar numa rede virtual. Pode ainda restringir o acesso aos recursos de alguns serviços do Azure PaaS para o tráfego apenas a partir de uma sub-rede de rede virtual apesar. Avançar para o próximo tutorial para saber como restringir o acesso de rede para recursos do Azure PaaS.

> [!div class="nextstepaction"]
> [Restringir o acesso de rede para recursos de PaaS](tutorial-restrict-network-access-to-resources.md)
