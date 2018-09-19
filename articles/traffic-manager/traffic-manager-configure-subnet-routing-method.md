---
title: Configurar o método de encaminhamento de tráfego de sub-rede com o Gestor de tráfego do Azure | Documentos da Microsoft
description: Este artigo explica como configurar o Gestor de tráfego para encaminhar o tráfego de sub-redes específicas.
services: traffic-manager
documentationcenter: ''
author: KumudD
manager: jpconnock
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/17/2018
ms.author: kumud
ms.openlocfilehash: 6e5e6008741306d322ebd07bcbf144133ca4e632
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46131281"
---
# <a name="direct-traffic-to-specific-endpoints-based-on-user-subnet-using-traffic-manager"></a>Direcionar o tráfego para pontos de extremidade específicos com base na sub-rede de utilizador utilizando o Gestor de tráfego

Este artigo descreve como configurar o método de encaminhamento de tráfego de sub-rede. O **sub-rede** o método de encaminhamento de tráfego permite-lhe mapear um conjunto de intervalos de endereços IP para os pontos finais e quando é recebido um pedido pelo Gestor de tráfego, ele inspeciona o IP de origem do pedido e retorna o ponto final associado ao mesmo. 

O cenário abordados neste artigo, usando o roteamento de sub-rede, consoante o endereço IP da consulta do utilizador, o tráfego seja é encaminhado para um Web site interno ou um site de produção.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos
Para ver o Gestor de tráfego em ação, este tutorial requer que implemente o seguinte:
- dois Web sites basic em execução em diferentes regiões do Azure - **E.U.A. Leste** (serve como o Web site interno) e **Europa Ocidental** (serve como o Web site de produção).
- dois VMs de teste para testar o Gestor de tráfego - uma VM na **E.U.A. Leste** e a segunda VM na **Europa Ocidental**. 

O teste de VMs são usados para ilustrar como o Gestor de tráfego encaminha o tráfego de utilizador para o site interno ou para o site de produção com base na sub-rede de onde origina a consulta do utilizador.

### <a name="sign-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no portal do Azure em https://portal.azure.com.

### <a name="create-websites"></a>Criar Web sites

Nesta secção, vai criar duas instâncias do Web site que fornecem os pontos de extremidade do serviço de dois para o perfil do Gestor de tráfego em duas regiões do Azure. Criar os dois Web sites inclui os seguintes passos:
1. Crie duas VMs para a execução de um site básico - um em **E.U.A. Leste**e outra num **Europa Ocidental**.
2. Instalar o servidor do IIS em cada VM e atualize a página de Web site que descreve o nome da VM que um usuário estiver conectado ao visitar o Web site.

#### <a name="create-vms-for-running-websites"></a>Criar VMs para a execução de Web sites
Nesta secção, vai criar duas VMs *myEndpointVMEastUS* e *myEndpointVMWEurope* no **E.U.A. Leste** e **Europa Ocidental** Azure regiões.

1. No canto superior, esquerda do portal do Azure, selecione **criar um recurso** > **computação** > **VM do Windows Server 2016**.
2. Introduza ou selecione as seguintes informações em **Informações Básicas**, aceite as predefinições das restantes definições e, em seguida, selecione **Criar**:

    |Definição|Valor|
    |---|---|
    |Nome|myIISVMEastUS|
    |Nome de utilizador| Introduza um nome de utilizador à sua escolha.|
    |Palavra-passe| Introduza uma palavra-passe à sua escolha. A palavra-passe tem de ter, pelo menos, 12 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Grupo de recursos| Selecione **New** e, em seguida, escreva *myResourceGroupTM1*.|
    |Localização| Selecione **E.U.A. Leste**.|
    |||
4. Selecione um tamanho de VM em **Escolher um tamanho**.
5. Selecione os seguintes valores para **Definições** e, em seguida, selecione **OK**:
    
    |Definição|Valor|
    |---|---|
    |Rede virtual| Selecione **rede Virtual**, na **criar rede virtual**, para **nome**, introduza *myVNet1*, para a sub-rede, introduza  *mySubnet*.|
    |Grupo de Segurança de Rede|Selecione **básica**e, na **Selecione as portas de entrada públicas** menu pendente, selecione **HTTP** e **RDP** |
    |Diagnósticos de arranque|Selecione **desativado**.|
    |||
6. Em **Criar** no **Resumo**, selecione **Criar** para iniciar a implementação da VM.

7. Conclua os passos 1 a 6 novamente, com as seguintes alterações:

    |Definição|Valor|
    |---|---|
    |Grupo de recursos | Selecione **Novo** e introduza *myResourceGroupTM2*|
    |Localização|Europa Ocidental|
    |Nome da VM | myIISVMWEurope|
    |Rede virtual | Selecione **rede Virtual**, na **criar rede virtual**, para **nome**, introduza *myVNet2*, para a sub-rede, introduza  *mySubnet*.|
    |||
8. A criação das VMs demora alguns minutos. Não prossiga com os restantes passos até que ambas as VMs tenham sido criadas.

   ![Criar uma VM](./media/tutorial-traffic-manager-improve-website-response/createVM.png)

#### <a name="install-iis-and-customize-the-default-web-page"></a>Instalar o IIS e personalizar a página Web predefinida

Nesta secção, instalar o servidor IIS em duas VMs - *myIISVMEastUS*  & *myIISVMWEurope*e, em seguida, atualize a página de Web site predefinido. A página de Web site personalizado mostra o nome da VM que está a ligar para quando visitar o Web site de um navegador da web.

1. Selecione **todos os recursos** no menu da esquerda e, em seguida, na lista de recursos, clique em *myIISVMEastUS* que se encontra no *myResourceGroupTM1* grupo de recursos.
2. Sobre o **descrição geral** página, clique em **Connect**e, em seguida, no **ligar à máquina virtual**, selecione **ficheiro RDP transferir**. 
3. Abra o ficheiro rdp transferido. Se lhe for pedido, selecione **Ligar**. Introduza o nome de utilizador e a palavra-passe que especificou ao criar a VM. Poderá ter de selecionar **Mais opções** e **Utilizar uma conta diferente** para especificar as credenciais que introduziu quando criou a VM. 
4. Selecione **OK**.
5. Poderá receber um aviso de certificado durante o processo de início de sessão. Se receber o aviso, selecione **Sim** ou **Continuar** para prosseguir com a ligação.
6. No ambiente de trabalho do servidor, navegue para **Ferramentas Administrativas do Windows**>**Gestor de Servidor**.
7. Lançamento de Windows PowerShell no *myIISVMEastUS* e com os comandos seguintes para instalar o servidor IIS e atualizar o arquivo do padrão htm.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from my test website server - " + $env:computername)
    ```
8. Feche a ligação de RDP com *myIISVMEastUS*.
9. Repita os passos 1 a 6 com através da criação de uma ligação RDP com a VM *myIISVMWEurope* dentro do *myResourceGroupTM2* grupo de recursos para instalar o IIS e personalizar a sua página da web padrão.
10. Lançamento de Windows PowerShell no *myIISVMWEurope* e com os comandos seguintes para instalar o servidor IIS e atualizar o arquivo do padrão htm.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from my production website server - " + $env:computername)
    ```

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>Configurar nomes DNS para as VMs que executam o IIS

O Gestor de tráfego encaminha o tráfego de utilizador com base no nome DNS dos pontos de extremidade de serviço. Nesta secção, vai configurar os nomes DNS para os servidores IIS - *myIISVMEastUS* e *myIISVMWEurope*.

1. Clique em **todos os recursos** no menu da esquerda e, em seguida, na lista de recursos, selecione *myIISVMEastUS* que se encontra no *myResourceGroupTM1* grupo de recursos.
2. Sobre o **descrição geral** página, em **nome DNS**, selecione **configurar**.
3. Sobre o **Configuration** página, em etiqueta de nome DNS, adicione um nome exclusivo e, em seguida, selecione **guardar**.
4. Repita os passos 1 a 3, para a VM com o nome *myIISVMWEurope* que se encontra no *myResourceGroupTM1* grupo de recursos.

### <a name="create-test-vms"></a>Criar VMs de teste

Nesta secção, vai criar uma VM (*mVMEastUS* e *myVMWestEurope*) em cada região do Azure (**E.U.A. Leste** e **Europa Ocidental**. Irá utilizar estas VMs para testar a forma como o Gestor de tráfego encaminha o tráfego para o servidor mais próximo do IIS ao navegar para o Web site.

1. No canto superior, esquerda do portal do Azure, selecione **criar um recurso** > **computação** > **VM do Windows Server 2016**.
2. Introduza ou selecione as seguintes informações em **Informações Básicas**, aceite as predefinições das restantes definições e, em seguida, selecione **Criar**:

    |Definição|Valor|
    |---|---|
    |Nome|myVMEastUS|
    |Nome de utilizador| Introduza um nome de utilizador à sua escolha.|
    |Palavra-passe| Introduza uma palavra-passe à sua escolha. A palavra-passe tem de ter, pelo menos, 12 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Grupo de recursos| Selecione **Existente** e selecione *myResourceGroupTM1*.|
    |||

4. Selecione um tamanho de VM em **Escolher um tamanho**.
5. Selecione os seguintes valores para **Definições** e, em seguida, selecione **OK**:
    |Definição|Valor|
    |---|---|
    |Rede virtual| Selecione **rede Virtual**, na **criar rede virtual**, para **nome**, introduza *myVNet3*, para a sub-rede, introduza  *mySubnet3*.|
    |Grupo de Segurança de Rede|Selecione **básica**e, na **Selecione as portas de entrada públicas** menu pendente, selecione **HTTP** e **RDP** |
    |Diagnósticos de arranque|Selecione **desativado**.|
    |||

6. Em **Criar** no **Resumo**, selecione **Criar** para iniciar a implementação da VM.

7. Conclua os passos 1 a 5 novamente, com as seguintes alterações:

    |Definição|Valor|
    |---|---|
    |Nome da VM | *myVMWEurope*|
    |Grupo de recursos | Selecione **existentes**e, em seguida, escreva *myResourceGroupTM2*|
    |Rede virtual | Selecione **rede Virtual**, na **criar rede virtual**, para **nome**, introduza *myVNet4*, para a sub-rede, introduza  *mySubnet4*.|
    |||

8. A criação das VMs demora alguns minutos. Não prossiga com os restantes passos até que ambas as VMs tenham sido criadas.

## <a name="create-a-traffic-manager-profile"></a>Criar um perfil do Gestor de Tráfego
Crie um perfil de Gestor de tráfego permite-lhe devolver os pontos finais com base no IP de origem do pedido.

1. No canto superior esquerdo do ecrã, selecione **Criar um recurso** > **Rede** > **Perfil do Gestor de Tráfego** > **Criar**.
2. Em **Criar perfil do Gestor de Tráfego**, introduza ou selecione as informações seguintes, aceite as predefinições das definições restantes e selecione **Criar**:
    | Definição                 | Valor                                              |
    | ---                     | ---                                                |
    | Nome                   | Este nome tem de ser exclusivo dentro da zona trafficmanager.net e os resultados no nome do DNS, trafficmanager.net, que é utilizado para aceder ao seu perfil do Gestor de tráfego.                                   |
    | Método de encaminhamento          | Selecione o **sub-rede** método de encaminhamento.                                       |
    | Subscrição            | Selecione a sua subscrição.                          |
    | Grupo de recursos          | Selecione **existentes** e introduza *myResourceGroupTM1*. |
    | |                              |
    |
  
    ![Criar um perfil do Gestor de Tráfego](./media/traffic-manager-subnet-routing-method/create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Adicionar pontos finais do Gestor de Tráfego

Adicione as duas VMs com o IIS servidores – *myIISVMEastUS*  & *myIISVMWEurope* para encaminhar o tráfego de utilizador com base na sub-rede de consulta do utilizador.

1. Na barra de pesquisa do portal, procure o nome do perfil do Gestor de Tráfego que criou na secção anterior e selecione-o nos resultados apresentados.
2. Em **Perfil do Gestor de Tráfego** , na secção **Definições**, clique em **Pontos Finais** e em **Adicionar**.
3. Introduza ou selecione as seguintes informações, aceite as predefinições para as restantes definições e, em seguida, selecione **OK**:

    | Definição                 | Valor                                              |
    | ---                     | ---                                                |
    | Tipo                    | Ponto final do Azure                                   |
    | Nome           | myTestWebSiteEndpoint                                        |
    | Tipo de recurso de destino           | Endereço IP Público                          |
    | Recurso de destino          | **Escolha um endereço IP público** para mostrar a lista de recursos com endereços IP públicos na mesma subscrição. Na **Resource**, selecione o endereço IP público com o nome *myIISVMEastUS-ip*. Este é o endereço IP público da VM na região E.U.A. leste do servidor de IIS.|
    |  Definições de encaminhamento de sub-rede    |   Adicionar o endereço IP do *myVMEastUS* VM de teste. Qualquer consulta de utilizador provenientes desta VM será direcionada para o *myTestWebSiteEndpoint*.    |

4. Repita os passos 2 e 3 para adicionar outro ponto final com o nome *myProductionEndpoint* para o endereço IP público *myIISVMWEurope-ip* que está associado a VM com o nome do servidor IIS *myIISVMWEurope* . Para **definições de encaminhamento de sub-rede**, adicione o endereço IP do VM - teste *myVMWestEurope*. Qualquer consulta de utilizador a partir deste teste será encaminhada para o ponto final - VM *myProductionWebsiteEndpoint*.
5.  Quando a adição de ambos os pontos finais estiver concluída, estes são apresentados em **Perfil do Gestor de Tráfego**, juntamente com o respetivo estado de monitorização como **Online**.

    ![Adicionar um ponto final do Gestor de Tráfego](./media/traffic-manager-subnet-routing-method/customize-endpoint-with-subnet-routing-eastus.png)
  
## <a name="test-traffic-manager-profile"></a>Testar o perfil do Gestor de Tráfego
Nesta secção, vai testar como o Gestor de tráfego encaminha o tráfego de utilizador de uma determinada sub-rede para um ponto de extremidade específico. Para ver o Gestor de tráfego em ação, conclua os seguintes passos:
1. Determine o nome DNS do seu perfil do Gestor de tráfego.
2. Gestor de tráfego de vista em ação, da seguinte forma:
    - Da VM de teste (*myVMEastUS*) que se encontra no **E.U.A. Leste** região, num browser, navegue para o nome DNS do perfil do Traffic Manager.
    - Da VM de teste (*myVMEastUS*) que se encontra no **Europa Ocidental** região, num browser, navegue para o nome DNS do perfil do Traffic Manager.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>Determinar o nome DNS do perfil do Gestor de tráfego
Neste tutorial, para manter a simplicidade, utilize o nome DNS do perfil do Gestor de tráfego para visitar o Web sites. 

É possível determinar o nome DNS do perfil do Gestor de tráfego da seguinte forma:

1.  Na barra de pesquisa do portal, procure o nome do **perfil do Gestor de Tráfego** que criou na secção anterior. Nos resultados que são apresentados, clique no perfil do Gestor de Tráfego.
1. Clique em **Descrição geral**.
2. O **Perfil do Gestor de Tráfego** mostra o nome DNS do perfil que acabou de criar. Nas implementações de produção, configure um nome de domínio personalizado para apontar para o nome de domínio do Gestor de tráfego, com um registo CNAME de DNS.

   ![Nome DNS do Gestor de Tráfego](./media/traffic-manager-subnet-routing-method/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Ver o Gestor de Tráfego em ação
Nesta seção, pode ver o Gestor de tráfego é a ação. 

1. Selecione **todos os recursos** no menu da esquerda e, em seguida, na lista de recursos, clique em *myVMEastUS* que se encontra no *myResourceGroupTM1* grupo de recursos.
2. Sobre o **descrição geral** página, clique em **Connect**e, em seguida, no **ligar à máquina virtual**, selecione **ficheiro RDP transferir**. 
3. Abra o ficheiro rdp transferido. Se lhe for pedido, selecione **Ligar**. Introduza o nome de utilizador e a palavra-passe que especificou ao criar a VM. Poderá ter de selecionar **Mais opções** e **Utilizar uma conta diferente** para especificar as credenciais que introduziu quando criou a VM. 
4. Selecione **OK**.
5. Poderá receber um aviso de certificado durante o processo de início de sessão. Se receber o aviso, selecione **Sim** ou **Continuar** para prosseguir com a ligação. 
1. Num navegador da web na VM *myVMEastUS*, escreva o nome DNS do perfil do Traffic Manager para ver o seu Web site. Desde a VM *myVMEastUS* endereço IP está associado com o ponto final *myIISVMEastUS*, o navegador da web inicia o servidor do Web site de teste - *myIISVMEastUS*.

   ![Testar o perfil do Gestor de Tráfego](./media/traffic-manager-subnet-routing-method/test-traffic-manager.png)

2. Em seguida, ligar à VM *myVMWestEurope* localizado na **Europa Ocidental** utilize os passos 1 a 5 e navegue para o nome de domínio de perfil do Gestor de tráfego desta VM. Desde a VM *myVMWestEurope* endereço IP está associado com o ponto final *myIISVMEastUS*, o navegador da web inicia o servidor do Web site de teste - *myIISVMWEurope*. 
  
## <a name="delete-the-traffic-manager-profile"></a>Eliminar o perfil do Gestor de tráfego
Quando já não for necessário, elimine os grupos de recursos (**ResourceGroupTM1** e **ResourceGroupTM2**). Para tal, selecione o grupo de recursos (**ResourceGroupTM1** ou **ResourceGroupTM2**) e, em seguida, selecione **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [ponderada de método de encaminhamento de tráfego](traffic-manager-configure-weighted-routing-method.md).
- Saiba mais sobre [método de encaminhamento prioritário](traffic-manager-configure-priority-routing-method.md).
- Saiba mais sobre [método de encaminhamento geográfico](traffic-manager-configure-geographic-routing-method.md).


