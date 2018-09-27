---
title: Tutorial - encaminhar o tráfego para melhorar a resposta do Web site utilizando o Gestor de tráfego do Azure | Documentos da Microsoft
description: Este artigo de tutorial descreve como criar um perfil do Gestor de tráfego para criar um Web site de elevada capacidade de resposta.
services: traffic-manager
documentationcenter: ''
author: kumudd
manager: jeconnoc
editor: ''
Customer intent: As an IT Admin, I want to route traffic so I can improve website response by choosing the endpoint with lowest latency.
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/23/2018
ms.author: kumud
ms.openlocfilehash: 087dcda5826d96ad064c472fc897be7e61133387
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392523"
---
# <a name="tutorial-improve-website-response-using-traffic-manager"></a>Tutorial: Melhorar a resposta do Web site utilizando o Gestor de tráfego 

Este tutorial descreve como utilizar o Gestor de tráfego para criar um site de elevada capacidade de resposta ao direcionar o tráfego de utilizador para o Web site com a menor latência. Normalmente, o Centro de dados com a latência mais baixa é aquele que está mais próximo na distância geográfica.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Crie duas VMs em execução um Web site básico no IIS
> * Criar teste duas VMs para ver o Gestor de tráfego em ação
> * Configurar nome DNS para as VMs que executam o IIS
> * Criar um perfil do Gestor de tráfego para um desempenho melhorado de site
> * Adicionar pontos finais da VM para o perfil do Gestor de tráfego
> * Ver o Gestor de Tráfego em ação

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos
Para ver o Gestor de tráfego em ação, este tutorial requer que implemente o seguinte:
- duas instâncias do Web sites basic em execução em diferentes regiões do Azure - **E.U.A. Leste** e **Europa Ocidental**.
- dois VMs de teste para testar o Gestor de tráfego - uma VM na **E.U.A. Leste** e a segunda VM na **Europa Ocidental**. O teste de VMs são usados para ilustrar como o Gestor de tráfego encaminha o tráfego de utilizador para o site no qual está em execução na mesma região pois ele fornece a latência mais baixa.

### <a name="sign-in-to-azure"></a>Iniciar sessão no Azure 

Inicie sessão no portal do Azure em https://portal.azure.com.

### <a name="create-websites"></a>Criar Web sites

Nesta secção, vai criar duas instâncias do Web site que fornecem os pontos de extremidade do serviço de dois para o perfil do Gestor de tráfego em duas regiões do Azure. Criar os dois Web sites inclui os seguintes passos:
1. Crie duas VMs para a execução de um site básico - um em **E.U.A. Leste**e outra num **Europa Ocidental**.
2. Instalar o servidor do IIS em cada VM e atualize a página de Web site que descreve o nome da VM que um usuário estiver conectado ao visitar o Web site.

#### <a name="create-vms-for-running-websites"></a>Criar VMs para a execução de Web sites
Nesta secção, vai criar duas VMs *myIISVMEastUS* e *myIISVMWEurope* no **E.U.A. Leste** e **Europa Ocidental** regiões do Azure.

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
7. Inicie o Windows PowerShell na VM1 e utilize os seguintes comandos para instalar o servidor de ISS e atualizar o ficheiro html predefinido.
    ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```

     ![Instalar o IIS e personalizar a página da web](./media/tutorial-traffic-manager-improve-website-response/deployiis.png)
8. Feche a ligação de RDP com *myIISVMEastUS*.
9. Repita os passos 1 a 8 com através da criação de uma ligação RDP com a VM *myIISVMWEurope* dentro do *myResourceGroupTM2* grupo de recursos para instalar o IIS e personalizar a sua página da web padrão.

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
    |Rede virtual| Selecione **rede Virtual**, na **criar rede virtual**, para **nome**, introduza *myVNet3*, para a sub-rede, introduza  *mySubnet*.|
    |Grupo de Segurança de Rede|Selecione **básica**e, na **Selecione as portas de entrada públicas** menu pendente, selecione **HTTP** e **RDP** |
    |Diagnósticos de arranque|Selecione **desativado**.|
    |||

6. Em **Criar** no **Resumo**, selecione **Criar** para iniciar a implementação da VM.

7. Conclua os passos 1 a 5 novamente, com as seguintes alterações:

    |Definição|Valor|
    |---|---|
    |Nome da VM | *myVMWEurope*|
    |Grupo de recursos | Selecione **existentes**e, em seguida, escreva *myResourceGroupTM2*|
    |Rede virtual | Selecione **rede Virtual**, na **criar rede virtual**, para **nome**, introduza *myVNet4*, para a sub-rede, introduza  *mySubnet*.|
    |||

8. A criação das VMs demora alguns minutos. Não prossiga com os restantes passos até que ambas as VMs tenham sido criadas.

## <a name="create-a-traffic-manager-profile"></a>Criar um perfil do Gestor de Tráfego
Crie um perfil do Gestor de tráfego que direciona o tráfego de utilizadores, enviando-as para o ponto final com latência mais baixa.

1. No canto superior esquerdo do ecrã, selecione **Criar um recurso** > **Rede** > **Perfil do Gestor de Tráfego** > **Criar**.
2. Em **Criar perfil do Gestor de Tráfego**, introduza ou selecione as informações seguintes, aceite as predefinições das definições restantes e selecione **Criar**:
    | Definição                 | Valor                                              |
    | ---                     | ---                                                |
    | Nome                   | Este nome tem de ser exclusivo dentro da zona trafficmanager.net e os resultados no nome do DNS, trafficmanager.net, que é utilizado para aceder ao seu perfil do Gestor de tráfego.                                   |
    | Método de encaminhamento          | Selecione o **desempenho** método de encaminhamento.                                       |
    | Subscrição            | Selecione a sua subscrição.                          |
    | Grupo de recursos          | Selecione **criar novo** e introduza *myResourceGroupTM1*. |
    | Localização                | Selecione **E.U.A. Leste**.  Esta definição refere-se à localização do grupo de recursos e não tem qualquer impacto no perfil do Gestor de Tráfego que vai ser implementado globalmente.                              |
    |
  
    ![Criar um perfil do Gestor de Tráfego](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Adicionar pontos finais do Gestor de Tráfego

Adicione as duas VMs com o IIS servidores – *myIISVMEastUS*  & *myIISVMWEurope* para encaminhar o tráfego de utilizador para o ponto final mais próximo ao usuário.

1. Na barra de pesquisa do portal, procure o nome do perfil do Gestor de Tráfego que criou na secção anterior e selecione-o nos resultados apresentados.
2. Em **Perfil do Gestor de Tráfego** , na secção **Definições**, clique em **Pontos Finais** e em **Adicionar**.
3. Introduza ou selecione as seguintes informações, aceite as predefinições para as restantes definições e, em seguida, selecione **OK**:

    | Definição                 | Valor                                              |
    | ---                     | ---                                                |
    | Tipo                    | Ponto final do Azure                                   |
    | Nome           | myEastUSEndpoint                                        |
    | Tipo de recurso de destino           | Endereço IP Público                          |
    | Recurso de destino          | **Escolha um endereço IP público** para mostrar a lista de recursos com endereços IP públicos na mesma subscrição. Na **Resource**, selecione o endereço IP público com o nome *myIISVMEastUS-ip*. Este é o endereço IP público da VM na região E.U.A. leste do servidor de IIS.|
    |        |           |

4. Repita os passos 2 e 3 para adicionar outro ponto final com o nome *myWestEuropeEndpoint* para o endereço IP público *myIISVMWEurope-ip* que está associado a VM com o nome do servidor IIS *myIISVMWEurope* .
5.  Quando a adição de ambos os pontos finais estiver concluída, estes são apresentados em **Perfil do Gestor de Tráfego**, juntamente com o respetivo estado de monitorização como **Online**.

    ![Adicionar um ponto final do Gestor de Tráfego](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-endpoint.png)
  

## <a name="test-traffic-manager-profile"></a>Testar o perfil do Gestor de Tráfego
Nesta secção, vai testar a forma como o Gestor de tráfego encaminha o tráfego de utilizador para as VMs mais próximos em execução o Web site para fornecer uma latência mínima. Para ver o Gestor de tráfego em ação, conclua os seguintes passos:
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

   ![Nome DNS do Gestor de Tráfego](./media/tutorial-traffic-manager-improve-website-response/traffic-manager-dns-name.png)

### <a name="view-traffic-manager-in-action"></a>Ver o Gestor de Tráfego em ação
Nesta seção, pode ver o Gestor de tráfego é a ação. 

1. Selecione **todos os recursos** no menu da esquerda e, em seguida, na lista de recursos, clique em *myVMEastUS* que se encontra no *myResourceGroupTM1* grupo de recursos.
2. Sobre o **descrição geral** página, clique em **Connect**e, em seguida, no **ligar à máquina virtual**, selecione **ficheiro RDP transferir**. 
3. Abra o ficheiro rdp transferido. Se lhe for pedido, selecione **Ligar**. Introduza o nome de utilizador e a palavra-passe que especificou ao criar a VM. Poderá ter de selecionar **Mais opções** e **Utilizar uma conta diferente** para especificar as credenciais que introduziu quando criou a VM. 
4. Selecione **OK**.
5. Poderá receber um aviso de certificado durante o processo de início de sessão. Se receber o aviso, selecione **Sim** ou **Continuar** para prosseguir com a ligação. 
1. Num navegador da web na VM *myVMEastUS*, escreva o nome DNS do perfil do Traffic Manager para ver o seu Web site. Uma vez que a VM localizado em **E.U.A. Leste**, são encaminhados para o site mais próximo hospedado no servidor do IIS mais próximo *myIISVMEastUS* que se encontra no **E.U.A. Leste**.

   ![Testar o perfil do Gestor de Tráfego](./media/tutorial-traffic-manager-improve-website-response/eastus-traffic-manager-test.png)

2. Em seguida, ligar à VM *myVMWestEurope* localizado na **Europa Ocidental** utilize os passos 1 a 5 e navegue para o nome de domínio de perfil do Gestor de tráfego desta VM.  Uma vez que a VM localizado em **Europa Ocidental**, agora é encaminhados para o Web site alojado no mais próximo do servidor do IIS *myIISVMWEurope* que se encontra no **Europa Ocidental**. 

   ![Testar o perfil do Gestor de Tráfego](./media/tutorial-traffic-manager-improve-website-response/westeurope-traffic-manager-test.png)
   
## <a name="delete-the-traffic-manager-profile"></a>Eliminar o perfil do Gestor de tráfego
Quando já não for necessário, elimine os grupos de recursos (**ResourceGroupTM1** e **ResourceGroupTM2**). Para tal, selecione o grupo de recursos (**ResourceGroupTM1** ou **ResourceGroupTM2**) e, em seguida, selecione **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Distribuir o tráfego para um conjunto de pontos finais](traffic-manager-configure-weighted-routing-method.md)


