---
title: "Encaminhar o tráfego de rede - portal do Azure | Microsoft Docs"
description: "Saiba como encaminhar o tráfego de rede com uma tabela de rota com o portal do Azure."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/05/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 4cc814e1fd3ee8979662695f9dec3dcce335dc2a
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Encaminhar o tráfego de rede com uma tabela de rota com o portal do Azure

Azure automaticamente as rotas de tráfego entre todas as sub-redes na rede virtual, por predefinição. Pode criar as seus próprios rotas para substituir do Azure encaminhamento predefinido. A capacidade de criar rotas personalizadas é útil se, por exemplo, pretender encaminhar o tráfego entre sub-redes através de uma firewall. Neste artigo, saiba como:

> [!div class="checklist"]
> * Criar uma tabela de rota
> * Criar uma rota
> * Associar uma tabela de rota para uma sub-rede de rede virtual
> * Encaminhamento de teste
> * Resolver problemas de encaminhamento

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="create-a-route-table"></a>Criar uma tabela de rota

Tráfego de rotas do Azure entre todas as sub-redes na rede virtual, por predefinição. Para saber mais sobre as rotas predefinidas do Azure, consulte o artigo [rotas de sistema](virtual-networks-udr-overview.md). Substituir predefinição do Azure encaminhamento, pode criar uma tabela de rota que contém rotas e associar a tabela de rotas para uma sub-rede de rede virtual.

1. Selecione **+ criar um recurso** no canto superior, esquerda canto do portal do Azure.
2. Selecione **redes**e, em seguida, selecione **tabela de rotas**.
3. Selecione o **subscrição** e selecione ou introduza as seguintes informações, em seguida, selecione **criar**:
 
    ![Criar a tabela de rotas](./media/tutorial-create-route-table-portal/create-route-table.png) 

## <a name="create-a-route"></a>Criar uma rota

Uma tabela de rota contém zero ou mais rotas. 

1. No *procurar recursos, serviços e docs* caixa na parte superior do portal, começa a escrever *myRouteTablePublic*. Quando **myRouteTablePublic** aparece nos resultados da pesquisa, selecionados-lo.
2. Em **definições**, selecione **rotas** e, em seguida, selecione **+ adicionar**, conforme mostrado na imagem seguinte:

    ![Adicionar rota](./media/tutorial-create-route-table-portal/add-route.png) 
 
4. Em **adicionar rota**, selecione ou introduza as seguintes informações e, em seguida, selecione **OK**:
    - **Nome da rota**: *ToPrivateSubnet*
    - **Prefixo de endereço**: 10.0.1.0/24
    - **Próximo salto tipo**: selecione **aplicação Virtual**.
    - **Endereço do próximo salto**: 10.0.2.4

    A rota irá direcionar todo o tráfego destinado ao prefixo do endereço 10.0.1.0/24 através de uma aplicação virtual de rede com o endereço IP 10.0.2.4. O dispositivo de rede virtual e a sub-rede com o prefixo de endereço especificado são criados em passos posteriores. A rota substitui a predefinição do Azure encaminhamento, que encaminha o tráfego entre sub-redes diretamente. Cada rota especifica um tipo de salto seguinte. O tipo de próximo salto dá instruções ao Azure como encaminhar o tráfego. Neste exemplo, o tipo de próximo salto é *VirtualAppliance*. Para saber mais sobre os tipos de salto seguintes disponíveis no Azure e, quando a utilizá-los, consulte [próximo salto tipos](virtual-networks-udr-overview.md#custom-routes).

## <a name="associate-a-route-table-to-a-subnet"></a>Associar uma tabela de rota para uma sub-rede

Antes de poder associar uma tabela de rota para uma sub-rede, terá de criar uma rede virtual e a sub-rede:

1. Selecione **+ criar um recurso** no canto superior, esquerda canto do portal do Azure.
2. Selecione **redes**e, em seguida, selecione **rede Virtual**.
3. Em **criar rede virtual**, selecionar, ou introduza as seguintes informações, em seguida, selecione **criar**:
    
    - **Name**: *myVirtualNetwork*
    - **Espaço de endereços**: *10.0.0.0/16*
    - **Subscrição**: selecione a sua subscrição.
    - **Grupo de recursos**: selecione **utilizar existente** e selecione **myResourceGroup**.
    - **Localização**: selecione *EUA leste*
    - **Nome da sub-rede**: *público*
    - **Intervalo de endereços:** *10.0.0.0/24*

4. No **procurar recursos, serviços e docs** caixa na parte superior do portal, começa a escrever *myVirtualNetwork*. Quando **myVirtualNetwork** aparece nos resultados da pesquisa, selecionados-lo.
5. Adicione as duas sub-redes adicionais para a rede virtual. Em **definições**, selecione **sub-redes** e, em seguida, selecione **+ sub-rede**, conforme mostrado na imagem seguinte:

    ![Adicionar sub-rede](./media/tutorial-create-route-table-portal/add-subnet.png) 

6. Selecione ou introduza as seguintes informações, em seguida, selecione **OK**:
    - **Nome**: privada
    - **Espaço de endereços**: *10.0.1.0/24*

7. Conclua os passos 5 e 6 novamente, fornecendo as seguintes informações:
    - **Nome**: DMZ
    - **Espaço de endereços**: *10.0.2.0/24*

Pode associar uma tabela de rota para zero ou mais sub-redes. Uma sub-rede pode ter zero ou uma tabela de rotas associada ao mesmo. Tráfego de saída de uma sub-rede é encaminhado com base nas rotas predefinidas do Azure e as rotas personalizadas que adicionou a uma tabela de rota que associar a uma sub-rede. Associar o *myRouteTablePublic* tabela de rotas para o *pública* sub-rede:

1. O **myVirtualNetwork - sub-redes** é apresentada a caixa depois de concluir o passo anterior. Em **definições**, selecione **sub-redes** e, em seguida, selecione **pública**.
2. Como é mostrado na imagem seguinte, selecione **tabela de rotas**, em seguida, selecione **MyRouteTablePublic**.

    ![Associar a tabela de rotas](./media/tutorial-create-route-table-portal/associate-route-table.png) 

3. Selecione **Guardar**.

## <a name="test-routing"></a>Encaminhamento de teste

Para testar o encaminhamento, irá criar uma máquina virtual que funciona como a aplicação virtual de rede que a rota que criou no passo anterior encaminha através de. Depois de criar a aplicação virtual de rede, irá implementar uma máquina virtual para o *pública* e *privada* sub-redes. Em seguida, irá encaminhar o tráfego do *pública* sub-rede para o *privada* sub-rede através da aplicação virtual de rede.

### <a name="create-a-network-virtual-appliance"></a>Criar uma aplicação virtual de rede

No passo anterior, criou uma rota que especificada uma aplicação virtual de rede como o tipo de salto seguinte. Uma máquina virtual com uma aplicação de rede é frequentemente referida como uma aplicação virtual de rede. Em ambientes de produção, a aplicação virtual de rede que implementa é frequentemente uma máquina virtual previamente configurada. Várias aplicações virtuais de rede estão disponíveis a partir de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1). Neste artigo, é criada uma máquina virtual básica.

1. Selecione **+ criar um recurso** no canto superior, esquerda canto do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. Pode selecionar um sistema operativo diferente, mas os restantes passos partem do princípio de que selecionou **Datacenter do Windows Server 2016**. 
3. Selecione ou introduza as seguintes informações para **Noções básicas**, em seguida, selecione **OK**:
    - **Name**: *myVmNva*
    - **Grupo de recursos**: selecione **utilizar existente** e, em seguida, selecione *myResourceGroup*.
    - **Localização**: selecione *EUA Leste*.

    O **nome de utilizador** e **palavra-passe** introduzir são utilizadas num passo posterior. A palavra-passe tem de ter, pelo menos, 12 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm). O **localização** e **subscrição** selecionada tem de ser os mesmos como a localização e a subscrição da rede virtual está a ser. Não é necessário que seleciona o mesmo grupo de recursos que a rede virtual foi criada, mas o mesmo grupo de recursos está selecionado para este tutorial.
4. Selecione um tamanho VM em **escolher um tamanho**.
5. Selecione ou introduza as seguintes informações para **definições**, em seguida, selecione **OK**:
    - **Rede virtual**: Certifique-se de que **myVirtualNetwork** está selecionada. Se não, selecione **rede Virtual**, em seguida, selecione **myVirtualNetwork** em **escolha de rede virtual**.
    - **Sub-rede**: selecione **sub-rede** e, em seguida, selecione **DMZ** em **escolha subrede**.
    - **Endereço IP público**: selecione **endereço IP público** e selecione **nenhum** em **escolher endereço IP público**. Nenhum endereço IP público é atribuído a esta máquina virtual, uma vez que não será ligada a partir da Internet.
6. Em **criar** no **resumo**, selecione **criar** para iniciar a implementação da máquina virtual.

A máquina virtual demora alguns minutos a criar. Continue para o passo seguinte até que o Azure acaba de criar a máquina virtual e abre uma caixa com informações sobre a máquina virtual.

Quando o Azure criar a máquina virtual, também criou um [interface de rede](virtual-network-network-interface.md) no *DMZ* sub-rede e ligada a interface de rede para a máquina virtual. Reencaminhamento IP tem de estar ativado para cada interface de rede do Azure que reencaminha o tráfego destinado a qualquer endereço IP que não está atribuído à interface de rede.

1. Na caixa que aberta para a máquina virtual depois de foi criado, em **definições**, selecione **redes**e, em seguida, selecione **myvmnva158** (a interface de rede do Azure criado para a máquina virtual tem um número diferente após **myvmnva**), conforme mostrado na imagem seguinte:

    ![Redes de máquina virtual](./media/tutorial-create-route-table-portal/virtual-machine-networking.png) 

    Quando criou a aplicação virtual de rede no *DMZ* sub-rede, do Azure automaticamente criada a interface de rede, anexar a interface de rede para a máquina virtual e atribuir o endereço IP privado  *10.0.2.4* à interface de rede. 

2. Em **definições**, selecione **configurações de IP**, selecione **ativado** para **reencaminhamento IP**e, em seguida, selecione **guardar** , conforme mostrado na imagem seguinte:

    ![Ativar o reencaminhamento IP](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png) 

### <a name="create-virtual-machines"></a>Criar máquinas virtuais

Criar duas máquinas virtuais na rede virtual para que possa validar que o tráfego de entrada a *pública* sub-rede é encaminhada para o *privada* sub-rede através da aplicação virtual de rede num passo posterior.

Execute os passos 1-6 de [criar uma aplicação virtual de rede](#create-a-network-virtual-appliance). Utilize as mesmas definições nos passos 3 e 5, exceto as seguintes alterações:

|Máquina virtual   |Nome      |Subrede      | Endereço IP público     |
|---------         |--------- | -----------|---------              |
|Máquina virtual 1 | myVmWeb  | Público     | Aceite a predefinição de portal |
|máquina virtual 2 | myVmMgmt | Privado    | Aceite a predefinição de portal |

Pode criar o *myVmMgmt* máquina virtual enquanto o Azure cria a *myVmWeb* máquina virtual. Não continue com os seguintes passos até que o Azure acaba de criar ambas as máquinas virtuais.

### <a name="route-traffic-through-a-network-virtual-appliance"></a>Encaminhar o tráfego através de uma aplicação virtual de rede

1. No *pesquisa* caixa na parte superior do portal, começa a escrever *myVmMgmt*. Quando **myVmMgmt** aparece nos resultados da pesquisa, selecionados-lo.
2. Criar uma ligação de ambiente de trabalho remota para o *myVmMgmt* máquina virtual, selecionando **Connect**, conforme mostrado na imagem seguinte:

    ![Conectar à máquina virtual](./media/tutorial-create-route-table-portal/connect-to-virtual-machine.png)  

3. Para ligar à VM, abra o ficheiro RDP transferido. Se lhe for pedido, selecione **Connect**.
4. Introduza o nome de utilizador e palavra-passe que especificou ao criar a máquina virtual (poderá ter de selecionar **mais opções**, em seguida, **utilizar uma conta diferente**, para especificar as credenciais introduzidas quando a criar a máquina virtual), em seguida, selecione **OK**.
5. Poderá receber um aviso de certificado durante o processo de início de sessão. Selecione **Sim** para continuar com a ligação.
6. Num passo posterior, o comando tracert.exe é utilizado para testar o encaminhamento. Tracert utiliza o ICMP, que é negado através da Firewall do Windows. Ative o ICMP através da firewall do Windows, introduzindo o seguinte comando numa linha de comandos:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

    Embora tracert é utilizado para testar o encaminhamento neste artigo, permitir ICMP através da Firewall do Windows para implementações de produção não é recomendada.
7. Ativar o reencaminhamento IP no Azure para a interface de rede da máquina virtual no [ativar IP fowarding](#enable-ip-forwarding). Na máquina virtual, o sistema operativo ou uma aplicação em execução numa máquina virtual, deve também conseguir encaminhar o tráfego de rede. Quando implementa uma aplicação virtual de rede num ambiente de produção, o dispositivo normalmente filtra, regista ou efetua algumas outra função antes do reencaminhamento de tráfego. No entanto, neste artigo o sistema operativo simplesmente reencaminham todo o tráfego que recebe. Ativar o reencaminhamento IP no sistema operativo do *myVmNva* concluindo os seguintes passos do *myVmMgmt* máquina virtual:

    Ambiente de trabalho remoto para o *myVmNva* máquina virtual com o seguinte comando numa linha de comandos:

    ``` 
    mstsc /v:myvmnva
    ```
    
    Para ativar o IP de reencaminhamento no sistema operativo, introduza o seguinte comando do PowerShell:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```
    
    Reinicie a máquina virtual, que também irá desligar a sessão de ambiente de trabalho remota.
8. Enquanto ainda ligada para o *myVmMgmt* máquina virtual, após o *myVmNva* reinícios da máquina virtual, criar uma sessão de ambiente de trabalho remoto para o *myVmWeb* máquina virtual com o seguinte comando:

    ``` 
    mstsc /v:myVmWeb
    ```
    
    Ative o ICMP através da firewall do Windows, introduzindo o seguinte comando numa linha de comandos:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

9. Para testar o encaminhamento de tráfego de rede para o *myVmMgmt* máquina virtual a partir de *myVmWeb* máquina virtual, introduza o seguinte comando numa linha de comandos:

    ```
    tracert myvmmgmt
    ```

    A resposta é semelhante ao seguinte exemplo:
    
    ```
    Tracing route to myvmmgmt.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
    over a maximum of 30 hops:
        
    1    <1 ms     *        1 ms  10.0.2.4
    2     1 ms     1 ms     1 ms  10.0.1.4
        
    Trace complete.
    ```
      
    Pode ver que o primeiro salto é 10.0.2.4, que é o endereço IP privado a rede aplicação virtual. O segundo hop é 10.0.1.4, o endereço IP privado do *myVmMgmt* máquina virtual. A rota à *myRouteTablePublic* tabela de rotas e associado ao *pública* sub-rede causado do Azure encaminhar o tráfego através de NVA, em vez de diretamente para o *privada* sub-rede.
10.  Fechar a sessão de ambiente de trabalho remoto para o *myVmWeb* máquina virtual, deixando-ainda ligado para o *myVmMgmt* máquina virtual.
11. Para testar o encaminhamento de tráfego de rede para o *myVmWeb* máquina virtual a partir de *myVmMgmt* máquina virtual, introduza o seguinte comando numa linha de comandos:

    ```
    tracert myvmweb
    ```

    A resposta é semelhante ao seguinte exemplo:

    ```
    Tracing route to myvmweb.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:
    
    1     1 ms     1 ms     1 ms  10.0.0.4
    
    Trace complete.
    ```

    Pode ver que o tráfego é encaminhado diretamente a partir de *myVmMgmt* máquina virtual para o *myVmWeb* máquina virtual. Por predefinição, o tráfego de rotas do Azure diretamente entre sub-redes.
12. Fechar a sessão de ambiente de trabalho remoto para o *myVmMgmt* máquina virtual.

## <a name="troubleshoot-routing"></a>Resolver problemas de encaminhamento

Conforme aprendido nos passos anteriores, Azure aplica-se rotas predefinidas, que pode, opcionalmente, substituir com as seus próprios rotas. Por vezes, o tráfego poderão não ser encaminhado conforme esperado que seja. Pode utilizar o [próximo salto](../network-watcher/network-watcher-check-next-hop-portal.md) capacidade do observador de rede do Azure para determinar a forma como o Azure está encaminhamento de tráfego entre duas máquinas virtuais. 

1. No *pesquisa* caixa na parte superior do portal, começa a escrever *observador de rede*. Quando **observador de rede** aparece nos resultados da pesquisa, selecionados-lo.
2. Em **as ferramentas de diagnóstico de rede**, selecione **próximo salto**.
3. Para testar o encaminhamento de tráfego do *myVmWeb* (10.0.0.4) máquina virtual para o *myVmMgmt* (10.0.1.4) máquina virtual, selecione a sua subscrição, introduza as informações mostradas na imagem seguinte (o **Interface de rede** nome é ligeiramente diferente) e, em seguida, selecione **próximo salto**:

    ![Próximo salto](./media/tutorial-create-route-table-portal/next-hop.png)  

    O **resultado** saída informa que o próximo salto endereço IP para o tráfego de *myVmWeb* para *myVmMgmt* é 10.0.2.4 (o *myVmNva* máquina virtual), que o tipo de próximo salto é *VirtualAppliance*, sendo que a tabela de rota que faz com que o encaminhamento *myRouteTablePublic*.

As rotas em vigor para cada interface de rede são uma combinação de rotas predefinidas do Azure e as rotas que define. Para ver todas as rotas eficazes para uma interface de rede numa máquina virtual, conclua os seguintes passos:

1. No *pesquisa* caixa na parte superior do portal, começa a escrever *myVmWeb*. Quando **myVmWeb** aparece nos resultados da pesquisa, selecionados-lo.
2. Em **definições**, selecione **redes**e, em seguida, selecione **myvmweb369** (a interface de rede Azure criado para a máquina virtual tem um número diferente após **myvmweb**).
3. Em **suporte + resolução de problemas**, selecione **rotas efetivas**, conforme mostrado na imagem seguinte:

    ![Rotas efetivas](./media/tutorial-create-route-table-portal/effective-routes.png) 

    Para ver as rotas predefinidas do Azure e a rota adicionado no *myRouteTablePublic* tabela de rotas. Para obter mais informações sobre como o Azure seleciona uma rota da lista de rotas, consulte o artigo [como Azure seleciona uma rota](virtual-networks-udr-overview.md#how-azure-selects-a-route).

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, elimine o grupo de recursos e todos os recursos que contém: 

1. Introduza *myResourceGroup* no **pesquisa** caixa na parte superior do portal. Quando vir **myResourceGroup** nos resultados da pesquisa, selecione-o.
2. Selecione **Eliminar grupo de recursos**.
3. Introduza *myResourceGroup* para **tipo o nome de grupo de recursos:** e selecione **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, criou uma tabela de rota e associados-la a uma sub-rede. Criar uma aplicação virtual de rede que encaminhados tráfego de sub-rede público para uma sub-rede privada. Enquanto pode implementar vários recursos do Azure dentro de uma rede virtual, os recursos para alguns serviços do Azure PaaS não não possível implementar numa rede virtual. Pode ainda restringir o acesso aos recursos de alguns serviços do Azure PaaS para o tráfego apenas a partir de uma sub-rede de rede virtual apesar. Avançar para o próximo tutorial para saber como restringir o acesso de rede para recursos do Azure PaaS.

> [!div class="nextstepaction"]
> [Restringir o acesso de rede para recursos de PaaS](virtual-network-service-endpoints-configure.md#azure-portal)
