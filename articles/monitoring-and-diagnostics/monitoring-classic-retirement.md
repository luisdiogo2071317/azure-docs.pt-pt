---
title: Unificada alertas e monitorização no Azure Monitor substitui clássico alertas e monitorização
description: Descrição geral de desativação de serviços clássicos de monitorização e a funcionalidade, mostrado anteriormente no portal do Azure em alertas (clássico). Clássico alertas e monitorização inclui alertas de métricas clássicas para recursos do Azure, os alertas de métricas de clássicos para o Application Insights, alertas clássicos de webtest do Application Insights, métrica personalizada clássica com base em alertas para o Application Insights e o clássico alertas para a v1 do Application Insights SmartDetection
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 10b1d2088a489c73d8929bdf22069e21b8beac50
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48817508"
---
# <a name="unified-alerting--monitoring-in-azure-monitor-replaces-classic-alerting--monitoring"></a>Unificada alertas e monitorização no Azure Monitor substitui clássico alertas e monitorização

O Azure Monitor tem agora se tornará am de pilha completa unificada monitorização do serviço que suporta agora "Métricas um" e "Alertas de um" todos os recursos; Para obter mais informações, consulte nosso [mensagem de blogue no novo Azure Monitor](https://azure.microsoft.com/en-us/blog/new-full-stack-monitoring-capabilities-in-azure-monitor/). O novo Azure de monitorização e alertas plataformas foi projetado para ser rápido e inteligente e extensível – ritmo de manutenção com a crescente expanse da nuvem, computação e em linha com a filosofia de Cloud inteligente da Microsoft. 

Com o novo Azure de monitorização e alertas plataforma no local, será descontinuado o "clássico" monitorização e alertas plataforma - hospedada no *ver alertas clássicos* seção de alertas do Azure, vai ser preterido pela de 2019 de Junho.

 ![Alerta clássica no portal do Azure](./media/monitoring-overview-alerts-classic/monitor-alert-screen2.png) 

Incentivamos a começar a utilizar o e recrie os alertas na nova plataforma. Para clientes que possuam um grande número de alertas, está trabalhando para fornecer uma forma de mover alertas clássicos existentes para o novo sistema de alertas sem interrupção ou adicionado os custos.

## <a name="unified-metrics-and-alerts-in-application-insights"></a>Unificada de métricas e alertas no Application Insights

Plataforma de métrica mais recente do Monitor do Azure irá agora power monitorização provenientes de Application Insights. Esta mudança significa o que Application Insights irá vincular a grupos de ação, permitir opções muito mais do que apenas os e-mail e webhook chamadas anteriores. Alertas agora podem acionar chamadas de voz, as funções do Azure, o Logic Apps, SMS e ferramentas de ITSM, como o ServiceNow e Runbooks de automatização. Com quase em tempo real de monitorização e alertas, a nova plataforma permite aos utilizadores do Application Insights aproveitar a mesma tecnologia existente monitorização através de outros recursos do Azure e a base da monitorização de produtos da Microsoft.

O novo monitorização unificada e alertas para o Application Insights irão abranger:

- **Métricas do Application Insights plataforma** – que fornece métricas pré-criados populares do produto do Application Insights. Para obter mais informações, consulte este artigo sobre como usar [métricas de plataforma para o Application Insights no novo Azure Monitor](../application-insights/pre-aggregated-metrics-log-metrics.md#pre-aggregated-metrics).
- **Teste de disponibilidade de informações de aplicações e Web** -que fornece a capacidade de avaliar a capacidade de resposta e a disponibilidade da sua aplicação web ou servidor. Para obter mais informações, consulte este artigo sobre como usar [testes de disponibilidade e alertas para o Application Insights no novo Azure Monitor](../application-insights/app-insights-monitor-web-app-availability.md).
- **Métricas do Application Insights personalizado** – que lhe permite definir e emitir as suas próprias métricas de monitorização e alertas. Para obter mais informações, consulte este artigo sobre como usar [métrica personalizada para o Application Insights no novo Azure Monitor](../application-insights/pre-aggregated-metrics-log-metrics.md#using-pre-aggregation-with-application-insights-custom-metrics).
- **Anomalias de falha do Application Insights (parte de deteção inteligente)** – que notifica-o automaticamente em tempo quase real se a sua aplicação web sofrerem um aumento anormal na taxa de pedidos HTTP falhados ou as chamadas de dependência. Application Insights de anomalias de falha (parte de deteção inteligente) como parte do novo Monitor do Azure, estará disponível em breve e iremos atualizar este documento com links na próxima iteração como ela é revertida-out nos próximos meses.

## <a name="unified-metrics--alerts-for-other-azure-resources"></a>Unificada métricas e alertas para outros recursos do Azure

Desde Março de 2018, a próxima geração de alertas e monitoramento multidimensional para recursos do Azure foram na disponibilidade. Agora a plataforma mais recente de métrica e alertas, é mais rápido com capacidades de quase em tempo real. Mais importante, os alertas de métrica de plataforma mais recente fornecem mais granularidade, como a plataforma mais recente inclui a opção de dimensões, o que lhe permitem ao setor e filtrar a combinação de valor específico, a condição ou a operação. Como todos os alertas no Monitor do Azure novo, os alertas de métricas mais recentes são mais extensíveis com o uso de ActionGroups – permitindo notificações expandir além do e-mail ou webhook para SMS, voz, função do Azure, Runbook de automatização e muito mais.
As métricas de mais recente de recursos do Azure estão disponíveis como:

- **Métricas de plataforma do Azure Monitor padrão** – que fornece métricas pré-preenchidos populares de vários produtos e serviços do Azure. Para obter mais informações, veja este artigo sobre [suportado métricas no Azure Monitor](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported) e [suportam alertas de métricas no Azure Monitor](alert-metric-overview.md#supported-resource-types-for-metric-alerts).
- **Métricas do Monitor personalizado do Azure** – que fornece as métricas de utilizador controlado por origens, incluindo o agente de diagnóstico do Azure. Para obter mais informações, veja este artigo sobre [métricas personalizadas no Azure Monitor](metrics-custom-overview.md). Utilizar métricas personalizadas, também pode publicar métricas recolhidas pelo [agente do Windows Azure Diagnostics](metrics-store-custom-guestos-resource-manager-vm.md) e [InfluxData Telegraf agente](metrics-store-custom-linux-telegraf.md).

## <a name="retirement-of-classic-monitoring-and-alerting-platform"></a>Extinção do clássico de monitorização e alertas de plataforma

Como já declarado, clássica de monitorização e alertas atualmente utilizável de plataforma a [alertas (clássica) secção](monitoring-overview-alerts-classic.md) do Azure vai ser descontinuado portal dentro de 12 meses, tendo em conta foram substituídas por sistema mais recente.
Clássico mais antigo, monitorização e alertas vai ser descontinuado a 30 de Junho de 2019; incluindo o fechamento de APIs relacionadas, interface do portal do Azure e serviços no mesmo. Especificamente, estas funcionalidades serão preteridas:

- Métricas (clássicas) mais antigas e alertas para recursos do Azure como atualmente disponíveis através do [alertas (clássica) secção](monitoring-overview-alerts-classic.md) do Azure portal; acessível como [microsoft.insights/alertrules](https://docs.microsoft.com/en-us/rest/api/monitor/alertrules) recursos
- Plataforma (clássica) mais antiga e métricas personalizadas para o Application Insights, bem como alertas nos mesmos como atualmente disponíveis através do [alertas (clássica) secção](monitoring-overview-alerts-classic.md) do Azure, portal e acessível como [microsoft.insights/ alertrules](https://docs.microsoft.com/en-us/rest/api/monitor/alertrules) recursos
- Alerta de anomalias de falha (clássica) mais antiga atualmente disponível na [deteção inteligente no Application Insights](../application-insights/app-insights-proactive-diagnostics.md) no portal do Azure; com alertas configurados mostrado na [alertas (clássica) secção](monitoring-overview-alerts-classic.md) do Azure Portal

Todos os clássico monitorização e alertas de sistemas, incluindo correspondente [API](https://msdn.microsoft.com/library/azure/dn931945.aspx), [PowerShell](insights-alerts-powershell.md), [CLI](insights-alerts-command-line-interface.md), [página do portal do Azure, e [modelo do Resource](monitoring-enable-alerts-using-template.md) permanecerão utilizável até Junho de 2019. Após esta data, a monitorização clássico e o serviço de alertas será extinto e já não está disponível para utilização; enquanto as regras de qualquer alerta que continuam a pertencer nos alertas (clássicas) para além de Junho de 2019 irão continuar a executar, mas não está disponível para modificação.

Todos os alertas restantes clássico de monitorização e alertas de plataforma para além de 2019 de Junho, serão migrados automaticamente pela Microsoft para seus equivalentes em nova plataforma do Azure monitor em Outubro de 2019. O processo seja ininterrupto sem qualquer período de inatividade e certifique-se de que os clientes têm sem perda de monitorização de cobertura.

Em breve fornecemos ferramentas para que possa migrar voluntariamente seus alertas a partir [alertas (clássica) secção](monitoring-overview-alerts-classic.md) do portal do Azure para os novos alertas do Azure. Todas as regras configuradas nos alertas (clássicas) que são migradas para o novo Azure Monitor serão gratuitas e não será cobradas. Regras de alerta clássicas migradas também serão não assume qualquer custo para enviar notificações por e-mail, webhook ou LogicApp. No entanto, o uso dos tipos mais recente de notificação ou ação (como o SMS, chamada de voz, integração de ITSM, etc.) será cobráveis se adicionado a um alerta de migração ou a nova. Para obter mais informações, consulte [preços do Azure Monitor](https://azure.microsoft.com/en-us/pricing/details/monitor/).

Além disso, o seguinte será faturáveis em ambit dos [preços do Azure Monitor](https://azure.microsoft.com/en-us/pricing/details/monitor/):

- Qualquer nova (não migrados) regra de alerta criada acima unidades gratuitas, na nova plataforma do Azure Monitor
- Todos os dados ingeridos e mantidas além das unidades gratuitas incluídas por Monitor do Azure
- Qualquer teste web de teste multi executado pelo Application Insights
- Qualquer métricas personalizadas armazenadas para além dos unidades gratuitas incluídas no Azure Monitor

Este artigo será links será continuamente atualizada & detalhes sobre a monitorização do Azure novo e alertas de funcionalidade, bem como a disponibilidade de ferramentas para ajudar os utilizadores a adotar a nova plataforma do Azure Monitor.


## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre o [novo unificada do Azure Monitor](../azure-monitor/overview.md).
* Saiba mais sobre a nova [alertas do Azure](monitoring-overview-unified-alerts.md).
