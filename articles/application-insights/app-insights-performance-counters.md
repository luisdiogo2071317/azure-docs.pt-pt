---
title: Contadores de desempenho no Application Insights | Documentos da Microsoft
description: Monitorize o sistema e de contadores de desempenho do .NET personalizados no Application Insights.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 5b816f4c-a77a-4674-ae36-802ee3a2f56d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/11/2016
ms.author: mbullwin
ms.openlocfilehash: bcfb3a52793ba0daca980564d5d2248629b5caf4
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53323017"
---
# <a name="system-performance-counters-in-application-insights"></a>Contadores de desempenho do sistema no Application Insights
Windows fornece uma grande variedade de [contadores de desempenho](http://www.codeproject.com/Articles/8590/An-Introduction-To-Performance-Counters) , como a ocupação da CPU, memória, disco e utilização de rede. Também pode definir seus próprios. [O Application Insights](app-insights-overview.md) pode mostrar esses contadores de desempenho se seu aplicativo seja executado sob o IIS num anfitrião no local ou máquina virtual para o qual tem acesso administrativo. Os gráficos de indicam os recursos disponíveis para a aplicação em direto e podem ajudar a identificar a carga desequilibrada entre instâncias de servidor.

Contadores de desempenho são apresentados no painel de servidores, que inclui uma tabela que segmentos por instância de servidor.

![Contadores de desempenho comunicados no Application Insights](./media/app-insights-performance-counters/counters-by-server-instance.png)

(Contadores de desempenho não estão disponíveis para aplicações Web do Azure. Mas pode [enviar diagnósticos do Azure para o Application Insights](../azure-monitor/platform/diagnostics-extension-to-application-insights.md).)

## <a name="view-counters"></a>Contadores do Vista
O painel de servidores mostra um conjunto predefinido de contadores de desempenho. 

Para ver outros contadores, editar os gráficos no painel de servidores ou abra uma nova [Explorador de métricas](app-insights-metrics-explorer.md) painel e adicionar novos gráficos. 

Os contadores disponíveis estão listados como métricas ao editar um gráfico.

![Contadores de desempenho comunicados no Application Insights](./media/app-insights-performance-counters/choose-performance-counters.png)

Para ver todos os seus gráficos mais úteis num único lugar, crie uma [dashboard](app-insights-dashboards.md) e afixá-los à mesma.

## <a name="add-counters"></a>Adicionar contadores
Se pretender que o contador de desempenho não é mostrado na lista de métricas, isso é porque não está a recolher-o SDK do Application Insights no seu servidor web. Pode configurar para fazer isso.

1. Descubra quais contadores estão disponíveis no seu servidor, utilizando este comando do PowerShell no servidor:
   
    `Get-Counter -ListSet *`
   
    (Consulte [ `Get-Counter` ](https://technet.microsoft.com/library/hh849685.aspx).)
2. Abra o applicationinsights. config.
   
   * Se tiver adicionado o Application Insights à sua aplicação durante o desenvolvimento, edite applicationinsights. config no seu projeto e, em seguida, implementá-lo novamente para os seus servidores.
   * Se tiver utilizado o Monitor de estado para instrumentar uma aplicação web em tempo de execução, encontre o applicationinsights. config no diretório de raiz da aplicação no IIS. Atualize-o aí em cada instância de servidor.
3. Edite a diretiva de recoletor de desempenho:
   
```XML
   
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\Objects\Processes"/>
        <Add PerformanceCounter="\Sales(photo)\# Items Sold" ReportAs="Photo sales"/>
      </Counters>
    </Add>

```

Pode capturar os contadores padrão e aqueles que implementou por conta própria. `\Objects\Processes` é um exemplo de um contador padrão, disponível em todos os sistemas Windows. `\Sales(photo)\# Items Sold` é um exemplo de um contador personalizado que pode ser implementado num serviço web. 

O formato é `\Category(instance)\Counter"`, ou para as categorias que não têm instâncias, just `\Category\Counter`.

`ReportAs` é necessário para nomes de contador que se neshodují `[a-zA-Z()/-_ \.]+` -ou seja, contêm carateres que não estão a ser os seguintes conjuntos: letras, gira entre colchetes, barra, hífen, caráter de sublinhado, espaço, ponto.

Se especificar uma instância, serão recolhido como uma dimensão "CounterInstanceName" da métrica comunicada.

### <a name="collecting-performance-counters-in-code"></a>Recolha de contadores de desempenho no código
Para recolher contadores de desempenho do sistema e enviá-los para o Application Insights, pode adaptar o trecho a seguir:


``` C#

    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\.NET CLR Memory([replace-with-application-process-name])\# GC Handles", "GC Handles")));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```
Ou pode fazer a mesma coisa com métricas personalizadas que criou:

``` C#
    var perfCollectorModule = new PerformanceCollectorModule();
    perfCollectorModule.Counters.Add(new PerformanceCounterCollectionRequest(
      @"\Sales(photo)\# Items Sold", "Photo sales"));
    perfCollectorModule.Initialize(TelemetryConfiguration.Active);
```

## <a name="performance-counters-in-analytics"></a>Contadores de desempenho no Analytics
Pode pesquisar e apresentar relatórios de contador de desempenho na [Analytics](app-insights-analytics.md).

O **performanceCounters** esquema expõe a `category`, `counter` nome, e `instance` nome de cada contador de desempenho.  A telemetria de cada aplicativo, verá apenas os contadores para essa aplicação. Por exemplo, para ver quais contadores estão disponíveis: 

![Contadores de desempenho na análise do Application Insights](./media/app-insights-performance-counters/analytics-performance-counters.png)

(Aqui "Instância" refere-se para a instância do contador de desempenho, não a instância de máquina função ou servidor. O nome de instância do contador de desempenho normalmente segmenta contadores como tempo de processador pelo nome do processo ou aplicação.)

Para obter um gráfico de memória disponível ao longo do período recente: 

![Pego de memória no analytics do Application Insights](./media/app-insights-performance-counters/analytics-available-memory.png)

Como outra telemetria **performanceCounters** também tem uma coluna `cloud_RoleInstance` indica que a identidade de instância do servidor de anfitrião no qual a aplicação está em execução. Por exemplo, para comparar o desempenho da sua aplicação em diferentes computadores: 

![Desempenho segmentados por instância de função no Application Insights analytics](./media/app-insights-performance-counters/analytics-metrics-role-instance.png)

## <a name="aspnet-and-application-insights-counts"></a>ASP.NET e contagens de Application Insights
*O que é a diferença entre a taxa de exceções e métricas de exceções?*

* *Taxa de exceções* é um contador de desempenho do sistema. O CLR conta todas as exceções a processadas e não processadas que são geradas e divide o total num intervalo de amostragem pelo comprimento do intervalo. O SDK do Application Insights recolhe este resultado e envia-os para o portal.
* *Exceções* é uma contagem dos relatórios TrackException recebido pelo portal no intervalo de amostragem do gráfico. Ele inclui apenas as exceções manipuladas em que escreveu TrackException chama em seu código e não incluir a totalidade [exceções não processadas](app-insights-asp-net-exceptions.md). 

## <a name="performance-counters-in-aspnet-core-applications"></a>Contadores de desempenho em aplicativos do Asp.Net Core
Contadores de desempenho são suportados apenas se o aplicativo se destina o .NET Framework completo. Não existe nenhuma capacidade para coletar os contadores de desempenho para o .net aplicativos principais.

## <a name="alerts"></a>Alertas
Como outras métricas, pode [definir um alerta](app-insights-alerts.md) para avisá-lo se um contador de desempenho sair de um limite que especificar. Abra o painel de alertas e clique em Adicionar alerta.

## <a name="next"></a>Passos seguintes
* [Rastreamento de dependências](app-insights-asp-net-dependencies.md)
* [Controlo de exceções](app-insights-asp-net-exceptions.md)

