---
title: Analisar vídeos com os Serviços de Multimédia do Azure | Microsoft Docs
description: Siga os passos deste tutorial para analisar vídeos com os Serviços de Multimédia do Microsoft Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/09/2018
ms.author: juliako
ms.openlocfilehash: a228443a787db2cc71a7700b978d8f52edb59e14
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-analyze-videos-with-azure-media-services"></a>Tutorial: analisar vídeos com os Serviços de Multimédia do Azure 

Este tutorial mostra-lhe como analisar vídeos com os Serviços de Multimédia do Microsoft Azure. Existem muitos cenários em que pode querer obter mais informações sobre conteúdos de áudio ou vídeos gravados. Por exemplo, para alcançar uma maior satisfação do cliente, as organizações podem executar o processamento de voz em texto para converter gravações de suporte de clientes num catálogo pesquisável, com índices e dashboards. Em seguida, podem obter informações sobre a empresa, tal como uma lista de queixas comuns, origens dessas queixas, etc.

Este tutorial mostrar-lhe como:    

> [!div class="checklist"]
> * Iniciar o Azure Cloud Shell
> * Criar uma conta dos Media Services
> * Aceder à API dos Serviços de Multimédia
> * Configurar a aplicação de exemplo
> * Examinar o código de exemplo em detalhe
> * Executar a aplicação
> * Examinar o ficheiro de saída
> * Limpar recursos

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Se não tiver o Visual Studio instalado, pode obter o [Visual Studio Community 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15).

## <a name="download-the-sample"></a>Transferir o exemplo

Clone o repositório do GitHub que contém o exemplo de .NET para o seu computador com o seguinte comando:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="examine-the-sample-code-in-detail"></a>Examinar o código de exemplo em detalhe

Esta secção examina funções definidas no ficheiro [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/AnalyzeVideos/Program.cs) do projeto *AnalyzeVideos*.

### <a name="start-using-media-services-apis-with-net-sdk"></a>Começar a utilizar APIs dos Serviços de Multimédia com SDK do .NET

Para começar a utilizar as APIs dos Serviços de Multimédia com .NET, tem de criar um objeto **AzureMediaServicesClient**. Para criar o objeto, tem de fornecer as credenciais necessárias para o cliente se ligar ao Azure com o Azure AD. Primeiro tem de obter um token e, em seguida, criar um objeto **ClientCredential** a partir do token obtido. No código que clonou no início do artigo, o objeto **ArmClientCredential** é utilizado para obter o token.  

```csharp
private static IAzureMediaServicesClient CreateMediaServicesClient(ConfigWrapper config)
{
    ArmClientCredentials credentials = new ArmClientCredentials(config);

    return new AzureMediaServicesClient(config.ArmEndpoint, credentials)
    {
        SubscriptionId = config.SubscriptionId,
    };
}
```

### <a name="create-an-output-asset-to-store-the-result-of-a-job"></a>Criar um elemento de saída para armazenar o resultado de uma tarefa 

O elemento de saída armazena o resultado da sua tarefa. O projeto define a função **DownloadResults** que transfere os resultados deste elemento de saída para a pasta "saída", para que possa ver o que recebeu.

```csharp
private static Asset CreateOutputAsset(IAzureMediaServicesClient client, string resourceGroupName, string accountName, string assetName)
{
    // Check if an Asset already exists
    Asset outputAsset = client.Assets.Get(resourceGroupName, accountName, assetName);
    Asset asset = new Asset();
    string outputAssetName = assetName;

    if (outputAsset != null)
    {
        // Name collision! In order to get the sample to work, let's just go ahead and create a unique asset name
        // Note that the returned Asset can have a different name than the one specified as an input parameter.
        // You may want to update this part to throw an Exception instead, and handle name collisions differently.
        string uniqueness = @"-" + Guid.NewGuid().ToString();
        outputAssetName += uniqueness;
    }

    return client.Assets.CreateOrUpdate(resourceGroupName, accountName, outputAssetName, asset);
}
```
### <a name="create-a-transform-and-a-job-that-analyzes-videos"></a>Criar uma transformação e uma tarefa que analisa vídeos

Ao codificar ou processar conteúdos nos Serviços de Multimédia, é comum configurar as definições de codificação como uma receita. Em seguida, deveria submeter uma **Tarefa** para aplicar essa receita a um vídeo. Ao submeter novas Tarefas para cada novo vídeo, está a aplicar essa receita a todos os vídeos na sua biblioteca. Uma receita nos Serviços de Multimédia chama-se uma **Transformação**. Para obter mais informações, veja [Transforms and jobs](transform-concept.md) (Transformações e tarefas). O exemplo descrito neste tutorial define uma receita que analisa o vídeo especificado. 

#### <a name="transform"></a>Transformar

Ao criar uma nova instância **Transformar**, tem de especificar a saída resultante que pretende. O parâmetro necessário é um objeto **TransformOutput**, conforme apresentado no código acima. Cada **TransformOutput** contém uma **Predefinição**. A **Predefinição** descreve instruções passo a passo das operações de processamento de áudio e/ou vídeo que estão a ser utilizadas para gerir o **TransformOutput** pretendido. Neste exemplo, a predefinição **VideoAnalyzerPreset** é utilizada e o idioma ("en-US") é transmitido para o construtor. Esta predefinição permite-lhe extrair múltiplas informações de vídeo e áudio a partir de um vídeo. Pode utilizar a predefinição **AudioAnalyzerPreset** se precisar de extrair múltiplas informações de áudio a partir de um vídeo. 

Ao criar uma **Transformação**, primeiro deve verificar se já existe uma com o método **Get**, conforme apresentado no código que se segue.  Nos Serviços de Multimédia v3, os métodos **Get** em entidades devolvem um valor **nulo** se a entidade não existir (uma verificação não sensível a maiúsculas e minúsculas no nome).

```csharp
private static Transform EnsureTransformExists(IAzureMediaServicesClient client, 
    string resourceGroupName, 
    string accountName, 
    string transformName, 
    Preset preset)
{
    // Does a Transform already exist with the desired name? Assume that an existing Transform with the desired name
    // also uses the same recipe or Preset for processing content.
    Transform transform = client.Transforms.Get(resourceGroupName, accountName, transformName);

    if (transform == null)
    {
        // Start by defining the desired outputs.
        TransformOutput[] outputs = new TransformOutput[]
        {
            new TransformOutput(preset),
        };

        transform = client.Transforms.CreateOrUpdate(resourceGroupName, accountName, transformName, outputs);
    }

    return transform;
}
```

#### <a name="job"></a>Tarefa

Conforme mencionado acima, o objeto **Transform** é a receita e uma **Tarefa** é o pedido real para os Serviços de Multimédia aplicarem essa **Transformação** a um determinado conteúdo de áudio ou vídeo de entrada. A **Tarefa** especifica informações como a localização do vídeo de entrada e a localização para a saída. Pode especificar a localização do seu vídeo com: URLs HTTPS, URLs SAS ou Recursos que estão na sua conta dos Serviços de Multimédia. 

Neste exemplo, o vídeo de entrada é carregado a partir de um URL HTTPS especificado.  

```csharp
private static Job SubmitJob(IAzureMediaServicesClient client, string resourceGroupName, string accountName, string transformName, string jobName, string outputAssetName)
{
    JobInputHttp jobInput =
        new JobInputHttp(files: new[] { "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/Ignite-short.mp4" });
            
    string uniqueJobName = jobName;
    Job job = client.Jobs.Get(resourceGroupName, accountName, transformName, jobName);

    if (job != null)
    {
        // Job already exists with the same name, so let's append a GUID
        string uniqueness = @"-" + Guid.NewGuid().ToString();
        uniqueJobName += uniqueness;
    }

    JobOutput[] jobOutputs =
    {
        new JobOutputAsset(outputAssetName),
    };

    job = client.Jobs.Create(
                resourceGroupName,
                accountName,
                transformName,
                uniqueJobName,
                new Job
                {
                    Input = jobInput,
                    Outputs = jobOutputs,
                });

    return job;
}
```

### <a name="wait-for-the-job-to-complete"></a>Aguarde até que a tarefa termine

A tarefa demora algum tempo a terminar, por isso irá querer receber uma notificação quando for concluída. Existem algumas opções diferentes para ser notificado sobre a conclusão da tarefa. A opção mais simples (que é apresentada aqui) é utilizar uma consulta. 

Utilizar uma consulta não é uma prática recomendada para produzir aplicações devido à potencial latência. A consulta pode ser limitada se for utilizada excessivamente numa conta. Em alternativa, os programadores devem utilizar o Event Grid.

O Event Grid foi concebido para ter uma elevada disponibilidade, um desempenho consistente e um dimensionamento dinâmico. Com o Event Grid, as aplicações podem escutar e reagir a eventos a partir de praticamente todos os serviços do Azure, bem como de origens personalizadas. O processamento de eventos simples, reativo e baseado em HTTP ajuda-o a criar soluções eficientes através da filtragem e do encaminhamento inteligente de eventos. Veja [Encaminhar eventos para um ponto final Web personalizado](job-state-events-cli-how-to.md).

Normalmente, a **Tarefa** passa pelos seguintes estados: **Agendada**, **Em fila**, **Em processamento**, **Concluída** (o estado final). Se tiver ocorrido um erro na tarefa, verá o estado **erro**. Se a tarefa estiver a ser cancelada, verá **A cancelar** e **Cancelada** quando terminar.

```csharp
private static Job WaitForJobToFinish(IAzureMediaServicesClient client,
    string resourceGroupName,
    string accountName,
    string transformName,
    string jobName)
{
    int SleepInterval = 60 * 1000;

    Job job = null;

    while (true)
    {
        job = client.Jobs.Get(resourceGroupName, accountName, transformName, jobName);

        if (job.State == JobState.Finished || job.State == JobState.Error || job.State == JobState.Canceled)
        {
            break;
        }

        Console.WriteLine($"Job is {job.State}.");
        for (int i = 0; i < job.Outputs.Count; i++)
        {
            JobOutput output = job.Outputs[i];
            Console.Write($"\tJobOutput[{i}] is {output.State}.");
            if (output.State == JobState.Processing)
            {
                Console.Write($"  Progress: {output.Progress}");
            }
            Console.WriteLine();
        }
        System.Threading.Thread.Sleep(SleepInterval);
    }

    return job;
}
```

### <a name="download-the-result-of-the-job"></a>Transferir o resultado da tarefa

 A seguinte função transfere os resultados do elemento de saída para a pasta "saída", para que possa examinar os resultados da tarefa. 

```csharp
private static void DownloadResults(IAzureMediaServicesClient client,
    string resourceGroup,
    string accountName,
    string assetName,
    string resultsFolder)
{
    AssetContainerSas assetContainerSas = client.Assets.ListContainerSas(
            resourceGroup,
            accountName,
            assetName,
            permissions: AssetContainerPermission.Read,
            expiryTime: DateTime.UtcNow.AddHours(1).ToUniversalTime()
            );

    Uri containerSasUrl = new Uri(assetContainerSas.AssetContainerSasUrls.FirstOrDefault());
    CloudBlobContainer container = new CloudBlobContainer(containerSasUrl);

    string directory = Path.Combine(resultsFolder, assetName);
    Directory.CreateDirectory(directory);

    Console.WriteLine("Downloading results to {0}.", directory);

    foreach (IListBlobItem blobItem in container.ListBlobs(null, true, BlobListingDetails.None))
    {
        if (blobItem is CloudBlockBlob)
        {
            CloudBlockBlob blob = blobItem as CloudBlockBlob;
            string filename = Path.Combine(directory, blob.Name);

            blob.DownloadToFile(filename, FileMode.Create);
        }
    }

    Console.WriteLine("Download complete.");
}
```

### <a name="clean-up-resource-in-your-media-services-account"></a>Limpar os recursos na conta dos Serviços de Multimédia

Geralmente, deve limpar tudo exceto os objetos que planeia reutilizar (normalmente, reutiliza as Transformações e mantém StreamingLocators, etc.). Se quiser que a sua conta seja limpa após fazer experiências, deve eliminar os recursos que não planeia reutilizar. Por exemplo, o seguinte código elimina as Tarefas.

```csharp
static void CleanUp(IAzureMediaServicesClient client,
        string resourceGroupName,
        string accountName, 
        string transformName)
{
    foreach (var job in client.Jobs.List(resourceGroupName, accountName, transformName))
    {
        client.Jobs.Delete(resourceGroupName, accountName, transformName, job.Name);
    }

    foreach (var asset in client.Assets.List(resourceGroupName, accountName))
    {
        client.Assets.Delete(resourceGroupName, accountName, asset.Name);
    }
}
```

## <a name="run-the-sample-app"></a>Execute a aplicação de exemplo

Prima Ctrl+F5 para executar a aplicação *AnalyzeVideos*.

Quando executa o programa, a tarefa produz miniaturas para cada rosto que encontra no vídeo. Também produz o ficheiro insights.json.

## <a name="examine-the-output"></a>Examinar o ficheiro de saída

O ficheiro de saída para analisar vídeos chama-se insights.json. Este ficheiro contém informações sobre o seu vídeo. Encontrará a descrição dos elementos contidos no ficheiro json no artigo [Informações de multimédia](intelligence-concept.md).

## <a name="clean-up-resources"></a>Limpar recursos

Se já não precisa de nenhum dos recursos presentes no seu grupo de recursos, incluindo as contas de armazenamento que criou e os Serviços de Multimédia que carregou neste tutorial, elimine o grupo de recursos que criou anteriormente. Pode utilizar a ferramenta **CloudShell**.

No **CloudShell**, execute o seguinte comando:

```azurecli-interactive
az group delete --name amsResourceGroup
```

## <a name="multithreading"></a>Multithreading

Os SDKs dos Serviços de Multimédia do Azure v3 não são seguros para threads. Ao trabalhar com aplicações com vários threads, deve gerar um novo objeto AzureMediaServicesClient por thread.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Tutorial: carregar, codificar e transmitir vídeos](stream-files-tutorial-with-api.md)
