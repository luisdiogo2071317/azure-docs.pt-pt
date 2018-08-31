---
title: Descrição geral das métricas no Microsoft Azure
description: Descrição geral de métricas e seu uso no Microsoft Azure
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/05/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: d61ac48aa7c51bc4b215a7d56b1bbedfdc613f9f
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43287561"
---
# <a name="overview-of-metrics-in-microsoft-azure"></a>Descrição geral das métricas no Microsoft Azure
Este artigo descreve o que as métricas que estão no Microsoft Azure, seus benefícios e como começar a usá-los.  

## <a name="what-are-metrics"></a>Quais são as métricas?
O Azure Monitor permite-lhe consumir telemetria ganhar visibilidade sobre o desempenho e estado de funcionamento das suas cargas de trabalho no Azure. O tipo mais importante de dados de telemetria do Azure é as métricas (também chamadas de contadores de desempenho) emitidas pelos recursos mais do Azure. O Azure Monitor proporciona várias formas de configurar e consumir estas métricas de monitorização e resolução de problemas.

## <a name="what-are-the-characteristics-of-metrics"></a>Quais são as características de métricas?
As métricas têm as seguintes características:

* Todas as métricas têm **frequência de um minuto** (a menos que especificado de outra forma na definição de uma métrica). Receber um valor de métrica a cada minuto do seu recurso, dando-lhe perto de visibilidade em tempo real sobre o estado e estado de funcionamento do seu recurso.
* As métricas estão **disponíveis imediatamente**. Não precisa de opção ativa de participação ou configurar diagnósticos adicionais.
* Pode acessar **93 dias do histórico de** para cada uma. Pode ver rapidamente as tendências recentes e mensais no desempenho ou estado de funcionamento do seu recurso.
* Algumas métricas podem ter atributos de par nome / valor chamados **dimensões**. Isso permite que ainda mais segmentar e explorar uma métrica de uma forma mais significativa.

## <a name="what-can-you-do-with-metrics"></a>O que pode fazer com a métrica?
Métricas permitem que faça as seguintes tarefas:


- Configurar uma métrica **alerta regra que envia uma notificação ou demora automatizada ação** quando a métrica ultrapassa o limiar de que definiu. Ações são controladas através de [grupos de ação](monitoring-action-groups.md). Ações de exemplo incluem e-mail, telefone e notificações de SMS, chamar um webhook, a partir de um runbook e muito mais. **Dimensionamento automático** é uma ação automática especial que permite-lhe aumentar são um recurso e reduzir verticalmente para processar carga, mas manter os custos mais baixos quando não está sob carga. Pode configurar uma regra de definição de dimensionamento automático para aumentar e reduzir com base numa métrica cruzar um limiar.
- **Rota** todas as métricas para *Application Insights* ou *do Log Analytics* para ativar a análise instantânea, pesquisa e alertas personalizados em dados de métricas de seus recursos. Também pode transmitir em fluxo métricas para uma *Hub de eventos*, permitindo que depois roteá-los para o Azure Stream Analytics ou para aplicações personalizadas para análise quase em tempo real. Iremos configurar o Hub de eventos transmissão em fluxo através das definições de diagnóstico.
- **Arquivo** o histórico de desempenho ou o estado de funcionamento do seu recurso de conformidade, auditoria ou criação de relatórios offline.  Pode encaminhar as suas métricas para o armazenamento de Blobs do Azure quando configurar as definições de diagnóstico para o seu recurso.
- Utilize o **portal do Azure** para detetar, aceder e ver todas as métricas ao selecionar um recurso e as métricas num gráfico de desenho. Pode controlar o desempenho do seu recurso (por exemplo, uma VM, Web site ou aplicação lógica) ao afixar esse gráfico ao dashboard.  
- **Realizar análises avançadas** ou relatórios sobre as tendências de desempenho ou na utilização do seu recurso.
- **Consulta** métricas com os cmdlets do PowerShell ou a API de REST para várias plataformas.
- **Consumir** as métricas através de novas APIs de REST do Azure Monitor.

  ![Encaminhamento de métricas no Azure Monitor](./media/monitoring-overview-metrics/Metrics_Overview_v4.png)

## <a name="access-metrics-via-the-portal"></a>Métricas de acesso através do portal
Segue-se um passo a passo rápido de como criar um gráfico de métricas com o portal do Azure.

### <a name="to-view-metrics-after-creating-a-resource"></a>Para visualizar as métricas depois de criar um recurso
1. Abra o portal do Azure.
2. Crie um Web site do serviço de aplicações do Azure.
3. Depois de criar um Web site, vá para o **descrição geral** painel do Web site.
4. Pode ver a nova métrica como um **monitorização** mosaico. Em seguida, pode editar o mosaico e selecionar métricas mais.

   ![Métricas sobre um recurso no Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview1.png)

### <a name="to-access-all-metrics-in-a-single-place"></a>Para aceder a todas as métricas num único local
1. Abra o portal do Azure.
2. Navegue para a nova **Monitor** separador e, em seguida e selecione o **métricas** opção abaixo dela.
3. Selecione a sua subscrição, grupo de recursos e o nome do recurso na lista pendente.
4. Ver a lista de métricas disponíveis. Em seguida, selecione a métrica está interessado e desenhá-lo.
5. Pode afixá-la ao dashboard ao clicar o pin, no canto superior direito.

   ![Aceder a todas as métricas num único local no Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview2.png)

> [!NOTE]
> Pode aceder a métricas ao nível do anfitrião de VMs (com base do Azure Resource Manager) e conjuntos de dimensionamento de máquina virtual sem qualquer configuração de diagnóstico adicional. Estas novas métricas ao nível do anfitrião estão disponíveis para as instâncias do Windows e Linux. Estas métricas não são deve ser confundido com as métricas de nível do SO convidado que tem acesso para quando ativar o diagnóstico do Azure nas VMs ou dimensionamento de máquinas virtuais conjuntos. Para saber mais sobre como configurar diagnósticos, veja [o que é o Microsoft Azure Diagnostics](../azure-diagnostics.md).
>
>

O Azure Monitor também tem uma nova avaliação de criação de gráficos experiência disponível em pré-visualização. Esta experiência permite aos utilizadores sobre as métricas de vários recursos num gráfico. Os utilizadores também podem representar, segmento e filtrar com esta métrica de nova experiência de criação de gráficos de métricas multidimensionais. Para saber mais [clique aqui](https://aka.ms/azuremonitor/new-metrics-charts)

## <a name="access-metrics-via-the-rest-api"></a>Métricas de acesso através da API REST
Métricas do Azure podem ser acessadas por meio das APIs de Monitor do Azure. Existem duas APIs que o ajudam a detetarem e aceder a métricas:

* Utilize o [definições de métrica de Monitor de Azure REST API](https://docs.microsoft.com/rest/api/monitor/metricdefinitions) aceder à lista de métricas e qualquer dimensões, que estão disponíveis para um serviço.
* Utilize o [API de REST do Azure Monitor métricas](https://docs.microsoft.com/rest/api/monitor/metrics) segmentar, filtrar e acessar os dados de métricas real.

> [!NOTE]
> Este artigo aborda as métricas através da [nova API de métricas](https://docs.microsoft.com/rest/api/monitor/) para recursos do Azure. A versão de API para as novas definições de métricas e métricas de APIs é 2018-01-01. As definições de métricas de legado e métricas podem ser acessadas com a versão 2014-04-01 de API.
>
>

Para obter instruções mais detalhadas com as APIs de REST do Azure Monitor, consulte [instruções de API de REST do Azure Monitor](monitoring-rest-api-walkthrough.md).

## <a name="export-metrics"></a>Exportar métricas
Pode ir para o **as definições de diagnóstico** painel no **Monitor** separador e ver as opções de exportação para métricas. Pode selecionar métricas (e os registos de diagnóstico) para ser encaminhado para o armazenamento de BLOBs, para os Hubs de eventos do Azure ou para o Log Analytics para casos de utilização que foram mencionados anteriormente neste artigo.

 ![Opções de exportação para métricas no Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview3.png)

Pode configurar estas opções por meio de modelos do Resource Manager, [PowerShell](insights-powershell-samples.md), [CLI do Azure](insights-cli-samples.md), ou [REST APIs](https://msdn.microsoft.com/library/dn931943.aspx).

> [!NOTE]
> Atualmente, o envio de métricas multidimensionais através das definições de diagnóstico não é suportado. As métricas com dimensões são exportadas como métricas dimensionais simples e agregadas em valores de dimensões.
>
> *Por exemplo*: a métrica “Mensagens Recebidas” num hub do Hub de Eventos pode ser explorada e representada ao nível da linha. No entanto, se for exportada através das definições de diagnóstico, a métrica será representada como todas as mensagens recebidas em todas as filas do hub do Hub de Eventos.
>
>

## <a name="take-action-on-metrics"></a>Tomar medidas em métricas
Para receber notificações ou tome medidas automatizadas nos dados de métrica, pode configurar regras de alerta ou definições de dimensionamento automático.

### <a name="configure-alert-rules"></a>Configurar regras de alerta
Pode configurar regras de alertas em métricas. Estas regras de alerta podem verificar se uma métrica ultrapassou um determinado limiar. Existem dois recursos de alertas métrica oferecidos pelo Azure Monitor.

Alertas de métricas: em seguida, pode notificar por e-mail ou acionar um webhook que pode ser utilizado para executar qualquer script personalizado. Também pode utilizar o webhook para configurar integrações de produtos de terceiros.

 ![Métricas e regras de alerta no Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview4.png)

Alertas de métricas mais recentes têm a capacidade de monitorizar a várias métricas e limiares, para um recurso e, em seguida, notificá-lo por meio de um [grupo de ação](monitoring-action-groups.md). Saiba mais sobre [alertas mais recentes aqui](https://aka.ms/azuremonitor/near-real-time-alerts).


### <a name="autoscale-your-azure-resources"></a>Dimensionamento automático do Azure recursos
Alguns recursos do Azure suportam o dimensionamento horizontalmente ou de várias instâncias para processar cargas de trabalho. Dimensionamento automático aplica-se para o serviço de aplicações (aplicações Web), os conjuntos de dimensionamento de máquinas virtuais e serviços Cloud do Azure clássico. Pode configurar regras de dimensionamento automático para aumentar horizontalmente ou no, quando uma métrica que tem impacto sobre a sua carga de trabalho ultrapassa um limiar que especificar. Para obter mais informações, consulte [descrição geral do dimensionamento automático](monitoring-overview-autoscale.md).

 ![Métricas e dimensionamento automático no Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="learn-about-supported-services-and-metrics"></a>Saiba mais sobre os serviços suportados e métricas
Pode ver uma lista detalhada de todos os serviços suportados e suas métricas [métricas do Azure Monitor - métricas suportadas por tipo de recurso](monitoring-supported-metrics.md).

## <a name="next-steps"></a>Passos Seguintes
Veja as ligações ao longo deste artigo. Além disso, saiba mais sobre:  

* [Métricas comuns do dimensionamento automático](insights-autoscale-common-metrics.md)
* [Como criar regras de alerta](insights-alerts-portal.md)
* [Analisar registos do armazenamento do Azure com o Log Analytics](../log-analytics/log-analytics-azure-storage.md)
