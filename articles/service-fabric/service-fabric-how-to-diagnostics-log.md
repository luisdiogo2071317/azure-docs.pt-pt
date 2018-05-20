---
title: Gerar eventos de registo a partir de uma aplicação .NET Service Fabric no Azure ou um cluster autónomo
description: Saiba mais sobre como adicionar o registo para a aplicação de Service Fabric do .NET alojada num cluster do Azure ou um cluster autónomo.
services: service-fabric
documentationcenter: .net
author: thraka
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/27/2018
ms.author: adegeo
ms.openlocfilehash: ed9aaf67b4f6749ea6d505a51fbc76e3d1cf0870
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="add-logging-to-your-service-fabric-application"></a>Adicionar o registo para a aplicação de Service Fabric

A aplicação tem de fornecer informações suficientes para depurar forensically quando ocorrerem problemas. O registo é uma das ações que mais importante que pode adicionar à sua aplicação de Service Fabric. Quando ocorre uma falha, registo boa pode dar-lhe uma forma para investigar falhas. Ao analisar padrões de registo, pode encontrar formas de melhorar o desempenho ou a estrutura da sua aplicação. Este documento demonstra Algumas opções de registo diferentes.

## <a name="eventflow"></a>EventFlow

O [EventFlow biblioteca](https://github.com/Azure/diagnostics-eventflow) suite permite que as aplicações definir os dados de diagnóstico para recolher e, em que deve ser debitados para. Dados de diagnóstico podem ser qualquer coisa dos contadores de desempenho para rastreios de aplicações. É executado no mesmo processo da aplicação, de modo a sobrecarga de comunicação é minimizada. Para obter mais informações sobre EventFlow e recursos de infraestrutura de serviço, consulte [agregação de eventos do Azure Service Fabric com EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md).

### <a name="using-structured-eventsource-events"></a>A utilização de eventos de EventSource estruturados

Mensagem definição eventos ao utilizam as maiúsculas e minúsculas permite-lhe dados de pacote sobre o evento, no contexto do evento. Mais facilmente pode pesquisar e filtrar com base nos nomes ou valores das propriedades do evento especificado. Structuring torna de saída instrumentação mais fáceis de ler, mas requer mais profundamente e tempo para definir um evento para cada caso de utilização. 

Algumas definições de evento podem ser partilhadas entre a aplicação completa. Por exemplo, um início de método ou parar eventos deverá ser reutilizados em vários serviços dentro de uma aplicação. Um serviço de domínio específico, como um sistema de ordem, pode ter um **CreateOrder** evento, o que tem o seu próprio evento exclusivo. Esta abordagem poderá gerar muitos eventos e potencialmente requerem a coordenação de identificadores entre equipas de projeto. 

```csharp
[EventSource(Name = "MyCompany-VotingState-VotingStateService")]
internal sealed class ServiceEventSource : EventSource
{
    public static readonly ServiceEventSource Current = new ServiceEventSource();

    // The instance constructor is private to enforce singleton semantics.
    private ServiceEventSource() : base() { }

    ...

    // The ServiceTypeRegistered event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
    private const int ServiceTypeRegisteredEventId = 3;
    [Event(ServiceTypeRegisteredEventId, Level = EventLevel.Informational, Message = "Service host process {0} registered service type {1}", Keywords = Keywords.ServiceInitialization)]
    public void ServiceTypeRegistered(int hostProcessId, string serviceType)
    {
        WriteEvent(ServiceTypeRegisteredEventId, hostProcessId, serviceType);
    }

    // The ServiceHostInitializationFailed event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
    private const int ServiceHostInitializationFailedEventId = 4;
    [Event(ServiceHostInitializationFailedEventId, Level = EventLevel.Error, Message = "Service host initialization failed", Keywords = Keywords.ServiceInitialization)]
    public void ServiceHostInitializationFailed(string exception)
    {
        WriteEvent(ServiceHostInitializationFailedEventId, exception);
    }

    ...

```

### <a name="using-eventsource-generically"></a>Utilizar EventSource genericamente

Como definir eventos específicos pode ser difícil, muitas pessoas definem alguns eventos com um conjunto comum de parâmetros que geralmente as respetivas informações como uma cadeia de saída. Grande parte do aspeto estruturado perde-se, e é mais difícil procurar e filtrar os resultados. Esta abordagem, alguns eventos que normalmente correspondem aos níveis de registo são definidos. O fragmento seguinte define uma mensagem de erro e de depuração:

```csharp
[EventSource(Name = "MyCompany-VotingState-VotingStateService")]
internal sealed class ServiceEventSource : EventSource
{
    public static readonly ServiceEventSource Current = new ServiceEventSource();

    // The Instance constructor is private, to enforce singleton semantics.
    private ServiceEventSource() : base() { }

    ...

    private const int DebugEventId = 10;
    [Event(DebugEventId, Level = EventLevel.Verbose, Message = "{0}")]
    public void Debug(string msg)
    {
        WriteEvent(DebugEventId, msg);
    }

    private const int ErrorEventId = 11;
    [Event(ErrorEventId, Level = EventLevel.Error, Message = "Error: {0} - {1}")]
    public void Error(string error, string msg)
    {
        WriteEvent(ErrorEventId, error, msg);
    }

    ...

```

Utilizar uma versão híbrida do estruturados e genérica instrumentação também pode funcionar bem. Instrumentação Structured é utilizada para relatórios de erros e de métricas. Eventos genéricos podem ser utilizados para o registo de detalhado que é consumido pelo engenheiros para resolução de problemas.

## <a name="microsoftextensionslogging"></a>Microsoft.Extensions.Logging

O registo do ASP.NET Core ([pacote Microsoft.Extensions.Logging NuGet](https://www.nuget.org/packages/Microsoft.Extensions.Logging)) é uma arquitetura de registo que fornece uma API de registo padrão para a sua aplicação. Suporte para outros back-ends de registo pode ser ligado registo do ASP.NET Core. Isto dá-lhe uma grande variedade de suporte para registo na sua aplicação é processada, sem ter de alterar muito código.

1. Adicionar o **Microsoft.Extensions.Logging** pacote NuGet para o projeto pretende instrumento. Além disso, adicione quaisquer pacotes de fornecedor. Para obter mais informações, consulte [iniciar sessão ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).
2. Adicionar um **utilizando** directiva para **Microsoft.Extensions.Logging** ao seu ficheiro de serviço.
3. Defina uma variável privada dentro da sua classe de serviço.

   ```csharp
   private ILogger _logger = null;
   ```

4. O construtor de classe de serviço, adicione este código:

   ```csharp
   _logger = new LoggerFactory().CreateLogger<Stateless>();
   ```

5. Inicie a instrumentação o código no seu métodos. Seguem-se alguns exemplos:

   ```csharp
   _logger.LogDebug("Debug-level event from Microsoft.Logging");
   _logger.LogInformation("Informational-level event from Microsoft.Logging");

   // In this variant, we're adding structured properties RequestName and Duration, which have values MyRequest and the duration of the request.
   // Later in the article, we discuss why this step is useful.
   _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);
   ```

### <a name="using-other-logging-providers"></a>Utilizar outros fornecedores de registo

Alguns fornecedores de terceiros utilizar a abordagem descrita na secção anterior, incluindo [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), e [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging). Pode ligue cada um no registo do ASP.NET Core ou, pode utilizá-las separadamente. Serilog tem uma funcionalidade que otimiza todas as mensagens enviadas a partir de um registo. Esta funcionalidade pode ser útil para o nome do serviço, tipo e informações da partição de saída. Para utilizar esta capacidade na infraestrutura do ASP.NET Core, execute estes passos:

1. Adicionar o **Serilog**, **Serilog.Extensions.Logging**, **Serilog.Sinks.Literate**, e **Serilog.Sinks.Observable** pacotes NuGet para o projeto. 
2. Criar um `LoggerConfiguration` e a instância de registo.

   ```csharp
   Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
   ```

3. Adicionar um `Serilog.ILogger` argumento de construtor de serviço e a passagem de registo criado recentemente.

   ```csharp
   ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
   ```

4. O construtor de serviço, cria enrichers de propriedade para **ServiceTypeName**, **ServiceName**, **PartitionId**, e **InstanceId** .

   ```csharp
   public Stateless(StatelessServiceContext context, Serilog.ILogger serilog)
       : base(context)
   {
       PropertyEnricher[] properties = new PropertyEnricher[]
       {
           new PropertyEnricher("ServiceTypeName", context.ServiceTypeName),
           new PropertyEnricher("ServiceName", context.ServiceName),
           new PropertyEnricher("PartitionId", context.PartitionId),
           new PropertyEnricher("InstanceId", context.ReplicaOrInstanceId),
       };

       serilog.ForContext(properties);

       _logger = new LoggerFactory().AddSerilog(serilog.ForContext(properties)).CreateLogger<Stateless>();
   }
   ```

5. Instrumente o código a mesma como se estava a utilizar o ASP.NET Core sem Serilog.

   >[!NOTE]
   >Recomendamos que lhe *não* utilizar a estática `Log.Logger` com o exemplo anterior. Service Fabric pode alojar várias instâncias do mesmo tipo de serviço dentro de um único processo. Se utilizar a estática `Log.Logger`, ao último escritor de enrichers a propriedade mostrará valores para todas as instâncias que estão em execução. Este é um motivo por que razão a variável de _logger é uma variável de membro privado da classe de serviço. Além disso, é necessário efetuar o `_logger` disponíveis para código comum, que pode ser utilizado em serviços.

## <a name="next-steps"></a>Passos Seguintes

- Leia mais informações sobre [aplicação de monitorização no Service Fabric](service-fabric-diagnostics-event-generation-app.md).
- Leia sobre o registo com [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) e [Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md).










