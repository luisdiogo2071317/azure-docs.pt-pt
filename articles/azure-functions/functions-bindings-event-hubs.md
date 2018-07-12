---
title: Enlaces de Hubs de eventos do Azure para as funções do Azure
description: Compreenda como utilizar os enlaces de Hubs de eventos do Azure nas funções do Azure.
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
ms.openlocfilehash: 7ea233f3d5b0e0b6ad1470af146f963fce6c4e94
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970677"
---
# <a name="azure-event-hubs-bindings-for-azure-functions"></a>Enlaces de Hubs de eventos do Azure para as funções do Azure

Este artigo explica como trabalhar com [os Hubs de eventos do Azure](../event-hubs/event-hubs-what-is-event-hubs.md) enlaces das funções do Azure. Funções do Azure suporta acionaram e enlaces dos Hubs de eventos de saída.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pacotes - funções 1.x

Para a versão das funções do Azure 1.x, os enlaces de Hubs de eventos são fornecidos na [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) pacote NuGet, versão 2.x.
Código-fonte para o pacote está no [sdk do webjobs do azure](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs) repositório do GitHub.


[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Pacotes - funções 2.x

Para as funções 2.x, utilize o [Microsoft.Azure.WebJobs.Extensions.EventHubs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs) pacote, a versão 3.x.
Código-fonte para o pacote está no [sdk do webjobs do azure](https://github.com/Azure/azure-webjobs-sdk/tree/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs) repositório do GitHub.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Acionador

Utilize o acionador de Hubs de eventos para responder a um evento enviado para um fluxo de eventos do hub de eventos. Tem de ter acesso de leitura para o hub de eventos para configurar o acionador.

Quando é acionada uma função de Acionador de Hubs de eventos, a mensagem que aciona ele é passada para a função como uma cadeia de caracteres.

## <a name="trigger---scaling"></a>Acionar - dimensionamento

Cada instância de uma função de Event Hub-Triggered conta com apenas 1 instância do EventProcessorHost (EPH). Os Hubs de eventos garante que apenas 1 EPH pode obter uma concessão numa determinada partição.

Por exemplo, suponha que começamos com a seguinte configuração e suposições para um Hub de eventos:

1. 10 partições.
1. 1000 eventos distribuídos uniformemente por todas as partições = > 100 mensagens em cada partição.

Quando a sua função é habilitada pela primeira vez, existe apenas 1 instância da função. Vamos chamar esta instância de função Function_0. Function_0 terá 1 EPH que gere para obter uma concessão sobre todas as partições de 10. Irá começar a ler eventos de 0 a 9 de partições. A partir deste ponto, irá ocorrer um dos seguintes:

* **É necessária a instância de função apenas 1** -Function_0 é capaz de processar todos os 1000 antes de lógica de dimensionamento das funções do Azure entra em ação. Portanto, todas as mensagens de 1000 são processadas pelo Function_0.

* **Adicionar 1 instância de função mais** -lógica das funções do Azure de dimensionamento determina que Function_0 tem mais mensagens do que consegue processar, pelo que uma nova instância, Function_1, é criada. Os Hubs de eventos Deteta que uma nova instância EPH está a tentar ler as mensagens. Os Hubs de eventos serão iniciado o balanceamento de carga as partições entre as instâncias EPH, por exemplo, partições de 0 a 4 são atribuídas a Function_0 e partições 5-9 são atribuídas a Function_1. 

* **Adicionar N mais instâncias de função** -lógica das funções do Azure de dimensionamento determina que Function_0 e Function_1 têm mais mensagens do que o que podem ser processadas. Irá dimensionar novamente para Function_2... N, onde N é maior do que as partições do Hub de eventos. Os Hubs de eventos irão carregar balancear as partições Function_0... 9 instâncias.

O fato de que N é maior do que o número de partições é dimensionamento lógica único atual das funções do Azure. Isso é feito para garantir que sempre há instâncias de EPH prontamente disponível para obter rapidamente o bloqueio de partição (ões) à medida que ficam disponíveis a partir de outras instâncias. Os utilizadores só são cobrados os recursos utilizados quando a instância de função for executada e não são cobradas deste aprovisionamento excessivo.

Se todas as execuções de função for bem-sucedida sem erros, os pontos de verificação são adicionados à conta de armazenamento associados. Quando a verificação for bem sucedida, todas as mensagens de 1000 nunca devem ser obtidas novamente.

## <a name="trigger---example"></a>Acionador - exemplo

Veja o exemplo de idioma específico:

* [C#](#trigger---c-example)
* [Script do c# (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Acionador - exemplo do c#

A exemplo a seguir mostra um [função c#](functions-dotnet-class-library.md) que regista o corpo da mensagem do acionador do hub de eventos.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Para obter acesso ao [metadados de eventos](#trigger---event-metadata) no código de função, vincular a um [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) objeto (requer a uma utilização de instrução para `Microsoft.ServiceBus.Messaging`). Também pode acessar as mesmas propriedades utilizando expressões de associação na assinatura do método.  O exemplo seguinte mostra as duas formas de obter os mesmos dados:

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

Para receber eventos num lote, tornar `string` ou `EventData` uma matriz:

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

O exemplo seguinte mostra um acionador do hub de eventos de enlace num *Function* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace. A função regista o corpo da mensagem do acionador do hub de eventos.

Os exemplos seguintes mostram a vinculação de dados de Hubs de eventos no *Function* ficheiro. O primeiro exemplo é para as funções 1.x e a segunda é para as funções 2.x. 

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

Aqui está o código de script do c#:

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Para obter acesso ao [metadados de eventos](#trigger---event-metadata) no código de função, vincular a um [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) objeto (requer a uma utilização de instrução para `Microsoft.ServiceBus.Messaging`). Também pode acessar as mesmas propriedades utilizando expressões de associação na assinatura do método.  O exemplo seguinte mostra as duas formas de obter os mesmos dados:

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

Para receber eventos num lote, tornar `string` ou `EventData` uma matriz:

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### <a name="trigger---f-example"></a>Acionador - exemplo do F #

O exemplo seguinte mostra um acionador do hub de eventos de enlace num *Function* ficheiro e uma [função F #](functions-reference-fsharp.md) que utiliza o enlace. A função regista o corpo da mensagem do acionador do hub de eventos.

Os exemplos seguintes mostram a vinculação de dados de Hubs de eventos no *Function* ficheiro. O primeiro exemplo é para as funções 1.x e a segunda é para as funções 2.x. 

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

O exemplo seguinte mostra um acionador do hub de eventos de enlace num *Function* ficheiro e uma [função JavaScript](functions-reference-node.md) que utiliza o enlace. A função lê [metadados de eventos](#trigger---event-metadata) e regista a mensagem.

Os exemplos seguintes mostram a vinculação de dados de Hubs de eventos no *Function* ficheiro. O primeiro exemplo é para as funções 1.x e a segunda é para as funções 2.x. 

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

Para receber eventos num lote, defina `cardinality` para `many` no *Function* ficheiro, conforme mostrado nos exemplos a seguir. O primeiro exemplo é para as funções 1.x e a segunda é para as funções 2.x. 

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

Na [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [EventHubTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs) atributo.

Construtor o atributo usa o nome do hub de eventos, o nome do grupo de consumidores e o nome de uma definição de aplicação que contém a cadeia de ligação. Para obter mais informações sobre estas definições, consulte a [acionar a seção de configuração](#trigger---configuration). Aqui está um `EventHubTriggerAttribute` exemplo de atributo:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, TraceWriter log)
{
    ...
}
```

Para obter um exemplo completo, consulte [acionador - exemplo do c#](#trigger---c-example).

## <a name="trigger---configuration"></a>Acionador - configuração

A tabela seguinte explica as propriedades de configuração de ligação definida no *Function* ficheiro e o `EventHubTrigger` atributo.

|propriedade de Function | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**tipo** | n/d | Tem de ser definido como `eventHubTrigger`. Esta propriedade é definida automaticamente ao criar o acionador no portal do Azure.|
|**direção** | n/d | Tem de ser definido como `in`. Esta propriedade é definida automaticamente ao criar o acionador no portal do Azure. |
|**name** | n/d | O nome da variável que representa o item de evento no código de função. | 
|**path** |**EventHubName** | Funciona apenas 1.x. O nome do hub de eventos.  | 
|**eventHubName** |**EventHubName** | Funciona apenas 2.x. O nome do hub de eventos.  |
|**consumerGroup** |**ConsumerGroup** | Uma propriedade opcional que define a [grupo de consumidores](../event-hubs/event-hubs-features.md#event-consumers) utilizada para subscrever a eventos no hub. Se for omitido, o `$Default` for utilizado. | 
|**Cardinalidade** | n/d | Para Javascript. Definido como `many` para ativar a criação de batches.  Se for omitido ou definido como `one`, única mensagem transmitido à função. | 
|**ligação** |**ligação** | O nome de uma definição de aplicação que contém a cadeia de ligação ao espaço de nomes do hub de eventos. Copiar esta cadeia de ligação ao clicar no **informações da ligação** botão para o [espaço de nomes](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace), não o hub de eventos em si. Esta cadeia de ligação tem de ter, pelo menos, permissões de leitura para ativar o acionador.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---event-metadata"></a>Acionador - metadados de eventos

O acionador de Hubs de eventos fornece vários [propriedades de metadados](functions-triggers-bindings.md#binding-expressions---trigger-metadata). Essas propriedades podem ser utilizadas como parte das expressões de associação nas outras associações, ou como parâmetros no seu código. Estas são as propriedades do [EventData](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.eventdata) classe.

|Propriedade|Tipo|Descrição|
|--------|----|-----------|
|`PartitionContext`|[PartitionContext](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.partitioncontext)|O `PartitionContext` instância.|
|`EnqueuedTimeUtc`|`DateTime`|O tempo de colocados em fila em UTC.|
|`Offset`|`string`|O deslocamento de dados em relação ao fluxo de partição do Hub de eventos. O desvio é um marcador ou um identificador para um evento dentro do fluxo de Hubs de eventos. O identificador é exclusivo dentro de uma partição da transmissão em fluxo dos Hubs de eventos.|
|`PartitionKey`|`string`|A partição para o evento que dados devem ser enviados.|
|`Properties`|`IDictionary<String,Object>`|As propriedades de utilizador dos dados do evento.|
|`SequenceNumber`|`Int64`|O número de sequência lógica do evento.|
|`SystemProperties`|`IDictionary<String,Object>`|As propriedades do sistema, incluindo os dados de eventos.|

Ver [exemplos de código](#trigger---example) que utilizam estas propriedades no início deste artigo.

## <a name="trigger---hostjson-properties"></a>Acionador - propriedades de Host. JSON

O [Host. JSON](functions-host-json.md#eventhub) ficheiro contém as definições que controlam o comportamento de Acionador de Hubs de eventos.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="output"></a>Saída

Utilize a enlace para gravar eventos para um fluxo de eventos de saída de Hubs de eventos. Tem de ter permissão de envio para um hub de eventos para escrever eventos no mesmo.

## <a name="output---example"></a>Saída - exemplo

Veja o exemplo de idioma específico:

* [C#](#output---c-example)
* [Script do c# (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Saída - exemplo do c#

A exemplo a seguir mostra um [função c#](functions-dotnet-class-library.md) que escreve uma mensagem para um hub de eventos, com o valor de retorno do método como a saída:

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

O exemplo seguinte mostra um acionador do hub de eventos de enlace num *Function* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace. A função escreve uma mensagem para um hub de eventos.

Os exemplos seguintes mostram a vinculação de dados de Hubs de eventos no *Function* ficheiro. O primeiro exemplo é para as funções 1.x e a segunda é para as funções 2.x. 

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

Aqui está o script código c# que cria uma mensagem:

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

O script código c# aqui está que cria várias mensagens:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Saída - exemplo do F #

O exemplo seguinte mostra um acionador do hub de eventos de enlace num *Function* ficheiro e uma [função F #](functions-reference-fsharp.md) que utiliza o enlace. A função escreve uma mensagem para um hub de eventos.

Os exemplos seguintes mostram a vinculação de dados de Hubs de eventos no *Function* ficheiro. O primeiro exemplo é para as funções 1.x e a segunda é para as funções 2.x. 

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

O exemplo seguinte mostra um acionador do hub de eventos de enlace num *Function* ficheiro e uma [função JavaScript](functions-reference-node.md) que utiliza o enlace. A função escreve uma mensagem para um hub de eventos.

Os exemplos seguintes mostram a vinculação de dados de Hubs de eventos no *Function* ficheiro. O primeiro exemplo é para as funções 1.x e a segunda é para as funções 2.x. 

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

Código de JavaScript a seguir que envia uma única mensagem:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Código de JavaScript a seguir que envia várias mensagens:

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

Construtor do atributo utiliza o nome do hub de eventos e o nome de uma definição de aplicação que contém a cadeia de ligação. Para obter mais informações sobre estas definições, consulte [de saída - configuração](#output---configuration). Aqui está um `EventHub` exemplo de atributo:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    ...
}
```

Para obter um exemplo completo, consulte [saída - exemplo do c#](#output---c-example).

## <a name="output---configuration"></a>Saída - configuração

A tabela seguinte explica as propriedades de configuração de ligação definida no *Function* ficheiro e o `EventHub` atributo.

|propriedade de Function | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**tipo** | n/d | Tem de ser definido para "eventHub". |
|**direção** | n/d | Tem de ser definido para "Sair". Este parâmetro é definido automaticamente quando criar o enlace no portal do Azure. |
|**name** | n/d | O nome da variável no código de função que representa o evento. | 
|**path** |**EventHubName** | Funciona apenas 1.x. O nome do hub de eventos.  | 
|**eventHubName** |**EventHubName** | Funciona apenas 2.x. O nome do hub de eventos.  |
|**ligação** |**ligação** | O nome de uma definição de aplicação que contém a cadeia de ligação ao espaço de nomes do hub de eventos. Copiar esta cadeia de ligação ao clicar no **informações da ligação** botão para o *espaço de nomes*, não o hub de eventos em si. Esta cadeia de ligação tem de ter permissões de envio para enviar a mensagem para o fluxo de eventos.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Saída - utilização

No c# e de script c#, enviar mensagens com um parâmetro de método como `out string paramName`. No script c# `paramName` é o valor especificado no `name` propriedade da *Function*. Para escrever várias mensagens, pode usar `ICollector<string>` ou `IAsyncCollector<string>` em vez de `out string`.

No JavaScript, acessar o evento de saída usando `context.bindings.<name>`. `<name>` é o valor especificado no `name` propriedade do *Function*.

## <a name="exceptions-and-return-codes"></a>Exceções e códigos de retorno

| Vínculo | Referência |
|---|---|
| Hub de Eventos | [Guia de operações](https://docs.microsoft.com/rest/api/eventhub/publisher-policy-operations) |

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre as funções do Azure acionadores e enlaces](functions-triggers-bindings.md)
