---
title: Descrição geral do Azure, os agentes de monitorização | Microsoft Docs
description: Este artigo fornece uma descrição detalhada dos agentes do Azure disponíveis que suportam a monitorização de VMs do Azure.
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
ms.openlocfilehash: a399c3968e5ee1e2d1f6d623a68dbb1e15cef212
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37088643"
---
# <a name="overview-of-the-azure-agents-to-monitor-azure-virtual-machines"></a>Descrição geral dos agentes do Azure para monitorizar máquinas virtuais do Azure
Microsoft Azure fornece várias formas para recolher os diferentes tipos de dados de máquinas virtuais alojadas no Azure ou de outros fornecedores de nuvem com o Microsoft Windows e Linux.  Este artigo ajuda descrevem as diferenças e capacidades disponíveis com cada agente por ordem para determinar qual deles irá suportar a gestão de serviço ou gerais de requisitos de monitorização.  

## <a name="comparing-azure-diagnostic-and-log-analytics-agent"></a>Comparar o agente do Azure diagnóstico e análise de registos
Hoje em dia no Azure, existem dois tipos de agentes disponíveis para monitorizar uma VM do Azure - a extensão de diagnóstico do Azure e o agente de análise do registo para Linux e Windows.  Fundamentalmente, estes agentes são concebidos para recolher métricas e registos e reencaminhá-los para um repositório. No entanto, que é onde os respetivos semelhanças terminar.  

O [extensão de diagnóstico do Azure](../monitoring-and-diagnostics/azure-diagnostics.md), que foi fornecido para uma Cloud Services do Azure, uma vez que está normalmente disponível no 2010, é um agente que fornece uma coleção simple de dados de diagnóstico de um recurso de IaaS do Azure como uma VM, e manter storage do Azure.  Uma vez no armazenamento, que escolheu ver com uma das várias ferramentas disponíveis, tais como [Explorador de servidores no Visual Studio](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md) e [Explorador de armazenamento do Azure](../vs-azure-tools-storage-manage-with-storage-explorer.md).

Pode optar por recolher:

* Um conjunto predefinido de contadores de desempenho do sistema operativo e os registos de eventos ou pode especificar que pretende recolher. 
* Todos os pedidos de e/ou pedidos falhados para o servidor web do IIS
* Saída de registos de rastreio de aplicações de .NET
* Rastreio de eventos para os eventos do Windows (ETW) 
* Recolher eventos de registo do syslog  
* Informações de falha de sistema 

Dados possam ser reencaminhados como alternativa para [Application Insights](../application-insights/app-insights-cloudservices.md), [Log Analytics](../log-analytics/log-analytics-overview.md), ou aos serviços do Azure não com [Hub de eventos](../event-hubs/event-hubs-what-is-event-hubs.md). 

Para monitorização avançada onde precisa de mais do que recolher métricas e um subconjunto de registos, é necessário o agente de análise de registos para o Windows e Linux.  Com este agente, conseguir utilizam os serviços do Azure, tais como a automatização e análise de registos, incluindo o conjunto completo de funcionalidades que oferecem, para fornecer gestão abrangente de suas VMs do Azure através do respetivo ciclo de vida. Isto inclui:

* [Gestão de atualizações de automatização do Azure](../automation/automation-update-management.md) de atualizações do sistema operativo
* [Configuração de estado de Desired de automatização do Azure](../automation/automation-dsc-overview.md) para manter o estado de configuração consistentes
* Controlar as alterações de configuração com [inventário e de controlo de alterações de automatização do Azure](../automation/automation-change-tracking.md)
* Coleção de registos personalizados do SO e aplicações hospedadas como [FluentD](../log-analytics/log-analytics-data-sources-json.md), [registos personalizados](../log-analytics/log-analytics-data-sources-custom-logs.md), [MySQL e Apache](../log-analytics/log-analytics-data-sources-linux-applications.md) com a análise de registos
* Azure tal como serviços [Application Insights](https://docs.microsoft.com/azure/application-insights/) e [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/) nativamente armazenam os seus dados diretamente na análise de registos.  

## <a name="next-steps"></a>Passos Seguintes

- Consulte [recolher dados de computadores no seu ambiente com a análise de registos](../log-analytics/log-analytics-concept-hybrid.md) para rever os requisitos e métodos disponíveis para implementar o agente em computadores no seu centro de dados ou outro ambiente de nuvem.
- Consulte [Recolher dados sobre Máquinas Virtuais do Azure](../log-analytics/log-analytics-quick-collect-azurevm.md) para configurar a recolha de dados a partir de VMs do Azure. 