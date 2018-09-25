---
title: Descrição geral da extensão do diagnóstico do Azure
description: Utilizar o diagnóstico do Azure para depuração, avaliar o desempenho, monitoramento, análise de tráfego nos serviços cloud, máquinas virtuais e o service fabric
services: azure-monitor
author: rboucher
ms.service: azure-monitor
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: robb
ms.component: diagnostic-extension
ms.openlocfilehash: ef1422db799db6d635ad9f03908e3a34f312e408
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46974243"
---
# <a name="what-is-azure-diagnostics-extension"></a>O que é a extensão de diagnóstico do Azure
A extensão de diagnóstico do Azure é um agente no Azure que permite a recolha de dados de diagnóstico num aplicativo implantado. Pode utilizar a extensão de diagnóstico de várias origens diferentes. Atualmente suportados são Web do serviço de Cloud do Azure (clássico) e funções de trabalho, máquinas virtuais, conjuntos de dimensionamento de máquinas virtuais e o Service Fabric. Outros serviços do Azure têm métodos de diagnóstico diferentes. Ver [descrição geral da monitorização no Azure](monitoring-overview.md).

## <a name="linux-agent"></a>Agente Linux
R [Linux versão da extensão](../virtual-machines/linux/diagnostic-extension.md) está disponível para máquinas virtuais em execução no Linux. As estatísticas recolhidas e comportamento variam em relação a versão do Windows.

## <a name="data-you-can-collect"></a>Pode recolher de dados
A extensão de diagnóstico do Azure pode recolher os seguintes tipos de dados:

| Origem de Dados | Descrição |
| --- | --- |
| Contadores de desempenho |Sistema operativo e contadores de desempenho personalizado |
| Registos de Aplicações |Mensagens de rastreio escritas pelo seu aplicativo |
| Registos de Eventos do Windows |Informações enviadas para o sistema de Registro em log de eventos do Windows |
| Origem de evento de .NET |Código de escrita de eventos usando o .NET [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) classe |
| Registos do IIS |Informações sobre os web sites do IIS |
| ETW com base em manifesto |Eventos de rastreio para o Windows de eventos gerados por algum processo. (1) |
| Informações de falha de sistema |Informações sobre o estado do processo em caso de uma falha de aplicativo |
| Registos de erros personalizados |Registos criados pela sua aplicação ou serviço |
| Registos de infraestrutura de diagnóstico do Azure |Informações sobre o diagnóstico em si |

(1) para obter uma lista de fornecedores ETW, execute `c:\Windows\System32\logman.exe query providers` numa janela de consola no computador que pretende recolher informações de.

## <a name="data-storage"></a>Armazenamento de dados
A extensão armazena os respetivos dados num [conta de armazenamento do Azure](azure-diagnostics-storage.md) que especificar.

Também pode enviá-lo para [Application Insights](../application-insights/app-insights-cloudservices.md). Outra opção é transmiti-lo para [Hub de eventos](../event-hubs/event-hubs-what-is-event-hubs.md), que depois permite-lhe enviar para os serviços de monitorização não pertencente ao Azure.

### <a name="azure-monitor"></a>Azure Monitor
Tem também a opção de enviar seus dados para o Azure Monitor. Neste momento, este sink só é aplicável a contadores de desempenho. Permite-lhe enviar os contadores de desempenho recolhidos na sua VM, VMSS, ou serviço para o Azure Monitor, como métricas personalizadas em nuvem. Suporta o sink do Azure Monitor:
* Obter todos os contadores de desempenho enviados para o Azure Monitor através do [métricas do Azure Monitor APIs.](https://docs.microsoft.com/rest/api/monitor/)
* Alertas em todos os contadores de desempenho enviados para o Azure Monitor através do novo [experiência de alertas unificada](monitoring-overview-unified-alerts.md) no Azure Monitor
* Ao tratar o operador de caráter universal no contadores de desempenho como a dimensão de "Instância" em sua métrica.  Por exemplo, se recolheu o "disco lógico (\*) / DiskWrites/seg" contador deve ser capaz de filtro e dividir a dimensão de "Instância" plotagem ou alertar relativamente a escritas de disco/seg para cada disco lógico na VM (c:, d:, etc.)

Para obter mais informações sobre como configurar este sink, veja o [documentação de esquema de diagnóstico do Azure.](azure-diagnostics-schema-1dot3-and-later.md)

## <a name="versioning-and-configuration-schema"></a>Esquema de controlo de versões e a configuração
Ver [histórico de versões do diagnóstico do Azure e o esquema](azure-diagnostics-versioning-history.md).


## <a name="next-steps"></a>Passos Seguintes
Escolha qual serviço está a tentar recolher diagnósticos em e utilize os seguintes artigos para começar a utilizar. Utilize as ligações de gerais de diagnóstico do Azure para referência para tarefas específicas.

## <a name="cloud-services-using-azure-diagnostics"></a>Serviços cloud com o diagnóstico do Azure
* Se utilizar o Visual Studio, consulte [Use o Visual Studio para rastrear um aplicativo de serviços Cloud](../vs-azure-tools-debug-cloud-services-virtual-machines.md) para começar a utilizar. Caso contrário, consulte
* [Como monitorizar serviços em nuvem com o diagnóstico do Azure](../cloud-services/cloud-services-how-to-monitor.md)
* [Configurar o diagnóstico do Azure numa aplicação de serviços Cloud](../cloud-services/cloud-services-dotnet-diagnostics.md)

Para tópicos mais avançados, consulte

* [Utilizar o diagnóstico do Azure com o Application Insights para serviços em nuvem](../application-insights/app-insights-cloudservices.md)
* [Rastrear o fluxo de um aplicativo de serviços em nuvem com o diagnóstico do Azure](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
* [Utilize o PowerShell para configurar diagnósticos nos serviços Cloud](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="virtual-machines"></a>Virtual Machines
* Se utilizar o Visual Studio, consulte [Use o Visual Studio para máquinas virtuais do Azure de rastreio](../vs-azure-tools-debug-cloud-services-virtual-machines.md) para começar a utilizar. Caso contrário, consulte
* [Configurar o diagnóstico do Azure numa máquina Virtual do Azure](../virtual-machines-dotnet-diagnostics.md)

Para tópicos mais avançados, consulte

* [Utilize o PowerShell para configurar diagnósticos em máquinas de virtuais do Azure](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Criar uma Máquina Virtual do Windows com monitorização e diagnóstico com o modelo do Azure Resource Manager](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric"></a>Service Fabric
Comece a utilizar [monitorizar uma aplicação do Service Fabric](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Muitos outros artigos de diagnóstico de Service Fabric estão disponíveis na árvore de navegação à esquerda depois de obter a este artigo.

## <a name="general-articles"></a>Artigos gerais
* Aprenda a [utilizar contadores de desempenho no diagnóstico do Azure](../cloud-services/diagnostics-performance-counters.md).
* Se tiver problemas com a partir de diagnóstico ou localizar os seus dados nas tabelas de armazenamento do Azure, consulte [resolução de problemas do diagnóstico do Azure](azure-diagnostics-troubleshooting.md)
