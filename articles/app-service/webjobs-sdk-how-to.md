---
title: Como utilizar o SDK de WebJobs - Azure
description: Saiba mais sobre como escrever código para o SDK do WebJobs. Crie tarefas de processamento que acedem a dados nos serviços de terceiros e de serviços do Azure de condicionada por eventos em segundo plano.
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/19/2019
ms.author: glenga
ms.openlocfilehash: ab502c25a632977065e55d2eeafd684203636b14
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/12/2019
ms.locfileid: "56109916"
---
# <a name="how-to-use-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Como utilizar o SDK de WebJobs do Azure para processamento condicionada por eventos em segundo plano

Este artigo fornece orientações sobre como escrever código para [o SDK de WebJobs do Azure](webjobs-sdk-get-started.md). A documentação aplica-se os dois versão 3.x e 2.x do SDK do WebJobs. Sempre que existirem diferenças de API, são fornecidos exemplos de ambos. A principal alteração introduzida pela versão 3.x é o uso do .NET Core em vez do .NET Framework.

>[!NOTE]
> [As funções do Azure](../azure-functions/functions-overview.md) baseia-se sobre o SDK do WebJobs e este artigo liga para documentação de funções do Azure para alguns tópicos. Tenha em atenção as seguintes diferenças entre as funções e o SDK do WebJobs:
> * A versão das funções do Azure 2.x corresponde à versão do SDK de WebJobs 3.x e as funções do Azure 1.x corresponde ao SDK do WebJobs 2.x. Repositórios de código de origem, siga o SDK de WebJobs numeração.
> * Código de exemplo para bibliotecas de classes do funções do Azure c# é como o SDK de WebJobs de código, exceto pelo fato não é necessário um `FunctionName` atributo num projeto do SDK do WebJobs.
> * Alguns tipos de enlace só são suportados em funções, como HTTP, o webhook e o Event Grid (que é baseado em HTTP).
> 
> Para obter mais informações, consulte [Compare os WebJobs SDK e as funções do Azure](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs).

## <a name="prerequisites"></a>Pré-requisitos

Este artigo pressupõe que tenha lido e concluir as tarefas no [começar com o SDK de WebJobs](webjobs-sdk-get-started.md).

## <a name="webjobs-host"></a>Anfitrião de WebJobs

O anfitrião é um contentor de tempo de execução para as funções.  Escuta acionadores e as chamadas de funções. Na versão 3.x, o host é uma implementação de `IHost`e na versão 2.x é usar o `JobHost` objeto. Criar uma instância de host no seu código e escrever código para personalizar seu comportamento.

Esta é a principal diferença entre usar diretamente o SDK do WebJobs e usá-lo indiretamente através das funções do Azure. As funções do Azure, o serviço controla o anfitrião e não é possível personalizá-lo ao escrever código. As funções do Azure permite-lhe personalizar o comportamento de host através de definições no *Host. JSON* ficheiro. Essas definições são cadeias de caracteres, não código, que limita os tipos de personalizações que pode fazer.

### <a name="host-connection-strings"></a>Cadeias de ligação do anfitrião 

O SDK do WebJobs procura cadeias de ligação de armazenamento do Azure e o Azure Service Bus no *Settings* ficheiro ao executar localmente ou num ambiente do WebJob quando executa no Azure. Por predefinição, com o nome de definição da cadeia de uma ligação de armazenamento `AzureWebJobsStorage` é necessária.  

Versão 2.x do SDK permite-lhe utilizar os seus próprios nomes para essas cadeias de caracteres de conexão ou armazená-los em outro lugar. Pode defini-las no código, conforme mostrado aqui:

```cs
static void Main(string[] args)
{
    var _storageConn = ConfigurationManager
        .ConnectionStrings["MyStorageConnection"].ConnectionString;

    //// Dashboard logging is deprecated; use Application Insights.
    //var _dashboardConn = ConfigurationManager
    //    .ConnectionStrings["MyDashboardConnection"].ConnectionString;

    JobHostConfiguration config = new JobHostConfiguration();
    config.StorageConnectionString = _storageConn;
    //config.DashboardConnectionString = _dashboardConn;
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

Porque utiliza a configuração predefinida do .NET Core APIs, não há nenhuma API na versão 3.x para alterar os nomes de cadeia de ligação.

### <a name="host-development-settings"></a>Definições de desenvolvimento do anfitrião

Pode executar o anfitrião no modo de desenvolvimento para tornar o desenvolvimento local mais eficiente. Aqui estão algumas das definições que são alteradas quando em execução no modo de desenvolvimento:

| Propriedade | Definição de desenvolvimento |
| ------------- | ------------- |
| `Tracing.ConsoleLevel` | `TraceLevel.Verbose` para maximizar a saída de registo. |
| `Queues.MaxPollingInterval`  | Um valor baixo para garantir que os métodos de fila são acionados imediatamente.  |
| `Singleton.ListenerLockPeriod` | 15 segundos para ajudar no rápido desenvolvimento iterativo. |

A versão do SDK depende da forma que ativa o modo de desenvolvimento. 

#### <a name="version-3x"></a>Versão 3.x

Versão 3.x utiliza as APIs de núcleo do ASP.NET padrão. Chamar o [UseEnvironment](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useenvironment) método no [ `HostBuilder` ](/dotnet/api/microsoft.extensions.hosting.hostbuilder) instância. Passar uma cadeia de caracteres com o nome `development`, como no exemplo a seguir:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.UseEnvironment("development");
    builder.ConfigureWebJobs(b =>
            {
                b.AddAzureStorageCoreServices();
            });
    var host = builder.Build();
    using (host)
    {
        host.Run();
    }
}
```

#### <a name="version-2x"></a>Versão 2.x

O `JobHostConfiguration` classe tem um `UseDevelopmentSettings` método que permite que o modo de desenvolvimento.  O exemplo seguinte mostra como utilizar as definições de desenvolvimento. Para tornar `config.IsDevelopment` retornar `true` ao executar localmente, defina uma variável de ambiente local chamada `AzureWebJobsEnv` com o valor `Development`.

```cs
static void Main()
{
    config = new JobHostConfiguration();

    if (config.IsDevelopment)
    {
        config.UseDevelopmentSettings();
    }

    var host = new JobHost(config);
    host.RunAndBlock();
}
```

### <a name="jobhost-servicepointmanager-settings"></a>Gerenciamento de conexões simultâneas (v2.x)

Na versão 3.x, o limite de ligação está predefinida para ligações de infinitas. Se por algum motivo precisar de alterar este limite, pode utilizar o [MaxConnectionsPerServer](/dotnet/api/system.net.http.winhttphandler.maxconnectionsperserver) propriedade da [WinHttpHander](/dotnet/api/system.net.http.winhttphandler) classe.

Para a versão 2.x, controla o número de ligações simultâneas a um anfitrião utilizando o [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) API. 2.x, deve aumentar este valor da predefinição de 2 antes de iniciar seu host de WebJobs.

Saída de todos os pedidos HTTP que fizer uma função utilizando `HttpClient` fluem a `ServicePointManager`. Quando atingir o `DefaultConnectionLimit`, o `ServicePointManager` começa da fila as solicitações antes de enviá-los. Suponha que sua `DefaultConnectionLimit` está definido como 2 e os pedidos de HTTP 1.000 do código faz. Inicialmente, apenas duas são permitidos pedidos por meio de para o sistema operacional. Os outros 998 são colocados em fila até que haja espaço para eles. Isso significa que seu `HttpClient` talvez o tempo limite, porque ele *pensa* ele fez o pedido, mas nunca foi enviado o pedido pelo sistema operacional para o servidor de destino. Pelo que poderá ver o comportamento que parece que não fazem sentido: local `HttpClient` está a demorar 10 segundos para concluir um pedido, mas o seu serviço está a devolver todos os pedidos em 200 ms. 

O valor predefinido para aplicativos ASP.NET é `Int32.MaxValue`, e isso é muito provável que funcione bem para o WebJobs em execução num plano de serviço de aplicações básico ou superior. WebJobs normalmente, tem da definição Always On e que só é suportada por planos de serviço de aplicações básico ou superiores. 

Se estiver a executar o WebJob num gratuito ou partilhado plano do serviço de aplicações, seu aplicativo está restringido pelo sandbox do serviço de aplicações, que tem atualmente um [limite de ligação de 300](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#per-sandbox-per-appper-site-numerical-limits). Com um limite de ligação desvinculado em `ServicePointManager`, é mais provável que irá ser atingido o limiar de ligação de área restrita e o site é encerrado. Nesse caso, a definição `DefaultConnectionLimit` para algo mais baixa, como 50 ou 100, pode impedir que isto aconteça e ainda permitir para débito suficiente.

A definição tem de ser configurada antes de serem efetuados quaisquer pedidos HTTP. Por esse motivo, o anfitrião de WebJobs não deve tentar ajustar a definição automaticamente; Pode haver pedidos HTTP que ocorrem antes do anfitrião é iniciado e isso pode causar um comportamento inesperado. A melhor abordagem é definir o valor imediatamente em seu `Main` método antes de inicializar o `JobHost`, conforme mostrado no exemplo a seguir

```csharp
static void Main(string[] args)
{
    // Set this immediately so that it is used by all requests.
    ServicePointManager.DefaultConnectionLimit = Int32.MaxValue;

    var host = new JobHost();
    host.RunAndBlock();
}
```

## <a name="triggers"></a>Acionadores

As funções têm de ser métodos públicos e tem de ter um atributo de Acionador ou o [NoAutomaticTrigger](#manual-trigger) atributo.

### <a name="automatic-trigger"></a>Início automático

Acionadores automática chamam uma função em resposta a um evento. Por exemplo, veja o acionador de fila no [artigo de introdução de Get](webjobs-sdk-get-started.md).

### <a name="manual-trigger"></a>Acionador manual

Para acionar manualmente uma função, utilize o `NoAutomaticTrigger` de atributo, conforme mostrado no exemplo a seguir:

```cs
[NoAutomaticTrigger]
public static void CreateQueueMessage(
ILogger logger,
string value,
[Queue("outputqueue")] out string message)
{
    message = value;
    logger.LogInformation("Creating queue message: ", message);
}
```

A forma como vai acionar manualmente a função depende da versão do SDK.

#### <a name="version-3x"></a>Versão 3.x

```cs
static async Task Main(string[] args)
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddAzureStorage();
    });
    var host = builder.Build();
    using (host)
    {
        var jobHost = host.Services.GetService(typeof(IJobHost)) as JobHost;
        var inputs = new Dictionary<string, object>
        {
            { "value", "Hello world!" }
        };

        await host.StartAsync();
        await jobHost.CallAsync("CreateQueueMessage", inputs);
        await host.StopAsync();
    }
}
```

#### <a name="version-2x"></a>Versão 2.x

```cs
static void Main(string[] args)
{
    JobHost host = new JobHost();
    host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
}
```

## <a name="input-and-output-bindings"></a>Entrada e saída enlaces

Enlaces de entrada proporcionam uma forma declarativa para que os dados do Azure ou serviços de terceiros disponíveis para o seu código. Enlaces de saída proporcionam uma forma para atualizar os dados. O [artigo de introdução de Get](webjobs-sdk-get-started.md) mostra um exemplo de cada.

Pode utilizar um valor de retorno do método para um enlace de saída, ao aplicar o atributo para o valor de retorno do método. Veja o exemplo em funções do Azure [Acionadores e enlaces](../azure-functions/functions-triggers-bindings.md#using-the-function-return-value) artigo.

## <a name="binding-types"></a>Tipos de enlace

A maneira que tipos de enlace estão instalados e gerenciados é diferente entre a versão 3.x e 2.x do SDK. Pode encontrar o pacote para instalação de um tipo de ligação específica na **pacotes** secção desse tipo de enlace [artigo de referência](#binding-reference-information) das funções do Azure. Uma exceção é o acionador de ficheiros e de ligação (para o sistema de ficheiros local), que não é suportada pelas funções do Azure.

#### <a name="version-3x"></a>Versão 3.x

Na versão 3.x, os enlaces de armazenamento estão incluídos no `Microsoft.Azure.WebJobs.Extensions.Storage` pacote. Chamar o `AddAzureStorage` método de extensão no `ConfigureWebJobs` método conforme mostrado no exemplo a seguir:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
            {
                b.AddAzureStorageCoreServices();
                b.AddAzureStorage();
            });
    var host = builder.Build();
    using (host)
    {
        host.Run();
    }
}
```

Para utilizar outros acionador e os tipos de ligação, instale o pacote NuGet que contém-los e chame o `Add<binding>` implementado na extensão de método de extensão. Por exemplo, se quiser usar uma ligação do Azure Cosmos DB, instale `Microsoft.Azure.WebJobs.Extensions.CosmosDB` e chamar `AddCosmosDB`, como no exemplo a seguir:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
            {
                b.AddAzureStorageCoreServices();
                b.AddCosmosDB();
            });
    var host = builder.Build();
    using (host)
    {
        host.Run();
    }
}
```

Para utilizar o acionador de temporizador ou os ficheiros de ligação, que fazem parte de serviços principais, chamar o `AddTimers` ou `AddFiles` métodos de extensão, respectivamente.

#### <a name="version-2x"></a>Versão 2.x

Os seguintes tipos de Acionador e vinculação estão incluídos na versão 2.x do `Microsoft.Azure.WebJobs` pacote:

* Armazenamento de blobs
* Armazenamento de filas
* Table Storage

Para utilizar outro acionador e tipos de ligação, instale o pacote NuGet que contém-los e chamar uma `Use<binding>` método no `JobHostConfiguration` objeto. Por exemplo, se pretender utilizar um acionador de temporizador, instale `Microsoft.Azure.WebJobs.Extensions` e chamar `UseTimers` no `Main` método, tal como neste exemplo:

```cs
static void Main()
{
    config = new JobHostConfiguration();
    config.UseTimers();
    var host = new JobHost(config);
    host.RunAndBlock();
}
```

Para utilizar os ficheiros de ligação, instale `Microsoft.Azure.WebJobs.Extensions` e chamar `UseFiles`.

### <a name="executioncontext"></a>ExecutionContext

WebJobs permite-lhe ligar a uma [ `ExecutionContext` ]. Com essa ligação, pode aceder a [ `ExecutionContext` ] como um parâmetro na assinatura de função. Por exemplo, o código a seguir usa o objeto de contexto para acessar o ID de invocação, o que pode usar para correlacionar a todos os registos produzidos por uma invocação de função especificada.  

```cs
public class Functions
{
    public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        ExecutionContext executionContext,
        ILogger logger)
    {
        logger.LogInformation($"{message}\n{executionContext.InvocationId}");
    }
}
```

A maneira que ligar para o [ `ExecutionContext` ] depende da sua versão do SDK.

#### <a name="version-3x"></a>Versão 3.x

Chamar o `AddExecutionContextBinding` método de extensão no `ConfigureWebJobs` método conforme mostrado no exemplo a seguir:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
            {
                b.AddAzureStorageCoreServices();
                b.AddExecutionContextBinding();
            });
    var host = builder.Build();
    using (host)
    {
        host.Run();
    }
}
```

#### <a name="version-2x"></a>Versão 2.x

O `Microsoft.Azure.WebJobs.Extensions` pacote mencionado anteriormente também fornece um tipo especial de associação que pode registrar chamando o `UseCore` método. Esta ligação permite que defina uma [ `ExecutionContext` ] parâmetro na assinatura de função, que está ativada da seguinte forma:

```cs
class Program
{
    static void Main()
    {
        config = new JobHostConfiguration();
        config.UseCore();
        var host = new JobHost(config);
        host.RunAndBlock();
    }
}
```

## <a name="binding-configuration"></a>Configuração de ligação

Alguns acionador e os enlaces permitem-lhe configurar seu comportamento. A forma de configurá-las depende da versão do SDK.

* **Versão 3.x:** Configuração é definida quando o `Add<Binding>` método é chamado `ConfigureWebJobs`.
* **Versão 2.x:** Defina propriedades num objeto de configuração que passa para o `JobHost`.

### <a name="queue-trigger-configuration"></a>Configuração do acionador de fila

As definições que pode configurar para o acionador de fila de armazenamento são explicadas em funções do Azure [referência de Host. JSON](../azure-functions/functions-host-json.md#queues). Os exemplos seguintes mostram como configurá-los na sua configuração:

#### <a name="version-3x"></a>Versão 3.x

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddAzureStorage(a => {
            a.BatchSize = 8;
            a.NewBatchThreshold = 4;
            a.MaxDequeueCount = 4;
            a.MaxPollingInterval = TimeSpan.FromSeconds(15);
        });
    });
    var host = builder.Build();
    using (host)
    {

        host.Run();
    }
}
```

#### <a name="version-2x"></a>Versão 2.x

```cs
static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.Queues.BatchSize = 8;
    config.Queues.NewBatchThreshold = 4;
    config.Queues.MaxDequeueCount = 4;
    config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

### <a name="configuration-for-other-bindings"></a>Configuração para outros enlaces

Alguns tipos de Acionador e vinculação de definem seu próprio tipo de configuração personalizada. Por exemplo, o acionador de arquivo permite-lhe especificar o caminho de raiz para monitorizar, como nos exemplos seguintes:

#### <a name="version-3x"></a>Versão 3.x

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddFiles(a => a.RootPath = @"c:\data\import");
    });
    var host = builder.Build();
    using (host)
    {

        host.Run();
    }
}
```

#### <a name="version-2x"></a>Versão 2.x

```cs
static void Main()
{
    config = new JobHostConfiguration();
    var filesConfig = new FilesConfiguration
    {
        RootPath = @"c:\data\import"
    };
    config.UseFiles(filesConfig);
    var host = new JobHost(config);
    host.RunAndBlock();
}
```

## <a name="binding-expressions"></a>Expressões de associação

Nos parâmetros do construtor de atributo, pode usar as expressões que resolva para valores de várias origens. Por exemplo, no código a seguir, o caminho para o `BlobTrigger` atributo cria uma expressão com o nome `filename`. Quando utilizada para o enlace de saída, `filename` é resolvido para o nome do blob acionadora.

```cs
public static void CreateThumbnail(
    [BlobTrigger("sample-images/{filename}")] Stream image,
    [Blob("sample-images-sm/{filename}", FileAccess.Write)] Stream imageSmall,
    string filename,
    ILogger logger)
{
    logger.Info($"Blob trigger processing: {filename}");
    // ...
}
```

Para obter mais informações sobre expressões de associação, consulte [expressões e padrões de vinculação](../azure-functions/functions-triggers-bindings.md#binding-expressions-and-patterns) na documentação das funções do Azure.

### <a name="custom-binding-expressions"></a>Expressões de associação personalizado

Por vezes, pretende especificar um nome de fila, um nome de blob ou contentor ou nome de uma tabela no código, em vez de codificá-lo. Por exemplo, pode pretender especificar o nome da fila para o `QueueTrigger` atributo numa variável de ambiente ou ficheiro de configuração.

Pode fazê-lo ao transmitir uma `NameResolver` objeto para o `JobHostConfiguration` objeto. Incluir espaços reservados no acionador ou parâmetros do construtor de atributo de ligação e seu `NameResolver` código fornece os valores reais para ser utilizado em vez dos marcadores de posição. Os marcadores de posição são identificados rodeando-os com sinais de sinal de percentagem (), conforme mostrado no exemplo a seguir:

```cs
public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
{
    Console.WriteLine(logMessage);
}
```

Esse código permite-lhe utilizar uma fila com o nome `logqueuetest` no ambiente de teste e um denominado `logqueueprod` na produção. Em vez de um nome de fila codificada, especifique o nome de uma entrada no `appSettings` coleção.

Existe um padrão NameResolver entra em vigor, se não fornecer um personalizado. A predefinição obtém os valores de definições da aplicação ou variáveis de ambiente.

Sua `NameResolver` classe obtém o nome da fila de `appSettings` conforme mostrado no exemplo a seguir:

```cs
public class CustomNameResolver : INameResolver
{
    public string Resolve(string name)
    {
        return ConfigurationManager.AppSettings[name].ToString();
    }
}
```

#### <a name="version-3x"></a>Versão 3.x

O resolvedor é configurado usando injeção de dependência. Estes exemplos de exigem o seguinte `using` instrução:

```cs
using Microsoft.Extensions.DependencyInjection;
```

O resolvedor é adicionado ao chamar o [ `ConfigureServices` ] método de extensão no [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder), como no exemplo a seguir:

```cs
static async Task Main(string[] args)
{
    var builder = new HostBuilder();
    var resolver = new CustomNameResolver();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
    });
    builder.ConfigureServices(s => s.AddSingleton<INameResolver>(resolver));
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

#### <a name="version-2x"></a>Versão 2.x

Passar seu `NameResolver` classe para o `JobHost` objeto conforme mostrado no exemplo a seguir:

```cs
 static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.NameResolver = new CustomNameResolver();
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

Azure implementa funções `INameResolver` para obter valores de definições da aplicação, conforme mostrado no exemplo. Quando utiliza o SDK do WebJobs diretamente, pode escrever uma implementação personalizada que obtém os valores de substituição de marcador de posição de qualquer origem que preferir. 

## <a name="binding-at-runtime"></a>Ligação em tempo de execução

Se precisar de fazer algum trabalho na sua função antes de utilizar, tais como um atributo de vinculação `Queue`, `Blob`, ou `Table`, pode utilizar o `IBinder` interface.

O exemplo a seguir usa uma mensagem de fila de entrada e cria uma nova mensagem com o mesmo conteúdo numa fila de saída. O nome da fila de saída é definido pelo código no corpo da função.

```cs
public static void CreateQueueMessage(
    [QueueTrigger("inputqueue")] string queueMessage,
    IBinder binder)
{
    string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
    QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
    CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
    outputQueue.AddMessageAsync(new CloudQueueMessage(queueMessage));
}
```

Para obter mais informações, consulte [enlace em tempo de execução](../azure-functions/functions-dotnet-class-library.md#binding-at-runtime) na documentação das funções do Azure.

## <a name="binding-reference-information"></a>Informações de referência de ligação

Informações de referência sobre cada tipo de enlace são fornecidas na documentação das funções do Azure. Utilizar a fila de armazenamento como exemplo, encontrará as seguintes informações em cada artigo de referência de ligação:

* [Pacotes](../azure-functions/functions-bindings-storage-queue.md#packages---functions-1x) -o que o pacote para instalar o modo a incluir suporte para o enlace num projeto do SDK do WebJobs.
* [Exemplos](../azure-functions/functions-bindings-storage-queue.md#trigger---example) -o exemplo # C classe biblioteca aplica-se ao SDK do WebJobs; basta omitir o `FunctionName` atributo.
* [Atributos](../azure-functions/functions-bindings-storage-queue.md#trigger---attributes) -os atributos a utilizar para o tipo de associação.
* [Configuração](../azure-functions/functions-bindings-storage-queue.md#trigger---configuration) -explicações das propriedades de atributo e parâmetros do construtor.
* [Utilização](../azure-functions/functions-bindings-storage-queue.md#trigger---usage) - o que tipos de fazer a ligação e o enlace de informações sobre como funciona. Por exemplo: consulta algoritmo, inviabilizar o processamento de fila.
  
Para obter uma lista de artigos de referência de ligação, veja **suportado enlaces** no [Acionadores e enlaces](../azure-functions/functions-triggers-bindings.md#supported-bindings) artigo para as funções do Azure. Nessa lista, os enlaces de HTTP, o webhook e o Event Grid são suportados apenas por funções do Azure, não pelo SDK do WebJobs.

## <a name="disable-attribute"></a>Desativar o atributo 

O [desativar](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/DisableAttribute.cs) atributo lhe permite controlar se uma função pode ser acionado. 

No exemplo seguinte, se a definição de aplicação `Disable_TestJob` tem um valor de "1" ou "True (maiúsculas de minúsculas), a função não será executado". Nesse caso, o tempo de execução cria uma mensagem de registo *função 'Functions.TestJob' está desabilitada*.

```cs
[Disable("Disable_TestJob")]
public static void TestJob([QueueTrigger("testqueue2")] string message)
{
    Console.WriteLine("Function with Disable attribute executed!");
}
```

Quando altera os valores de definição de aplicação no portal do Azure, ele faz com que o WebJob seja reiniciado, a nova definição de separação.

O atributo pode ser declarado no nível de classe, método ou parâmetro. O nome da definição também pode conter expressões de associação.

## <a name="timeout-attribute"></a>Atributo de tempo limite

O [tempo limite](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TimeoutAttribute.cs) atributo faz com que uma função para ser cancelado se ele não é concluído dentro de um período de tempo especificado. No exemplo seguinte, a função seria executado durante um dia sem o tempo limite. Com o tempo limite, a função é cancelada após 15 segundos.

```cs
[Timeout("00:00:15")]
public static async Task TimeoutJob(
    [QueueTrigger("testqueue2")] string message,
    CancellationToken token,
    TextWriter log)
{
    await log.WriteLineAsync("Job starting");
    await Task.Delay(TimeSpan.FromDays(1), token);
    await log.WriteLineAsync("Job completed");
}
```

Pode aplicar o atributo de tempo limite ao nível de classe ou método e pode especificar um tempo limite global utilizando `JobHostConfiguration.FunctionTimeout`. Tempos limite de nível de classe ou método substituir o tempo limite global.

## <a name="singleton-attribute"></a>Atributo de singleton

Utilize o [Singleton](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/SingletonAttribute.cs) atributo para garantir que apenas uma instância de uma função é executada, mesmo quando existem várias instâncias da aplicação de web do anfitrião. Ele faz isso implementando [distribuído bloqueio](#viewing-lease-blobs).

No exemplo a seguir, apenas uma única instância do `ProcessImage` função é executada num determinado momento:

```cs
[Singleton]
public static async Task ProcessImage([BlobTrigger("images")] Stream image)
{
     // Process the image
}
```

### <a name="singletonmodelistener"></a>SingletonMode.Listener

Alguns acionadores têm suporte incorporado para gerenciamento de simultaneidade:

* **QueueTrigger** - defina `JobHostConfiguration.Queues.BatchSize` como 1.
* **ServiceBusTrigger** - defina `ServiceBusConfiguration.MessageOptions.MaxConcurrentCalls` como 1.
* **FileTrigger** - defina `FileProcessor.MaxDegreeOfParallelism` como 1.

Pode utilizar estas definições para se certificar de que sua função é executada como um singleton numa única instância. Para garantir que apenas uma única instância da função está em execução quando a aplicação web aumenta horizontalmente para várias instâncias, aplicar um bloqueio de Singleton de nível de serviço de escuta na função (`[Singleton(Mode = SingletonMode.Listener)]`). Na inicialização do JobHost, os bloqueios de serviço de escuta são adquiridos. Se iniciar três instâncias aumentadas horizontalmente todos ao mesmo tempo, apenas uma das instâncias adquire o bloqueio e começa a apenas um serviço de escuta.

### <a name="scope-values"></a>Valores de âmbito

Pode especificar uma **escopo/valor da expressão** no Singleton que garante que todas as execuções da função desse escopo serão serializadas. Implementação de um bloqueio mais granular desta forma pode permitir algum nível de paralelismo para sua função, durante a serialização invocações de outras, conforme ditado pelo seus requisitos. Por exemplo, no exemplo a seguir a expressão de âmbito liga para o `Region` valor de mensagem de entrada. Quando a fila contém mensagens de três em regiões "Leste", "Leste" e "Oeste", respectivamente, em seguida, as mensagens que têm a região "Leste" é executado serialmente enquanto a mensagem com a região "Oeste" é executado em paralelo com as pessoas de "Leste."

```csharp
[Singleton("{Region}")]
public static async Task ProcessWorkItem([QueueTrigger("workitems")] WorkItem workItem)
{
     // Process the work item
}

public class WorkItem
{
     public int ID { get; set; }
     public string Region { get; set; }
     public int Category { get; set; }
     public string Description { get; set; }
}
```

### <a name="singletonscopehost"></a>SingletonScope.Host

O escopo do padrão de um bloqueio for `SingletonScope.Function` que significa que o âmbito de bloqueio (o caminho de concessão do blob) está associado ao nome da função completamente qualificado. Para bloquear em funções, especifique `SingletonScope.Host` e utilizar um nome de ID de âmbito que é o mesmo nos todas as funções que não pretende que sejam executados em simultâneo. No exemplo a seguir, apenas uma instância `AddItem` ou `RemoveItem` é executado por vez:

```csharp
[Singleton("ItemsLock", SingletonScope.Host)]
public static void AddItem([QueueTrigger("add-item")] string message)
{
     // Perform the add operation
}

[Singleton("ItemsLock", SingletonScope.Host)]
public static void RemoveItem([QueueTrigger("remove-item")] string message)
{
     // Perform the remove operation
}
```

### <a name="viewing-lease-blobs"></a>Blobs de concessão de visualização

Utiliza o SDK do WebJobs [concessões de Blobs do Azure](../storage/common/storage-concurrency.md#pessimistic-concurrency-for-blobs) nos bastidores para implementar o bloqueio distribuído. Os blobs de concessão utilizados pelo Singleton podem ser encontrados na `azure-webjobs-host` contentor no `AzureWebJobsStorage` conta de armazenamento no caminho "bloqueios". Por exemplo, o caminho de blob de concessão para os primeiros `ProcessImage` exemplo mostrado anteriormente pode ser `locks/061851c758f04938a4426aa9ab3869c0/WebJobs.Functions.ProcessImage`. Todos os caminhos incluem o ID de JobHost, neste 061851c758f04938a4426aa9ab3869c0 maiúsculas.

## <a name="async-functions"></a>Funções de Async

Para obter informações sobre como as funções do código assíncrono, consulte a documentação de funções do Azure no [Async funções](../azure-functions/functions-dotnet-class-library.md#async).

## <a name="cancellation-tokens"></a>Tokens de cancelamento

Para obter informações sobre como lidar com cancelamento dos tokens, consulte a documentação de funções do Azure no [cancelamento dos tokens e de encerramento correto](../azure-functions/functions-dotnet-class-library.md#cancellation-tokens).

## <a name="multiple-instances"></a>Várias instâncias

Se a sua aplicação web é executado em várias instâncias, é executado um trabalho Web contínuo em cada instância, à escuta de acionadores e funções de chamada. Os vários enlaces de Acionador foram concebidos para compartilhar com eficiência trabalho em colaboração entre instâncias, para que o aumento horizontal para mais instâncias permite-lhe lidar com mais carga.

Os acionadores de fila e blob automaticamente impedir que uma função de processamento de uma mensagem de fila ou blob mais do que uma vez; funções não têm de ser idempotentes.

O acionador de temporizador automaticamente garante que apenas uma instância de execuções de timer, tem mais de uma instância de função em execução num determinado momento agendada.

Se desejar garantir que apenas uma instância de uma função é executada mesmo quando existem várias instâncias da aplicação de web do anfitrião, pode utilizar o [atributo Singleton](#singleton-attribute).

## <a name="filters"></a>Filtros

Filtros de função (pré-visualização) fornecem uma forma de personalizar o pipeline de execução do WebJobs com a sua própria lógica. Filtros são similares [filtros do ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/controllers/filters). Podem ser implementados como atributos declarativos que são aplicados às suas funções ou classes. Para obter mais informações, consulte [filtros de função](https://github.com/Azure/azure-webjobs-sdk/wiki/Function-Filters).

## <a name="logging-and-monitoring"></a>Início de sessão e monitorização

Recomendamos que a arquitetura de registo que foi desenvolvida para o ASP.NET, e o [começar](webjobs-sdk-get-started.md) artigo mostra como utilizá-lo. 

### <a name="log-filtering"></a>Filtragem de log

Cada log criado por um `ILogger` instância se associado ao `Category` e `Level`. [LogLevel](/dotnet/api/microsoft.extensions.logging.loglevel) é uma enumeração, e o código de número inteiro indica a importância relativa:

|LogLevel    |Código|
|------------|---|
|Rastreio       | 0 |
|Depurar       | 1 |
|Informações | 2 |
|Aviso     | 3 |
|Erro       | 4 |
|Crítica    | 5 |
|Nenhuma        | 6 |

Cada categoria pode ser filtrada forma independente para uma determinada [LogLevel](/dotnet/api/microsoft.extensions.logging.loglevel). Por exemplo, pode querer ver todos os registos para o blob acionador processamento, mas apenas `Error` e superior para todo o resto.

#### <a name="version-3x"></a>Versão 3.x

Versão 3.x do SDK depende da filtragem incorporados ao .NET Core. O `LogCategories` classe permite definir categorias para funções específicas, acionadores ou utilizadores. Também define os filtros para os Estados de host específico, tal como `Startup` e `Results`. Dessa forma, pode ajustar a saída de registo. Se nenhuma correspondência for encontrada em categorias definidas, o filtro de reverter para o `Default` valor ao decidir se pretende filtrar a mensagem.

`LogCategories` requer o seguinte usando a instrução:

```cs
using Microsoft.Azure.WebJobs.Logging; 
```

O exemplo seguinte constrói um filtro que, por predefinição, filtra todos os registos no `Warning` nível. Categorias de `Function` ou `results` (equivalente a `Host.Results` na versão 2.x) são filtrados no `Error` nível. O filtro compara a categoria atual para todos os níveis de registados no `LogCategories` de instância e escolhe a correspondência mais longa. Isso significa que o `Debug` nível registado `Host.Triggers` corresponderá `Host.Triggers.Queue` ou `Host.Triggers.Blob`. Isto permite-lhe controlar as categorias mais amplas sem a necessidade de adicionar cada um deles.

```cs
static async Task Main(string[] args)
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
    });
    builder.ConfigureLogging(logging =>
            {
                logging.SetMinimumLevel(LogLevel.Warning);
                logging.AddFilter("Function", LogLevel.Error);
                logging.AddFilter(LogCategories.CreateFunctionCategory("MySpecificFunctionName"),
                    LogLevel.Debug);
                logging.AddFilter(LogCategories.Results, LogLevel.Error);
                logging.AddFilter("Host.Triggers", LogLevel.Debug);
            });
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

#### <a name="version-2x"></a>Versão 2.x

Na versão 2.x do SDK, o `LogCategoryFilter` classe é utilizada para controlar a filtragem. O `LogCategoryFilter` tem uma `Default` propriedade com um valor inicial `Information`, que significa que todas as mensagens com níveis de `Information`, `Warning`, `Error`, ou `Critical` são registadas, mas todas as mensagens com níveis de `Debug` ou `Trace` são filtrados de distância.

Tal como acontece com `LogCategories` na versão 23.x, o `CategoryLevels` propriedade permite-lhe especificar os níveis de registo de categorias específicas, para que possa ajustar a saída de registo. Se nenhuma correspondência for localizada na `CategoryLevels` dicionário, o filtro de reverter para o `Default` valor ao decidir se pretende filtrar a mensagem.

O exemplo seguinte constrói um filtro que, por predefinição, filtra todos os registos no `Warning` nível. Categorias de `Function` ou `Host.Results` são filtrados no `Error` nível. O `LogCategoryFilter` compara a categoria atual para todos os registado `CategoryLevels` e escolhe a correspondência mais longa. Isso significa que o `Debug` nível registado `Host.Triggers` corresponderá `Host.Triggers.Queue` ou `Host.Triggers.Blob`. Isto permite-lhe controlar as categorias mais amplas sem a necessidade de adicionar cada um deles.

```csharp
var filter = new LogCategoryFilter();
filter.DefaultLevel = LogLevel.Warning;
filter.CategoryLevels[LogCategories.Function] = LogLevel.Error;
filter.CategoryLevels[LogCategories.Results] = LogLevel.Error;
filter.CategoryLevels["Host.Triggers"] = LogLevel.Debug;

config.LoggerFactory = new LoggerFactory()
    .AddApplicationInsights(instrumentationKey, filter.Filter)
    .AddConsole(filter.Filter);
```

### <a name="custom-telemetry-for-application-insights"></a>Telemetria personalizada para o Application Insights

A maneira que implementa a telemetria personalizada [Application Insights](../azure-monitor/app/app-insights-overview.md) depende da versão do SDK que está a utilizar. Para saber como configurar o Application Insights, veja [registo de adicionar o Application Insights](webjobs-sdk-get-started.md#add-application-insights-logging).

#### <a name="version-3x"></a>Versão 3.x

Desde a versão 3.x do SDK do WebJobs baseia-se no anfitrião genérico do .NET Core, não há mais uma fábrica de telemetria personalizada fornecida. No entanto, pode adicionar telemetria personalizada para o pipeline com a injeção de dependência. Os exemplos nesta secção exigem o seguinte `using` instruções:

```cs
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.ApplicationInsights.Channel;
```

A seguinte implementação personalizada de [ `ITelemetryInitializer` ] permite-lhe adicionar seus próprios [ `ITelemetry` ](/dotnet/api/microsoft.applicationinsights.channel.itelemetry) para a predefinição [ `TelemetryConfiguration` ].

```cs
internal class CustomTelemetryInitializer : ITelemetryInitializer
{
    public void Initialize(ITelemetry telemetry)
    {
        // Do something with telemetry.
    }
}
```

Chamar [ `ConfigureServices` ] no construtor de adicionar seu personalizada [ `ITelemetryInitializer` ] no pipeline.

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
    });
    builder.ConfigureLogging((context, b) =>
    {
        // Add Logging Providers
        b.AddConsole();

        // If this key exists in any config, use it to enable App Insights
        string appInsightsKey = context.Configuration["APPINSIGHTS_INSTRUMENTATIONKEY"];
        if (!string.IsNullOrEmpty(appInsightsKey))
        {
            // This uses the options callback to explicitly set the instrumentation key.
            b.AddApplicationInsights(o => o.InstrumentationKey = appInsightsKey);
        }
    });
    builder.ConfigureServices(services =>
        {
            services.AddSingleton<ITelemetryInitializer, CustomTelemetryInitializer>();
        });
    var host = builder.Build();
    using (host)
    {

        host.Run();
    }
}
```

Quando o [ `TelemetryConfiguration` ] é construído, registado todos os tipos de [ `ITelemetryInitializer` ] estão incluídos. Para obter mais informações sobre como trabalhar os, consulte [API do Application Insights para métricas e eventos personalizados](../azure-monitor/app/api-custom-events-metrics.md).

Na versão 3.x, deixará de ter que liberar o [ `TelemetryClient` ] quando deixa de anfitrião. O sistema de injeção de dependência do .NET Core descartado automaticamente pelo registrada `ApplicationInsightsLoggerProvider`, que esvaziamentos dos [ `TelemetryClient` ].

#### <a name="version-2x"></a>Versão 2.x

Na versão 2.x, o [ `TelemetryClient` ] criado internamente pelo provedor do Application Insights para o SDK de WebJobs utiliza o [ServerTelemetryChannel](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs). Quando o ponto de final do Application Insights está indisponíveis ou limitação solicitações de entrada, este canal [guarda pedidos no sistema de ficheiros da aplicação web e submete-los novamente mais tarde](https://apmtips.com/blog/2015/09/03/more-telemetry-channels).

O [ `TelemetryClient` ] é criada por uma classe que implementa `ITelemetryClientFactory`. Por predefinição, este é o [ `DefaultTelemetryClientFactory` ](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Logging.ApplicationInsights/DefaultTelemetryClientFactory.cs).

Se pretender modificar qualquer parte do pipeline Application Insights, pode fornecer seus próprios `ITelemetryClientFactory`, e o anfitrião irá utilizar a sua classe para construir um [ `TelemetryClient` ]. Por exemplo, este código substitui o `DefaultTelemetryClientFactory` para modificar uma propriedade do `ServerTelemetryChannel`:

```csharp
private class CustomTelemetryClientFactory : DefaultTelemetryClientFactory
{
    public CustomTelemetryClientFactory(string instrumentationKey, Func<string, LogLevel, bool> filter)
        : base(instrumentationKey, new SamplingPercentageEstimatorSettings(), filter)
    {
    }

    protected override ITelemetryChannel CreateTelemetryChannel()
    {
        ServerTelemetryChannel channel = new ServerTelemetryChannel();

        // change the default from 30 seconds to 15 seconds
        channel.MaxTelemetryBufferDelay = TimeSpan.FromSeconds(15);

        return channel;
    }
}
```

Configura o objeto de SamplingPercentageEstimatorSettings [amostragem adaptável](https://docs.microsoft.com/azure/application-insights/app-insights-sampling#adaptive-sampling-at-your-web-server). Isso significa que, em determinados cenários de volume elevado, o App Insights envia um subconjunto selecionado de dados de telemetria para o servidor.

Depois de criar a fábrica de telemetria, passá-lo para o fornecedor de registo do Application Insights:

```csharp
var clientFactory = new CustomTelemetryClientFactory(instrumentationKey, filter.Filter);

config.LoggerFactory = new LoggerFactory()
    .AddApplicationInsights(clientFactory);
```

## <a id="nextsteps"></a> Passos seguintes

Este guia fornece trechos de código que demonstram como lidar com cenários comuns para trabalhar com o SDK de WebJobs. Para obter exemplos completos, consulte [azure-webjobs-sdk-samples](https://github.com/Azure/azure-webjobs-sdk-samples).

[`ExecutionContext`]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions/Extensions/Core/ExecutionContext.cs
[`TelemetryClient`]: /dotnet/api/microsoft.applicationinsights.telemetryclient
[`ConfigureServices`]: /dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices
['ITelemetryInitializer']: /dotnet/api/microsoft.applicationinsights.extensibility.itelemetryinitializer
[`TelemetryConfiguration`]: /dotnet/api/microsoft.applicationinsights.extensibility.telemetryconfiguration
