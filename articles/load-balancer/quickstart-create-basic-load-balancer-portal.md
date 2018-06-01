---
title: 'Guia de Início Rápido: Criar um balanceador de carga Básico público com o Portal do Azure | Microsoft Docs'
description: Este guia de início rápido mostra como criar um balanceador de carga Básico público com o portal do Azure.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to create a Basic Load balancer so that I can load balance internet traffic to VMs.
ms.assetid: aa9d26ca-3d8a-4a99-83b7-c410dd20b9d0
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 49fa4cf9b24c432b0956f930a1429e1cdf827f1b
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/18/2018
ms.locfileid: "34304883"
---
# <a name="quickstart-create-a-public-basic-load-balancer-by-using-the-azure-portal"></a>Guia de Início Rápido: Criar um balanceador de carga Básico público com o portal do Azure

O balanceamento de carga oferece um nível mais elevado de disponibilidade e dimensionamento ao propagar os pedidos recebidos por múltiplas máquinas virtuais (VMs). Pode utilizar o portal do Azure para criar um balanceador de carga que faça o balanceamento de carga das máquinas virtuais. Este guia de início rápido mostra-lhe como criar recursos de rede, servidores de back-end e um balanceador de carga no escalão de preço Básico.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar. 

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Para todas as tarefas neste guia de início rápido, inicie sessão no [portal do Azure](http://portal.azure.com).

## <a name="create-a-basic-load-balancer"></a>Criar um balanceador de carga Básico

Nesta secção, irá utilizar o portal para criar um balanceador de carga Básico público. O endereço IP público é automaticamente configurado como front-end do balanceador de carga, ao criar o IP público e o recurso do balanceador de carga através do portal. O nome do front-end é **LoadBalancerFrontend**.

1. No canto superior esquerdo do portal, selecione **Criar um recurso** > **Rede** > **Balanceador de Carga**.
2. No painel **Criar balanceador de carga**, introduza estes valores:
   - **myLoadBalancer** no nome do balanceador de carga
   - **Público** no tipo de balanceador de carga 
   - **myPublicIP** no IP público que tem de criar, com o **SKU** definido como **Básico** e a **Atribuição** definida como **Dinâmica**
   - **myResourceGroupLB** no nome do novo grupo de recursos
3. Selecione **Criar**.
   
![Criar um balanceador de carga](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)


## <a name="create-back-end-servers"></a>Criar servidores de back-end

Nesta secção, irá criar uma rede virtual e duas máquinas virtuais para o conjunto de back-end do seu balanceador de carga Básico. Em seguida, irá instalar os Serviços de Informação Internet (IIS) nas máquinas virtuais para ajudar a testar o balanceador de carga.

### <a name="create-a-virtual-network"></a>Criar uma rede virtual
1. No canto superior esquerdo do portal, selecione **Novo** > **Rede** > **Rede virtual**.
2. No painel **Criar rede virtual**, introduza estes valores e, em seguida, selecione **Criar**:
   - **myVnet** no nome da rede virtual
   - **myResourceGroupLB** no nome do grupo de recursos existente
   - **myBackendSubnet** no nome da sub-rede

   ![Criar uma rede virtual](./media/load-balancer-get-started-internet-portal/2-load-balancer-virtual-network.png)

### <a name="create-virtual-machines"></a>Criar máquinas virtuais

1. No canto superior esquerdo do portal, selecione **Novo** > **Computação** > **Windows Server 2016 Datacenter**. 
2. Introduza estes valores para a máquina virtual e, em seguida, selecione **OK**:
   - **myVM1** no nome da máquina virtual.        
   - **azureuser** no nome de utilizador do administrador.    
   - **myResourceGroupLB** no grupo de recursos. (Em **Grupo de recursos**, selecione **Utilizar existente** e, em seguida, selecione **myResourceGroupLB**.)   
3. Selecione **DS1_V2** para o tamanho da máquina virtual e clique em **Selecionar**.
4. Introduza estes valores para as definições de VM:
   - **myAvailabilitySet** no nome do novo conjunto de disponibilidade que criar.
   - **myVNet** no nome da rede virtual. (Certifique-se de que está selecionado.)
   - **myBackendSubnet** no nome da sub-rede. (Certifique-se de que está selecionado.)
   - **myVM1-ip** no endereço IP público.
   - **myNetworkSecurityGroup** no nome do novo grupo de segurança de rede (NSG, um tipo de firewall) que tem de criar.
5. Selecione **Desativado** para desativar o diagnóstico de arranque.
6. Selecione **OK**, reveja as definições na página de resumo e, em seguida, selecione **Criar**.
7. Ao utilizar os passos 1 a 6, crie uma segunda VM com o nome **VM2**, com:
   - **myAvailabilityset** como conjunto de disponibilidade.
   - **myVnet** como rede virtual.
   - **myBackendSubnet** como sub-rede.
   - **myNetworkSecurityGroup** como grupo de segurança de rede. 

### <a name="create-nsg-rules"></a>Criar regras de NSG

Nesta secção, irá criar regras de NSG para permitir ligações de entrada que utilizam HTTP e RDP.

1. Selecione **Todos os recursos** no menu da esquerda. Na lista de recursos, selecione **myNetworkSecurityGroup** no grupo de recursos **myResourceGroupLB**.
2. Em **Definições**, selecione **Regras de segurança de entrada** e, em seguida, selecione **Adicionar**.
3. Introduza os seguintes valores para a regra de segurança de entrada com o nome **myHTTPRule** para permitir ligações HTTP de entrada que utilizem a porta 80. Em seguida, selecione **OK**.
   - **Service Tag** na **Origem**
   - **Internet** na **Etiqueta do serviço de origem**
   - **80** – nos **Intervalos de portas de destino**
   - **TCP** – no **Protocolo**
   - **Allow** na **Ação**
   - **100** - na **Prioridade**
   - **myHTTPRule** – no **Nome**
   - **Allow HTTP** na **Descrição**
 
   ![Criar uma regra NSG](./media/load-balancer-get-started-internet-portal/8-load-balancer-nsg-rules.png)
4. Repita os passos 2 e 3 para criar outra regra com o nome **myRDPRule** para permitir uma ligação RDP de entrada através da porta 3389. Utilize os seguintes valores:
   - **Service Tag** na **Origem**
   - **Internet** na **Etiqueta do serviço de origem**
   - **3389** – nos **Intervalos de portas de destino**
   - **TCP** – no **Protocolo**
   - **Allow** na **Ação**
   - **200** - na **Prioridade**
   - **myRDPRule** – no **Nome**
   - **Allow RDP** na **Descrição**

   

### <a name="install-iis"></a>Instalar o IIS

1. Selecione **Todos os recursos** no menu da esquerda. A partir da lista de recursos, selecione **myVM1** no grupo de recursos **myResourceGroupLB**.
2. Na página **Descrição geral**, selecione **Ligar** para estabelecer o RDP para a VM.
3. Inicie sessão na VM com o nome de utilizador **azureuser** e a palavra-passe **Azure123456!**.
4. No ambiente de trabalho do servidor, navegue para **Ferramentas Administrativas do Windows** > **Gestor de Servidor**.
5. No Gestor de Servidor, selecione **Gerir** e, em seguida, selecione **Adicionar funções e funcionalidades**.
   ![Adicionar a função de gestor de servidor](./media/load-balancer-get-started-internet-portal/servermanager.png)
6. No Assistente Adicionar Funções e Funcionalidades, utilize os seguintes valores:
   - Na página **Selecionar tipo de instalação**, selecione **Instalação baseada em funções ou baseada em funcionalidades**.
   - Na página **Selecionar servidor de destino**, selecione **myVM1**.
   - Na página **Selecionar função de servidor**, selecione **Servidor Web (IIS)**.
   - Siga as instruções para concluir o resto do assistente. 
7. Repita os passos 1 a 6 para a máquina virtual **myVM2**.

## <a name="create-resources-for-the-basic-load-balancer"></a>Criar recursos para o balanceador de carga Básico

Nesta secção, irá configurar as definições de balanceador de carga de um conjunto de endereços de back-end e de uma sonda de estado de funcionamento. Tem também de especificar o balanceador de carga e regras de NAT.


### <a name="create-a-back-end-address-pool"></a>Criar um conjunto de endereços de back-end

Para distribuir o tráfego pelas VMs, um conjunto de endereços de back-end contém os endereços IP das placas virtuais (NICs) que estão ligadas ao balanceador de carga. Crie o conjunto de endereços de back-end **myBackendPool** de forma a incluir **VM1** e **VM2**.

1. Selecione **Todos os recursos** no menu da esquerda e, em seguida, selecione **myLoadBalancer** na lista de recursos.
2. Em **Definições**, selecione **Conjuntos de back-end** e, em seguida, selecione **Adicionar**.
3. Na página **Adicionar um conjunto de back-end**, faça o seguinte e, em seguida, selecione **OK**:
   - Para **Nome**, introduza **myBackEndPool**.
   - Em **Associado a**, no menu pendente, selecione **Conjunto de disponibilidade**.
   - Em **Conjunto de disponibilidade**, selecione **myAvailabilitySet**.
   - Selecione **Adicionar uma configuração de IP de rede de destino** para adicionar cada máquina virtual (**myVM1** e **myVM2**) que criou para o conjunto de back-end.

   ![Adicionar ao conjunto de endereços de back-end](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Certifique-se de que a definição do conjunto de back-end do balanceador de carga apresenta ambas as VMs, **VM1** e **VM2**.

### <a name="create-a-health-probe"></a>Criar uma sonda de estado de funcionamento

Para permitir ao balanceador de carga Básico monitorizar o estado da aplicação, pode utilizar uma sonda de estado de funcionamento. A sonda de estado de funcionamento adiciona ou remove dinamicamente VMs da rotação do balanceador de carga com base na respetiva resposta às verificações de estado de funcionamento. Crie uma sonda de estado de funcionamento com o nome **myHealthProbe**, para monitorizar o estado de funcionamento das VMs.

1. Selecione **Todos os recursos** no menu da esquerda e, em seguida, selecione **myLoadBalancer** na lista de recursos.
2. Em **Definições**, selecione **Sondas de estado de funcionamento** e, em seguida, selecione **Adicionar**.
3. Utilize estes valores e, em seguida, selecione **OK**:
   - **myHealthProbe** no nome da sonda de estado de funcionamento
   - **HTTP** no tipo de protocolo
   - **80** no número da porta
   - **15** no **Intervalo**, o número de segundos entre tentativas da sonda
   - **2** no **Limiar de mau estado de funcionamento**, o número de falhas consecutivas da sonda que têm de ocorrer para que uma VM seja considerada como estando em mau estado de funcionamento

   ![Adicionar uma sonda](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Crie uma regra de balanceador de carga

Irá utilizar uma regra de balanceador de carga para definir a forma como o tráfego é distribuído pelas VMs. Pode definir a configuração de IP de front-end do tráfego de entrada e o conjunto de IPs de back-end para receber o tráfego, juntamente com a porta de origem e de destino necessárias. 

Crie uma regra de balanceador de carga com o nome **myLoadBalancerRuleWeb** para a escuta da porta 80 no front-end **LoadBalancerFrontEnd**. A regra também serve para o envio de tráfego de rede com balanceamento de carga para o conjunto de endereços de back-end **myBackEndPool**, também através da porta 80. 

1. Selecione **Todos os recursos** no menu da esquerda e, em seguida, selecione **myLoadBalancer** na lista de recursos.
2. Em **Definições**, selecione **Regras de balanceamento de carga** e, em seguida, selecione **Adicionar**.
3. Utilize estes valores e, em seguida, selecione **OK**:
   - **myHTTPRule** no nome da regra do balanceador de carga
   - **TCP** – no tipo de protocolo
   - **80** no número da porta
   - **80** na porta de back-end
   - **myBackendPool** no nome do conjunto de back-end
   - **myHealthProbe** no nome da sonda de estado de funcionamento
    
   ![Adicionar uma regra de balanceador de carga](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

## <a name="test-the-load-balancer"></a>Testar o balanceador de carga
1. Localize o endereço IP público do balanceador de carga no ecrã **Descrição geral**. Selecione **Todos os recursos** e, em seguida, selecione **myPublicIP**.

2. Copie o endereço IP público e cole-o na barra de endereço do browser. A página predefinida do servidor Web do IIS é apresentada no browser.

   ![Servidor Web do IIS](./media/load-balancer-get-started-internet-portal/9-load-balancer-test.png)

## <a name="clean-up-resources"></a>Limpar recursos

Pode eliminar o grupo de recursos, o balanceador de carga e todos os recursos relacionados, quando já não forem necessários. Selecione o grupo de recursos que contém o balanceador de carga e selecione **Eliminar**.

## <a name="next-steps"></a>Passos seguintes

Neste guia de início rápido, criou um grupo de recursos, recursos de rede e servidores de back-end. Em seguida, utilizou esses recursos para criar um Balanceador de Carga Básico. Para saber mais sobre o Balanceador de Carga do Azure, avance para os tutoriais do Balanceador de Carga do Azure.

> [!div class="nextstepaction"]
> [Tutoriais do Balanceador de Carga do Azure](tutorial-load-balancer-basic-internal-portal.md)
