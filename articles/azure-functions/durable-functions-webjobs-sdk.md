---
title: Como executar funções duráveis como WebJobs - Azure
description: Aprenda a codificar e configurar funções duráveis para executar na WebJobs com o SDK do WebJobs.
services: functions
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/25/2018
ms.author: azfuncdf
ms.openlocfilehash: 94d443505c5c1634c9da00d455e0163b2762d969
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426039"
---
# <a name="how-to-run-durable-functions-as-webjobs"></a>Como executar funções duráveis como WebJobs

[As funções do Azure](functions-overview.md) e o [funções duráveis](durable-functions-overview.md) extensão são criadas com o [SDK de WebJobs](../app-service/web-sites-create-web-jobs.md). O `JobHost` no SDK do WebJobs é o tempo de execução nas funções do Azure. Se precisar de controle `JobHost` comportamento de formas não é possíveis nas funções do Azure, pode desenvolver e executar funções duráveis com o SDK de WebJobs por conta própria. Em seguida, pode executar as suas funções duráveis na Web do Azure ou é executado em qualquer lugar de um aplicativo de console.

O exemplo de funções duráveis encadeamento está disponível numa versão do SDK do WebJobs: Transfira ou clone a [repositório de funções duráveis](https://github.com/azure/azure-functions-durable-extension/) e navegue para o *exemplos\\webjobssdk\\encadeamento* pasta.

## <a name="prerequisites"></a>Pré-requisitos

Este artigo pressupõe que está familiarizado com os fundamentos básicos do SDK do WebJobs, classe biblioteca desenvolvimento em c# para as funções do Azure e funções duráveis. Se precisar de uma introdução a estes tópicos, consulte os seguintes recursos:

* [Introdução ao SDK do WebJobs](../app-service/webjobs-sdk-get-started.md)
* [Criar a sua primeira função com o Visual Studio](functions-create-your-first-function-visual-studio.md)
* [Funções duráveis](durable-functions-sequence.md)

Para concluir os passos neste artigo:

* [Instale o Visual Studio 2017 versão 15,6 ou posterior](https://docs.microsoft.com/visualstudio/install/) com o **desenvolvimento do Azure** carga de trabalho.

  Se já tiver o Visual Studio, mas não tem essa carga de trabalho, adicione a carga de trabalho selecionando **ferramentas > obter ferramentas e funcionalidades**. 

  (Pode usar [Visual Studio Code](https://code.visualstudio.com/) em vez disso, mas algumas das instruções são específicas para o Visual Studio.)

* Instalar e executar [emulador de armazenamento do Azure](../storage/common/storage-use-emulator.md) versão 5.2 ou posterior. Uma alternativa é atualizar o *App. config* ficheiro com uma cadeia de ligação de armazenamento do Azure.

## <a name="webjobs-sdk-versions"></a>Versões do SDK de WebJobs

Este artigo explica como desenvolver um projeto do SDK do WebJobs 2.x (equivalente as funções do Azure versão 1.x). Para obter informações sobre a versão 3.x, consulte [SDK do WebJobs 3.x](#webjobs-sdk-3x) mais adiante neste artigo. 

## <a name="create-console-app"></a>Criar aplicação de consola

Um projeto do SDK do WebJobs é apenas um projeto de aplicação de consola com os pacotes de NuGet adequados instalado.

No Visual Studio **novo projeto** caixa de diálogo, selecione **ambiente de trabalho clássico do Windows > aplicação de consola (.NET Framework)**. No arquivo de projeto, o `TargetFrameworkVersion` deve ser `v4.6.1`.

O Visual Studio também tem um modelo de projeto do WebJob, que pode utilizar, selecionando **nuvem > WebJob do Azure (.NET Framework)**. Esse modelo instala muitos pacotes, que alguns dos quais poderá não ser necessário.

## <a name="install-nuget-packages"></a>Instalar pacotes NuGet

Precisa de pacotes NuGet para o SDK do WebJobs, enlaces de núcleos, a arquitetura de registo e a extensão de tarefas durável. Seguem-se **Package Manager Console** comandos para esses pacotes, com números de versão estável mais recente a partir da data deste artigo foi escrito:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions -version 2.2.0
Install-Package Microsoft.Extensions.Logging -version 2.0.1
Install-Package Microsoft.Azure.WebJobs.Extensions.DurableTask -version 1.4.0
```

Terá também de provedores de Registro. Os seguintes comandos de instalar o fornecedor do Application Insights e o `ConfigurationManager`. O `ConfigurationManager` permite que obtenha a chave de instrumentação do Application Insights a partir das definições de aplicação.

```powershell
Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -version 2.2.0
Install-Package System.Configuration.ConfigurationManager -version 4.4.1
```

O seguinte comando instala o fornecedor de consola:

```powershell
Install-Package Microsoft.Extensions.Logging.Console -version 2.0.1
```

## <a name="jobhost-code"></a>Código de JobHost

Para utilizar a extensão de funções duráveis, chame `UseDurableTask` sobre o `JobHostConfiguration` objeto em sua `Main` método:

```cs
var config = new JobHostConfiguration();
config.UseDurableTask(new DurableTaskExtension
{
    HubName = "MyTaskHub",
};
```

Para obter uma lista de propriedades que podem ser definidas no `DurableTaskExtension` objeto, consulte [Host. JSON](functions-host-json.md#durabletask).

O `Main` método também é o local onde pode configurar fornecedores de registo. O exemplo seguinte configura a consola e fornecedores do Application Insights.

```cs
static void Main(string[] args)
{
    using (var loggerFactory = new LoggerFactory())
    {
        var config = new JobHostConfiguration();

        config.DashboardConnectionString = "";

        var instrumentationKey =
            ConfigurationManager.AppSettings["APPINSIGHTS_INSTRUMENTATIONKEY"];

        config.LoggerFactory = loggerFactory
            .AddApplicationInsights(instrumentationKey, null)
            .AddConsole();

        config.UseTimers();
        config.UseDurableTask(new DurableTaskExtension
        {
            HubName = "MyTaskHub",
        });
        var host = new JobHost(config);
        host.RunAndBlock();
    }
}
```

## <a name="functions"></a>Funções

Existem algumas diferenças no código escrito para as funções do SDK de WebJobs em comparação comparadas o que escrever para o serviço de funções do Azure.

O WebJobs SDK não suporta as seguintes funcionalidades de funções do Azure:

* [Atributo FunctionName](#functionname-attribute)
* [Acionador HTTP](#http-trigger)
* [API de gestão de funções HTTP durável](#http-management-api)

### <a name="functionname-attribute"></a>Atributo FunctionName

Num projeto do SDK do WebJobs, o nome do método de uma função é o nome de função. O `FunctionName` atributo é utilizado apenas nas funções do Azure.

### <a name="http-trigger"></a>Acionador HTTP

O SDK do WebJobs não tem um acionador HTTP. Cliente de orquestração do projeto de exemplo utiliza um acionador de temporizador:

```cs
public static async Task CronJob(
    [TimerTrigger("0 */2 * * * *")] TimerInfo timer,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger logger)
{
  ...
}
```

### <a name="http-management-api"></a>API de gestão de HTTP

Porque não tem nenhum acionador HTTP, não tem o SDK do WebJobs [API de gestão HTTP](durable-functions-http-api.md).

Num projeto do SDK do WebJobs, pode chamar métodos no objeto de cliente de orquestração, em vez de enviarem pedidos de HTTP. Os métodos seguintes correspondem aos três tarefas que pode fazer com a API de gestão de HTTP:

* `GetStatusAsync`
* `RaiseEventAsync`
* `TerminateAsync`

A função de cliente de orquestração no projeto de exemplo começa a função de orquestrador e, em seguida, entra num loop que chamasse `GetStatusAsync` cada 2 segundos:

```cs
string instanceId = await client.StartNewAsync(nameof(HelloSequence), input: null);
logger.LogInformation($"Started new instance with ID = {instanceId}.");

DurableOrchestrationStatus status;
while (true)
{
    status = await client.GetStatusAsync(instanceId);
    logger.LogInformation($"Status: {status.RuntimeStatus}, Last update: {status.LastUpdatedTime}.");

    if (status.RuntimeStatus == OrchestrationRuntimeStatus.Completed ||
        status.RuntimeStatus == OrchestrationRuntimeStatus.Failed ||
        status.RuntimeStatus == OrchestrationRuntimeStatus.Terminated)
    {
        break;
    }

    await Task.Delay(TimeSpan.FromSeconds(2));
}
```

## <a name="run-the-sample"></a>Executar o exemplo

Esta secção fornece uma visão geral de como executar o [projeto de exemplo](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/webjobssdk/chaining). Para obter instruções detalhadas que explicam como executar um projeto do SDK do WebJobs localmente e implementá-la para um WebJob do Azure, consulte [começar com o SDK de WebJobs](../app-service/webjobs-sdk-get-started.md#deploy-as-a-webjob).

### <a name="run-locally"></a>Executar localmente

1. Certifique-se de que o emulador de armazenamento está em execução (veja [pré-requisitos](#prerequisites)).

1. Se quiser ver registos no Application Insights ao executar localmente:

  a. Criar um recurso do Application Insights, tipo de aplicação **gerais**.

  b. Guardar a chave de instrumentação na *App. config* ficheiro.

1. Execute o projeto.

### <a name="run-in-azure"></a>Executar no Azure

1. Crie uma aplicação web e uma conta de armazenamento.

1. Na aplicação web, guarde a cadeia de ligação de armazenamento numa definição de aplicação com o nome AzureWebJobsStorage.

1. Criar um recurso do Application Insights, tipo de aplicação **gerais**.

1. Guarde a chave de instrumentação numa definição de aplicação com o nome APPINSIGHTS_INSTRUMENTATIONKEY.

1. Implemente como um WebJob.

## <a name="webjobs-sdk-3x"></a>SDK do WebJobs 3.x

A principal alteração introduzida por 3.x é o uso do .NET Core em vez do .NET Framework. Para criar um projeto de 3.x as instruções são os mesmos, com as seguintes exceções:

1. Crie uma aplicação de consola .NET Core. No Visual Studio **novo projeto** caixa de diálogo, selecione **.NET Core > aplicação de consola (.NET Core)**. O ficheiro de projeto Especifica que `TargetFramework` é `netcoreapp2.0`.

1. Escolha a versão de pré-lançamento 3.x dos pacotes seguintes:

  * `Microsoft.Azure.WebJobs.Extensions`
  * `Microsoft.Azure.WebJobs.Logging.ApplicationInsights`

1. Alteração `Main` código de método para obter a cadeia de ligação de armazenamento e a chave de instrumentação do Application Insights de um *appSettings* de ficheiros, usando a estrutura de configuração do .NET Core.  Segue-se um exemplo:

   ```cs
   static void Main(string[] args)
   {
       var builder = new ConfigurationBuilder()
           .SetBasePath(Directory.GetCurrentDirectory())
           .AddJsonFile("appsettings.json");

       var appSettingsConfig = builder.Build();

       using (var loggerFactory = new LoggerFactory())
       {
           var config = new JobHostConfiguration();

           config.DashboardConnectionString = "";
           config.StorageConnectionString = 
               appSettingsConfig.GetConnectionString("AzureWebJobsStorage");
           var instrumentationKey =
               appSettingsConfig["APPINSIGHTS_INSTRUMENTATIONKEY"];

           config.LoggerFactory = loggerFactory
               .AddApplicationInsights(instrumentationKey, null)
               .AddConsole();

           config.UseTimers();
           config.UseDurableTask(new DurableTaskExtension
           {
               HubName = "MyTaskHub",
           });
           var host = new JobHost(config);
           host.RunAndBlock();
       }
   }
   ```

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre o SDK de WebJobs, consulte [como utilizar o SDK do WebJobs](../app-service/webjobs-sdk-how-to.md).

