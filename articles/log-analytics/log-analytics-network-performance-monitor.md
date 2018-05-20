---
title: Solução de Monitor de desempenho no Azure de rede | Microsoft Docs
description: Monitor de desempenho de rede no Azure ajuda-o a monitorizar o desempenho de redes, em tempo real, para detetar e localizar congestionamentos de desempenho de rede.
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
ms.openlocfilehash: a3c81d63593bb7bf268c1026064e34c3a363ef86
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="network-performance-monitor-solution-in-azure"></a>Solução de Monitor de desempenho no Azure de rede

![Símbolo de Monitor de desempenho de rede](./media/log-analytics-network-performance-monitor/npm-symbol.png)


Monitor de desempenho de rede é uma solução que ajuda a monitorizar o desempenho de rede entre vários pontos na sua infraestrutura de rede de monitorização de rede de baseado na nuvem híbrida. Também o ajuda a monitorizar a conectividade de rede para o serviço e os pontos finais de aplicação e monitorizar o desempenho do Azure ExpressRoute. 

Monitor de desempenho de rede Deteta problemas de rede, como tráfego blackholing, encaminhamento erros e problemas que métodos de monitorização de rede convencional não são capazes de detetar. A solução gera alertas e notifica-o quando um limiar é quebrado para uma ligação de rede. Também garante uma deteção atempada do problemas de desempenho de rede e localizes a origem do problema para um segmento de rede específico ou o dispositivo. 

Monitor de desempenho de rede oferece três capacidades abrangentes: 

* [Monitor de desempenho](log-analytics-network-performance-monitor-performance-monitor.md): pode monitorizar a conectividade de rede em implementações de nuvem e localizações no local, vários centros de dados e sucursais e aplicações com múltiplas camadas fundamentais ou micro-serviços. Com a monitorização de desempenho, pode detetar problemas de rede antes dos utilizadores queixarem.

* [Monitor de ponto final de serviço](log-analytics-network-performance-monitor-service-endpoint.md): pode monitorizar a conectividade dos seus utilizadores para os serviços que lhe interessam, determinar a infraestrutura de que está a ser o caminho e identificar qual ocorrem congestionamentos de rede. Pode saber sobre falhas antes dos utilizadores e consulte a localização exata dos problemas no seu caminho de rede. 

    Esta capacidade ajuda a efetuar testes com base em HTTP, HTTPS, TCP e ICMP para monitorizar no quase em tempo real ou historicamente a disponibilidade e o tempo de resposta do seu serviço. Também pode monitorizar a contribuição da rede em perda de pacotes e latência. Com um mapa de topologia de rede, pode isolar abrandamentos de rede. Pode identificar oportunidades de problema que ocorrem ao longo do caminho de rede a partir do nó para o serviço, com dados de latência em cada hop. Com testes incorporados, pode monitorizar a conectividade de rede para o Office 365 e o Dynamics CRM sem qualquer preconfiguration. Com esta capacidade, pode monitorizar a conectividade de rede para qualquer ponto final compatível com o TCP, tais como Web sites, aplicações de SaaS, PaaS e bases de dados SQL.

* [Monitor de ExpressRoute](log-analytics-network-performance-monitor-expressroute.md): monitorizar a conetividade ponto a ponto e o desempenho entre os escritórios e o Azure, através do ExpressRoute do Azure.  

Obter mais informações sobre as diversas capacidades suportadas pelo [Monitor de desempenho de rede](https://docs.microsoft.com/azure/networking/network-monitoring-overview) está disponível online.
 
## <a name="supported-regions"></a>Regiões suportadas
NPM pode monitorizar a conectividade entre redes e aplicações em qualquer parte do mundo, a partir de uma área de trabalho que está alojado das regiões do seguintes:
* Europa Ocidental
* EUA Centro-Oeste
* EUA Leste
* Sudeste Asiático
* Austrália Leste-Sul
* RU-Sul
* E.u. a Government Virginia

A lista de regiões suportadas para o Monitor de ExpressRoute está disponível no [documentação](https://docs.microsoft.com/azure/expressroute/how-to-npm?utm_swu=8117#regions).


## <a name="set-up-and-configure"></a>Configurar

### <a name="install-and-configure-agents"></a>Instalar e configurar agentes 

Utilize os processos de básico para instalar agentes em [computadores Windows ligar ao Log Analytics do Azure](log-analytics-windows-agents.md) e [estabelecer a ligação do Operations Manager ao Log Analytics](log-analytics-om-agents.md).

### <a name="where-to-install-the-agents"></a>Onde pretende instalar os agentes 

* **Monitor de desempenho**: agentes de instalar o Operations Management Suite em pelo menos um nó ligado a cada sub-rede de que pretende monitorizar a conectividade de rede com outros sub-redes.

    Para monitorizar uma ligação de rede, instale agentes em ambos os pontos finais dessa ligação. Se não tiver a certeza sobre a topologia da sua rede, instale os agentes em servidores com cargas de trabalho críticas entre os quais pretende monitorizar o desempenho da rede. Por exemplo, se pretender monitorizar a ligação de rede entre um servidor web e um servidor que executa o SQL Server, instale um agente em ambos os servidores. Os agentes monitorizam a conectividade de rede (ligações) entre anfitriões, não os anfitriões próprios. 

* **Monitor de ponto final de serviço**: instalar um agente do Operations Management Suite em cada nó a partir do qual pretende monitorizar a conectividade de rede para ponto final do serviço. Um exemplo é se pretende monitorizar a conectividade de rede para o Office 365 de sites do office com a etiqueta O1, O2 e O3. Instale o agente do Operations Management Suite em pelo menos um nó do O1, O2 e O3. 

* **Monitor de ExpressRoute**: instalar pelo menos um agente do Operations Management Suite na sua rede virtual do Azure. Instale também, pelo menos, um agente no seu sub-rede no local, que está ligada através do peering privado do ExpressRoute.  

### <a name="configure-operations-management-suite-agents-for-monitoring"></a>Configurar os agentes do Operations Management Suite para monitorização 

Monitor de desempenho de rede utiliza transações sintéticas para monitorizar o desempenho de rede entre os agentes de origem e de destino. Pode escolher entre TCP e ICMP como o protocolo para a monitorização em capacidades de Monitor de desempenho e Monitor do ponto final de serviço. TCP é utilizado para o Monitor do ExpressRoute. Certifique-se de que a firewall permite a comunicação entre os agentes do Operations Management Suite para monitorizar no protocolo que escolher. 

* **Protocolo TCP**: Se optar por TCP como protocolo para monitorização, abra a porta de firewall em agentes utilizado para o Monitor de desempenho de rede e o Monitor de ExpressRoute para se certificar de que os agentes podem ligar-se entre si. Para abrir a porta, execute o [EnableRules.ps1](https://aka.ms/npmpowershellscript) script do PowerShell sem quaisquer parâmetros numa janela do PowerShell com privilégios administrativos.

    O script cria as chaves de registo necessárias para a solução. Também cria regras de Firewall do Windows para permitir que os agentes para criar ligações TCP entre si. As chaves de registo criadas pelo script Especifique se pretende registar os registos de depuração e o caminho para o ficheiro de registos. O script também define a porta TCP de agente utilizada para comunicação. Os valores para estas chaves são configurados automaticamente pelo script. Não altere manualmente estas chaves. A porta aberta por predefinição é 8084. Pode utilizar uma porta personalizada, fornecendo o portNumber de parâmetro para o script. Utilize a mesma porta em todos os computadores em que o script é executado. 

    >[!NOTE]
    > O script configura apenas Firewall do Windows localmente. Se tiver uma firewall de rede, certifique-se de que permite o tráfego destinado a porta TCP utilizada pelo Monitor de desempenho de rede.

    >[!NOTE]
    > Não precisa de executar o [EnableRules.ps1](https://aka.ms/npmpowershellscript ) script do PowerShell para o Monitor de ponto final de serviço.

    

* **Protocolo ICMP**: Se optar por ICMP como o protocolo para monitorização, ative as seguintes regras de firewall de forma fiável utilizar ICMP:
    
   ```
   netsh advfirewall firewall add rule name="NPMDICMPV4Echo" protocol="icmpv4:8,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6Echo" protocol="icmpv6:128,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV4DestinationUnreachable" protocol="icmpv4:3,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6DestinationUnreachable" protocol="icmpv6:1,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV4TimeExceeded" protocol="icmpv4:11,any" dir=in action=allow 
   netsh advfirewall firewall add rule name="NPMDICMPV6TimeExceeded" protocol="icmpv6:3,any" dir=in action=allow 
   ```
 

### <a name="configure-the-solution"></a>Configurar a solução 

1. Adicionar a solução de Monitor de desempenho de rede para a sua área de trabalho a partir de [do Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview). Também pode utilizar o processo descrito no [soluções de análise de registos adicionar da galeria do soluções](log-analytics-add-solutions.md). 
2. Abra a área de trabalho de análise de registos e selecione o **descrição geral** mosaico. 
3. Selecione o **Monitor de desempenho de rede** mosaico com a mensagem *solução requer configuração adicional*.

   ![Mosaico de Monitor de desempenho de rede](media/log-analytics-network-performance-monitor/npm-config.png)

4. No **configuração** página, verá a opção para instalar agentes do Operations Management Suite e configurar os agentes para monitorização no **definições comuns** vista. Como explicado anteriormente, se tiver instalado e configurado os agentes do Operations Management Suite, selecione o **configuração** vista para configurar a capacidade de que pretende utilizar. 

   **Monitor de desempenho**: escolher o protocolo a utilizar para transações sintéticas no **predefinido** regra de Monitor de desempenho e selecione **Guardar & continuar**. Esta seleção do protocolo contém apenas para a regra predefinida gerada pelo sistema. Tem de escolher o protocolo sempre que cria uma regra de Monitor de desempenho explicitamente. Pode sempre mover para o **predefinido** regra definições no **Monitor de desempenho** separador (que aparece depois de concluir a configuração do dia-0) e o protocolo de alteração mais tarde. Se não pretender que a capacidade de Monitor de rPerfomance, pode desativar a regra predefinida do **predefinido** regra definições no **Monitor de desempenho** separador.

   ![Vista de Monitor de desempenho](media/log-analytics-network-performance-monitor/npm-synthetic-transactions.png)
    
   **Monitor de ponto final de serviço**: A capacidade fornece testes pré-configuradas incorporados para monitorizar a conectividade de rede para o Office 365 e Dynamics 365 dos agentes. Escolha os serviços do Office 365 e Dynamics 365 que pretende monitorizar, selecionando as caixas de verificação ao lado-los. Para escolher os agentes a partir do qual pretende monitorizar, selecione **adicionar agentes**. Se não quiser utilizar esta capacidade ou pretende configurá-lo mais tarde, não escolha nada e selecione **Guardar & continuar**.

   ![Vista de Monitor de ponto final de serviço](media/log-analytics-network-performance-monitor/npm-service-endpoint-monitor.png)

   **Monitor de ExpressRoute**: selecione **detetar agora** para detetar todos os ExpressRoute peerings privadas que estão ligados a redes virtuais na subscrição do Azure ligado com esta área de trabalho de análise de registos. 

   >[!NOTE] 
   > A solução Deteta atualmente apenas ExpressRoute privada peerings. 

   >[!NOTE] 
   > Apenas são detetados peerings privadas que estão ligados a redes virtuais associadas à subscrição ligada com esta área de trabalho de análise de registos. Se o ExpressRoute é ligado a redes virtuais fora da subscrição associado a esta área de trabalho, crie uma área de trabalho de análise de registos nessas subscrições. Utilize o Monitor de desempenho de rede para monitorizar os peerings.

   ![Vista de Monitor do ExpressRoute](media/log-analytics-network-performance-monitor/npm-express-route.png)

   Depois de concluída a deteção, os peerings privados detetados estão listados na tabela. 

   ![Página de configuração de Monitor de desempenho de rede](media/log-analytics-network-performance-monitor/npm-private-peerings.png)
    
A monitorização para estes peerings inicialmente está num estado desativado. Selecione cada peering que pretende monitorizar e configurar a monitorização para as mesmas da vista de detalhes no lado direito. Selecione **guardar** para guardar a configuração. Para obter mais informações, consulte o artigo "ExpressRoute configurar monitorização". 

Após a conclusão do programa de configuração, que demora 30 minutos para uma hora para que os dados preencher. Enquanto a solução agrega os dados da sua rede, consulte a mensagem *solução requer configuração adicional* no Monitor de desempenho de rede **descrição geral** mosaico. Depois dos dados são recolhidos e indexados, de **descrição geral** mosaico alterações e informa-o estado de funcionamento de rede num resumo. Em seguida, pode editar a monitorização de nós em que o Operations Management Suite os agentes estiverem instalados, bem como as sub-redes detetadas a partir do seu ambiente.

#### <a name="edit-monitoring-settings-for-subnets-and-nodes"></a>Editar as definições de monitorização sub-redes e nós 

Todas as sub-redes com, pelo menos, um agente instalado são listadas no **sub-redes** separador na página de configuração. 


Para ativar ou desativar a monitorização de determinada sub-redes:

1. Selecione ou desmarque a caixa de verificação junto a **ID de sub-rede**. Em seguida, certifique-se de que **utilização para a monitorização** está selecionado ou desmarcada, conforme adequado. Pode selecionar ou desmarcar várias sub-redes. Quando desativado, sub-redes não são monitorizados e os agentes foram atualizados para parar o envio de ping outros agentes. 
2. Escolha os nós que pretende monitorizar numa determinada sub-rede. Selecione a sub-rede da lista e mover nós necessários entre as listas que contêm nós não monitorizados e monitorizados. Pode adicionar uma descrição personalizada para a sub-rede.
3. Selecione **guardar** para guardar a configuração. 

#### <a name="choose-nodes-to-monitor"></a>Escolher nós para monitorizar

Todos os nós que tenha um agente instalado nos mesmos são listados no **nós** separador. 

1. Selecione ou desmarque os nós que pretende monitorizar ou parar a monitorização. 
2. Selecione **utilização para a monitorização**, ou limpá-lo, conforme apropriado. 
3. Selecione **Guardar**. 


Configure as capacidades que pretende:

- [Monitor de desempenho](log-analytics-network-performance-monitor-performance-monitor.md#configuration)
- [Monitor de ponto final de serviço](log-analytics-network-performance-monitor-performance-monitor.md#configuration)
- [Monitor do ExpressRoute](log-analytics-network-performance-monitor-expressroute.md#configuration)

 

## <a name="data-collection-details"></a>Detalhes de recolha de dados
Para recolher informações de perda e latência, o Monitor de desempenho de rede utiliza pacotes de handshake de TCP SIN-SYNACK-ACK quando escolhe o TCP como protocolo. Monitor de desempenho de rede utiliza a resposta de eco ICMP do ICMP ECHO quando escolhe ICMP como o protocolo. Rota de rastreio também é utilizada para obter informações de topologia.

A tabela seguinte mostra os métodos de recolha de dados e outros detalhes sobre como os dados são recolhidos para o Monitor de desempenho de rede.

| Plataforma | Direcionar o agente | Agente do System Center Operations Manager | Storage do Azure | O Operations Manager necessárias? | Dados de agente do Operations Manager enviados através do grupo de gestão | Frequência da recolha |
| --- | --- | --- | --- | --- | --- | --- |
| Windows | &#8226; | &#8226; |  |  |  |Mensagens de eco handshakes/ICMP do TCP a cada cinco segundos, dados enviados a cada 3 minutos |
 

 
A solução utiliza transações sintéticas para avaliar o estado de funcionamento da rede. Agentes do Operations Management Suite instalada em vários pontos no pacotes TCP do exchange de rede ou eco de ICMP com outro. Se os agentes utilizam pacotes TCP ou eco de ICMP depende o protocolo que selecionou para monitorização. No processo de agentes saiba a perda de tempo e pacote reportadas round-trip, se aplicável. Periodicamente, cada agente também executa uma rota de rastreio por outros agentes para localizar todas as rotas vários na rede que deve ser testada. Utilizando estes dados, os agentes podem deduzir a latência de rede e números de perda de pacotes. Os testes são repetidos a cada cinco segundos. Dados são agregados durante cerca de três minutos pelos agentes antes de ser carregado para o serviço de análise de registos.



>[!NOTE]
> Embora os agentes comunicam entre si com frequência, estas não geram significativo tráfego de rede ao realizar os testes. Agentes baseiam-se apenas nos pacotes de handshake de TCP SIN-SYNACK-ACK para determinar a perda e latência. Não existem pacotes de dados são trocados. Durante este processo, os agentes de comunicarem entre si apenas quando necessário. A topologia de comunicação do agente está otimizada para reduzir o tráfego de rede.

## <a name="use-the-solution"></a>Utilizar a solução 

### <a name="network-performance-monitor-overview-tile"></a>Mosaico de descrição geral do Monitor de desempenho de rede 

Depois de ativar a solução de Monitor de desempenho de rede, a solução de mosaico do **descrição geral** página fornece uma descrição geral rápida do Estado de funcionamento de rede. 

 ![Mosaico de descrição geral do Monitor de desempenho de rede](media/log-analytics-network-performance-monitor/npm-overview-tile.png)

### <a name="network-performance-monitor-dashboard"></a>Dashboard de monitorização de desempenho de rede 

* **Principais eventos de estado de funcionamento de rede**: esta página fornece uma lista dos eventos mais recentes do Estado de funcionamento e alertas no sistema e a hora, uma vez que os eventos foram Active Directory. Um evento de estado de funcionamento ou o alerta é gerado sempre que o valor da métrica escolhido (perda, latência, tempo de resposta ou utilização de largura de banda) para a regra de monitorização excede o limiar. 

* **Monitor de ExpressRoute**: esta página fornece resumos de estado de funcionamento a solução para as ligações de peering de ExpressRoute vários monitores. O **topologia** mosaico mostra o número de caminhos de rede através dos circuitos ExpressRoute que são monitorizados na sua rede. Selecione este mosaico para ir para o **topologia** vista.

* **Monitor de ponto final de serviço**: esta página fornece resumos de estado de funcionamento para os testes de diferentes que criou. O **topologia** mosaico mostra o número de pontos finais que são monitorizadas. Selecione este mosaico para ir para o **topologia** vista.

* **Monitor de desempenho**: esta página fornece resumos do Estado de funcionamento para o **rede** ligações e **sub-rede** ligações que monitoriza a solução. O **topologia** mosaico mostra o número de caminhos de rede que são monitorizadas na sua rede. Selecione este mosaico para ir para o **topologia** vista. 

* **Consultas comuns**: esta página contém um conjunto de consultas de pesquisa obter diretamente de dados de monitorização de rede não processado. Pode utilizar estas consultas como um ponto de partida para criar as suas próprias consultas para relatórios personalizados. 

   ![Dashboard de monitorização de desempenho de rede](media/log-analytics-network-performance-monitor/npm-dashboard.png)

 

### <a name="drill-down-for-depth"></a>desagregar profundidade 

Pode selecionar várias ligações no dashboard da solução para desagregar baixo mais aprofundado para qualquer área de interesse. Por exemplo, quando for apresentada uma ligação de rede danificado apareça no dashboard ou de um alerta, selecione-o para investigação mais aprofundada. Uma página apresenta uma lista de todas as ligações de sub-rede para a ligação de rede específico. Pode ver a perda, a latência e o estado de funcionamento de cada ligação de sub-rede. Pode rapidamente descobrir qual ligação sub-rede provoca problemas. Selecione **ver ligações de nó** para ver todas as ligações de nó para a ligação de sub-rede mau estado de funcionamento. Em seguida, pode ver ligações de nó para nó individuais e localizar as ligações de nó mau estado de funcionamento. 

Selecione **topologia de vista** para visualizar a topologia de salto por salto de rotas entre os nós de origem e de destino. As rotas mau estado de funcionamento aparecem vermelho. Pode ver a latência contribuída por cada salto, para que as possa identificar rapidamente o problema para uma determinada parte da rede.

 

### <a name="network-state-recorder-control"></a>Controlo do gravador de estado de rede

Cada vista apresenta um instantâneo do Estado de funcionamento do rede num determinado ponto no tempo. Por predefinição, é apresentado o estado mais recente. A barra na parte superior da página mostra o ponto no tempo para o qual o estado é apresentado. Para ver um instantâneo do Estado de funcionamento do rede a uma hora anterior, selecione **ações**. Também pode ativar ou desativar a atualização automática para qualquer página enquanto ver o estado mais recente. 

 ![Gravador de estado de rede](media/log-analytics-network-performance-monitor/network-state-recorder.png)

 

### <a name="trend-charts"></a>Gráficos de tendência 

Cada nível que lhe desagregar, pode ver a tendência da métrica aplicável. Pode ser perda, latência, tempo de resposta ou utilização de largura de banda. Para alterar o intervalo de tempo para a tendência, utilize o controlo de tempo na parte superior do gráfico. 

Gráficos de tendência mostram-lhe uma perspetiva de histórico do desempenho de uma métrica de desempenho. Alguns problemas de rede são transitórios natureza e são difíceis de detetar ao observar o estado atual da rede. Problemas podem superfície rapidamente e desaparecer antes que qualquer pessoa avisos, apenas a reaparecer tiver posteriormente no tempo. Tais problemas transitórios também podem ser difícil para os administradores de aplicação. Os problemas, muitas vezes, apareçam à medida que aumenta unexplained no tempo de resposta da aplicação, mesmo quando todos os componentes da aplicação são apresentados executar facilmente. 

Pode facilmente detetar estes tipos de problemas ao observar um gráfico de tendência. É apresentado o problema como um pico de pedidos repentino na latência de rede ou perda de pacotes. Para investigar o problema, utilize o controlo de gravador de estado de rede para ver o instantâneo de rede e a topologia para esse ponto no tempo quando ocorreu o problema.

 
![Gráficos de tendência](media/log-analytics-network-performance-monitor/trend-charts.png)
 

### <a name="topology-map"></a>Mapa de topologia 

Monitor de desempenho de rede mostra a topologia de salto por salto de rotas entre o ponto final de origem e de destino num mapa de topologia interativo. Para ver o mapa de topologia, selecione o **topologia** mosaico no dashboard da solução. Também pode selecionar o **topologia de vista** ligação nas páginas desagregar. 

O mapa de topologia apresenta rotas quantos encontram-se entre a origem e destino e o que caminhos demorar os pacotes de dados. A latência contribuída por cada salto de rede também está visível. Todos os caminhos para os quais a latência total do caminho é superior ao limiar (configurar a regra de monitorização correspondente) são mostrados a vermelho. 

Quando seleciona um nó ou paire sobre os mesmos no mapa de topologia, pode ver as propriedades de nó, tais como endereço IP e FQDN. Selecione um salto para ver o respetivo endereço IP. Pode identificar o salto de rede troublesome por dar por isso a latência que contribui. Para filtrar rotas específicas, utilize os filtros no painel de ações expansíveis. Para simplificar as topologias de rede, ocultar os saltos intermédios utilizando o controlo de deslize no painel de ações. Pode ampliar ou reduzir fora do mapa de topologia, utilizando a roda do rato. 

É a topologia de camada 3 e não contém ligações de dispositivos de camada 2 e a topologia apresentada no mapa. 

 
![Mapa de topologia](media/log-analytics-network-performance-monitor/topology-map.png)
 

## <a name="log-analytics-search"></a>Análise de registos de pesquisa 

Todos os dados que é páginas graficamente expostas através do dashboard de monitorização de desempenho de rede e desagregar também está disponível nativamente no [análise de registos de pesquisa](log-analytics-log-search-new.md). Pode efetuar análises interativas dos dados no repositório e correlacionar dados de diferentes origens. Também pode criar alertas personalizados e vistas e exportar os dados para Excel, Power BI ou uma ligação partilhável. O **consultas comuns** área no dashboard tem algumas consultas útil que pode utilizar como ponto de partida para criar as suas próprias consultas e relatórios. 

## <a name="alerts"></a>Alertas

Monitor de desempenho de rede utiliza as capacidades de alertas de [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts).

Isto significa que todos os alertas é gerida utilizando [grupos ação](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups#overview).  

Se for um utilizador NPM, criar um alerta através do OMS: 
1. Verá uma ligação que irá redirecionar para o Portal do Azure. Clique nele para aceder ao portal.
2. Clique no mosaico da solução de Monitor de desempenho de rede. 
3. Navegue para configurar.  
4. Selecione o teste de que pretende criar um alerta e siga o abaixo mencionado passos.

Se for um utilizador NPM, criar um alerta através do Portal do Azure:  
1. Pode optar por introduzir o seu correio eletrónico diretamente ou pode optar por criar alertas através de grupos de ação.
2. Se optar por introduzir diretamente o seu correio eletrónico, grupo de uma ação com o nome **NPM E-Mail ActionGroup** é criado e o id de correio eletrónico é adicionado a esse grupo de ação.
3. Se optar por utilizar grupos de ação, terá de selecionar um grupo de ação criado anteriormente. Pode saber como criar um grupo de ação [aqui.](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups#create-an-action-group-by-using-the-azure-portal) 
4. Depois do alerta foi criado com êxito, pode utilizar a ligação de gerir alertas para gerir os alertas. 

##<a name="pricing"></a>Preços

Estão disponíveis informações sobre os preços [online](log-analytics-network-performance-monitor-pricing-faq.md).

## <a name="provide-feedback"></a>Enviar comentários 

* **UserVoice:** pode publicar as suas ideias para funcionalidades de Monitor de desempenho de rede que pretende que sejam-nos para funcionar no. Visite o [página do UserVoice](https://feedback.azure.com/forums/267889-log-analytics/category/188146-network-monitoring). 

* **Associar o nosso coorte:** sempre estamos interessados em novos clientes aderir ao nosso coorte a ter. Como parte do mesmo, obter acesso atempado a novas funcionalidades e uma oportunidade de ajudar-na melhorar o Monitor de desempenho de rede. Se estiver interessado em associar, preencha este [inquérito Rápido](https://aka.ms/npmcohort). 

## <a name="next-steps"></a>Passos Seguintes 
Saiba mais sobre [Monitor de desempenho](log-analytics-network-performance-monitor-performance-monitor.md), [Monitor de ponto final de serviço](log-analytics-network-performance-monitor-performance-monitor.md), e [ExpressRoute Monitor](log-analytics-network-performance-monitor-expressroute.md). 
