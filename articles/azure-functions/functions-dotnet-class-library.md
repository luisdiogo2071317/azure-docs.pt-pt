---
title: Azure funções c# de referência para programadores
description: Compreenda como desenvolver as funções do Azure com c#.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: funções do azure, funções, processamento de eventos, webhooks, computação dinâmica, arquitetura sem servidor
ms.service: functions
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/12/2017
ms.author: tdykstra
ms.openlocfilehash: 53eaef775647795acf3e8e6fcd127181414b1c0e
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234500"
---
# <a name="azure-functions-c-developer-reference"></a>Azure funções c# de referência para programadores

<!-- When updating this article, make corresponding changes to any duplicate content in functions-reference-csharp.md -->

Este artigo é uma introdução para desenvolver as funções do Azure com c# in bibliotecas de classe do .NET.

As funções do Azure suporta c# e c# script linguagens de programação. Se estiver à procura de documentação de orientação [com c# no portal do Azure](functions-create-function-app-portal.md), consulte [referência para programadores script (.csx) c#](functions-reference-csharp.md).

Este artigo pressupõe que já leu os artigos seguintes:

* [Guia para programadores as funções do Azure](functions-reference.md)
* [Ferramentas do Visual Studio 2017 das funções do Azure](functions-develop-vs.md)

## <a name="functions-class-library-project"></a>As funções de projeto da biblioteca de classe

No Visual Studio, o **das funções do Azure** modelo de projeto cria um classe biblioteca projeto c# que contém os seguintes ficheiros:

* [Host.JSON](functions-host-json.md) -armazena as definições de configuração que afetam todas as funções no projeto ao executar localmente ou no Azure.
* [local.Settings.JSON](functions-run-local.md#local-settings-file) -armazena as definições de aplicação e cadeias de ligação que são utilizadas ao executar localmente.

> [!IMPORTANT]
> O processo de compilação cria um *function.json* ficheiro para cada função. Isto *function.json* ficheiros não se destinar a ser editado diretamente. Não é possível alterar a configuração do enlace ou desativar a função ao editar este ficheiro. Para desativar uma função, utilize o [desativar](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/DisableAttribute.cs) atributo. Por exemplo, adicionar uma aplicação booleana definição MY_TIMER_DISABLED e aplicar `[Disable("MY_TIMER_DISABLED")]` a sua função. Em seguida, pode ativar e desativá-lo alterando a definição de aplicação.

## <a name="methods-recognized-as-functions"></a>Métodos reconhecidos como funções

Na biblioteca de classes, uma função é um método estático com um `FunctionName` e um atributo de Acionador, conforme mostrado no exemplo seguinte:

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
} 
```

O `FunctionName` atributo marca o método como um ponto de entrada da função. O nome tem de ser exclusivos dentro de um projeto, começar com uma letra e conter apenas letras, números, `_` e `-`, até 127 carateres de comprimento. Modelos de projeto, muitas vezes, criam um método denominado `Run`, mas o nome do método pode ser qualquer nome de método do c# válido.

O atributo de Acionador Especifica o tipo de Acionador e está vinculado a dados de entrada para um parâmetro de método. A função de exemplo é acionada por uma mensagem de fila, e a mensagem da fila é transferida para o método de `myQueueItem` parâmetro.

## <a name="method-signature-parameters"></a>Parâmetros de assinatura de método

A assinatura de método pode conter parâmetros diferente do utilizado com o atributo de Acionador. Seguem-se alguns dos parâmetros adicionais que pode incluir:

* [Entrada e saída enlaces](functions-triggers-bindings.md) marcados como tal, decorating-los com os atributos.  
* Um `ILogger` ou `TraceWriter` parâmetro [registo](#logging).
* A `CancellationToken` parâmetro [encerramento correto](#cancellation-tokens).
* [Expressões de enlace](functions-triggers-bindings.md#binding-expressions-and-patterns) parâmetros para acionam metadados.

Não importa a ordem dos parâmetros de assinatura da função. Por exemplo, pode colocar os parâmetros de Acionador antes ou depois noutros enlaces e pode colocar o parâmetro de registo antes ou depois de parâmetros de enlace ou acionador.

### <a name="output-binding-example"></a>Exemplo de saída de enlace

O exemplo seguinte modifica um anterior ao adicionar um enlace de fila de saída. A função escreve a mensagem da fila que aciona a função para uma nova mensagem da fila numa fila diferente.

```csharp
public static class SimpleExampleWithOutput
{
    [FunctionName("CopyQueueMessage")]
    public static void Run(
        [QueueTrigger("myqueue-items-source")] string myQueueItem, 
        [Queue("myqueue-items-destination")] out string myQueueItemCopy,
        TraceWriter log)
    {
        log.Info($"CopyQueueMessage function processed: {myQueueItem}");
        myQueueItemCopy = myQueueItem;
    }
}
```

Os artigos de referência de enlace ([as filas de armazenamento](functions-bindings-storage-queue.md), por exemplo) explicam os tipos de parâmetros que pode utilizar com o acionador, entrada ou saída vínculo de atributos.

### <a name="binding-expressions-example"></a>Exemplo de expressões de enlace

O código seguinte obtém o nome da fila de monitorização a partir de uma definição de aplicação e obtém o tempo de criação de mensagem da fila `insertionTime` parâmetro.

```csharp
public static class BindingExpressionsExample
{
    [FunctionName("LogQueueMessage")]
    public static void Run(
        [QueueTrigger("%queueappsetting%")] string myQueueItem,
        DateTimeOffset insertionTime,
        TraceWriter log)
    {
        log.Info($"Message content: {myQueueItem}");
        log.Info($"Created at: {insertionTime}");
    }
}
```

## <a name="autogenerated-functionjson"></a>Function.json gerado automaticamente

O processo de compilação cria um *function.json* ficheiros numa pasta função na pasta de compilação. Conforme indicado anteriormente, este ficheiro não se destinar a ser editado diretamente. Não é possível alterar a configuração do enlace ou desativar a função ao editar este ficheiro. 

O objetivo deste ficheiro é fornecer informações para o controlador de escala a utilizar para [dimensionamento decisões no plano de consumo](functions-scale.md#how-the-consumption-plan-works). Por este motivo, o ficheiro tem apenas informações de Acionador, não de entrada ou saída de enlaces.

Gerado *function.json* ficheiro inclui um `configurationSource` propriedade que indica o tempo de execução para utilizar atributos do .NET para enlaces, vez *function.json* configuração. Segue-se um exemplo:

```json
{
  "generatedBy": "Microsoft.NET.Sdk.Functions-1.0.0.0",
  "configurationSource": "attributes",
  "bindings": [
    {
      "type": "queueTrigger",
      "queueName": "%input-queue-name%",
      "name": "myQueueItem"
    }
  ],
  "disabled": false,
  "scriptFile": "..\\bin\\FunctionApp1.dll",
  "entryPoint": "FunctionApp1.QueueTrigger.Run"
}
```

## <a name="microsoftnetsdkfunctions"></a>Microsoft.NET.Sdk.Functions

O *function.json* geração do ficheiro é realizada o pacote NuGet [Microsoft\.NET\.Sdk\.funções](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions). 

O mesmo pacote é utilizado para versões de 1. x e 2 do tempo de execução de funções. O framework de destino é que um projeto de 1. x diferencia de a um projeto de 2. x. Seguem-se as partes relevantes do *. csproj* de destino de ficheiros, que mostra diferentes estruturas e o mesmo `Sdk` pacote:

**As funções 1. x**

```xml
<PropertyGroup>
  <TargetFramework>net461</TargetFramework>
</PropertyGroup>
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Sdk.Functions" Version="1.0.8" />
</ItemGroup>
```

**Funciona 2. x**

```xml
<PropertyGroup>
  <TargetFramework>netstandard2.0</TargetFramework>
  <AzureFunctionsVersion>v2</AzureFunctionsVersion>
</PropertyGroup>
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Sdk.Functions" Version="1.0.8" />
</ItemGroup>
```

Entre o `Sdk` as dependências de pacote são acionadores e enlaces. Um projeto de 1. x refere-se 1. x acionadores e enlaces porque os o .NET Framework de destino enquanto 2 acionadores e enlaces .NET Core de destino.

O `Sdk` pacote também depende [newtonsoft](http://www.nuget.org/packages/Newtonsoft.Json)e indiretamente no [Windowsazure](http://www.nuget.org/packages/WindowsAzure.Storage). Estas dependências Certifique-se de que o seu projeto utiliza as versões desses pacotes que funcionam com a versão de runtime de funções que os destinos de projeto. Por exemplo, `Newtonsoft.Json` tem versão 11 para .NET Framework 4.6.1, mas só é compatível com o tempo de execução de funções que vise o .NET Framework 4.6.1 `Newtonsoft.Json` 9.0.1. Para o seu código de função no projeto de que também tem de utilizar `Newtonsoft.Json` 9.0.1.

O código de origem para `Microsoft.NET.Sdk.Functions` está disponível no repositório GitHub [azure\-funções\-vs\-criar\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).

## <a name="runtime-version"></a>Versão de runtime

Visual Studio utiliza o [ferramentas de núcleos de funções do Azure](functions-run-local.md#install-the-azure-functions-core-tools) para executar os projetos de funções. As ferramentas de Core é uma interface de linha de comandos para o tempo de execução de funções.

Se instalar as ferramentas de núcleos utilizando npm, que não afeta a versão de ferramentas principal utilizada pelo Visual Studio. Para a versão de runtime funções 1. x, o Visual Studio armazena versões principais ferramentas no *%USERPROFILE%\AppData\Local\Azure.Functions.Cli* e utiliza a versão mais recente armazenada não existe. Para funções 2. x, as ferramentas de núcleos são incluídas no **das funções do Azure e ferramentas de trabalhos Web** extensão. Para o 1. x e 2. x, pode ver a versão que está a ser utilizada o resultado da consola ao executar um projeto de funções:

```terminal
[3/1/2018 9:59:53 AM] Starting Host (HostId=contoso2-1518597420, Version=2.0.11353.0, ProcessId=22020, Debug=False, Attempt=0, FunctionsExtensionVersion=)
```

## <a name="supported-types-for-bindings"></a>Tipos suportados para enlaces

Cada enlace tem os seus próprios tipos suportados; Por exemplo, um atributo de Acionador de blob pode ser aplicado a um parâmetro de cadeia, um parâmetro POCO um `CloudBlockBlob` parâmetro ou qualquer um dos vários outros tipos suportados. O [artigo de referência de enlace para enlaces de blob](functions-bindings-storage-blob.md#trigger---usage) tipos de parâmetro apresenta uma lista de todos os suportado. Para obter mais informações, consulte [Acionadores e enlaces](functions-triggers-bindings.md) e [docs de referência de enlace para cada tipo de enlace](functions-triggers-bindings.md#next-steps).

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="binding-to-method-return-value"></a>Enlace para o valor de retorno do método

Pode utilizar um valor de retorno do método para um enlace de saída, aplicando o atributo para o valor de retorno do método. Para obter exemplos, consulte [Acionadores e enlaces](functions-triggers-bindings.md#using-the-function-return-value).

## <a name="writing-multiple-output-values"></a>Escrita de vários valores de saída

Para escrever valores múltiplos para um enlace de saída, utilize o [ `ICollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) ou [ `IAsyncCollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) tipos. Estes tipos são coleções de só de escrita que são escritas para o enlace de saída quando o método for concluída.

Neste exemplo escreve vários de fila de mensagens em fila o mesmo, utilizando `ICollector`:

```csharp
public static class ICollectorExample
{
    [FunctionName("CopyQueueMessageICollector")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-3")] string myQueueItem,
        [Queue("myqueue-items-destination")] ICollector<string> myQueueItemCopy,
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
        myQueueItemCopy.Add($"Copy 1: {myQueueItem}");
        myQueueItemCopy.Add($"Copy 2: {myQueueItem}");
    }
}
```

## <a name="logging"></a>Registo

Para iniciar a sessão de saída para os registos de transmissão em fluxo em c#, incluir um argumento de tipo `TraceWriter`. Recomendamos que o nome `log`. Evite utilizar `Console.Write` nas funções do Azure. 

`TraceWriter` está definido no [SDK de WebJobs do Azure](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs). O nível de registo para `TraceWriter` podem ser configuradas no [host.json](functions-host-json.md).

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
} 
```

> [!NOTE]
> Para obter informações sobre uma arquitetura de registo mais recente que pode utilizar em vez de `TraceWriter`, consulte [registos de escrita em c# funções](functions-monitoring.md#write-logs-in-c-functions) no **das funções do Azure do Monitor** artigo.

## <a name="async"></a>Async

Para tornar uma função [assíncrona](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/), utilize o `async` palavra-chave e devolver um `Task` objeto.

```csharp
public static class AsyncExample
{
    [FunctionName("BlobCopy")]
    public static async Task RunAsync(
        [BlobTrigger("sample-images/{blobName}")] Stream blobInput,
        [Blob("sample-images-copies/{blobName}", FileAccess.Write)] Stream blobOutput,
        CancellationToken token,
        TraceWriter log)
    {
        log.Info($"BlobCopy function processed.");
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
}
```

## <a name="cancellation-tokens"></a>Tokens de cancelamento

Uma função pode aceitar um [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) parâmetro, que permite que o sistema operativo notificar o seu código quando a função está prestes a ser terminada. Pode utilizar esta notificação para se certificar de que a função não terminar inesperadamente de uma forma que mantém os dados num estado inconsistente.

O exemplo seguinte mostra como procurar iminente terminação de função.

```csharp
public static class CancellationTokenExample
{
    public static void Run(
        [QueueTrigger("inputqueue")] string inputText,
        TextWriter logger,
        CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(5000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }
}
```

## <a name="environment-variables"></a>Variáveis de ambiente

Para obter uma variável de ambiente ou uma valor de definição de aplicação, utilize `System.Environment.GetEnvironmentVariable`, conforme mostrado no exemplo de código seguinte:

```csharp
public static class EnvironmentVariablesExample
{
    [FunctionName("GetEnvironmentVariables")]
    public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
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
}
```

O [System.Configuration.ConfigurationManager.AppSettings](https://docs.microsoft.com/en-us/dotnet/api/system.configuration.configurationmanager.appsettings) propriedade é uma API alternativa para obter os valores de definição de aplicação, mas recomendamos que utilize `GetEnvironmentVariable` conforme mostrado aqui.

## <a name="binding-at-runtime"></a>Enlace no tempo de execução

Em c# e outras linguagens .NET, pode utilizar um [imperativo](https://en.wikipedia.org/wiki/Imperative_programming) enlace padrão, como opposed para o [ *declarativa* ](https://en.wikipedia.org/wiki/Declarative_programming) enlaces de atributos. Enlace imperativo é útil quando os parâmetros de enlace tem de ser calculada ao tempo de tempo de execução, em vez de design. Com este padrão, é possível vincular suportada de entrada e saída enlaces no-a-momento no seu código de função.

Defina um imperativo enlace da seguinte forma:

- **Não** incluem um atributo na assinatura da função para os enlaces imperativo pretendidos.
- Passagem de um parâmetro de entrada [ `Binder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) ou [ `IBinder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- Utilize o seguinte c# padrão para efetuar o enlace de dados.

  ```cs
  using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
  {
      ...
  }
  ```

  `BindingTypeAttribute` é o atributo de .NET que define o enlace e `T` é um tipo de entrada ou de saída que é suportado por esse tipo de enlace. `T` não pode ser um `out` tipo de parâmetro (tais como `out JObject`). Por exemplo, a tabela de Mobile Apps saída enlace suporta [seis tipos de saída](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), mas só pode utilizar [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) ou [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs)com imperativo enlace.

### <a name="single-attribute-example"></a>Exemplo de atributo

O código de exemplo seguinte cria um [vínculo de saída de blob de armazenamento](functions-bindings-storage-blob.md#output) com BLOBs caminho definido em tempo de execução, em seguida, escreve uma cadeia de blob.

```cs
public static class IBinderExample
{
    [FunctionName("CreateBlobUsingBinder")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-4")] string myQueueItem,
        IBinder binder,
        TraceWriter log)
    {
        log.Info($"CreateBlobUsingBinder function processed: {myQueueItem}");
        using (var writer = binder.Bind<TextWriter>(new BlobAttribute(
                    $"samples-output/{myQueueItem}", FileAccess.Write)))
        {
            writer.Write("Hello World!");
        };
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) define o [blob de armazenamento](functions-bindings-storage-blob.md) entrada ou saída de enlace, e [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) é um tipo de enlace de saída suportado.

### <a name="multiple-attribute-example"></a>Exemplo de atributo vários

O exemplo anterior obtém a definição de aplicação para a cadeia de ligação para a aplicação de função principal armazenamento conta (que é `AzureWebJobsStorage`). Pode especificar uma definição de aplicação personalizada a utilizar para a conta de armazenamento, adicionando o [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) e transmitir a matriz de atributo para `BindAsync<T>()`. Utilize um `Binder` parâmetro, não `IBinder`.  Por exemplo:

```cs
public static class IBinderExampleMultipleAttributes
{
    [FunctionName("CreateBlobInDifferentStorageAccount")]
    public async static Task RunAsync(
            [QueueTrigger("myqueue-items-source-binder2")] string myQueueItem,
            Binder binder,
            TraceWriter log)
    {
        log.Info($"CreateBlobInDifferentStorageAccount function processed: {myQueueItem}");
        var attributes = new Attribute[]
        {
        new BlobAttribute($"samples-output/{myQueueItem}", FileAccess.Write),
        new StorageAccountAttribute("MyStorageAccount")
        };
        using (var writer = await binder.BindAsync<TextWriter>(attributes))
        {
            await writer.WriteAsync("Hello World!!");
        }
    }
}
```

## <a name="triggers-and-bindings"></a>Acionadores e enlaces 

[!INCLUDE [Supported triggers and bindings](../../includes/functions-bindings.md)]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre os acionadores e enlaces](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Saiba mais sobre as melhores práticas para as funções do Azure](functions-best-practices.md)
