---
title: "Solução de Monitor de desempenho no Log Analytics do Azure de rede | Microsoft Docs"
description: "A capacidade de Gestor de ExpressRoute no Monitor de desempenho de rede permite-lhe monitorizar conetividade ponto a ponto e o desempenho entre os escritórios e o Azure, através do ExpressRoute do Azure."
services: log-analytics
documentationcenter: 
author: abshamsft
manager: carmonm
editor: 
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: abshamsft
ms.openlocfilehash: 405bcd7d69e93f838d35b61be489464fcf55ee88
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/23/2018
---
# <a name="expressroute-manager"></a>Gestor de ExpressRoute

A capacidade de Gestor de ExpressRoute na [Monitor de desempenho de rede](log-analytics-network-performance-monitor.md) permite-lhe monitorizar conetividade ponto a ponto e o desempenho entre os escritórios e o Azure, através do ExpressRoute do Azure. Vantagens chaves são: 

- Deteção automática dos circuitos do ExpressRoute associados à subscrição 
- Controlo de utilização de largura de banda, perda e latência de circuito, o peering e a nível de VNet para o ExpressRoute 
- Deteção de topologia de rede do seu circuitos do ExpressRoute 

![ExpressRoute Monitor](media/log-analytics-network-performance-monitor/expressroute-intro.png)

## <a name="configuration"></a>Configuração 
Para abrir a configuração de Monitor de desempenho de rede, abra o [solução de Monitor de desempenho de rede](log-analytics-network-performance-monitor.md) e clique em de **configurar** botão.

### <a name="configure-nsg-rules"></a>Configurar as regras do NSG 
Para os servidores no Azure que estão a ser utilizados para a monitorização através do NPM, tem de configurar regras para permitir tráfego TCP na porta utilizada pelo NPM para transações sintéticas (NSG) do grupo de segurança de rede. A porta predefinida é 8084. Isto permite que o agente do OMS instalado na VM do Azure para comunicar com um local agente de monitorização. 

Para mais informações sobre o NSG, consulte [grupos de segurança de rede](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 

>[!NOTE]
> Certifique-se de que instalou os agentes (o agente de servidor no local e o agente do servidor do Azure) e ter a executar o script do PowerShell de EnableRules.ps1 antes de continuar com este passo. 

 
### <a name="discover-expressroute-peering-connections"></a>Detetar ligações de Peering do ExpressRoute 
 
1. Clique em de **ExpressRoute Peerings** vista.  
2. Clique em de **detetar agora** botão para detetar todos os ExpressRoute peerings privadas que estão ligados a VNETs na subscrição do Azure ligado com esta área de trabalho de análise de registos.  

>[!NOTE]  
> A solução Deteta atualmente apenas ExpressRoute privada peerings. 

>[!NOTE]  
> Apenas os peerings privadas são detetados que estão ligados a VNETs associadas à subscrição ligada com esta área de trabalho de análise de registos. Se o ExpressRoute é ligado a VNETs fora da subscrição associada a esta área de trabalho, terá de criar uma área de trabalho de análise de registos nessas subscrições e utilizar NPM para monitorizar os peerings. 

 ![Configurar o ExpressRoute Monitor](media/log-analytics-network-performance-monitor/expressroute-configure.png)
 
 Assim que a deteção estiver concluída, as ligações de peering privadas detetadas estão listadas na tabela. A monitorização para estes peerings inicialmente serão no estado desativado. 

### <a name="enable-monitoring-of-the-expressroute-peering-connections"></a>Ativar a monitorização das ligações de peering de ExpressRoute 

1. Clique em peering privado ligar-se de está interessado na monitorização.  
2. No painel de RHS, clique na caixa de verificação para **monitorizar este Peering**. 
3. Se pretender criar eventos de estado de funcionamento para esta ligação, em seguida, verifique **Ativar monitorização do Estado de funcionamento para este peering**. 
4. Escolha a monitorização de condições. Pode definir limiares personalizados para a geração de eventos de estado de funcionamento, escrevendo os valores de limiar. Sempre que o valor da condição ultrapassar o limiar selecionado para a ligação peering, é gerado um evento de estado de funcionamento. 
5. Clique em **adicionar agentes** para escolher os agentes de monitorização que pretende utilizar para esta ligação de peering de monitorização. Terá de se certificar de que adiciona agentes em ambas as extremidades da ligação, pelo menos um agente na VNET do Azure ligado a este peering, bem como a, pelo menos, um agente de no local ligada a este peering. 
6. Clique em **guardar** para guardar a configuração. 

![Configurar o ExpressRoute Monitor](media/log-analytics-network-performance-monitor/expressroute-configure-discovery.png)


Depois de ativar as regras e selecionar os valores e os agentes que pretende monitorizar, há um Aguarde aproximadamente 30-60 minutos para os valores começar a preencher e **ExpressRoute monitorização** mosaicos fique disponível. Depois de ver os mosaicos de monitorização, os circuitos ExpressRoute e recursos de ligação estão a ser monitorizados pelo NPM. 

>[!NOTE]
> Esta capacidade funciona fiável em áreas de trabalho que tem atualizado para o novo idioma de consulta.  

## <a name="walkthrough"></a>Instruções 

O dashboard de monitorização de desempenho de rede mostra uma descrição geral do Estado de funcionamento dos circuitos ExpressRoute e ligações de peering. 

![NPM Dashboard ExpressRoute](media/log-analytics-network-performance-monitor/npm-dashboard-expressroute.png) 

### <a name="circuits-list"></a>Lista de circuitos 

Para ver uma lista de todos os circuitos ExpressRoute monitorizados, clique no mosaico de circuitos do ExpressRoute. Pode selecionar um circuito e ver o estado de funcionamento, gráficos de tendência de perda de pacotes, utilização de largura de banda e latência. Os gráficos são interativos. Pode selecionar uma janela de tempo personalizadas para representar os gráficos. Pode arrastar o rato através de uma área no gráfico para ampliar e ver os pontos de dados de detalhado. 

![Lista de circuitos do ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-circuits.png) 

### <a name="trend-of-loss-latency-and-throughput"></a>Tendência de perda, débito e latência 

Os gráficos de largura de banda, a latência e perda são interativos. Pode ampliar em qualquer secção nestes gráficos, utilizar controlos de rato. Também pode ver a largura de banda, a latência e dados de perda de outros intervalos clicando data/hora, localizado abaixo do botão de ações no canto superior esquerdo. 

![Latência de ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-latency.png) 

### <a name="peerings-list"></a>Lista de Peerings 

Clicar no **privada Peerings** mosaico no dashboard apresenta uma lista de todas as ligações a redes virtuais através de peering privado. Aqui, pode selecionar um virtual ligação de rede e ver o estado de funcionamento, gráficos de tendência de perda de pacotes, utilização de largura de banda e latência. 

![Peerings do ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-peerings.png) 

### <a name="circuit-topology"></a>Topologia de circuito 

Para visualizar a topologia de circuito, clique no **topologia** mosaico. Isto leva-o para a vista de topologia de selecionado circuito ou peering. O diagrama de topologia fornece a latência de cada segmento na rede e cada salto de camada 3 é representado por um nó do diagrama. Clicar num salto de revela mais detalhes sobre o salto. Pode aumentar o nível de visibilidade para incluir saltos no local, movendo a barra do controlo de deslize abaixo **filtros**. Mover a barra do controlo de deslize para a esquerda ou direita, aumenta/diminuições o número de saltos no gráfico de topologia. A latência em cada segmento está visível, que permite isolamento mais rápido de segmentos de latência elevada na sua rede. 

![Topologia do ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-topology.png)  

### <a name="detailed-topology-view-of-a-circuit"></a>Vista detalhada da topologia de um circuito 

Esta vista mostra as ligações VNet. 

![VNet do ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-vnet.png)
 

### <a name="diagnostics"></a>Diagnóstico 

Monitor de desempenho de rede ajuda-o a diagnosticar problemas de conectividade várias do circuito. Alguns dos problemas estão listados abaixo 

**Circuito está inativo.** NPM notifica-o assim que a conectividade entre os recursos no local e as VNETs do Azure é perdida. Isto ajuda a ação proativa antes de receber Escalamentos de utilizador e a reduzir o tempo de indisponibilidade 

![Circuito de ExpressRoute para baixo](media/log-analytics-network-performance-monitor/expressroute-circuit-down.png)
 

**Tráfego não circulam circuito pretendido.** NPM pode notificá-lo sempre que o tráfego é inesperadamente não circulam no circuito de ExpressRoute pretendido. Isto pode acontecer se o circuito está inativo e o tráfego é que circulam pela rota de cópia de segurança, ou se houver um problema de encaminhamento. Estas informações ajudam proactivamente gerir problemas de configuração nas suas políticas de encaminhamento e certifique-se de que a rota mais segura e ideal é utilizada. 

 

**Tráfego não circulam circuito primário.** A capacidade de notifica-o quando o tráfego é que circulam pelo circuito de ExpressRoute secundário. Apesar de não verificará problemas de conectividade neste caso, mas a resolução de problemas proativamente os problemas com o circuito primário fazer com que melhor preparado. 

 
![Fluxo de tráfego do ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-traffic-flow.png)


**Degradação devido a picos de utilização.** Pode correlacionar a tendência de utilização de largura de banda com a tendência de latência para identificar se a degradação de carga de trabalho do Azure é devido a das horas de ponta na utilização de largura de banda ou não e tomar medidas em conformidade.  

![ExpressRoute Monitor](media/log-analytics-network-performance-monitor/expressroute-peak-utilization.png)

 

## <a name="next-steps"></a>Passos Seguintes
* [Pesquisar registos](log-analytics-log-searches.md) para ver registos de dados de desempenho de rede detalhada.
