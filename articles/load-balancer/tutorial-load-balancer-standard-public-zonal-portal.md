---
title: 'Tutorial: VMs de Balanceador de Carga dentro de uma zona – Portal do Azure | Microsoft Docs'
description: Este tutorial demonstra como criar um Balanceador de Carga Standard com front-end zonal para VMs de balanceamento de carga dentro de uma zona de disponibilidade através do portal do Azure
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: As an IT administrator, I want to create a load balancer that load balances incoming internet traffic to virtual machines within a specific zone in a region.
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/20/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 9067ea350997ed0c4fc5c65dccb72f403adfa774
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="tutorialload-balance-vms-within-an-availability-zone-with-a-standard-load-balancer-using-the-azure-portal"></a>Tutorial: VMs de balanceamento de carga dentro de uma zona de disponibilidade com um Balanceador de Carga Standard através do portal do Azure

Este tutorial passa pela criação de um [Balanceador de Carga Standard](https://aka.ms/azureloadbalancerstandard) público com um front-end zonal que utiliza um endereço IP Standard Público através do portal do Azure. Neste cenário, especifique uma zona específica para as instâncias de front-end e back-end, para alinhar o caminho de dados e os recursos com uma zona específica. Saiba como:

> [!div class="checklist"]
> * Criar um Balanceador de Carga Standard do Azure com um front-end zonal
> * Criar grupos de segurança de rede para definir regras de tráfego de entrada
> * Criar VMs zonais e anexar a um balanceador de carga
> * Criar uma sonda de estado de funcionamento do balanceador de carga
> * Criar regras de tráfego do balanceador de carga
> * Criar um site do IIS básico
> * Ver um balanceador de carga em ação

Para obter mais informações sobre a utilização das Zonas de disponibilidade com o Balanceador de Carga Standard, veja [Balanceador de Carga Standard e Zonas de Disponibilidade](load-balancer-standard-availability-zones.md).

Se preferir, pode concluir este tutorial com a [CLI do Azure](load-balancer-standard-public-zonal-cli.md).

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em [http://portal.azure.com](http://portal.azure.com).

## <a name="create-a-public-standard-load-balancer"></a>Criar um Balanceador de Carga Standard público

O Balanceador de Carga Standard só suporta endereços IP Públicos Standard. Ao criar um novo IP público durante a criação do balanceador de carga, é automaticamente configurado como uma versão de SKU Standard e tem também redundância de zona automaticamente.

1. No canto superior esquerdo do ecrã, clique em **Criar um recurso** > **Rede** > **Balanceador de Carga**.
2. Na página **Criar um balanceador de carga**, introduza estes valores para o balanceador de carga:
    - *myLoadBalancer* – no nome do balanceador de carga.
    - **Público** – no tipo de balanceador de carga.
     - *myPublicIPZonal* – no endereço IP Público novo que criar. Para tal, clique em **Selecionar um endereço IP público** e, em seguida, clique em **Criar novo**. Para o tipo de nome *myPublicIP*, o SKU é Standard por predefinição e seleciona **Zona 1** para a **Zona de disponibilidade**.
    - *myResourceGroupZLB* – no nome do novo grupo de recursos que criar.
    - **westeurope** – na localização.
3. Clique em **Criar** para criar o balanceador de carga.
   
    ![Criar o Balanceador de Carga Standard zonal com o portal do Azure](./media/tutorial-load-balancer-standard-zonal-portal/create-load-balancer-zonal-frontend.png)


## <a name="create-backend-servers"></a>Criar servidores de back-end

Nesta secção, irá criar uma rede virtual, duas máquinas virtuais na mesma zona (nomeadamente, zona 1) para a região para adicionar ao conjunto de back-end do balanceador de carga e, em seguida, instalar o IIS nas máquinas virtuais para o ajudar a testar o balanceador de carga com redundância de zona. Por conseguinte, se uma VM falhar, a sonda de estado de funcionamento da VM na mesma zona falha e o tráfego continua a ser servido por outra VM na mesma zona.

### <a name="create-a-virtual-network"></a>Criar uma rede virtual
1. No canto superior esquerdo do ecrã, clique em **Criar um recurso** > **Rede** > **Rede virtual** e introduza estes valores para a rede virtual:
    - *myVnet* – no nome da rede virtual.
    - *myResourceGroupZLB* – no nome do grupo de recursos existente
    - *myBackendSubnet* – no nome da sub-rede.
2. Clique em **Criar** para criar a rede virtual.

    ![Criar uma rede virtual](./media/tutorial-load-balancer-standard-zonal-portal/create-virtual-network.png)

## <a name="create-a-network-security-group"></a>Criar um grupo de segurança de rede

1. No canto superior esquerdo do ecrã, clique em **Criar um recurso**, no tipo de caixa de pesquisa *Grupo de Segurança de Rede* e, na página do grupo de segurança de rede, clique em **Criar**.
2. Na página Criar grupo de segurança de rede, introduza estes valores:
    - *myNetworkSecurityGroup* – no nome do grupo de segurança de rede.
    - *myResourceGroupLBAZ* – no nome do grupo de recursos existente.
   
    ![Criar uma rede virtual](./media/tutorial-load-balancer-standard-zonal-portal/create-network-security-group.png)

### <a name="create-nsg-rules"></a>Criar regras de NSG

Nesta secção, irá criar regras de NSG para permitir ligações de entrada através de HTTP e RDP no portal do Azure.

1. No portal do Azure, clique em **Todos os recursos**, no menu da esquerda e, em seguida, clique em **myNetworkSecurityGroup**, que se encontra no grupo de recursos **myResourceGroupZLB**.
2. Em **Definições**, clique em **Regras de segurança de entrada** e, em seguida, clique em**Adicionar**.
3. Introduza estes valores para a regra de segurança de entrada com o nome *myHTTPRule* para permitir ligações HTTP de entrada através da porta 80:
    - *Service Tag* – na **Origem**.
    - *Internet* – na **Etiqueta do serviço de origem**
    - *80* – nos **Intervalos de portas de destino**
    - *TCP* – no **Protocolo**
    - *Allow* – na **Ação**
    - *100* – na **Prioridade**
    - *myHTTPRule* – no **Nome**
    - *Allow HTTP* – na **Descrição**
4. Clique em **OK**.
 
 ![Criar uma rede virtual](./media/load-balancer-standard-public-availability-zones-portal/8-load-balancer-nsg-rules.png)

5. Repita os passos 2 a 4 para criar outra regra com o nome *myRDPRule* para permitir uma ligação RDP de entrada através da porta 3389 com os seguintes valores:
    - *Service Tag* – na **Origem**.
    - *Internet* – na **Etiqueta do serviço de origem**
    - *3389* – nos **Intervalos de portas de destino**
    - *TCP* – no **Protocolo**
    - *Allow* – na **Ação**
    - *200* – na **Prioridade**
    - *myRDPRule* – no nome
    - *Allow RDP* – na descrição

    ![Criar regra RDP](./media/tutorial-load-balancer-standard-zonal-portal/create-rdp-rule.png)

### <a name="create-virtual-machines"></a>Criar máquinas virtuais

1. No canto superior esquerdo do ecrã, clique em **Criar um recurso** > **Computação** > **Windows Server 2016 Datacenter** e introduza estes valores para a máquina virtual:
    - *myVM1* – no nome da máquina virtual.        
    - *azureuser* – no nome de utilizador do administrador.    
    - *myResourceGroupZLB* – no **Grupo de recursos**, selecione **Utilizar existente** e, em seguida, selecione *myResourceGroupZLB*.
2. Clique em **OK**.
3. Selecione **DS1_V2** para o tamanho da máquina virtual e clique em **Selecionar**.
4. Introduza estes valores para as definições de VM:
    - *zone 1* – na Zona de Disponibilidade onde colocar a VM.
    -  *myVNet* – certifique-se de que está selecionado como a rede virtual.
    - *myVM1PIP* – no Endereço IP Público Standard que criar. Clique em *Criar novo* e, em seguida, no tipo de nome, *myVM1PIP*, na Zona selecione **1**. O SKU do endereço IP é Standard por predefinição.
    - *myBackendSubnet* – certifique-se de que está selecionado como a sub-rede.
    - *myNetworkSecurityGroup* – no nome do grupo de segurança de rede (firewall) já existente.
5. Clique em **Desativado** para desativar o diagnóstico de arranque.
6. Clique em **OK**, reveja as definições na página de resumo e, em seguida, clique em **Criar**.
7. Crie uma segunda VM com o nome *myVM2* na Zona 1, com *myVnet* como rede virtual, *myVM2PIP* como endereço IP Público Standard, *myBackendSubnet* como sub-rede e **myNetworkSecurityGroup* como grupo de segurança de rede através dos passos 1 a 6.

    ![Criar regra RDP](./media/tutorial-load-balancer-standard-zonal-portal/create-virtual-machine.png) 

### <a name="install-iis-on-vms"></a>Instalar o IIS nas VMs

1. Clique em **Todos os recursos**, no menu da esquerda e, na lista de recursos, clique em **myVM1**, que se encontra no grupo de recursos *myResourceGroupZLB*.
2. Na página **Descrição geral**, clique em **Ligar** para estabelecer o RDP para a VM.
3. Inicie sessão na VM com o nome de utilizador e a palavra-passe que especificou ao criar a VM (poderá ter de selecionar **Mais opções** e **Utilizar uma conta diferente** para especificar as credenciais que introduziu quando criou a VM) e, em seguida, selecione **OK**. Poderá receber um aviso de certificado durante o processo de início de sessão. Selecione **Sim** para continuar a ligação.
4. No ambiente de trabalho do servidor, navegue para **Ferramentas Administrativas do Windows**>**Windows PowerShell**.
6. Na janela do PowerShell, execute os seguintes comandos para instalar o servidor IIS, remova o ficheiro default.htm, adicione um novo ficheiro default.htm que apresenta o nome da VM:

   ```azurepowershell-interactive
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    # remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    # Add a new htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello from" + $env:computername)
   ```
8. Fechar a sessão RDP com *myVM1*
9. Repita os passos 1 a 8 para instalar o IIS no *myVM2*.

## <a name="create-load-balancer-resources"></a>Criar recursos do balanceador de carga

Nesta secção, vai configurar as definições de balanceador de carga de um conjunto de endereços de back-end e de uma sonda de estado de funcionamento, bem como especificar o balanceador de carga e regras NAT.


### <a name="create-a-backend-address-pool"></a>Criar um conjunto de endereços de back-end

Para distribuir o tráfego pelas VMs, um conjunto de endereços de back-end contém os endereços IP das placas virtuais (NICs) ligadas ao balanceador de carga. Crie o conjunto de endereços de back-end *myBackendPool* de forma a incluir *VM1* e *VM2*.

1. Clique em **Todos os recursos**, no menu da esquerda e, em seguida, clique em **myLoadBalancer* na lista de recursos.
2. Em **Definições**, clique em **Conjuntos de back-end** e, em seguida, clique em **Adicionar**.
3. Na página **Adicionar um conjunto de back-end**, faça o seguinte:
    - No nome, escreva *myBackEndPool* como o nome do conjunto de back-end.
    - Para **Rede virtual**, no menu pendente, clique em *myVNet*
    - Para **Máquina virtual** e **Endereço IP**, adicione *myVM1* e *myVM2*, assim como os endereços IP públicos correspondentes.
4. Clique em **Adicionar**.
5. Confirme que a definição do conjunto de back-end do balanceador de carga apresenta ambas as VMs: **myVM1** e **myVM2**.
 
    ![Criar conjunto de back-end](./media/tutorial-load-balancer-standard-zonal-portal/create-backend-pool.png) 

### <a name="create-a-health-probe"></a>Criar uma sonda de estado de funcionamento

Para permitir ao balanceador de carga monitorizar o estado da aplicação, pode utilizar uma sonda de estado de funcionamento. A sonda de estado de funcionamento adiciona ou remove dinamicamente VMs da rotação do balanceador de carga com base na respetiva resposta às verificações de estado de funcionamento. Crie uma sonda de estado de funcionamento, *myHealthProbe*, para monitorizar o estado de funcionamento das VMs.

1. Clique em **Todos os recursos**, no menu da esquerda, e clique em **myLoadBalancer** na lista de recursos.
2. Em **Definições**, clique em **Sondas de estado de funcionamento** e, em seguida, clique em **Adicionar**.
3. Utilize estes valores para criar a sonda de estado de funcionamento:
    - *myHealthProbe* – no nome da sonda de estado de funcionamento.
    - **HTTP** – no tipo de protocolo.
    - *80* – no número da porta.
    - *15* – no número do **Intervalo**, em segundos, entre tentativas da sonda.
    - *2* – no número do **Limiar de mau estado de funcionamento** ou das falhas consecutivas da sonda que têm de ocorrer para que uma VM seja considerada como estando em mau estado de funcionamento.
4. Clique em **OK**.

   ![Adicionar uma sonda](./media/load-balancer-standard-public-availability-zones-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Crie uma regra de balanceador de carga

É utilizada uma regra de balanceador de carga para definir a forma como o tráfego é distribuído pelas VMs. Pode definir a configuração de IP de front-end do tráfego de entrada e o conjunto de IPs de back-end para receber o tráfego, juntamente com a porta de origem e de destino necessárias. Crie uma regra de balanceador de carga *myLoadBalancerRuleWeb* para escutar na porta 80 no front-end *FrontendLoadBalancer* e enviar o tráfego de rede com balanceamento de carga para o conjunto de endereços de back-end *myBackEndPool* também através da porta 80. 

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
    
    ![Adicionar uma regra de balanceamento de carga](./media/tutorial-load-balancer-standard-zonal-portal/load-balancing-rule.png)

## <a name="test-the-load-balancer"></a>Testar o balanceador de carga
1. Localize o endereço IP público do Balanceador de Carga no ecrã **Descrição geral**. Clique em **Todos os recursos** e, em seguida, clique em **myPublicIP**. 

2. Copie o endereço IP público e cole-o na barra de endereço do browser. A página predefinida que inclui o nome da página do servidor Web é apresentada no browser.

      ![Servidor Web do IIS](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)
3. Para ver o balanceador de carga em ação, pode forçar a paragem da VM que é apresentada e atualizar o browser para ver o nome do outro servidor apresentado no browser.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine o grupo de recursos, o balanceador de carga e todos os recursos relacionados. Para tal, selecione o grupo de recursos que contém o balanceador de carga e clique em **Eliminar**.

## <a name="next-steps"></a>Passos seguintes

- Saiba mais sobre o [Balanceador de Carga Standard](load-balancer-standard-overview.md).
- [Balancear carga de VMs por zonas de disponibilidade](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
