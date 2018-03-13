---
title: "Criar uma rede Virtual do Azure com várias sub-redes - Portal | Microsoft Docs"
description: "Saiba como criar uma rede virtual com várias sub-redes no portal do Azure."
services: virtual-network
documentationcenter: 
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 898bdef779282d7312c76696f744b97ec2dfcded
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="create-a-virtual-network-with-multiple-subnets-using-the-azure-portal"></a>Criar uma rede virtual com várias sub-redes no portal do Azure

Uma rede virtual permite que vários tipos de recursos do Azure comuniquem com a Internet e modo privado entre si. Criar várias sub-redes numa rede virtual permite-lhe segmentar a sua rede para que pode filtrar ou controlar o fluxo de tráfego entre sub-redes. Neste artigo, saiba como:

> [!div class="checklist"]
> * Criar uma rede virtual
> * Criar uma sub-rede
> * Comunicação de rede entre as máquinas virtuais de teste

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="create-a-virtual-network"></a>Criar uma rede virtual

1. Selecione **+ criar um recurso** no canto superior, esquerda canto do portal do Azure.
2. Selecione **redes**e, em seguida, selecione **rede Virtual**.
3. Como é mostrado na imagem seguinte, introduza *myVirtualNetwork* para **nome**, *10.0.0.0/16* para **espaço de endereços**,  **myResourceGroup** para **grupo de recursos**, *pública* para sub-rede **nome**, 10.0.0.0/24 para sub-rede **deintervalodeendereços**, selecione um **localização** e os seus **subscrição**, aceite as restantes predefinições e, em seguida, selecione **criar**:

    ![Criar uma rede virtual](./media/virtual-networks-create-vnet-arm-pportal/create-virtual-network.png)

    O **espaço de endereços** e **intervalo de endereços** estão especificados na notação CIDR. Especificado **espaço de endereços** inclui 10.0.0.0-10.0.255.254 de endereços IP. O **intervalo de endereços** especificado para uma sub-rede, tem de estar dentro do **espaço de endereços** definido para a rede virtual. Azure DHCP atribui endereços IP de um intervalo de endereço de sub-rede para recursos implementados numa sub-rede. Azure atribui apenas o 10.0.0.4-10.0.0.254 endereços a recursos implementados no **pública** sub-rede, porque o Azure reserva de quatro primeiras endereços (10.0.0.0-10.0.0.3 para a sub-rede, neste exemplo) e o último endereço ( 10.0.0.255 para a sub-rede, neste exemplo) em cada sub-rede.

## <a name="create-a-subnet"></a>Criar uma sub-rede

1. No **procurar recursos, serviços e docs** caixa na parte superior do portal, começa a escrever *myVirtualNetwork*. Quando **myVirtualNetwork** aparece nos resultados da pesquisa, selecionados-lo.
2. Selecione **sub-redes** e, em seguida, selecione **+ sub-rede**, conforme mostrado na imagem seguinte:

     ![Adicionar uma sub-rede](./media/virtual-networks-create-vnet-arm-pportal/add-subnet.png)
     
3. No **adicionar sub-rede** caixa que aparece, introduza *privada* para **nome**, introduza *10.0.1.0/24* para **deintervalodeendereços**e, em seguida, selecione **OK**.  Um intervalo de endereços de sub-rede não pode sobrepor-se com os intervalos de endereços de outras sub-redes dentro de uma rede virtual. 

Antes de implementar redes virtuais do Azure e sub-redes para utilização em produção, recomenda-se que lhe exaustivamente familiarizar-se com o espaço de endereços [considerações](manage-virtual-network.md#create-a-virtual-network) e [limites de rede virtual](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Depois de recursos são implementados em sub-redes, algumas rede virtual e alterações de sub-rede, tal como alterar os intervalos de endereços, podem exigir a reimplementação de recursos do Azure existentes implementadas em sub-redes.

## <a name="test-network-communication"></a>Comunicação de rede de teste

Uma rede virtual permite que vários tipos de recursos do Azure comuniquem com a Internet e modo privado entre si. Um tipo de recurso que pode implementar numa rede virtual é uma máquina virtual. Crie duas máquinas virtuais na rede virtual para que possa testá comunicação de rede entre eles e a Internet num passo posterior.

### <a name="create-virtual-machines"></a>Criar máquinas virtuais

1. Selecione **+ criar um recurso** no canto superior, esquerda canto do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. Pode selecionar um sistema operativo diferente, mas os restantes passos partem do princípio de que selecionou **Datacenter do Windows Server 2016**. 
3. Selecione ou introduza as seguintes informações para **Noções básicas**, em seguida, selecione **OK**:
    - **Name**: *myVmWeb*
    - **Grupo de recursos**: selecione **utilizar existente** e, em seguida, selecione *myResourceGroup*.
    - **Localização**: selecione *EUA Leste*.

    O **nome de utilizador** e **palavra-passe** introduzir são utilizadas num passo posterior. A palavra-passe tem de ter, pelo menos, 12 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm). O **localização** e **subscrição** selecionada tem de ser os mesmos como a localização e a subscrição da rede virtual está a ser. Não é necessário que seleciona o mesmo grupo de recursos que a rede virtual foi criada, mas o mesmo grupo de recursos está selecionado para este tutorial.
4. Selecione um tamanho VM em **escolher um tamanho**.
5. Selecione ou introduza as seguintes informações para **definições**, em seguida, selecione **OK**:
    - **Rede virtual**: Certifique-se de que **myVirtualNetwork** está selecionada. Se não, selecione **rede Virtual** e, em seguida, selecione **myVirtualNetwork** em **escolha de rede virtual**.
    - **Sub-rede**: Certifique-se de que **pública** está selecionada. Se não, selecione **sub-rede** e, em seguida, selecione **pública** em **escolha subrede**, conforme mostrado na imagem seguinte:
    
        ![Definições da máquina virtual](./media/virtual-networks-create-vnet-arm-pportal/virtual-machine-settings.png)
 
6. Em **criar** no **resumo**, selecione **criar** para iniciar a implementação da máquina virtual.
7. Execute novamente os passos 1-6, mas introduzir *myVmMgmt* para o **nome** da máquina virtual e selecione **privada** para o **sub-rede**.

As máquinas virtuais demorar alguns minutos a criar. Não continue com os restantes passos até que ambas as máquinas virtuais são criadas.

As máquinas virtuais criadas neste artigo tem um [interface de rede](virtual-network-network-interface.md) com um endereço IP dinamicamente atribuído à interface de rede. Depois de implementar a VM, pode [adicionar vários endereços IP públicos e privados, ou alterar o método de atribuição de endereços IP para estático](virtual-network-network-interface-addresses.md#add-ip-addresses). Pode [adicionar interfaces de rede](virtual-network-network-interface-vm.md#vm-add-nic), até ao limite suportado pelo [tamanho da VM](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) que selecionar quando criar uma máquina virtual. Também pode [ativar a virtualização de e/s de raiz única (SR-IOV)](create-vm-accelerated-networking-powershell.md) para uma VM, mas apenas quando criar uma VM com um tamanho VM que suporta a capacidade.

### <a name="communicate-between-virtual-machines-and-with-the-internet"></a>Comunicar entre máquinas virtuais e com a internet

1. No *pesquisa* caixa na parte superior do portal, começa a escrever *myVmMgmt*. Quando **myVmMgmt** aparece nos resultados da pesquisa, selecionados-lo.
2. Criar uma ligação de ambiente de trabalho remota para o *myVmMgmt* máquina virtual, selecionando **Connect**, conforme mostrado na imagem seguinte:

    ![Conectar à máquina virtual](./media/virtual-networks-create-vnet-arm-pportal/connect-to-virtual-machine.png)  

3. Para ligar à VM, abra o ficheiro RDP transferido. Se lhe for pedido, selecione **Connect**.
4. Introduza o nome de utilizador e palavra-passe que especificou ao criar a máquina virtual (poderá ter de selecionar **mais opções**, em seguida, **utilizar uma conta diferente**, para especificar as credenciais introduzidas quando a criar a máquina virtual), em seguida, selecione **OK**.
5. Poderá receber um aviso de certificado durante o processo de início de sessão. Selecione **Sim** para continuar com a ligação.
6. Num passo posterior, ping é utilizado para comunicar com o *myVmMgmt* máquina virtual a partir de *myVmWeb* máquina virtual. Utiliza o ping ICMP, que é negado através da Firewall do Windows, por predefinição. Ative o ICMP através da firewall do Windows, introduzindo o seguinte comando numa linha de comandos:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

    Apesar de ping é utilizado neste artigo, permitir ICMP através da Firewall do Windows para implementações de produção não é recomendada.
7. Por motivos de segurança, é comum para limitar o número de máquinas virtuais que podem ser ligados remotamente numa rede virtual. Neste tutorial, o *myVmMgmt* máquina virtual é utilizada para gerir o *myVmWeb* máquina virtual na rede virtual. Para o ambiente de trabalho remoto para o *myVmWeb* máquina virtual a partir de *myVmMgmt* máquina virtual, introduza o seguinte comando numa linha de comandos:

    ``` 
    mstsc /v:myVmWeb
    ```
8. Para comunicar com o *myVmMgmt* máquina virtual a partir de *myVmWeb* máquina virtual, introduza o seguinte comando numa linha de comandos:

    ```
    ping myvmmgmt
    ```

    Poderá recebe um resultado semelhante ao seguinte exemplo de saída:
    
    ```
    Pinging myvmmgmt.dar5p44cif3ulfq00wxznl3i3f.bx.internal.cloudapp.net [10.0.1.4] with 32 bytes of data:
    Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    
    Ping statistics for 10.0.1.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 0ms, Average = 0ms
    ```
      
    Pode ver que o endereço do *myVmMgmt* máquina virtual é 10.0.1.4. 10.0.1.4 foi o primeiro endereço IP disponível no intervalo de endereços do *privada* sub-rede que implementou o *myVmMgmt* máquina virtual num passo anterior.  Verá que o nome de domínio completamente qualificado da máquina virtual é *myvmmgmt.dar5p44cif3ulfq00wxznl3i3f.bx.internal.cloudapp.net*. Embora o *dar5p44cif3ulfq00wxznl3i3f* parte do nome de domínio é diferente para a máquina virtual, o restante do nome de domínio são os mesmos. 

    Por predefinição, todas as máquinas virtuais do Azure utilizam o serviço de DNS do Azure predefinido. Todas as máquinas virtuais dentro de uma rede virtual pode resolver os nomes de todas as outras máquinas virtuais na mesma rede virtual com o serviço DNS do Azure predefinido. Em vez de utilizar o serviço DNS predefinido do Azure, pode utilizar o seu próprio servidor DNS ou a capacidade de domínio privada do serviço DNS do Azure. Para obter mais informações, consulte [resolução de nomes utilizando o seu próprio servidor DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) ou [utilizando o DNS do Azure para domínios privados](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

9. Para instalar serviços de informação Internet (IIS) para o Windows Server no *myVmWeb* máquina virtual, introduza o comando seguinte a partir de uma sessão do PowerShell:

    ```powershell
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    ```

10. Depois de concluída a instalação do IIS, desligue o *myVmWeb* sessões de ambiente de trabalho remoto, deixando-na *myVmMgmt* sessões de ambiente de trabalho remoto. Abra um browser e navegue para http://myvmweb. Consulte o página de boas-vindas do IIS.
11. Desligar o *myVmMgmt* sessões de ambiente de trabalho remoto.
12. Localizar o endereço IP público do *myVmWeb* máquina virtual. Quando o Azure criado o *myVmWeb* máquina virtual, um recurso de endereço IP público com o nome *myVmWeb* também foi criada e atribuída à máquina virtual. Pode ver que 52.170.5.92 foi atribuída ao **endereço IP público** para o *myVmMgmt* máquina virtual na imagem no passo 2. Para localizar o endereço IP público atribuído para o *myVmWeb* máquina virtual, procure *myVmWeb* na caixa de pesquisa do portal, em seguida, selecioná-lo quando é apresentado nos resultados da pesquisa.

    Apesar de uma máquina virtual não é necessário ter um endereço IP público atribuído ao mesmo, o Azure atribui um endereço IP público para cada máquina virtual que cria, por predefinição. Para comunicar a partir da Internet para uma máquina virtual, um endereço IP público tem de ser atribuído à máquina virtual. Todas as máquinas virtuais podem comunicar saída com a Internet, quer tenha ou não está atribuído um endereço IP público para a máquina virtual. Para saber mais sobre ligações de Internet de saída no Azure, consulte o artigo [ligações de saída no Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
13. No seu computador, navegue para o endereço IP público do *myVmWeb* máquina virtual. Falha ao tentar ver a página de boas-vindas do IIS do seu computador. A tentativa falhar porque quando as máquinas virtuais foram implementadas, o Azure criado um grupo de segurança de rede para cada máquina virtual, por predefinição. 

     Um grupo de segurança de rede contém regras de segurança que permitem ou negam o tráfego de rede de entrada e saída pela porta e endereço IP. O grupo de segurança de rede predefinido criado do Azure permite a comunicação através de todas as portas entre os recursos na mesma rede virtual. Para máquinas virtuais do Windows, o grupo de segurança de rede predefinido nega todo o tráfego de entrada da Internet através de todas as portas, exceto a porta TCP 3389 (RDP). Como consequência, por predefinição, também pode RDP diretamente para o *myVmWeb* máquina virtual a partir da Internet, apesar de não poderá porta 3389 aberto a um servidor web. Uma vez que a navegação na web comunica através da porta 80, falha de comunicação da Internet porque não há nenhuma regra num grupo de segurança de rede predefinido a permitir o tráfego através da porta 80.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não é necessário, elimine o grupo de recursos e todos os recursos que contém: 

1. Introduza *myResourceGroup* no **pesquisa** caixa na parte superior do portal. Quando vir **myResourceGroup** nos resultados da pesquisa, selecione-o.
2. Selecione **Eliminar grupo de recursos**.
3. Introduza *myResourceGroup* para **tipo o nome de grupo de recursos:** e selecione **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu a implementar uma rede virtual com várias sub-redes. Também aprendeu que quando cria uma máquina virtual do Windows, o Azure cria uma interface de rede que liga à máquina virtual e cria um grupo de segurança de rede que permita que apenas o tráfego através da porta 3389, através da Internet. Avançar para o próximo tutorial para saber como filtrar o tráfego de rede a sub-redes, em vez de para máquinas virtuais individuais.

> [!div class="nextstepaction"]
> [Filtrar o tráfego de rede para sub-redes](./virtual-networks-create-nsg-arm-pportal.md)
