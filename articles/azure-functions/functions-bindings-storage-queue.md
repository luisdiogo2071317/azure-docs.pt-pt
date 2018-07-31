---
title: Enlaces de armazenamento de filas do Azure para as funções do Azure
description: Compreenda como utilizar o acionador do armazenamento de filas do Azure e enlace nas funções do Azure de saída.
services: functions
documentationcenter: na
author: ggailey777
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
ms.author: glenga
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 04502e80cea096ce384f97559bc7bad95ee2bcd8
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/30/2018
ms.locfileid: "39344104"
---
# <a name="azure-queue-storage-bindings-for-azure-functions"></a>Enlaces de armazenamento de filas do Azure para as funções do Azure

Este artigo explica como trabalhar com ligações de armazenamento de filas do Azure nas funções do Azure. Funções do Azure suporta acionaram e enlaces de filas de saída.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pacotes - funções 1.x

Os enlaces de armazenamento de filas são fornecidos na [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) pacote NuGet, versão 2.x. Código-fonte para o pacote está no [sdk do webjobs do azure](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Queue) repositório do GitHub.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="packages---functions-2x"></a>Pacotes - funções 2.x

Os enlaces de armazenamento de filas são fornecidos na [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) pacote NuGet, versão 3.x. Código-fonte para o pacote está no [sdk do webjobs do azure](https://github.com/Azure/azure-webjobs-sdk/tree/master/src/Microsoft.Azure.WebJobs.Storage/Queue) repositório do GitHub.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="trigger"></a>Acionador

Utilize o acionador de fila para iniciar uma função, quando um novo item é recebido numa fila. A mensagem de fila é fornecida como entrada para a função.

## <a name="trigger---example"></a>Acionador - exemplo

Veja o exemplo de idioma específico:

* [C#](#trigger---c-example)
* [Script do c# (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Acionador - exemplo do c#

A exemplo a seguir mostra um [função c#](functions-dotnet-class-library.md) que consulta o `myqueue-items` da fila e registra um log sempre que um item da fila é processado.

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

O exemplo seguinte mostra um acionador de fila de enlace num *Function* ficheiro e [script c# (. csx)](functions-reference-csharp.md) código que utiliza o enlace. O consulta de função a `myqueue-items` da fila e registra um log sempre que um item da fila é processado.

Aqui está o *Function* ficheiro:

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

O [configuração](#trigger---configuration) seção explica essas propriedades.

Aqui está o código de script do c#:

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

O [utilização](#trigger---usage) secção explica `myQueueItem`, que é chamado pelo `name` propriedade na Function.  O [secção de metadados de mensagens](#trigger---message-metadata) explica todas as outras variáveis mostradas.

### <a name="trigger---javascript-example"></a>Acionador - exemplo de JavaScript

O exemplo seguinte mostra um acionador de fila de enlace num *Function* ficheiro e uma [função JavaScript](functions-reference-node.md) que utiliza o enlace. O consulta de função a `myqueue-items` da fila e registra um log sempre que um item da fila é processado.

Aqui está o *Function* ficheiro:

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

O [configuração](#trigger---configuration) seção explica essas propriedades.

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

O [utilização](#trigger---usage) secção explica `myQueueItem`, que é chamado pelo `name` propriedade na Function.  O [secção de metadados de mensagens](#trigger---message-metadata) explica todas as outras variáveis mostradas.

## <a name="trigger---attributes"></a>Acionador - atributos
 
Na [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize os seguintes atributos para configurar um acionador de fila:

* [QueueTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueTriggerAttribute.cs)

  Construtor do atributo utiliza o nome da fila para monitorizar, conforme mostrado no exemplo a seguir:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items")] string myQueueItem, 
      TraceWriter log)
  {
      ...
  }
  ```

  Pode definir o `Connection` propriedade para especificar a conta de armazenamento a utilizar, conforme mostrado no exemplo a seguir:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items", Connection = "StorageConnectionAppSetting")] string myQueueItem, 
      TraceWriter log)
  {
      ....
  }
  ```
 
  Para obter um exemplo completo, consulte [acionador - exemplo do c#](#trigger---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Proporciona outra forma de especificar a conta de armazenamento a utilizar. O construtor obtém o nome de uma definição de aplicação que contém uma cadeia de ligação de armazenamento. O atributo pode ser aplicado no nível de classe, método ou parâmetro. O exemplo seguinte mostra o nível de classe e método:

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
* O `StorageAccount` aplicado à classe de atributo.
* A definição de aplicação "AzureWebJobsStorage".

## <a name="trigger---configuration"></a>Acionador - configuração

A tabela seguinte explica as propriedades de configuração de ligação definida no *Function* ficheiro e o `QueueTrigger` atributo.

|propriedade de Function | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**tipo** | n/d| Tem de ser definido como `queueTrigger`. Esta propriedade é definida automaticamente ao criar o acionador no portal do Azure.|
|**direção**| n/d | Na *Function* apenas de ficheiros. Tem de ser definido como `in`. Esta propriedade é definida automaticamente ao criar o acionador no portal do Azure. |
|**name** | n/d |O nome da variável que representa a fila no código de função.  | 
|**queueName** | **QueueName**| O nome da fila para consultar. | 
|**ligação** | **ligação** |O nome de uma definição de aplicação que contém a cadeia de ligação de armazenamento a utilizar para essa ligação. Se o nome da definição de aplicação começa com "AzureWebJobs", pode especificar apenas o restante do nome aqui. Por exemplo, se definir `connection` para "MyStorage", o runtime das funções procura uma definição de aplicação com o nome "AzureWebJobsMyStorage." Se deixar `connection` vazio, o runtime das funções utiliza a cadeia de ligação de armazenamento predefinida na definição da aplicação com o nome `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Acionador - utilização
 
No c# e de script c#, acessar os dados de mensagem com um parâmetro de método como `string paramName`. No script c# `paramName` é o valor especificado no `name` propriedade da *Function*. É possível ligar a qualquer um dos seguintes tipos:

* Objeto - o runtime das funções desserializa um payload JSON numa instância de uma classe arbitrária definida no seu código. 
* `string`
* `byte[]`
* [CloudQueueMessage]

Se tentar fazer a ligação `CloudQueueMessage` e obter uma mensagem de erro, certifique-se de que tem uma referência ao [a versão correta do SDK de armazenamento](#azure-storage-sdk-version-in-functions-1x).

No JavaScript, utilize `context.bindings.<name>` para acessar o payload de item de fila. Se o payload JSON, é desserializado num objeto.

## <a name="trigger---message-metadata"></a>Acionador - metadados de mensagem

O acionador de fila fornece vários [propriedades de metadados](functions-triggers-bindings.md#binding-expressions---trigger-metadata). Essas propriedades podem ser utilizadas como parte das expressões de associação nas outras associações, ou como parâmetros no seu código. Estas são as propriedades do [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage) classe.

|Propriedade|Tipo|Descrição|
|--------|----|-----------|
|`QueueTrigger`|`string`|Payload de fila (se uma cadeia de caracteres válida). Se a fila de mensagens payload como uma cadeia de caracteres `QueueTrigger` tem o mesmo valor que a variável com o nome do `name` propriedade na *Function*.|
|`DequeueCount`|`int`|O número de vezes que esta mensagem foram removidos da fila.|
|`ExpirationTime`|`DateTimeOffset`|O tempo que a mensagem expira.|
|`Id`|`string`|ID de mensagem de fila.|
|`InsertionTime`|`DateTimeOffset`|O tempo que a mensagem foi adicionada à fila de mensagens em fila.|
|`NextVisibleTime`|`DateTimeOffset`|O tempo que a mensagem a seguir estarão visível.|
|`PopReceipt`|`string`|Receção de pop da mensagem.|

## <a name="trigger---poison-messages"></a>Acionador - mensagens suspeitas

Quando uma função de Acionador de fila falha, as funções do Azure repete a função de até cinco vezes de uma mensagem de fila indicada, incluindo a primeira tentativa. Se todas as tentativas de cinco falharem, o runtime das funções adiciona uma mensagem a uma fila com o nome  *&lt;originalqueuename >-não processáveis*. Pode escrever uma função para processar mensagens da fila não processáveis por registar ou enviar uma notificação que atenção manual é necessária.

Para lidar com mensagens suspeitas manualmente, consulte a [dequeueCount](#trigger---message-metadata) da mensagem de fila.

## <a name="trigger---polling-algorithm"></a>Acionador - algoritmo da consulta

O acionador de fila implementa um aleatório exponencial término algoritmo para minimizar o efeito da fila de inatividade de consulta nos custos de transação de armazenamento.  Quando é encontrada uma mensagem, o tempo de execução tem de aguardar dois segundos e, em seguida, verifica a existência de outra mensagem; Quando não é encontrada nenhuma mensagem, ele aguarda cerca de quatro segundos antes de tentar novamente. Depois de tentativas falhadas subsequentes para obter uma mensagem de fila, o tempo de espera continua a aumentar até atingir o tempo de espera máximo, o que está predefinida para um minuto. O tempo de espera máximo é configurável através da `maxPollingInterval` propriedade no [ficheiro Host. JSON](functions-host-json.md#queues).

## <a name="trigger---concurrency"></a>Acionador - a simultaneidade

Quando existem várias mensagens na fila à espera, o acionador de fila obtém um lote de mensagens e invoca a instâncias de função em simultâneo para processá-las. Por predefinição, o tamanho do lote é 16. Quando o número a ser processado chega a 8, o tempo de execução obtém outro lote e começa a processar essas mensagens. Portanto, o número máximo de mensagens em simultâneo a ser processado por função numa máquina virtual (VM) é 24. Este limite aplica-se em separado para cada função de acionada por fila de mensagens em fila em cada VM. Se a sua aplicação function app aumenta horizontalmente para várias VMs, cada VM irá esperar para acionadores e tentar executar as funções. Por exemplo, se uma aplicação de funções, aumenta horizontalmente, de 3 VMs, o número máximo predefinido de instâncias simultâneas de uma função acionada por fila é 72.

O tamanho de lote e o limiar para obter um novo lote podem ser configuradas no [Host. JSON ficheiro](functions-host-json.md#queues). Se deseja minimizar a execução paralela para funções de acionada por fila de mensagens em fila numa aplicação de funções, pode definir o tamanho do lote para 1. Esta definição elimina simultaneidade apenas, desde que a aplicação de função for executada numa única máquina virtual (VM). 

O acionador de fila automaticamente impede que uma função de processamento de uma mensagem de fila várias vezes; as funções não tem de ser escrito para ser idempotentes.

## <a name="trigger---hostjson-properties"></a>Acionador - propriedades de Host. JSON

O [Host. JSON](functions-host-json.md#queues) ficheiro contém as definições que controlam o comportamento de Acionador de fila.

[!INCLUDE [functions-host-json-queues](../../includes/functions-host-json-queues.md)]

## <a name="output"></a>Saída

Utilize a saída de armazenamento de filas do Azure ao escrever as mensagens numa fila de enlace.

## <a name="output---example"></a>Saída - exemplo

Veja o exemplo de idioma específico:

* [C#](#output---c-example)
* [Script do c# (.csx)](#output---c-script-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Saída - exemplo do c#

A exemplo a seguir mostra um [função c#](functions-dotnet-class-library.md) que cria uma mensagem de fila para cada pedido HTTP recebido.

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

O exemplo seguinte mostra um acionador HTTP de enlace num *Function* ficheiro e [script c# (. csx)](functions-reference-csharp.md) código que utiliza o enlace. A função cria um item de fila com um **CustomQueueMessage** payload de objeto para cada pedido HTTP recebido.

Aqui está o *Function* ficheiro:

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

O [configuração](#output---configuration) seção explica essas propriedades.

Aqui está o script código c# que cria uma mensagem de fila única:

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

Pode enviar várias mensagens de uma só vez com um `ICollector` ou `IAsyncCollector` parâmetro. O script código c# aqui está que envia várias mensagens, um com os dados de pedido HTTP e outro com valores codificados:

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

O exemplo seguinte mostra um acionador HTTP de enlace num *Function* ficheiro e uma [função JavaScript](functions-reference-node.md) que utiliza o enlace. A função cria um item da fila para cada pedido HTTP recebido.

Aqui está o *Function* ficheiro:

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

O [configuração](#output---configuration) seção explica essas propriedades.

Eis o código JavaScript:

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

Pode enviar várias mensagens ao mesmo tempo, definindo uma matriz de mensagem para o `myQueueItem` enlace de saída. O seguinte código JavaScript envia dois de fila de mensagens com valores codificados para cada pedido HTTP recebido.

```javascript
module.exports = function(context) {
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="output---attributes"></a>Saída - atributos
 
Na [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [QueueAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs).

O atributo aplica-se a um `out` parâmetro ou o valor de retorno da função. Construtor do atributo utiliza o nome da fila, conforme mostrado no exemplo a seguir:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items")]
public static string Run([HttpTrigger] dynamic input,  TraceWriter log)
{
    ...
}
```

Pode definir o `Connection` propriedade para especificar a conta de armazenamento a utilizar, conforme mostrado no exemplo a seguir:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items", Connection = "StorageConnectionAppSetting")]
public static string Run([HttpTrigger] dynamic input,  TraceWriter log)
{
    ...
}
```

Para obter um exemplo completo, consulte [saída - exemplo do c#](#output---c-example).

Pode utilizar o `StorageAccount` atributo para especificar a conta de armazenamento ao nível de classe, método ou parâmetro. Para obter mais informações, consulte [acionador - atributos](#trigger---attribute).

## <a name="output---configuration"></a>Saída - configuração

A tabela seguinte explica as propriedades de configuração de ligação definida no *Function* ficheiro e o `Queue` atributo.

|propriedade de Function | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**tipo** | n/d | Tem de ser definido como `queue`. Esta propriedade é definida automaticamente ao criar o acionador no portal do Azure.|
|**direção** | n/d | Tem de ser definido como `out`. Esta propriedade é definida automaticamente ao criar o acionador no portal do Azure. |
|**name** | n/d | O nome da variável que representa a fila no código de função. Definido como `$return` para referenciar o valor de retorno da função.| 
|**queueName** |**QueueName** | O nome da fila. | 
|**ligação** | **ligação** |O nome de uma definição de aplicação que contém a cadeia de ligação de armazenamento a utilizar para essa ligação. Se o nome da definição de aplicação começa com "AzureWebJobs", pode especificar apenas o restante do nome aqui. Por exemplo, se definir `connection` para "MyStorage", o runtime das funções procura uma definição de aplicação com o nome "AzureWebJobsMyStorage." Se deixar `connection` vazio, o runtime das funções utiliza a cadeia de ligação de armazenamento predefinida na definição da aplicação com o nome `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Saída - utilização
 
No c# e de script c#, gravar uma mensagem de fila única com um parâmetro de método como `out T paramName`. No script c# `paramName` é o valor especificado no `name` propriedade da *Function*. Pode usar o tipo de retorno do método em vez de um `out` parâmetro, e `T` pode ser qualquer um dos seguintes tipos:

* Um objeto serializável como JSON
* `string`
* `byte[]`
* [CloudQueueMessage] 

Se tentar fazer a ligação `CloudQueueMessage` e obter uma mensagem de erro, certifique-se de que tem uma referência ao [a versão correta do SDK de armazenamento](#azure-storage-sdk-version-in-functions-1x).

No c# e no c# script, escreva várias mensagens de fila utilizando um dos seguintes tipos: 

* `ICollector<T>` ou `IAsyncCollector<T>`
* [CloudQueue](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

Nas funções de JavaScript, utilize `context.bindings.<name>` para aceder a mensagem de fila de saída. Pode usar uma cadeia de caracteres ou um objeto JSON serializável para o payload de item de fila.


## <a name="exceptions-and-return-codes"></a>Exceções e códigos de retorno

| Vínculo |  Referência |
|---|---|
| Filas | [Códigos de erro de fila](https://docs.microsoft.com/rest/api/storageservices/queue-service-error-codes) |
| BLOB, tabela, fila | [Códigos de erro de armazenamento](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| BLOB, tabela, fila |  [Resolução de problemas](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Ir para um início rápido que utiliza um acionador do armazenamento de fila](functions-create-storage-queue-triggered-function.md)

> [!div class="nextstepaction"]
> [Enlace de saída de ir para um tutorial que utiliza um armazenamento de filas](functions-integrate-storage-queue-output-binding.md)

> [!div class="nextstepaction"]
> [Saiba mais sobre as funções do Azure acionadores e enlaces](functions-triggers-bindings.md)

<!-- LINKS -->

[CloudQueueMessage]: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage
