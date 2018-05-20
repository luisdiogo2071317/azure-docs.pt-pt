---
title: Azure Application Insights para aplicações de consola | Microsoft Docs
description: Monitorizar aplicações web de disponibilidade, desempenho e utilização.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: lmolkova; mbullwin
ms.openlocfilehash: 679a5d82fbede4d9c464e137d615fc1367522878
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="application-insights-for-net-console-applications"></a>Aplicações de consola do Application Insights para .NET
[Application Insights](app-insights-overview.md) permite-lhe monitorizar a sua aplicação web de disponibilidade, desempenho e utilização.

Necessita de uma subscrição com [Microsoft Azure](http://azure.com). Inicie sessão com uma conta Microsoft, o que poderá ter de Windows, Xbox Live ou outros serviços em nuvem da Microsoft. A equipa pode ter uma subscrição organizacional do Azure: peça ao proprietário para adicioná-lo utilizando a sua conta Microsoft.

## <a name="getting-started"></a>Introdução

* No [portal do Azure](https://portal.azure.com), [crie um recurso do Application Insights](app-insights-create-new-resource.md). Para o tipo de aplicação, escolha **geral**.
* Faça uma cópia da Chave de Instrumentação. Localizar a chave no **Essentials** pendente do novo recurso que criou. 
* Instalar a versão mais recente [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) pacote.
* Defina a chave de instrumentação no seu código antes de qualquer telemetria de controlo (ou variável de ambiente APPINSIGHTS_INSTRUMENTATIONKEY conjunto). Depois disso, deve ser capaz de manualmente controlar telemetria e vê-lo no portal do Azure

```csharp
TelemetryConfiguration.Active.InstrumentationKey = " *your key* ";
var telemetryClient = new TelemetryClient();
telemetryClient.TrackTrace("Hello World!");
```

* Instalar a versão mais recente do [Microsoft.ApplicationInsights.DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) pacote - controla automaticamente HTTP, SQL Server ou outras chamadas de dependências externas.

Pode iniciar e configurar o Application Insights do código ou utilização `ApplicationInsights.config` ficheiro. Certifique-se a inicialização acontece antecipadamente quanto possível. 

> [!NOTE]
> As instruções que faça referência a **Applicationinsights** só são aplicáveis às aplicações que estão a filtrar .NET padrão e não se aplicam a aplicações de .NET Core. 

### <a name="using-config-file"></a>Utilizar o ficheiro de configuração

Por predefinição, Application Insights SDK procura `ApplicationInsights.config` ficheiro no diretório de trabalho quando `TelemetryConfiguration` está a ser criada

```csharp
TelemetryConfiguration config = TelemetryConfiguration.Active; // Reads ApplicationInsights.config file if present
```

Também pode especificar o caminho para o ficheiro de configuração.

```csharp
TelemetryConfiguration configuration = TelemetryConfiguration.CreateFromConfiguration("ApplicationInsights.config");
```

Para obter mais informações, consulte [referência de ficheiro de configuração](app-insights-configuration-with-applicationinsights-config.md).

Pode obter um exemplo completo do ficheiro de configuração ao instalar a versão mais recente do [Microsoft.ApplicationInsights.WindowsServer](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer) pacote. Eis o **mínima** configuração para a coleção de dependência que é equivalente ao exemplo de código.

```XML
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <InstrumentationKey>Your Key</InstrumentationKey>
  <TelemetryInitializers>
    <Add Type="Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer, Microsoft.AI.DependencyCollector"/>
  </TelemetryInitializers>
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule, Microsoft.AI.DependencyCollector">
      <ExcludeComponentCorrelationHttpHeadersOnDomains>
        <Add>core.windows.net</Add>
        <Add>core.chinacloudapi.cn</Add>
        <Add>core.cloudapi.de</Add>
        <Add>core.usgovcloudapi.net</Add>
        <Add>localhost</Add>
        <Add>127.0.0.1</Add>
      </ExcludeComponentCorrelationHttpHeadersOnDomains>
      <IncludeDiagnosticSourceActivities>
        <Add>Microsoft.Azure.ServiceBus</Add>
        <Add>Microsoft.Azure.EventHubs</Add>
      </IncludeDiagnosticSourceActivities>
    </Add>
  </TelemetryModules>
  <TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel, Microsoft.AI.ServerTelemetryChannel"/>
</ApplicationInsights>

```

### <a name="configuring-telemetry-collection-from-code"></a>Configurar a recolha de telemetria a partir do código

* Durante o arranque de aplicação criar e configurar `DependencyTrackingTelemetryModule` instância - tem de ser singleton e tem de ser preservada para duração da aplicação.

```csharp
var module = new DependencyTrackingTelemetryModule();

// prevent Correlation Id to be sent to certain endpoints. You may add other domains as needed.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
//...

// enable known dependency tracking, note that in future versions, we will extend this list. 
// please check default settings in https://github.com/Microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/DependencyCollector/NuGet/ApplicationInsights.config.install.xdt#L20
module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.ServiceBus");
module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.EventHubs");
//....

// initialize the module
module.Initialize(configuration);
```

* Adicionar comuns inicializadores de telemetria

```csharp
// stamps telemetry with correlation identifiers
TelemetryConfiguration.Active.TelemetryInitializers.Add(new OperationCorrelationTelemetryInitializer());

// ensures proper DependencyTelemetry.Type is set for Azure RESTful API calls
TelemetryConfiguration.Active.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());
```

* Para a aplicação do Windows do .NET Framework, também pode instalar e inicializar o módulo de recoletor de contador de desempenho, tal como descrito [aqui](http://apmtips.com/blog/2017/02/13/enable-application-insights-live-metrics-from-code/)

#### <a name="full-example"></a>Exemplo completo

```csharp
static void Main(string[] args)
{
    TelemetryConfiguration configuration = TelemetryConfiguration.Active;

    configuration.InstrumentationKey = "removed";
    configuration.TelemetryInitializers.Add(new OperationCorrelationTelemetryInitializer());
    configuration.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());

    var telemetryClient = new TelemetryClient();
    using (InitializeDependencyTracking(configuration))
    {
        // run app...
        
        telemetryClient.TrackTrace("Hello World!");

        using (var httpClient = new HttpClient())
        {
            // Http dependency is automatically tracked!
            httpClient.GetAsync("https://microsoft.com").Wait();
        }

    }

    // before exit, flush the remaining data
    telemetryClient.Flush();
    
    // flush is not blocking so wait a bit
    Task.Delay(5000).Wait();

}

static DependencyTrackingTelemetryModule InitializeDependencyTracking(TelemetryConfiguration configuration)
{
    var module = new DependencyTrackingTelemetryModule();
    
    // prevent Correlation Id to be sent to certain endpoints. You may add other domains as needed.
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.chinacloudapi.cn");
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.cloudapi.de");
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.usgovcloudapi.net");
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("localhost");
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("127.0.0.1");

    // enable known dependency tracking, note that in future versions, we will extend this list. 
    // please check default settings in https://github.com/Microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/DependencyCollector/NuGet/ApplicationInsights.config.install.xdt#L20
    module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.ServiceBus");
    module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.EventHubs");

    // initialize the module
    module.Initialize(configuration);

    return module;
}
```

## <a name="next-steps"></a>Passos Seguintes
* [Monitorizar dependências](app-insights-asp-net-dependencies.md) para ver se REST, SQL Server ou outros recursos externos são abrandamento.
* [Utilize a API](app-insights-api-custom-events-metrics.md) para enviar os seus próprios eventos e as métricas para uma vista mais detalhada do desempenho e a utilização da sua aplicação.
