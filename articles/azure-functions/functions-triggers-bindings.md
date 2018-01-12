---
title: "Acionadores e enlaces de funções do Azure"
description: "Saiba como utilizar acionadores e enlaces das funções do Azure para ligar a execução do código para serviços baseados na nuvem e eventos online."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "funções do azure, funções, processamento de eventos, webhooks, computação dinâmica, arquitetura sem servidor"
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: a122271b5fdffd9db33a7dca5908e15f002041d7
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2018
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Conceitos de enlaces e acionadores de funções do Azure

Este artigo é uma descrição geral conceptual dos acionadores e enlaces de funções do Azure. As funcionalidades que são comuns a todos os enlaces e todos os idiomas suportados são descritas aqui.

## <a name="overview"></a>Descrição geral

A *acionador* define a forma como uma função é invocada. Uma função tem de ter exatamente um acionador. Acionadores associou dados, o que é normalmente o payload que acionou a função.

Entrada e saída *enlaces* proporcionam uma forma declarativa para ligar a dados a partir de dentro do seu código. Enlaces são opcionais e uma função pode ter vários de entrada e saída enlaces. 

Acionadores e enlaces permitem-lhe evitar codificar os detalhes dos serviços de está a trabalhar. A função recebe dados (por exemplo, o conteúdo de uma mensagem de fila) nos parâmetros de função. Enviar dados (por exemplo, para criar uma mensagem de fila) utilizando o valor de retorno da função, uma `out` parâmetro, ou um [objeto recoletor](functions-reference-csharp.md#writing-multiple-output-values).

Quando desenvolver as funções utilizando o portal do Azure, acionadores e enlaces estão configurados num *function.json* ficheiro. O portal fornece uma IU para esta configuração, mas pode editar o ficheiro diretamente pela alteração para o **editor avançada**.

Quando desenvolver as funções utilizando o Visual Studio para criar uma biblioteca de classe, configure acionadores e enlaces ao decorating métodos e os parâmetros com atributos.

## <a name="supported-bindings"></a>Enlaces suportados

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Para obter informações sobre os quais os enlaces estão na pré-visualização ou estão aprovados para utilização em produção, consulte [idiomas suportados](supported-languages.md).

## <a name="example-queue-trigger-and-table-output-binding"></a>Exemplo: acionador de filas e tabela de saída do enlace

Suponha que pretende escrever uma nova linha no Table storage do Azure, sempre que uma nova mensagem é apresentada no armazenamento de filas do Azure. Este cenário pode ser implementado através de uma fila do Azure vínculo de saída de Acionador de armazenamento e um Table storage do Azure. 

Eis um *function.json* ficheiros para este cenário. 

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

O primeiro elemento no `bindings` matriz é o acionador de armazenamento de filas. O `type` e `direction` propriedades identificam o acionador. O `name` propriedade identifica o parâmetro de função que irão receber o conteúdo da mensagem de fila. O nome da fila para monitorizar está a ser `queueName`, e a cadeia de ligação tem a definição de aplicação identificada por `connection`.

O elemento de segundo a `bindings` matriz é o Table Storage do Azure vínculo de saída. O `type` e `direction` propriedades identificam o enlace. O `name` propriedade especifica a forma como a função irá fornecer a nova linha de tabela, neste caso, utilizando o valor de retorno da função. O nome da tabela está a ser `tableName`, e a cadeia de ligação tem a definição de aplicação identificada por `connection`.

Para ver e editar o conteúdo do *function.json* no portal do Azure, clique em de **editor avançada** opção o **integrar** separador da sua função.

> [!NOTE]
> O valor de `connection` é o nome de uma definição de aplicação que contenha a cadeia de ligação, não a cadeia de ligação em si. Enlaces utilizam ligação cadeias armazenadas nas definições de aplicação para impor as melhores práticas que *function.json* não contém segredos do serviço.

Eis o script código c# que funciona com este acionador e o enlace. Tenha em atenção que o nome do parâmetro que fornece o conteúdo da mensagem de fila é `order`; este nome é necessário porque o `name` valor da propriedade no *function.json* é`order` 

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table storage
// The method return value creates a new row in Table Storage
public static Person Run(JObject order, TraceWriter log)
{
    return new Person() { 
            PartitionKey = "Orders", 
            RowKey = Guid.NewGuid().ToString(),  
            Name = order["Name"].ToString(),
            MobileNumber = order["MobileNumber"].ToString() };  
}
 
public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

O mesmo ficheiro function.json pode ser utilizado com uma função de JavaScript:

```javascript
// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The second parameter to context.done is used as the value for the new row
module.exports = function (context, order) {
    order.PartitionKey = "Orders";
    order.RowKey = generateRandomId(); 

    context.done(null, order);
};

function generateRandomId() {
    return Math.random().toString(36).substring(2, 15) +
        Math.random().toString(36).substring(2, 15);
}
```

Na biblioteca de classes, o mesmo acionador e informações de enlace &mdash; parâmetros de entrada e saída da função de nomes de filas e tabela, contas de armazenamento, &mdash; é fornecido por atributos:

```csharp
 public static class QueueTriggerTableOutput
 {
     [FunctionName("QueueTriggerTableOutput")]
     [return: Table("outTable", Connection = "MY_TABLE_STORAGE_ACCT_APP_SETTING")]
     public static Person Run(
         [QueueTrigger("myqueue-items", Connection = "MY_STORAGE_ACCT_APP_SETTING")]JObject order, 
         TraceWriter log)
     {
         return new Person() {
                 PartitionKey = "Orders",
                 RowKey = Guid.NewGuid().ToString(),
                 Name = order["Name"].ToString(),
                 MobileNumber = order["MobileNumber"].ToString() };
     }
 }

 public class Person
 {
     public string PartitionKey { get; set; }
     public string RowKey { get; set; }
     public string Name { get; set; }
     public string MobileNumber { get; set; }
 }
```

## <a name="binding-direction"></a>Direção de enlace

Todos os acionadores e enlaces de tem um `direction` propriedade no *function.json* ficheiro:

- Para acionadores, a direção é sempre`in`
- Utilizam enlaces de entrada e de saída `in` e`out`
- Alguns enlaces suportam uma direção especial `inout`. Se utilizar `inout`, apenas o **editor avançada** está disponível no **integrar** separador.

Quando utiliza [atributos na biblioteca de classes](functions-dotnet-class-library.md) para configurar os acionadores e enlaces, a direção é fornecida no construtor de atributos ou inferida a partir do tipo de parâmetro.

## <a name="using-the-function-return-type-to-return-a-single-output"></a>Utilizar o tipo de retorno da função para devolver um resultado único

O exemplo anterior mostra como utilizar o valor de retorno da função para fornecer a saída para um enlace, o que é especificado na *function.json* utilizando o valor especial `$return` para o `name` propriedade. (Esta é apenas suportada em idiomas que tenham um valor de retorno, como o script do c#, JavaScript e F #.) Se uma função tem múltiplos enlaces de resultados, utilize `$return` para apenas um dos enlaces de saída. 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Os exemplos abaixo mostram como devolvem tipos são utilizados com enlaces de saída em c# script, JavaScript e F #.

```cs
// C# example: use method return value for output binding
public static string Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
// C# example: async method, using return value for output binding
public static Task<string> Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

```javascript
// JavaScript: return a value in the second parameter to context.done
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

```fsharp
// F# example: use return value for output binding
let Run(input: WorkItem, log: TraceWriter) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.Info(sprintf "F# script processed queue message '%s'" json)
    json
```

## <a name="binding-datatype-property"></a>Propriedade de tipo de dados de enlace

No .NET, utilize o tipo de parâmetro para definir o tipo de dados para dados de entrada. Por exemplo, utilize `string` ao vincular ao texto de um acionador de fila, uma matriz de bytes para leitura como binário e um tipo personalizado para anular a serialização para um objeto de POCO.

Para os idiomas que são escritos dinamicamente, como JavaScript, utilize o `dataType` propriedade no *function.json* ficheiro. Por exemplo, para ler o conteúdo de um pedido HTTP no formato binário, defina `dataType` para `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Outras opções para `dataType` são `stream` e `string`.

## <a name="resolving-app-settings"></a>Resolver as definições de aplicação

Como melhor prática, os segredos e cadeias de ligação devem ser geridas utilizando as definições de aplicação, em vez de ficheiros de configuração. Isto limita o acesso a estes segredos e torna seguro armazenar *function.json* num repositório de controlo de origem público.

As definições de aplicações também são úteis sempre que pretender alterar a configuração com base no ambiente. Por exemplo, num ambiente de teste, pode querer monitorizar um contentor de armazenamento de fila ou blob diferente.

São resolvidas as definições de aplicação sempre que um valor está entre símbolos de percentagem inicia, tais como `%MyAppSetting%`. Tenha em atenção que o `connection` propriedade de acionadores e enlaces é num caso especial e resolve automaticamente os valores das definições de aplicação. 

O exemplo seguinte é um acionador de armazenamento de filas do Azure que utiliza uma definição de aplicação `%input-queue-name%` para definir a fila para acionar.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "%input-queue-name%",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

Pode utilizar a mesma abordagem em bibliotecas de classe:

```csharp
[FunctionName("QueueTrigger")]
public static void Run(
    [QueueTrigger("%input-queue-name%")]string myQueueItem, 
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
}
```

## <a name="trigger-metadata-properties"></a>Propriedades de metadados do acionador

Para além do payload de dados fornecido por um acionador (por exemplo, a mensagem da fila que acionou uma função), acionadores muitos forneça valores de metadados adicionais. Estes valores podem ser utilizados como parâmetros de entrada em c# e F # ou propriedades no `context.bindings` objeto em JavaScript. 

Por exemplo, um acionador de armazenamento de filas do Azure suporta as seguintes propriedades:

* QueueTrigger - acionar o conteúdo da mensagem se uma cadeia válida
* DequeueCount
* expirationTime
* Id
* InsertionTime
* NextVisibleTime
* PopReceipt

Estes valores de metadados estão acessíveis no *function.json* propriedades do ficheiro. Por exemplo, suponha que utiliza um acionador de fila e a mensagem da fila contém o nome de um blob que pretende ler. No *function.json* ficheiro, pode utilizar `queueTrigger` propriedade de metadados no blob `path` propriedade, conforme mostrado no exemplo seguinte:

```json
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "direction": "in",
      "connection": "MyStorageConnection"
    }
  ]
```

Detalhes das propriedades de metadados para cada acionador são descritos no artigo de referência correspondente. Por exemplo, consulte [metadados de Acionador de fila](functions-bindings-storage-queue.md#trigger---message-metadata). Também está disponível na documentação sobre o **integrar** separador do portal, no **documentação** secção abaixo da área de configuração do enlace.  

## <a name="binding-expressions-and-patterns"></a>Expressões de enlace e padrões

Uma das funcionalidades mais poderosas de acionadores e enlaces é *expressões de enlace*. Na configuração para um enlace, pode definir as expressões de padrão que, em seguida, podem ser utilizadas noutros enlaces ou o seu código. Metadados de Acionador também podem ser utilizado em expressões de enlace, conforme mostrado na secção anterior.

Por exemplo, suponha que pretende redimensionar imagens de um determinado contentor de blob storage, semelhante ao **Resizer imagem** modelo no **nova função** página do portal do Azure (consulte o **amostras**  cenário). 

Eis o *function.json* definição:

```json
{
  "bindings": [
    {
      "name": "image",
      "type": "blobTrigger",
      "path": "sample-images/{filename}",
      "direction": "in",
      "connection": "MyStorageConnection"
    },
    {
      "name": "imageSmall",
      "type": "blob",
      "path": "sample-images-sm/{filename}",
      "direction": "out",
      "connection": "MyStorageConnection"
    }
  ],
}
```

Tenha em atenção que o `filename` parâmetro é utilizado na definição do acionador de blob e o blob de saída do enlace. Este parâmetro também pode ser utilizado no código da função.

```csharp
// C# example of binding to {filename}
public static void Run(Stream image, string filename, Stream imageSmall, TraceWriter log)  
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
} 
```

<!--TODO: add JavaScript example -->
<!-- Blocked by bug https://github.com/Azure/Azure-Functions/issues/248 -->

A mesma capacidade de utilizar expressões de enlace e padrões se aplica a atributos em bibliotecas de classes. Por exemplo, aqui está uma imagem de redimensionamento função numa biblioteca de classe:

```csharp
[FunctionName("ResizeImage")]
[StorageAccount("AzureWebJobsStorage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-sm/{name}", FileAccess.Write)] Stream imageSmall, 
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageMedium)
{
    var imageBuilder = ImageResizer.ImageBuilder.Current;
    var size = imageDimensionsTable[ImageSize.Small];

    imageBuilder.Build(image, imageSmall,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);

    image.Position = 0;
    size = imageDimensionsTable[ImageSize.Medium];

    imageBuilder.Build(image, imageMedium,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);
}

public enum ImageSize { ExtraSmall, Small, Medium }

private static Dictionary<ImageSize, (int, int)> imageDimensionsTable = new Dictionary<ImageSize, (int, int)>() {
    { ImageSize.ExtraSmall, (320, 200) },
    { ImageSize.Small,      (640, 400) },
    { ImageSize.Medium,     (800, 600) }
};
```

### <a name="create-guids"></a>Criar GUIDs

O `{rand-guid}` expressão de enlace cria um GUID. O exemplo seguinte utiliza um GUID para criar um nome exclusivo de blob: 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a>Hora atual

A expressão de enlace `DateTime` é resolvido para `DateTime.UtcNow`.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-to-custom-input-properties"></a>Vincular a propriedades de entrada personalizadas

Expressões de enlace também podem referenciar propriedades que são definidas no payload do acionador em si. Por exemplo, poderá pretender dinamicamente vincular a um ficheiro de armazenamento de BLOBs a partir de um nome de ficheiro fornecido num webhook.

Por exemplo, o seguinte *function.json* utiliza uma propriedade denominada `BlobName` do payload do acionador:

```json
{
  "bindings": [
    {
      "name": "info",
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "genericJson"
    },
    {
      "name": "blobContents",
      "type": "blob",
      "direction": "in",
      "path": "strings/{BlobName}",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

Para tal em c# e F #, tem de definir um POCO que define os campos que serão possível anular a serialização no payload do acionador.

```csharp
using System.Net;

public class BlobInfo
{
    public string BlobName { get; set; }
}
  
public static HttpResponseMessage Run(HttpRequestMessage req, BlobInfo info, string blobContents)
{
    if (blobContents == null) {
        return req.CreateResponse(HttpStatusCode.NotFound);
    } 

    return req.CreateResponse(HttpStatusCode.OK, new {
        data = $"{blobContents}"
    });
}
```

Em JavaScript, a desserialização de JSON é automaticamente executada e pode utilizar as propriedades diretamente.

```javascript
module.exports = function (context, info) {
    if ('BlobName' in info) {
        context.res = {
            body: { 'data': context.bindings.blobContents }
        }
    }
    else {
        context.res = {
            status: 404
        };
    }
    context.done();
}
```

## <a name="configuring-binding-data-at-runtime"></a>Configuração de enlace de dados em tempo de execução

Em c# e outras linguagens .NET, pode utilizar um padrão de enlace imperativo, por oposição os enlaces declarativos no *function.json* e atributos. Enlace imperativo é útil quando os parâmetros de enlace tem de ser calculada ao tempo de tempo de execução, em vez de design. Para obter mais informações, consulte [enlace no tempo de execução através dos enlaces imperativo](functions-reference-csharp.md#imperative-bindings) a referência de programador do c#.

## <a name="functionjson-file-schema"></a>esquema do ficheiro Function.JSON

O *function.json* esquema do ficheiro está disponível em [http://json.schemastore.org/function](http://json.schemastore.org/function).

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre um enlace específico, consulte os artigos seguintes:

- [HTTP e webhooks](functions-bindings-http-webhook.md)
- [Temporizador](functions-bindings-timer.md)
- [Armazenamento de filas](functions-bindings-storage-queue.md)
- [Armazenamento de blobs](functions-bindings-storage-blob.md)
- [Armazenamento de tabelas](functions-bindings-storage-table.md)
- [Hub de Eventos](functions-bindings-event-hubs.md)
- [Service Bus](functions-bindings-service-bus.md)
- [BD do Cosmos para o Azure](functions-bindings-cosmosdb.md)
- [Microsoft Graph](functions-bindings-microsoft-graph.md)
- [SendGrid](functions-bindings-sendgrid.md)
- [Twilio](functions-bindings-twilio.md)
- [Hubs de Notificação](functions-bindings-notification-hubs.md)
- [Aplicações Móveis](functions-bindings-mobile-apps.md)
- [Ficheiro externo](functions-bindings-external-file.md)
