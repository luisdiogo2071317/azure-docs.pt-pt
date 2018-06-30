---
title: Solução de Monitor de desempenho no Log Analytics do Azure de rede | Microsoft Docs
description: Utilize a capacidade de Gestor de ExpressRoute no Monitor de desempenho de rede para monitorizar conetividade ponto a ponto e o desempenho entre os escritórios e o Azure, através do ExpressRoute do Azure.
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
ms.topic: conceptual
ms.date: 02/20/2018
ms.author: abshamsft
ms.component: na
ms.openlocfilehash: 55308c2f144ea90636fb477f82c19fd3f8276af5
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131133"
---
# <a name="expressroute-manager"></a>Gestor de ExpressRoute

Pode utilizar a capacidade de Azure ExpressRoute Manager na [Monitor de desempenho de rede](log-analytics-network-performance-monitor.md) para monitorizar a conetividade ponto a ponto e o desempenho entre os escritórios e o Azure, através do ExpressRoute do Azure. Vantagens chaves são: 

- Circuitos Autodetection do ExpressRoute associados à subscrição.
- Controlo de utilização de largura de banda, perda e latência de circuito, peering e nível de rede Virtual do Azure para o ExpressRoute.
- Deteção de topologia de rede do seu circuitos do ExpressRoute.

![Monitor do ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-intro.png)

## <a name="configuration"></a>Configuração 
Para abrir a configuração de Monitor de desempenho de rede, abra o [solução de Monitor de desempenho de rede](log-analytics-network-performance-monitor.md) e selecione **configurar**.

### <a name="configure-network-security-group-rules"></a>Configurar regras de grupo de segurança de rede 
Para os servidores no Azure que são utilizados para a monitorização através do Monitor de desempenho de rede, configure regras de grupo (NSG) de segurança de rede para permitir tráfego TCP na porta utilizada pelo Monitor de desempenho de rede para transações sintéticas. A porta predefinida é 8084. Esta configuração permite que o agente do Operations Management Suite instalado em VMs do Azure para comunicar com um local agente de monitorização. 

Para obter mais informações sobre NSGs, consulte [grupos de segurança de rede](../virtual-network/manage-network-security-group.md). 

>[!NOTE]
> Antes de continuar com este passo, instale o agente de servidor no local e o agente do servidor do Azure e execute o script de EnableRules.ps1 PowerShell. 

 
### <a name="discover-expressroute-peering-connections"></a>Detetar ligações de peering do ExpressRoute 
 
1. Selecione o **ExpressRoute Peerings** vista.
2. Selecione **detetar agora** para detetar todos os ExpressRoute peerings privadas que estão ligados a redes virtuais na subscrição do Azure ligado com esta área de trabalho do Log Analytics do Azure.

    >[!NOTE]
    > A solução Deteta atualmente apenas ExpressRoute privada peerings. 

    >[!NOTE]
    > Apenas são detetados peerings privadas ligados a redes virtuais associadas à subscrição ligada com esta área de trabalho de análise de registos. Se o ExpressRoute é ligado a redes virtuais fora da subscrição associado a esta área de trabalho, crie uma área de trabalho de análise de registos nessas subscrições. Em seguida, utilize o Monitor de desempenho de rede para monitorizar esses peerings. 

    ![Configuração de Monitor do ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-configure.png)
 
 Depois de concluída a deteção, as ligações de peering privadas detetadas estão listadas na tabela. A monitorização para estes peerings inicialmente está num estado desativado. 

### <a name="enable-monitoring-of-the-expressroute-peering-connections"></a>Ativar a monitorização das ligações de peering de ExpressRoute 

1. Selecione a ligação de peering privada que pretende monitorizar.
2. No painel à direita, selecione o **monitorizar este Peering** caixa de verificação. 
3. Se pretender criar eventos de estado de funcionamento para esta ligação, selecione **Ativar monitorização do Estado de funcionamento para este peering**. 
4. Escolha a monitorização de condições. Pode definir limiares personalizados para a geração de eventos de estado de funcionamento, introduzindo os valores de limiar. Sempre que o valor da condição ultrapassar o limiar selecionado para a ligação peering, é gerado um evento de estado de funcionamento. 
5. Selecione **adicionar agentes** para escolher os agentes de monitorização que pretende utilizar para esta ligação de peering de monitorização. Certifique-se de que adiciona agentes em ambas as extremidades da ligação. Precisa de, pelo menos, um agente na rede virtual ligado a este peering. Também precisa de, pelo menos, um agente de no local ligado a este peering. 
6. Selecione **guardar** para guardar a configuração. 

   ![Configuração de monitorização do ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-configure-discovery.png)


Depois de ativar as regras e selecionados os valores e os agentes, aguarde 30 a 60 minutos para que os valores preencher e **ExpressRoute monitorização** mosaicos a apresentar. Quando vir os mosaicos de monitorização, os circuitos ExpressRoute e recursos de ligação são agora monitorizados pelo Monitor de desempenho de rede. 

>[!NOTE]
> Esta capacidade funciona consistentemente em áreas de trabalho que tem atualizado para o novo idioma de consulta.

## <a name="walkthrough"></a>Instruções 

O dashboard de monitorização de desempenho de rede mostra uma descrição geral do Estado de funcionamento dos circuitos ExpressRoute e ligações de peering. 

![Dashboard de monitorização de desempenho de rede](media/log-analytics-network-performance-monitor/npm-dashboard-expressroute.png) 

### <a name="circuits-list"></a>Lista de circuitos 

Para ver uma lista de todos os circuitos ExpressRoute monitorizados, selecione o mosaico de circuitos do ExpressRoute. Pode selecionar um circuito e ver o estado de funcionamento, gráficos de tendência de perda de pacotes, utilização de largura de banda e latência. Os gráficos são interativos. Pode selecionar uma janela de tempo personalizadas para representar os gráficos. Arraste o rato sobre uma área de gráfico para ampliar e ver os pontos de dados de detalhado. 

![Lista de circuitos do ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-circuits.png) 

### <a name="trends-of-loss-latency-and-throughput"></a>Tendências de perda, débito e latência 

Os gráficos de utilização, a latência e perda de largura de banda são interativos. Pode ampliar a qualquer secção nestes gráficos ao utilizar controlos de rato. Também pode ver a largura de banda, a latência e dados de perda para outros intervalos. No canto superior esquerdo sob o **ações** botão, selecione **data/hora**. 

![Latência de ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-latency.png) 

### <a name="peerings-list"></a>Lista de Peerings 

Para colocar uma lista de todas as ligações a redes virtuais através de peering privado, selecione o **privada Peerings** mosaico no dashboard. Aqui, pode selecionar um virtual ligação de rede e ver o estado de funcionamento, gráficos de tendência de perda de pacotes, utilização de largura de banda e latência. 

![Peerings do ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-peerings.png) 

### <a name="circuit-topology"></a>Topologia de circuito 

Para visualizar a topologia de circuito, selecione o **topologia** mosaico. Esta ação leva-o para a vista de topologia do selecionado circuito ou peering. O diagrama de topologia fornece a latência de cada segmento na rede e cada salto de camada 3 é representado por um nó do diagrama. Selecionar um salto de revela mais detalhes sobre o salto. Para aumentar o nível de visibilidade para incluir saltos no local, desloque o controlo de deslize em **filtros**. Mover a barra do controlo de deslize para a esquerda ou direita aumenta ou diminui o número de saltos no gráfico de topologia. A latência em cada segmento está visível, que permite isolamento mais rápido de segmentos de latência alta na sua rede. 

![Topologia do ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-topology.png)

### <a name="detailed-topology-view-of-a-circuit"></a>Vista de topologia de detalhado de um circuito 

Esta vista mostra as ligações de rede virtual. 

![Ligações de rede virtual do ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-vnet.png)
 

### <a name="diagnostics"></a>Diagnóstico 

Monitor de desempenho de rede ajuda-o a diagnosticar problemas de conectividade várias do circuito. Alguns dos problemas estão listados aqui. 

**Circuito está inativo.** Monitor de desempenho de rede notifica-o assim que a conectividade entre os recursos no local e redes virtuais do Azure é perdida. Esta notificação ajuda-o a ação proativa antes de receber Escalamentos de utilizador e reduzir o período de indisponibilidade.

![O circuito de ExpressRoute está inativo](media/log-analytics-network-performance-monitor/expressroute-circuit-down.png)
 

**Tráfego não circulam circuito pretendido.** Monitor de desempenho de rede notifica-o sempre que o tráfego não é que circulam pelo circuito de ExpressRoute pretendido. Este problema pode ocorrer se o circuito está inativo e o tráfego é que circulam pela rota de cópia de segurança. Também pode acontecer se houver um problema de encaminhamento. Estas informações ajudam proactivamente gerir problemas de configuração nas suas políticas de encaminhamento e certifique-se de que a rota mais segura e ideal é utilizada. 

 

**Tráfego não circulam circuito primário.** Monitor de desempenho de rede notifica-o quando o tráfego é que circulam pelo circuito de ExpressRoute secundário. Apesar de não ter quaisquer problemas de conectividade, neste caso, proativamente resolução de problemas com o circuito primário torna a melhor preparado. 

 
![Fluxo de tráfego do ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-traffic-flow.png)


**Degradação devido a picos de utilização.** Pode correlacionar a tendência de utilização de largura de banda com a tendência de latência para identificar se a degradação de carga de trabalho do Azure é devido a das horas de ponta na utilização de largura de banda ou não. Em seguida, pode tomar medidas em conformidade.

![Utilização de largura de banda do ExpressRoute](media/log-analytics-network-performance-monitor/expressroute-peak-utilization.png)

 

## <a name="next-steps"></a>Passos Seguintes
[Pesquisar registos](log-analytics-log-searches.md) para ver registos de dados de desempenho de rede detalhada.
