---
title: VMs de Balanceador de carga através de zonas de disponibilidade - portal do Azure | Microsoft Docs
description: Criar um balanceador de carga Standard com front-end com redundância de zona para equilibrar as VMs em zonas de disponibilidade com o portal do Azure
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: ''
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/18
ms.author: kumud
ms.openlocfilehash: ad476922342844a908961960407eb344711932f5
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="load-balance-vms-across-availability-zones-with-a-standard-load-balancer-using-the-azure-portal"></a>Balanceamento de carga VMs através de zonas de disponibilidade com um balanceador de carga padrão no portal do Azure

Passos neste artigo longo da criação de um padrão de Balanceador de público de carga com uma zona redundante front-end para conseguir alcançarem a redundância de zona sem uma dependência de vários registos DNS. Um único endereço IP Front-end num Balanceador de carga padrão é automaticamente com redundância de zona. Zona redundante front-end a utilizar para o Balanceador de carga, com um único endereço IP agora pode aceder qualquer VM numa rede virtual numa região entre todas as zonas de disponibilidade. Utilize as zonas de disponibilidade para proteger as aplicações e os dados de uma falha pouco provável ou da perda de um datacenter completo. Com redundância de zona, pode efetuar uma ou mais zonas de disponibilidade e o caminho de dados survives quanto uma zona nos permanecem região bom estado de funcionamento. 

Para obter mais informações sobre a utilização de zonas de disponibilidade com o Balanceador de carga padrão, consulte [padrão Balanceador de carga e zonas de disponibilidade](load-balancer-standard-availability-zones.md).

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar. 

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em [http://portal.azure.com](http://portal.azure.com).

## <a name="create-a-public-standard-load-balancer"></a>Criar um balanceador de carga público padrão

O Balanceador de Carga Standard só suporta endereços IP Públicos Standard. Quando cria um novo IP público durante a criação do Balanceador de carga, é automaticamente configurado como uma versão de Standard SKU e também é automaticamente com redundância de zona.

1. No canto superior esquerdo do ecrã, clique em **Criar um recurso** > **Rede** > **Balanceador de Carga**.
2. Na página **Criar um balanceador de carga**, introduza estes valores para o balanceador de carga:
    - *myLoadBalancer* - no nome do balanceador de carga.
    - **Público** - no tipo de balanceador de carga.
     - *myPublicIP* – para o novo endereço IP público que criar. Para tal, clique em **escolher um endereço IP público**e, em seguida, clique em **criar nova**. Para o tipo de nome *myPublicIP*, SKU é padrão, por predefinição e selecione **zona redundante** para **zona disponibilidade**.
    - *myResourceGroupLBAZ* - para que o nome do novo grupo de recursos que criar.
    - **westeurope** - na localização.
3. Clique em **Criar** para criar o balanceador de carga.
   
    ![Criar um balanceador de carga](./media/load-balancer-standard-public-availability-zones-portal/1a-load-balancer.png)


## <a name="create-backend-servers"></a>Criar servidores de back-end

Nesta secção, criar uma rede virtual, criar máquinas virtuais em diferentes zonas (zona 1, 2 de zona e zona 3) para a região adicionar o o conjunto de back-end do Balanceador de carga e, em seguida, instale o IIS nas máquinas virtuais para o ajudar a testar o bala de carga com redundância de zona ncer. Por conseguinte, se uma zona falhar, a pesquisa de estado de funcionamento da VM no mesmo horário falha e tráfego continua a ser servidos por VMs em de outras zonas.

### <a name="create-a-virtual-network"></a>Criar uma rede virtual
1. No lado superior esquerdo do ecrã, clique em **crie um recurso** > **redes** > **rede Virtual** e introduza estes valores para o rede virtual:
    - *myVnet* - no nome da rede virtual.
    - *myResourceGroupLBAZ* - para que o nome do grupo de recursos existente
    - *myBackendSubnet* - no nome da sub-rede.
2. Clique em **Criar** para criar a rede virtual.

    ![Criar uma rede virtual](./media/load-balancer-standard-public-availability-zones-portal/2-load-balancer-virtual-network.png)

## <a name="create-a-network-security-group"></a>Criar um grupo de segurança de rede

1. No lado superior esquerdo do ecrã, clique em **crie um recurso**, no tipo de caixa de pesquisa *grupo de segurança de rede*e na página de grupo de segurança de rede, clique em **criar**.
2. Na página grupo de segurança de rede de criação, introduza estes valores:
    - *myNetworkSecurityGroup* - para que o nome do grupo de segurança de rede.
    - *myResourceGroupLBAZ* - para que o nome do grupo de recursos existente.
   
![Criar uma rede virtual](./media/load-balancer-standard-public-availability-zones-portal/create-nsg.png)

### <a name="create-nsg-rules"></a>Criar regras de NSG

Nesta secção, crie regras do NSG para permitir ligações de entrada através de HTTP e RDP no portal do Azure.

1. No portal do Azure, clique em **todos os recursos** no menu da esquerda e, em seguida, pesquisa e clique **myNetworkSecurityGroup** que está localizado no **myResourceGroupLBAZ** grupo de recursos.
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
 
 ![Criar uma rede virtual](./media/load-balancer-standard-public-availability-zones-portal/8-load-balancer-nsg-rules.png)
5. Repita os passos 2 a 4 para criar outra regra com o nome *myRDPRule* para permitir uma ligação RDP de entrada através da porta 3389 com os seguintes valores:
    - *Service Tag* - na **Origem**.
    - *Internet* - na **Etiqueta do serviço de origem**
    - *3389* - nos **Intervalos de portas de destino**
    - *TCP* - no **Protocolo**
    - *Allow* - na **Ação**
    - *200* - na **Prioridade**
    - *myRDPRule* - no nome
    - *Allow RDP* - na descrição


### <a name="create-virtual-machines"></a>Criar máquinas virtuais

1. No lado superior esquerdo do ecrã, clique em **crie um recurso** > **computação** > **Datacenter do Windows Server 2016** e introduza estes valores para a máquina virtual:
    - *myVM1* - no nome da máquina virtual.        
    - *azureuser* - no nome de utilizador do administrador.    
    - *myResourceGroupLBAZ* - para **grupo de recursos**, selecione **utilizar existente**e, em seguida, selecione *myResourceGroupLBAZ*.
2. Clique em **OK**.
3. Selecione **DS1_V2** para o tamanho da máquina virtual e clique em **Selecionar**.
4. Introduza estes valores para as definições de VM:
    - *zona 1* - para a zona onde colocar a VM.
    -  *myVNet* - certifique-se de que está selecionado como a rede virtual.
    - *myBackendSubnet* - certifique-se de que está selecionado como a sub-rede.
    - *myNetworkSecurityGroup* - para que o nome do grupo de segurança de rede (firewall).
5. Clique em **Desativado** para desativar o diagnóstico de arranque.
6. Clique em **OK**, reveja as definições na página de resumo e, em seguida, clique em **Criar**.
  
 ![Criar uma máquina virtual](./media/load-balancer-standard-public-availability-zones-portal/create-vm-standard-ip.png)

7. Criar uma segunda VM, com o nome *VM2* na zona 2 e o terceiro VM na zona 3 e com *myVnet* que a rede virtual, *myBackendSubnet* como a sub-rede e * *myNetworkSecurityGroup* como a segurança de rede grupo utilizando os passos 1-6.

### <a name="install-iis-on-vms"></a>Instalar o IIS em VMs

1. Clique em **todos os recursos** clique no menu da esquerda e, em seguida, na lista de recursos **myVM1** que está localizado no *myResourceGroupLBAZ* grupo de recursos.
2. Na página **Descrição geral**, clique em **Ligar** para estabelecer o RDP para a VM.
3. Inicie sessão na VM com o nome de utilizador *azureuser*.
4. No ambiente de trabalho do servidor, navegue para **Ferramentas Administrativas do Windows**>**Gestor de Servidor**.
5. Na página de início rápido do Gestor de servidores, clique em **para adicionar funções e funcionalidades**.

   ![Adicionar ao conjunto de endereços back-end - ](./media/load-balancer-standard-public-availability-zones-portal/servermanager.png)    

1. No **Assistente Adicionar Funções e Funcionalidades**, utilize os seguintes valores:
    - Na página **Selecionar tipo de instalação**, clique em **Instalação baseada em funções ou baseada em funcionalidades**.
    - No **servidor de destino selecione** página, clique em **myVM1**.
    - No **função de servidor selecione** página, clique em **servidor Web (IIS)**.
    - Siga as instruções para concluir o resto do assistente.
2. Feche a sessão RDP com a máquina virtual - *myVM1*.
3. Repita os passos 1 a 7 para instalar o IIS em VMs *myVM2* e *myVM3*.

## <a name="create-load-balancer-resources"></a>Criar recursos do balanceador de carga

Nesta secção, vai configurar as definições de balanceador de carga de um conjunto de endereços de back-end e de uma sonda de estado de funcionamento, bem como especificar o balanceador de carga e regras NAT.


### <a name="create-a-backend-address-pool"></a>Criar um conjunto de endereços de back-end

Para distribuir o tráfego pelas VMs, um conjunto de endereços de back-end contém os endereços IP das placas virtuais (NICs) ligadas ao balanceador de carga. Crie o conjunto de endereços de back-end *myBackendPool* de forma a incluir *VM1* e *VM2*.

1. Clique em **Todos os recursos**, no menu da esquerda, e clique em **myLoadBalancer** na lista de recursos.
2. Em **Definições**, clique em **Conjuntos de back-end** e, em seguida, clique em **Adicionar**.
3. Na página **Adicionar um conjunto de back-end**, faça o seguinte:
    - No nome, escreva *myBackEndPool como o nome do conjunto de back-end.
    - Para **rede Virtual**, no menu pendente, clique em **myVNet**
    - Para **Máquina Virtual**, no menu pendente, clique em, **myVM1**.
    - Para **endereço IP**, no menu pendente, clique no endereço IP do myVM1.
4. Clique em **adicionar novo recurso de back-end** para adicionar cada máquina virtual (*myVM2* e *myVM3*) para adicionar ao agrupamento de back-end de Balanceador de carga.
5. Clique em **Adicionar**.

    ![Adicionar ao conjunto de endereços back-end - ](./media/load-balancer-standard-public-availability-zones-portal/add-backend-pool.png)

3. Certifique-se de que a definição de conjunto de back-end de Balanceador de carga apresenta todas as três VMs - **myVM1**, **myVM2** e **myVM3**.

### <a name="create-a-health-probe"></a>Criar uma sonda de estado de funcionamento

Para permitir ao balanceador de carga monitorizar o estado da aplicação, pode utilizar uma sonda de estado de funcionamento. A sonda de estado de funcionamento adiciona ou remove dinamicamente VMs da rotação do balanceador de carga com base na respetiva resposta às verificações de estado de funcionamento. Crie uma sonda de estado de funcionamento, *myHealthProbe*, para monitorizar o estado de funcionamento das VMs.

1. Clique em **Todos os recursos**, no menu da esquerda, e clique em **myLoadBalancer** na lista de recursos.
2. Em **Definições**, clique em **Sondas de estado de funcionamento** e, em seguida, clique em **Adicionar**.
3. Utilize estes valores para criar a sonda de estado de funcionamento:
    - *myHealthProbe* - no nome da sonda de estado de funcionamento.
    - **HTTP** - no tipo de protocolo.
    - *80* - no número da porta.
    - *15* - no número do **Intervalo**, em segundos, entre tentativas da sonda.
    - *2* - no número do **Limiar de mau estado de funcionamento** ou das falhas consecutivas da sonda que têm de ocorrer para que uma VM seja considerada como estando em mau estado de funcionamento.
4. Clique em **OK**.

   ![Adicionar uma sonda](./media/load-balancer-standard-public-availability-zones-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Crie uma regra de balanceador de carga

É utilizada uma regra de balanceador de carga para definir a forma como o tráfego é distribuído pelas VMs. Pode definir a configuração de IP de front-end do tráfego de entrada e o conjunto de IPs de back-end para receber o tráfego, juntamente com a porta de origem e de destino necessárias. Crie uma regra de balanceador de carga *myLoadBalancerRuleWeb* para escutar na porta 80 no front-end *FrontendLoadBalancer* e enviar o tráfego de rede com balanceamento de carga para o conjunto de endereços de back-end *myBackEndPool* também através da porta 80. 

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
    
    ![Adicionar uma regra de balanceamento de carga](./media/load-balancer-standard-public-availability-zones-portal/load-balancing-rule.png)

## <a name="test-the-load-balancer"></a>Testar o balanceador de carga
1. Localize o endereço IP público do Balanceador de Carga no ecrã **Descrição geral**. Clique em **Todos os recursos** e, em seguida, clique em **myPublicIP**.

2. Copie o endereço IP público e cole-o na barra de endereço do browser. A página predefinida do servidor Web do IIS é apresentada no browser.

      ![Servidor Web do IIS](./media/load-balancer-standard-public-availability-zones-portal/9-load-balancer-test.png)

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine o grupo de recursos, o balanceador de carga e todos os recursos relacionados. Para tal, selecione o grupo de recursos que contém o balanceador de carga e clique em **Eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais o [Balanceador de Carga Standard](load-balancer-standard-overview.md).
