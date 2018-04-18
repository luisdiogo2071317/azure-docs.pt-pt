---
title: Criar um Balanceador de Carga Básico público – Portal do Azure | Microsoft Docs
description: Saiba como criar um Balanceador de Carga Básico público com o Portal do Azure.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: aa9d26ca-3d8a-4a99-83b7-c410dd20b9d0
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: kumud
ms.openlocfilehash: c646b0b1ab0ec62cffb4f7cf7474b48c68dfabb4
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="create-a-public-basic-load-balancer-to-load-balance-vms-using-the-azure-portal"></a>Criar um Balanceador de Carga Básico público para fazer o balanceamento de carga das VMs com o Portal do Azure

O balanceamento de carga oferece um nível mais elevado de disponibilidade e dimensionamento ao propagar os pedidos recebidos por várias máquinas virtuais. Pode utilizar o Portal do Azure para criar um balanceador de carga que faça o balanceamento de carga das máquinas virtuais. Este início rápido mostra-lhe como criar recursos de rede, servidores de back-end e um Balanceador de Carga Básico público.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar. 

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no Portal do Azure em [http://portal.azure.com](http://portal.azure.com).

## <a name="create-a-basic-load-balancer"></a>Criar um Balanceador de Carga Básico

Nesta secção, irá utilizar o portal para criar um Balanceador de Carga Básico público. O endereço IP público é automaticamente configurado como front-end do Balanceador de Carga, com o nome *LoadBalancerFrontend* ao criar o IP público no momento em que cria o recurso do balanceador de carga através do portal.

1. No canto superior esquerdo do ecrã, clique em **Criar um recurso** > **Rede** > **Balanceador de Carga**.
2. Na página **Criar um balanceador de carga**, introduza estes valores para o balanceador de carga:
    - *myLoadBalancer* - no nome do balanceador de carga.
    - **Público** - no tipo de balanceador de carga.
    - *myPublicIP* – o IP Público que tem de criar com o SKU como **Básico** e **Atribuição** definido como **Dinâmico**.
    - *myResourceGroupLB* - no nome do novo grupo de recursos que criar.
3. Clique em **Criar** para criar o balanceador de carga.
   
    ![Criar um balanceador de carga](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)


## <a name="create-backend-servers"></a>Criar servidores de back-end

Nesta secção, irá criar uma rede virtual, duas máquinas virtuais para o conjunto de back-end do seu Balanceador de Carga Básico e, em seguida, instalar o IIS nas máquinas virtuais para o ajudar a testar o balanceador de carga.

### <a name="create-a-virtual-network"></a>Criar uma rede virtual
1. No canto superior esquerdo do ecrã, clique em **Novo** > **Redes** > **Rede virtual** e introduza estes valores para a rede virtual:
    - *myVnet* - no nome da rede virtual.
    - *myResourceGroupLB* - no nome do grupo de recursos existente
    - *myBackendSubnet* - no nome da sub-rede.
2. Clique em **Criar** para criar a rede virtual.

    ![Criar uma rede virtual](./media/load-balancer-get-started-internet-portal/2-load-balancer-virtual-network.png)

### <a name="create-virtual-machines"></a>Criar máquinas virtuais

1. No canto superior esquerdo do ecrã, clique em **Novo** > **Computação** > **Windows Server 2016 Datacenter** e introduza estes valores para a máquina virtual:
    - *myVM1* - no nome da máquina virtual.        
    - *azureuser* - no nome de utilizador do administrador. -    
    - *myResourceGroupLB* - para o **Grupo de recursos**, selecione **Utilizar existente** e, em seguida, selecione *myResourceGroupLB*.
2. Clique em **OK**.
3. Selecione **DS1_V2** para o tamanho da máquina virtual e clique em **Selecionar**.
4. Introduza estes valores para as definições de VM:
    - *myAvailabilitySet* - no nome do novo Conjunto de disponibilidade que criar.
    -  *myVNet* - certifique-se de que está selecionado como a rede virtual.
    - *myBackendSubnet* - certifique-se de que está selecionado como sub-rede.
    - *myVM1-ip* - no endereço IP Público.
    - *myNetworkSecurityGroup* - no nome do novo grupo de segurança de rede (firewall) que tem de criar.
5. Clique em **Desativado** para desativar o diagnóstico de arranque.
6. Clique em **OK**, reveja as definições na página de resumo e, em seguida, clique em **Criar**.
7. Com os passos 1-6, crie uma segunda VM, com o nome *VM2* com *myAvailabilityset* como Conjunto de disponibilidade, *myVnet* como rede virtual, *myBackendSubnet* como sub-rede e *myNetworkSecurityGroup* como respetivo Grupo de Segurança de Rede. 

### <a name="create-nsg-rules"></a>Criar regras de NSG

Nesta secção, vai criar regras de NSG para permitir ligações de entrada através de HTTP e RDP.

1. Clique em **Todos os recursos** no menu da esquerda e, em seguida, na lista de recursos, clique em **myNetworkSecurityGroup**, que se encontra no grupo de recursos **myResourceGroupLB**.
2. Em **Definições**, clique em **Regras de segurança de entrada** e, em seguida, clique em**Adicionar**.
3. Introduza estes valores para a regra de segurança de entrada com o nome *myHTTPRule* para permitir ligações HTTP de entrada através da porta 80:
    - *Service Tag* - na **Origem**.
    - *Internet* - na **Etiqueta do serviço de origem**
    - *80* - nos **Intervalos de portas de destino**
    - *TCP* - no **Protocolo**
    - *Allow* - na **Ação**
    - *100* - na **Prioridade**
    - *myHTTPRule* - no nome
    - *Allow HTTP* - na descrição
4. Clique em **OK**.
 
 ![Criar uma rede virtual](./media/load-balancer-get-started-internet-portal/8-load-balancer-nsg-rules.png)
5. Repita os passos 2 a 4 para criar outra regra com o nome *myRDPRule* para permitir uma ligação RDP de entrada através da porta 3389 com os seguintes valores:
    - *Service Tag* - na **Origem**.
    - *Internet* - na **Etiqueta do serviço de origem**
    - *3389* - nos **Intervalos de portas de destino**
    - *TCP* - no **Protocolo**
    - *Allow* - na **Ação**
    - *200* - na **Prioridade**
    - *myRDPRule* - no nome
    - *Allow RDP* - na descrição

   

### <a name="install-iis"></a>Instalar o IIS

1. Clique em **Todos os recursos**, no menu da esquerda e, na lista de recursos, clique em **myVM1**, que se encontra no grupo de recursos *myResourceGroupLB*.
2. Na página **Descrição geral**, clique em **Ligar** para estabelecer o RDP para a VM.
3. Inicie sessão na VM com o nome de utilizador *azureuser* e a palavra-passe *Azure123456!*
4. No ambiente de trabalho do servidor, navegue para **Ferramentas Administrativas do Windows**>**Gestor de Servidor**.
5. No Gestor de Servidor, clique em Gerir e, em seguida, em **Adicionar funções e funcionalidades**.
 ![Adicionar a função de gestor de servidor](./media/load-balancer-get-started-internet-portal/servermanager.png)
6. No **Assistente Adicionar Funções e Funcionalidades**, utilize os seguintes valores:
    - Na página **Selecionar tipo de instalação**, clique em **Instalação baseada em funções ou baseada em funcionalidades**.
    - Na página **Selecionar servidor de destino**, clique em **myVM1**
    - Na página **Selecionar função de servidor**, clique em **Servidor Web (IIS)**
    - Siga as instruções para concluir o resto do assistente 
7. Repita os passos 1 a 6 para a máquina virtual *myVM2*.

## <a name="create-basic-load-balancer-resources"></a>Criar recursos do Balanceador de Carga Básico

Nesta secção, vai configurar as definições de balanceador de carga de um conjunto de endereços de back-end e de uma sonda de estado de funcionamento, bem como especificar o balanceador de carga e regras NAT.


### <a name="create-a-backend-address-pool"></a>Criar um conjunto de endereços de back-end

Para distribuir o tráfego pelas VMs, um conjunto de endereços de back-end contém os endereços IP das placas virtuais (NICs) ligadas ao balanceador de carga. Crie o conjunto de endereços de back-end *myBackendPool* de forma a incluir *VM1* e *VM2*.

1. Clique em **Todos os recursos**, no menu da esquerda, e clique em **myLoadBalancer** na lista de recursos.
2. Em **Definições**, clique em **Conjuntos de back-end** e, em seguida, clique em **Adicionar**.
3. Na página **Adicionar um conjunto de back-end**, faça o seguinte:
    - No nome, escreva *myBackEndPool como o nome do conjunto de back-end.
    - Em **Associado a**, no menu pendente, clique em **Conjunto de disponibilidade**
    - Em **Conjunto de disponibilidade**, clique em **myAvailabilitySet**.
    - Clique em **Adicionar uma configuração de IP de rede de destino** para adicionar cada máquina virtual (*myVM1* & *myVM2*) que criou para o conjunto de back-end.
    - Clique em **OK**.

    ![Adicionar ao conjunto de endereços back-end - ](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Confirme que a definição do conjunto de back-end do balanceador de carga apresenta ambas as VMs, **VM1** e **VM2**.

### <a name="create-a-health-probe"></a>Criar uma sonda de estado de funcionamento

Para permitir ao Balanceador de Carga Básico monitorizar o estado da aplicação, pode utilizar uma sonda de estado de funcionamento. A sonda de estado de funcionamento adiciona ou remove dinamicamente VMs da rotação do balanceador de carga com base na respetiva resposta às verificações de estado de funcionamento. Crie uma sonda de estado de funcionamento, *myHealthProbe*, para monitorizar o estado de funcionamento das VMs.

1. Clique em **Todos os recursos**, no menu da esquerda, e clique em **myLoadBalancer** na lista de recursos.
2. Em **Definições**, clique em **Sondas de estado de funcionamento** e, em seguida, clique em **Adicionar**.
3. Utilize estes valores para criar a sonda de estado de funcionamento:
    - *myHealthProbe* - no nome da sonda de estado de funcionamento.
    - **HTTP** - no tipo de protocolo.
    - *80* - no número da porta.
    - *15* - no número do **Intervalo**, em segundos, entre tentativas da sonda.
    - *2* - no número do **Limiar de mau estado de funcionamento** ou das falhas consecutivas da sonda que têm de ocorrer para que uma VM seja considerada como estando em mau estado de funcionamento.
4. Clique em **OK**.

   ![Adicionar uma sonda](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Crie uma regra de Balanceador de Carga

Pode utilizar uma regra de Balanceador de Carga para definir a forma como o tráfego é distribuído pelas VMs. Pode definir a configuração de IP de front-end do tráfego de entrada e o conjunto de IPs de back-end para receber o tráfego, juntamente com a porta de origem e de destino necessárias. Crie uma regra de Balanceador de Carga *myLoadBalancerRuleWeb* para escutar a porta 80 no front-end *LoadBalancerFrontEnd* e enviar o tráfego de rede com balanceamento de carga para o conjunto de endereços de back-end *myBackEndPool* também através da porta 80. 

1. Clique em **Todos os recursos**, no menu da esquerda, e clique em **myLoadBalancer** na lista de recursos.
2. Em **Definições**, clique em **Regras de balanceamento de carga** e, em seguida, clique em **Adicionar**.
3. Utilize estes valores para configurar a regra de balanceamento de carga:
    - *myHTTPRule* - no nome da regra de balanceamento de carga.
    - **TCP** - no tipo de protocolo.
    - *80* - no número da porta.
    - *80* - na porta de back-end.
    - *myBackendPool* - no nome do conjunto de back-end.
    - *myHealthProbe* - no nome da sonda de estado de funcionamento.
4. Clique em **OK**.
    
    ![Adicionar uma regra de balanceamento de carga](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

## <a name="test-the-load-balancer"></a>Testar o balanceador de carga
1. Localize o endereço IP público do Balanceador de Carga no ecrã **Descrição geral**. Clique em **Todos os recursos** e, em seguida, clique em **myPublicIP**.

2. Copie o endereço IP público e cole-o na barra de endereço do browser. A página predefinida do servidor Web do IIS é apresentada no browser.

  ![Servidor Web do IIS](./media/load-balancer-get-started-internet-portal/9-load-balancer-test.png)

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine o grupo de recursos, o balanceador de carga e todos os recursos relacionados. Para tal, selecione o grupo de recursos que contém o balanceador de carga e clique em **Eliminar**.

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, criou um grupo de recursos, recursos de rede e servidores de back-end. Em seguida, utilizou esses recursos para criar um balanceador de carga. Para saber mais sobre balanceadores de carga e recursos associados, avance para os artigos de tutorial.
