---
title: Descrição geral do Azure agentes de monitorização | Documentos da Microsoft
description: Este artigo fornece uma visão geral detalhada dos agentes do Azure disponíveis que suportam a monitorização de VMs do Azure.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2018
ms.author: magoedte
ms.openlocfilehash: 81db6720422de111cc5b390c58e9020d7c19f90a
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282038"
---
# <a name="overview-of-the-azure-agents-to-monitor-azure-virtual-machines"></a>Descrição geral dos agentes do Azure para monitorizar máquinas virtuais do Azure
O Microsoft Azure fornece várias formas de recolher diferentes tipos de dados de máquinas virtuais alojadas no Azure ou noutros fornecedores de cloud com o Microsoft Windows e Linux.  Este artigo o ajudará a descrever as diferenças e as capacidades disponíveis com cada agente para poder determinar qual deles irá suportar a gestão de serviço ou gerais de requisitos de monitorização.  

## <a name="comparing-azure-diagnostic-and-log-analytics-agent"></a>Comparação entre o agente de diagnóstico do Azure e o Log Analytics
Hoje em dia no Azure, existem dois tipos de agentes disponíveis para monitorizar uma VM do Azure - a extensão de diagnóstico do Azure e o Log Analytics Agent para Linux e Windows.  Fundamentalmente, esses agentes foram concebidos para recolher métricas e registos e reencaminhar para um repositório. No entanto, que é onde as suas semelhanças terminam.  

O [extensão de diagnóstico do Azure](../monitoring-and-diagnostics/azure-diagnostics.md), que é fornecido para Cloud Services do Azure, uma vez que tornou-se em disponibilidade geral em 2010, é um agente que fornece uma coleção simples de dados de diagnóstico de um recurso de IaaS do Azure, como uma VM, e mantê-lo no armazenamento do Azure.  Uma vez no armazenamento, que escolheu para ver uma das várias ferramentas disponíveis, como [Explorador de servidores no Visual Studio](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) e [Explorador de armazenamento do Azure](../vs-azure-tools-storage-manage-with-storage-explorer.md).

Pode optar por coletar:

* Um conjunto predefinido de contadores de desempenho do sistema operativo e logs de eventos ou pode especificar quais quer coletar. 
* Todos os pedidos de e/ou pedidos falhados para um servidor web IIS
* Aplicação .NET nos registos de saída de rastreio
* Rastreamento de eventos para eventos do Windows (ETW) 
* Recolher eventos de registo do syslog  
* Informações de falha de sistema 

Dados podem ser reencaminhados como alternativa para [Application Insights](../application-insights/app-insights-cloudservices.md), [do Log Analytics](../log-analytics/log-analytics-queries.md), ou para serviços não pertencente ao Azure com [Hub de eventos](../event-hubs/event-hubs-about.md). 

Para a monitorização avançada em que precisa de mais de coleta de métricas e um subconjunto de registos, o agente do Log Analytics para Windows e Linux, é necessário.  Com este agente, é possível utilizar os serviços do Azure, como a automatização e o Log Analytics, incluindo o conjunto completo de recursos que elas oferecem, para fornecer gerenciamento abrangente das suas VMs do Azure através do respetivo ciclo de vida. Isto inclui:

* [Gestão de atualizações de automatização do Azure](../automation/automation-update-management.md) de atualizações do sistema operativo
* [Azure Automation Desired State Configuration](../automation/automation-dsc-overview.md) para manter o estado de configuração consistente
* Controlar as alterações de configuração com [inventário e controlo de alterações de automatização do Azure](../automation/automation-change-tracking.md)
* Recolha de registos personalizados a partir do sistema operacional e aplicativos hospedados como [FluentD](../log-analytics/log-analytics-data-sources-json.md), [registos personalizados](../log-analytics/log-analytics-data-sources-custom-logs.md), [MySQL e Apache](../log-analytics/log-analytics-data-sources-linux-applications.md) com o Log Analytics
* Serviços do Azure, tal como [Application Insights](https://docs.microsoft.com/azure/application-insights/) e [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/) nativamente armazenam os dados diretamente no Log Analytics.  

## <a name="next-steps"></a>Passos Seguintes

- Ver [recolher dados de computadores no seu ambiente com o Log Analytics](../log-analytics/log-analytics-concept-hybrid.md) rever os requisitos e métodos disponíveis para implementar o agente em computadores no seu datacenter ou outro ambiente de cloud.
- Consulte [Recolher dados sobre Máquinas Virtuais do Azure](../log-analytics/log-analytics-quick-collect-azurevm.md) para configurar a recolha de dados a partir de VMs do Azure. 
