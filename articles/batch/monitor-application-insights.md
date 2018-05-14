---
title: Monitorizar Batch com o Azure Application Insights | Microsoft Docs
description: Saiba como instrumentar uma aplicação do Azure Batch .NET através da biblioteca do Azure Application Insights.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: .NET
ms.topic: article
ms.workload: na
ms.date: 04/05/2018
ms.author: danlep
ms.openlocfilehash: 9f989ada01a2ffced509b42df9e46aa001386ab6
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="monitor-and-debug-an-azure-batch-net-application-with-application-insights"></a>Monitorizar e depurar uma aplicação .NET do Batch do Azure com o Application Insights

[Application Insights](../application-insights/app-insights-overview.md) fornece uma forma elegante e poderosa para programadores para monitorizar e depuração as aplicações implementadas para serviços do Azure. Utilize o Application Insights para contadores de monitor de desempenho e exceções, bem como instrumento seu código com métricas personalizadas e rastreio. Integração do Application Insights com a sua aplicação do Azure Batch permite-lhe obter conhecimentos aprofundados em comportamentos e investigar problemas na quase em tempo real.

Este artigo mostra como adicionar e configurar a biblioteca do Application Insights na sua solução do Azure Batch .NET e instrumentar código da aplicação. Também mostra formas de monitorizar a sua aplicação através do portal do Azure e criar dashboards personalizados. Para o Application Insights suportar outros idiomas, observe o [documentação linguagens, plataformas e integrações](../application-insights/app-insights-platforms.md).

Uma exemplo c# solução com o código para acompanham neste artigo está disponível em [GitHub](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ApplicationInsights). Este exemplo adiciona o código de instrumentação do Application Insights para o [TopNWords](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords) exemplo. Se não estiver familiarizado com esse exemplo, tente primeiro TopNWords criar e executar. Fazer isto irá ajudar a compreender um fluxo de trabalho do Batch básico do processamento de um conjunto de blobs de entrada em paralelo em diversos nós de computação. 

## <a name="prerequisites"></a>Pré-requisitos
* [IDE do Visual Studio](https://www.visualstudio.com/vs) (Visual Studio 2015 ou uma versão mais recente)

* [Conta do batch e a conta de armazenamento ligada](batch-account-create-portal.md)

* [Recurso do Application Insights](../application-insights/app-insights-create-new-resource.md)
  
   * Utilizar o portal do Azure para criar um Application Insights *recursos*. Selecione o *geral* **tipo de aplicação**.

   * Copiar o [chave de instrumentação](../application-insights/app-insights-create-new-resource.md#copy-the-instrumentation-key) do portal. É necessário neste artigo.
  
  > [!NOTE]
  > Poderá estar [cobrados](https://azure.microsoft.com/pricing/details/application-insights/) para os dados armazenados no Application Insights. Isto inclui o diagnóstico e monitorização dados abordados neste artigo.
  > 

## <a name="add-application-insights-to-your-project"></a>Adicionar as Informações da Aplicação ao seu projeto

O **Microsoft.ApplicationInsights.WindowsServer** pacote NuGet e as respetivas dependências são necessárias para o seu projeto. Adicionar ou restaurá-las ao projeto da aplicação. Para instalar o pacote, utilize o `Install-Package` comando ou o Gestor de pacotes NuGet.

```powershell
Install-Package Microsoft.ApplicationInsights.WindowsServer
```
Referenciar o Application Insights da sua aplicação do .NET utilizando o **Microsoft.ApplicationInsights** espaço de nomes.

## <a name="instrument-your-code"></a>Instrumentar código

Para instrumentar código, a solução tem de criar um Application Insights [TelemetryClient](/dotnet/api/microsoft.applicationinsights.telemetryclient). No exemplo, o TelemetryClient carrega a respetiva configuração a partir de [Applicationinsights](../application-insights/app-insights-configuration-with-applicationinsights-config.md) ficheiro. Não se esqueça de atualizar Applicationinsights na seguintes projetos com a sua chave de instrumentação do Application Insights: Microsoft.Azure.Batch.Samples.TelemetryStartTask e TopNWordsSample.

```xml
<InstrumentationKey>YOUR-IKEY-GOES-HERE</InstrumentationKey>
```
Também adicione a chave de instrumentação no ficheiro TopNWords.cs.

O exemplo na TopNWords.cs utiliza as seguintes [chamadas de instrumentação](../application-insights/app-insights-api-custom-events-metrics.md) do Application Insights API:
* `TrackMetric()` -Regista o período de tempo, em média, um nó de computação demora para transferir o ficheiro de texto necessário.
* `TrackTrace()` -Adiciona depuração chamadas para o seu código.
* `TrackEvent()` -Controla interessantes eventos para capturar.

Neste exemplo propositadamente deixa o processamento de exceções. Em vez disso, Application Insights automaticamente relatórios de exceções não processadas, que melhora significativamente a experiência de depuração. 

O fragmento seguinte ilustra como utilizar estes métodos.

```csharp
public void CountWords(string blobName, int numTopN, string storageAccountName, string storageAccountKey)
{
    // simulate exception for some set of tasks
    Random rand = new Random();
    if (rand.Next(0, 10) % 10 == 0)
    {
        blobName += ".badUrl";
    }

    // log the url we are downloading the file from
    insightsClient.TrackTrace(new TraceTelemetry(string.Format("Task {0}: Download file from: {1}", this.taskId, blobName), SeverityLevel.Verbose));

    // open the cloud blob that contains the book
    var storageCred = new StorageCredentials(storageAccountName, storageAccountKey);
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(blobName), storageCred);
    using (Stream memoryStream = new MemoryStream())
    {
        // calculate blob download time
        DateTime start = DateTime.Now;
        blob.DownloadToStream(memoryStream);
        TimeSpan downloadTime = DateTime.Now.Subtract(start);

        // track how long the blob takes to download on this node
        // this will help debug timing issues or identify poorly performing nodes
        insightsClient.TrackMetric("Blob download in seconds", downloadTime.TotalSeconds, this.CommonProperties);

        memoryStream.Position = 0; //Reset the stream
        var sr = new StreamReader(memoryStream);
        var myStr = sr.ReadToEnd();
        string[] words = myStr.Split(' ');

        // log how many words were found in the text file
        insightsClient.TrackTrace(new TraceTelemetry(string.Format("Task {0}: Found {1} words", this.taskId, words.Length), SeverityLevel.Verbose));
        var topNWords =
            words.
                Where(word => word.Length > 0).
                GroupBy(word => word, (key, group) => new KeyValuePair<String, long>(key, group.LongCount())).
                OrderByDescending(x => x.Value).
                Take(numTopN).
                ToList();
        foreach (var pair in topNWords)
        {
            Console.WriteLine("{0} {1}", pair.Key, pair.Value);
        }

        // emit an event to track the completion of the task
        insightsClient.TrackEvent("Done counting words");
    }
}
```

### <a name="azure-batch-telemetry-initializer-helper"></a>Auxiliar de inicializador de telemetria do Azure Batch
Quando o relatório de telemetria para um determinado servidor e instância, o Application Insights utiliza o nome de função da VM do Azure e VM para os valores predefinidos. No contexto do Azure Batch, o exemplo mostra como utilizar o nome do conjunto e em vez disso, o nome do nó de computação. Utilize um [inicializador de telemetria](../application-insights/app-insights-api-filtering-sampling.md#add-properties) para substituir os valores predefinidos. 

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;
using System;
using System.Threading;

namespace Microsoft.Azure.Batch.Samples.TelemetryInitializer
{
    public class AzureBatchNodeTelemetryInitializer : ITelemetryInitializer
    {
        // Azure Batch environment variables
        private const string PoolIdEnvironmentVariable = "AZ_BATCH_POOL_ID";
        private const string NodeIdEnvironmentVariable = "AZ_BATCH_NODE_ID";

        private string roleInstanceName;
        private string roleName;

        public void Initialize(ITelemetry telemetry)
        {
            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleName))
            {
                // override the role name with the Azure Batch Pool name
                string name = LazyInitializer.EnsureInitialized(ref this.roleName, this.GetPoolName);
                telemetry.Context.Cloud.RoleName = name;
            }

            if (string.IsNullOrEmpty(telemetry.Context.Cloud.RoleInstance))
            {
                // override the role instance with the Azure Batch Compute Node name
                string name = LazyInitializer.EnsureInitialized(ref this.roleInstanceName, this.GetNodeName);
                telemetry.Context.Cloud.RoleInstance = name;
            }
        }

        private string GetPoolName()
        {
            return Environment.GetEnvironmentVariable(PoolIdEnvironmentVariable) ?? string.Empty;
        }

        private string GetNodeName()
        {
            return Environment.GetEnvironmentVariable(NodeIdEnvironmentVariable) ?? string.Empty;
        }
    }
}
```

Para ativar o inicializador de telemetria, o ficheiro Applicationinsights no projeto TopNWordsSample inclui o seguinte:

```xml
<TelemetryInitializers>
    <Add Type="Microsoft.Azure.Batch.Samples.TelemetryInitializer.AzureBatchNodeTelemetryInitializer, Microsoft.Azure.Batch.Samples.TelemetryInitializer"/>
</TelemetryInitializers>
``` 

## <a name="update-the-job-and-tasks-to-include-application-insights-binaries"></a>Atualizar a tarefa e tarefas para incluir os binários do Application Insights

Por ordem para o Application Insights seja corretamente executado em nós de computação, certifique-se que os binários são colocados corretamente. Adicione os binários necessários para a coleção de ficheiros de recursos da tarefa para que possam ser descarregados no momento que da tarefa ser executada. Os seguintes fragmentos são semelhantes ao código no Job.cs.

Em primeiro lugar, crie uma lista estática de ficheiros do Application Insights para carregar.

```csharp
private static readonly List<string> AIFilesToUpload = new List<string>()
{
    // Application Insights config and assemblies
    "ApplicationInsights.config",
    "Microsoft.ApplicationInsights.dll",
    "Microsoft.AI.Agent.Intercept.dll",
    "Microsoft.AI.DependencyCollector.dll",
    "Microsoft.AI.PerfCounterCollector.dll",
    "Microsoft.AI.ServerTelemetryChannel.dll",
    "Microsoft.AI.WindowsServer.dll",
    
    // custom telemetry initializer assemblies
    "Microsoft.Azure.Batch.Samples.TelemetryInitializer.dll",
 };
...
```

Em seguida, crie os ficheiros de testes que são utilizados pela tarefa.
```csharp
...
// create file staging objects that represent the executable and its dependent assembly to run as the task.
// These files are copied to every node before the corresponding task is scheduled to run on that node.
FileToStage topNWordExe = new FileToStage(TopNWordsExeName, stagingStorageAccount);
FileToStage storageDll = new FileToStage(StorageClientDllName, stagingStorageAccount);

// Upload Application Insights assemblies
List<FileToStage> aiStagedFiles = new List<FileToStage>();
foreach (string aiFile in AIFilesToUpload)
{
    aiStagedFiles.Add(new FileToStage(aiFile, stagingStorageAccount));
}
...
```

O `FileToStage` método é uma função de programa auxiliar do exemplo de código que lhe permite facilmente carregar um ficheiro de disco local para um blob de armazenamento do Azure. Cada ficheiro mais tarde é transferido para um nó de computação e referenciado por uma tarefa.

Por fim, adicione as tarefas ao trabalho e incluem os binários do Application Insights necessários.
```csharp
...
// initialize a collection to hold the tasks that will be submitted in their entirety
List<CloudTask> tasksToRun = new List<CloudTask>(topNWordsConfiguration.NumberOfTasks);
for (int i = 1; i <= topNWordsConfiguration.NumberOfTasks; i++)
{
    CloudTask task = new CloudTask("task_no_" + i, String.Format("{0} --Task {1} {2} {3} {4}",
        TopNWordsExeName,
        string.Format("https://{0}.blob.core.windows.net/{1}",
            accountSettings.StorageAccountName,
            documents[i]),
        topNWordsConfiguration.TopWordCount,
        accountSettings.StorageAccountName,
        accountSettings.StorageAccountKey));

    //This is the list of files to stage to a container -- for each job, one container is created and 
    //files all resolve to Azure Blobs by their name (so two tasks with the same named file will create just 1 blob in
    //the container).
    task.FilesToStage = new List<IFileStagingProvider>
                        {
                            // required application binaries
                            topNWordExe,
                            storageDll,
                        };
    foreach (FileToStage stagedFile in aiStagedFiles)
   {
        task.FilesToStage.Add(stagedFile);
   }    
    task.RunElevated = false;
    tasksToRun.Add(task);
}
```

## <a name="view-data-in-the-azure-portal"></a>Ver dados no portal do Azure

Agora que configurou a tarefa e tarefas para utilizar o Application Insights, execute a tarefa de exemplo no seu agrupamento. Navegue para o portal do Azure e abrir o recurso do Application Insights que aprovisionou. Depois do conjunto é aprovisionado, deve começar a ver o fluxo de dados e obter a sessão iniciada. O resto deste artigo tocar apenas algumas funcionalidades do Application Insights, mas não hesite em explorar o conjunto completo de funcionalidades.

### <a name="view-live-stream-data"></a>Ver dados de fluxo em direto

Para ver registos de rastreio no seu recurso informações de aplicações, clique em **em fluxo em direto**. Captura de ecrã seguinte mostra como ver dados dinâmicos proveniente de nós de computação no conjunto, por exemplo a utilização da CPU por nó de computação.

![Dados de nó de computação de fluxo dinâmicos](./media/monitor-application-insights/applicationinsightslivestream.png)

### <a name="view-trace-logs"></a>Ver registos de rastreio

Para ver registos de rastreio no seu recurso informações de aplicações, clique em **pesquisa**. Esta vista mostra uma lista de dados de diagnóstico capturadas pelo Application Insights, incluindo os rastreios, eventos e exceções. 

Captura de ecrã seguinte mostra como um rastreio único para uma tarefa é iniciado e consultar mais tarde para fins de depuração.

![Imagem de registos de rastreio](./media/monitor-application-insights/tracelogsfortask.png)

### <a name="view-unhandled-exceptions"></a>Ver as exceções não processadas

As capturas de ecrã seguinte mostra como o Application Insights regista exceções acionadas a partir da sua aplicação. Neste caso, em segundos, da aplicação a gerar a exceção, pode explorar uma exceção específica e diagnosticar o problema.

![Exceções não processadas](./media/monitor-application-insights/exception.png)

### <a name="measure-blob-download-time"></a>Tempo de transferência de blob de medida

Métricas personalizadas também são uma ferramenta valiosa no portal. Por exemplo, pode apresentar o tempo médio, que demorou cada nó de computação para transferir o ficheiro de texto necessário estava processar.

Para criar um gráfico de exemplo:
1. No recurso do Application Insights, clique em **Explorador de métricas** > **adicionar gráfico**.
2. Clique em **editar** no gráfico que foi adicionado.
2. Atualize os detalhes do gráfico da seguinte forma:
   * Definir **tipo de gráfico** para **grelha**.
   * Definir **agregação** para **médio**.
   * Definir **Agrupar por** para **NodeId**.
   * No **métricas**, selecione **personalizada** > **transferir BLOBs, em segundos**.
   * Ajustar apresentar **paleta de cores** à sua escolha. 

![Tempo de transferência de blob por nó](./media/monitor-application-insights/blobdownloadtime.png)


## <a name="monitor-compute-nodes-continuously"></a>Nós de computação de monitor continuamente

Poderá ter reparado que as métricas, incluindo os contadores de desempenho apenas são registadas quando as tarefas em execução. Este comportamento é útil porque está limitada a quantidade de dados que regista o Application Insights. No entanto, há casos quando sempre que gostaria de monitorizar os nós de computação. Por exemplo, se podem estar em execução trabalho em segundo plano que não foi agendado através do serviço de Batch. Neste caso, defina um processo de monitorização sejam executadas durante a vigência de nó de computação. 

Uma forma de alcançar este comportamento é para gerar um processo que carrega a biblioteca do Application Insights e é executado em segundo plano. No exemplo, a tarefa de início carrega os binários na máquina e mantém um processo em execução indefinidamente. Configure o ficheiro de configuração do Application Insights para que este processo emitir dados adicionais que interessa, tais como os contadores de desempenho.

```csharp
...
 // Batch start task telemetry runner
private const string BatchStartTaskFolderName = "StartTask";
private const string BatchStartTaskTelemetryRunnerName = "Microsoft.Azure.Batch.Samples.TelemetryStartTask.exe";
private const string BatchStartTaskTelemetryRunnerAIConfig = "ApplicationInsights.config";
...
CloudPool pool = client.PoolOperations.CreatePool(
    topNWordsConfiguration.PoolId,
    targetDedicated: topNWordsConfiguration.PoolNodeCount,
    virtualMachineSize: "small",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));
...

// Create a start task which will run a dummy exe in background that simply emits performance
// counter data as defined in the relevant ApplicationInsights.config.
// Note that the waitForSuccess on the start task was not set so the Compute Node will be
// available immediately after this command is run.
pool.StartTask = new StartTask()
{
    CommandLine = string.Format("cmd /c {0}", BatchStartTaskTelemetryRunnerName),
    ResourceFiles = resourceFiles
};
...
```

> [!TIP]
> Para aumentar a capacidade de gestão da sua solução, pode agrupar a assemblagem num [pacote de aplicação](./batch-application-packages.md). Em seguida, implementar automaticamente o pacote de aplicações aos seus agrupamentos, adicione uma referência de pacote de aplicação para a configuração do agrupamento.
>

## <a name="throttle-and-sample-data"></a>Dados de exemplo e de limitação 

Devido à natureza em grande escala de aplicações do Azure Batch em execução na produção, pode querer limitar a quantidade de dados recolhidos pelo Application Insights para gerir os custos. Consulte [amostragem no Application Insights](../application-insights/app-insights-sampling.md) algumas mecanismos conseguir isto.


## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [Application Insights](../application-insights/app-insights-overview.md).

* Para o Application Insights suportar outros idiomas, observe o [documentação linguagens, plataformas e integrações](../application-insights/app-insights-platforms.md).


