---
title: "Referência do programador do Azure funções c# script"
description: "Compreenda como desenvolver as funções do Azure utilizando o script do c#."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "funções do azure, funções, processamento de eventos, webhooks, computação dinâmica, arquitetura sem servidor"
ms.service: functions
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/12/2017
ms.author: glenga
ms.openlocfilehash: b6b18f79b0ef50c30335218ef45ba6ed932cb586
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="azure-functions-c-script-csx-developer-reference"></a>Referência do programador do Azure funções c# script (.csx)

<!-- When updating this article, make corresponding changes to any duplicate content in functions-dotnet-class-library.md -->

Este artigo é uma introdução para desenvolver as funções do Azure utilizando o script do c# (*.csx*).

As funções do Azure suporta c# e c# script linguagens de programação. Se estiver à procura de documentação de orientação [utilizar c# num projeto de biblioteca de classe do Visual Studio](functions-develop-vs.md), consulte [referência de programador do c#](functions-dotnet-class-library.md).

Este artigo pressupõe que já leu o [guia de programadores das funções do Azure](functions-reference.md).

## <a name="how-csx-works"></a>Como funciona o .csx

A experiência de script do c# para as funções do Azure baseia-se no [SDK de WebJobs do Azure](https://github.com/Azure/azure-webjobs-sdk/wiki/Introduction). Fluxos de dados para a sua função de c# através de argumentos de método. Argumento nomes estão especificados num `function.json` de ficheiros e estão predefinidas nomes para aceder ao coisas como os tokens de registo e o cancelamento da função.

O *.csx* formato permite-lhe escrever menos "automático" e concentrar-se na escrita apenas uma função de c#. Em vez de encapsulamento de aplicações tudo numa classe e espaço de nomes, basta definir um `Run` método. Inclua quaisquer referências de assemblagem e espaços de nomes no início do ficheiro como habitualmente.

Uma aplicação de função *.csx* ficheiros são compilados Quando uma instância é inicializada. Este passo de compilação significa coisas como frio poderá demorar mais tempo para funções de script do c# em comparação comparadas bibliotecas de classes do c#. Este passo de compilação é também por que motivo funções de script do c# são editáveis no Portal do Azure, enquanto as bibliotecas de classes c# não são.

## <a name="binding-to-arguments"></a>A associação à argumentos

Dados de entrada ou saídos estão vinculados a um c# script parâmetro de função através de `name` propriedade no *function.json* ficheiro de configuração. O seguinte exemplo mostra um *function.json* ficheiro e *run.csx* ficheiro para uma função acionada pela fila. O parâmetro que recebe dados da mensagem de fila é denominado `myQueueItem` uma vez que é o valor da `name` propriedade.

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

```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem.AsString}");
}
```

O `#r` instrução é explicada [posteriormente neste artigo](#referencing-external-assemblies).

## <a name="supported-types-for-bindings"></a>Tipos suportados para enlaces

Cada enlace tem os seus próprios tipos suportados; Por exemplo, um acionador de blob pode ser utilizado com um parâmetro de cadeia, um parâmetro POCO um `CloudBlockBlob` parâmetro ou qualquer um dos vários outros tipos suportados. O [artigo de referência de enlace para enlaces de blob](functions-bindings-storage-blob.md#trigger---usage) apresenta uma lista de todos os suportados tipos de parâmetro para acionadores de blob. Para obter mais informações, consulte [Acionadores e enlaces](functions-triggers-bindings.md) e [docs de referência de enlace para cada tipo de enlace](functions-triggers-bindings.md#next-steps).

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="referencing-custom-classes"></a>Referência de classes personalizadas

Se precisar de utilizar uma classe simples antigo CLR objeto (POCO) personalizada, pode incluir a definição de classe dentro do mesmo ficheiro ou colocá-la num ficheiro separado.

O seguinte exemplo mostra um *run.csx* exemplo inclui uma definição de classe POCO.

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

Uma classe POCO tem de ter um getter e setter definido para cada propriedade.

## <a name="reusing-csx-code"></a>Reutilizar o código de .csx

Pode utilizar classes e métodos definidos noutras *.csx* ficheiros no seu *run.csx* ficheiro. Para tal, utilize `#load` diretivas do seu *run.csx* ficheiro. No exemplo seguinte, uma rotina de registo com o nome `MyLogger` é partilhada na *myLogger.csx* e carregados para *run.csx* utilizando o `#load` diretiva:

Exemplo *run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}");
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Exemplo *mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext);
}
```

Utilizar um partilhado *.csx* ficheiro é um padrão comum quando pretender vivamente escreva dados thet transmitidos entre as funções por utilizar um objeto POCO. No exemplo seguinte simplificado, um acionador HTTP e o acionador de fila partilham um objeto POCO com o nome `Order` vivamente escreve os dados de ordem:

Exemplo *run.csx* para o acionador HTTP:

```cs
#load "..\shared\order.csx"

using System.Net;

public static async Task<HttpResponseMessage> Run(Order req, IAsyncCollector<Order> outputQueueItem, TraceWriter log)
{
    log.Info("C# HTTP trigger function received an order.");
    log.Info(req.ToString());
    log.Info("Submitting to processing queue.");

    if (req.orderId == null)
    {
        return new HttpResponseMessage(HttpStatusCode.BadRequest);
    }
    else
    {
        await outputQueueItem.AddAsync(req);
        return new HttpResponseMessage(HttpStatusCode.OK);
    }
}
```

Exemplo *run.csx* para o acionador de fila:

```cs
#load "..\shared\order.csx"

using System;

public static void Run(Order myQueueItem, out Order outputQueueItem,TraceWriter log)
{
    log.Info($"C# Queue trigger function processed order...");
    log.Info(myQueueItem.ToString());

    outputQueueItem = myQueueItem;
}
```

Exemplo *order.csx*:

```cs
public class Order
{
    public string orderId {get; set; }
    public string custName {get; set;}
    public string custAddress {get; set;}
    public string custEmail {get; set;}
    public string cartId {get; set; }

    public override String ToString()
    {
        return "\n{\n\torderId : " + orderId +
                  "\n\tcustName : " + custName +             
                  "\n\tcustAddress : " + custAddress +             
                  "\n\tcustEmail : " + custEmail +             
                  "\n\tcartId : " + cartId + "\n}";             
    }
}
```

Pode utilizar um caminho relativo com o `#load` diretiva:

* `#load "mylogger.csx"`carrega um ficheiro localizado na pasta de função.
* `#load "loadedfiles\mylogger.csx"`carrega um ficheiro localizado numa pasta na pasta de função.
* `#load "..\shared\mylogger.csx"`carrega um ficheiro localizado numa pasta com o mesmo nível, como a pasta de função, ou seja, diretamente em *wwwroot*.

O `#load` diretiva funciona apenas com *.csx* ficheiros, não com *CS* ficheiros.

## <a name="binding-to-method-return-value"></a>Enlace para o valor de retorno do método

Pode utilizar um valor de retorno do método para um enlace de saída, utilizando o nome `$return` no *function.json*:

```json
{
    "type": "queue",
    "direction": "out",
    "name": "$return",
    "queueName": "outqueue",
    "connection": "MyStorageConnectionString",
}
```

```csharp
public static string Run(string input, TraceWriter log)
{
    return input;
}
```

## <a name="writing-multiple-output-values"></a>Escrita de vários valores de saída

Para escrever valores múltiplos para um enlace de saída, utilize o [ `ICollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) ou [ `IAsyncCollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) tipos. Estes tipos são coleções de só de escrita que são escritas para o enlace de saída quando o método for concluída.

Neste exemplo escreve vários de fila de mensagens em fila o mesmo, utilizando `ICollector`:

```csharp
public static void Run(ICollector<string> myQueueItem, TraceWriter log)
{
    myQueueItem.Add("Hello");
    myQueueItem.Add("World!");
}
```

## <a name="logging"></a>Registo

Para iniciar a sessão de saída para os registos de transmissão em fluxo em c#, incluir um argumento de tipo `TraceWriter`. Recomendamos que o nome `log`. Evite utilizar `Console.Write` nas funções do Azure. 

`TraceWriter`está definido no [SDK de WebJobs do Azure](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs). O nível de registo para `TraceWriter` podem ser configuradas no [host.json](functions-host-json.md).

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

> [!NOTE]
> Para obter informações sobre uma arquitetura de registo mais recente que pode utilizar em vez de `TraceWriter`, consulte [registos de escrita em c# funções](functions-monitoring.md#write-logs-in-c-functions) no **das funções do Azure do Monitor** artigo.

## <a name="async"></a>Async

Para efetuar uma função assíncrona, utilize o `async` palavra-chave e devolver um `Task` objeto.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName,
        Stream blobInput,
        Stream blobOutput)
{
    await blobInput.CopyToAsync(blobOutput, 4096);
}
```

## <a name="cancellation-tokens"></a>Tokens de cancelamento

Algumas operações necessitam de encerramento correto. Enquanto é sempre melhor escrever código que pode processar a falhar, nos casos em que pretende processar pedidos de encerramento, definir uma [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argumento de tipo.  A `CancellationToken` é fornecido para assinalar que é acionado um encerramento de anfitrião.

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
    string blobName,
    Stream blobInput,
    Stream blobOutput,
    CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a>Importação de espaços de nomes

Se precisar de importar os espaços de nomes, pode fazê-lo, por isso, normal, com o `using` cláusula.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Os seguintes espaços de nomes são importados automaticamente e, por isso, são opcionais:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`

## <a name="referencing-external-assemblies"></a>Referência a assemblagens externas

Para assemblagens de estrutura, adicionar referências utilizando o `#r "AssemblyName"` diretiva.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

As assemblagens seguintes são adicionadas automaticamente as funções do Azure, ambiente de alojamento:

* `mscorlib`
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`

As assemblagens seguintes podem ser referenciadas pelo nome simples (por exemplo, `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNet.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

## <a name="referencing-custom-assemblies"></a>Referência a assemblagens personalizadas

Para fazer referência a uma assemblagem personalizada, pode utilizar tanto um *partilhado* assemblagem ou um *privada* assemblagem:
- As assemblagens partilhadas são partilhadas entre todas as funções dentro de uma aplicação de função. Para fazer referência a uma assemblagem personalizada, carregar a assemblagem para a sua aplicação de função, tal como num `bin` pasta na raiz da aplicação de função. 
- Assemblagens privadas fazem parte do contexto de uma função especificada e suportam sideloading das versões diferentes. As assemblagens privadas devem ser carregadas um `bin` pasta no diretório de função. Referência as assemblagens com o nome de ficheiro, tal como `#r "MyAssembly.dll"`. 

Para obter informações sobre como carregar ficheiros para a pasta de função, consulte a secção sobre [pacote de gestão](#using-nuget-packages).

### <a name="watched-directories"></a>Observada diretórios

O diretório que contém o ficheiro de script de função é automaticamente observado para alterações a assemblagens. Para ver as alterações de assemblagem noutros diretórios, adicioná-los para o `watchDirectories` lista [host.json](functions-host-json.md).

## <a name="using-nuget-packages"></a>Utilizar pacotes de NuGet

Para utilizar pacotes de NuGet numa função de c#, carregar um *project.json* ficheiro para pasta a função no sistema de ficheiros da aplicação de função. Eis um exemplo *project.json* ficheiro que adiciona uma referência à Microsoft.ProjectOxford.Face versão 1.1.0:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

No Azure funciona 1. x, apenas o .NET Framework 4.6 é suportada, por isso, certifique-se de que o *project.json* ficheiro Especifica `net46` conforme mostrado aqui.

Quando carregar um *project.json* obtém os pacotes de ficheiros, o tempo de execução e adiciona automaticamente referências para as assemblagens de pacote. Não tem de adicionar `#r "AssemblyName"` diretivas. Para utilizar os tipos definidos nos pacotes NuGet; Basta adicionar necessários `using` instruções para sua *run.csx* ficheiro. 

O tempo de execução de funções, restauro NuGet funciona através da comparação `project.json` e `project.lock.json`. Se os carimbos de data e hora dos ficheiros **não** corresponde a, um restauro do NuGet executa e transferências do NuGet atualizado pacotes. No entanto, se os carimbos de data e hora dos ficheiros **fazer** corresponde a, NuGet não efetuar um restauro. Por conseguinte, `project.lock.json` devem não ser implementados, que faz com que NuGet ignorar o restauro do pacote. Para evitar a implementar o ficheiro de bloqueio, adicione o `project.lock.json` para o `.gitignore` ficheiro.

Para utilizar um feed de NuGet personalizada, especifique do feed num *Nuget.Config* ficheiro na raiz da aplicação de função. Para obter mais informações, consulte [configurar NuGet comportamento](/nuget/consume-packages/configuring-nuget-behavior).

### <a name="using-a-projectjson-file"></a>Utilizando um ficheiro de project.json

1. Abra a função no portal do Azure. Separador registos apresenta a saída de instalação do pacote.
2. Para carregar um ficheiro de project.json, utilize um dos métodos descritos no [como atualizar os ficheiros de aplicação de função](functions-reference.md#fileupdate) o tópico de referência para programadores das funções do Azure.
3. Depois do *project.json* ficheiro é carregado, consulte o resultado semelhante ao seguinte exemplo na sua função de transmissão em fluxo de registo:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Variáveis de ambiente

Para obter uma variável de ambiente ou uma valor de definição de aplicação, utilize `System.Environment.GetEnvironmentVariable`, conforme mostrado no exemplo de código seguinte:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " +
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

<a name="imperative-bindings"></a> 

## <a name="binding-at-runtime"></a>Enlace no tempo de execução

Em c# e outras linguagens .NET, pode utilizar um [imperativo](https://en.wikipedia.org/wiki/Imperative_programming) enlace padrão, como opposed para o [ *declarativa* ](https://en.wikipedia.org/wiki/Declarative_programming) enlaces no *function.json*. Enlace imperativo é útil quando os parâmetros de enlace tem de ser calculada ao tempo de tempo de execução, em vez de design. Com este padrão, é possível vincular suportada de entrada e saída enlaces no-a-momento no seu código de função.

Defina um imperativo enlace da seguinte forma:

- **Não** incluem uma entrada no *function.json* dos enlaces imperativo pretendidos.
- Passagem de um parâmetro de entrada [ `Binder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) ou [ `IBinder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- Utilize o seguinte c# padrão para efetuar o enlace de dados.

```cs
using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
{
    ...
}
```

`BindingTypeAttribute`é o atributo de .NET que define o enlace e `T` é um tipo de entrada ou de saída que é suportado por esse tipo de enlace. `T`não pode ser um `out` tipo de parâmetro (tais como `out JObject`). Por exemplo, a tabela de Mobile Apps saída enlace suporta [seis tipos de saída](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), mas só pode utilizar [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) ou [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) para `T`.

### <a name="single-attribute-example"></a>Exemplo de atributo

O código de exemplo seguinte cria um [vínculo de saída de blob de armazenamento](functions-bindings-storage-blob.md#output) com BLOBs caminho definido em tempo de execução, em seguida, escreve uma cadeia de blob.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    using (var writer = await binder.BindAsync<TextWriter>(new BlobAttribute("samples-output/path")))
    {
        writer.Write("Hello World!!");
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) define o [blob de armazenamento](functions-bindings-storage-blob.md) entrada ou saída de enlace, e [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) é um tipo de enlace de saída suportado.

### <a name="multiple-attribute-example"></a>Exemplo de atributo vários

O exemplo anterior obtém a definição de aplicação para a cadeia de ligação para a aplicação de função principal armazenamento conta (que é `AzureWebJobsStorage`). Pode especificar uma definição de aplicação personalizada a utilizar para a conta de armazenamento, adicionando o [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) e transmitir a matriz de atributo para `BindAsync<T>()`. Utilize um `Binder` parâmetro, não `IBinder`.  Por exemplo:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    var attributes = new Attribute[]
    {    
        new BlobAttribute("samples-output/path"),
        new StorageAccountAttribute("MyStorageAccount")
    };

    using (var writer = await binder.BindAsync<TextWriter>(attributes))
    {
        writer.Write("Hello World!");
    }
}
```

A tabela seguinte lista os atributos de .NET para cada tipo de enlace e os pacotes que são definidos.

> [!div class="mx-codeBreakAll"]
| Vínculo | Atributo | Adicionar referência |
|------|------|------|
| BD do Cosmos | [`Microsoft.Azure.WebJobs.DocumentDBAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.CosmosDB"` |
| Event Hubs | [`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.Jobs.ServiceBus"` |
| Mobile Apps | [`Microsoft.Azure.WebJobs.MobileTableAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.MobileApps"` |
| Hubs de Notificação | [`Microsoft.Azure.WebJobs.NotificationHubAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.NotificationHubs"` |
| Service Bus | [`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.WebJobs.ServiceBus"` |
| Fila de armazenamento | [`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Blob de armazenamento | [`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Tabela de armazenamento | [`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Twilio | [`Microsoft.Azure.WebJobs.TwilioSmsAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.Twilio"` |

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre os acionadores e enlaces](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Saiba mais sobre as melhores práticas para as funções do Azure](functions-best-practices.md)
