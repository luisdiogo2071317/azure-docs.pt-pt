---
title: Enlaces de armazenamento de filas do Azure para as funções do Azure
description: Compreenda como utilizar o acionador de armazenamento de filas do Azure e de saída do enlace das funções do Azure.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: das funções do Azure, funções, processamento de eventos, computação dinâmica, arquitetura sem servidor
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/23/2017
ms.author: tdykstra
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: d140822b47325e8749d3b2788b47cf820f720a39
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="azure-queue-storage-bindings-for-azure-functions"></a>Enlaces de armazenamento de filas do Azure para as funções do Azure

Este artigo explica como trabalhar com enlaces de armazenamento de filas do Azure das funções do Azure. Funções do Azure suporta acionam e enlaces para filas de saída.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages"></a>Pacotes

Os enlaces de armazenamento de filas são fornecidos no [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) pacote NuGet. Código de origem para o pacote está a ser o [sdk de webjobs do azure](https://github.com/Azure/azure-webjobs-sdk/tree/master/src) repositório do GitHub.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="trigger"></a>Acionador

Utilize o acionador de fila para iniciar uma função quando é recebido um novo item de uma fila. A mensagem da fila é fornecida como entrada para a função.

## <a name="trigger---example"></a>Acionador - exemplo

Veja o exemplo de específicas do idioma:

* [C#](#trigger---c-example)
* [Script do c# (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Acionador - c# exemplo

O seguinte exemplo mostra um [c# função](functions-dotnet-class-library.md) que consulta o `myqueue-items` fila e escreve um registo de cada vez que um item de fila é processado.

```csharp
public static class QueueFunctions
{
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}
```

### <a name="trigger---c-script-example"></a>Acionador - exemplo de script do c#

O exemplo seguinte mostra um acionador de fila enlace num *function.json* ficheiro e [script do c# (.csx)](functions-reference-csharp.md) código que utiliza o enlace. Os inquéritos de função a `myqueue-items` fila e escreve um registo de cada vez que um item de fila é processado.

Eis o *function.json* ficheiro:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

O [configuração](#trigger---configuration) secção explica estas propriedades.

Eis o código de script do c#:

```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem.AsString}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

O [utilização](#trigger---usage) secção explica `myQueueItem`, que é designado pelo `name` propriedade no function.json.  O [secção de metadados da mensagem](#trigger---message-metadata) explica todas as outras variáveis mostradas.

### <a name="trigger---javascript-example"></a>Acionador - exemplo de JavaScript

O exemplo seguinte mostra um acionador de fila enlace num *function.json* ficheiro e uma [JavaScript função](functions-reference-node.md) que utiliza o enlace. Os inquéritos de função a `myqueue-items` fila e escreve um registo de cada vez que um item de fila é processado.

Eis o *function.json* ficheiro:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

O [configuração](#trigger---configuration) secção explica estas propriedades.

Eis o código JavaScript:

```javascript
module.exports = function (context) {
    context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('queueTrigger =', context.bindingData.queueTrigger);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id =', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

O [utilização](#trigger---usage) secção explica `myQueueItem`, que é designado pelo `name` propriedade no function.json.  O [secção de metadados da mensagem](#trigger---message-metadata) explica todas as outras variáveis mostradas.

## <a name="trigger---attributes"></a>Acionador - atributos
 
No [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize os seguintes atributos para configurar um acionador de fila:

* [QueueTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueTriggerAttribute.cs)

  O construtor do atributo utiliza o nome da fila para monitorizar, conforme mostrado no exemplo seguinte:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items")] string myQueueItem, 
      TraceWriter log)
  {
      ...
  }
  ```

  Pode definir o `Connection` propriedade para especificar a conta de armazenamento a utilizar, conforme mostrado no exemplo seguinte:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items", Connection = "StorageConnectionAppSetting")] string myQueueItem, 
      TraceWriter log)
  {
      ....
  }
  ```
 
  Para obter um exemplo completado, consulte [acionador - c# exemplo](#trigger---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Outra forma para especificar a conta de armazenamento a utilizar. O construtor tem o nome de uma definição de aplicação que contém uma cadeia de ligação de armazenamento. O atributo pode ser aplicado no parâmetro, método ou nível de classe. O exemplo seguinte mostra o nível de classe e o nível de método:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("QueueTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

A conta de armazenamento a utilizar é determinada pela seguinte ordem:

* O `QueueTrigger` do atributo `Connection` propriedade.
* O `StorageAccount` atributo aplicado para o mesmo parâmetro como o `QueueTrigger` atributo.
* O `StorageAccount` atributo aplicado à função.
* O `StorageAccount` atributo aplicado à classe.
* A definição de aplicação "AzureWebJobsStorage".

## <a name="trigger---configuration"></a>Acionador - configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiros e o `QueueTrigger` atributo.

|propriedade de Function.JSON | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**tipo** | n/d| tem de ser definido como `queueTrigger`. Esta propriedade é definida automaticamente quando criar o acionador no portal do Azure.|
|**direção**| n/d | No *function.json* apenas de ficheiros. tem de ser definido como `in`. Esta propriedade é definida automaticamente quando criar o acionador no portal do Azure. |
|**name** | n/d |O nome da variável que representa a fila no código da função.  | 
|**queueName** | **QueueName**| O nome da fila para consultar. | 
|**Ligação** | **Ligação** |O nome de uma definição de aplicação que contém a cadeia de ligação de armazenamento a utilizar para este enlace. Se o nome da definição de aplicação começa com "AzureWebJobs", pode especificar apenas o resto do nome aqui. Por exemplo, se definir `connection` para "MyStorage", o tempo de execução de funções procura uma definição de aplicação com o nome "AzureWebJobsMyStorage." Se deixar `connection` vazio, o tempo de execução de funções utiliza a cadeia de ligação de armazenamento predefinida na definição de aplicação com o nome `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Acionador - utilização
 
Em c# e c# script, aceder aos dados mensagem utilizando um parâmetro de método como `string paramName`. No script do c#, `paramName` é o valor especificado no `name` propriedade *function.json*. É possível vincular a qualquer um dos seguintes tipos:

* Objeto - as funções de tempo de execução deserializes um payload JSON para uma instância de uma classe arbitrária definida no seu código. 
* `string`
* `byte[]`
* [CloudQueueMessage]

Em JavaScript, utilize `context.bindings.<name>` para aceder ao payload de item de fila. Se o payload JSON, este é anular a serialização para um objeto.

## <a name="trigger---message-metadata"></a>Acionador - mensagem metadados

O acionador de fila fornece vários [propriedades de metadados](functions-triggers-bindings.md#binding-expressions---trigger-metadata). Estas propriedades podem ser utilizadas como parte das expressões de enlace noutros enlaces ou como parâmetros no seu código. Os valores têm a mesma semântica como [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage).

|Propriedade|Tipo|Descrição|
|--------|----|-----------|
|`QueueTrigger`|`string`|Payload de fila (se for uma cadeia válida). Se a fila de mensagens payload como uma cadeia, `QueueTrigger` tem o mesmo valor que a variável com o nome de `name` propriedade no *function.json*.|
|`DequeueCount`|`int`|O número de vezes que esta mensagem foi removida.|
|`ExpirationTime`|`DateTimeOffset`|A hora de expiração a mensagem.|
|`Id`|`string`|ID de mensagem de fila.|
|`InsertionTime`|`DateTimeOffset`|O tempo que a mensagem foi adicionada à fila.|
|`NextVisibleTime`|`DateTimeOffset`|O tempo que a mensagem seguinte será visível.|
|`PopReceipt`|`string`|Receção pop da mensagem.|

## <a name="trigger---poison-messages"></a>Acionador - mensagens nocivas

Quando uma função de Acionador de fila falha, as funções do Azure tentará novamente a função de até cinco vezes para uma mensagem de fila especificado, incluindo a primeira tentativa. Se a todas as tentativas de cinco falharem, o tempo de execução de funções adiciona uma mensagem para uma fila com o nome  *&lt;originalqueuename >-nocivas*. Pode escrever uma função para processar mensagens da fila nocivas pelo registá-los ou enviar uma notificação que atenção manual é necessária.

Para processar mensagens nocivas manualmente, verifique o [dequeueCount](#trigger---message-metadata) da mensagem de fila.

## <a name="trigger---polling-algorithm"></a>Acionador - algoritmo de consulta

O acionador de fila implementa um aleatório exponencial término algoritmo para minimizar o efeito da fila de inatividade de consulta em custos de transação de armazenamento.  Quando é encontrada uma mensagem, tem de aguardar dois segundos e, em seguida, verifica a existência de outra mensagem; o tempo de execução Quando não é encontrada nenhuma mensagem, aguarda cerca de quatro segundos antes de tentar novamente. Depois de tentativas falhadas subsequentes para receber uma mensagem de fila, o tempo de espera continua a aumentar até atingir o tempo de espera máximo, que está predefinida para um minuto. O tempo de espera máximo é configurável através de `maxPollingInterval` propriedade no [host.json ficheiro](functions-host-json.md#queues).

## <a name="trigger---concurrency"></a>Acionador - simultaneidade

Quando existem vários de fila de mensagens a aguardar, o acionador de fila obtém um lote de mensagens e invoca instâncias de função em simultâneo para processá-los. Por predefinição, o tamanho do lote, é 16. Quando o número a ser processado obtém-se para baixo para 8, o tempo de execução obtém outro lote e começa a processar as mensagens. Por isso, o número máximo de mensagens em simultâneo a ser processado por uma máquina virtual (VM), a função é 24. Este limite aplica-se em separado para cada função de acionada pela fila de mensagens em fila em cada VM. Se a sua aplicação de função aumenta horizontalmente de forma a várias VMs, cada VM irá aguardar acionadores e tente executar as funções. Por exemplo, se uma aplicação de função aumenta horizontalmente de forma a 3 VMs, o número máximo predefinido de instâncias em simultâneo de uma função acionada pela fila é 72.

O tamanho de lote e o limiar para obter um novo lote são configuráveis no [host.json ficheiro](functions-host-json.md#queues). Se pretender minimizar a execução paralela para funções de acionada pela fila de mensagens em fila na aplicação de função, pode definir o tamanho do lote para 1. Esta definição elimina simultaneidade apenas desde que a aplicação de função é executada numa única máquina virtual (VM). 

O acionador de fila automaticamente impede que uma função de processar uma mensagem de fila várias vezes; as funções não tem de ser escrito para ser idempotent.

## <a name="trigger---hostjson-properties"></a>Acionador - host.json propriedades

O [host.json](functions-host-json.md#queues) ficheiro contém definições que controlam o comportamento de Acionador de fila.

[!INCLUDE [functions-host-json-queues](../../includes/functions-host-json-queues.md)]

## <a name="output"></a>Saída

Utilize a saída de armazenamento de filas do Azure para guardar as mensagens de uma fila de enlace.

## <a name="output---example"></a>De saída - exemplo

Veja o exemplo de específicas do idioma:

* [C#](#output---c-example)
* [Script do c# (.csx)](#output---c-script-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Saída - c# exemplo

O seguinte exemplo mostra um [c# função](functions-dotnet-class-library.md) que cria uma mensagem de fila para cada pedido HTTP recebido.

```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  TraceWriter log)
    {
        log.Info($"C# function processed: {input.Text}");
        return input.Text;
    }
}
```

### <a name="output---c-script-example"></a>Saída - exemplo de script do c#

O exemplo seguinte mostra um acionador HTTP enlace num *function.json* ficheiro e [script do c# (.csx)](functions-reference-csharp.md) código que utiliza o enlace. A função cria um item de fila com um **CustomQueueMessage** payload de objeto para cada pedido HTTP recebido.

Eis o *function.json* ficheiro:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionAppSetting",
    }
  ]
}
``` 

O [configuração](#output---configuration) secção explica estas propriedades.

Eis c# código de script que cria uma mensagem de fila única:

```cs
public class CustomQueueMessage
{
    public string PersonName { get; set; }
    public string Title { get; set; }
}

public static CustomQueueMessage Run(CustomQueueMessage input, TraceWriter log)
{
    return input;
}
```

Pode enviar mensagens de vários em simultâneo utilizando um `ICollector` ou `IAsyncCollector` parâmetro. Script código c# Eis que envia mensagens vários, uma com os dados do pedido de HTTP e uma com valores hard-coded:

```cs
public static void Run(
    CustomQueueMessage input, 
    ICollector<CustomQueueMessage> myQueueItems, 
    TraceWriter log)
{
    myQueueItems.Add(input);
    myQueueItems.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

### <a name="output---javascript-example"></a>Saída - exemplo de JavaScript

O exemplo seguinte mostra um acionador HTTP enlace num *function.json* ficheiro e uma [JavaScript função](functions-reference-node.md) que utiliza o enlace. A função cria um item de fila para cada pedido HTTP recebido.

Eis o *function.json* ficheiro:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionAppSetting",
    }
  ]
}
``` 

O [configuração](#output---configuration) secção explica estas propriedades.

Eis o código JavaScript:

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

Pode enviar várias mensagens ao mesmo tempo que define uma matriz de mensagem para o `myQueueItem` vínculo de saída. O seguinte código JavaScript envia duas mensagens de fila com valores hard-coded para cada pedido HTTP recebido.

```javascript
module.exports = function(context) {
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="output---attributes"></a>Saída - atributos
 
No [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [QueueAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs).

O atributo aplica-se a um `out` parâmetro ou o valor de retorno da função. O construtor do atributo utiliza o nome da fila, conforme mostrado no exemplo seguinte:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items")]
public static string Run([HttpTrigger] dynamic input,  TraceWriter log)
{
    ...
}
```

Pode definir o `Connection` propriedade para especificar a conta de armazenamento a utilizar, conforme mostrado no exemplo seguinte:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items", Connection = "StorageConnectionAppSetting")]
public static string Run([HttpTrigger] dynamic input,  TraceWriter log)
{
    ...
}
```

Para obter um exemplo completado, consulte [resultado - c# exemplo](#output---c-example).

Pode utilizar o `StorageAccount` atributo para especificar a conta de armazenamento ao nível de classe, método ou parâmetro. Para obter mais informações, consulte [acionador - atributos](#trigger---attribute).

## <a name="output---configuration"></a>De saída - configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiros e o `Queue` atributo.

|propriedade de Function.JSON | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**tipo** | n/d | tem de ser definido como `queue`. Esta propriedade é definida automaticamente quando criar o acionador no portal do Azure.|
|**direção** | n/d | tem de ser definido como `out`. Esta propriedade é definida automaticamente quando criar o acionador no portal do Azure. |
|**name** | n/d | O nome da variável que representa a fila no código da função. Definido como `$return` para referenciar o valor de retorno da função.| 
|**queueName** |**QueueName** | O nome da fila. | 
|**Ligação** | **Ligação** |O nome de uma definição de aplicação que contém a cadeia de ligação de armazenamento a utilizar para este enlace. Se o nome da definição de aplicação começa com "AzureWebJobs", pode especificar apenas o resto do nome aqui. Por exemplo, se definir `connection` para "MyStorage", o tempo de execução de funções procura uma definição de aplicação com o nome "AzureWebJobsMyStorage." Se deixar `connection` vazio, o tempo de execução de funções utiliza a cadeia de ligação de armazenamento predefinida na definição de aplicação com o nome `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Saída - utilização
 
Em c# e c# script, gravar uma mensagem de fila única utilizando um parâmetro de método como `out T paramName`. No script do c#, `paramName` é o valor especificado no `name` propriedade *function.json*. Pode utilizar o tipo de retorno do método em vez de um `out` parâmetro, e `T` pode ser qualquer um dos seguintes tipos:

* Um objeto serializável como JSON
* `string`
* `byte[]`
* [CloudQueueMessage] 

Em c# e c# script, escreva vários de fila de mensagens através de um dos seguintes tipos: 

* `ICollector<T>` ou `IAsyncCollector<T>`
* [CloudQueue](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

Funções de JavaScript, utilizar `context.bindings.<name>` para aceder a mensagem da fila de saída. Pode utilizar uma cadeia ou um objeto JSON serializável para o payload de item de fila.


## <a name="exceptions-and-return-codes"></a>Exceções e códigos de retorno

| Vínculo |  Referência |
|---|---|
| Filas | [Códigos de erro da fila](https://docs.microsoft.com/rest/api/storageservices/queue-service-error-codes) |
| BLOB, tabela, fila | [Códigos de erro do armazenamento](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| BLOB, tabela, fila |  [Resolução de problemas](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Ir para um guia de introdução que utiliza um acionador de armazenamento de filas](functions-create-storage-queue-triggered-function.md)

> [!div class="nextstepaction"]
> [Ir para um tutorial que utiliza um armazenamento de filas de enlace de saída](functions-integrate-storage-queue-output-binding.md)

> [!div class="nextstepaction"]
> [Saiba mais sobre as funções do Azure acionadores e enlaces](functions-triggers-bindings.md)

<!-- LINKS -->

[CloudQueueMessage]: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage
