---
title: Enlaces de Event Hubs do Azure para as funções do Azure
description: Compreenda como utilizar os enlaces de Event Hubs do Azure das funções do Azure.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: das funções do Azure, funções, processamento de eventos, computação dinâmica, arquitetura sem servidor
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/08/2017
ms.author: tdykstra
ms.openlocfilehash: 64914a1b3efe81a152f5463f74c70c22f01ec0c1
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34724049"
---
# <a name="azure-event-hubs-bindings-for-azure-functions"></a>Enlaces de Event Hubs do Azure para as funções do Azure

Este artigo explica como trabalhar com [Event Hubs do Azure](../event-hubs/event-hubs-what-is-event-hubs.md) enlaces para as funções do Azure. Funções do Azure suporta acionam e de saída vínculos para os Event Hubs.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pacotes - funciona 1. x

Para a versão das funções do Azure 1. x, os enlaces de Event Hubs são fornecidos no [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) pacote NuGet, versão 2. x.
Código de origem para o pacote está a ser o [sdk de webjobs do azure](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs) repositório do GitHub.


[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Pacotes - funciona 2. x

Para encontrar funções de 2. x, utilize o [Microsoft.Azure.WebJobs.Extensions.EventHubs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs) pacote, versão 3.
Código de origem para o pacote está a ser o [sdk de webjobs do azure](https://github.com/Azure/azure-webjobs-sdk/tree/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs) repositório do GitHub.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Acionador

Utilize o acionador de Event Hubs para responder a um evento enviado para um fluxo de eventos de hub de eventos. Tem de ter acesso de leitura para o hub de eventos para configurar o acionador.

Quando uma função de Acionador de Event Hubs é acionada, a mensagem que aciona é transmitida para a função como uma cadeia.

## <a name="trigger---scaling"></a>Acionar - dimensionamento

Cada instância de uma função de Event Hub-Triggered é copiada por apenas 1 instância do EventProcessorHost (EPH). Os Event Hubs garantem que apenas 1 EPH pode obter uma concessão numa determinada partição.

Por exemplo, suponha que começam com a seguinte configuração e pressupostos para um Hub de eventos:

1. 10 partições.
1. 1000 eventos distribuído uniformemente em todas as partições = mensagens de > 100 em cada partição.

Quando a sua função primeiro está ativada, não há apenas 1 instância da função. Vamos chamar esta instância de função Function_0. Function_0 terão 1 EPH que gere para obter uma concessão em todas as partições de 10. Será iniciada ler eventos de partições 0-9. A partir deste ponto, irá ocorrer um dos seguintes:

* **É necessária a instância de função apenas 1** -Function_0 é capaz de processar todos os 1000 antes de lógica de dimensionamento das funções do Azure se inicia. Por conseguinte, todas as mensagens de 1000 são processadas pelo Function_0.

* **Adicionar 1 instância de função mais** -lógica das funções do Azure de dimensionamento determina que Function_0 tem mais de mensagens que pode processá-lo, pelo que é criada uma nova instância, Function_1,. Os Event Hubs Deteta que uma nova instância EPH está a tentar ler as mensagens. Os Event Hubs iniciará o balanceamento de carga as partições entre as instâncias EPH, por exemplo, partições 0-4 estão atribuídas ao Function_0 e partições 5 9 estão atribuídas a Function_1. 

* **Adicionar N mais instâncias de função** -lógica das funções do Azure de dimensionamento determina que Function_0 e Function_1 têm mais mensagens que o podem processar. Será dimensionado novamente para Function_2... N, em que N é maior do que as partições do Hub de eventos. Os Event Hubs carregará balancear as partições Function_0 … 9 instâncias.

O facto de que N é maior do que o número de partições é dimensionamento lógica exclusivo atual das funções do Azure. Isto é feito para se certificar de que existem sempre instâncias de EPH prontamente disponível para obter rapidamente o bloqueio de partition(s) à medida que ficam disponíveis a partir de outras instâncias. Os utilizadores só são-lhe cobrados os recursos utilizados quando executa a instância de função e desta sobre-aprovisionar não serem cobrados.

Se todas as execuções de função tenha êxito sem erros, os pontos de verificação são adicionados à conta de armazenamento associados. Quando a apontar verificação for bem sucedida, todas as mensagens de 1000 nunca devem ser obtidas novamente.

## <a name="trigger---example"></a>Acionador - exemplo

Veja o exemplo de específicas do idioma:

* [C#](#trigger---c-example)
* [Script do c# (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Acionador - c# exemplo

O seguinte exemplo mostra um [c# função](functions-dotnet-class-library.md) que regista o corpo da mensagem do acionador de hub de eventos.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Para obter acesso a [metadados de evento](#trigger---event-metadata) no código de função, vincular a um [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) objeto (requer um utilizando instrução para `Microsoft.ServiceBus.Messaging`). Também pode aceder às propriedades do mesmas através de expressões de enlace da assinatura do método.  O exemplo seguinte mostra ambas as formas de obter os dados do mesmos:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run(
    [EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] EventData myEventHubMessage, 
    DateTime enqueuedTimeUtc, 
    Int64 sequenceNumber,
    string offset,
    TraceWriter log)
{
    log.Info($"Event: {Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
    // Metadata accessed by binding to EventData
    log.Info($"EnqueuedTimeUtc={myEventHubMessage.EnqueuedTimeUtc}");
    log.Info($"SequenceNumber={myEventHubMessage.SequenceNumber}");
    log.Info($"Offset={myEventHubMessage.Offset}");
    // Metadata accessed by using binding expressions
    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"SequenceNumber={sequenceNumber}");
    log.Info($"Offset={offset}");
}
```

Para receber eventos num batch, certifique- `string` ou `EventData` uma matriz:

```cs
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### <a name="trigger---c-script-example"></a>Acionador - exemplo de script do c#

O exemplo seguinte mostra um acionador de hub de eventos enlace num *function.json* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace. A função regista o corpo da mensagem do acionador de hub de eventos.

Os exemplos seguintes mostram os dados de enlace de Event Hubs no *function.json* ficheiro. O primeiro exemplo destina-se as funções 1. x, não sendo segunda de funções 2. x. 

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Eis o código de script do c#:

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Para obter acesso a [metadados de evento](#trigger---event-metadata) no código de função, vincular a um [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) objeto (requer um utilizando instrução para `Microsoft.ServiceBus.Messaging`). Também pode aceder às propriedades do mesmas através de expressões de enlace da assinatura do método.  O exemplo seguinte mostra ambas as formas de obter os dados do mesmos:

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using System;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage,
    DateTime enqueuedTimeUtc, 
    Int64 sequenceNumber,
    string offset,
    TraceWriter log)
{
    log.Info($"Event: {Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
    // Metadata accessed by binding to EventData
    log.Info($"EnqueuedTimeUtc={myEventHubMessage.EnqueuedTimeUtc}");
    log.Info($"SequenceNumber={myEventHubMessage.SequenceNumber}");
    log.Info($"Offset={myEventHubMessage.Offset}");
    // Metadata accessed by using binding expressions
    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"SequenceNumber={sequenceNumber}");
    log.Info($"Offset={offset}");
}
```

Para receber eventos num batch, certifique- `string` ou `EventData` uma matriz:

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### <a name="trigger---f-example"></a>Acionador - F # exemplo

O exemplo seguinte mostra um acionador de hub de eventos enlace num *function.json* ficheiro e uma [F # função](functions-reference-fsharp.md) que utiliza o enlace. A função regista o corpo da mensagem do acionador de hub de eventos.

Os exemplos seguintes mostram os dados de enlace de Event Hubs no *function.json* ficheiro. O primeiro exemplo destina-se as funções 1. x, não sendo segunda de funções 2. x. 

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Eis o código F #:

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

### <a name="trigger---javascript-example"></a>Acionador - exemplo de JavaScript

O exemplo seguinte mostra um acionador de hub de eventos enlace num *function.json* ficheiro e uma [JavaScript função](functions-reference-node.md) que utiliza o enlace. A função lê [eventos metadados](#trigger---event-metadata) e regista a mensagem.

Os exemplos seguintes mostram os dados de enlace de Event Hubs no *function.json* ficheiro. O primeiro exemplo destina-se as funções 1. x, não sendo segunda de funções 2. x. 

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Eis o código JavaScript:

```javascript
module.exports = function (context, eventHubMessage) {
    context.log('Event Hubs trigger function processed message: ', myEventHubMessage);
    context.log('EnqueuedTimeUtc =', context.bindingData.enqueuedTimeUtc);
    context.log('SequenceNumber =', context.bindingData.sequenceNumber);
    context.log('Offset =', context.bindingData.offset);
     
    context.done();
};
```

Para receber eventos num batch, defina `cardinality` para `many` no *function.json* ficheiro, conforme mostrado nos exemplos seguintes. O primeiro exemplo destina-se as funções 1. x, não sendo segunda de funções 2. x. 

```json
{
  "type": "eventHubTrigger",
  "name": "eventHubMessages",
  "direction": "in",
  "path": "MyEventHub",
  "cardinality": "many",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "eventHubMessages",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "cardinality": "many",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Eis o código JavaScript:

```javascript
module.exports = function (context, eventHubMessages) {
    context.log(`JavaScript eventhub trigger function called for message array ${eventHubMessages}`);
    
    eventHubMessages.forEach(message => {
        context.log(`Processed message ${message}`);
    });

    context.done();
};
```

## <a name="trigger---attributes"></a>Acionador - atributos

No [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [EventHubTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs) atributo.

O construtor do atributo tem o nome do hub de eventos, o nome do grupo de consumidores e o nome de uma definição de aplicação que contenha a cadeia de ligação. Para obter mais informações sobre estas definições, consulte o [acionar a secção de configuração](#trigger---configuration). Eis um `EventHubTriggerAttribute` exemplo do atributo:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, TraceWriter log)
{
    ...
}
```

Para obter um exemplo completado, consulte [acionador - c# exemplo](#trigger---c-example).

## <a name="trigger---configuration"></a>Acionador - configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiros e o `EventHubTrigger` atributo.

|propriedade de Function.JSON | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**tipo** | n/d | tem de ser definido como `eventHubTrigger`. Esta propriedade é definida automaticamente quando criar o acionador no portal do Azure.|
|**direção** | n/d | tem de ser definido como `in`. Esta propriedade é definida automaticamente quando criar o acionador no portal do Azure. |
|**name** | n/d | O nome da variável que representa o item de eventos no código da função. | 
|**path** |**EventHubName** | Funciona apenas 1. x. O nome do hub de eventos.  | 
|**EventHubName** |**EventHubName** | Funciona apenas 2. x. O nome do hub de eventos.  |
|**consumerGroup** |**ConsumerGroup** | Uma propriedade opcional que define o [grupo de consumidores](../event-hubs/event-hubs-features.md#event-consumers) utilizado para subscrever o hub de eventos. Se for omitido, o `$Default` é utilizado o grupo de consumidores. | 
|**Cardinalidade** | n/d | Para Javascript. Definido como `many` para activar a criação de batches.  Se for omitido ou definido como `one`, única mensagem transmitido à função. | 
|**Ligação** |**Ligação** | O nome de uma definição de aplicação que contenha a cadeia de ligação ao espaço de nomes o hub de eventos. Copie esta cadeia de ligação ao clicar no **informações de ligação** botão para o [espaço de nomes](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace), não o hub de eventos em si. Esta cadeia de ligação tem de ter, pelo menos, permissões de leitura para ativar o acionador.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---event-metadata"></a>Acionador - metadados de eventos

O acionador de Event Hubs fornece vários [propriedades de metadados](functions-triggers-bindings.md#binding-expressions---trigger-metadata). Estas propriedades podem ser utilizadas como parte das expressões de enlace noutros enlaces ou como parâmetros no seu código. Estas são as propriedades do [EventData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicebus.messaging.eventdata) classe.

|Propriedade|Tipo|Descrição|
|--------|----|-----------|
|`PartitionContext`|[PartitionContext](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.partitioncontext)|O `PartitionContext` instância.|
|`EnqueuedTimeUtc`|`DateTime`|O tempo, colocados em fila em UTC.|
|`Offset`|`string`|O desvio de dados relativo para o fluxo de partição do Hub de eventos. O desvio é um marcador ou o identificador de um evento dentro do fluxo de Event Hubs. O identificador é exclusivo dentro de uma partição do fluxo de Event Hubs.|
|`PartitionKey`|`string`|A partição para a qual o evento devem ser enviados dados.|
|`Properties`|`IDictionary<String,Object>`|As propriedades de utilizador dos dados do evento.|
|`SequenceNumber`|`Int64`|O número de sequência lógica do evento.|
|`SystemProperties`|`IDictionary<String,Object>`|As propriedades do sistema, incluindo os dados do evento.|

Consulte [exemplos de código](#trigger---example) que utilize estas propriedades anteriormente neste artigo.

## <a name="trigger---hostjson-properties"></a>Acionador - host.json propriedades

O [host.json](functions-host-json.md#eventhub) ficheiro contém definições que controlam o comportamento de Acionador de Event Hubs.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="output"></a>Saída

Utilize a saída de Event Hubs enlace escrever eventos no registo para uma transmissão de eventos. Tem de ter permissão de envio para um hub de eventos para escrever eventos no mesmo.

## <a name="output---example"></a>De saída - exemplo

Veja o exemplo de específicas do idioma:

* [C#](#output---c-example)
* [Script do c# (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Saída - c# exemplo

O seguinte exemplo mostra um [c# função](functions-dotnet-class-library.md) que escreve uma mensagem para um hub de eventos, o valor de retorno do método a utilizar como o resultado:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

### <a name="output---c-script-example"></a>Saída - exemplo de script do c#

O exemplo seguinte mostra um acionador de hub de eventos enlace num *function.json* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace. A função escreve uma mensagem para um hub de eventos.

Os exemplos seguintes mostram os dados de enlace de Event Hubs no *function.json* ficheiro. O primeiro exemplo destina-se as funções 1. x, não sendo segunda de funções 2. x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Eis c# código de script que cria uma mensagem:

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

C# código de script Eis que cria várias mensagens:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Saída - F # exemplo

O exemplo seguinte mostra um acionador de hub de eventos enlace num *function.json* ficheiro e uma [F # função](functions-reference-fsharp.md) que utiliza o enlace. A função escreve uma mensagem para um hub de eventos.

Os exemplos seguintes mostram os dados de enlace de Event Hubs no *function.json* ficheiro. O primeiro exemplo destina-se as funções 1. x, não sendo segunda de funções 2. x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Eis o código F #:

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

### <a name="output---javascript-example"></a>Saída - exemplo de JavaScript

O exemplo seguinte mostra um acionador de hub de eventos enlace num *function.json* ficheiro e uma [JavaScript função](functions-reference-node.md) que utiliza o enlace. A função escreve uma mensagem para um hub de eventos.

Os exemplos seguintes mostram os dados de enlace de Event Hubs no *function.json* ficheiro. O primeiro exemplo destina-se as funções 1. x, não sendo segunda de funções 2. x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

No código JavaScript Eis que envia uma mensagem único:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

No código JavaScript Eis que envia mensagens vários:

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();
    var message = 'Event Hub message created at: ' + timeStamp;

    context.bindings.outputEventHubMessage = [];

    context.bindings.outputEventHubMessage.push("1 " + message);
    context.bindings.outputEventHubMessage.push("2 " + message);
    context.done();
};
```

## <a name="output---attributes"></a>Saída - atributos

Para [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [EventHubAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs) atributo.

O construtor do atributo tem o nome do hub de eventos e o nome de uma definição de aplicação que contenha a cadeia de ligação. Para obter mais informações sobre estas definições, consulte [de saída - configuração](#output---configuration). Eis um `EventHub` exemplo do atributo:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    ...
}
```

Para obter um exemplo completado, consulte [resultado - c# exemplo](#output---c-example).

## <a name="output---configuration"></a>De saída - configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiros e o `EventHub` atributo.

|propriedade de Function.JSON | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**tipo** | n/d | Tem de ser definida para "eventHub". |
|**direção** | n/d | Tem de ser definida para "out". Este parâmetro é definido automaticamente quando criar o enlace no portal do Azure. |
|**name** | n/d | O nome da variável utilizado no código de função que representa o evento. | 
|**path** |**EventHubName** | Funciona apenas 1. x. O nome do hub de eventos.  | 
|**EventHubName** |**EventHubName** | Funciona apenas 2. x. O nome do hub de eventos.  |
|**Ligação** |**Ligação** | O nome de uma definição de aplicação que contenha a cadeia de ligação ao espaço de nomes o hub de eventos. Copie esta cadeia de ligação ao clicar no **informações de ligação** botão para o *espaço de nomes*, não o hub de eventos em si. Esta cadeia de ligação tem de ter permissões de envio para enviar a mensagem para o fluxo de eventos.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Saída - utilização

Em c# e c# script, enviar mensagens com um parâmetro de método como `out string paramName`. No script do c#, `paramName` é o valor especificado no `name` propriedade *function.json*. Para escrever várias mensagens, pode utilizar `ICollector<string>` ou `IAsyncCollector<string>` em vez de `out string`.

Em JavaScript, o evento de saída de acesso utilizando `context.bindings.<name>`. `<name>` o valor especificado no `name` propriedade *function.json*.

## <a name="exceptions-and-return-codes"></a>Exceções e códigos de retorno

| Vínculo | Referência |
|---|---|
| Hub de Eventos | [Guia de operações](https://docs.microsoft.com/rest/api/eventhub/publisher-policy-operations) |

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre as funções do Azure acionadores e enlaces](functions-triggers-bindings.md)
