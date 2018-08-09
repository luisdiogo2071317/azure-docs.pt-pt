---
title: Origens de dados no Azure de monitorização | Documentos da Microsoft
description: Descreve os dados disponíveis para monitorizar o estado de funcionamento e desempenho dos seus recursos do Azure e as aplicações em execução nos mesmos.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/05/2018
ms.author: bwren
ms.openlocfilehash: 48cbfac78b41b47419799584837e094d45757628
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627462"
---
# <a name="sources-of-monitoring-data-in-azure"></a>Origens de dados no Azure de monitorização
Este artigo descreve os dados disponíveis para monitorizar o estado de funcionamento e desempenho dos seus recursos do Azure e as aplicações em execução nos mesmos.  Recolher e analisar estes dados com as ferramentas descritas no [coleta de dados no Azure de monitorização](monitoring-data-collection.md)

A monitorização dos dados no Azure provém de uma variedade de origens que podem ser organizadas em camadas, o escalão mais elevado, que está a ser seu aplicativo e o escalão mais baixo que está a ser a plataforma do Azure. Isso está ilustrado no diagrama seguinte com cada camada descrita detalhadamente nas seções a seguir.

![Camadas de dados de monitorização](media/monitoring-data-sources/monitoring-tiers.png)


## <a name="azure-platform"></a>Plataforma Azure
Telemetria relacionados com o estado de funcionamento e a operação do Azure em si inclui dados sobre a operação e a gestão da sua subscrição do Azure ou o inquilino. Ele inclui dados de estado de funcionamento do serviço armazenados no registo de atividades do Azure e os registos de auditoria do Azure Active Directory.

![Coleção do Azure](media/monitoring-data-sources/azure-collection.png)

### <a name="azure-service-health"></a>Azure Service Health
[O Azure Service Health](../monitoring-and-diagnostics/monitoring-service-notifications.md) fornece informações sobre o estado de funcionamento dos serviços do Azure na sua subscrição que a aplicação e os recursos se baseiam em. Pode criar alertas para serem notificadas do atuais e esperados problemas críticos que possam afetar a sua aplicação. Estado de funcionamento do serviço de registos é armazenados na [registo de atividades do Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), para que possa vê-los no Gerenciador de registo de atividade e copiá-los para o Log Analytics.

### <a name="azure-activity-log"></a>Registo de atividades do Azure
O [registo de atividades do Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) inclui registos de estado de funcionamento do serviço, juntamente com os registos em quaisquer alterações de configuração feitas aos seus recursos do Azure. O registo de atividades está disponível para todos os recursos do Azure e representa seus _externo_ vista. Os tipos específicos de registos no registo de Atividades são descritos nas [esquema de eventos de registo de atividades do Azure](../monitoring-and-diagnostics/monitoring-activity-log-schema.md).

Pode ver o registo de atividades para um recurso específico na sua página nos registos do Azure de portal ou a vista de vários recursos na [atividade Log Explorer](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md). É particularmente útil copiar as entradas de registo para o Log Analytics para combiná-los com outros dados de monitorização. Também pode enviar-lhes para outras localizações usando [os Hubs de eventos](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md).


### <a name="azure-active-directory-audit-logs"></a>Registos de auditoria do Azure Active Directory
[Relatórios do Azure Active Directory](../active-directory/reports-monitoring/overview-reports.md) contém o histórico de início de sessão atividade e auditoria do registo de alterações feitas a um inquilino específico. Atualmente não é possível combinar dados de auditoria do Azure Active Directory com outros dados de monitorização como só é acessível através do Azure Active Directory e o [do Azure Active Directory reporting API](../active-directory/reports-monitoring/concept-reporting-api.md).


## <a name="azure-services"></a>Serviços do Azure
Registos de diagnóstico ao nível métricas e recursos fornecem informações sobre o _interno_ operação dos recursos do Azure. Estas estão disponíveis para serviços do Azure mais e soluções de gerenciamento fornecem informações adicionais sobre os serviços específicos.

![Coleção de recursos do Azure](media/monitoring-data-sources/azure-resource-collection.png)


### <a name="metrics"></a>Métricas
Serviços do Azure mais irão gerar métricas que refletem o desempenho e operação. As específicas [métricas irão variar para cada tipo de recurso](../monitoring-and-diagnostics/monitoring-supported-metrics.md).  Eles estão acessíveis a partir do Explorador de métricas e podem ser copiados para o Log Analytics para análise de tendência e outra.


### <a name="resource-diagnostic-logs"></a>Registos de diagnóstico de recursos
Embora o registo de atividades fornece informações sobre as operações executadas num recursos do Azure, ao nível de recursos [registos de diagnóstico](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) fornecem informações sobre o funcionamento do recurso em si.   Os requisitos de configuração e conteúdo estes registos [varia consoante o tipo de recurso](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).

Não pode ver os registos de diagnóstico diretamente no portal do Azure, mas pode [enviá-los para o armazenamento do Azure para arquivar](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md) e exportá-las para [Hub de eventos](../event-hubs/event-hubs-what-is-event-hubs.md) para o redirecionamento para outros serviços, ou [para registo Análise](../monitoring-and-diagnostics/monitor-stream-diagnostic-logs-log-analytics.md) para análise. Podem escrever alguns recursos diretamente ao Log Analytics, enquanto outras pessoas a escrever para uma conta de armazenamento antes de ser [importados para o Log Analytics](../log-analytics/log-analytics-azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage).

### <a name="management-solutions"></a>Soluções de Gestão
 [Soluções de gestão](../monitoring/monitoring-solutions.md) recolher dados para fornecer informações adicionais sobre o funcionamento de um serviço específico. Eles recolhem dados no Log Analytics, onde pode ser analisado com o [linguagem de consulta](../log-analytics/log-analytics-log-search.md) ou vistas a que normalmente estão incluídas na solução.

## <a name="guest-operating-system"></a>Sistema Operativo convidado
Além de telemetria gerada por todos os serviços do Azure, recursos de computação têm um sistema de operativo convidado para monitorizar. Com a instalação de um ou mais agentes, pode recolher telemetria do convidado para as mesmas ferramentas de monitorização, como os serviços do Azure.

![Coleção de recursos de computação do Azure](media/monitoring-data-sources/compute-resource-collection.png)

### <a name="diagnostic-extension"></a>Extensão de diagnóstico
Com o [extensão de diagnóstico do Azure](../monitoring-and-diagnostics/azure-diagnostics.md), pode recolher registos e recursos de computação de dados de desempenho do sistema operativo de cliente do Azure. Métricas e registos recolhidos a partir de clientes são armazenados numa conta de armazenamento do Azure que pode [configurar o Log Analytics para importar a partir](../log-analytics/log-analytics-azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage).  O Explorador de métricas compreende como ler a partir da conta de armazenamento e inclui métricas de cliente com outras métricas recolhidas.


### <a name="log-analytics-agent"></a>Agente do log Analytics
Pode instalar o agente Log Analytics em qualquer máquina virtual do Windows ou Linux ou o computador físico. A máquina virtual pode estar em execução no Azure, noutra cloud ou no local.  O agente se liga ao Log Analytics ou diretamente ou através uma [grupo de gestão do System Center Operations Manager ligado](../log-analytics/log-analytics-om-agents.md) e permite-lhe recolher dados a partir de [origens de dados](../log-analytics/log-analytics-data-sources.md) que configurou ou a partir de [soluções de gestão](../monitoring/monitoring-solutions.md) que fornecem informações adicionais sobre as aplicações em execução na máquina virtual.

### <a name="service-map"></a>Mapa de Serviços
[Mapa de serviço](../operations-management-suite/operations-management-suite-service-map.md) requer um agente de dependência em máquinas virtuais Windows e Linux. Isso funciona com o agente recolhe dados sobre os processos em execução a máquina virtual e as dependências de processos externos do Log Analytics. Ele armazena estes dados no Log Analytics e inclui uma consola que apresenta visualmente os dados que coleta, além de outros dados armazenados no Log Analytics.

## <a name="applications"></a>Aplicações
Além de telemetria que seu aplicativo pode gravar o sistema operativo convidado, monitorização detalhada de aplicações é feita com [Application Insights](https://docs.microsoft.com/azure/application-insights/). O Application Insights pode recolher dados de aplicações em execução num vasto leque de plataformas. A aplicação pode estar em execução no Azure, noutra cloud ou no local.

![Recolha de dados de aplicação](media/monitoring-data-sources/application-collection.png)


#### <a name="application-data"></a>Dados da aplicação
Quando ativar o Application Insights para uma aplicação ao instalar um pacote de instrumentação, recolhe métricas e registos relacionados com o desempenho e a operação da aplicação. Isto inclui informações detalhadas sobre exceções, pedidos de aplicações e vistas de página. O Application Insights armazena os dados que coleta de métricas do Azure e o Log Analytics. Ele inclui ferramentas abrangentes para analisar estes dados, mas também pode analisá-los com dados de outras origens usando ferramentas como o Explorador de métricas e pesquisas de registos.

Também pode utilizar o Application Insights para [criar uma métrica personalizada](../application-insights/app-insights-api-custom-events-metrics.md).  Isto permite-lhe definir sua própria lógica para calcular um valor numérico e, em seguida, armazenamos esse valor com outras métricas que podem ser acedidas a partir do Explorador de métricas e utilizadas para [dimensionamento automático](../monitoring-and-diagnostics/monitoring-autoscale-scale-by-custom-metric.md) e alertas de métricas.

#### <a name="dependencies"></a>Dependências
Para monitorizar operações lógicas diferentes de uma aplicação, tem [recolher telemetria em vários componentes](../application-insights/app-insights-transaction-diagnostics.md). O Application Insights suporta [distribuído a correlação de telemetria](../application-insights/application-insights-correlation.md) que identifica as dependências entre os componentes que permite analisá-los em conjunto.

#### <a name="availability-tests"></a>Testes de disponibilidade
[Testes de disponibilidade](../application-insights/app-insights-monitor-web-app-availability.md) no Application Insights lhe permitem testar a disponibilidade e capacidade de resposta da sua aplicação a partir de diferentes locais na Internet pública. Pode fazer um teste de ping simples para verificar se o aplicativo está ativo ou utilizar o Visual Studio para criar um teste da web que simula um cenário de utilizador.  Testes de disponibilidade não requerem qualquer instrumentação no aplicativo.

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre o [tipos de dados e as ferramentas do Azure de monitorização](monitoring-data-collection.md) utilizado para recolher e analisá-los.
