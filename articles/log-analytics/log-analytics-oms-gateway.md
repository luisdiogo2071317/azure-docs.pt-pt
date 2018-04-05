---
title: Ligar computadores ao utilizar o Gateway do OMS | Microsoft Docs
description: Ligar os seus dispositivos e computadores monitorizados de Operations Manager com o Gateway do OMS para enviar dados para a automatização do Azure e o serviço de análise de registos quando não têm acesso à internet.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ae9a1623-d2ba-41d3-bd97-36e65d3ca119
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: magoedte
ms.openlocfilehash: 66e5444f5346a44cfc8a43cf2b43dbaeacffedc9
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="connect-computers-without-internet-access-by-using-the-oms-gateway"></a>Ligar a computadores sem acesso à internet, utilizando o Gateway do OMS
Este documento descreve como configurar a comunicação com a automatização do Azure e análise de registos ao utilizar o Gateway do OMS quando diretamente ligados computadores ou computadores monitorizados de Operations Manager não tem acesso à internet. O Gateway do OMS é um proxy de HTTP direta que suporte o HTTP de túnel utilizando o comando HTTP ligar. Este pode recolher dados e envia-as à automatização do Azure e análise de registos em nome de computadores sem acesso à internet.  

O Gateway do OMS suporta:

* Os Runbook Workers híbridos da automatização do Azure  
* Computadores Windows com o Microsoft Monitoring Agent diretamente ligado a uma área de trabalho de análise de registos
* Computadores com Linux com o agente do OMS para Linux diretamente ligado a uma área de trabalho de análise de registos  
* System Center Operations Manager 2012 SP1 com UR7, Operations Manager 2012 R2 com UR3, o Operations Manager 2016 e o grupo de gestão do Operations Manager versão 1801 integrado com a análise de registos  

Se as políticas de segurança de TI não permitem a determinados computadores na sua rede para ligar à internet (como ponto de venda dispositivos ou servidores que suportam serviços de TI), mas tem de estabelecer a ligação da automatização do Azure ou Log Analytics para gerir e monitorizá-las, eles pode ser configurado para comunicar diretamente com o Gateway do OMS.

 Se estes computadores estão configurados com o agente do OMS para ligar diretamente a uma área de trabalho de análise de registos, todos os computadores em vez disso, a comunicar com o Gateway do OMS. O Gateway do OMS transfere dados dos agentes para o serviço diretamente. -Não analisar quaisquer dados enquanto esta se encontra em trânsito.

Quando um grupo de gestão do Operations Manager está integrado com a análise de registos, os servidores de gestão podem ser configurados para ligar ao Gateway OMS para receber as informações de configuração e enviar os dados recolhidos. Agentes do Operations Manager enviarem alguns dados, tais como alertas do Operations Manager, avaliação de configuração, espaço de instância e dados de capacidade para o servidor de gestão. Outros dados de elevado volume, tais como registos de IIS, informações de desempenho e eventos de segurança, são enviados diretamente para o Gateway do OMS.  

Se tiver um ou mais servidores de Gateway do Operations Manager que são implementados no DMZ ou outra rede isolado, para monitorizar os sistemas não fidedignos, não conseguirem comunicar com um Gateway do OMS. Servidores de Gateway do Gestor de operações só podem reportar a um servidor de gestão. 

Quando um grupo de gestão do Operations Manager estiver configurado para comunicar com o Gateway do OMS, as informações de configuração de proxy são distribuídas automaticamente para todos os computadores geridos por agente que está configurado para recolher dados para análise de registos, mesmo se a definição está vazia.    

Para fornecer elevada disponibilidade para direta ligados ou grupos de gestão de operações que comunicam com a análise de registos através do Gateway do OMS, pode utilizar o balanceamento de carga na rede para redirecionar e distribuir o tráfego por vários servidores de Gateway do OMS. Se um servidor de gateway ficar inativo, o tráfego é redirecionado para outro nó disponível.  

Recomendamos que instale o agente do OMS no computador que esteja a executar o software de Gateway do OMS monitorizá-lo e analisar dados de desempenho ou eventos. Além disso, o agente ajuda a identificar os pontos finais de serviço que necessita para comunicar com o Gateway de OMS.

Todos os agentes tem de ter conectividade de rede para o seu gateway para que estes podem ser transferidos automaticamente dados de e para o gateway. Recomendamos que não instale um gateway num controlador de domínio.

O diagrama seguinte mostra o fluxo de dados de agentes diretos para a automatização do Azure e análise de registos com o servidor de gateway. A configuração de proxy do agente tem de utilizar a mesma porta que o Gateway do OMS utiliza para comunicar com o serviço.  

![comunicação do agente direta com diagrama de serviços](./media/log-analytics-oms-gateway/oms-omsgateway-agentdirectconnect.png)

O diagrama seguinte mostra o fluxo de dados de um grupo de gestão do Operations Manager para análise de registos.   

![Comunicações do Gestor de operações com o diagrama de análise de registos](./media/log-analytics-oms-gateway/log-analytics-agent-opsmgrconnect.png)

## <a name="prerequisites"></a>Pré-requisitos

Ao designar um computador a executar o Gateway do OMS, certifique-se de que tem os seguintes componentes:

* Windows 10, Windows 8.1, Windows 7
* Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008
* .NET Framework 4.5
* Mínimo de 8 GB de memória e 4 núcleos processador 

### <a name="language-availability"></a>Disponibilidade de idioma

O Gateway do OMS está disponível nos seguintes idiomas:

- Chinês (simplificado)
- Chinês (tradicional)
- Checo
- Neerlandês
- Português
- Francês
- Alemão
- Húngaro
- Italiano
- Japonês
- Coreano
- Polaco
- Português (Brasil)
- Português (Portugal)
- Russo
- Espanhol (internacional)

### <a name="supported-encryption-protocols"></a>Protocolos de encriptação suportados
O Gateway do OMS suporta apenas Transport Layer Security (TLS) 1.0, 1.1 e 1.2.  Não suporta Secure Sockets Layer (SSL).

### <a name="supported-number-of-agent-connections"></a>Número suportado de ligações de agente
A tabela seguinte realça o número suportado de agentes que estão a comunicar com um servidor de gateway. Este suporte é baseado nos agentes carregar cerca de 200 KB de dados a cada 6 segundos. O volume de dados por agente testado é 2.7 GB por dia.

|Gateway |Número aproximado de agentes que são suportados|  
|--------|----------------------------------|  
|-CPU: Intel XEON CPU E5-2660 v3 @ 2.6 GHz 2 núcleos<br> -Memória: 4 GB<br> -A largura de banda: 1 Gbps| 600|  
|-CPU: Intel XEON CPU E5-2660 v3 @ 2.6 GHz 4 núcleos<br> -Memória: 8 GB<br> -A largura de banda: 1 Gbps| 1000|  

## <a name="download-the-oms-gateway"></a>Transfira o Gateway do OMS

Existem duas formas de obter a versão mais recente do ficheiro de configuração do Gateway do OMS.

1. A transferir o [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=54443).

2. Transfiram a partir do portal do Azure. Depois de iniciar sessão portal do Azure, siga os passos seguintes:  

   1. Procurar a lista de serviços e, em seguida, selecione **Log Analytics**.  
   2. Selecione uma área de trabalho.
   3. No painel da área de trabalho, sob **geral**, selecione **início rápido**.
   4. Em **escolha uma origem de dados para ligar à área de trabalho**, selecione **computadores**.
   5. No **agente direta** painel, selecione **transferir Gateway do OMS**.
   
    ![Transfira o Gateway do OMS](./media/log-analytics-oms-gateway/download-gateway.png)

-OR- 

   1. No painel da área de trabalho, sob **definições**, selecione **definições avançadas**.
   2. Selecione **ligado origens** > **servidores Windows**. Em seguida, selecione **transferir Gateway do OMS**.

## <a name="install-the-oms-gateway"></a>Instalar o Gateway do OMS

Para instalar um gateway, siga os passos seguintes. Se instalou a versão anterior, anteriormente denominadas *reencaminhador de análise do registo*, será atualizada para esta versão.  

1. Na pasta de destino, selecione **OMS Gateway.msi**.
2. Na página **Bem-vindo**, selecione **Seguinte**.

 ![Assistente de configuração do gateway](./media/log-analytics-oms-gateway/gateway-wizard01.png)
  
3. No **contrato de licença** página, selecione **aceito os termos no contrato de licença**. Em seguida, selecione **Seguinte**.

4. No **porta e o proxy de endereço** página:

   a. Escreva o número da porta TCP para ser utilizado para o gateway. Programa de configuração configura uma regra de entrada com este número de porta na firewall do Windows. O valor predefinido é 8080. O intervalo válido do número de porta é 1 e 65535. Se a entrada não se enquadram este intervalo, é apresentada uma mensagem de erro.

   b. Opcionalmente, se o servidor onde o gateway está instalado tem de comunicar através de um proxy, escreva o proxy para o qual o gateway tem de ser ligada. Um exemplo seria `http://myorgname.corp.contoso.com:80`, conforme mostrado na captura de ecrã seguinte. Se deixar este campo em branco, o gateway tenta ligar diretamente à internet.  Se o servidor proxy requer autenticação, introduza um nome de utilizador e palavra-passe.
   
    ![Configuração de proxy do Assistente de gateway](./media/log-analytics-oms-gateway/gateway-wizard02.png)

4. Selecione **Seguinte**.

5. Se não tiver o Microsoft Update ativado, é apresentada a página do Microsoft Update e pode optar por ativá-la. Fazer uma seleção e, em seguida, selecione **seguinte**. Caso contrário, avance para o passo seguinte.

6. No **pasta de destino** página, deixe a pasta predefinida como **c:\Programas\Microsoft Files\OMS Gateway** ou escreva a localização onde pretende instalar o gateway do OMS. Em seguida, selecione **Seguinte**.

7. No **pronto para instalar** página, selecione **instalar**. Controlo de conta de utilizador podem aparecer pedir permissão para instalar. Se obtiver um pedido de permissão, selecione **Sim**.

8. Após a configuração, selecione **concluir**. Pode verificar que o serviço está em execução ao abrir o snap-in services.msc, certificando-se de que **OMS Gateway** aparece na lista de serviços, e que o respetivo estado é **executar**.

    ![Serviços – o Gateway do OMS](./media/log-analytics-oms-gateway/gateway-service.png)  

## <a name="configure-network-load-balancing"></a>Configurar o balanceamento de carga de rede 
Pode configurar o gateway de elevada disponibilidade utilizando (NLB) de balanceamento de carga de rede. Utilize o Microsoft rede balanceamento de carga ou balanceadores de carga baseada em hardware.  O Balanceador de carga gere tráfego redirecionar as pedido ligações dos agentes OMS ou servidores de gestão do Operations Manager em todos os nós. Se um servidor de gateway ficar inativo, o tráfego redirecionado para outros nós.

Para saber como estruturar e implementar uma cluster de balanceamento de carga na rede do Windows Server 2016, consulte [balanceamento de carga na rede](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  Os passos seguintes descrevem como configurar uma cluster de balanceamento de carga na rede Microsoft.  

1.  Inicie sessão com uma conta de administrador para o Windows server que seja membro do NLB cluster.  

2.  No Gestor de servidor, abra o Gestor de balanceamento de carga na rede. Selecione **ferramentas**e, em seguida, selecione **Gestor de balanceamento de carga na rede**.

3. Para ligar um servidor de Gateway do OMS com o Microsoft Monitoring Agent instalada, clique no endereço IP do cluster e, em seguida, selecione **Adicionar anfitrião ao Cluster**.

 ![Carga de rede balanceamento Manager-- Adicionar anfitrião ao Cluster](./media/log-analytics-oms-gateway/nlb02.png)

4. Introduza o endereço IP do servidor de gateway para o qual pretende ligar.

    ![Rede do Gestor de balanceamento de carga – Adicionar anfitrião ao Cluster: ligar](./media/log-analytics-oms-gateway/nlb03.png) 
    
## <a name="configure-the-oms-agent-and-the-operations-manager-management-group"></a>Configurar o agente do OMS e o grupo de gestão do Operations Manager
As seguintes secções neste artigo incluem passos sobre como configurar ligados diretamente agentes do OMS, um grupo de gestão do Operations Manager ou trabalhadores de Runbook de híbrida da automatização do Azure com o Gateway do OMS para comunicar com a automatização do Azure ou análise de registos.  

Para compreender os requisitos e passos para instalar o agente do OMS em computadores com Windows ligar diretamente ao Log Analytics, consulte [recolher dados de computadores no seu ambiente com a análise de registos](log-analytics-concept-hybrid.md#prerequisites).

 Para computadores Linux, consulte [computadores Linux ligar ao Log Analytics](log-analytics-quick-collect-linux-computer.md). Para informações relacionadas com o trabalho de Runbook híbrida da automatização, consulte [Runbook Worker híbrido implementar](../automation/automation-hybrid-runbook-worker.md).

### <a name="configure-the-standalone-oms-agent"></a>Configurar o agente do OMS autónomo
Para obter informações sobre como configurar um agente para utilizar um servidor de proxy (que neste caso, é o gateway), consulte [recolher dados de computadores no seu ambiente com a análise de registos](log-analytics-concept-hybrid.md#prerequisites). Se tiver implementado por trás de um balanceador de carga de rede de vários servidores de gateway, a configuração de proxy de agente do OMS é o endereço IP virtual do NLB:

![Propriedades do agente do Microsoft Monitoring – definições do proxy](./media/log-analytics-oms-gateway/nlb04.png)

### <a name="configure-operations-manager-all-agents-use-the-same-proxy-server"></a>Configurar o Operations Manager: todos os agentes utilizam o mesmo servidor proxy
Configurar o Operations Manager para adicionar o servidor de gateway.  A configuração de proxy do Operations Manager é aplicada automaticamente a todos os agentes que reportam ao Operations Manager, mesmo se a definição está vazia.  

Para utilizar o Gateway do OMS para suportar o Operations Manager, os seguintes componentes tem de ser cumpridos:

* O Microsoft Monitoring Agent (versão do agente **8.0.10900.0** ou posterior) instalado no servidor de gateway e configurado para as áreas de trabalho de análise de registos com o qual pretende comunicar.

* Um gateway que tem acesso à internet ou uma ligação a um servidor proxy que suporta.

> [!NOTE]
> Se não especificar um valor para o gateway, os valores em branco são enviadas por push para todos os agentes.
> 

Se esta for a primeira vez que o grupo de gestão do Operations Manager está a registar com uma área de trabalho de análise de registos, a opção de especificar a configuração de proxy para o grupo de gestão não estarão disponível na consola de operações. 

O grupo de gestão tem de ser registado com êxito com o serviço antes desta opção está disponível. Atualize a configuração de proxy do sistema utilizando o Netsh no mesmo sistema em que estiver a executar a consola de operações e todos os servidores de gestão no grupo de gestão.

1. Abra uma linha de comandos elevada.

    a. Aceda a **iniciar**. Em seguida, escreva **cmd**.
    
    b. Clique com botão direito **linha de comandos**. Em seguida, selecione **executar como administrador**.
    
2. Introduza o seguinte comando e, em seguida, selecione **Enter**:

    `netsh winhttp set proxy <proxy>:<port>`

Depois de concluir a integração com a análise de registos, pode remover a alteração executando `netsh winhttp reset proxy`. Em seguida, utilize o **configurar o servidor de proxy** opção na consola de operações para especificar o servidor de Gateway do OMS. 

1. Abra a consola do Operations Manager. Em **Operations Management Suite**, selecione **ligação**. Em seguida, selecione **configurar o servidor de Proxy**.

    ![Operations Manager-- configurar o servidor Proxy](./media/log-analytics-oms-gateway/scom01.png)

2. Selecione **utilizar um servidor proxy para aceder ao Operations Management Suite**. Em seguida, escreva o endereço IP do servidor de Gateway do OMS ou o endereço IP virtual do NLB. Certifique-se de que inicie com o `http://` prefixo.

    ![O Operations Manager – endereço do servidor proxy](./media/log-analytics-oms-gateway/scom02.png)

3. Selecione **Concluir**. O grupo de gestão do Operations Manager está agora configurado para comunicar através do servidor de gateway para o serviço de análise de registos.

### <a name="configure-operations-manager-specific-agents-use-proxy-server"></a>Configurar o Operations Manager: agentes específicos utilizam servidor proxy
Para ambientes de grandes ou complexas, só poderá servidores específicos (ou grupos) para utilizar o servidor de Gateway do OMS. Para estes servidores, não é possível atualizar o agente do Operations Manager diretamente, porque este valor é substituído pelo valor global para o grupo de gestão. Em vez disso, pode substituir a regra que é utilizada para emitir estes valores.  

> [!NOTE] 
> Esta mesma técnica de configuração pode ser utilizada para permitir a utilização de vários servidores de Gateway do OMS no seu ambiente. Por exemplo, poderá necessitar de servidores de Gateway do OMS específicos especificado numa base por região.
>  

1. Abra a consola do Operations Manager e, em seguida, selecione o **criação** área de trabalho.

2. Na área de trabalho de criação de conteúdos, selecione **regras**. Em seguida, selecione o **âmbito** botão na barra de ferramentas do Operations Manager. Se este botão não estiver disponível, certifique-se de se certificar de que tem um objeto, não uma pasta, selecionado no **monitorização** painel. O **objetos de pacote de gestão do âmbito** caixa de diálogo apresenta uma lista de classes de destino comuns, grupos ou objetos. 

3. no **procure** , digite **serviço de integridade**. Em seguida, selecione na lista. Selecione **OK**.  

4. Procure a regra **regra de definição de Proxy do Advisor**. Na barra de ferramentas da consola de operações, selecione **substitui**. Em seguida, selecione **o Rule\For um objeto específico da classe de substituição: serviço de integridade**. 

5. Em seguida, selecione um objeto específico da lista. 

6. (Opcional) Crie um grupo personalizado que contém o objeto de serviço do Estado de funcionamento dos servidores nos quais pretende aplicar esta substituição. Em seguida, em seguida, pode aplicar a substituição a esse grupo.

7. No **propriedades da substituição** caixa de diálogo, clique para colocar uma marca de verificação no **substituir** coluna seguinte para o **WebProxyAddress** parâmetro.  No **valor de substituição** campo, introduza o URL do servidor de Gateway do OMS, garantindo que começa com o `http://` prefixo.  

    >[!NOTE]
    > Não precisa de ativar a regra. Já é gerida automaticamente com uma substituição que tem contidos no pacote de gestão Microsoft System Center Advisor Secure referência Override que o Microsoft System Center Advisor monitorização grupo do servidor de destino.
    >   

8. Selecione um pacote de gestão a **selecionar pacote de gestão de destino** lista ou crie um novo pacote de gestão não selado, selecionando **novo**. 

9. Quando concluir as suas alterações, selecione **OK**. 

### <a name="configure-the-oms-gateway-for-automation-hybrid-runbook-workers"></a>Configurar o Gateway do OMS para automatização de Runbook híbridos
Se tiver de automatização de Runbook híbridos no seu ambiente, os passos seguintes fornecem soluções manuais, temporárias para configurar o Gateway do OMS para os suportar.

Nos passos seguintes, tem de saber a região do Azure onde reside a conta de automatização. Para localizar a localização, siga os passos seguintes:

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
2. Selecione o serviço de automatização do Azure.
3. Selecione a conta de automatização do Azure adequada.
4. Em **localização**, ver a região da conta.

    ![Portal do Azure – localização da conta de automatização](./media/log-analytics-oms-gateway/location.png)  

Utilize as tabelas seguintes para identificar o URL para cada localização:

**URLs do serviço de dados de tempo de execução de tarefa**

| **Localização** | **URL** |
| --- | --- |
| EUA Centro-Norte |ncus-jobruntimedata-prod-su1.azure-automation.net |
| Europa Ocidental |we-jobruntimedata-prod-su1.azure-automation.net |
| EUA Centro-Sul |scus-jobruntimedata-prod-su1.azure-automation.net |
| EUA Leste 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| Canadá central |cc-jobruntimedata-prod-su1.azure-automation.net |
| Europa do Norte |ne-jobruntimedata-prod-su1.azure-automation.net |
| Sudeste Asiático |sea-jobruntimedata-prod-su1.azure-automation.net |
| Índia Central |cid-jobruntimedata-prod-su1.azure-automation.net |
| Japão |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Austrália |ase-jobruntimedata-prod-su1.azure-automation.net |

**URLs do serviço de agente**

| **Localização** | **URL** |
| --- | --- |
| EUA Centro-Norte |ncus-agentservice-prod-1.azure-automation.net |
| Europa Ocidental |we-agentservice-prod-1.azure-automation.net |
| EUA Centro-Sul |scus-agentservice-prod-1.azure-automation.net |
| EUA Leste 2 |eus2-agentservice-prod-1.azure-automation.net |
| Canadá central |cc-agentservice-prod-1.azure-automation.net |
| Europa do Norte |ne-agentservice-prod-1.azure-automation.net |
| Sudeste Asiático |sea-agentservice-prod-1.azure-automation.net |
| Índia Central |cid-agentservice-prod-1.azure-automation.net |
| Japão |jpe-agentservice-prod-1.azure-automation.net |
| Austrália |ase-agentservice-prod-1.azure-automation.net |

Se o computador é automaticamente registado como um Runbook Worker híbrido para aplicação de patches utilizando a solução de gestão de atualizações, siga estes passos:

1. Adicione os URLs do serviço de dados de tempo de execução da tarefa para a lista de anfitriões permitido no Gateway do OMS. Por exemplo, escreva o seguinte: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`

2. Reinicie o serviço de Gateway do OMS, utilizando o cmdlet PowerShell seguinte: `Restart-Service OMSGatewayService`

Se uma o computador para automatização do Azure utilizando o cmdlet de registo do Runbook Worker híbrido, siga estes passos:

1. Adicione o URL de registo do serviço de agente para a lista de anfitriões permitido no Gateway do OMS. Por exemplo, escreva o seguinte: `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`

2. Adicione os URLs do serviço de dados de tempo de execução da tarefa para a lista de anfitriões permitido no Gateway do OMS. Por exemplo, escreva o seguinte: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`

3. Reinicie o serviço de Gateway do OMS:  `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Cmdlets do PowerShell útil
Os cmdlets podem ajudar a concluir as tarefas que são necessárias para atualizar as definições de configuração do OMS Gateway. Antes de utilizá-los, é necessário executar os seguintes passos:

1. Instale o Gateway do OMS (MSI).
2. Abra uma janela da consola do PowerShell.
3. Importe o módulo escrevendo este comando: `Import-Module OMSGateway`.
4. Se nenhum erro no passo anterior, o módulo foi importado com êxito e os cmdlets podem ser utilizados. Digite `Get-Module OMSGateway`.
5. Depois de efetuar alterações ao utilizar os cmdlets, certifique-se de que reinicie o serviço de Gateway do OMS.

Se obtiver um erro no passo 3, o módulo não foi importado. O erro pode ocorrer quando não consegue localizar o módulo PowerShell. Pode encontrá-lo no caminho de instalação do Gateway do OMS: *C:\Program Files\Microsoft OMS Gateway\PowerShell*.

| **Cmdlet** | **Parâmetros** | **Descrição** | **Exemplo** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Chave |Obtém a configuração do serviço |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Chave (obrigatório) <br> Valor |Altera a configuração do serviço |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Obtém o endereço do proxy (montante) do reencaminhamento de |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Endereço<br> Nome de utilizador<br> Palavra-passe |Define o endereço (e a credencial) do proxy (montante) do reencaminhamento de |1. Defina um proxy de reencaminhamento e da credencial:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. Defina um proxy de reencaminhamento que não necessita de autenticação: `Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Desmarque a definição de proxy de reencaminhamento:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |Obtém o anfitrião atualmente permitido (apenas localmente configurado permitido anfitrião; não inclui anfitriões permitidos transferidos automaticamente) |`Get-OMSGatewayAllowedHost` | 
| `Add-OMSGatewayAllowedHost` |Anfitrião (obrigatório) |Adiciona o anfitrião à lista de permitidos |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Anfitrião (obrigatório) |Remove o anfitrião da lista de permitidos |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Assunto (obrigatório) |Adiciona o certificado de cliente sujeitos a lista de permitidos |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Assunto (obrigatório) |Remove o requerente do certificado de cliente da lista de permitidos |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |Obtém o cliente atualmente permitido assuntos de certificado (apenas localmente configurado permitido assuntos; não inclui assuntos permitidos transferidos automaticamente) |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>Resolução de problemas
Para recolher eventos registados pelo gateway, tem também de ter instalado o agente do OMS.

![Visualizador de eventos - registo de Gateway do OMS](./media/log-analytics-oms-gateway/event-viewer.png)

**Os IDs de evento de Gateway do OMS e descrições**

A tabela seguinte mostra os IDs de eventos e as descrições de eventos de registo de Gateway do OMS:

| **ID** | **Descrição** |
| --- | --- |
| 400 |Qualquer erro de aplicação que não tenha um ID específico. |
| 401 |Configuração errada. Por exemplo: listenPort = "text", em vez de um número inteiro. |
| 402 |Excepção na análise de mensagens de handshake TLS. |
| 403 |Erro de rede. Por exemplo: não é possível ligar ao servidor de destino. |
| 100 |Informações gerais. |
| 101 |Serviço foi iniciado. |
| 102 |Serviço foi parado. |
| 103 |Recebido um comando HTTP estabelecer a ligação do cliente. |
| 104 |Não um ligação HTTP comando. |
| 105 |Servidor de destino não está na lista de permissões ou a porta de destino não é porta segura (443). <br> <br> Certifique-se de que o agente MMA no seu servidor de Gateway e os agentes que estão a comunicar com o Gateway está ligado à mesma área de trabalho de análise de registos. |
| 105 |Erro TcpConnection – certificado de cliente inválido: CN = Gateway <br><br> Certifique-se de que: <br>    <br> -Estiver a utilizar um Gateway com o número de versão 1.0.395.0 ou superior. <br> -O agente MMA no seu servidor de Gateway e os agentes que estão a comunicar com o gateway estão ligados ao mesmo espaço de trabalho de análise de registos. |
| 106 |O Gateway do OMS só suporta TLS 1.0, TLS 1.1 e 1.2.  Não suporta SSL. Para qualquer versão de protocolo do TLS/SSL não suportada, o OMS Gateway gera 106 de ID de evento.|
| 107 |A sessão TLS foi verificada. |

**Contadores de desempenho para recolher**

A tabela seguinte mostra os contadores de desempenho que estão disponíveis para o Gateway do OMS. Pode adicionar os contadores, utilizando o Monitor de desempenho.

| **Nome** | **Descrição** |
| --- | --- |
| Ligação de cliente de Gateway/ativo do OMS |Número de ligações de rede (TCP) do Active Directory do cliente |
| Contagem de erros/Gateway do OMS |Número de erros |
| Cliente de Gateway/ligado do OMS |Número de clientes ligados |
| Contagem de Gateway/rejeição do OMS |Número de rejeições devido a algum erro de validação de TLS |

![Contadores de desempenho do Gateway do OMS](./media/log-analytics-oms-gateway/counters.png)

## <a name="get-assistance"></a>Obter assistência
Quando tem sessão iniciada portal do Azure, pode criar um pedido de assistência com o Gateway do OMS ou qualquer outro serviço do Azure ou a funcionalidade de um serviço.

Para pedir assistência, selecione o símbolo de ponto de interrogação no canto superior direito do portal. Em seguida, selecione **novo pedido de suporte**. Preencha o formulário de pedido de suporte novo.

![Novo pedido de suporte](./media/log-analytics-oms-gateway/support.png)

## <a name="next-steps"></a>Passos seguintes
[Adicionar origens de dados](log-analytics-data-sources.md) para recolher dados de origens de ligado e armazene-o na sua área de trabalho de análise de registos.
