---
title: Ligar computadores a utilizar o gateway do Log Analytics | Documentos da Microsoft
description: Ligue os seus dispositivos e computadores monitorizados do Operations Manager com o gateway do Log Analytics para enviar dados para a automatização do Azure e o serviço do Log Analytics, quando não têm acesso à Internet.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ae9a1623-d2ba-41d3-bd97-36e65d3ca119
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/02/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 5294d5919b6d4d80c61e183866409123a9edbb60
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53082668"
---
# <a name="connect-computers-without-internet-access-using-the-log-analytics-gateway"></a>Ligar computadores sem acesso à Internet através do gateway do Log Analytics
Este documento descreve como configurar a comunicação com a automatização do Azure e ligadas do Log Analytics com o gateway do Log Analytics quando direto ou computadores monitorizados do Operations Manager não tem acesso à Internet.  O gateway do Log Analytics, que é um proxy de encaminhamento de HTTP que suporte a HTTP com o comando de ligação HTTP de túnel, pode recolher dados e enviá-lo para a automatização do Azure e o Log Analytics em seu nome.  

O gateway do Log Analytics suporta:

* Runbook Workers híbridos de automatização do Azure  
* Computadores Windows com o Microsoft Monitoring Agent diretamente ligado a uma área de trabalho do Log Analytics
* Computadores com Linux com o agente do Log Analytics para Linux diretamente ligado a uma área de trabalho do Log Analytics  
* System Center Operations Manager 2012 SP1 com UR7, Operations Manager 2012 R2 com o UR3, o Operations Manager 2016 e o grupo de gestão do Operations Manager versão 1801 integrado com o Log Analytics.  

Se as políticas de segurança de TI não permitir que os computadores na sua rede para ligar à Internet, tais como ponto de venda (POS) dispositivos ou servidores que suportam serviços de TI, mas precisa conectá-los a automatização do Azure ou o Log Analytics para gerir e monitorizá-las , pode ser configurados para comunicar diretamente com o gateway do Log Analytics para receber a configuração e reencaminhar dados em seu nome.  Se estes computadores são configurados com o agente do Log Analytics para ligar diretamente a uma área de trabalho do Log Analytics, todos os computadores em vez disso, comunica com o gateway do Log Analytics.  O gateway transfere dados dos agentes para o serviço diretamente, não analisa quaisquer dados em trânsito.

Quando um grupo de gestão do Operations Manager está integrado com o Log Analytics, os servidores de gestão podem ser configurados para ligar ao gateway do Log Analytics para receber informações de configuração e enviar os dados recolhidos consoante a solução que tiver ativado.  Agentes do Operations Manager enviam alguns dados, como alertas do Operations Manager, avaliação de configuração, espaço de instância e dados de capacidade para o servidor de gestão. Outros dados de grande volume, como registos do IIS, o desempenho e eventos de segurança são enviados diretamente para o gateway do Log Analytics.  Se tiver uma ou mais servidores de Gateway do Operations Manager implementados numa rede de Perímetro ou outra rede isolada para monitorizar os sistemas não confiáveis, não consegue comunicar com um gateway do Log Analytics.  Servidores de Gateway do Gestor de operações podem apenas informar a um servidor de gestão.  Quando um grupo de gestão do Operations Manager estiver configurado para comunicar com o gateway do Log Analytics, as informações de configuração de proxy é automaticamente distribuídas a todos os computadores geridos por agente que está configurado para recolher dados para o Log Analytics, até mesmo Se a definição está vazia.    

Para proporcionar elevada disponibilidade para o direct ligado ou grupos de gestão de operações que comunicam com o Log Analytics através do gateway, pode utilizar o balanceamento de carga na rede para redirecionar e distribuir o tráfego por vários servidores de gateway.  Se um servidor de gateway ficar inativo, o tráfego será redirecionado para outro nó disponível.  

O agente Log Analytics é necessária no computador com o gateway do Log Analytics para que ele identificar os pontos finais de serviço que necessita para comunicar com e monitorizar o gateway do Log Analytics para analisar o desempenho ou dados de eventos.

Cada agente tem de ter conectividade de rede para um gateway para que os agentes podem transferir automaticamente os dados de e para o gateway. Não é recomendável instalar o gateway num controlador de domínio.

O diagrama seguinte mostra o fluxo de dados pelos agentes diretos para automatização do Azure e o Log Analytics com o servidor de gateway.  Agentes tem de ter suas configurações de proxy que correspondem a mesma porta que o gateway de Log Analytics é configurado para comunicar com o serviço.  

![comunicação do agente direto com o diagrama de serviços](./media/gateway/oms-omsgateway-agentdirectconnect.png)

O diagrama seguinte mostra o fluxo de dados de um grupo de gestão do Operations Manager ao Log Analytics.   

![Comunicações do Gestor de operações com o diagrama de Log Analytics](./media/gateway/log-analytics-agent-opsmgrconnect.png)

## <a name="prerequisites"></a>Pré-requisitos

Ao designar um computador para executar o gateway do Log Analytics, este computador tem de ter o seguinte:

* Windows 10, Windows 8.1, Windows 7
* Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2,  Windows Server 2008
* .NET framework 4.5
* Mínimo de um processador de núcleo de 4 e 8 GB de memória 
* Agente de análise de registo para Windows 

### <a name="language-availability"></a>Disponibilidade de idioma

O gateway do Log Analytics está disponível nos seguintes idiomas:

- Chinês (Simplificado)
- Chinês (Tradicional)
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
O gateway do Log Analytics só suporta Transport Layer Security (TLS) 1.0, 1.1 e 1.2.  Não suporta a Secure Sockets Layer (SSL).  Para garantir a segurança dos dados em trânsito para o Log Analytics, é altamente recomendável que para configurar o gateway a utilizar, pelo menos, Transport Layer Security (TLS) 1.2. As versões mais antigas do TLS/Secure Sockets Layer (SSL) foram encontradas vulneráveis e enquanto trabalham ainda atualmente para permitir a compatibilidade com versões anteriores, estão **não recomendada**.  Para obter mais informações, consulte [enviar dados de forma segura através de TLS 1.2](../../azure-monitor/platform/data-security.md#sending-data-securely-using-tls-12). 

### <a name="supported-number-of-agent-connections"></a>Número suportado de ligações de agente
A tabela seguinte realça o número suportado de agentes ao comunicar com um servidor de gateway.  Esse suporte é baseado em agentes a carregar ~ 200KB de dados a cada 6 segundos. O volume de dados por agente testado é cerca de 2,7 GB por dia.

|Gateway |Aprox. número de agentes suportados|  
|--------|----------------------------------|  
|-CPU: Intel XEON CPU E5 2660 v3 \@ núcleos de 2,6 GHz 2<br> -Memória: 4 GB<br> -Largura de banda de rede: 1 Gbps| 600|  
|-CPU: Intel XEON CPU E5 2660 v3 \@ 2,6 GHz 4 núcleos<br> -Memória: 8 GB<br> -Largura de banda de rede: 1 Gbps| 1000|  

## <a name="download-the-log-analytics-gateway"></a>Transfira o gateway do Log Analytics

Existem duas formas de obter a versão mais recente do ficheiro de configuração do gateway do Log Analytics.

1. Transferir a partir da [Centro de transferências da Microsoft](https://www.microsoft.com/download/details.aspx?id=54443).

1. Transferir a partir do portal do Azure.  Após iniciar sessão no portal do Azure:  

   1. Navegue na lista de serviços e, em seguida, selecione **do Log Analytics**.  
   1. Selecione uma área de trabalho.
   1. No painel da área de trabalho do sob **gerais**, clique em **início rápido**.
   1. Sob **escolha uma origem de dados para ligar à área de trabalho**, clique em **computadores**.
   1. Na **agente direto** painel, clique em **gateway de baixar o Log Analytics**.<br><br> ![Transferir gateway do Log Analytics](./media/gateway/download-gateway.png)

ou 

   1. No painel da área de trabalho do sob **configurações**, clique em **definições avançadas**.
   1. Navegue para **origens ligadas** > **servidores de Windows** e clique em **gateway de baixar o Log Analytics**.

## <a name="install-the-log-analytics-gateway"></a>Instalar o gateway do Log Analytics

Para instalar um gateway, execute os seguintes passos.  Se tiver instalado uma versão anterior, anteriormente denominado *reencaminhador do Log Analytics*, ela será atualizada para esta versão.  

1. Na pasta de destino, faça duplo clique em **gateway.msi do Log Analytics**.
1. Na página **Bem-vindo**, clique em **Seguinte**.<br><br> ![Assistente de configuração do gateway](./media/gateway/gateway-wizard01.png)<br> 
1. Sobre o **contrato de licença** página, selecione **aceito os termos no contrato de licença** para aceitar o EULA e, em seguida, clique em **seguinte**.
1. Sobre o **endereço de proxy e porta** página:
   1. Escreva o número de porta TCP para ser utilizada para o gateway. A configuração configura uma regra de entrada com este número de porta na firewall do Windows.  O valor predefinido é 8080.
      O intervalo válido de número de porta é 1 e 65535. Se a entrada não se encaixa este intervalo, é apresentada uma mensagem de erro.
   1. Opcionalmente, se o servidor onde está instalado o gateway tiver de comunicar através de um proxy, escreva o endereço de proxy em que o gateway tem de se ligar. Por exemplo, `http://myorgname.corp.contoso.com:80`.  Se deixado em branco, o gateway irá tentar ligar diretamente à Internet.  Se o servidor proxy requer autenticação, introduza um nome de utilizador e palavra-passe.<br><br> ![Configuração de proxy do Assistente de gateway](./media/gateway/gateway-wizard02.png)<br>   
   1. Clique em **Seguinte**.
1. Se não tiver o Microsoft Update ativado, a Microsoft Update é apresentada a página onde pode optar por ativá-la. Faça uma seleção e, em seguida, clique em **seguinte**. Caso contrário, avance para o passo seguinte.
1. Sobre o **pasta de destino** página, deixe a pasta predefinida C:\Program Files\OMS Gateway ou escreva a localização onde pretende instalar o gateway e, em seguida, clique em **próxima**.
1. Sobre o **pronto para instalar o** página, clique em **instalar**. Controle de conta de utilizador poderá ser apresentada solicitando permissão para instalar. Se assim for, clique em **Sim**.
1. Depois de concluída a configuração, clique em **concluir**. Pode verificar que o serviço está em execução ao abrir o snap-in de Services. msc e certifique-se de que **gateway do Log Analytics** aparece na lista de serviços e o estado é **em execução**.<br><br> ![Serviços – gateway do Log Analytics](./media/gateway/gateway-service.png)  

## <a name="configure-network-load-balancing"></a>Configurar o balanceamento de carga de rede 
Pode configurar o gateway para elevada disponibilidade com a rede balanceamento de carga (NLB) com o Microsoft rede balanceamento de carga (NLB) ou balanceadores de carga baseada em hardware.  O Balanceador de carga gere tráfego ao redirecionar as ligações pedidas dos agentes do Log Analytics ou servidores de gestão do Operations Manager em todos os seus nós. Se um servidor de Gateway ficar inativo, o tráfego é redirecionado para outros nós.

Para saber como projetar e implementar uma cluster de balanceamento de carga na rede Windows Server 2016, veja [balanceamento de carga na rede](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  Os passos seguintes descrevem como configurar uma cluster de balanceamento de carga na rede Microsoft.  

1. Inicie sessão para o servidor do Windows que é um membro do cluster NLB com uma conta administrativa.  
1. Abra o Gestor de balanceamento de carga na rede no Gestor de servidores, clique em **ferramentas**e, em seguida, clique em **Gestor de balanceamento de carga na rede**.
1. Para ligar um servidor de gateway do Log Analytics com o Microsoft Monitoring Agent instalado, endereço IP do cluster com o botão direito e, em seguida, clique em **Adicionar anfitrião ao Cluster**.<br><br> ![Rede de carga, Gestor de balanceamento – Adicionar anfitrião ao Cluster](./media/gateway/nlb02.png)<br> 
1. Introduza o endereço IP do servidor de gateway que pretende ligar.<br><br> ![Rede do Gestor de balanceamento de carga – Adicionar anfitrião ao Cluster: ligar](./media/gateway/nlb03.png) 
    
## <a name="configure-log-analytics-agent-and-operations-manager-management-group"></a>Configurar o agente do Log Analytics e o grupo de gestão do Operations Manager
A seção a seguir inclui os passos sobre como configurar os agentes, um grupo de gestão do Operations Manager ou do Azure Automation Hybrid Runbook Workers do Log Analytics ligadas diretamente com o gateway do Log Analytics para comunicar com a automatização do Azure ou de registo Análise.  

### <a name="configure-standalone-log-analytics-agent"></a>Configurar o agente do Log Analytics autónomo
Para compreender os requisitos e passos sobre como instalar o agente Log Analytics em computadores Windows, ligando-se diretamente ao Log Analytics, veja [computadores Windows ligar ao Log Analytics](agent-windows.md) ou para Linux, consulte computadores [ Ligar computadores Linux ao Log Analytics](../../azure-monitor/learn/quick-collect-linux-computer.md). No lugar de especificar um servidor proxy ao configurar o agente, é possível substituir esse valor com o endereço IP do servidor de gateway do Log Analytics e o respetivo número de porta.  Se tiver implementado por trás de um balanceador de carga de rede de vários servidores de gateway, a configuração de proxy de agente do Log Analytics é o endereço IP virtual do NLB.  

Para informações relacionadas com o Runbook Worker do Automation híbrida, veja [implementar a função de trabalho de Runbook do híbrida](../../automation/automation-hybrid-runbook-worker.md).

### <a name="configure-operations-manager---all-agents-use-the-same-proxy-server"></a>Configurar o Operations Manager - todos os agentes de utilizam o mesmo servidor proxy
Configurar o Operations Manager para adicionar o servidor de gateway.  A configuração de proxy do Operations Manager é automaticamente aplicada a todos os agentes que reportam ao Operations Manager, mesmo que a definição está vazia.  

Para utilizar o Gateway para suportar o Operations Manager, tem de ter:

* Microsoft Monitoring Agent (versão do agente – **8.0.10900.0** ou posterior) instalado no servidor de Gateway e configurado para áreas de trabalho do Log Analytics com o qual deseja comunicar.
* O gateway tem de ter conectividade à Internet ou ser ligado a um servidor de proxy que faz.

> [!NOTE]
> Se não especificar um valor para o gateway, valores em branco são enviados por push para todos os agentes.
> 

Se esta for a primeira vez que o grupo de gestão do Operations Manager está a registar com uma área de trabalho do Log Analytics, a opção de especificar a configuração de proxy para o grupo de gestão não está disponível na consola de operações.  O grupo de gestão tem de ser registado com êxito no serviço para esta opção ficar disponível.  Tem de atualizar a configuração do proxy do sistema através do Netsh no sistema no qual está a executar a Consola de operações para configurar a integração e todos os servidores de gestão no grupo de gestão.  

1. Abra uma linha de comandos elevada.
   a. Aceda a **começar** e escreva **cmd**.
   b. Com o botão direito **linha de comandos** e selecionar executar como administrador * *.
1. Introduza o seguinte comando e prima **Enter**:

    `netsh winhttp set proxy <proxy>:<port>`

Depois de concluir a integração com o Log Analytics, pode remover a alteração executando `netsh winhttp reset proxy` e, em seguida, utilizar o **configurar o servidor de proxy** opção na consola de operações para especificar o servidor de gateway do Log Analytics. 

1. Abra a consola do Operations Manager e, em **Operations Management Suite**, clique em **ligação** e, em seguida, clique em **configurar servidor Proxy**.<br><br> ![Operations Manager – configurar o servidor Proxy](./media/gateway/scom01.png)<br> 
1. Selecione **utilizar um servidor proxy para aceder ao Operations Management Suite** e, em seguida, escreva o endereço IP do servidor de gateway do Log Analytics ou o endereço IP virtual do NLB. Certifique-se de que comece com o `http://` prefixo.<br><br> ![Operations Manager – endereço do servidor proxy](./media/gateway/scom02.png)<br> 
1. Clique em **Concluir**. Grupo de gestão do Operations Manager está agora configurado para comunicar através do servidor de gateway para o serviço Log Analytics.

### <a name="configure-operations-manager---specific-agents-use-proxy-server"></a>Configurar o Operations Manager - agentes específicos de utilizam servidor proxy
Para ambientes de grandes ou complexos, pode querer apenas servidores específicos (ou grupos) para utilizar o servidor de gateway do Log Analytics.  Para estes servidores, não é possível atualizar o agente do Operations Manager diretamente como este valor é substituído pelo valor global para o grupo de gestão.  Em vez disso, terá de substituir a regra utilizada para enviar estes valores.  

> [!NOTE] 
> Essa mesma técnica de configuração pode ser utilizada para permitir a utilização de vários servidores de gateway do Log Analytics no seu ambiente.  Por exemplo, poderá precisar de servidores de gateway do Log Analytics específicos seja especificado numa base por região.
>  

1. Abra a consola do Operations Manager e selecione o **Authoring** área de trabalho.  
1. Na área de trabalho de criação, selecione **regras** e clique nas **âmbito** botão na barra de ferramentas do Operations Manager. Se este botão não estiver disponível, verifique se tem um objeto, não uma pasta, selecionado no painel de monitorização. O **objetos de pacote de gestão do âmbito** caixa de diálogo apresenta uma lista de classes comuns de destinados, grupos ou objetos. 
1. Tipo **serviço de integridade** no **procure** campo e selecione-o na lista.  Clique em **OK**.  
1. Procure a regra **regra de definição de Proxy do Advisor** e na barra de ferramentas da consola de operações, clique em **substituições** e, em seguida, aponte para **substituir o Rule\For um objeto específico da classe: serviço de estado de funcionamento**  e selecione um objeto específico da lista.  Opcionalmente, pode criar um grupo personalizado que contém o objeto de serviço de estado de funcionamento dos servidores que pretende aplicar esta substituição para e, em seguida, aplicar a substituição para esse grupo.
1. Na **propriedades da substituição** caixa de diálogo, clique para colocar uma marca de verificação no **substituir** coluna seguinte para o **WebProxyAddress** parâmetro.  Na **valor de substituição** campo, introduza o URL da garantia de servidor de gateway do Log Analytics que comece com o `http://` prefixo.  

    >[!NOTE]
    > Não é necessário ativar a regra, conforme já está gerido automaticamente com uma substituição contida no pacote de gestão Microsoft System Center Advisor Secure referência substituir direcionamento do Microsoft System Center Advisor Monitoring Server Group.
    >   

1. Selecione um pacote de gestão do **selecionar o pacote de gestão de destino** listar ou crie um novo pacote de gestão não selado, clicando em **New**. 
1. Quando concluir as alterações, clique em **OK**. 

### <a name="configure-for-automation-hybrid-workers"></a>Configurar para funções de trabalho de híbrida de automatização
Se tiver os Runbook Workers híbridos de automatização no seu ambiente, os seguintes passos fornecem soluções alternativas manuais, temporárias para configurar o Gateway para suportá-los.

Nas etapas a seguir, precisa saber onde reside a conta de automatização da região do Azure. Para localizar a localização:

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
1. Selecione o serviço de automatização do Azure.
1. Selecione a conta de automatização do Azure adequada.
1. Ver a sua região sob **localização**.<br><br> ![Portal do Azure – a localização da conta de automatização](./media/gateway/location.png)  

Utilize as tabelas seguintes para identificar o URL para cada localização:

**URLs do serviço de dados de tempo de execução de tarefa**

| **localização** | **URL** |
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

| **localização** | **URL** |
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

Se o seu computador é registrado automaticamente como uma função de trabalho de Runbook híbrida para a aplicação de patches a utilizar a solução de gestão de atualizações, siga estes passos:

1. Adicione os URLs do serviço de dados de tempo de execução de tarefa para a lista de anfitriões permitido no gateway do Log Analytics. Por exemplo: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
1. Reinicie o serviço de gateway do Log Analytics, utilizando o cmdlet PowerShell seguinte: `Restart-Service OMSGatewayService`

Se o computador está integrado à automatização do Azure utilizando o cmdlet de registo do Runbook Worker híbrido, siga estes passos:

1. Adicione o URL de registo do serviço de agente para a lista de anfitriões permitido no gateway do Log Analytics. Por exemplo: `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
1. Adicione os URLs do serviço de dados de tempo de execução de tarefa para a lista de anfitriões permitido no gateway do Log Analytics. Por exemplo: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
1. Reinicie o serviço de gateway do Log Analytics.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Cmdlets do PowerShell útil
Cmdlets pode ajudá-lo a concluir tarefas que são necessários para atualizar as definições de configuração do gateway do Log Analytics. Antes de usá-las, certifique-se de que para:

1. Instale o gateway do Log Analytics (MSI).
1. Abra uma janela de consola do PowerShell.
1. Para importar o módulo, escreva este comando: `Import-Module OMSGateway`
1. Se não ocorrer nenhum erro no passo anterior, o módulo foi importado com êxito e os cmdlets podem ser utilizados. Tipo `Get-Module OMSGateway`
1. Depois de efetuar alterações, utilizando os cmdlets, certifique-se de que reiniciar o serviço de Gateway.

Se obtiver um erro no passo 3, o módulo não foi importado. O erro pode ocorrer quando não consegue encontrar o módulo PowerShell. Pode encontrá-lo no caminho de instalação do Gateway: *C:\Program Files\Microsoft OMS Gateway\PowerShell\OmsGateway*.

| **Cmdlet** | **Parâmetros** | **Descrição** | **Exemplo** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Chave |Obtém a configuração do serviço |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Chave (obrigatório) <br> Valor |Altera a configuração do serviço |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Obtém o endereço do proxy de reencaminhamento (a montante) |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Endereço<br> Nome de utilizador<br> Palavra-passe |Define o endereço (e a credencial) do proxy de reencaminhamento (a montante) |1. Defina um proxy de reencaminhamento e credenciais:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. Defina um proxy de reencaminhamento que não necessita de autenticação: `Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Desmarque a definição de proxy de reencaminhamento:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |Obtém o anfitrião atualmente permitido (apenas o configuradas localmente anfitrião permitido, não inclui anfitriões permitidos transferidos automaticamente) |`Get-OMSGatewayAllowedHost` | 
| `Add-OMSGatewayAllowedHost` |Anfitrião (obrigatório) |Adiciona o anfitrião à lista de permitidos |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Anfitrião (obrigatório) |Remove o anfitrião da lista de permitidos |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Assunto (obrigatório) |Adiciona o certificado de cliente sujeitos a lista de permitidos |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Assunto (obrigatório) |Remove o requerente do certificado de cliente da lista de permitidos |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |Obtém o cliente atualmente permitido assuntos de certificado (configuradas localmente permitia assuntos, apenas que não inclui assuntos permitidos transferidos automaticamente) |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>Resolução de problemas
Para recolher eventos registados pelo gateway, tem também de ter instalado o agente de Log Analytics.<br><br> ![Visualizador de eventos – registo do gateway de Log Analytics](./media/gateway/event-viewer.png)

**IDs de eventos do log Analytics gateway e descrições**

A tabela seguinte mostra os IDs de evento e descrições para o gateway do Log Analytics eventos de registo.

| **ID** | **Descrição** |
| --- | --- |
| 400 |Qualquer erro de aplicação que não tenha um ID específico |
| 401 |Configuração de errado. Por exemplo: listenPort = "text", em vez de um número inteiro |
| 402 |Exceção na análise de mensagens de handshake TLS |
| 403 |Erro de sistema de rede. Por exemplo: não é possível ligar ao servidor de destino |
| 100 |Informações gerais |
| 101 |Serviço foi iniciado |
| 102 |O serviço foi parado |
| 103 |Recebeu um comando de ligação HTTP do cliente |
| 104 |Não um comando de ligação HTTP |
| 105 |Servidor de destino não está na lista de permitidos ou a porta de destino não é a porta segura (443) <br> <br> Certifique-se de que o agente MMA no seu servidor de Gateway e os agentes a comunicar com o Gateway estão ligadas a mesma área de trabalho do Log Analytics. |
| 105 |Erro TcpConnection – certificado de cliente inválido: CN = Gateway <br><br> Certifique-se de que: <br>    <br> &#149;Estiver a utilizar um Gateway com o número de versão 1.0.395.0 ou superior. <br> &#149;O agente MMA no seu servidor de Gateway e os agentes a comunicar com o Gateway está ligado à mesma área de trabalho do Log Analytics. |
| 106 |O gateway do Log Analytics só suporta o TLS 1.0, TLS 1.1 e 1.2.  Não suporta SSL. Para qualquer versão do protocolo TLS/SSL não suportado, o gateway do Log Analytics gera o evento ID 106.|
| 107 |A sessão TLS foi verificada |

**Contadores de desempenho a recolher**

A tabela seguinte mostra os contadores de desempenho disponíveis para o gateway do Log Analytics. Pode adicionar os contadores utilizando o Monitor de desempenho.

| **Nome** | **Descrição** |
| --- | --- |
| Ligação de cliente de Gateway/ativo do log Analytics |Número de ligações de rede (TCP) de cliente do Active Directory |
| Contagem de Gateway/erro de análise de registo |Número de erros |
| Cliente de Gateway/ligados do log Analytics |Número de clientes ligados |
| Contagem de Gateway/rejeição do log Analytics |Número de rejeições devido a qualquer erro de validação de TLS |

![Contadores de desempenho do gateway de análise de registo](./media/gateway/counters.png)

## <a name="get-assistance"></a>Obter assistência
Quando tem sessão iniciada portal do Azure, pode criar uma solicitação de assistência com o gateway do Log Analytics ou qualquer outro serviço do Azure ou a funcionalidade de um serviço.
Para pedir assistência, clique no símbolo de ponto de interrogação no canto superior direito do portal e, em seguida, clique em **novo pedido de suporte**. Em seguida, conclua o novo formulário de pedido de suporte.

![Novo pedido de suporte](./media/gateway/support.png)

## <a name="next-steps"></a>Passos Seguintes
[Adicionar origens de dados](../../azure-monitor/platform/agent-data-sources.md) para recolher dados a partir de origens ligadas e armazená-la na sua área de trabalho do Log Analytics.
