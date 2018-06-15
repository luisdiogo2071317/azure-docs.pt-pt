---
title: Como executar durável funciona como WebJobs - Azure
description: Saiba como code e configurar funções durável sejam executadas em WebJobs através do SDK de WebJobs.
services: functions
author: tdykstra
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
ms.openlocfilehash: 3fc84d1492d2855ffa3bb5538226da049a928339
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33767323"
---
# <a name="how-to-run-durable-functions-as-webjobs"></a>Como executar durável funciona como WebJobs

[As funções do Azure](functions-overview.md) e [funções durável](durable-functions-overview.md) extensão são criadas no [SDK de WebJobs](../app-service/web-sites-create-web-jobs.md). O `JobHost` o SDK de WebJobs é o tempo de execução das funções do Azure. Se precisar de controlo `JobHost` comportamento de formas não é possíveis nas funções do Azure, pode desenvolver e executar funções duráveis através do SDK de WebJobs por si. Em seguida, pode executar as suas funções duráveis num trabalho Web do Azure ou em qualquer lugar executa uma aplicação de consola.

O exemplo de funções durável encadeamento está disponível uma versão do SDK de WebJobs: transferir ou clonar a [repositório funções durável](https://github.com/azure/azure-functions-durable-extension/) e navegue para o *amostras\\webjobssdk\\encadeamento* pasta.

## <a name="prerequisites"></a>Pré-requisitos

Este artigo pressupõe que está familiarizado com as noções básicas do SDK de WebJobs, c# classe biblioteca programação do Azure e as funções, durável. Se precisar de uma introdução a estes tópicos, consulte os seguintes recursos:

* [Começar com o SDK de WebJobs](../app-service/webjobs-sdk-get-started.md)
* [Criar a sua primeira função com o Visual Studio](functions-create-your-first-function-visual-studio.md)
* [Funções duráveis](durable-functions-sequence.md)

Para concluir os passos neste artigo:

* [Instalar Visual Studio 2017 versão 15.6 ou posterior](https://docs.microsoft.com/visualstudio/install/) com o **programação do Azure** carga de trabalho.

  Se já tiver o Visual Studio, mas não tem que carga de trabalho, adicionar a carga de trabalho selecionando **ferramentas > obter ferramentas e funcionalidades**. 

  (Pode utilizar [Visual Studio Code](https://code.visualstudio.com/) em vez disso, mas algumas das instruções são específicas do Visual Studio.)

* Instalar e executar [emulador do Storage do Azure](../storage/common/storage-use-emulator.md) versão 5.2 ou posterior. É uma alternativa ao atualizar o *App. config* ficheiro com uma cadeia de ligação do Storage do Azure.

## <a name="webjobs-sdk-versions"></a>Versões do SDK de WebJobs

Este artigo explica como desenvolver um projeto de 2. x do SDK de WebJobs (equivalente a versão das funções do Azure 1. x). Para obter informações sobre a versão 3, consulte [SDK de WebJobs 3](#webjobs-sdk-3x) posteriormente neste artigo. 

## <a name="create-console-app"></a>Criar aplicação de consola

Um projeto do SDK de WebJobs é apenas um projeto de aplicação de consola com os pacotes de NuGet adequados instalado.

No Visual Studio **novo projeto** caixa de diálogo, selecione **ambiente de trabalho clássico do Windows > aplicação de consola (.NET Framework)**. No ficheiro de projeto, a `TargetFrameworkVersion` deve ser `v4.6.1`.

Visual Studio também tem um modelo de projeto do trabalho Web, que pode utilizar, selecionando **nuvem > trabalho Web do Azure (.NET Framework)**. Este modelo instala o número de pacotes, que algumas das quais poderá não ser necessário.

## <a name="install-nuget-packages"></a>Instalar pacotes NuGet

Precisa de pacotes NuGet para o SDK de WebJobs, enlaces de core, a arquitetura de registo e a extensão de tarefa durável. Seguem-se **consola do Gestor de pacotes** comandos para esses pacotes, com os números de versão estável mais recente a partir da data este artigo foi escrito:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions -version 2.2.0
Install-Package Microsoft.Extensions.Logging -version 2.0.1
Install-Package Microsoft.Azure.WebJobs.Extensions.DurableTask -version 1.4.0
```

Também precisa de fornecedores de registo. Os seguintes comandos instalar o fornecedor do Application Insights e o `ConfigurationManager`. O `ConfigurationManager` permite-lhe obter a chave de instrumentação do Application Insights das definições de aplicação.

```powershell
Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -version 2.2.0
Install-Package System.Configuration.ConfigurationManager -version 4.4.1
```

O seguinte comando instala o fornecedor de consola:

```powershell
Install-Package Microsoft.Extensions.Logging.Console -version 2.0.1
```

## <a name="jobhost-code"></a>Código de JobHost

Para utilizar a extensão de funções durável, chame `UseDurableTask` no `JobHostConfiguration` objeto na sua `Main` método:

```cs
var config = new JobHostConfiguration();
config.UseDurableTask(new DurableTaskExtension
{
    HubName = "MyTaskHub",
};
```

Para obter uma lista de propriedades que podem ser definidas no `DurableTaskExtension` objeto, consulte [host.json](functions-host-json.md#durabletask).

O `Main` método também é o local para configurar fornecedores de registo. O exemplo a seguir configura a consola e os fornecedores do Application Insights.

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

Existem algumas diferenças no código de escrita para as funções do SDK de WebJobs em comparação comparadas a escrever para o serviço de funções do Azure.

O SDK de WebJobs não suporta as seguintes funcionalidades de funções do Azure:

* [Atributo de FunctionName](#functionname-attribute)
* [Acionador HTTP](#http-trigger)
* [API de gestão de funções HTTP durável](#http-management-api)

### <a name="functionname-attribute"></a>Atributo de FunctionName

Um projeto do SDK de WebJobs, o nome do método de uma função é o nome de função. O `FunctionName` atributo é utilizado apenas das funções do Azure.

### <a name="http-trigger"></a>Acionador HTTP

O SDK de WebJobs não tem um acionador HTTP. Cliente de orquestração do projeto de exemplo utiliza um acionador de temporizador:

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

Porque não tem nenhum acionador HTTP, não tem o SDK de WebJobs [API de gestão de HTTP](durable-functions-http-api.md).

Num projeto SDK de WebJobs, pode chamar os métodos no objeto de cliente de orquestração, em vez de enviarem pedidos de HTTP. Os seguintes métodos de corresponder às três tarefas que pode fazer com a API de gestão de HTTP:

* `GetStatusAsync`
* `RaiseEventAsync`
* `TerminateAsync`

A função de cliente de orquestração no projeto de exemplo inicia a função do orchestrator e, em seguida, entra no ciclo que chama `GetStatusAsync` cada segundos 2:

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

Esta secção fornece uma descrição geral de como executar o [projeto de exemplo](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/webjobssdk/chaining). Para obter instruções detalhadas que explicam como executar um projeto do SDK de WebJobs localmente e implementá-la para um trabalho Web do Azure, consulte [começar com o SDK de WebJobs](../app-service/webjobs-sdk-get-started.md#deploy-as-a-webjob).

### <a name="run-locally"></a>Executar localmente

1. Certifique-se de que o emulador de armazenamento está em execução (consulte [pré-requisitos](#prerequisites)).

1. Se pretender ver os registos no Application Insights ao executar localmente:

  a. Crie um recurso do Application Insights, tipo de aplicação **geral**.

  b. Guardar a chave de instrumentação no *App. config* ficheiro.

1. Execute o projeto.

### <a name="run-in-azure"></a>Executar no Azure

1. Crie uma aplicação web e uma conta de armazenamento.

1. Na aplicação web, guarde a cadeia de ligação de armazenamento com o nome AzureWebJobsStorage uma definição de aplicação.

1. Crie um recurso do Application Insights, tipo de aplicação **geral**.

1. Guarde a chave de instrumentação numa definição de aplicação com o nome APPINSIGHTS_INSTRUMENTATIONKEY.

1. Implemente como um WebJob.

## <a name="webjobs-sdk-3x"></a>SDK de WebJobs 3

A alteração principal que foi introduzida pela 3 é a utilização do .NET Core em vez do .NET Framework. Para criar um projeto de 3 as instruções são os mesmos, com as seguintes exceções:

1. Crie uma aplicação de consola .NET Core. No Visual Studio **novo projeto** caixa de diálogo, selecione **.NET Core > aplicação de consola (.NET Core)**. O ficheiro de projeto Especifica que `TargetFramework` é `netcoreapp2.0`.

1. Escolha a versão de pré-lançamento 3 dos seguintes pacotes:

  * `Microsoft.Azure.WebJobs.Extensions`
  * `Microsoft.Azure.WebJobs.Logging.ApplicationInsights`

2. Alteração `Main` código de método para obter a cadeia de ligação de armazenamento e a chave de instrumentação do Application Insights de um *appsettings.json* ficheiro, utilizando a estrutura de configuração do .NET Core.  Segue-se um exemplo:

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

Para saber mais sobre o SDK de WebJobs, consulte [como utilizar o SDK de WebJobs](../app-service/webjobs-sdk-how-to.md).

