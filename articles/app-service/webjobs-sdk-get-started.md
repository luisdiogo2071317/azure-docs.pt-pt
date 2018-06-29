---
title: Introdução ao Azure WebJobs SDK
description: Introdução ao SDK de WebJobs para processamento em segundo plano condicionada por eventos. Saiba como aceder a dados nos serviços de terceiros e serviços do Azure.
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
ms.openlocfilehash: 6cdcfbf16f77525d6cee2085fdde9fdd630027e3
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096684"
---
# <a name="get-started-with-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Introdução ao SDK de WebJobs do Azure para o processamento em segundo plano condicionada por eventos

Este artigo mostra como criar um projeto do SDK de WebJobs do Azure, executá-la localmente e implementá-la para o App Service do Azure.

As instruções são para [Visual Studio 2017](https://www.visualstudio.com/vs/), mas as mesmas tarefas podem ser realizadas com outras ferramentas, tal como [Visual Studio Code](https://code.visualstudio.com/).

## <a name="what-is-the-azure-webjobs-sdk"></a>O que é o Azure WebJobs SDK

O SDK de WebJobs do Azure é uma arquitetura que simplifica a tarefa de escrever o código de processamento em segundo plano que acede aos dados nos serviços do Azure. O SDK inclui uma sintaxe declarativa para especificar os eventos devem acionar uma função, como uma nova mensagem adicionada a uma fila. Sintaxe declarativa semelhante controla ler e escrever dados depois de uma função foi acionada. Este sistema de acionadores e enlaces encarrega-se da maioria das tarefas de codificação nível baixo associadas ao aceder ao Azure e os serviços de terceiros.

### <a name="functions-triggers-and-bindings"></a>As funções, acionadores e enlaces

Um projeto do SDK de WebJobs define um ou mais *funções*. Uma função é um método que tem um atributo de Acionador na respetiva assinatura do método. Acionadores especificam as condições para chamar uma função, e enlaces que de leitura e escrita. Por exemplo, o atributo de Acionador a seguinte função indica o tempo de execução para chamar a função sempre que é apresentada uma mensagem de fila no `items` fila. O `Blob` atributo indica o tempo de execução para utilizar a mensagem da fila para ler um blob no *workitems* contentor. O conteúdo da mensagem de fila &mdash; fornecidas a `queueTrigger` parâmetro &mdash; é o nome do blob.

```cs
public static void Run(
    [QueueTrigger("items")] string myQueueItem,
    [Blob("workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    TraceWriter log)
{
    log.Info($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

### <a name="versions-2x-and-3x"></a>Versões de 2 e 3

As instruções indicam como criar um projeto de 2. x do SDK de WebJobs versão, com notas sobre o que é diferente para 3 (em pré-visualização). A alteração principal que foi introduzida pela 3 é a utilização do .NET Core em vez do .NET Framework.

> [!NOTE]
> 3 está em pré-visualização e está em desenvolvimento e as instruções de 3 neste artigo estão Desatualizadas. Para informações mais recentes, consulte os problemas do GitHub na parte inferior da página.

### <a name="azure-functions"></a>Funções do Azure

[As funções do Azure](../azure-functions/functions-overview.md) baseiam o SDK de WebJobs e é uma opção quando não precisa de utilizar o SDK de WebJobs diretamente. Azure funções 1. x utiliza o SDK de WebJobs 2. x. Para obter mais informações, consulte [comparação entre as funções do Azure e o SDK de WebJobs](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs).

## <a name="prerequisites"></a>Pré-requisitos

Este artigo pressupõe que tem [uma conta do Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) e experiência com [apps no App Service do Azure](app-service-web-overview.md). Para concluir os passos neste artigo:

* [Instalar Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/) com o **programação do Azure** carga de trabalho. Se já tiver o Visual Studio, mas não tem que carga de trabalho, adicionar a carga de trabalho selecionando **ferramentas > obter ferramentas e funcionalidades**.
* [Criar uma aplicação do app Service](app-service-web-get-started-dotnet-framework.md). Se já tiver um que pode implementar um WebJob para, pode utilizar que em vez de criar um novo.

## <a name="create-a-project"></a>Criar um projeto

1. No Visual Studio, selecione **ficheiro > novo projeto**.

2. Selecione **ambiente de trabalho clássico do Windows > aplicação (.NET Framework) de consola**.

   Para criar um projeto de 3, selecione **.NET Core > aplicação de consola (.NET Core)**.
   
   > [!NOTE]
   > 3 está em pré-visualização e está em desenvolvimento e as instruções de 3 neste artigo estão Desatualizadas. Para informações mais recentes, consulte os problemas do GitHub na parte inferior da página.

3. Nomeie o projeto *WebJobsSDKSample*e, em seguida, selecione **OK**.

   ![Caixa de diálogo Novo Projeto](./media/webjobs-sdk-get-started/new-project.png)

## <a name="add-webjobs-nuget-package"></a>Adicionar o pacote WebJobs NuGet

1. Instale a versão mais recente do estável 2 do pacote NuGet `Microsoft.Azure.WebJobs`. (Para o SDK de WebJobs 3, deverá escolher a versão mais recente de 3.)
 
   Eis o **consola do Gestor de pacotes** comando para a versão 2.2.0:

   ```powershell
   Install-Package Microsoft.Azure.WebJobs -version 2.2.0
   ``` 

## <a name="create-the-jobhost"></a>Criar o JobHost

O `JobHost` objeto é o contentor de runtime para funções: escuta acionadores e chamadas de funções. 

1. No *Program.cs*, adicione um `using` instrução:

   ```cs
   using Microsoft.Azure.WebJobs;
   ```

1. Substitua o método `Main` pelo código abaixo:

   ```cs
   static void Main()
   {
       var config = new JobHostConfiguration();
       var host = new JobHost(config);
       host.RunAndBlock();
   }
   ```

## <a name="enable-console-logging"></a>Ativar o registo de consola

Existem várias opções para o registo no projeto do SDK de WebJobs. Um Recomendamos é o [arquitetura de registo que foi desenvolvida para ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging). Esta estrutura oferece um melhor desempenho e mais flexibilidade na filtragem e de suporte de armazenamento. 

Nesta secção, iremos configurar o registo de consola que utiliza a nova arquitetura.

1. Instale a versão estável mais recente dos pacotes de NuGet seguintes:

   * `Microsoft.Extensions.Logging` -A arquitetura de registo.
   * `Microsoft.Extensions.Logging.Console` -Consola *fornecedor*. Um fornecedor envia os registos para um destino específico, neste caso, para a consola. 
 
   Seguem-se a **consola do Gestor de pacotes** comandos para a versão 2.0.1:

   ```powershell
   Install-Package Microsoft.Extensions.Logging -version 2.0.1
   ``` 

   ```powershell
   Install-Package Microsoft.Extensions.Logging.Console -version 2.0.1
   ``` 

2. No *Program.cs*, adicione um `using` instrução:

   ```cs
   using Microsoft.Extensions.Logging;
   ```

3. No `Main` método, adicione o código para atualizar o `JobHostConfiguration` antes de criar o `JobHost`:
 
   ```
   config.DashboardConnectionString = "";
   var loggerFactory = new LoggerFactory();
   config.LoggerFactory = loggerFactory
       .AddConsole();
   ```

   Este código torna as seguintes alterações:

   * Desativa [registo dashboard](https://github.com/Azure/azure-webjobs-sdk/wiki/Queues#logs). O dashboard é um ferramenta de monitorização de legado e o registo de dashboard não é recomendado para cenários de produção de débito elevado.
   * Adiciona o fornecedor de consola com predefinição [filtragem](webjobs-sdk-how-to.md#log-filtering). 

   O `Main` método agora este aspeto:

   ```
   var config = new JobHostConfiguration();
   config.DashboardConnectionString = "";
   var loggerFactory = new LoggerFactory();
   config.LoggerFactory = loggerFactory
       .AddConsole();
   var host = new JobHost(config);
   host.RunAndBlock();
   ```
   
## <a name="create-a-function"></a>Criar uma função

1. Criar *Functions.cs* na pasta do projeto e substitua o código de modelo com este código:

   ```cs
   using Microsoft.Azure.WebJobs;
   using Microsoft.Azure.WebJobs.Host;
   using Microsoft.Extensions.Logging;

   namespace WebJobsSDKSample
   {
       public class Functions
       {
           public static void ProcessQueueMessage([QueueTrigger("queue")] string message, ILogger logger)
           {
               logger.LogInformation(message);
           }
       }
   }
   ```

   O `QueueTrigger` atributo indica o tempo de execução para chamar esta função quando uma nova mensagem de erro é escrita numa fila de armazenamento do Azure chamada `queue`. O conteúdo da mensagem de fila é fornecido para o código do método no `message` parâmetro. O corpo do método é onde pode processa os dados de Acionador. Neste exemplo, o código regista apenas a mensagem.

   O `message` parâmetro não tem de ser uma cadeia. É também possível vincular a um objeto JSON, uma matriz de bytes, ou um [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage) objeto. [Ver utilização de Acionador de fila](../azure-functions/functions-bindings-storage-queue.md#trigger---usage). Cada tipo de enlace (por exemplo, filas, blobs, tabelas ou) tem um conjunto diferente de tipos de parâmetro que é possível vincular a.

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento

O emulador de armazenamento do Azure que é executada localmente não tem todas as funcionalidades que tem o SDK de WebJobs. Por isso, nesta secção criar uma conta de armazenamento no Azure e configurar o projeto para utilizá-lo.

1. Abra **Explorador de servidores** e inicie sessão no Azure. Clique com botão direito do **Azure** nó e, em seguida, selecione **ligar à subscrição do Microsoft Azure**.

   ![Iniciar sessão no Azure](./media/webjobs-sdk-get-started/sign-in.png)

1. Sob o **Azure** no nó **Explorador de servidores**, faça duplo clique **armazenamento**e, em seguida, selecione **conta de armazenamento criar**.

   ![Criar o menu da conta de armazenamento](./media/webjobs-sdk-get-started/create-storage-account-menu.png)

2. No **criar conta de armazenamento** caixa de diálogo, introduza um nome exclusivo para a conta de armazenamento.

3. Selecione a mesma **região** que criou a aplicação de serviço de aplicações no, ou uma região fechar para si.

1. Selecione **Criar**.

   ![Criar conta de armazenamento](./media/webjobs-sdk-get-started/create-storage-account.png)

1. Sob o **armazenamento** no nó **Explorador de servidores**, selecione a nova conta de armazenamento. No **propriedades** janela, selecione o botão de reticências (**...** ) no lado direito do **cadeia de ligação** campo valor.

   ![Botão de reticências de cadeia de ligação](./media/webjobs-sdk-get-started/conn-string-ellipsis.png)

2. Copie a cadeia de ligação e guarde este valor algures que pode copiá-lo novamente de imediato.

   ![Copie a cadeia de ligação](./media/webjobs-sdk-get-started/copy-key.png)

## <a name="configure-storage-for-running-locally"></a>Configurar o armazenamento para executar localmente

O SDK de WebJobs procura a cadeia de ligação de armazenamento na coleção de definições da aplicação. Quando executar localmente, procuram este valor no *App. config* variáveis de ambiente ou ficheiro.

1. Adicione o seguinte XML para o *App. config* ficheiro, imediatamente após a abertura `<configuration>` etiquetas.

   ```xml
   <connectionStrings>
     <add name="AzureWebJobsStorage" connectionString="{storage connection string}" />
   </connectionStrings>
   ```

2. Substitua *{cadeia de ligação de armazenamento}* com a cadeia de ligação que copiou anteriormente.

   Mais tarde utilizará a cadeia de ligação novamente, ao configurar a aplicação de serviço de aplicações no Azure.

## <a name="test-locally"></a>Testar localmente

Nesta secção, criar e executar localmente o projeto e acionar a função através da criação de uma mensagem de fila.

1. Prima Ctrl + F5 para executar o projeto.

   A consola mostra que o tempo de execução encontrar a função e está à espera da fila de mensagens acioná-lo.

   ```console
   Found the following functions:
   WebJobsSDKSample.Functions.ProcessQueueMessage
   info: Host.Startup[0]
         Found the following functions:
         WebJobsSDKSample.Functions.ProcessQueueMessage
   Job host started
   info: Host.Startup[0]
         Job host started
   ```

   Poderá ver uma mensagem de aviso um `ServicePointManager` definição. Para testar que irá fazer com este projeto, pode ignorar o aviso. Para obter mais informações sobre o aviso, consulte [como utilizar o SDK de WebJobs](webjobs-sdk-how-to.md#jobhost-servicepointmanager-settings).

2. Feche a janela de consola.

1. No **Explorador de servidores**, expanda o nó para a sua nova conta de armazenamento e, em seguida, clique com botão direito **filas**. 

2. Selecione **criar fila**. 

3. Introduza *fila* como o nome para a fila e, em seguida, selecione **OK**.

   ![Criar fila](./media/webjobs-sdk-get-started/create-queue.png)

4. O nó para a nova fila com o botão direito e, em seguida, selecione **vista fila**.

5. Selecione o **Adicionar mensagem** ícone.

   ![Criar fila](./media/webjobs-sdk-get-started/create-queue-message.png)

6. No **Adicionar mensagem** caixa de diálogo, introduza *Olá, mundo!* como o **texto da mensagem**e, em seguida, selecione **OK**.

   ![Criar fila](./media/webjobs-sdk-get-started/hello-world-text.png)

7. Execute novamente o projeto.

   Porque é utilizado o `QueueTrigger` atributo no `ProcessQueueMessage` função, o tempo de execução do WeJobs SDK escuta fila de mensagens quando iniciar. Encontrar uma nova mensagem da fila na fila com o nome *fila* e chama a função.

   Devido a [fila consulta término exponencial](../azure-functions/functions-bindings-storage-queue.md#trigger---polling-algorithm), poderá demorar até 2 minutos para o tempo de execução localizar a mensagem e invocar a função. Este tempo de espera, pode ser reduzido através da execução no [modo de desenvolvimento](webjobs-sdk-how-to.md#jobhost-development-settings).

  O resultado da consola tem o seguinte aspeto:

   ```console
   Found the following functions:
   WebJobsSDKSample.Functions.ProcessQueueMessage
   info: Host.Startup[0]
         Found the following functions:
         WebJobsSDKSample.Functions.ProcessQueueMessage
   Job host started
   info: Host.Startup[0]
         Job host started
   Executing 'Functions.ProcessQueueMessage' (Reason='New queue message detected on 'queue'.', Id=ebcb275d-0d7c-4293-a1af-93e0804b9e49)
   info: Function[0]
         Hello World!
   info: Host.Results[0]
         Executed 'Functions.ProcessQueueMessage' (Succeeded, Id=ebcb275d-0d7c-4293-a1af-93e0804b9e49)
   Executed 'Functions.ProcessQueueMessage' (Succeeded, Id=ebcb275d-0d7c-4293-a1af-93e0804b9e49)
   ```

8. Feche a janela de consola.

## <a name="add-application-insights-logging"></a>Adicionar o registo do Application Insights

Quando executa o projeto no Azure, não é possível monitorizar a execução de função visualizando o resultado da consola. A solução de monitorização que recomendamos é [Application Insights](../application-insights/app-insights-overview.md). Para obter mais informações, consulte [das funções do Azure do Monitor](../azure-functions/functions-monitoring.md).

Nesta secção, efetue as seguintes tarefas para configurar o Application Insights registo antes de implementar no Azure:

* Certifique-se de que tem uma aplicação de serviço de aplicações e uma instância do Application Insights para trabalhar com.
* Configure a aplicação do app Service para utilizar a instância do Application Insights e a conta de armazenamento que criou anteriormente.
* Configure o projeto para o registo para o Application Insights.

### <a name="create-app-service-app-and-application-insights-instance"></a>Criar aplicação de serviço de aplicações e a instância do Application Insights

1. Se ainda não tiver uma aplicação de serviço de aplicações que pode utilizar, [criar um](app-service-web-get-started-dotnet-framework.md).

2. Se ainda não tiver um recurso do Application Insights que pode utilizar, [criar um](../application-insights/app-insights-create-new-resource.md). Definir **tipo de aplicação** para **geral**, utilizar e ignorar as secções que se seguem **copiar a chave de instrumentação**.

3. Se já tiver um recurso do Application Insights que pretende utilizar, [copiar a chave de instrumentação](../application-insights/app-insights-create-new-resource.md#copy-the-instrumentation-key).

### <a name="configure-app-settings"></a>Configurar as definições da aplicação 

1. No **Explorador de servidores**, expanda o **do serviço de aplicações** nó **Azure**.

1. Expanda o grupo de recursos que a aplicação de serviço de aplicações está a ser e, em seguida, clique com botão direito a aplicação de serviço de aplicações.

3. Selecione **ver definições**.

4. No **cadeias de ligação** caixa, adicione a seguinte entrada.

   |Nome  |Cadeia de ligação  |Tipo de Base de Dados|
   |---------|---------|------|
   |AzureWebJobsStorage | {Armazenamento cadeia de ligação que copiou anteriormente}|Personalizado|
   
6. Se o **definições da aplicação** caixa não tem uma instrumentação do Application Insights da chave, adicione um que copiou anteriormente. (A chave de instrumentação pode já estar aqui, consoante a forma como que criou a aplicação de serviço de aplicações.)

   |Nome  |Valor  |
   |---------|---------|
   |APPINSIGHTS_INSTRUMENTATIONKEY | {chave de instrumentação} |

2. Substitua *{chave de instrumentação}* com a chave de instrumentação do recurso Application Insights que estiver a utilizar.

2. Selecione **Guardar**.

1. Adicione o seguinte XML para o *App. config* ficheiro, imediatamente após a recolha de cadeias de ligação.

   ```xml
   <appSettings>
     <add key="APPINSIGHTS_INSTRUMENTATIONKEY" value="{instrumentation key}" />
   </appSettings>
   ```

2. Substitua *{chave de instrumentação}* com a chave de instrumentação do recurso Application Insights que estiver a utilizar.

   Adicionar estes dados para o *App. config* ficheiro permite-lhe testar a ligação do Application Insights ao executar o projeto localmente. 

3. Guarde as alterações.

### <a name="add-application-insights-logging-provider"></a>Adicione o fornecedor de registo do Application Insights

1. Instalar a versão mais recente do estável 2 do pacote NuGet para o fornecedor de registo do Application Insights: `Microsoft.Azure.WebJobs.Logging.ApplicationInsights`. (Para o SDK de WebJobs 3, escolha a versão 3 mais recente do pacote.)

   Eis o **consola do Gestor de pacotes** comando para a versão 2.2.0:

   ```powershell
   Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -version 2.2.0
   ``` 

1. Instalar a versão 4. x estável mais recente do pacote NuGet para o Gestor de configuração do .NET: `System.Configuration.ConfigurationManager`.

   Eis o **consola do Gestor de pacotes** comando para a versão 4.4.1:

   ```powershell
   Install-Package System.Configuration.ConfigurationManager -version 4.4.1
   ``` 

2. Abra *Program.cs* e adicione um `using` instrução para o configuration manager:

   ```csharp
   using System.Configuration;
   ```

2. Substitua o código no `Main` método com o seguinte código:

   ```csharp
   using (var loggerFactory = new LoggerFactory())
   {
       var config = new JobHostConfiguration();
       var instrumentationKey =
           ConfigurationManager.AppSettings["APPINSIGHTS_INSTRUMENTATIONKEY"];
       config.DashboardConnectionString = "";
       config.LoggerFactory = loggerFactory
           .AddApplicationInsights(instrumentationKey, null)
           .AddConsole();
       var host = new JobHost(config);
       host.RunAndBlock();
   }
   ```

   Este código torna as seguintes alterações:

   * Adiciona um fornecedor de registo do Application Insights com predefinição [filtragem](webjobs-sdk-how-to.md#log-filtering); todas as informações e os registos de nível superiores agora passará para a consola e o Application Insights quando estiver a executar localmente. 
   * Coloca o `LoggerFactory` objeto um `using` bloco para garantir que a saída de registo é descarregada quando sai do anfitrião. 

## <a name="test-application-insights-logging"></a>Registo do Application Insights de teste

Nesta secção, executar localmente novamente para verificar que dados de registo agora vai Application Insights, bem como a consola.

1. Utilize **Explorador de servidores** para criar uma mensagem de fila, da mesma forma fez [anterior](#trigger-the-function), exceto introduza *Hello App Insights!* como o texto da mensagem.

1. Execute o projeto.

   O SDK de WebJobs processa a mensagem da fila e consulte os registos na janela da consola.

1. Feche a janela de consola.

1. Abra o [portal do Azure](https://portal.azure.com/)e vá para o recurso do Application Insights.

2. Selecione **pesquisa**.

   ![Selecione a pesquisa](./media/webjobs-sdk-get-started/select-search.png)

1. Se não vir o *Hello App Insights!* mensagem, selecione **atualizar** periodicamente durante vários minutos. (Os registos não aparecem imediatamente porque, demora algum tempo para o cliente do Application Insights esvaziar os registos processa.)

   ![Registos no Application Insights](./media/webjobs-sdk-get-started/logs-in-ai.png)

3. Feche a janela de consola.

## <a name="deploy-as-a-webjob"></a>Implementar como um trabalho Web

Nesta secção implementar o projeto como um WebJob. Implementar uma aplicação de serviço de aplicações que [criado anteriormente](#create-app-service-app-and-application-insights-instance). Para testar o seu código enquanto é executada no Azure, irá acionar uma invocação de função através da criação de uma mensagem de fila.

1. No **Explorador de soluções**, com o botão direito no projeto e, em seguida, selecione **publicar como trabalho Web do Azure**.

1. No **trabalho Web do Azure de adicionar** caixa de diálogo, selecione **OK**.

   ![Adicionar o trabalho Web do Azure](./media/webjobs-sdk-get-started/add-azure-webjob.png)

   Visual Studio instala automaticamente um pacote NuGet para publicação do trabalho Web.

1. No **perfil** passo o **publicar** assistente, selecione **App Service do Microsoft Azure**.

   ![Caixa de diálogo Publicar](./media/webjobs-sdk-get-started/publish-dialog.png)

1. No **do serviço de aplicações** caixa de diálogo, selecione **o grupo de recursos > a aplicação de serviço de aplicações**e, em seguida, selecione **OK**.

   ![Caixa de diálogo App Service](./media/webjobs-sdk-get-started/app-service-dialog.png)

1. No **ligação** passo do assistente, selecione **publicar**.

## <a name="trigger-the-function-in-azure"></a>Acionar a função no Azure

1. Certifique-se de que não está a executar localmente (fechar a janela de consola, se estiver aberto). Caso contrário, a instância local poderá ser o primeiro a processar as mensagens de fila que cria.

1. Utilize **Explorador de servidores** para criar uma mensagem de fila, da mesma forma fez [anterior](#trigger-the-function), exceto introduza *Olá, Azure!*.

7. Atualize o **fila** página no Visual Studio e a nova mensagem desapareceu porque a função em execução no App Service do Azure processada-lo.

   > [!TIP]
   > Quando estiver a testar no Azure, utilize [modo de desenvolvimento](webjobs-sdk-how-to.md#jobhost-development-settings) para garantir que uma função de Acionador de fila é invocada imediato e evitar atrasos devido a [fila consulta término exponencial](../azure-functions/functions-bindings-storage-queue.md#trigger---polling-algorithm).

### <a name="view-logs-in-application-insights"></a>Ver registos no Application Insights

1. Abra o [portal do Azure](https://portal.azure.com/)e vá para o recurso do Application Insights.

2. Selecione **pesquisa**.

1. Se não vir o *Olá, Azure!* mensagem, selecione **atualizar** periodicamente durante vários minutos.

   Consulte os registos da função em execução num WebJob, incluindo o *Olá, Azure!* texto que introduziu na secção anterior.

## <a name="add-an-input-binding"></a>Adicionar um enlace de entrada

Enlaces de entrada simplificam o código que lê dados. Neste exemplo, a mensagem da fila será um nome de blob e irá utilizar o nome do blob para encontrar e ler um blob no Storage do Azure.

1. No *Functions.cs*, substitua o `ProcessQueueMessage` método com o seguinte código:

   ```cs
   public static void ProcessQueueMessage(
       [QueueTrigger("queue")] string message,
       [Blob("container/{queueTrigger}", FileAccess.Read)] Stream myBlob,
       ILogger logger)
   {
       logger.LogInformation($"Blob name:{message} \n Size: {myBlob.Length} bytes");
   }
   ```

   Neste código, `queueTrigger` é um [expressão de enlace](../azure-functions/functions-triggers-bindings.md#binding-expressions-and-patterns), que significa é resolvido para um valor diferente no tempo de execução.  Tem o conteúdo da mensagem de fila no tempo de execução.

2. Adicionar um `using`:

   ```cs
   using System.IO;
   ```

3. Crie um contentor de BLOBs na sua conta de armazenamento.

   a. No **Explorador de servidores**, expanda o nó para a sua conta de armazenamento, clique com botão direito **Blobs**e, em seguida, selecione **criar contentor de Blob**.

   b. No **criar contentor de Blob** caixa de diálogo, introduza *contentor* como o nome do contentor e, em seguida, clique em **OK**.

4. Carregar o *Program.cs* ficheiro para o contentor de blob. (Este ficheiro é utilizado aqui como um exemplo; pode carregar qualquer ficheiro de texto e criar uma mensagem de fila com o nome do ficheiro.)

   a. No **Explorador de servidores**, faça duplo clique no nó para o contentor que acabou de criar.

   b. No **contentor** janela, selecione o **carregar** botão.

   ![Botão de carregamento de blob](./media/webjobs-sdk-get-started/blob-upload-button.png)

   c. Localize e selecione *Program.cs*e, em seguida, selecione **OK**.

5. Criar uma mensagem de fila na fila que criou anteriormente, com *Program.cs* como o texto da mensagem.

   ![Mensagem de fila Program.cs](./media/webjobs-sdk-get-started/queue-msg-program-cs.png)

6. Execute o projeto.

   A mensagem da fila aciona a função, o que, em seguida, lê o blob e regista o período. O resultado da consola tem o seguinte aspeto:

   ```console
   Found the following functions:
   ConsoleApp1.Functions.ProcessQueueMessage
   Job host started
   Executing 'Functions.ProcessQueueMessage' (Reason='New queue message detected on 'queue'.', Id=5a2ac479-de13-4f41-aae9-1361f291ff88)
   Blob name:Program.cs
   Size: 532 bytes
   Executed 'Functions.ProcessQueueMessage' (Succeeded, Id=5a2ac479-de13-4f41-aae9-1361f291ff88)
   ```

## <a name="add-an-output-binding"></a>Adicionar um enlace de saída

Enlaces de saída simplificam o código que escreve dados. Neste exemplo modifica um anterior ao escrever uma cópia do blob em vez de registo de tamanho.

1. Substitua o método `ProcessQueueMessage` pelo código abaixo:

   ```cs
   public static void ProcessQueueMessage(
       [QueueTrigger("queue")] string message,
       [Blob("container/{queueTrigger}", FileAccess.Read)] Stream myBlob,
       [Blob("container/copy-{queueTrigger}", FileAccess.Write)] Stream outputBlob,
       ILogger logger)
   {
       logger.LogInformation($"Blob name:{message} \n Size: {myBlob.Length} bytes");
       myBlob.CopyTo(outputBlob);
   }
   ```

5. Criar outra mensagem de fila com *Program.cs* como o texto da mensagem.

6. Execute o projeto.

   A mensagem da fila aciona a função, o que, em seguida, lê o blob, regista o período e cria um novo blob. O resultado da consola é o mesmo, mas quando Ir para a janela de contentor de blob e selecione **atualizar**, verá um novo blob com o nome *Program.cs cópia.*

## <a name="next-steps"></a>Passos Seguintes

Este guia foi apresentado como criar, executar e implementar um projeto do SDK de WebJobs.

Para mostrar tudo o que passa para um projeto do SDK de WebJobs, as instruções tinham a criar um projeto a partir do zero. No entanto, quando criar o projeto seguinte, considere a utilização de **trabalho Web do Azure** modelo no **nuvem** categoria. Este modelo cria um projeto com pacotes de NuGet e código de exemplo já definida. Tenha em atenção que o código de exemplo poderá ter de ser alterado para utilizar a nova arquitetura de registo.

Para obter mais informações, consulte [como utilizar o SDK de WebJobs](webjobs-sdk-how-to.md).
