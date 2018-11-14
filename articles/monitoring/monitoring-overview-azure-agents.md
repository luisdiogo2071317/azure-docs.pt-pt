---
title: Descrição geral do Azure agentes de monitorização | Documentos da Microsoft
description: Este artigo fornece uma visão geral detalhada dos agentes do Azure disponíveis que suporte máquinas virtuais alojadas num ambiente híbrido do Azure ou de monitorização.
services: azure-monitor
documentationcenter: azure-monitor
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: magoedte
ms.openlocfilehash: ce6f40e580595e4f3fbf0519aa1ba8be0355ded1
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51621910"
---
# <a name="overview-of-the-azure-monitoring-agents"></a>Descrição geral do Azure agentes de monitorização 
O Microsoft Azure fornece várias formas de recolher diferentes tipos de dados de máquinas virtuais alojadas no Azure ou noutros fornecedores de cloud com o Microsoft Windows e Linux. Este artigo o ajudará a descrever as diferenças e as capacidades disponíveis com cada agente para poder determinar qual deles irá suportar a gestão de serviço de TI ou gerais de requisitos de monitorização.  

## <a name="comparing-agents"></a>Comparação entre agentes
Hoje em dia no Azure, existem três tipos de agentes disponíveis para monitorizar uma VM do Azure - a extensão de diagnóstico do Azure, o agente de dependência e o Log Analytics Agent para Linux e Windows.  Essencialmente, a extensão de dependência do Azure e os agentes do Log Analytics são concebidos para recolher métricas e registos e reencaminhar para um repositório. No entanto, que é onde as suas semelhanças terminam.  

### <a name="azure-diagnostic-extension"></a>Extensão de diagnóstico do Azure
O [extensão de diagnóstico do Azure](../monitoring-and-diagnostics/azure-diagnostics.md) (geralmente conhecida como a extensão de diagnóstico do Windows Azure (WAD) ou o diagnóstico do Linux do Azure (LAD)), que é fornecido para Cloud Services do Azure, uma vez que tornou-se em disponibilidade geral em 2010, é um agente que oferece uma coleção simples de dados de diagnóstico de um recurso de computação do Azure, como uma VM e mantê-los ao armazenamento do Azure. Uma vez no armazenamento, que escolheu para ver uma das várias ferramentas disponíveis, como [Explorador de servidores no Visual Studio](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) e [Explorador de armazenamento do Azure](../vs-azure-tools-storage-manage-with-storage-explorer.md).

Pode optar por coletar:

* Um conjunto predefinido de contadores de desempenho do sistema operativo e logs de eventos ou pode especificar quais quer coletar. 
* Todos os pedidos de e/ou pedidos falhados para um servidor web IIS
* Aplicação .NET nos registos de saída de rastreio
* Rastreamento de eventos para eventos do Windows (ETW) 
* Recolher eventos de registo do syslog  
* Informações de falha de sistema 

O agente de diagnóstico do Azure deve ser utilizado quando:

* Pretende arquivar os registos e métricas para o armazenamento do Azure
* Integre dados de monitorização com as ferramentas de terceiros. Essas ferramentas usam uma variedade de métodos, incluindo consultar reencaminhada para a conta de armazenamento [os Hubs de eventos](../event-hubs/event-hubs-about.md), ou consultar com o [API de REST de monitorização do Azure](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md)
* Carregar dados para o Azure Monitor para criar gráficos de métricas no portal do Azure ou crie quase em tempo real [alertas de métricas](../monitoring-and-diagnostics/alert-metric-overview.md). 
* Os conjuntos de dimensionamento de máquinas virtuais do dimensionamento automático e os serviços de Cloud clássico com base em métricas de SO convidado.
* Investigar problemas de arranque de VMS com [diagnóstico de arranque](../virtual-machines/troubleshooting/boot-diagnostics.md).
* Compreender o desempenho das suas aplicações e identifica proativamente os problemas que afetam-los com [Application Insights](../azure-monitor/overview.md).
* Configurar o Log Analytics para importar as métricas e registos de dados recolhidos a partir de serviços Cloud, as VMs clássicas, e nós do Service Fabric armazenados numa conta de armazenamento do Azure.

### <a name="log-analytics-agent"></a>Agente do log Analytics
Para a monitorização avançada em que precisa de mais de coleta de métricas e um subconjunto de registos, o agente do Log Analytics para Windows e Linux, é necessário. O agente Log Analytics foi desenvolvido para gerenciamento abrangente em locais máquinas físicas e virtuais, computadores monitorizados pelo System Center Operations Manager e VMs no alojados noutras Clouds. Ligar os agentes Windows e Linux para uma área de trabalho do Log Analytics para recolher os dados com base em solução de monitorização, bem como origens de dados personalizado que configurou.

[!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

O agente Log Analytics deve ser utilizado quando:

* Tem máquinas em execução no local ou outro ambiente de cloud
* Utilizar uma das soluções de monitorização, tais como o Azure Monitor [do Azure Monitor para VMs](../monitoring/monitoring-vminsights-overview.md?toc=%2fazure%2fmonitoring%2ftoc.json), [Monitor do Azure para contentores](../monitoring/monitoring-container-insights-overview.md?toc=%2fazure%2fmonitoring%2ftoc.json), etc.  
* Com um dos outros serviços de gestão do Azure, como [Centro de segurança do Azure](../security-center/security-center-intro.md), [automatização do Azure](../automation/automation-intro.md), etc.
* A carregar registo de e/ou dados métricos para o Azure Monitor.

Anteriormente, os vários serviços do Azure foram agrupados como o *Operations Management Suite*, e assim o agente Log Analytics é partilhado entre os serviços, incluindo o Centro de segurança do Azure e o automatização do Azure.  Isto inclui o conjunto completo de recursos que elas oferecem, fornecendo gerenciamento abrangente das suas VMs do Azure através do respetivo ciclo de vida. Isto inclui:

* [Gestão de atualizações de automatização do Azure](../automation/automation-update-management.md) de atualizações do sistema operativo.
* [Azure Automation Desired State Configuration](../automation/automation-dsc-overview.md) para manter o estado de configuração consistentes.
* Controlar as alterações de configuração com [controlo de alterações de automatização do Azure e de inventário](../automation/automation-change-tracking.md).
* Recolha de registos personalizados a partir do sistema operacional e aplicativos hospedados como [FluentD](../log-analytics/log-analytics-data-sources-json.md), [registos personalizados](../log-analytics/log-analytics-data-sources-custom-logs.md), e [MySQL e Apache](../log-analytics/log-analytics-data-sources-linux-applications.md) com o Log Analytics.
* Serviços do Azure, tal como [Application Insights](https://docs.microsoft.com/azure/application-insights/) e [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/) nativamente armazenam os dados diretamente no Log Analytics.  

### <a name="dependency-agent"></a>Agente de Dependência
O agente de dependência foi desenvolvido como parte da solução mapa de serviço, que foi originalmente desenvolvida externamente da Microsoft. [Mapa de serviço](../monitoring/monitoring-service-map.md) e [do Azure Monitor para VMs](monitoring-vminsights-overview.md) requer um agente de dependência no Windows e Linux máquinas virtuais e ele se integra com o agente do Log Analytics recolhe detetados dados sobre os processos em execução no virtual máquina e dependências de processo externo. Ele armazena estes dados no Log Analytics e visualiza os componentes interconectados detetados.

Poderá ter alguma combinação destes agentes para monitorizar a sua VM. Os agentes podem ser instalados lado a lado como extensões do Azure, no entanto no Linux, o agente Log Analytics *tem* ser instalado primeiro, caso contrário instalação irá falhar. 

## <a name="next-steps"></a>Passos Seguintes

- Ver [descrição geral do agente do Log Analytics](../log-analytics/log-analytics-agent-overview.md) rever os requisitos e métodos suportados para implementar o agente aos computadores alojados no Azure, no seu datacenter ou outro ambiente de cloud.

