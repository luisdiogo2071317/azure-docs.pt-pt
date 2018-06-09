---
title: Como utilizar o SDK de WebJobs do Azure
description: Saiba mais sobre como escrever código para o SDK de WebJobs. Crie condicionada por eventos em segundo plano as tarefas de processamento que acedem aos dados nos serviços de terceiros e serviços do Azure.
services: app-service\web, storage
documentationcenter: .net
author: tdykstra
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/27/2018
ms.author: tdykstra
ms.openlocfilehash: 08272ba7d828f744336723f25b482bf06b9e43dc
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234655"
---
# <a name="how-to-use-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Como utilizar o SDK de WebJobs do Azure para o processamento em segundo plano condicionada por eventos

Este artigo fornece orientação sobre como escrever código para [o SDK de WebJobs do Azure](webjobs-sdk-get-started.md). A documentação se aplica a versões 2 e 3, exceto se encontra onde indicado em contrário. A alteração principal que foi introduzida pela 3 é a utilização do .NET Core em vez do .NET Framework.

>[!NOTE]
> [As funções do Azure](../azure-functions/functions-overview.md) está incorporada no SDK de WebJobs e ligações neste artigo para documentação de funções do Azure para alguns tópicos. Tenha em atenção as seguintes diferenças entre as funções e o SDK de WebJobs:
> * Versão de funções do Azure, 1. x corresponde à versão do SDK de WebJobs 2. x e as funções do Azure 2 corresponde ao SDK de WebJobs 3. Repositórios de código de origem siga o SDK de WebJobs numeração e tem muitas ramificações v2.x, com o ramo principal atualmente com o código de 3.
> * Código de exemplo para bibliotecas de classes do Azure funções c# é como o SDK de WebJobs code exceto não precisa de um `FunctionName` atributo num projeto do SDK de WebJobs.
> * Alguns tipos de enlace só são suportados nas funções, como HTTP, webhook e grelha de evento (o que é baseado em HTTP). 
> 
> Para obter mais informações, consulte [comparar o SDK de WebJobs e as funções do Azure](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs). 

## <a name="prerequisites"></a>Pré-requisitos

Este artigo pressupõe que leu [começar com o SDK de WebJobs](webjobs-sdk-get-started.md).

## <a name="jobhost"></a>JobHost

O `JobHost` objeto é o contentor de runtime para funções: escuta acionadores e chamadas de funções. Criar o `JobHost` no seu código e o código de escrita para personalizar o comportamento.

Esta é uma chave diferença entre utilizar o SDK de WebJobs diretamente e utilizá-la indiretamente através das funções do Azure. Nas funções do Azure, os controlos de serviço a `JobHost`, e não é possível personalizá-lo ao escrever código. As funções do Azure permite-lhe personalizar o comportamento de anfitrião através das definições no *host.json* ficheiro. Essas definições são cadeias, não código, que limita os tipos de personalizações, pode fazê-lo.

### <a name="jobhost-connection-strings"></a>Cadeias de ligação de JobHost

O SDK de WebJobs procura cadeias de ligação de armazenamento e de barramento de serviço no *local.settings.json* quando executa localmente ou num ambiente de trabalho da Web quando executado no Azure. Se pretender utilizar os seus próprios nomes para estas cadeias de ligação ou armazene-lo noutro local, pode defini-los no código, conforme mostrado aqui:

```cs
static void Main(string[] args)
{
    var _storageConn = ConfigurationManager
        .ConnectionStrings["MyStorageConnection"].ConnectionString;

    var _dashboardConn = ConfigurationManager
        .ConnectionStrings["MyDashboardConnection"].ConnectionString;

    JobHostConfiguration config = new JobHostConfiguration();
    config.StorageConnectionString = _storageConn;
    config.DashboardConnectionString = _dashboardConn;
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

### <a name="jobhost-development-settings"></a>Definições de desenvolvimento JobHost

O `JobHostConfiguration` classe tem um `UseDevelopmentSettings` método que pode ser chamada para tornar o desenvolvimento local mais eficiente. Seguem-se algumas das definições de que este método é alterado:

| Propriedade | Definição de desenvolvimento |
| ------------- | ------------- |
| `Tracing.ConsoleLevel` | `TraceLevel.Verbose` para maximizar a saída de registo. |
| `Queues.MaxPollingInterval`  | Um valor baixo para garantir que os métodos de fila são acionados imediatamente.  |
| `Singleton.ListenerLockPeriod` | 15 segundos para ajudar a um desenvolvimento rápido iterativo. |

O exemplo seguinte mostra como utilizar as definições de desenvolvimento. Para tornar `config.IsDevelopment` devolver `true` ao executar localmente, definir uma variável de ambiente local com o nome `AzureWebJobsEnv` com valor `Development`.

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

### <a name="jobhost-servicepointmanager-settings"></a>JobHost ServicePointManager definições

O .NET Framework contém uma chamada de API [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) que controla o número de ligações simultâneas a um anfitrião. Recomendamos que aumente este valor da predefinição de 2 antes de iniciar o anfitrião de WebJobs.

Enviados todos os pedidos HTTP que efetua uma função utilizando `HttpClient` fluir pelo `ServicePointManager`. Depois de clicar no `DefaultConnectionLimit`, a `ServicePointManager` inicia pedidos de colocação antes de os enviar. Suponha que o `DefaultConnectionLimit` está definida como 2 e o seu código torna 1.000 os pedidos de HTTP. Inicialmente, apenas 2 pedidos são realmente autorizada através do SO. Os outros 998 são colocados em fila até que não há espaço para os mesmos. Isto significa que o `HttpClient` poderá tempo limite, porque- *pensa* efetuou o pedido, mas o pedido nunca foi enviado pelo sistema operativo para o servidor de destino. Para que poderá ver o comportamento que parece não fazer sentido: local `HttpClient` está a demorar 10 segundos para concluir um pedido, mas o serviço está a devolver todos os pedidos em 200 ms. 

O valor predefinido para aplicações ASP.NET é `Int32.MaxValue`, e que é provável que funcionam bem para WebJobs em execução num plano de serviço de aplicações básico ou superior. WebJobs normalmente, tem a definição Always On e que só é suportada por planos de serviço de aplicações básico e planos superiores. 

Se estiver a executar o trabalho Web num gratuito ou partilhado plano do App Service, a aplicação é restringida pelo sandbox do serviço de aplicações, que tem atualmente um [limite de ligação de 300](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#per-sandbox-per-appper-site-numerical-limits). Com um limite de ligação desvinculado em `ServicePointManager`, é mais provável que irá ser atingido o limiar de ligação da sandbox e encerrar o site. Nesse caso, a definição `DefaultConnectionLimit` para algo inferior, como 50 ou 100, pode impedir que isto aconteça e ainda permitir débito suficiente.

A definição tem de ser configurada antes de serem efetuados quaisquer pedidos HTTP. Por este motivo, o anfitrião de WebJobs não deve tentar ajustar a definição automaticamente; é possível que existam pedidos de HTTP que ocorrem antes do anfitrião inicia e isto pode levar a um comportamento inesperado. É a melhor abordagem para definir o valor imediatamente no seu `Main` método antes de inicializar o `JobHost`, conforme mostrado no exemplo seguinte

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

As funções tem de ser métodos públicos e tem de ter um atributo de Acionador ou [NoAutomaticTrigger](#manual-trigger) atributo.

### <a name="automatic-trigger"></a>Início automático

Acionadores automáticas chamar uma função em resposta a um evento. Por exemplo, consulte o acionador de fila no [artigo introdução Get](webjobs-sdk-get-started.md).

### <a name="manual-trigger"></a>Acionador manual

Para acionar manualmente uma função, utilize o `NoAutomaticTrigger` atributo, conforme mostrado no exemplo seguinte:

```cs
static void Main(string[] args)
{
    JobHost host = new JobHost();
    host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
}
```

```cs
[NoAutomaticTrigger]
public static void CreateQueueMessage(
    TextWriter logger,
    string value,
    [Queue("outputqueue")] out string message)
{
    message = value;
    logger.WriteLine("Creating queue message: ", message);
}
```

## <a name="input-and-output-bindings"></a>Entrada e saída enlaces

Enlaces de entrada fornecem uma forma declarativa de disponibilizar dados do Azure ou serviços de terceiros para o seu código. Enlaces de saída proporcionam uma forma de atualizar os dados. O [artigo introdução Get](webjobs-sdk-get-started.md) mostra um exemplo de cada.

Pode utilizar um valor de retorno do método para um enlace de saída, aplicando o atributo para o valor de retorno do método. Consulte o exemplo nas funções do Azure [Acionadores e enlaces](../azure-functions/functions-triggers-bindings.md#using-the-function-return-value) artigo.

## <a name="binding-types"></a>Tipos de enlace

Os seguintes tipos de Acionador e enlace estão incluídos no `Microsoft.Azure.WebJobs` pacote:

* Armazenamento de blobs
* Armazenamento de filas
* Table Storage

Para utilizar outros acionador e tipos de enlace, instale o pacote NuGet que contém-los e chamar um `Use<binding>` método no `JobHostConfiguration` objeto. Por exemplo, se pretender utilizar um acionador de temporizador, instale `Microsoft.Azure.WebJobs.Extensions` e chame `UseTimers` no `Main` método, tal como neste exemplo:

```cs
static void Main()
{
    config = new JobHostConfiguration();
    config.UseTimers();
    var host = new JobHost(config);
    host.RunAndBlock();
}
```

Pode encontrar o pacote para instalar para um tipo de enlace específico a **pacotes** secção desse tipo de enlace [artigo de referência](#binding-reference-information) para as funções do Azure. Uma exceção é o acionador de ficheiros e o enlace (para o sistema de ficheiros local), que não é suportado das funções do Azure. Para utilizar os ficheiros de enlace, instalar `Microsoft.Azure.WebJobs.Extensions` e chame `UseFiles`.

### <a name="usecore"></a>UseCore

O `Microsoft.Azure.WebJobs.Extensions` pacote mencionado anteriormente também fornece um tipo de enlace especial que pode registar, chamando o `UseCore` método. Este enlace permite-lhe definir um [ExecutionContext](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Core/ExecutionContext.cs) parâmetro na sua assinatura da função. O objeto de contexto dá-lhe acesso ao ID de invocação e que pode utilizar para correlacionar todos os registos produzidos por uma invocação de função especificada. Segue-se um exemplo:

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

## <a name="binding-configuration"></a>Configuração de enlace

Muitas acionam e tipos de enlace permitem-lhe configurar o respetivo comportamento definindo as propriedades no objeto de configuração que passa para o `JobHost`.

### <a name="queue-trigger-configuration"></a>Configuração de Acionador de fila

As definições que pode configurar para o acionador de fila de armazenamento são explicados com as funções do Azure [host.json referência](../azure-functions/functions-host-json.md#queues). How to set-los num projeto do SDK de WebJobs como é mostrado no exemplo seguinte:

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

### <a name="configuration-for-other-bindings"></a>Configuração para noutros enlaces

Alguns tipos de Acionador e enlace definem os seus próprios o tipo de configuração personalizada. Por exemplo, o acionador de ficheiro permite-lhe especificar o caminho de raiz para monitorizar:

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

## <a name="binding-expressions"></a>Expressões de enlace

Nos parâmetros do construtor de atributo, pode utilizar expressões de resolver para valores de várias origens. Por exemplo, no seguinte código, o caminho para o `BlobTrigger` atributo cria uma expressão com o nome `filename`. Quando utilizado para o enlace de saída, `filename` é resolvido para o nome do blob acionadora.
 
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

Para obter mais informações sobre as expressões de enlace, consulte [expressões e padrões de enlace](../azure-functions/functions-triggers-bindings.md#binding-expressions-and-patterns) na documentação de funções do Azure.

### <a name="custom-binding-expressions"></a>Expressões de enlace personalizado

Por vezes, pretende especificar um nome de fila, um nome de blob ou contentor ou nome de uma tabela no código em vez de codificá-lo. Por exemplo, poderá pretender especificar o nome da fila para o `QueueTrigger` atributo numa variável de ambiente ou ficheiro de configuração.

Pode fazê-lo mediante a transmissão num `NameResolver` de objeto para o `JobHostConfiguration` objeto. Incluir marcadores de posição no acionador ou parâmetros de construtor de atributo de enlace e os seus `NameResolver` código fornece os valores reais para ser utilizado em vez dos marcadores de posição. Os marcadores de posição são identificados pelo envolvente-los com sinais de sinal de percentagem (), conforme mostrado no exemplo seguinte:
 
```cs
public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
{
    Console.WriteLine(logMessage);
}
```

Este código permite-lhe utilizar uma fila com o nome logqueuetest no ambiente de teste e uma logqueueprod nomeado em produção. Em vez de um nome de fila hard-coded, especifique o nome de uma entrada no `appSettings` coleção. 

Há uma predefinição NameResolver que entra em vigor se não fornecer um personalizado. A predefinição obtém os valores de definições da aplicação ou variáveis de ambiente.

O `NameResolver` classe obtém o nome da fila de `appSettings` conforme mostrado no exemplo seguinte:

```cs
public class CustomNameResolver : INameResolver
{
    public string Resolve(string name)
    {
        return ConfigurationManager.AppSettings[name].ToString();
    }
}
```

Transmita o `NameResolver` de classe para o `JobHost` objeto conforme mostrado no exemplo seguinte:

```cs
 static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.NameResolver = new CustomNameResolver();
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

Implementa de funções do Azure `INameResolver` para obter os valores das definições de aplicação, conforme mostrado no exemplo. Quando utiliza o SDK de WebJobs diretamente, pode escrever uma implementação personalizada que obtém os valores de substituição de marcador de posição de qualquer origem preferir. 

## <a name="binding-at-runtime"></a>Enlace no tempo de execução

Se precisar de fazer alguns cálculos na sua função antes de o utilizar como um atributo de enlace `Queue`, `Blob`, ou `Table`, pode utilizar o `IBinder` interface.

O exemplo seguinte utiliza uma mensagem de fila de entrada e cria uma nova mensagem com o mesmo conteúdo de uma fila de saída. O nome da fila de saída está definido por código no corpo da função.

```cs
public static void CreateQueueMessage(
    [QueueTrigger("inputqueue")] string queueMessage,
    IBinder binder)
{
    string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
    QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
    CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
    outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
}
```

Para obter mais informações, consulte [enlace no tempo de execução](../azure-functions/functions-dotnet-class-library.md#binding-at-runtime) na documentação de funções do Azure.

## <a name="binding-reference-information"></a>Informações de referência de enlace

Informações de referência sobre cada tipo de enlace são fornecidas na documentação de funções do Azure. Utilizar fila de armazenamento como exemplo, encontrará as seguintes informações em cada artigo de referência de enlace:

* [Pacotes](../azure-functions/functions-bindings-storage-queue.md#packages---functions-1x) -o pacote de instalação para incluir suporte para o enlace num projeto do SDK de WebJobs.
* [Exemplos](../azure-functions/functions-bindings-storage-queue.md#trigger---example) -exemplo de biblioteca o c# classe aplica-se para o SDK de WebJobs; apenas omitir o `FunctionName` atributo.
* [Atributos](../azure-functions/functions-bindings-storage-queue.md#trigger---attributes) -os atributos a utilizar para o tipo de enlace.
* [Configuração](../azure-functions/functions-bindings-storage-queue.md#trigger---configuration) -explicações das propriedades de atributo e os parâmetros do construtor.
* [Utilização](../azure-functions/functions-bindings-storage-queue.md#trigger---usage) - que tipos podem vincular ao e informações sobre como funciona o enlace. Por exemplo: algoritmo de consulta, poison processamento de fila.
  
Para obter uma lista dos artigos de referência de enlace, consulte **suportados enlaces** no [Acionadores e enlaces](../azure-functions/functions-triggers-bindings.md#supported-bindings) artigo para as funções do Azure. Em que lista os enlaces de HTTP, webhook e grelha de evento são suportados apenas por funções do Azure, não para o SDK de WebJobs.

## <a name="disable-attribute"></a>Desativar o atributo 

O [desativar](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/DisableAttribute.cs) permite atributo controlar se uma função pode ser acionada. 

No exemplo seguinte, se a definição de aplicação `Disable_TestJob` tem um valor de "1" ou "True" (sensível), a função não será executado. Nesse caso, o tempo de execução cria uma mensagem de registo *função 'Functions.TestJob' está desativada*.

```cs
[Disable("Disable_TestJob")]
public static void TestJob([QueueTrigger("testqueue2")] string message)
{
    Console.WriteLine("Function with Disable attribute executed!");
}
```

Quando altera os valores de definição de aplicação no portal do Azure, faz com que o trabalho Web seja reiniciado, escolha se a nova definição.

O atributo pode ser declarado no parâmetro, método ou nível de classe. O nome da definição também pode conter expressões de enlace.

## <a name="timeout-attribute"></a>Atributo de tempo limite

O [tempo limite](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TimeoutAttribute.cs) atributo faz com que uma função terá de ser cancelada se não concluir a dentro de um período de tempo especificado. No exemplo seguinte, iria executar a função durante um dia sem o tempo limite. Com o tempo limite, a função, será cancelada após 15 segundos.

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

Pode aplicar o atributo de tempo limite ao nível de classe ou método e pode especificar um tempo limite global utilizando `JobHostConfiguration.FunctionTimeout`. Tempos limite de nível de classe ou método substituam o tempo limite global.

## <a name="singleton-attribute"></a>Atributo de singleton

Utilize o [Singleton](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/SingletonAttribute.cs) atributo para se certificar de que apenas uma instância de uma função é executado mesmo quando existem múltiplas instâncias da aplicação de web do anfitrião. Fazê-lo implementando [distribuídas bloqueio](#viewing-lease-blobs).

No exemplo seguinte, apenas uma única instância de `ProcessImage` função é executada em qualquer momento:

```cs
[Singleton]
public static async Task ProcessImage([BlobTrigger("images")] Stream image)
{
     // Process the image
}
```

### <a name="singletonmodelistener"></a>SingletonMode.Listener

Alguns acionadores têm suporte incorporado para gestão de concorrência:

* **QueueTrigger** – configure `JobHostConfiguration.Queues.BatchSize` para 1.
* **ServiceBusTrigger** – configure `ServiceBusConfiguration.MessageOptions.MaxConcurrentCalls` para 1.
* **FileTrigger** – configure `FileProcessor.MaxDegreeOfParallelism` para 1.

Pode utilizar estas definições para se certificar de que a sua função é executada como singleton numa única instância. Para garantir que apenas uma única instância da função está em execução quando a aplicação web aumenta horizontalmente de forma a várias instâncias, aplicar um bloqueio de Singleton de nível de serviço de escuta da função (`[Singleton(Mode = SingletonMode.Listener)]`). São adquirir bloqueios de serviço de escuta no arranque do JobHost. Se iniciar três instâncias de escalamento horizontal, todos os ao mesmo tempo, apenas uma das instâncias adquire o bloqueio e começa a apenas um serviço de escuta.

### <a name="scope-values"></a>Valores de âmbito

Pode especificar um **âmbito/valor da expressão** no Singleton que irá garantir que todas as execuções de função no âmbito de que irão ser serializadas. Implementar mais granular bloquear desta forma pode permitir algum nível de paralelismo para a sua função, ao serializar outras invocações conforme ditado pelos seus requisitos. Por exemplo, no exemplo a seguir a expressão de âmbito está vinculada ao `Region` valor da mensagem a receber. Se a fila contém 3 mensagens no regiões "Leste", "Leste" e "Oeste" respetivamente, em seguida, as mensagens com a região "Leste" executado serialmente enquanto a mensagem com a região "Oeste" será executado em paralelo com os.

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

O âmbito da predefinição de um bloqueio é `SingletonScope.Function` que significa que o âmbito de bloqueio (o caminho de concessão de blob) está associado ao nome de função completamente qualificado. Para bloquear em funções, especifique `SingletonScope.Host` e utilizar um nome de ID de âmbito é igual em todas as funções que não pretende que sejam executados em simultâneo. No exemplo seguinte, apenas uma instância de `AddItem` ou `RemoveItem` executa cada vez:

```charp
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

Utiliza o SDK de WebJobs [concessões de Blobs do Azure](../storage/common/storage-concurrency.md#pessimistic-concurrency-for-blobs) nos bastidores para implementar o bloqueio distribuída. Os blobs de concessão utilizados pelo Singleton podem ser encontrados no `azure-webjobs-host` contentor no `AzureWebJobsStorage` conta de armazenamento no caminho "bloqueia". Por exemplo, o caminho de blob de concessão para o primeiro `ProcessImage` exemplo mostrado anteriormente poderá ser `locks/061851c758f04938a4426aa9ab3869c0/WebJobs.Functions.ProcessImage`. Todos os caminhos incluem o ID de JobHost, num 061851c758f04938a4426aa9ab3869c0 cenário.

## <a name="async-functions"></a>Async funções

Para obter informações sobre como as funções de assíncrona de código, consulte a documentação de funções do Azure no [Async funções](../azure-functions/functions-dotnet-class-library.md#async).

## <a name="cancellation-tokens"></a>Tokens de cancelamento

Para obter informações sobre como processar os tokens de cancelamento, consulte a documentação de funções do Azure no [tokens de cancelamento e encerramento correto](../azure-functions/functions-dotnet-class-library.md#cancellation-tokens).

## <a name="multiple-instances"></a>Várias instâncias

Se a sua aplicação web é executada em várias instâncias, é executado um WebJob contínuo em cada instância, à escuta de acionadores e chamar funções. Os vários enlaces de Acionador foram concebidos para partilha de forma eficiente trabalho em equipa em instâncias, para que como aumentar horizontalmente mais instâncias permite-lhe para processar carga mais.

Os acionadores de fila e blob automaticamente impedem que uma função de processar uma mensagem de fila ou blob mais do que uma vez; as funções tem de ser idempotent.

O acionador de temporizador assegura que apenas uma instância de execuções de temporizador, automaticamente, pelo que não a obter mais do que uma instância de função em execução num determinado momento agendada.

Se pretender certificar-se de que apenas uma instância de uma função é executado mesmo quando existem múltiplas instâncias da aplicação web anfitrião, pode utilizar o [Singleton](#singleton) atributo.
    
## <a name="filters"></a>Filtros 

Filtros de função (pré-visualização) fornecem uma forma de personalizar o pipeline de execução de WebJobs com o seus próprios lógica. Os filtros são semelhantes às [ASP.NET Core filtros](https://docs.microsoft.com/aspnet/core/mvc/controllers/filters). Estes podem ser implementadas como atributos declarativos que são aplicados às suas funções ou classes. Para obter mais informações, consulte [filtros de função](https://github.com/Azure/azure-webjobs-sdk/wiki/Function-Filters).

## <a name="logging-and-monitoring"></a>Início de sessão e monitorização

Recomendamos a arquitetura de registo que foi desenvolvida para ASP.NET, e o [começar](webjobs-sdk-get-started.md) artigo mostra como utilizá-la. 

### <a name="log-filtering"></a>Filtragem de registo

Cada registo criado por um `ILogger` instância tem um associados `Category` e `Level`. [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel#Microsoft_Extensions_Logging_LogLevel) é uma enumeração, e o código de número inteiro indica a importância relativa:

|LogLevel    |Código|
|------------|---|
|Rastreio       | 0 |
|Depurar       | 1 |
|Informações | 2 |
|Aviso     | 3 |
|Erro       | 4 |
|Crítica    | 5 |
|Nenhuma        | 6 |

Cada categoria que pode ser filtrada independentemente a um determinado [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel). Por exemplo, pode querer ver todos os registos para o blob acionador processamento, mas apenas `Error` e superior para tudo o resto.

Para tornar mais fácil especificar regras de filtragem, o SDK de WebJobs fornece o `LogCategoryFilter` que pode ser transmitido muitos dos fornecedores de registo existente, incluindo o Application Insights e a consola.

O `LogCategoryFilter` tem um `Default` propriedade com um valor inicial do `Information`, que significa que as mensagens com níveis de `Information`, `Warning`, `Error`, ou `Critical` sessão iniciada, mas todas as mensagens com níveis de `Debug` ou `Trace` são filtrados ausente.

O `CategoryLevels` propriedade permite-lhe especificar os níveis de registo de categorias específicas, pelo que pode ajustar a saída de registo. Não se for encontrada nenhuma correspondência dentro de `CategoryLevels` dicionário, o filtro retrocede para o `Default` valor ao decidir se pretende filtrar a mensagem.

O exemplo seguinte constrói um filtro que, por predefinição, filtra todos os registos no `Warning` nível. Categorias de `Function` ou `Host.Results` são filtrados no `Error` nível. O `LogCategoryFilter` compara a categoria atual para todas as registadas `CategoryLevels` e escolhe a correspondência mais longo. Isto significa que o `Debug` nível registado para `Host.Triggers` corresponderá `Host.Triggers.Queue` ou `Host.Triggers.Blob`. Isto permite-lhe controlar mais ampla categorias sem necessidade de adicionar cada um deles.

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

Internamente, o `TelemetryClient` criado pelo fornecedor de Application Insights para o SDK de WebJobs utiliza o [ServerTelemetryChannel](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs). Quando o ponto final do Application Insights está indisponíveis ou limitação pedidos recebidos, este canal [guarda pedidos no sistema de ficheiros da aplicação web e resubmits-los mais tarde](http://apmtips.com/blog/2015/09/03/more-telemetry-channels).

O `TelemetryClient` é criada por uma classe que implementa `ITelemetryClientFactory`. Por predefinição, este é o [DefaultTelemetryClientFactory](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Logging.ApplicationInsights/DefaultTelemetryClientFactory.cs).

Se pretender modificar qualquer parte do pipeline do Application Insights, pode fornecer as suas próprias `ITelemetryClientFactory`, e o anfitrião irá utilizar a classe para construir uma `TelemetryClient`. Por exemplo, este código substitui o `DefaultTelemetryClientFactory` modificar uma propriedade do `ServerTelemetryChannel`:

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

Configura o objeto de SamplingPercentageEstimatorSettings [amostragem adaptável](https://docs.microsoft.com/azure/application-insights/app-insights-sampling#adaptive-sampling-at-your-web-server). Isto significa que, em certos cenários de volume elevado, App Insights envia um subconjunto selecionado dos dados de telemetria para o servidor.

Assim que tiver criado a fábrica de telemetria, passa-se para o fornecedor de registo do Application Insights:

```csharp
var clientFactory = new CustomTelemetryClientFactory(instrumentationKey, filter.Filter);

config.LoggerFactory = new LoggerFactory()
    .AddApplicationInsights(clientFactory);
```

## <a id="nextsteps"></a> Passos seguintes

Este guia forneceu fragmentos de código que demonstram como lidar com cenários comuns para trabalhar com o SDK de WebJobs. Para exemplos completos, consulte [azure-webjobs-sdk-samples](https://github.com/Azure/azure-webjobs-sdk-samples).