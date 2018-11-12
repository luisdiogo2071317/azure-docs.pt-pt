---
title: Origens de dados no Azure de monitorização
description: Saiba mais sobre todas as monitorização origens de dados disponíveis no Azure hoje mesmo.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/12/2018
ms.author: johnkem
ms.component: ''
ms.openlocfilehash: b01dafcda9ed8fe14e3f0d50daf25bae1658faee
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282633"
---
# <a name="consume-monitoring-data-from-azure"></a>Consumir dados de monitorização do Azure

Nas plataformas do Azure, estamos a juntar dados de monitorização num único local com o Azure Monitor do pipeline, mas praticamente reconhece que hoje nem todos os dados de monitorização está disponível nesse pipeline ainda. Neste artigo, resumimos as várias formas de como é possível acessar programaticamente dados de monitorização dos serviços do Azure.

## <a name="options-for-data-consumption"></a>Opções para consumo de dados

| Tipo de dados | Categoria | Serviços suportados | Métodos de acesso |
| --- | --- | --- | --- |
| Métricas de nível de plataforma do Azure Monitor | Métricas | [Ver lista aqui](monitoring-supported-metrics.md) | <ul><li>**REST API:** [API de métricas de Monitor do Azure](https://docs.microsoft.com/rest/api/monitor/metrics)</li><li>**Hub de eventos ou blob de armazenamento:** [definições de diagnóstico](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings)</li></ul> |
| Computação (por exemplo, o métricas de SO convidado contadores de desempenho) | Métricas | [Windows](/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines) e máquinas virtuais do Linux (v2), [serviços Cloud](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [do Service Fabric](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**Tabela de armazenamento ou um blob:** [diagnóstico do Windows ou Linux do Azure](azure-diagnostics-storage.md)</li><li>**Hub de eventos:** [diagnóstico do Windows Azure](azure-diagnostics-streaming-event-hubs.md)</li></ul> |
| Métricas personalizadas ou aplicação | Métricas | Qualquer aplicativo instrumentado com o Application Insights | <ul><li>**REST API:** [REST API do Application Insights](https://dev.applicationinsights.io/reference)</li></ul> |
| Métricas de armazenamento | Métricas | Storage do Azure | <ul><li>**Tabela de armazenamento:** [análise de armazenamento](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| Dados de faturação | Métricas | Todos os serviços do Azure | <ul><li>**REST API:** [utilização de recursos do Azure e RateCard APIs](../billing/billing-usage-rate-card-overview.md)</li></ul> |
| Registo de Atividades | Eventos | Todos os serviços do Azure | <ul><li>**REST API:** [eventos de Monitor do Azure API](https://docs.microsoft.com/rest/api/monitor/eventcategories)</li><li>**Hub de eventos ou blob de armazenamento:** [perfil de registo](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile)</li></ul> |
| Registos de diagnóstico do Azure Monitor | Eventos | [Ver lista aqui](monitoring-diagnostic-logs-schema.md) | <ul><li>**Hub de eventos ou blob de armazenamento:** [definições de diagnóstico](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings)</li></ul> |
| Computação de registos do SO convidado (ex. IIS, o ETW, syslogs) | Eventos | [Windows](/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines) e máquinas virtuais do Linux (v2), [serviços Cloud](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [do Service Fabric](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**Tabela de armazenamento ou um blob:** [diagnóstico do Windows ou Linux do Azure](azure-diagnostics-storage.md)</li><li>**Hub de eventos:** [diagnóstico do Windows Azure](azure-diagnostics-streaming-event-hubs.md)</li></ul> |
| Registos do serviço de aplicações | Eventos | Serviços aplicacionais | <ul><li>**Armazenamento de BLOBs, tabela ou ficheiro:** [de diagnóstico de aplicações da Web](../app-service/web-sites-enable-diagnostic-log.md)</li></ul> |
| Registos de armazenamento | Eventos | Storage do Azure | <ul><li>**Tabela de armazenamento:** [análise de armazenamento](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| Alertas do Centro de segurança | Eventos | Centro de Segurança do Azure | <ul><li>**REST API:** [alertas de segurança](https://msdn.microsoft.com/library/mt704050.aspx)</li></ul> |
| Relatórios do Active Directory | Eventos | Azure Active Directory | <ul><li>**REST API:** [graph API do Azure Active Directory](../active-directory/reports-monitoring/concept-reporting-api.md)</li></ul> |
| Estado de recurso do Centro de segurança | Estado | [Recursos de todos com suporte](https://msdn.microsoft.com/library/mt704041.aspx#Anchor_1) | <ul><li>**REST API:** [Estados de segurança](https://msdn.microsoft.com/library/mt704041.aspx)</li></ul> |
| Estado de Funcionamento de Recursos | Estado | Serviços suportados | <ul><li>**REST API:** [REST API do Estado de funcionamento do recurso](https://azure.microsoft.com/blog/reduce-troubleshooting-time-with-azure-resource-health/)</li></ul> |
| Alertas de métricas de Monitor do Azure | Notificações | [Ver lista aqui](monitoring-supported-metrics.md) | <ul><li>**Webhook:** [alertas de métricas do Azure](insights-webhooks-alerts.md)</li></ul> |
| Alertas de registo de atividade do Monitor do Azure | Notificações | Todos os serviços do Azure | <ul><li>**Webhook:** alertas de registo de atividades do Azure</li></ul> |
| Notificações de dimensionamento automático | Notificações | [Ver lista aqui](monitoring-overview-autoscale.md#supported-services-for-autoscale) | <ul><li>**Webhook:** [esquema de payload de webhook de notificação de dimensionamento automático](insights-autoscale-to-webhook-email.md#autoscale-notification-webhook-payload-schema)</li></ul> |
| Alertas de consulta de pesquisa de registo | Notificações | Log Analytics | <ul><li>**Webhook:** [ação do Webhook para regras de alerta de registo](../monitoring-and-diagnostics/monitor-alerts-unified-log-webhook.md)</li></ul> |
| Alertas de métricas do Application Insights | Notificações | Application Insights | <ul><li>**Webhook:** [alertas do Application Insights](../application-insights/app-insights-alerts.md)</li></ul> |
| Testes web do Application Insights | Notificações | Application Insights | <ul><li>**Webhook:** [alertas do Application Insights](../application-insights/app-insights-alerts.md)</li></ul> |

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [métricas do Azure Monitor](../monitoring/monitoring-data-collection.md)
- Saiba mais sobre [o registo de atividades do Azure](monitoring-overview-activity-logs.md)
- Saiba mais sobre [registos de diagnóstico do Azure](monitoring-overview-of-diagnostic-logs.md)
