---
title: Solução de Monitor de desempenho no Azure de rede | Microsoft Docs
description: Monitor de desempenho no Azure de rede ajuda a monitorizar o desempenho das suas redes-in quase real tempo a detetar e localizar congestionamentos de desempenho de rede.
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 90fd9b09f90d5b209e0eb8cc6996ef7fb1e7e0b4
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="network-performance-monitor-solution-in-azure"></a>Solução de Monitor de desempenho no Azure de rede

![Símbolo de Monitor de desempenho de rede](./media/log-analytics-network-performance-monitor/npm-symbol.png)


Monitor de desempenho de rede (NPM) é uma solução que ajuda a monitorizar o desempenho de rede entre vários pontos na sua infraestrutura de rede, a conectividade de rede do monitor para pontos finais de aplicação do serviço de monitorização de rede de baseado na nuvem híbrida e Monitorize o desempenho do seu Azure ExpressRoute.  

NPM Deteta problemas de rede, como tráfego blackholing, encaminhamento erros e problemas que métodos de monitorização de rede convencional não são possível detetar. A solução gera alertas, notifica quando um limiar é quebrado para uma ligação de rede, garante uma deteção atempada do problemas de desempenho de rede e localizes a origem do problema para um segmento de rede específico ou o dispositivo. 

NPM oferece três capacidades abrangentes: 

[Monitor de desempenho](log-analytics-network-performance-monitor-performance-monitor.md): monitorizar a conectividade de rede entre localizações de implementações e no local na nuvem, de múltiplos centros de dados e de sucursais, o missão críticos multicamados aplicações/microserviços. Com a monitorização de desempenho, pode detetar problemas de rede antes dos utilizadores queixarem.  

[Monitor de ponto final de serviço](log-analytics-network-performance-monitor-service-endpoint.md): pode monitorizar a conectividade do que os serviços que lhe interessam, determinar a infraestrutura de que está a ser o caminho e de rede onde os utilizadores estão a ocorrer congestionamentos. Conhecer falhas antes dos utilizadores e ver a localização exata dos problemas no seu caminho de rede. 

Esta capacidade ajuda-o a efetuar http, HTTPS, TCP e ICMP com base em testes para monitorizar no quase em tempo real ou historicamente a disponibilidade e o tempo de resposta do seu serviço e a contribuição da rede em perda de pacotes e latência. Com o mapa de topologia de rede, pode isolar abrandamentos de rede ao identificar oportunidades de problema que ocorrem ao longo do caminho de rede a partir do nó para o serviço, com dados de latência em cada hop. Com testes incorporados, monitorize a conectividade de rede para o Office 365 e Dynamics CRM sem qualquer pré configuração de. Com esta capacidade, pode monitorizar a conectividade de rede para qualquer ponto final com capacidade de TCP, tais como Web sites, SaaS, PaaS aplicações, bases de dados do SQL Server, etc.  

[Monitor de ExpressRoute](log-analytics-network-performance-monitor-expressroute.md): monitorizar a conetividade ponto a ponto e o desempenho entre os escritórios e o Azure, através do ExpressRoute do Azure.  

Obter mais informações sobre as diversas capacidades suportadas pelo [Monitor de desempenho de rede](https://docs.microsoft.com/azure/networking/network-monitoring-overview) está disponível online.
 

## <a name="set-up-and-configure"></a>Configurar

### <a name="install-and-configure-agents"></a>Instalar e configurar agentes 

Utilize os processos de básico para instalar agentes em [computadores Windows ligar ao Log Analytics](log-analytics-windows-agents.md) e [estabelecer a ligação do Operations Manager ao Log Analytics](log-analytics-om-agents.md).

### <a name="where-to-install-the-agents"></a>Onde pretende instalar os agentes 

**Monitor de desempenho:** instalar agentes do OMS no nó, pelo menos, um ligado a cada sub-rede de que pretende monitorizar a conectividade de rede com outros sub-redes.  

Para monitorizar uma ligação de rede, tem de instalar os agentes em ambos os pontos finais dessa ligação.  Se não souber sobre a topologia da sua rede, instale os agentes em servidores com cargas de trabalho críticas entre os quais pretende monitorizar o desempenho da rede. Por exemplo, se pretender monitorizar a ligação de rede entre um servidor Web e um servidor que executa o SQL Server, instale um agente em ambos os servidores. Os agentes monitorizam conectividade de rede (ligações) entre anfitriões não os anfitriões próprios. 

**Monitor de ponto final de serviço:** instalar agente do OMS em cada nó a partir do qual pretende monitorizar a conectividade de rede para ponto final do serviço. Por exemplo, se pretender monitorizar a conectividade de rede para o Office 365 do seu site do office O1, O2 e O3, em seguida, instalar o agente do OMS em pelo menos um nó do O1, O2 e O3. 

**Monitor de ExpressRoute:** instale, pelo menos, um agente do OMS no seu Azure VNET e, pelo menos, um agente no seu sub-rede no local, que está ligada através do Peering de ExpressRoute privada.  

### <a name="configure-oms-agents-for-monitoring"></a>Configurar os agentes do OMS para monitorização  

NPM utiliza transações sintéticas para monitorizar o desempenho de rede entre os agentes de origem e de destino. A solução oferece uma opção entre TCP e ICMP como o protocolo para a monitorização em capacidades de Monitor de desempenho e Monitor do ponto final de serviço, enquanto TCP é utilizado para o Monitor do ExpressRoute. Certifique-se de que a firewall permite a comunicação entre os agentes do OMS que está a ser utilizado para monitorizar no protocolo que escolheu para a monitorização.  

**Protocolo TCP:** se escolheu TCP como protocolo para monitorização, abra a porta de Firewall sobre os agentes que está a ser utilizado para capacidades de Monitor de desempenho e Monitor do ExpressRoute, para garantir que os agentes podem ligar-se entre si. Para tal, execute o script de EnableRules.ps1 PowerShell sem quaisquer parâmetros numa janela do power shell com privilégios administrativos.  

O script cria as chaves de registo necessárias pela solução e cria as regras de firewall do Windows para permitir que os agentes para criar ligações TCP entre si. As chaves de registo criadas pelo script também especificar se pretende registar os registos de depuração e o caminho para o ficheiro de registos. Também define a porta TCP de agente utilizada para comunicação. Os valores para estas chaves são configurados automaticamente pelo script de, pelo que não deve alterar manualmente estas chaves. A porta aberta por predefinição é 8084. Pode utilizar uma porta personalizada, fornecendo o portNumber de parâmetro para o script. No entanto, a mesma porta deve ser utilizada em todos os computadores em que o script é executado. 

>[!NOTE]
> O script configura apenas firewall do windows localmente. Se tiver uma firewall de rede, certifique-se de que está a permitir o tráfego destinado para a porta TCP que está a ser utilizada por NPM 

>[!NOTE]
> Não é necessário executar o script do EnableRules.ps1 PowerShell do Monitor de ponto final de serviço 

 

**Protocolo ICMP** - se de que escolheu ICMP, como o protocolo para monitorização, ative as seguintes regras de firewall para a utilização de forma fiável ICMP: 

 
```
netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
```
 

### <a name="configure-the-solution"></a>Configurar a solução 

1. Adicionar a solução de Monitor de desempenho de rede para a sua área de trabalho de [do Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview) ou utilizando o processo descrito no [soluções de análise de registos adicionar da galeria do soluções](log-analytics-add-solutions.md). 
2. Abra a área de trabalho de análise de registos e clique em de **descrição geral** mosaico.  
3. Clique no mosaico intitulado **Monitor de desempenho de rede** com a mensagem *solução requer configuração adicional*.

    ![Mosaico NPM](media/log-analytics-network-performance-monitor/npm-config.png)

3. No **configuração** página, verá a opção para instalar agentes OMS e configurar os agentes para monitorização no **definições comuns** vista. Tal como explicado anteriormente, se já tiver instalado e configurado os agentes do OMS, em seguida, clique em de **configuração** vista para configurar a capacidade que está interessado em utilização.  

    **Vista de Monitor de desempenho** -escolher o protocolo deve ser utilizado para transações sintéticas predefinida regra de monitor de desempenho e clique em Guardar e continuar. Esta seleção do protocolo contém apenas para a regra predefinida gerada pelo sistema e tem de escolher o protocolo sempre que cria uma regra de Monitor de desempenho explicitamente. Pode sempre mover para as predefinições de regras no separador de Monitor de desempenho (aparece depois de concluir a configuração do dia-0) e alterar o protocolo mais tarde. No caso de não interessado na capacidade de Monitor de rPerfomance, pode desativar a regra predefinida das predefinições de regras no separador de Monitor de desempenho. 

    ![Configuração de NPM](media/log-analytics-network-performance-monitor/npm-synthetic-transactions.png)
    
    **Vista de Monitor de ponto final de serviço** -a capacidade fornece testes pré-configuradas incorporados para monitorizar a conectividade de rede para o Office 365 e Dynamcis365 dos agentes. Escolha os serviços do Office 365 e Dynamcis365 que está interessado em monitorização marcando a caixa de verificação ao lado-los. Escolha os agentes a partir do qual pretende monitorizar, clicando no botão Adicionar agentes. Se não quiser utilizar esta capacidade ou pretende configurá-lo mais tarde, pode optar por ignorar esta e clique diretamente no **guardar** e **continuar** sem escolher nada.  

    ![Configuração de NPM](media/log-analytics-network-performance-monitor/npm-service-endpoint-monitor.png)

    **Vista de Monitor do ExpressRoute** -clique em de **detetar agora** botão para detetar todos os ExpressRoute peerings privadas que estão ligados a VNETs na subscrição do Azure ligado com esta área de trabalho de análise de registos.  


    >[!NOTE] 
    > A solução Deteta atualmente apenas ExpressRoute privada peerings. 

    >[!NOTE] 
    > Apenas os peerings privadas são detetados que estão ligados a VNETs associadas à subscrição ligada com esta área de trabalho de análise de registos. Se o ExpressRoute é ligado a VNETs fora da subscrição associada a esta área de trabalho, terá de criar uma área de trabalho de análise de registos nessas subscrições e utilizar NPM para monitorizar os peerings. 

    ![Configuração de NPM](media/log-analytics-network-performance-monitor/npm-express-route.png)

    Assim que a deteção estiver concluída, os peerings privados detetados estão listados na tabela.  

    ![Configuração de NPM](media/log-analytics-network-performance-monitor/npm-private-peerings.png)
    
    A monitorização para estes peerings são inicialmente no estado desativado. Clique em cada peering que está interessado na monitorização e configurar a monitorização para os mesmos a partir da vista de detalhes do lado direito (RHS).  Clique no botão para guardar a configuração de gravação. Consulte [ExpressRoute configurar monitorização]() para obter mais informações.  

    Assim que a configuração estiver concluída, que demora 30 minutos para uma hora para que os dados preencher. Enquanto a solução é agregar dados a partir da sua rede, consulte *solução requer configuração adicional* no mosaico de descrição geral do NPM. Depois dos dados são recolhidos e indexados, a descrição geral do mosaico alterações e informa-o resumo do Estado de funcionamento da sua rede. Em seguida, pode optar por editar a monitorização de nós em que se encontram instalados agentes OMS, bem como as sub-redes detetadas a partir do seu ambiente 

#### <a name="edit-monitoring-settings-for-subnets-and-nodes"></a>Editar as definições de monitorização sub-redes e nós 

Todas as sub-redes com, pelo menos, um agente instalado são apresentadas no separador sub-redes a página de configuração. 


Para ativar ou desativar a monitorização de determinada sub-redes 

1. Selecione ou desmarque a caixa junto a **ID de sub-rede** e, em seguida, certifique-se de que **utilização para a monitorização** está selecionado ou desmarcada, conforme adequado. Pode selecionar ou desmarcar várias sub-redes. Quando desativado, sub-redes não são monitorizados como os agentes foram atualizados para parar o envio de ping outros agentes. 
2. Escolha os nós que pretende monitorizar numa determinada sub-rede, selecionando a sub-rede da lista e mover nós necessários entre as listas de nós não monitorizados e monitorizados. Pode adicionar um **Descrição personalizada para** a sub-rede. 
3. Clique em **guardar** para guardar a configuração. 

#### <a name="choose-nodes-to-monitor"></a>Escolher nós para monitorizar

Todos os nós que tenha um agente instalado nos mesmos são listados no **nós** separador. 

1. Selecione ou desmarque os nós que pretende monitorizar ou parar a monitorização. 
2. Clique em **utilização para a monitorização**, ou limpá-lo, conforme apropriado. 
3. Clique em **Guardar**. 


Configure o capability(s) que está interessado: 
- Configurar [Monitor de desempenho](log-analytics-network-performance-monitor-performance-monitor.md#configuration)
- Configurar [Monitor de ponto final de serviço](log-analytics-network-performance-monitor-performance-monitor.md#configuration)
- Configurar [Monitor do ExpressRoute](log-analytics-network-performance-monitor-expressroute.md#configuration)

 

## <a name="data-collection-details"></a>Detalhes de recolha de dados
Monitor de desempenho de rede utiliza pacotes de handshake TCP SIN-SYNACK-ACK quando é escolhido TCP e resposta de eco ICMP do ICMP ECHO quando ICMP é escolhido como o protocolo para recolher informações de perda e latência. Traceroute também é utilizado para obter informações de topologia.

A tabela seguinte mostra os métodos de recolha de dados e outros detalhes sobre como os dados são recolhidos para o Monitor de desempenho de rede.

| Plataforma | Direcionar o agente | Agente do SCOM | Storage do Azure | SCOM necessário? | Dados de agente do SCOM enviados através do grupo de gestão | Frequência de recolha |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |Mensagens de eco handshakes/ICMP do TCP a cada cinco segundos, dados enviados a cada 3 minutos |
 

 
A solução utiliza transações sintéticas para avaliar o estado de funcionamento da rede. Agentes do OMS instalados em vários pontos no pacotes TCP do exchange de rede ou de eco ICMP (dependendo do protocolo selecionado para a monitorização) com outro. No processo de agentes saiba a perda de tempo e pacote reportadas round-trip, se aplicável. Periodicamente, cada agente também executa uma rota de rastreio por outros agentes para localizar todas as rotas vários na rede que deve ser testada. Utilizando estes dados, os agentes podem deduzir a latência de rede e números de perda de pacotes. Os testes são repetidos a cada cinco segundos e os dados são agregadas para um período de três minutos pelos agentes antes de o carregar para o serviço de análise de registos. 



>[!NOTE]
> Embora os agentes comunicam entre si com frequência, estas não geram significativas de tráfego de rede ao realizar os testes. Agentes baseiam-se apenas nos pacotes de handshake de TCP SIN-SYNACK-ACK para determinar a perda e latência – não existem dados trocados pacotes. Durante este processo, agentes comunicam entre si apenas quando necessário e a topologia de comunicação do agente está otimizada para reduzir o tráfego de rede.

## <a name="using-the-solution"></a>Utilizar a solução 

### <a name="npm-overview-tile"></a>Mosaico de descrição geral de NPM 

Depois de ativar a solução de Monitor de desempenho de rede, o mosaico de solução na página de descrição geral fornece uma descrição geral rápida do Estado de funcionamento de rede. 

 ![Mosaico de descrição geral NPM](media/log-analytics-network-performance-monitor/npm-overview-tile.png)

### <a name="network-performance-monitor-dashboard"></a>Dashboard de monitorização de desempenho de rede 

O **eventos de estado de funcionamento de rede superior** página fornece uma lista dos eventos mais recentes do Estado de funcionamento e alertas no sistema e a hora, uma vez que o evento foi Active Directory. Um evento de estado de funcionamento ou o alerta é gerado sempre que o valor da métrica escolhido (perda, latência, tempo de resposta ou utilização de largura de banda) para a regra de monitorização excede o limiar. 

O **Monitor de desempenho** página fornece um resumo do Estado de funcionamento das ligações de rede e ligações de sub-rede que está a ser monitorizados pela solução. O mosaico de topologia informa o número de caminhos de rede monitorizados na sua rede. Quando clica neste mosaico diretamente a navega para a vista de topologia. 

O **Monitor de ponto final de serviço** página fornece um resumo do Estado de funcionamento dos testes diferentes que criou. O mosaico de topologia informa o número de pontos finais a ser monitorizado. Quando clica neste mosaico diretamente a navega para a vista de topologia.

O **ExpressRoute Monitor** página fornece um resumo do Estado de funcionamento das várias ligações peering de ExpressRoute que está a ser monitorizados pela solução. O mosaico de topologia informa o número de caminhos de rede através de circuit(s) ExpressRoute a ser monitorizado na sua rede. Quando clica neste mosaico diretamente a navega para a vista de topologia.

O **consultas comuns** página contém um conjunto de consultas de pesquisa obter diretamente de dados de monitorização de rede não processado. Pode utilizar estas consultas como um ponto de partida para criar as suas próprias consultas para relatórios personalizados. 

![Dashboard NPM](media/log-analytics-network-performance-monitor/npm-dashboard.png)

 

### <a name="drill-down-for-depth"></a>desagregar profundidade 

Pode clicar em várias ligações no dashboard da solução para desagregar baixo mais aprofundado para qualquer área de interesse. Por exemplo, quando vir um alerta ou uma ligação de rede danificado apareça no dashboard, pode clicar para investigação mais aprofundada. É direcionado para uma página que apresenta uma lista de todas as ligações de sub-rede para a ligação de rede específico. Conseguir ver o estado de funcionamento, a latência e perda de cada hiperligação de sub-rede e rapidamente descobrir que ligações de sub-rede estão a causar o problema. Em seguida, pode clicar em **ver ligações de nó** para ver todas as ligações de nó para a ligação de sub-rede mau estado de funcionamento. Em seguida, pode ver ligações de nó para nó individuais e localizar as ligações de nó mau estado de funcionamento. 

Pode clicar em **topologia de vista** para visualizar a topologia de salto por salto de rotas entre os nós de origem e de destino. As rotas mau estado de funcionamento são apresentadas vermelho e pode ver a latência contribuída por cada salto, para que as possa identificar rapidamente o problema para uma determinada parte da rede. 

 

### <a name="network-state-recorder"></a>Gravador de estado de rede 

Cada vista apresenta um instantâneo do Estado de funcionamento do rede num determinado ponto no tempo. Por predefinição, é apresentado o estado mais recente. A barra na parte superior da página mostra o ponto no tempo para o qual o estado é apresentado. Pode optar por voltar atrás no tempo e ver o instantâneo do Estado de funcionamento do rede ao clicar na barra das ações. Também pode optar por ativar ou desativar a atualização automática para qualquer página, ao visualizar o estado mais recente. 

 ![Gravador de estado de rede](media/log-analytics-network-performance-monitor/network-state-recorder.png)

 

### <a name="trend-charts"></a>Gráficos de tendência 

Cada nível que lhe desagregar, pode ver a tendência de métrica aplicável – perda, latência, tempo de resposta, a utilização de largura de banda. Pode alterar o intervalo de tempo para a tendência utilizando o controlo de tempo na parte superior do gráfico. 

Gráficos de tendência mostram-lhe uma perspetiva de histórico do desempenho de uma métrica de desempenho. Alguns problemas de rede são transitórios natureza e seriam difíceis de detetar apenas ao observar o estado atual da rede. Isto acontece porque problemas podem superfície rapidamente e desaparecer antes que qualquer pessoa avisos, apenas a reaparecer tiver posteriormente no tempo. Tais problemas transitórios também podem ser difícil para os administradores de aplicação porque a superfície, muitas vezes, os problemas à medida que aumenta unexplained no tempo de resposta da aplicação, mesmo quando são apresentados todos os componentes da aplicação executar facilmente. 

Pode facilmente detetar esses tipos de problemas ao observar um gráfico de tendência onde aparece o problema como um pico de pedidos repentino na latência de rede ou perda de pacotes. Em seguida, pode investigar o problema utilizando o gravador de estado de rede para ver o instantâneo de rede e a topologia para esse ponto no tempo quando tinha Ocorreu o problema. 

 
![Gráficos de tendência](media/log-analytics-network-performance-monitor/trend-charts.png)
 

### <a name="topology-map"></a>Mapa de topologia 

NPM mostra a topologia de salto por salto de rotas entre a origem e destino ponto final, um mapa de topologia interativo. Pode ver o mapa de topologia, clicando no **topologia** mosaico no dashboard da solução ou clicando na **topologia de vista** ligação nas páginas desagregar.  

O mapa de topologia apresenta rotas quantos encontram-se entre a origem e destino e o que caminhos demorar os pacotes de dados. A latência contribuída por cada salto de rede também está visível. Todos os caminhos para os quais a latência total do caminho é superior ao limiar (configurar a regra de monitorização correspondente) são mostrados a vermelho.  

Ao clicar num nó ou paire sobre os mesmos no mapa de topologia, pode ver as propriedades do nó, como o endereço IP e FQDN. Clique num salto para ver o respetivo endereço IP. Pode identificar o salto de rede troublesome por dar por isso a latência que está a ser contribuída por este. Pode optar por filtrar rotas específicas utilizando os filtros no painel de ações expansíveis. Também pode simplificar as topologias de rede ao ocultar os saltos intermédios utilizando o controlo de deslize no painel de ações. Pode zoom-in ou fora do mapa de topologia, utilizando a roda do rato. 

Tenha em atenção que a topologia apresentada no mapa de topologia de camada 3 e não contém ligações e dispositivos de camada 2. 

 
![Mapa de topologia](media/log-analytics-network-performance-monitor/topology-map.png)
 

## <a name="log-analytics-search"></a>Pesquisa de análise do registo 

Todos os dados que é páginas graficamente expostas através do dashboard NPM e desagregar também está disponível nativamente no [análise de registos de pesquisa](log-analytics-log-search-new.md). Pode efetuar análises interativas dos dados no repositório, correlacionar dados de diferentes origens, criar alertas personalizados, criar vistas personalizadas e exportar os dados para o Excel, PowerBI ou uma ligação partilhável. A área de consultas comuns no dashboard tem algumas consultas útil que pode utilizar como ponto de partida para criar as suas próprias consultas e relatórios. 

 

## <a name="provide-feedback"></a>Enviar comentários 

**UserVoice** -pode publicar as suas ideias para funcionalidades de Monitor de desempenho de rede que pretende que sejam-nos para funcionar no. Visite a nossa [página do UserVoice](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring). 

**Associar o nosso coorte** -sempre estamos interessados em novos clientes aderir ao nosso coorte a ter. Como parte do mesmo, obter acesso atempado a novas funcionalidades e ajude-na melhorar o Monitor de desempenho de rede. Se estiver interessado em associar, preenchimento Escalamento isto [inquérito Rápido](https://aka.ms/npmcohort). 

## <a name="next-steps"></a>Passos Seguintes 
- Saiba mais sobre [Monitor de desempenho](log-analytics-network-performance-monitor-performance-monitor.md), [Monitor de ponto final de serviço](log-analytics-network-performance-monitor-performance-monitor.md), e [ExpressRoute Monitor](log-analytics-network-performance-monitor-expressroute.md). 
