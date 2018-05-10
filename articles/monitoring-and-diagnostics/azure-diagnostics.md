---
title: Descrição geral da extensão de diagnóstico do Azure | Microsoft Docs
description: Utilize o diagnóstico do Azure para depuração, medir o desempenho, monitorização, análise de tráfego em serviços em nuvem, as máquinas virtuais e recursos de infraestrutura de serviço
services: multiple
documentationcenter: .net
author: rboucher
manager: ''
editor: ''
ms.assetid: baad40d8-c915-4f93-b486-8b160bf33463
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/01/2018
ms.author: robb
ms.openlocfilehash: daeaddefa461e71fcc62af4efc4fb7084b237cf9
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="what-is-azure-diagnostics-extension"></a>O que é a extensão de diagnóstico do Azure
A extensão de diagnóstico do Azure é um agente no Azure que permite a recolha de dados de diagnóstico sobre uma aplicação implementada. Pode utilizar a extensão de diagnóstico de um número de diferentes origens. São atualmente suportados Web do serviço em nuvem do Azure (clássica) e as funções de trabalho, máquinas virtuais, conjuntos de dimensionamento da Máquina Virtual e o Service Fabric. Outros serviços do Azure têm métodos de diagnóstico diferentes. Consulte [descrição geral de monitorização no Azure](monitoring-overview.md). 

## <a name="linux-agent"></a>Agente Linux
A [versão do Linux da extensão](../virtual-machines/linux/diagnostic-extension.md) está disponível para máquinas virtuais com Linux. O comportamento e estatísticas recolhidas variam entre a versão do Windows. 

## <a name="data-you-can-collect"></a>É possível recolher de dados
A extensão de diagnóstico do Azure pode recolher os seguintes tipos de dados:

| Origem de Dados | Descrição |
| --- | --- |
| Contadores de desempenho |Sistema operativo e os contadores de desempenho personalizado |
| Registos de aplicações |Mensagens de rastreio escritas pela sua aplicação |
| Registos de eventos do Windows |Informações enviadas para o sistema de registo de eventos do Windows |
| Origem de evento de .NET |Código de escrita de eventos utilizando o .NET [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) classe |
| Registos de IIS |Informações sobre sites de web do IIS |
| O manifesto com base em ETW |Eventos de rastreio para o Windows de eventos gerados por qualquer processo |
| Informações de falha de sistema |Informações sobre o estado do processo em caso de uma falha de aplicação |
| Registos de erros personalizados |Registos criados pela sua aplicação ou serviço |
| Registos de diagnóstico de infraestrutura do Azure |Informações sobre diagnósticos próprio |

## <a name="data-storage"></a>Armazenamento de dados
A extensão armazena os respetivos dados num [conta de armazenamento do Azure](azure-diagnostics-storage.md) que especificar. 

Também pode enviá-lo para [Application Insights](../application-insights/app-insights-cloudservices.md). Outra opção consiste em fluxo para [Hub de eventos](../event-hubs/event-hubs-what-is-event-hubs.md), que depois permite-lhe enviar para o Azure não montoring serviços. 


## <a name="versioning-and-configuration-schema"></a>Esquema de configuração e de controlo de versões
Consulte [histórico de versões de diagnóstico do Azure e o esquema](azure-diagnostics-versioning-history.md).


## <a name="next-steps"></a>Passos Seguintes
Escolha o serviço está a tentar recolher diagnósticos no e utilize os artigos seguintes para começar a utilizar. Utilize as ligações de geral de diagnóstico do Azure para referência para tarefas específicas.

## <a name="cloud-services-using-azure-diagnostics"></a>Serviços em nuvem com o diagnóstico do Azure
* Se utilizar o Visual Studio, consulte [utilize o Visual Studio para rastrear uma aplicação de serviços em nuvem](../vs-azure-tools-debug-cloud-services-virtual-machines.md) para começar a utilizar. Caso contrário, consulte
* [Como monitorizar serviços em nuvem com o diagnóstico do Azure](../cloud-services/cloud-services-how-to-monitor.md)
* [Configurar o diagnóstico do Azure numa aplicação de serviços Cloud](../cloud-services/cloud-services-dotnet-diagnostics.md)

Para obter tópicos mais avançados, consulte

* [Utilizar o diagnóstico do Azure com o Application Insights para serviços em nuvem](../application-insights/app-insights-cloudservices.md)
* [Analisar o fluxo de uma aplicação de serviços em nuvem com o diagnóstico do Azure](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
* [Utilize o PowerShell para configurar diagnósticos no serviços Cloud](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="virtual-machines"></a>Virtual Machines
* Se utilizar o Visual Studio, consulte [utilize o Visual Studio para rastreio Virtual Machines do Azure](../vs-azure-tools-debug-cloud-services-virtual-machines.md) para começar a utilizar. Caso contrário, consulte
* [Configurar uma Máquina Virtual no Azure do diagnóstico do Azure](../virtual-machines-dotnet-diagnostics.md)

Para obter tópicos mais avançados, consulte

* [Utilize o PowerShell para configurar diagnósticos em Azure Virtual Machines](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Criar uma Máquina Virtual do Windows com a monitorização e diagnóstico com o modelo do Azure Resource Manager](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric"></a>Service Fabric
Começar a utilizar em [monitorizar uma aplicação de Service Fabric](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Muitos outros artigos de diagnóstico de Service Fabric estão disponíveis na árvore de navegação à esquerda, depois de obter a este artigo.

## <a name="general-articles"></a>Artigos gerais
* Saiba como [utilizar os contadores de desempenho no Azure Diagnostics](../cloud-services/diagnostics-performance-counters.md).
* Se tiver problemas com a iniciar o diagnóstico ou localizar os dados nas tabelas de armazenamento do Azure, consulte [resolução de problemas do diagnóstico do Azure](azure-diagnostics-troubleshooting.md)
