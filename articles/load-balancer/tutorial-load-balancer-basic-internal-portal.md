---
title: 'Tutorial: criar um Balanceador de Carga Básico público – Portal do Azure | Microsoft Docs'
description: Este tutorial mostra-lhe como criar um Balanceador de Carga Básico interno com o Portal do Azure.
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: As an IT administrator, I want to create a load balancer that load balances incoming internet traffic to virtual machines within a specific zone in a region.
ms.assetid: aa9d26ca-3d8a-4a99-83b7-c410dd20b9d0
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: a16e9ad5b72d87614f5d3630e24e6aa36def8c51
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
ms.locfileid: "32779584"
---
# <a name="tutorial-load-balance-internal-traffic-with-basic-load-balancer-to-vms-using-the-azure-portal"></a>Tutorial: balancear carga de tráfego interno com Balanceador de Carga Básico para VMs com o Portal do Azure

O balanceamento de carga oferece um nível mais elevado de disponibilidade e dimensionamento ao propagar os pedidos recebidos por várias máquinas virtuais. Pode utilizar o Portal do Azure para balancear a carga de tráfego interno para máquinas virtuais com um Balanceador de Carga Básico. Este tutorial mostra-lhe como criar recursos de rede, servidores de back-end e um Balanceador de Carga Básico interno.

Se preferir, pode concluir este tutorial com a [CLI do Azure](load-balancer-get-started-ilb-arm-cli.md) ou o [Azure PowerShell](load-balancer-get-started-ilb-arm-ps.md).

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar. 

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-virtual-network"></a>Criar uma rede virtual
1. No canto superior esquerdo do ecrã, clique em **Novo** > **Redes** > **Rede virtual** e introduza estes valores para a rede virtual:
    - *myVnet* – no nome da rede virtual.
    - *myResourceGroupILB* – no nome do grupo de recursos existente
    - *myBackendSubnet* – no nome da sub-rede.
2. Clique em **Criar** para criar a rede virtual.

## <a name="create-a-basic-load-balancer"></a>Criar um Balanceador de Carga Básico
Crie um Balanceador de Carga Básico interno com o portal.

1. No canto superior esquerdo do ecrã, clique em **Criar um recurso** > **Rede** > **Balanceador de Carga**.
2. Na página **Criar um balanceador de carga**, introduza estes valores para o balanceador de carga:
    - *myLoadBalancer* – no nome do balanceador de carga.
    - **Interno** – no tipo de balanceador de carga.
    - **Básico** – na versão de SKU.
    - **10.1.0.7** – o endereço IP privado estático.
    - *myVNet* – na rede virtual que escolheu a partir da lista de redes existentes.
    - *mySubnet* – na sub-rede que escolheu a partir da lista de sub-redes existentes.
    - *myResourceGroupILB* – no nome do novo grupo de recursos que criar.
3. Clique em **Criar** para criar o balanceador de carga.
   
    ![Criar um balanceador de carga](./media/tutorial-load-balancer-basic-internal-portal/1-load-balancer.png)


## <a name="create-backend-servers"></a>Criar servidores de back-end

Nesta secção, irá criar duas máquinas virtuais para o conjunto de back-end do seu Balanceador de Carga Básico e, em seguida, instalar o IIS nas máquinas virtuais para o ajudar a testar o balanceador de carga.

### <a name="create-virtual-machines"></a>Criar máquinas virtuais

1. No canto superior esquerdo do ecrã, clique em **Criar um recurso** > **Computação** > **Windows Server 2016 Datacenter** e introduza estes valores para a máquina virtual:
    - *myVM1* – no nome da máquina virtual.        
    - *azureuser* – no nome de utilizador do administrador.   
    - *myResourceGroupILB* – no **Grupo de recursos**, selecione **Utilizar existente** e, em seguida, selecione *myResourceGroupILB*.
2. Clique em **OK**.
3. Selecione **DS1_V2** para o tamanho da máquina virtual e clique em **Selecionar**.
4. Introduza estes valores para as definições de VM:
    - *myAvailabilitySet* – no nome do novo Conjunto de disponibilidade que criar.
    -  *myVNet* – certifique-se de que está selecionado como a rede virtual.
    - *myBackendSubnet* – certifique-se de que está selecionado como a sub-rede.
    - *myNetworkSecurityGroup* - no nome do novo grupo de segurança de rede (firewall) que tem de criar.
5. Clique em **Desativado** para desativar o diagnóstico de arranque.
6. Clique em **OK**, reveja as definições na página de resumo e, em seguida, clique em **Criar**.
7. Com os passos 1-6, crie uma segunda VM, com o nome *VM2* com *myAvailabilityset* como Conjunto de disponibilidade, *myVnet* como rede virtual, *myBackendSubnet* como sub-rede e *myNetworkSecurityGroup* como respetivo Grupo de Segurança de Rede. 

### <a name="install-iis-and-customize-the-default-web-page"></a>Instalar o IIS e personalizar a página Web predefinida

1. Clique em **Todos os recursos**, no menu esquerdo e, na lista de recursos, clique em **myVM1**, que se encontra no grupo de recursos *myResourceGroupILB*.
2. Na página **Descrição geral**, clique em **Ligar** para estabelecer o RDP para a VM.
3. Inicie sessão na VM.
4. No ambiente de trabalho do servidor, navegue para **Ferramentas Administrativas do Windows**>**Gestor de Servidor**.
5. Inicie o Windows PowerShell na VM1 e utilize os seguintes comandos para instalar o servidor de ISS e atualizar o ficheiro html predefinido.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```
5. Feche a ligação RDP com *myVM1*.
6. Repita os passos 1 a 5 com *myVM2* para instalar o IIS e personalizar a página Web predefinida.

## <a name="create-nsg-rules"></a>Criar regras de NSG

Nesta secção, vai criar regras de NSG para permitir ligações de entrada através de HTTP e RDP.

1. Clique em **Todos os recursos** no menu da esquerda e, em seguida, na lista de recursos, clique em **myNetworkSecurityGroup**, que se encontra no grupo de recursos **myResourceGroupLB**.
2. Em **Definições**, clique em **Regras de segurança de entrada** e, em seguida, clique em**Adicionar**.
3. Introduza estes valores para a regra de segurança de entrada com o nome *myHTTPRule* para permitir ligações HTTP de entrada através da porta 80:
    - *Service Tag* – na **Origem**.
    - *Internet* – na **Etiqueta do serviço de origem**
    - *80* – nos **Intervalos de portas de destino**
    - *TCP* – no **Protocolo**
    - *Allow* – na **Ação**
    - *100* – na **Prioridade**
    - *myHTTPRule* – no nome
    - *Allow HTTP* – na descrição
4. Clique em **OK**.
 
5. Repita os passos 2 a 4 para criar outra regra com o nome *myRDPRule* para permitir uma ligação RDP de entrada através da porta 3389 com os seguintes valores:
    - *Service Tag* – na **Origem**.
    - *Internet* – na **Etiqueta do serviço de origem**
    - *3389* – nos **Intervalos de portas de destino**
    - *TCP* – no **Protocolo**
    - *Allow* – na **Ação**
    - *200* – na **Prioridade**
    - *myRDPRule* – no nome
    - *Allow RDP* – na descrição

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

        ![Adicionar ao conjunto de endereços back-end - ](./media/tutorial-load-balancer-basic-internal-portal/3-load-balancer-backend-02.png)

3. Confirme que a definição do conjunto de back-end do balanceador de carga apresenta ambas as VMs, **VM1** e **VM2**.

### <a name="create-a-health-probe"></a>Criar uma sonda de estado de funcionamento

Para permitir ao Balanceador de Carga Básico monitorizar o estado da aplicação, pode utilizar uma sonda de estado de funcionamento. A sonda de estado de funcionamento adiciona ou remove dinamicamente VMs da rotação do balanceador de carga com base na respetiva resposta às verificações de estado de funcionamento. Crie uma sonda de estado de funcionamento, *myHealthProbe*, para monitorizar o estado de funcionamento das VMs.

1. Clique em **Todos os recursos**, no menu da esquerda, e clique em **myLoadBalancer** na lista de recursos.
2. Em **Definições**, clique em **Sondas de estado de funcionamento** e, em seguida, clique em **Adicionar**.
3. Utilize estes valores para criar a sonda de estado de funcionamento:
    - *myHealthProbe* – no nome da sonda de estado de funcionamento.
    - **HTTP** – no tipo de protocolo.
    - *80* – no número da porta.
    - *15* – no número do **Intervalo**, em segundos, entre tentativas da sonda.
    - *2* – no número do **Limiar de mau estado de funcionamento** ou das falhas consecutivas da sonda que têm de ocorrer para que uma VM seja considerada como estando em mau estado de funcionamento.
4. Clique em **OK**.

   ![Adicionar uma sonda](./media/tutorial-load-balancer-basic-internal-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Crie uma regra de Balanceador de Carga

É utilizada uma regra de balanceador de carga para definir a forma como o tráfego é distribuído pelas VMs. Pode definir a configuração de IP de front-end do tráfego de entrada e o conjunto de IPs de back-end para receber o tráfego, juntamente com a porta de origem e de destino necessárias. Crie uma regra de Balanceador de Carga *myLoadBalancerRuleWeb* para escutar a porta 80 no front-end *LoadBalancerFrontEnd* e enviar o tráfego de rede com balanceamento de carga para o conjunto de endereços de back-end *myBackEndPool* também através da porta 80. 

1. Clique em **Todos os recursos**, no menu da esquerda, e clique em **myLoadBalancer** na lista de recursos.
2. Em **Definições**, clique em **Regras de balanceamento de carga** e, em seguida, clique em **Adicionar**.
3. Utilize estes valores para configurar a regra de balanceamento de carga:
    - *myHTTPRule* – no nome da regra de balanceamento de carga.
    - **TCP** – no tipo de protocolo.
    - *80* – no número da porta.
    - *80* – na porta de back-end.
    - *myBackendPool* – no nome do conjunto de back-end.
    - *myHealthProbe* – no nome da sonda de estado de funcionamento.
4. Clique em **OK**.
    
    ![Adicionar uma regra de balanceamento de carga](./media/tutorial-load-balancer-basic-internal-portal/5-load-balancing-rules.png)

## <a name="create-a-virtual-machine-to-test-the-load-balancer"></a>Criar uma máquina virtual para testar o balanceador de carga
Para testar o balanceador de carga interno, tem de criar uma máquina virtual que está localizada na mesma rede virtual que as VMs de servidor de back-end.
1. No canto superior esquerdo do ecrã, clique em **Criar um recurso** > **Computação** > **Windows Server 2016 Datacenter** e introduza estes valores para a máquina virtual:
    - *myVMTest* – no nome da máquina virtual.        
    - *myResourceGroupILB* – no **Grupo de recursos**, selecione **Utilizar existente** e, em seguida, selecione *myResourceGroupILB*.
2. Clique em **OK**.
3. Selecione **DS1_V2** para o tamanho da máquina virtual e clique em **Selecionar**.
4. Introduza estes valores para as definições de VM:
    -  *myVNet* – certifique-se de que está selecionado como a rede virtual.
    - *myBackendSubnet* – certifique-se de que está selecionado como a sub-rede.
5. Clique em **Desativado** para desativar o diagnóstico de arranque.
6. Clique em **OK**, reveja as definições na página de resumo e, em seguida, clique em **Criar**.

## <a name="test-the-load-balancer"></a>Testar o balanceador de carga
1. No portal do Azure, obtenha o endereço IP privado do Balanceador de Carga no ecrã **Descrição geral**. Para tal: a. Clique em **Todos os recursos**, no menu da esquerda, e clique em **myLoadBalancer** na lista de recursos.
    b. Na página de detalhes **Descrição geral**, copie o endereço IP privado (neste exemplo, é 10.1.0.7).

2. Crie uma ligação remota para *myVMTest* da seguinte forma: a. Clique em **Todos os recursos**, no menu esquerdo e, na lista de recursos, clique em **myVMTest**, que se encontra no grupo de recursos *myResourceGroupILB*.
2. Na página **Descrição geral**, clique em **Ligar** para iniciar uma sessão remota com a VM.
3. Inicie sessão em *myVMTest*.
3. Cole o endereço IP privado na barra de endereços do browser em *myVMTest*. A página predefinida do servidor Web do IIS é apresentada no browser.

      ![Servidor Web do IIS](./media/tutorial-load-balancer-basic-internal-portal/9-load-balancer-test.png)

Para ver o balanceador de carga a distribuir tráfego pelas VMs que estão a executar a aplicação, pode forçar a atualização do browser.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine o grupo de recursos, o balanceador de carga e todos os recursos relacionados. Para tal, selecione o grupo de recursos que contém o balanceador de carga e clique em **Eliminar**.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou um grupo de recursos, recursos de rede e servidores de back-end. Utilizou esses recursos para criar um balanceador de carga interno para balancear a carga de tráfego interno para VMs. Em seguida, saiba como [balancear a carga de VMs por zonas de disponibilidade](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
