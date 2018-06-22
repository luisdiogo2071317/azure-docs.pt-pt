---
title: Transmissão em fluxo em direto com Serviços de Multimédia do Azure v3 com .NET Core | Microsoft Docs
description: Este tutorial explica os passos da transmissão em fluxo em direto com os Serviços de Multimédia v3 com .NET Core.
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/06/2018
ms.author: juliako
ms.openlocfilehash: f02820f8382268dfedeee8d07de2438660e5e33e
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35236887"
---
# <a name="stream-live-with-azure-media-services-v3-using-net-core"></a>Transmissão em fluxo em direto com os Serviços de Multimédia v3 com .NET Core

Nos Serviços de Multimédia, os [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) são responsáveis por processar o conteúdo da transmissão em fluxo em direto. Um LiveEvent fornece um ponto final de entrada (URL de ingestão) que, em seguida, o utilizador indica a um codificador em direto. O LiveEvent recebe fluxos de entrada em direto do codificador em direto e disponibiliza o mesmo para transmissão em fluxo através de um ou mais [StreamingEndpoints](https://docs.microsoft.com/rest/api/media/streamingendpoints). O LiveEvents também fornece um ponto final de pré-visualização (URL de pré-visualização) que pode utilizar para pré-visualizar e validar a sua transmissão antes de mais processamentos e entregas. Este tutorial mostra como utilizar um canal .NET Core para criar um evento em direto do tipo **pass-through**. 

> [!NOTE]
> Certifique-se de que revê [Transmissão em fluxo em direto com os Serviços de Multimédia v3](live-streaming-overview.md) antes de continuar. 

Este tutorial mostra-lhe como:    

> [!div class="checklist"]
> * Criar uma conta dos Media Services
> * Aceder à API dos Serviços de Multimédia
> * Configurar a aplicação de exemplo
> * Examinar o código que efetua a transmissão em fluxo em direto
> * Observar o evento com o [Leitor de Multimédia do Azure](http://amp.azure.net/libs/amp/latest/docs/index.html) em http://ampdemo.azureedge.net
> * Limpar recursos

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

O seguinte é necessário para concluir o tutorial.

* Instalar o Visual Studio Code ou o Visual Studio
* Uma câmara ou um dispositivo (como um computador portátil) que é utilizado para transmitir um evento.
* Um codificador em direto no local que converte sinais da câmara para transmissões em fluxos que são enviadas para o serviço de transmissão em fluxo em direto dos Serviços de Multimédia. O fluxo tem de ser em formato **RTMP** ou de **transmissão em fluxo uniforme**.

## <a name="download-the-sample"></a>Transferir o exemplo

Clone o repositório do GitHub que contém o exemplo de .NET de transmissão para o computador com o comando seguinte:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```

O exemplo de transmissão em fluxo em direto está localizado na pasta [Dinâmica](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/Live/MediaV3LiveApp).

> [!IMPORTANT]
> Este exemplo utiliza o sufixo exclusivo para cada recurso. Se cancelar a depuração ou se terminar a aplicação sem a executar, irá ficar com vários LiveEvents na sua conta. <br/>
> Certifique-se de que interrompe a execução de LiveEvents. Se não o fizer, estes ser-lhe-ão **faturados**!

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="examine-the-code-that-performs-live-streaming"></a>Examinar o código que efetua a transmissão em fluxo em direto

Esta secção examina funções definidas no ficheiro [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs) do projeto *MediaV3LiveApp*.

O exemplo cria um sufixo exclusivo para cada recurso para não ocorrerem colisões de nomes, se executar o exemplo várias vezes sem proceder a uma limpeza.

> [!IMPORTANT]
> Este exemplo utiliza o sufixo exclusivo para cada recurso. Se cancelar a depuração ou se terminar a aplicação sem a executar, irá ficar com vários LiveEvents na sua conta. <br/>
> Certifique-se de que interrompe a execução de LiveEvents. Se não o fizer, estes ser-lhe-ão **faturados**!
 
### <a name="start-using-media-services-apis-with-net-sdk"></a>Começar a utilizar as APIs dos Serviços de Multimédia com o SDK .NET

Para começar a utilizar as APIs dos Serviços de Multimédia com o .NET, tem de criar um objeto **AzureMediaServicesClient**. Para criar o objeto, tem de fornecer as credenciais necessárias para o cliente se ligar ao Azure com o Azure AD. No código que clonou no início do artigo, a função **GetCredentialsAsync** cria o objeto ServiceClientCredentials com base nas credenciais fornecidas no ficheiro de configuração local.  

```csharp
private static async Task<IAzureMediaServicesClient> CreateMediaServicesClientAsync(ConfigWrapper config)
{
    var credentials = await GetCredentialsAsync(config);

    return new AzureMediaServicesClient(config.ArmEndpoint, credentials)
    {
        SubscriptionId = config.SubscriptionId,
    };
}
```

### <a name="create-a-live-event"></a>Criar um evento em direto

Esta secção mostra como criar um LiveEvent do tipo **pass-through** (LiveEventEncodingType definido como None). Se pretende criar um LiveEvent que esteja ativado para Live Encoding defina LiveEventEncodingType como Básico. 

Algumas outras coisas que pode querer especificar ao criar o evento em direto são:

* A localização dos Serviços de Multimédia 
* O protocolo de transmissão em fluxo para o evento em direto (atualmente, são suportados os protocolos RTMP e a transmissão em fluxo uniforme)
       
    Não é possível alterar a opção de protocolo enquanto o LiveEvent ou os LiveOutputs associados estiverem em execução. Se necessitar de protocolos diferentes, deve criar LiveEvent separados para cada protocolo de transmissão em fluxo.  
* Restrições de IP na ingestão e na pré-visualização. Pode definir os endereços IP que estão autorizados a ingerir um vídeo neste LiveEvent. Os endereços IP permitidos podem ser especificados como um endereço IP único (por exemplo "10.0.0.1"), um intervalo de IP com um endereço IP e uma máscara de sub-rede CIDR (por exemplo, ' 10.0.0.1/22') ou um intervalo de IP com um endereço IP e uma máscara de sub-rede de ponto decimal (por exemplo , ' 10.0.0.1(255.255.252.0)').
    
    Se não for especificado qualquer endereço IP e se não existir nenhuma definição de regra, não será permitido nenhum endereço IP. Para permitir um endereço IP, crie uma regra e defina 0.0.0.0/0.

Ao criar o evento, poderá especificar o início automático do mesmo. 

```csharp
Console.WriteLine($"Creating a live event named {liveEventName}");
Console.WriteLine();

LiveEventPreview liveEventPreview = new LiveEventPreview
{
    AccessControl = new LiveEventPreviewAccessControl(
        ip: new IPAccessControl(
            allow: new IPRange[]
            {
                new IPRange (
                    name: "AllowAll",
                    address: "0.0.0.0",
                    subnetPrefixLength: 0
                )
            }
        )
    )
};

// This can sometimes take awhile. Be patient.
LiveEvent liveEvent = new LiveEvent(
    location: mediaService.Location, 
    description:"Sample LiveEvent for testing",
    vanityUrl:false,
    encoding: new LiveEventEncoding(
                // Set this to Basic to enable a transcoding LiveEvent, and None to enable a pass-through LiveEvent
                encodingType:LiveEventEncodingType.None, 
                presetName:null
            ),
    input: new LiveEventInput(LiveEventInputProtocol.RTMP), 
    preview: liveEventPreview,
    streamOptions: new List<StreamOptionsFlag?>()
    {
        // Set this to Default or Low Latency.
        // Low latency reduces the amount of buffering Media Services does.
        // Low latency can also reduce the stability of the live stream. 
        StreamOptionsFlag.Default
    }
);

Console.WriteLine($"Creating the LiveEvent, be patient this can take time...");
liveEvent = client.LiveEvents.Create(config.ResourceGroup, config.AccountName, liveEventName, liveEvent, autoStart:true);
```

### <a name="get-ingest-urls"></a>Obter URLs de inserção

Assim que o canal seja criado, pode obter os URLs de inserção que fornecerá ao codificador em direto. O codificador utiliza estes URLs para exibir uma transmissão um fluxo direto.


```csharp
// Get the input endpoint to configure the on premise encoder with
string ingestUrl = liveEvent.Input.Endpoints.First().Url;
Console.WriteLine($"The ingest url to configure the on premise encoder with is:");
Console.WriteLine($"\t{ingestUrl}");
Console.WriteLine();
```

### <a name="get-the-preview-url"></a>Obter o URL de pré-visualização

Utilize o previewEndpoint para pré-visualizar e certifique-se de que a entrada do codificador está a ser recebida.

> [!IMPORTANT]
> Certifique-se de que o vídeo está a fluir para o URL de pré-visualização antes de continuar!

```sharp
string previewEndpoint = liveEvent.Preview.Endpoints.First().Url;
Console.WriteLine($"The preview url is:");
Console.WriteLine($"\t{previewEndpoint}");
Console.WriteLine();

Console.WriteLine($"Open the live preview in your browser and use the Azure Media Player to monitor the preview playback:");
Console.WriteLine($"\thttps://ampdemo.azureedge.net/?url={previewEndpoint}");
Console.WriteLine();
```

### <a name="create-and-manage-liveevents-and-liveoutputs"></a>Criar e gerir LiveEvents e LiveOutputs

Assim que a transmissão em fluxo estiver a ser enviada para o LiveEvent, pode iniciar o evento de transmissão em fluxo através da criação de um Elemento, de LiveOutput e de StreamingLocator. Isto irá arquivar a transmissão em fluxo e torná-la disponível para os visualizadores através de StreamingEndpoint. 

#### <a name="create-an-asset"></a>Criar um Elemento

Crie um Elemento para LiveOutput a utilizar.

```csharp
string assetName = "archiveAsset" + uniqueness;
Console.WriteLine($"Creating an asset named {assetName}");
Console.WriteLine();
Asset asset = client.Assets.CreateOrUpdate(config.ResourceGroup, config.AccountName, assetName, new Asset());
```

#### <a name="create-a-liveoutput"></a>Criar um LiveOutput

```csharp
string manifestName = "output";
string liveOutputName = "liveOutput" + uniqueness;
Console.WriteLine($"Creating a live output named {liveOutputName}");
Console.WriteLine();

LiveOutput liveOutput = new LiveOutput(assetName: asset.Name, manifestName: manifestName, archiveWindowLength: TimeSpan.FromMinutes(10));
liveOutput = client.LiveOutputs.Create(config.ResourceGroup, config.AccountName, liveEventName, liveOutputName, liveOutput);
```

#### <a name="create-a-streaminglocator"></a>Criar um StreamingLocator

> [!NOTE]
> Quando a conta dos Serviços de Multimédia é criada, é adicionado um ponto final de transmissão em fluxo **predefinido** à mesma, no estado **Parado**. Para começar a transmitir o seu conteúdo em fluxo e a tirar partido do empacotamento e encriptação dinâmicos, o ponto final de transmissão em fluxo a partir do qual quer transmitir conteúdo tem de estar no estado **Em execução**. 


```csharp
StreamingLocator locator = new StreamingLocator(assetName: assetName, streamingPolicyName: PredefinedStreamingPolicy.ClearStreamingOnly);
locator = client.StreamingLocators.Create(config.ResourceGroup, config.AccountName, streamingLocatorName, locator);

// Get the default Streaming Endpoint on the account
StreamingEndpoint streamingEndpoint = client.StreamingEndpoints.Get(config.ResourceGroup, config.AccountName, "default");

// If it's not running, Start it. 
if (streamingEndpoint.ResourceState != StreamingEndpointResourceState.Running)
{
    Console.WriteLine("Streaming Endpoint was Stopped, restarting now..");
    client.StreamingEndpoints.Start(config.ResourceGroup, config.AccountName, "default");
}

// Get the url to stream the output
ListPathsResponse paths = await client.StreamingLocators.ListPathsAsync(resourceGroupName, accountName, locatorName);

foreach (StreamingPath path in paths.StreamingPaths)
{
    UriBuilder uriBuilder = new UriBuilder();
    uriBuilder.Scheme = "https";
    uriBuilder.Host = streamingEndpoint.HostName;

    uriBuilder.Path = path.Paths[0];
    // Get the URL from the uriBuilder: uriBuilder.ToString()
}
```

### <a name="cleaning-up-resources-in-your-media-services-account"></a>Limpar os recursos na sua conta de Serviços de Multimédia

Se terminar a transmissão em fluxo de eventos e pretende limpar os recursos aprovisionados anteriormente, siga o procedimento seguinte.

* Termine o envio da transmissão em fluxo do codificador.
* Pare o LiveEvent. Assim que o LiveEvent estiver parado, não será cobrado qualquer custo. Quando quiser reiniciar a transmissão, esta terá o mesmo URL de inserção, desta forma, não terá de reconfigurar o codificador.
* Pode parar o seu StreamingEndpoint, a menos que pretenda continuar a fornecer o arquivo do seu evento em direto como uma transmissão em fluxo a pedido. Se o LiveEvent estiver num estado parado, não será cobrado qualquer custo.

```csharp
private static void CleanupLiveEventAndOutput(IAzureMediaServicesClient client, string resourceGroup, string accountName, string liveEventName, string liveOutputName)
{
    // Delete the LiveOutput
    client.LiveOutputs.Delete(resourceGroup, accountName, liveEventName, liveOutputName);

    // Stop and delete the LiveEvent
    client.LiveEvents.Stop(resourceGroup, accountName, liveEventName);
    client.LiveEvents.Delete(resourceGroup, accountName, liveEventName);
}

private static void CleanupLocatorAssetAndStreamingEndpoint(IAzureMediaServicesClient client, string resourceGroup, string accountName, string streamingLocatorName, string assetName)
{
    // Delete the Streaming Locator
    client.StreamingLocators.Delete(resourceGroup, accountName, streamingLocatorName);

    // Delete the Archive Asset
    client.Assets.Delete(resourceGroup, accountName, assetName);
}

private static void CleanupAccount(IAzureMediaServicesClient client, string resourceGroup, string accountName)
{
    try{
        Console.WriteLine("Cleaning up the resources used, stopping the LiveEvent. This can take a few minutes to complete.");
        Console.WriteLine();

        var events = client.LiveEvents.List(resourceGroup, accountName);
        
        foreach (LiveEvent l in events)
        {
            if (l.Name == liveEventName){
                var outputs = client.LiveOutputs.List(resourceGroup, accountName, l.Name);

                foreach (LiveOutput o in outputs)
                {
                    client.LiveOutputs.Delete(resourceGroup, accountName, l.Name, o.Name);
                    Console.WriteLine($"LiveOutput: {o.Name} deleted from LiveEvent {l.Name}. The archived Asset and Streaming URLs are still retained for on-demand viewing.");
                }

                if (l.ResourceState == LiveEventResourceState.Running){
                    client.LiveEvents.Stop(resourceGroup, accountName, l.Name);
                    Console.WriteLine($"LiveEvent: {l.Name} Stopped.");
                    client.LiveEvents.Delete(resourceGroup, accountName, l.Name);
                    Console.WriteLine($"LiveEvent: {l.Name} Deleted.");
                    Console.WriteLine();
                }
            }
        }
    } 
    catch(ApiErrorException e)
    {
        Console.WriteLine("Hit ApiErrorException");
        Console.WriteLine($"\tCode: {e.Body.Error.Code}");
        Console.WriteLine($"\tCode: {e.Body.Error.Message}");
        Console.WriteLine();

    }
}
```        

## <a name="watch-the-event"></a>Ver o evento

Para ver o evento, copie o URL de transmissão em fluxo que recebeu quando executou código descrito em [Criar um StreamingLocator](#create-a-streaminglocator) e utilize um leitor da sua preferência. Pode utilizar o [Leitor de Multimédia do Azure](http://amp.azure.net/libs/amp/latest/docs/index.html) para testar a sua transmissão em fluxo em http://ampdemo.azureedge.net. 

O evento em direto converte automaticamente os eventos para conteúdo a pedido quando parado. Mesmo depois de parar e eliminar o evento, os utilizadores conseguirão transmitir o seu conteúdo arquivado como um vídeo a pedido, desde que não elimine o elemento. Não é possível eliminar um elemento se este é utilizado por um evento; o evento deve ser eliminado primeiro. 

## <a name="clean-up-resources"></a>Limpar recursos

Se já não precisar de nenhum dos recursos presentes no seu grupo de recursos, incluindo a conta dos Serviços de Multimédia e a conta de armazenamento que criou para este tutorial, elimine o grupo de recursos anteriormente criado. Pode utilizar a ferramenta **CloudShell**.

No **CloudShell**, execute o seguinte comando:

```azurecli-interactive
az group delete --name amsResourceGroup
```

> [!IMPORTANT]
> Deixar o LiveEvent em execução implica custos de faturação. Tenha em atenção que se o programa/projeto falhar ou se for fechado por qualquer motivo, pode deixar o LiveEvent em execução num estado de faturação.

## <a name="next-steps"></a>Passos seguintes

[Transmitir ficheiros em fluxo](stream-files-tutorial-with-api.md)

