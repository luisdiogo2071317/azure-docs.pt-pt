---
title: Detalhes de recolha de dados para soluções de gestão no Azure | Microsoft Docs
description: Soluções de gestão no Azure são uma coleção de regras de aquisição lógica, visualização e os dados que fornecem métricas pivoted em torno de uma área de problema específico.  Este artigo fornece uma lista de soluções de gestão disponíveis da Microsoft e detalhes sobre a respetiva método e a frequência de recolha de dados.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: bwren
ms.openlocfilehash: cab92187c6c36dc4670e202b15b35db8ad510239
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
---
# <a name="data-collection-details-for-management-solutions-in-azure"></a>Detalhes de recolha de dados para soluções de gestão no Azure
Este artigo inclui uma lista de [soluções de gestão](monitoring-solutions.md) disponíveis da Microsoft com ligações para a respetiva documentação detalhada.  Também fornece informações de método e frequência de recolha de dados para análise de registos.  Pode utilizar as informações neste artigo para identificar as diferentes soluções disponíveis e para compreender os requisitos de ligação e de fluxo de dados para soluções de gestão diferente. 

## <a name="list-of-management-solutions"></a>Lista de soluções de gestão

A tabela seguinte lista o [soluções de gestão](monitoring-solutions.md) no Azure fornecido pela Microsoft. Uma entrada na coluna significa que a solução recolhe dados para análise de registos utilizando esse método.  Se uma solução não tem colunas selecionadas, em seguida, escreve diretamente ao Log Analytics a partir de outro serviço do Azure. Siga a ligação para cada um da respetiva documentação para obter mais informações detalhada.

Uma explicação das colunas é os seguintes:

- **Agente de monitorização Microsoft** -agente utilizada no Windows e Linux para executar o Management pack de soluções de gestão e do SCOM a partir do Azure. Nesta configuração, o agente está ligado diretamente ao Log Analytics sem a ser ligado a um grupo de gestão do Operations Manager. 
- **O Operations Manager** -agente idêntica, como o Microsoft monitoring agent. Nesta configuração, tem [ligado a um grupo de gestão do Operations Manager](../log-analytics/log-analytics-om-agents.md) que está ligada à análise de registos. 
-  **Armazenamento do Azure** -solução recolhe dados de uma conta de armazenamento do Azure. 
- **O Operations Manager necessárias?** -Um grupo de gestão do Operations Manager ligado é necessário para a recolha de dados pela solução de gestão. 
- **Dados de agente do Operations Manager enviados através do grupo de gestão** - se o agente [ligado a um grupo de gestão do SCOM](../log-analytics/log-analytics-om-agents.md), em seguida, são enviados dados para análise de registos do servidor de gestão. Neste caso, o agente não tem de ligar diretamente ao Log Analytics. Se esta caixa não está selecionada, em seguida, são enviados dados do agente diretamente ao Log Analytics mesmo que o agente está ligado a um grupo de gestão do SCOM. terão de ser capaz de comunicar ao Log Analytics através de um [OMS gateway](../log-analytics/log-analytics-oms-gateway.md).
- **Frequência de recolha** -Especifica a frequência que dados são recolhidos pela solução de gestão. 



| **Solução de gestão** | **Plataforma** | **Agente de monitorização da Microsoft** | **Agente do Operations Manager** | **Armazenamento do Azure** | **O Operations Manager necessárias?** | **Dados de agente do Operations Manager enviados através do grupo de gestão** | **Frequência de recolha** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [Log Analytics da Atividade](../log-analytics/log-analytics-activity.md) | Azure | | | | | | na notificação |
| [Avaliação do AD](../log-analytics/log-analytics-ad-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 dias |
| [Estado de Replicação do AD](../log-analytics/log-analytics-ad-replication-status.md) |Windows |&#8226; |&#8226; | | |&#8226; |5 dias |
| [Agente de Funcionamento de Agente](../operations-management-suite/oms-solution-agenthealth.md) | Windows e Linux | &#8226; | &#8226; | | | &#8226; | 1 minuto |
| [Gestão de alertas](../log-analytics/log-analytics-solution-alert-management.md) (Nagios) |Linux |&#8226; | | | | |sobre chegada |
| [Gestão de alertas](../log-analytics/log-analytics-solution-alert-management.md) (Zabbix) |Linux |&#8226; | | | | |1 minuto |
| [Gestão de alertas](../log-analytics/log-analytics-solution-alert-management.md) (Operations Manager) |Windows | |&#8226; | |&#8226; |&#8226; |3 minutos |
| [Azure Site Recovery](../site-recovery/site-recovery-overview.md) | Azure | | | | | | n/d |
| [Conector do Application Insights (pré-visualização)](../log-analytics/log-analytics-app-insights-connector.md) | Azure | | | |  |  | na notificação |
| [Automatização de trabalho híbrida](../automation/automation-hybrid-runbook-worker.md) | Windows | &#8226; | &#8226; |  |  |  | n/d |
| [Análise de Gateway de aplicação do Azure](../log-analytics/log-analytics-azure-networking-analytics.md) | Azure |  |  |  |  |  | na notificação |
| **Solução de gestão** | **Plataforma** | **Agente de monitorização da Microsoft** | **Agente do Operations Manager** | **Armazenamento do Azure** | **O Operations Manager necessárias?** | **Dados de agente do Operations Manager enviados através do grupo de gestão** | **Frequência de recolha** |
| [Análise de grupo de segurança de rede do Azure](../log-analytics/log-analytics-azure-networking-analytics.md) | Azure |  |  |  |  |  | na notificação |
| [Análise de SQL do Azure (pré-visualização)](../log-analytics/log-analytics-azure-sql.md) | Windows | | | | | | 1 minuto |
| [Cópia de segurança](../backup/backup-introduction-to-azure-backup.md) | Azure |  |  |  |  |  | n/d |
| [Capacidade e o desempenho (pré-visualização)](../log-analytics/log-analytics-capacity.md) |Windows |&#8226; |&#8226; | | |&#8226; |sobre chegada |
| [Monitorização de Alterações](../log-analytics/log-analytics-change-tracking.md) |Windows |&#8226; |&#8226; | | |&#8226; |hora a hora |
| [Monitorização de Alterações](../log-analytics/log-analytics-change-tracking.md) |Linux |&#8226; | | | | |hora a hora |
| [Contentores](../log-analytics/log-analytics-containers.md) | Windows e Linux | &#8226; | &#8226; |  |  |  | 3 minutos |
| [Análise do Cofre de Chaves](../log-analytics/log-analytics-azure-key-vault.md) |Windows | | | | | |na notificação |
| [Avaliação de Software Maligno](../log-analytics/log-analytics-malware.md) |Windows |&#8226; |&#8226; | | |&#8226; |hora a hora |
| [Monitor de Desempenho da Rede](../log-analytics/log-analytics-network-performance-monitor.md) | Windows | &#8226; | &#8226; |  |  |  | TCP handshakes a cada cinco segundos, dados enviados a cada 3 minutos |
| [Análise do Office 365 (pré-visualização)](../operations-management-suite/oms-solution-office-365.md) |Windows | | | | | |na notificação |
| **Solução de gestão** | **Plataforma** | **Agente de monitorização da Microsoft** | **Agente do Operations Manager** | **Armazenamento do Azure** | **O Operations Manager necessárias?** | **Dados de agente do Operations Manager enviados através do grupo de gestão** | **Frequência de recolha** |
| [Auditoria e segurança](../operations-management-suite/oms-security-getting-started.md) (Syslog) | Linux | &#8226; | | |  |  | sobre chegada |
| [Auditoria e segurança](../operations-management-suite/oms-security-getting-started.md) (registos de eventos de segurança) |Windows |&#8226; |&#8226; |&#8226; |  |&#8226; | sobre chegada |
| [Auditoria e segurança](../operations-management-suite/oms-security-getting-started.md) (registos de Firewall) |Windows |&#8226; |&#8226; |  |  |  |sobre chegada |
| [Análise de recursos de infraestrutura de serviço (pré-visualização)](../log-analytics/log-analytics-service-fabric.md) |Windows | | |&#8226; | | |5 minutos |
| [Mapa do Serviço](../operations-management-suite/operations-management-suite-service-map.md) | Windows e Linux | &#8226; | &#8226; |  |  |  | 15 segundos |
| [Avaliação do SQL](../log-analytics/log-analytics-sql-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 dias |
| [SurfaceHub](../log-analytics/log-analytics-surface-hubs.md) |Windows |&#8226; | | | | |sobre chegada |
| [O System Center Operations Manager Assessment (pré-visualização)](../log-analytics/log-analytics-scom-assessment.md) | Windows | &#8226; | &#8226; |  |  | &#8226; | sete dias |
| [Gestão de Atualizações](../operations-management-suite/oms-solution-update-management.md) | Windows |&#8226; |&#8226; | | |&#8226; |pelo menos 2 vezes por dia e 15 minutos depois de instalar uma atualização |
| [Atualizar Preparação](https://docs.microsoft.com/windows/deployment/upgrade/upgrade-readiness-get-started) | Windows | &#8226; |  |  |  |  | 2 dias |
| [VMware monitorização (pré-visualização)](../log-analytics/log-analytics-vmware.md) | Linux | &#8226; |  |  |  |  | 3 minutos |
| [Dados por fio 2.0 (pré-visualização)](../log-analytics/log-analytics-wire-data.md) |Windows (2012 R2 / 8.1 ou posterior) |&#8226; |&#8226; | | | | 1 minuto |




## <a name="next-steps"></a>Passos Seguintes
* Saiba como [criar consultas](../log-analytics/log-analytics-log-searches.md) para analisar os dados recolhidos por soluções de gestão.
