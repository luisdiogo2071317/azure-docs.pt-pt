---
title: Recolher sobre os contadores de desempenho nos serviços em nuvem do Azure | Microsoft Docs
description: Aprenda a detetar, utilizar e criar os contadores de desempenho nos serviços em nuvem com o Azure Diagnostics e o Application Insights.
services: cloud-services
documentationcenter: .net
author: thraka
manager: timlt
editor: ''
ms.assetid: ''
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/18
ms.author: adegeo
ms.openlocfilehash: 3e0af48c172fa912f0ac9e05b7b761dd7eaad795
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
ms.locfileid: "29134352"
---
# <a name="collect-performance-counters-for-your-azure-cloud-service"></a>Recolher contadores de desempenho do serviço em nuvem do Azure

Contadores de desempenho fornecem uma forma de controlar o quão bem a aplicação e o anfitrião estão a efetuar. Windows Server fornece muitos diferentes contadores de desempenho relacionados com hardware, aplicações, o sistema operativo e muito mais. Recolha e envio de contadores de desempenho para o Azure, pode analisar estas informações para ajudar a tomar decisões melhores. 

## <a name="discover-available-counters"></a>Detetar contadores disponíveis

Um contador de desempenho é constituído por duas partes: um nome de conjunto (também conhecido como uma categoria) e os contadores de um ou mais. Pode utilizar o PowerShell para obter uma lista de contadores de desempenho disponíveis:

```PowerShell
Get-Counter -ListSet * | Select-Object CounterSetName, Paths | Sort-Object CounterSetName

CounterSetName                                  Paths
--------------                                  -----
.NET CLR Data                                   {\.NET CLR Data(*)\SqlClient...
.NET CLR Exceptions                             {\.NET CLR Exceptions(*)\# o...
.NET CLR Interop                                {\.NET CLR Interop(*)\# of C...
.NET CLR Jit                                    {\.NET CLR Jit(*)\# of Metho...
.NET Data Provider for Oracle                   {\.NET Data Provider for Ora...
.NET Data Provider for SqlServer                {\.NET Data Provider for Sql...
.NET Memory Cache 4.0                           {\.NET Memory Cache 4.0(*)\C...
AppV Client Streamed Data Percentage            {\AppV Client Streamed Data ...
ASP.NET                                         {\ASP.NET\Application Restar...
ASP.NET Apps v4.0.30319                         {\ASP.NET Apps v4.0.30319(*)...
ASP.NET State Service                           {\ASP.NET State Service\Stat...
ASP.NET v2.0.50727                              {\ASP.NET v2.0.50727\Applica...
ASP.NET v4.0.30319                              {\ASP.NET v4.0.30319\Applica...
Authorization Manager Applications              {\Authorization Manager Appl...

#... results cut to save space ...
```

O `CounterSetName` propriedade representa um conjunto (ou categoria) e é um bom indicador de que os contadores de desempenho relacionados com. O `Paths` propriedade representa uma coleção de contadores para um conjunto. Pode também obter a `Description` propriedade para obter mais informações sobre o conjunto de contadores.

Para obter todos os contadores para um conjunto, utilize o `CounterSetName` valor e expanda o `Paths` coleção. Cada item do caminho é um contador, pode consultar. Por exemplo, para obter os contadores disponíveis relacionados com o `Processor` definido, expanda o `Paths` coleção:

```PowerShell
Get-Counter -ListSet * | Where-Object CounterSetName -eq "Processor" | Select -ExpandProperty Paths

\Processor(*)\% Processor Time
\Processor(*)\% User Time
\Processor(*)\% Privileged Time
\Processor(*)\Interrupts/sec
\Processor(*)\% DPC Time
\Processor(*)\% Interrupt Time
\Processor(*)\DPCs Queued/sec
\Processor(*)\DPC Rate
\Processor(*)\% Idle Time
\Processor(*)\% C1 Time
\Processor(*)\% C2 Time
\Processor(*)\% C3 Time
\Processor(*)\C1 Transitions/sec
\Processor(*)\C2 Transitions/sec
\Processor(*)\C3 Transitions/sec
```

Estes caminhos podem ser adicionados para a estrutura de diagnóstico de contador individuais utiliza o seu serviço em nuvem. Para obter mais informações sobre como é criado um caminho de contador de desempenho, consulte [especificando um caminho de contador](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85)).

## <a name="collect-a-performance-counter"></a>Recolher um contador de desempenho

Um contador de desempenho pode ser adicionado ao seu serviço de nuvem para o diagnóstico do Azure ou para o Application Insights.

### <a name="application-insights"></a>Application Insights

Azure Application Insights para serviços em nuvem permite que especificar que os contadores de desempenho que pretende recolher. Depois de [adicionar o Application Insights ao seu projeto](../application-insights/app-insights-cloudservices.md#sdk), um ficheiro de configuração com o nome **Applicationinsights** é adicionado ao seu projeto de Visual Studio. Este ficheiro de configuração define o tipo de informações Application Insights recolhe e envia para o Azure.

Abra o **Applicationinsights** de ficheiros e localize o **ApplicationInsights** > **TelemetryModules** elemento. Cada `<Add>` elemento subordinado define um tipo de telemetria para recolher, juntamente com a respetiva configuração. O tipo de módulo de telemetria de contador de desempenho é `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector`. Se este elemento já está definido, não o adicione uma segunda vez. Cada contador de desempenho para recolher está definido um nó com o nome `<Counters>`. Eis um exemplo que recolhe os contadores de desempenho de unidade:

```xml
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">

  <TelemetryModules>

    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <Add PerformanceCounter="\LogicalDisk(C:)\Disk Write Bytes/sec" ReportAs="Disk write (C:)" />
        <Add PerformanceCounter="\LogicalDisk(C:)\Disk Read Bytes/sec" ReportAs="Disk read (C:)" />
      </Counters>
    </Add>

  </TelemetryModules>

<!-- ... cut to save space ... -->
```

Cada contador de desempenho é representada como uma `<Add>` elemento em `<Counters>`. O `PerformanceCounter` atributo define o contador de desempenho para recolher. O `ReportAs` atributo é o título para apresentar no portal do Azure para o contador de desempenho. Qualquer contador de desempenho que recolhe é inserida uma categoria com o nome **personalizada** no portal. Ao contrário do diagnóstico do Azure, não é possível definir o intervalo, estes contadores de desempenho são recolhidos e enviados para o Azure. Com o Application Insights, contadores de desempenho são recolhidos e enviados a cada minuto. 

Application Insights recolhe automaticamente os seguintes contadores de desempenho:

* \Process(??APP_WIN32_PROC??)\% Hora do Processador
* \Memory\Available Bytes
* \.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec
* \Process(??APP_WIN32_PROC??)\Private Bytes
* \Process(??APP_WIN32_PROC??)\IO Data Bytes/sec
* \Processor(_Total)\% Processor Time

Para obter mais informações, consulte [contadores de desempenho do sistema no Application Insights](../application-insights/app-insights-performance-counters.md) e [Application Insights para Cloud Services do Azure](../application-insights/app-insights-cloudservices.md#performance-counters).

### <a name="azure-diagnostics"></a>Diagnóstico do Azure

> [!IMPORTANT]
> Enquanto são agregados todos os dados para a conta de armazenamento, o portal **não** proporcionam uma forma nativa os dados de gráfico. Recomenda-se vivamente que integrar outro serviço de diagnóstico, como o Application Insights, na sua aplicação.

A extensão de diagnóstico do Azure para serviços em nuvem permite que especificar que os contadores de desempenho que pretende recolher. Para configurar diagnósticos do Azure, consulte [monitorização descrição geral do serviço em nuvem](cloud-services-how-to-monitor.md#setup-diagnostics-extension).

Os contadores de desempenho que pretende recolher estão definidos no **diagnostics.wadcfgx** ficheiro. Abrir este ficheiro (que é definida por função) no Visual Studio e localize o **DiagnosticsConfiguration** > **PublicConfig** > **WadCfg**  >  **DiagnosticMonitorConfiguration** > **PerformanceCounters** elemento. Adicione um novo **PerformanceCounterConfiguration** elemento como elemento subordinado. Este elemento tem dois atributos: `counterSpecifier` e `sampleRate`. O `counterSpecifier` atributo define o contador definido (delineado na secção anterior) para recolher o desempenho do sistema. O `sampleRate` valor indica com que frequência é consultado esse valor. Como um todo, todos os contadores de desempenho são transferidos para o Azure, de acordo com o elemento principal `PerformanceCounters` do elemento `scheduledTransferPeriod` valor de atributo.

Para obter mais informações sobre o `PerformanceCounters` elemento de esquema, consulte o [Azure Diagnostics esquema](../monitoring-and-diagnostics/azure-diagnostics-schema-1dot3-and-later.md#performancecounters-element).

O período definido pelo `sampleRate` utiliza de atributo do tipo de dados de duração XML para indicar a frequência é consultado o contador de desempenho. No exemplo abaixo, a taxa está definida como `PT3M`, que significa `[P]eriod[T]ime[3][M]inutes`: a cada três minutos.

Para obter mais informações sobre como o `sampleRate` e `scheduledTransferPeriod` são definidos, consulte o **tipo de dados de duração** secção o [W3 XML data e hora data tipos](https://www.w3schools.com/XML/schema_dtypes_date.asp) tutorial.

```xml
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration  xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig>
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096">

        <!-- ... cut to save space ... -->

        <PerformanceCounters scheduledTransferPeriod="PT1M">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />

          <!-- This is a new perf counter which will track the C: disk read activity in bytes per second, every minute -->
          <PerformanceCounterConfiguration counterSpecifier="\LogicalDisk(C:)\Disk Read Bytes/sec" sampleRate="PT1M" />

        </PerformanceCounters>
      </DiagnosticMonitorConfiguration>
    </WadCfg>
    
    <!-- ... cut to save space ... -->

  </PublicConfig>
</DiagnosticsConfiguration>
```

## <a name="create-a-new-perf-counter"></a>Criar um novo contador de desempenho

Um novo contador de desempenho pode ser criado e utilizado pelo seu código. O código que cria um novo contador de desempenho deve estar em execução elevado, caso contrário, ocorrerá uma falha. O serviço em nuvem `OnStart` código de arranque, pode criar o contador de desempenho, a necessidade de executar a função de um contexto elevados. Ou pode criar uma tarefa de arranque executada elevada, que cria o contador de desempenho. Para obter mais informações sobre tarefas de arranque, consulte [como configurar e executar tarefas de arranque para um serviço em nuvem](cloud-services-startup-tasks.md).

Para configurar a sua função de execução elevada, adicione um `<Runtime>` elemento para que o [. csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) ficheiro.

```xml
<ServiceDefinition name="CloudServiceLoadTesting" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRoleWithSBQueue1" vmsize="Large">

    <!-- ... cut to save space ... -->

    <Runtime executionContext="elevated">
      
    </Runtime>

    <!-- ... cut to save space ... -->

  </WorkerRole>
</ServiceDefinition>
```

Pode criar e registar um novo contador de desempenho com poucas linhas de código. Utilize o `System.Diagnostics.PerformanceCounterCategory.Create` sobrecarga do método que cria a categoria e o contador. O seguinte código verifica primeiro se a categoria existe e se estiverem em falta, cria a categoria e o contador.

```csharp
using System.Diagnostics;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRoleWithSBQueue1
{
    public class WorkerRole : RoleEntryPoint
    {
        // Perf counter variable representing times service was used.
        private PerformanceCounter counterServiceUsed;

        public override bool OnStart()
        {
            // ... Other startup code here ...

            // Define the cateogry and counter names.
            string perfCounterCatName = "MyService";
            string perfCounterName = "Times Used";

            // Create the counter if needed. Our counter category only has a single counter.
            // Both the category and counter are created in the same method call.
            if (!PerformanceCounterCategory.Exists(perfCounterCatName))
            {
                PerformanceCounterCategory.Create(perfCounterCatName, "Collects information about the cloud service.", 
                                                  PerformanceCounterCategoryType.SingleInstance, 
                                                  perfCounterName, "How many times the cloud service was used.");
            }

            // Get reference to our counter
            counterServiceUsed = new PerformanceCounter(perfCounterCatName, perfCounterName);
            counterServiceUsed.ReadOnly = false;
            
            return base.OnStart();
        }

        // ... cut class code to save space
    }
}
```

Quando pretender utilizar o contador, chame o `Increment` ou `IncrementBy` método.

```csharp
// Increase the counter by 1
counterServiceUsed.Increment();
```

Agora que a aplicação utiliza o contador personalizado, terá de configurar o diagnóstico do Azure ou do Application Insights para controlar o contador.


### <a name="application-insights"></a>Application Insights

Como anteriormente indicado, os contadores de desempenho para o Application Insights estão definidas no **Applicationinsights** ficheiro. Abra **Applicationinsights** e localize o **ApplicationInsights** > **TelemetryModules** > **adicionar**  >  **Contadores** elemento. Criar um `<Add>` elemento subordinado e defina o `PerformanceCounter` atributo para a categoria e o nome do contador de desempenho que criou no seu código. Definir o `ReportAs` atributo para um nome amigável que pretende ver no portal.

```xml
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">

  <TelemetryModules>

    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule, Microsoft.AI.PerfCounterCollector">
      <Counters>
        <!-- ... cut other perf counters to save space ... -->

        <!-- This new perf counter matches the [category name]\[counter name] defined in your code -->
        <Add PerformanceCounter="\MyService\Times Used" ReportAs="Service used counter" />
      </Counters>
    </Add>

  </TelemetryModules>

<!-- ... cut to save space ... -->
```

### <a name="azure-diagnostics"></a>Diagnóstico do Azure

Conforme indicado anteriormente, os contadores de desempenho que pretende recolher estão definidos no **diagnostics.wadcfgx** ficheiro. Abrir este ficheiro (que é definida por função) no Visual Studio e localize o **DiagnosticsConfiguration** > **PublicConfig** > **WadCfg**  >  **DiagnosticMonitorConfiguration** > **PerformanceCounters** elemento. Adicione um novo **PerformanceCounterConfiguration** elemento como elemento subordinado. Definir o `counterSpecifier` atributo para a categoria e o nome do contador de desempenho que criou no seu código. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration  xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig>
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096">

        <!-- ... cut to save space ... -->

        <PerformanceCounters scheduledTransferPeriod="PT1M">
          <!-- ... cut other perf counters to save space ... -->
          
          <!-- This new perf counter matches the [category name]\[counter name] defined in your code -->
          <PerformanceCounterConfiguration counterSpecifier="\MyService\Times Used" sampleRate="PT1M" />

        </PerformanceCounters>
      </DiagnosticMonitorConfiguration>
    </WadCfg>
    
    <!-- ... cut to save space ... -->

  </PublicConfig>
</DiagnosticsConfiguration>
```

## <a name="more-information"></a>Mais informações

- [Application Insights para serviços em nuvem do Azure](../application-insights/app-insights-cloudservices.md#performance-counters)
- [Contadores de desempenho do sistema no Application Insights](../application-insights/app-insights-performance-counters.md)
- [Especificar um caminho de contador](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85))
- [Esquema de diagnóstico do Azure - contadores de desempenho](../monitoring-and-diagnostics/azure-diagnostics-schema-1dot3-and-later.md#performancecounters-element)