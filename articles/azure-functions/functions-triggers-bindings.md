---
title: Acionadores e enlaces nas funções do Azure
description: Saiba como utilizar acionadores e enlaces nas funções do Azure para ligar a sua execução de código para eventos online e serviços baseados na nuvem.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: funções do azure, funções, processamento de eventos, webhooks, computação dinâmica, arquitetura sem servidor
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 09/24/2018
ms.author: cshoe
ms.openlocfilehash: 5e529953d06c37f382bdd5e65c23fe16d9928008
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50250908"
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Acionadores de funções do Azure e conceitos de enlaces

Este artigo é uma descrição geral conceptual dos acionadores e enlaces nas funções do Azure. Recursos que são comuns a todas as ligações e todos os idiomas suportados são descritos aqui.

## <a name="overview"></a>Descrição geral

R *acionador* define como uma função é invocada. Uma função tem de ter exatamente um acionador. Os acionadores têm dados associados, que é normalmente o payload que acionou a função.

Entrada e saída *enlaces* proporcionam uma forma declarativa para ligar a dados a partir de seu código. Enlaces são opcionais e uma função pode ter várias entradas de ligações de saída. 

Acionadores e enlaces permitem-lhe evitar os detalhes dos serviços que está trabalhando com a codificar. A função recebe dados (por exemplo, o conteúdo de uma mensagem de fila) nos parâmetros de função. Enviar dados (por exemplo, para criar uma mensagem de fila), utilizando o valor de retorno da função. No c# e de script c#, maneiras alternativas de enviar os dados são `out` parâmetros e [objetos de coletor](functions-reference-csharp.md#writing-multiple-output-values).

Ao desenvolver funções com o portal do Azure, os acionadores e enlaces estão configurados num *Function* ficheiro. O portal fornece uma interface do Usuário para esta configuração, mas pode editar o ficheiro diretamente ao alterar para o **editor avançado**.

Ao desenvolver funções utilizando o Visual Studio para criar uma biblioteca de classes, configurar acionadores e enlaces de decoração de métodos e parâmetros com atributos.

## <a name="example-trigger-and-binding"></a>Acionador de exemplo e ligação

Suponha que queira escrever uma nova linha para o armazenamento de tabelas do Azure, sempre que uma nova mensagem aparece no armazenamento de filas do Azure. Este cenário pode ser implementado usando uma fila do Azure enlace de saída de Acionador do armazenamento e um armazenamento de tabelas do Azure. 

Aqui está uma *Function* ficheiros para este cenário. 

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

O primeiro elemento no `bindings` matriz é o acionador do armazenamento de fila. O `type` e `direction` propriedades identificar o acionador. O `name` propriedade identifica o parâmetro de função que recebe o conteúdo da mensagem de fila. O nome da fila para monitorizar está `queueName`, e a cadeia de ligação é na definição da aplicação identificada pelo `connection`.

O segundo elemento no `bindings` matriz é o armazenamento de tabelas do Azure enlace de saída. O `type` e `direction` propriedades identificar o enlace. O `name` propriedade especifica como a função fornece a nova linha de tabela, neste caso, utilizando a função devolver o valor. O nome da tabela está `tableName`, e a cadeia de ligação é na definição da aplicação identificada pelo `connection`.

Para ver e editar o conteúdo de *Function* no portal do Azure, clique nas **editor avançado** opção os **integrar** separador da sua função.

> [!NOTE]
> O valor de `connection` é o nome de uma definição de aplicação que contém a cadeia de ligação, não a cadeia de ligação em si. Enlaces de ligação de utilizar cadeias de caracteres nas definições da aplicação para impor a melhor prática que *Function* não contém segredos de serviço.

Aqui está o script código c# que funciona com este acionador e a associação. Tenha em atenção que o nome do parâmetro que fornece o conteúdo da mensagem de fila é `order`; este nome é necessário porque o `name` valor da propriedade nas *Function* é `order` 

```cs
#r "Newtonsoft.Json"

using Microsoft.Extensions.Logging;
using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table storage
// The method return value creates a new row in Table Storage
public static Person Run(JObject order, ILogger log)
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

O mesmo arquivo de Function pode ser utilizado com uma função de JavaScript:

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

Numa biblioteca de classes, o mesmo acionador e informações de enlace &mdash; parâmetros de entrada e saída de função de nomes de tabela e fila, contas de armazenamento, &mdash; é fornecido por atributos em vez de um ficheiro de Function. Segue-se um exemplo:

```csharp
 public static class QueueTriggerTableOutput
 {
     [FunctionName("QueueTriggerTableOutput")]
     [return: Table("outTable", Connection = "MY_TABLE_STORAGE_ACCT_APP_SETTING")]
     public static Person Run(
         [QueueTrigger("myqueue-items", Connection = "MY_STORAGE_ACCT_APP_SETTING")]JObject order, 
         ILogger log)
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

## <a name="supported-bindings"></a>Enlaces suportados

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Para obter informações sobre as ligações estão em pré-visualização ou são aprovadas para utilização em produção, consulte [idiomas suportados](supported-languages.md).

## <a name="register-binding-extensions"></a>Registe-se as extensões de enlace

Em alguns ambientes de desenvolvimento, precisa explicitamente *registar* uma ligação que pretende utilizar. Extensões de ligação são fornecidas nos pacotes NuGet e para registar uma extensão, instale um pacote. A tabela seguinte indica quando e como registar as extensões de enlace.

|Ambiente de programação |Registo<br/> nas funções 1.x  |Registo<br/> nas funções 2.x  |
|---------|---------|---------|
|Portal do Azure|Automático|[Automático com linha de comandos](#azure-portal-development)|
|Local usando as ferramentas de núcleo das funções do Azure|Automático|[Utilizar comandos da CLI de ferramentas de núcleo](#local-development-azure-functions-core-tools)|
|Biblioteca de classes do c# com o Visual Studio 2017|[Utilizar as ferramentas do NuGet](#c-class-library-with-visual-studio-2017)|[Utilizar as ferramentas do NuGet](#c-class-library-with-visual-studio-2017)|
|Biblioteca de classes do c# com o Visual Studio Code|N/A|[Utilizar o .NET Core CLI](#c-class-library-with-visual-studio-code)|

Os seguintes tipos de ligação são exceções que não exigem Registro explícito, porque eles são automaticamente registrados em todas as versões e ambientes: HTTP e temporizador.

### <a name="azure-portal-development"></a>Desenvolvimento de portal do Azure

Esta secção aplica-se apenas às funções 2.x. Extensões de enlace não tem de ser explicitamente registado nas funções 1.x.

Quando criar uma função ou adicionar um enlace, é solicitado quando a extensão para o acionador ou enlace requer o registo. Responder à solicitação clicando **instalar** para registar a extensão. A instalação pode demorar até 10 minutos num plano de consumo.

Precisa apenas instalar cada extensão de uma vez para uma aplicação de função especificada. Para ligações suportadas que não estão disponíveis no portal ou para atualizar a uma extensão instalada, também pode [manualmente instalar ou atualizar as funções do Azure enlace extensões a partir do portal do](install-update-binding-extensions-manual.md).  

### <a name="local-development-azure-functions-core-tools"></a>Local de desenvolvimento de ferramentas de núcleo de funções do Azure

Esta secção aplica-se apenas às funções 2.x. Extensões de enlace não tem de ser explicitamente registado nas funções 1.x.

[!INCLUDE [functions-core-tools-install-extension](../../includes/functions-core-tools-install-extension.md)]

<a name="local-csharp"></a>
### <a name="c-class-library-with-visual-studio-2017"></a>Biblioteca de classes c# com o Visual Studio 2017

Na **Visual Studio 2017**, pode instalar pacotes a partir da consola do Gestor de pacotes utilizando o [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) de comando, conforme mostrado no exemplo a seguir:

```powershell
Install-Package Microsoft.Azure.WebJobs.ServiceBus --Version <target_version>
```

O nome do pacote para utilizar para uma determinada associação é fornecido no artigo de referência para esse enlace. Por exemplo, veja a [secção do artigo de referência de ligação do Service Bus de pacotes](functions-bindings-service-bus.md#packages---functions-1x).

Substitua `<target_version>` no exemplo com uma versão específica do pacote, tais como `3.0.0-beta5`. Versões válidas estão listadas nas páginas individuais do pacote na [NuGet.org](https://nuget.org). As versões principais que correspondem ao runtime das funções 1.x ou 2.x são especificados no artigo de referência para o enlace.

### <a name="c-class-library-with-visual-studio-code"></a>Biblioteca de classes c# com o Visual Studio Code

Na **Visual Studio Code**, pode instalar pacotes a partir da linha de comandos com o [dotnet Adicionar pacote](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) comando na CLI do núcleo do .NET, conforme mostrado no exemplo a seguir:

```terminal
dotnet add package Microsoft.Azure.WebJobs.ServiceBus --version <target_version>
```

Só pode ser utilizada a CLI do .NET Core para o desenvolvimento de 2.x as funções do Azure.

O nome do pacote para utilizar para uma determinada associação é fornecido no artigo de referência para esse enlace. Por exemplo, veja a [secção do artigo de referência de ligação do Service Bus de pacotes](functions-bindings-service-bus.md#packages---functions-1x).

Substitua `<target_version>` no exemplo com uma versão específica do pacote, tais como `3.0.0-beta5`. Versões válidas estão listadas nas páginas individuais do pacote na [NuGet.org](https://nuget.org). As versões principais que correspondem ao runtime das funções 1.x ou 2.x são especificados no artigo de referência para o enlace.

## <a name="binding-direction"></a>Direção de ligação

Todos os acionadores e enlaces de tem uma `direction` propriedade no *Function* ficheiro:

- Para acionadores, a direção é sempre `in`
- Ligações de entrada e saídas usam `in` e `out`
- Algumas ligações oferecem suporte a uma direção especial `inout`. Se usar `inout`, apenas o **editor avançado** está disponível na **integrar** separador.

Quando utiliza [atributos numa biblioteca de classe](functions-dotnet-class-library.md) para configurar acionadores e enlaces, a direção é fornecida no construtor de atributos ou inferida do tipo de parâmetro.

## <a name="using-the-function-return-value"></a>Utilizar o valor de retorno da função

Em idiomas que têm um valor de retorno, é possível vincular um enlace de saída para o valor de retorno:

* No c# biblioteca de classes, aplica o atributo de enlace de saída para o valor de retorno do método.
* Em outros idiomas, defina o `name` propriedade na *Function* para `$return`.

Se existirem vários enlaces de saída, utilize o valor de retorno para apenas um deles.

No c# e de script c#, maneiras alternativas de enviar dados para um enlace de saída são `out` parâmetros e [objetos de coletor](functions-reference-csharp.md#writing-multiple-output-values).

Veja o exemplo de idioma específico, que mostra a utilização de valor de retorno:

* [C#](#c-example)
* [Script do c# (.csx)](#c-script-example)
* [F#](#f-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>Exemplo do c#

O código c# aqui está que utiliza o valor de retorno para um enlace de saída, seguido de um exemplo de async:

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static string Run([QueueTrigger("inputqueue")]WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static Task<string> Run([QueueTrigger("inputqueue")]WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

### <a name="c-script-example"></a>Exemplo de script do c#

Este é o enlace de saída no *Function* ficheiro:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Aqui está o script código c#, seguido de um exemplo de async:

```cs
public static string Run(WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
public static Task<string> Run(WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

### <a name="f-example"></a>Exemplo do F #

Este é o enlace de saída no *Function* ficheiro:

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
let Run(input: WorkItem, log: ILogger) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.LogInformation(sprintf "F# script processed queue message '%s'" json)
    json
```

### <a name="javascript-example"></a>Exemplo de JavaScript

Este é o enlace de saída no *Function* ficheiro:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

No JavaScript, o valor de retorno vai no segundo parâmetro para `context.done`:

```javascript
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

## <a name="binding-datatype-property"></a>Propriedade de tipo de dados de ligação

No .NET, utilize o tipo de parâmetro para definir o tipo de dados para dados de entrada. Por exemplo, usar `string` para vincular ao texto de um acionador de fila, uma matriz de bytes para leitura como binário e um tipo personalizado para anular a serialização para um objeto POCO.

Para idiomas que são digitados dinamicamente, como JavaScript, utilize o `dataType` propriedade no *Function* ficheiro. Por exemplo, para ler o conteúdo de uma solicitação HTTP no formato binário, defina `dataType` para `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Outras opções para `dataType` estão `stream` e `string`.

## <a name="binding-expressions-and-patterns"></a>Expressões de associação e padrões

Um dos recursos mais poderosos de acionadores e enlaces é *expressões de enlace*. Na *Function* de ficheiros e nos parâmetros da função e código, pode usar expressões que resolva para valores de várias origens.

A maioria das expressões são identificadas, encapsulando-as chavetas. Por exemplo, numa função de Acionador de fila, `{queueTrigger}` é resolvido para o texto da mensagem de fila. Se o `path` propriedade para um blob de saída é de enlace `container/{queueTrigger}` e a função é acionada por uma mensagem de fila `HelloWorld`, um blob com o nome `HelloWorld` é criado.

Tipos de expressões de associação

* [Definições da aplicação](#binding-expressions---app-settings)
* [Nome de ficheiro de Acionador](#binding-expressions---trigger-file-name)
* [Metadados de Acionador](#binding-expressions---trigger-metadata)
* [Conteúdo JSON](#binding-expressions---json-payloads)
* [Novo GUID](#binding-expressions---create-guids)
* [Data e hora atuais](#binding-expressions---current-time)

### <a name="binding-expressions---app-settings"></a>Expressões de enlace - definições da aplicação

Como melhor prática, segredos e as cadeias de ligação devem ser geridas utilizando as definições da aplicação, em vez de arquivos de configuração. Isso limita o acesso a estes segredos e torna segura armazenar os ficheiros, tal como *Function* nos repositórios de controlo de origem pública.

Definições da aplicação também são úteis sempre que quiser alterar a configuração com base no ambiente. Por exemplo, num ambiente de teste, pode querer monitorizar um contentor de armazenamento de BLOBs ou filas diferente.

Expressões de associação de definição de aplicação são identificadas de forma diferente de outras expressões de enlace: eles são encapsulados em percentagem, em vez de chavetas. Por exemplo, se o caminho de enlace de saída do blob for `%Environment%/newblob.txt` e o `Environment` é o valor de definição de aplicação `Development`, será criado um blob a `Development` contentor.

Quando uma função é executada localmente, valores de definição de aplicação são provenientes do *Settings* ficheiro.

Tenha em atenção que o `connection` é um caso especial de propriedade de acionadores e enlaces e resolve automaticamente os valores como definições da aplicação, sem percentagem. 

O exemplo seguinte é um acionador do armazenamento de filas do Azure que utiliza uma definição de aplicação `%input-queue-name%` para definir a fila para acionar.

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

Pode utilizar a mesma abordagem nas bibliotecas de classes:

```csharp
[FunctionName("QueueTrigger")]
public static void Run(
    [QueueTrigger("%input-queue-name%")]string myQueueItem, 
    ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
}
```

### <a name="binding-expressions---trigger-file-name"></a>Expressões de enlace - nome do ficheiro de Acionador

O `path` para um Blob acionador pode ser um padrão que permite que fizer referência ao nome do blob acionadora em outros enlaces e código de função. O padrão também pode incluir os critérios de filtragem que especificam qual blobs, podem acionar uma invocação de função.

Por exemplo, no acionador de Blob seguinte ligação, o `path` padrão é `sample-images/{filename}`, que cria uma expressão de vinculação com o nome `filename`:

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

A expressão `filename` , em seguida, pode ser utilizado num enlace de saída para especificar o nome do blob que está sendo criado:

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
public static void Run(Stream image, string filename, Stream imageSmall, ILogger log)  
{
    log.LogInformation($"Blob trigger processing: {filename}");
    // ...
} 
```

<!--TODO: add JavaScript example -->
<!-- Blocked by bug https://github.com/Azure/Azure-Functions/issues/248 -->

A mesma capacidade de utilizar expressões de associação e padrões de se aplica a atributos nas bibliotecas de classes. No exemplo a seguir, os parâmetros do construtor de atributo são os mesmos `path` valores como precedente *Function* exemplos: 

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{filename}")] Stream image,
    [Blob("sample-images-sm/{filename}", FileAccess.Write)] Stream imageSmall,
    string filename,
    ILogger log)
{
    log.LogInformation($"Blob trigger processing: {filename}");
    // ...
}

```

Também pode criar expressões para partes do nome do ficheiro como a extensão. Para obter mais informações sobre como utilizar expressões e padrões na cadeia de caminho de Blob, consulte a [referência de ligação de blob de armazenamento](functions-bindings-storage-blob.md).
 
### <a name="binding-expressions---trigger-metadata"></a>Expressões de enlace - metadados de Acionador

O payload de dados fornecido por um acionador (por exemplo, o conteúdo da mensagem de fila que uma função acionada por um), além de muitos acionadores fornecem os valores de metadados adicionais. Estes valores podem ser utilizados como parâmetros de entrada em c# e F # ou propriedades no `context.bindings` objetos em JavaScript. 

Por exemplo, um acionador do armazenamento de filas do Azure suporta as seguintes propriedades:

* QueueTrigger - conteúdo da mensagem a acionar se uma cadeia válida
* DequeueCount
* expirationTime
* Id
* InsertionTime
* NextVisibleTime
* PopReceipt

Estes valores de metadados são acessíveis no *Function* propriedades do ficheiro. Por exemplo, suponha que usar um acionador de fila e a mensagem de fila contém o nome de um blob que pretende ler. Na *Function* arquivo, pode usar `queueTrigger` propriedade de metadados no blob `path` propriedade, conforme mostrado no exemplo a seguir:

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

Detalhes das propriedades de metadados para cada acionador são descritos no artigo de referência correspondente. Por exemplo, veja [metadados de Acionador de fila](functions-bindings-storage-queue.md#trigger---message-metadata). Documentação também está disponível na **integrar** separador do portal, na **documentação** seção a seguir a área de configuração de ligação.  

### <a name="binding-expressions---json-payloads"></a>Expressões de enlace - conteúdo JSON

Quando um payload de Acionador é um JSON, pode consultar as respetivas propriedades na configuração para outros enlaces na mesma função e no código de função.

A exemplo a seguir mostra a *Function* ficheiro para uma função de webhook que recebe um nome de blob em JSON: `{"BlobName":"HelloWorld.txt"}`. Um enlace de entrada de BLOBs lê o blob e o HTTP enlace devolve os conteúdos do blob na resposta HTTP de saída. Tenha em atenção que o enlace de entrada do BLOBs obtém o nome de blob por consultar diretamente para o `BlobName` propriedade (`"path": "strings/{BlobName}"`)

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

Para isso funcionar em c# e F #, precisa de uma classe que define os campos a ser desserializado, como no exemplo seguinte:

```csharp
using System.Net;
using Microsoft.Extensions.Logging;

public class BlobInfo
{
    public string BlobName { get; set; }
}
  
public static HttpResponseMessage Run(HttpRequestMessage req, BlobInfo info, string blobContents, ILogger log)
{
    if (blobContents == null) {
        return req.CreateResponse(HttpStatusCode.NotFound);
    } 

    log.LogInformation($"Processing: {info.BlobName}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        data = $"{blobContents}"
    });
}
```

No JavaScript, desserialização de JSON é executada automaticamente.

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

#### <a name="dot-notation"></a>Notação de ponto

Se algumas das propriedades no payload JSON são objetos com propriedades, pode consultar às diretamente, usando a notação de ponto. Por exemplo, suponha que o seu JSON é semelhante a este:

```json
{"BlobName": {
  "FileName":"HelloWorld",
  "Extension":"txt"
  }
}
```

Pode consultar diretamente à `FileName` como `BlobName.FileName`. Com esse formato JSON, eis o que o `path` propriedade no exemplo anterior teria o seguinte aspeto:

```json
"path": "strings/{BlobName.FileName}.{BlobName.Extension}",
```

No c#, precisaria duas classes:

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

O `{rand-guid}` enlace expressão cria um GUID. O seguinte caminho de blob num `function.json` arquivo cria um blob com um nome como *50710cb5-84b9 - 4d 87 9d 83-a03d6976a682.txt*.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="binding-expressions---current-time"></a>Expressões de enlace - hora atual

A expressão de vinculação `DateTime` é resolvido para `DateTime.UtcNow`. O seguinte caminho de blob num `function.json` arquivo cria um blob com um nome como *2018-02-16T17-59-55Z.txt*.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="binding-at-runtime"></a>Ligação em tempo de execução

No c# e de outras linguagens .NET, pode usar um padrão de enlace imperativa, em oposição as ligações declarativas na *Function* e atributos. Ligação de imperativa é útil quando os parâmetros de ligação tem de ser computado em tempo de tempo de execução, em vez de design. Para obter mais informações, consulte a [referência para programadores sobre c#](functions-dotnet-class-library.md#binding-at-runtime) ou o [referência para programadores script c#](functions-reference-csharp.md#binding-at-runtime).

## <a name="functionjson-file-schema"></a>esquema de ficheiro Function

O *Function* esquema de ficheiro está disponível no [ http://json.schemastore.org/function ](http://json.schemastore.org/function).

## <a name="handling-binding-errors"></a>Tratamento de erros de ligação

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

Para obter ligações para todos os tópicos de erro relevantes para os vários serviços suportados por funções, consulte a [códigos de erro de ligação](functions-bindings-error-pages.md#binding-error-codes) secção a [tratamento de erros de funções do Azure](functions-bindings-error-pages.md) tópico de descrição geral.  

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre uma ligação específica, consulte os artigos seguintes:

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
