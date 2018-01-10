---
title: "Rastreio de ponto a ponto do Service Bus do Azure e diagnóstico | Microsoft Docs"
description: "Descrição geral de diagnóstico do cliente de Service Bus e rastreio de ponto a ponto"
services: service-bus-messaging
documentationcenter: 
author: lmolkova
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: lmolkova
ms.openlocfilehash: 847056acd2d97391782dcac1874a2739b7f5825c
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="distributed-tracing-and-correlation-through-service-bus-messaging"></a>Rastreio distribuído e da correlação através de mensagens do Service Bus

Um dos problemas comuns de desenvolvimento de micro-serviços é a capacidade para a operação de rastreio de um cliente através de todos os serviços que estão envolvidos no processamento. É útil para depuração, análise de desempenho, A / B testar e outros cenários típicos de diagnóstico.
Uma parte deste problema está a controlar o trabalho de partes lógicos. Inclui o resultado e latência e chamadas de dependência externa de processamento de mensagens. Parte de outro é a correlação destes eventos de diagnóstico para além dos limites do processo.

Quando um produtor envia uma mensagem através de uma fila, esta situação ocorre normalmente no âmbito da outra operação lógica, iniciada por outro cliente ou serviço. A mesma operação é continuada pelo consumidor depois de receber uma mensagem. O produtor e consumidor (e outros serviços de processam a operação), emitir, supostamente, eventos de telemetria para o fluxo de operação e resultados de rastreio. Para correlacionar desses eventos e a operação de rastreio ponto-a-ponto, cada serviço que comunica telemetria tem cada evento do carimbo com um contexto de rastreio.

Mensagens do Microsoft Azure Service Bus definiu as propriedades de payload produtores e consumidores devem utilizar para transmitir esse contexto de rastreio.
O protocolo baseia-se no [protocolo HTTP correlação](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md).

| Nome da propriedade        | Descrição                                                 |
|----------------------|-------------------------------------------------------------|
|  Id de diagnóstico       | Identificador exclusivo de uma chamada externo produtor para a fila. Consulte [Request-Id no protocolo HTTP](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md#request-id) para a lógica por detrás, as considerações e formato |
|  Contexto de correlação | Contexto de operação, é propagado em todos os serviços envolvidos no processamento da operação. Para obter mais informações, consulte [contexto de correlação no protocolo HTTP](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md#correlation-context) |

## <a name="service-bus-net-client-auto-tracing"></a>Rastreio automática de cliente .NET do Service Bus

Começando com a versão 3.0.0 [cliente de barramento de serviço do Microsoft Azure para .NET](/dotnet/api/microsoft.azure.servicebus.queueclient) fornece pontos de instrumentação de rastreio que podem estabelecer ligação com ele por sistemas de rastreio ou o fragmento de código de cliente.
A instrumentação permite controlar todas as chamadas para o barramento de serviço de mensagens do serviço do lado do cliente. Se o processamento da mensagem é feito com o [padrão de processador de mensagens](/dotnet/api/microsoft.azure.servicebus.queueclient.registermessagehandler), também está instrumentado o processamento de mensagens

### <a name="tracking-with-azure-application-insights"></a>Controlo com o Azure Application Insights

[Microsoft Application Insights](https://azure.microsoft.com/services/application-insights/) proporciona um desempenho avançado, incluindo automagical pedido e controlo de dependência de capacidades de monitorização.

Consoante o tipo de projeto, instale o Application Insights SDK:
- [ASP.NET](../application-insights/app-insights-asp-net.md) versão 2.5-beta2 ou superior
- [ASP.NET Core](../application-insights/app-insights-asp-net-core.md) versão 2.2.0-beta2 ou superior.
Estas ligações fornecem detalhes sobre a instalar o SDK, a criação de recursos e a configuração SDK (se necessário). Para aplicações não ASP.NET, consulte [Azure Application Insights para aplicações de consola](../application-insights/application-insights-console.md) artigo.

Se utilizar [padrão de processador de mensagens](/dotnet/api/microsoft.azure.servicebus.queueclient.registermessagehandler) para processar mensagens, terminar: todas as chamadas de Service Bus efetuadas pelo seu serviço são controladas e correlacionadas com outros itens de telemetria automaticamente. Caso contrário, consulte o seguinte exemplo para o controlo de processamento de mensagens manual.

#### <a name="trace-message-processing"></a>Processamento de mensagens de rastreio

```csharp
private readonly TelemetryClient telemetryClient;

async Task ProcessAsync(Message message)
{
    var activity = message.ExtractActivity();
    
    // If you are using Microsoft.ApplicationInsights package version 2.6-beta or higher, you should call StartOperation<RequestTelemetry>(activity) instead
    using (var operation = telemetryClient.StartOperation<RequestTelemetry>("Process", activity.RootId, activity.ParentId))
    {
        telemetryClient.TrackTrace("Received message");
        try 
        {
           // process message
        }
        catch (Exception ex)
        {
             telemetryClient.TrackException(ex);
             operation.Telemetry.Success = false;
             throw;
        }

        telemetryClient.TrackTrace("Done");
   }
}
```

Neste exemplo, `RequestTelemetry` é comunicada para cada mensagem processada, ter uma timestamp, a duração e o resultado (êxito). A telemetria tem também um conjunto de propriedades de correlação.
Aninhada rastreios e exceções reportadas durante o processamento de mensagens são também carimbo de data / com propriedades de correlação, que representa-los como subordinados do `RequestTelemetry`.

No caso de efetuar chamadas para componentes externos suportados durante o processamento da mensagem, também são automagically controlada e correlacionado. Consulte [controlar operações personalizadas com o SDK .NET do Application Insights](../application-insights/application-insights-custom-operations-tracking.md) para correlação e o controlo manual.

### <a name="tracking-without-tracing-system"></a>Controlo sem sistema de rastreio
No caso do sistema de rastreio não suporta chamadas de Service Bus automagical controlo pode ser está à procura para adicionar esse suporte para um sistema de rastreio ou na sua aplicação. Esta secção descreve os eventos de diagnóstico enviados pelo cliente de .NET de barramento de serviço.  

Cliente de .NET do Service Bus está instrumentado utilizando primitivos de rastreio de .NET [System.Diagnostics.Activity](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) e [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md).

`Activity`funciona como um contexto de rastreio ao `DiagnosticSource` é um mecanismo de notificação. 

Se não houver nenhuma escuta de existência de eventos de DiagnosticSource, instrumentação está desativada, mantendo os custos zero instrumentação. DiagnosticSource fornece controlo de todos os para o serviço de escuta:
- serviço de escuta controla que origens e eventos para escutar
- taxa de eventos de controlos do serviço de escuta e a amostragem
- os eventos são enviados com um payload que fornece o contexto de completo para que possa aceder e modificar o objecto de mensagem durante o evento

Familiarize-se com [Guia do utilizador DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) antes de prosseguir com a implementação.

Vamos criar um serviço de escuta de eventos do Service Bus na aplicação de ASP.NET Core que escreve registos com Microsoft.Extension.Logger.
Utiliza [System.Reactive.Core](https://www.nuget.org/packages/System.Reactive.Core) biblioteca para subscrever DiagnosticSource (também é fácil subscrever DiagnosticSource sem ele)

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory factory, IApplicationLifetime applicationLifetime)
{
    // configuration...

    var serviceBusLogger = factory.CreateLogger("Microsoft.Azure.ServiceBus");

    IDisposable innerSubscription = null;
    IDisposable outerSubscription = DiagnosticListener.AllListeners.Subscribe(delegate (DiagnosticListener listener)
    {
        // subscribe to the Service Bus DiagnosticSource
        if (listener.Name == "Microsoft.Azure.ServiceBus")
        {
            // receive event from Service Bus DiagnosticSource
            innerSubscription = listener.Subscribe(delegate (KeyValuePair<string, object> evnt)
            {
                // Log operation details once it's done
                if (evnt.Key.EndsWith("Stop"))
                {
                    Activity currentActivity = Activity.Current;
                    TaskStatus status = (TaskStatus)evnt.Value.GetProperty("Status");
                    serviceBusLogger.LogInformation($"Operation {currentActivity.OperationName} is finished, Duration={currentActivity.Duration}, Status={status}, Id={currentActivity.Id}, StartTime={currentActivity.StartTimeUtc}");
                }
            });
        }
    });

    applicationLifetime.ApplicationStopping.Register(() =>
    {
        outerSubscription?.Dispose();
        innerSubscription?.Dispose();
    });
}
```

Neste exemplo, o serviço de escuta regista duração, o resultado, o identificador exclusivo e hora de início de cada operação de Service Bus.

#### <a name="events"></a>Eventos

Para cada operação, dois eventos são enviados: 'Iniciar' e 'Stop'. Provavelmente, apenas está interessado em 'Stop' eventos. Se fornecem o resultado da operação, bem como iniciam a hora e a duração como um propriedades da atividade.

Payload de evento fornece um serviço de escuta com o contexto da operação, replica parâmetros de entrada de API e devolve o valor. Payload de evento 'Stop' tem todas as propriedades do payload do evento de "Iniciar", pelo que pode ignorar o evento 'Start' completamente.

Todos os eventos também tem propriedades de 'Entity' e 'Ponto final', estão omitidos na tabela abaixo
  * `string Entity`– O nome da entidade (fila, tópico, etc.)
  * `Uri Endpoint`-URL de ponto final do Service Bus

Cada evento 'Stop' tem `Status` propriedade com `TaskStatus` operação assíncrona foi concluída com, que também for omitido na seguinte tabela de simplicidade.

Eis a lista completa das operações instrumentadas:

| Nome da Operação | Controlado de API | Propriedades de Payload específico|
|----------------|-------------|---------|
| Microsoft.Azure.ServiceBus.Send | [MessageSender.SendAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.sendasync) | IList<Message> mensagens - lista de mensagens que está a ser enviados |
| Microsoft.Azure.ServiceBus.ScheduleMessage | [MessageSender.ScheduleMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.schedulemessageasync) | Mensagem de mensagem - mensagem a ser processado<br/>DateTimeOffset ScheduleEnqueueTimeUtc - desvio de mensagem agendada<br/>SequenceNumber longo - número de sequência de mensagem agendada ('Stop' payload do evento) |
| Microsoft.Azure.ServiceBus.Cancel | [MessageSender.CancelScheduledMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.cancelscheduledmessageasync) | SequenceNumber longo - número de sequência de mensagem de r terá de ser cancelada | 
| Microsoft.Azure.ServiceBus.Receive | [MessageReceiver.ReceiveAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receiveasync) |Int RequestedMessageCount - o número máximo de mensagens em fila que pode ser recebido.<br/>IList<Message> mensagens - lista de mensagens recebidas ('Stop' payload do evento) |
| Microsoft.Azure.ServiceBus.Peek | [MessageReceiver.PeekAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync) | Int FromSequenceNumber - o ponto inicial a partir do qual pretende procurar um lote de mensagens.<br/>Int RequestedMessageCount - o número de mensagens em fila a obter.<br/>IList<Message> mensagens - lista de mensagens recebidas ('Stop' payload do evento) |
| Microsoft.Azure.ServiceBus.ReceiveDeferred | [MessageReceiver.ReceiveDeferredMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receivedeferredmessageasync) | IEnumerable<long> SequenceNumbers - na lista que contém os números de sequência a receber.<br/>IList<Message> mensagens - lista de mensagens recebidas ('Stop' payload do evento) |
| Microsoft.Azure.ServiceBus.Complete | [MessageReceiver.CompleteAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.completeasync) | IList<string> LockTokens - na lista que contém os tokens de bloqueio das mensagens correspondentes para concluir.|
| Microsoft.Azure.ServiceBus.Abandon | [MessageReceiver.AbandonAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.abandonasync) | cadeia LockToken - o token de bloqueio da mensagem abandone correspondente. |
| Microsoft.Azure.ServiceBus.Defer | [MessageReceiver.DeferAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deferasync) | cadeia LockToken - o token de bloqueio da mensagem correspondente para diferir. | 
| Microsoft.Azure.ServiceBus.DeadLetter | [MessageReceiver.DeadLetterAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deadletterasync) | cadeia LockToken - o token de bloqueio da mensagem correspondente para entregues. | 
| Microsoft.Azure.ServiceBus.RenewLock | [MessageReceiver.RenewLockAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.renewlockasync) | cadeia LockToken - o token de bloqueio da mensagem correspondente renove o bloqueio.<br/>DateTime LockedUntilUtc - novo data de expiração do token de bloqueio e a hora em formato UTC. ('Stop' payload do evento)|
| Microsoft.Azure.ServiceBus.Process | Função de lambda de processador de mensagens fornecida no [IReceiverClient.RegisterMessageHandler](/dotnet/api/microsoft.azure.servicebus.core.ireceiverclient.registermessagehandler) | Mensagem de mensagem - mensagem a ser processado. |
| Microsoft.Azure.ServiceBus.ProcessSession | Função de lambda de processador de sessão de mensagem fornecida no [IQueueClient.RegisterSessionHandler](/dotnet/api/microsoft.azure.servicebus.iqueueclient.registersessionhandler) | Mensagem de mensagem - mensagem a ser processado.<br/>Sessão IMessageSession - sessão a ser processado |
| Microsoft.Azure.ServiceBus.AddRule | [SubscriptionClient.AddRuleAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.addruleasync) | Regra de RuleDescription - a descrição da regra que fornece a regra para adicionar. |
| Microsoft.Azure.ServiceBus.RemoveRule | [SubscriptionClient.RemoveRuleAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.removeruleasync) | cadeia RuleName - nome da regra para remover. |
| Microsoft.Azure.ServiceBus.GetRules | [SubscriptionClient.GetRulesAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.getrulesasync) | IEnumerable<RuleDescription> regras de regras de todos os associadas à subscrição. (Apenas em 'Stop' payload) |
| Microsoft.Azure.ServiceBus.AcceptMessageSession | [ISessionClient.AcceptMessageSessionAsync](/dotnet/api/microsoft.azure.servicebus.isessionclient.acceptmessagesessionasync) | cadeia de SessionId - sessionId presente nas mensagens. |
| Microsoft.Azure.ServiceBus.GetSessionState | [IMessageSession.GetStateAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.getstateasync) | cadeia de SessionId - sessionId presente nas mensagens.<br/>byte [] Estado - Estado da sessão ('Stop' payload do evento) |
| Microsoft.Azure.ServiceBus.SetSessionState | [IMessageSession.SetStateAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.setstateasync) | cadeia de SessionId - sessionId presente nas mensagens.<br/>byte [] Estado - Estado da sessão |
| Microsoft.Azure.ServiceBus.RenewSessionLock | [IMessageSession.RenewSessionLockAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.renewsessionlockasync) | cadeia de SessionId - sessionId presente nas mensagens. |
| Microsoft.Azure.ServiceBus.Exception | qualquer instrumentados API| Exceção de exceção - instância de exceção |

Cada caso, pode aceder a `Activity.Current` que contém o contexto da operação atual.

#### <a name="logging-additional-properties"></a>Propriedades de registo adicionais

`Activty.Current`Fornece o contexto de detalhado da operação atual e das respetivas classes principais. Para obter mais informações, consulte [documentação de atividade](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) para obter mais detalhes.
Instrumentação de Service Bus fornece informações adicionais no `Activity.Current.Tags` -que detêm `MessageId` e `SessionId` sempre que estiverem disponíveis.

Atividades que controlam 'Receber', 'Observar' e 'ReceiveDeferred' evento também pode ter `RelatedTo` etiquetas. Realiza lista distinta de `Diagnostic-Id`(s) de mensagens em fila que foram recebidas como resultado.
Essa operação poderá resultar em várias mensagens relacionado com a serem recebidos. Além disso, o `Diagnostic-Id` não é conhecido quando inicia a operação, para operações de 'Receive' foi ser correlacionadas com operações de "Processo" utilizar apenas esta etiqueta. É útil ao analisar problemas de desempenho para verificar quanto tempo demorou a receber a mensagem.

É de forma eficiente para iniciar sessão etiquetas para iterá-los, para que adicionar etiquetas ao anterior exemplo aspeto 

```csharp
Activity currentActivity = Activity.Current;
TaskStatus status = (TaskStatus)evnt.Value.GetProperty("Status");

var tagsList = new StringBuilder();
foreach (var tags in currentActivity.Tags)
{
    tagsList.Append($", "{tags.Key}={tags.Value}");
}

serviceBusLogger.LogInformation($"{currentActivity.OperationName} is finished, Duration={currentActivity.Duration}, Status={status}, Id={currentActivity.Id}, StartTime={currentActivity.StartTimeUtc}{tagsList}");
```

#### <a name="filtering-and-sampling"></a>Filtragem e amostragem

Em alguns casos, é necessário para iniciar sessão apenas uma parte dos eventos para reduzir o consumo de armazenamento ou de sobrecarga de desempenho. Foi possível iniciar 'Stop' eventos só (tal como no exemplo anterior) ou a percentagem de exemplo dos eventos. 
`DiagnosticSource`Forneça a forma de alcançar com `IsEnabled` predicado. Para obter mais informações, consulte [filtragem baseada no contexto na DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#context-based-filtering).

`IsEnabled`pode ser chamado várias vezes para uma operação única minimizar o impacto no desempenho.

`IsEnabled`denomina-se na seguinte sequência:

1. `IsEnabled(<OperationName>, string entity, null)`Por exemplo, `IsEnabled("Microsoft.Azure.ServiceBus.Send", "MyQueue1")`. Tenha em atenção que é não 'Start' ou 'Stop' no final. Utilizá-lo para filtrar operações específicas ou filas. Se a chamada de retorno devolve `false`, eventos para a operação não são enviados

  * Para as operações 'Processo' e 'ProcessSession', receberá também `IsEnabled(<OperationName>, string entity, Activity activity)` chamada de retorno. Utilizá-la para filtrar eventos com base no `activity.Id` ou propriedades de etiquetas.
  
2. `IsEnabled(<OperationName>.Start)`Por exemplo, `IsEnabled("Microsoft.Azure.ServiceBus.Send.Start")`. Verifica se deve ser desencadeou o evento de "Iniciar". O resultado afeta apenas o evento de "Iniciar", mas ainda mais instrumentation não dependem dele.

Não existe nenhum `IsEnabled` para o evento 'Stop'.

Se algum resultado da operação é a exceção, `IsEnabled("Microsoft.Azure.ServiceBus.Exception")` é chamado. Só foi possível subscrever eventos de 'exceção do' e impedir que o resto da instrumentação. Neste caso, tem de lidar com essas exceções. Uma vez que outras instrumentação estiver desativada, não deverá contexto de rastreio para o fluxo com as mensagens de consumidor para o produtor.

Pode utilizar `IsEnabled` também implementar estratégias de amostragem. A amostragem com base no `Activity.Id` ou `Activity.RootId` garante a amostragem consistente em todos os lo (desde que esta é propagada pelo sistema de rastreio ou pelo seu próprio código).

Na presença de vários `DiagnosticSource` os serviços de escuta para a mesma origem, é suficiente para um serviço de escuta aceitar o evento, por isso, `IsEnabled` não é garantida a chamar,

## <a name="next-steps"></a>Passos Seguintes

* [Noções básicas sobre o Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Application Insights correlação](../application-insights/application-insights-correlation.md)
* [Dependências de Monitor do Application Insights](../application-insights/app-insights-asp-net-dependencies.md) para ver se REST, SQL Server ou outros recursos externos são abrandamento.
* [Controlar operações personalizadas com o Application Insights SDK do .NET](../application-insights/application-insights-custom-operations-tracking.md)
