---
title: Acionadores e enlaces de funções do Azure
description: Saiba como utilizar acionadores e enlaces das funções do Azure para ligar a execução do código para serviços baseados na nuvem e eventos online.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: funções do azure, funções, processamento de eventos, webhooks, computação dinâmica, arquitetura sem servidor
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/07/2018
ms.author: glenga
ms.openlocfilehash: d008e94186dd3c5a18c92617ebddfffa966358d5
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
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

## <a name="register-binding-extensions"></a>Registar as extensões de enlace

Versão 2 do tempo de execução das funções do Azure, tem explicitamente de registar o [enlace extensões](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/README.md) que utilizar na sua aplicação de função. 

As extensões são fornecidas como pacotes NuGet, onde o nome do pacote normalmente começa com [microsoft.azure.webjobs.extensions](https://www.nuget.org/packages?q=microsoft.azure.webjobs.extensions).  A forma como instalar e registar as extensões de enlace depende de como desenvolver as suas funções: 

+ [Localmente em c# utilizando o Visual Studio ou o VS Code](#local-c-development-using-visual-studio-or-vs-code)
+ [Localmente utilizando ferramentas de núcleos de funções do Azure](#local-development-azure-functions-core-tools)
+ [No portal do Azure](#azure-portal-development) 

Há um conjunto de núcleos de enlaces no versão 2. x que não são fornecidos como extensões. Não é necessário registar extensões para os seguintes acionadores e enlaces: HTTP, temporizador e armazenamento do Azure. 

Para obter informações sobre como definir uma aplicação de função para utilizar a versão 2 do tempo de execução funções, consulte [como destino a versões de tempo de execução das funções do Azure](set-runtime-version.md). Versão 2 do tempo de execução de funções está atualmente em pré-visualização. 

As versões do pacote apresentadas nesta secção são fornecidas apenas como exemplos. Verifique o [NuGet.org site](https://www.nuget.org/packages?q=microsoft.azure.webjobs.extensions) para determinar qual é a versão de uma determinada extensão são necessárias para as outras dependências na sua aplicação de função.    

###  <a name="local-c-development-using-visual-studio-or-vs-code"></a>C# desenvolvimento local utilizando o Visual Studio ou o VS Code 

Quando utilizar o Visual Studio ou Visual Studio Code para desenvolver localmente as funções em c#, basta terá de adicionar o pacote NuGet para a extensão. 

+ **Visual Studio**: utilizar as ferramentas do Gestor de pacotes NuGet. O seguinte [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) comando instala a extensão de BD do Cosmos Azure a partir da consola do Gestor de pacotes:

    ```
    Install-Package Microsoft.Azure.WebJobs.Extensions.CosmosDB -Version 3.0.0-beta6 
    ```
+ **Visual Studio Code**: poderá instalar pacotes a partir da linha de comandos utilizando a [dotnet Adicionar pacote](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) comando na .NET CLI, da seguinte forma:

    ```
    dotnet add package Microsoft.Azure.WebJobs.Extensions.CosmosDB --version 3.0.0-beta6 
    ```

### <a name="local-development-azure-functions-core-tools"></a>Azure funções principais ferramentas de desenvolvimento local

[!INCLUDE [Full bindings table](../../includes/functions-core-tools-install-extension.md)]

### <a name="azure-portal-development"></a>Desenvolvimento de portal do Azure

Quando criar uma função ou adicionar um enlace a uma função existente, serão apresentadas quando a extensão para o enlace a ser adicionado ou acionador necessita de registo.   

Depois de aparece um aviso para a extensão específica que está a ser instalada, clique em **instalar** para registar a extensão. Só é necessário instalar cada extensão de uma vez para uma aplicação de função especificada. 

>[!Note] 
>O processo de instalação no portal pode demorar até 10 minutos num plano de consumo.

## <a name="example-trigger-and-binding"></a>Acionador de exemplo e enlace

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

O primeiro elemento no `bindings` matriz é o acionador de armazenamento de filas. O `type` e `direction` propriedades identificam o acionador. O `name` propriedade identifica o parâmetro de função que recebe o conteúdo da mensagem de fila. O nome da fila para monitorizar está a ser `queueName`, e a cadeia de ligação tem a definição de aplicação identificada por `connection`.

O elemento de segundo a `bindings` matriz é o Table Storage do Azure vínculo de saída. O `type` e `direction` propriedades identificam o enlace. O `name` propriedade especifica a forma como a função fornece a nova linha de tabela, neste caso, utilizando a função devolve o valor. O nome da tabela está a ser `tableName`, e a cadeia de ligação tem a definição de aplicação identificada por `connection`.

Para ver e editar o conteúdo do *function.json* no portal do Azure, clique em de **editor avançada** opção o **integrar** separador da sua função.

> [!NOTE]
> O valor de `connection` é o nome de uma definição de aplicação que contenha a cadeia de ligação, não a cadeia de ligação em si. Enlaces utilizam ligação cadeias armazenadas nas definições de aplicação para impor as melhores práticas que *function.json* não contém segredos do serviço.

Eis o script código c# que funciona com este acionador e o enlace. Tenha em atenção que o nome do parâmetro que fornece o conteúdo da mensagem de fila é `order`; este nome é necessário porque o `name` valor da propriedade no *function.json* é `order` 

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

Na biblioteca de classes, o mesmo acionador e informações de enlace &mdash; parâmetros de entrada e saída da função de nomes de filas e tabela, contas de armazenamento, &mdash; é fornecido por atributos em vez de um ficheiro de function.json. Segue-se um exemplo:

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

- Para acionadores, a direção é sempre `in`
- Utilizam enlaces de entrada e de saída `in` e `out`
- Alguns enlaces suportam uma direção especial `inout`. Se utilizar `inout`, apenas o **editor avançada** está disponível no **integrar** separador.

Quando utiliza [atributos na biblioteca de classes](functions-dotnet-class-library.md) para configurar os acionadores e enlaces, a direção é fornecida no construtor de atributos ou inferida a partir do tipo de parâmetro.

## <a name="using-the-function-return-value"></a>Utilizar o valor de retorno da função

Os idiomas que tenham um valor de retorno, é possível vincular um enlace de saída para o valor devolvido:

* Em c# biblioteca de classes, aplique o atributo de enlace de saída para o valor de retorno do método.
* Outros idiomas, defina o `name` propriedade no *function.json* para `$return`.

Se precisar de mais de um item de escrita, utilize um [objeto recoletor](functions-reference-csharp.md#writing-multiple-output-values) em vez do valor de retorno. Se existirem vários enlaces de resultados, utilize o valor de retorno para apenas um deles.

Veja o exemplo de específicas do idioma:

* [C#](#c-example)
* [Script do c# (.csx)](#c-script-example)
* [F#](#f-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>Exemplo do c#

Código c# Eis que utiliza o valor de retorno para um enlace de saída, seguido de um exemplo de async:

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static string Run([QueueTrigger("inputqueue")]WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static Task<string> Run([QueueTrigger("inputqueue")]WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

### <a name="c-script-example"></a>Exemplo de script do c#

Eis o enlace de saída *function.json* ficheiro:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Eis o script código c#, seguido de um exemplo de async:

```cs
public static string Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
public static Task<string> Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

### <a name="f-example"></a>O F # exemplo

Eis o enlace de saída *function.json* ficheiro:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Eis o código F #:

```fsharp
let Run(input: WorkItem, log: TraceWriter) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.Info(sprintf "F# script processed queue message '%s'" json)
    json
```

### <a name="javascript-example"></a>Exemplo de JavaScript

Eis o enlace de saída *function.json* ficheiro:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Em JavaScript, o valor de retorno entra no segundo parâmetro para `context.done`:

```javascript
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
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

## <a name="binding-expressions-and-patterns"></a>Expressões de enlace e padrões

Uma das funcionalidades mais poderosas de acionadores e enlaces é *expressões de enlace*. No *function.json* ficheiros e de parâmetros de função e código, pode utilizar expressões de resolver para valores de várias origens.

Maior parte das expressões são identificados pelo encapsulamento-los chavetas. Por exemplo, numa função de Acionador de fila, `{queueTrigger}` é resolvido para o texto da mensagem de fila. Se o `path` enlace é de saída de propriedade para um blob `container/{queueTrigger}` e a função é acionada por uma mensagem de fila `HelloWorld`, um blob com o nome `HelloWorld` é criado.

Tipos de expressões de enlace

* [Definições de aplicação](#binding-expressions---app-settings)
* [Nome de ficheiro do acionador](#binding-expressions---trigger-file-name)
* [Metadados de Acionador](#binding-expressions---trigger-metadata)
* [JSON payloads](#binding-expressions---json-payloads)
* [Novo GUID](#binding-expressions---create-guids)
* [Data e hora atuais](#binding-expressions---current-time)

### <a name="binding-expressions---app-settings"></a>Expressões de enlace - as definições de aplicação

Como melhor prática, os segredos e cadeias de ligação devem ser geridas utilizando as definições de aplicação, em vez de ficheiros de configuração. Isto limita o acesso a estes segredos e torna seguro armazenar os ficheiros, tais como *function.json* em repositórios de controlo de origem público.

As definições de aplicações também são úteis sempre que pretender alterar a configuração com base no ambiente. Por exemplo, num ambiente de teste, pode querer monitorizar um contentor de armazenamento de fila ou blob diferente.

Expressões de enlace de definição de aplicação são identificadas de forma diferente de outras expressões de enlace: estes são moldadas numa percentagem sinais em vez de chavetas. Por exemplo, se o caminho de enlace de saída do blob é `%Environment%/newblob.txt` e `Environment` valor de definição de aplicação é `Development`, será criado um blob no `Development` contentor.

Quando uma função é executada localmente, os valores de definição de aplicação provenientes de *local.settings.json* ficheiro.

Tenha em atenção que o `connection` propriedade de acionadores e enlaces é num caso especial e resolve automaticamente os valores das definições de aplicação, sem sinais por cento. 

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

### <a name="binding-expressions---trigger-file-name"></a>Expressões de enlace - nome de ficheiro do acionador

O `path` para um Blob acionador pode ser um padrão que permite-lhe fazer referência ao nome do blob acionadora noutros enlaces e código de função. O padrão também pode incluir os critérios de filtragem que especifique que os blobs podem acionar uma invocação de função.

Por exemplo, no acionador Blob seguinte enlace, o `path` padrão é `sample-images/{filename}`, que cria uma expressão de enlace com o nome `filename`:

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
    ...
```

A expressão `filename` , em seguida, pode ser utilizado num enlace de saída para especificar o nome do blob a ser criado:

```json
    ...
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

Código de função tem acesso a este valor mesmo utilizando `filename` como um nome de parâmetro:

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

A mesma capacidade de utilizar expressões de enlace e padrões se aplica a atributos em bibliotecas de classes. No exemplo seguinte, os parâmetros do construtor de atributo são os mesmos `path` valores como precedente *function.json* exemplos: 

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{filename}")] Stream image,
    [Blob("sample-images-sm/{filename}", FileAccess.Write)] Stream imageSmall,
    string filename,
    TraceWriter log)
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
}

```

Também pode criar as expressões para partes do nome do ficheiro, como a extensão. Para obter mais informações sobre como utilizar expressões e padrões na cadeia de caminho de Blob, consulte o [referência de enlace de blob de armazenamento](functions-bindings-storage-blob.md).
 
### <a name="binding-expressions---trigger-metadata"></a>Expressões de enlace - acionar metadados

Para além do payload de dados fornecido por um acionador (por exemplo, o conteúdo da mensagem de fila que acionou uma função), acionadores muitos forneça valores de metadados adicionais. Estes valores podem ser utilizados como parâmetros de entrada em c# e F # ou propriedades no `context.bindings` objeto em JavaScript. 

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

### <a name="binding-expressions---json-payloads"></a>Expressões de enlace - JSON payloads

Quando um payload de Acionador é JSON, pode consultar as respetivas propriedades na configuração para noutros enlaces na mesma função e no código da função.

O seguinte exemplo mostra o *function.json* ficheiro para uma função de webhook que recebe um nome do blob no JSON: `{"BlobName":"HelloWorld.txt"}`. Um enlace de entrada do Blob lê o blob e o HTTP de saída do enlace devolve os conteúdos do blob na resposta HTTP. Tenha em atenção que o enlace de entrada do Blob obtém o nome do blob ao referir-se diretamente para o `BlobName` propriedade (`"path": "strings/{BlobName}"`)

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
      "path": "strings/{BlobName.FileName}.{BlobName.Extension}",
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

Para isto funcionar em c# e F #, precisa de uma classe que define os campos para anular a serialização, como no exemplo seguinte:

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

Em JavaScript, a desserialização de JSON é automaticamente executada.

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

#### <a name="dot-notation"></a>Notação de pontos

Se algumas das propriedades do payload JSON são objetos com propriedades, pode fazer referência aos diretamente, utilizando a notação de pontos. Por exemplo, suponha que o JSON tem o seguinte aspeto:

```json
{"BlobName": {
  "FileName":"HelloWorld",
  "Extension":"txt"
  }
}
```

Pode consultar diretamente para `FileName` como `BlobName.FileName`. Com este formato JSON, eis o que o `path` propriedade no exemplo anterior deverá ter o seguinte aspeto:

```json
"path": "strings/{BlobName.FileName}.{BlobName.Extension}",
```

Em c#, terá de duas classes:

```csharp
public class BlobInfo
{
    public BlobName BlobName { get; set; }
}
public class BlobName
{
    public string FileName { get; set; }
    public string Extension { get; set; }
}
```

### <a name="binding-expressions---create-guids"></a>Expressões de enlace - criar GUIDs

O `{rand-guid}` expressão de enlace cria um GUID. O seguinte caminho de blob num `function.json` ficheiro cria um blob com o nome como *50710cb5-84b9 - 4d 87 9d 83-a03d6976a682.txt*.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="binding-expressions---current-time"></a>Expressões de enlace - hora atual

A expressão de enlace `DateTime` é resolvido para `DateTime.UtcNow`. O seguinte caminho de blob num `function.json` ficheiro cria um blob com o nome como *2018-02-16T17-59-55Z.txt*.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="binding-at-runtime"></a>Enlace no tempo de execução

Em c# e outras linguagens .NET, pode utilizar um padrão de enlace imperativo, por oposição os enlaces declarativos no *function.json* e atributos. Enlace imperativo é útil quando os parâmetros de enlace tem de ser calculada ao tempo de tempo de execução, em vez de design. Para obter mais informações, consulte o [referência de programador do c#](functions-dotnet-class-library.md#binding-at-runtime) ou [referência para programadores script c#](functions-reference-csharp.md#binding-at-runtime).

## <a name="functionjson-file-schema"></a>esquema do ficheiro Function.JSON

O *function.json* esquema do ficheiro está disponível em [ http://json.schemastore.org/function ](http://json.schemastore.org/function).

## <a name="handling-binding-errors"></a>Processamento de erros de enlace

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

Ligações para todos os tópicos de erro relevantes para os serviços de suportado pelas funções, consulte o [códigos de erro de enlace](functions-bindings-error-pages.md#binding-error-codes) secção o [processamento de erros de funções do Azure](functions-bindings-error-pages.md) tópico de descrição geral.  

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
