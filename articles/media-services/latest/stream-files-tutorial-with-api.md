---
title: Carregar, codificar e transmitir em fluxo com os Serviços de Multimédia do Azure | Microsoft Docs
description: Siga os passos deste tutorial para carregar um ficheiro, codificar o vídeo e transmitir o seu conteúdo com os Serviços de Multimédia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: tutorial
ms.custom: mvc
ms.date: 11/11/2018
ms.author: juliako
ms.openlocfilehash: d2d648dd02426596e1c070ffd494458c68060085
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51613448"
---
# <a name="tutorial-upload-encode-and-stream-videos-using-apis"></a>Tutorial: Carregar, codificar e transmitir vídeos com as APIs

Serviços de multimédia do Azure permite-lhe a codificar seus arquivos de suporte de dados em formatos que podem ser jogados numa grande variedade de navegadores e dispositivos. Por exemplo, pode querer transmitir o conteúdo nos formatos HLS ou MPEG DASH da Apple. Antes de transmissão, deve codificar o ficheiro de multimédia digital de alta qualidade. Para obter orientações sobre a codificação, veja [Conceito de codificação](encoding-concept.md). Este tutorial carrega um ficheiro de vídeo local e codifica o ficheiro carregado. Também pode codificar o conteúdo que torna acessível através de um URL HTTPS. Para obter mais informações, veja [Criar uma entrada de tarefa a partir de um URL HTTP(s)](job-input-from-http-how-to.md).

![Reproduzir o vídeo](./media/stream-files-tutorial-with-api/final-video.png)

Este tutorial mostrar-lhe como:    

> [!div class="checklist"]
> * Aceder à API dos Serviços de Multimédia
> * Configurar a aplicação de exemplo
> * Examinar o código que carrega, codifica e transmite
> * Executar a aplicação
> * Testar o URL de transmissão em fluxo
> * Limpar recursos

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

- Se não tiver o Visual Studio instalado, poderá obter o [Visual Studio Community 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15).
- Instalar e utilizar a CLI localmente, este artigo requer a versão 2.0 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Se precisar de instalar ou atualizar, veja [Instalar a CLI do Azure](/cli/azure/install-azure-cli). 

    Atualmente, nem todos [dos serviços de multimédia v3 CLI](https://aka.ms/ams-v3-cli-ref) comandos trabalham no Azure Cloud Shell. Recomenda-se para utilizar a CLI localmente.

- [Criar uma conta de Media Services](create-account-cli-how-to.md).

    Lembre-se de que não se esqueça dos valores que utilizou para o nome do grupo de recursos e o nome de conta de serviços de multimédia

## <a name="download-the-sample"></a>Transferir o exemplo

Clone o repositório do GitHub que contém o exemplo de .NET de transmissão para o computador com o comando seguinte:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```

O exemplo está localizado na pasta [UploadEncodeAndStreamFiles](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/UploadEncodeAndStreamFiles).

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="examine-the-code-that-uploads-encodes-and-streams"></a>Examinar o código que carrega, codifica e transmite

Esta secção examina as funções definidas no ficheiro [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs) do projeto *UploadEncodeAndStreamFiles*.

O exemplo realiza as seguintes ações:

1. Cria uma Transformação nova (em primeiro lugar, verifica se existe a Transformação especificada). 
2. Cria um Elemento de saída que é utilizado como saída da Tarefa de codificação.
3. Crie um Elemento de entrada e carrega o ficheiro de vídeo local especificado para o mesmo. O Elemento é utilizado como entrada da Tarefa. 
4. Submete a Tarefa de codificação através da entrada e saída que foi criada.
5. Verifica o estado da Tarefa.
6. Cria um StreamingLocator.
7. Cria os URLs de transmissão.

### <a name="start-using-media-services-apis-with-net-sdk"></a>Começar a utilizar as APIs dos Serviços de Multimédia com o SDK .NET

Para começar a utilizar as APIs dos Serviços de Multimédia com o .NET, tem de criar um objeto **AzureMediaServicesClient**. Para criar o objeto, tem de fornecer as credenciais necessárias para o cliente se ligar ao Azure com o Azure AD. No código que clonou no início do artigo, a função **GetCredentialsAsync** cria o objeto ServiceClientCredentials com base nas credenciais fornecidas no ficheiro de configuração local. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateMediaServicesClient)]

### <a name="create-an-input-asset-and-upload-a-local-file-into-it"></a>Criar um elemento de entrada e carregar um ficheiro local para ele 

A função **CreateInputAsset** cria um novo [Elemento](https://docs.microsoft.com/rest/api/media/assets) de entrada e carrega o ficheiro de vídeo local especificado para ele. Este Elemento é utilizado como entrada para a Tarefa de codificação. Nos Serviços de Multimédia v3, a entrada para uma Tarefa pode ser um Elemento ou pode ser conteúdo que tenha disponibilizado na sua conta dos Serviços de Multimédia através de URLs HTTPS. Se quiser saber como codificar a partir de um URL HTTPS, veja [este](job-input-from-http-how-to.md) artigo.  

Nos Serviços de Multimédia v3, deverá utilizar as APIs do Armazenamento do Azure para carregar os ficheiros. O seguinte fragmento de .NET mostra como.

A função seguinte executa estes passos:

* Cria um Elemento 
* Obtém um [URL de SAS](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) gravável para o [contentor de armazenamento](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=windows#upload-blobs-to-the-container) do Elemento
* Carrega o ficheiro para o contentor de armazenamento através do URL de SAS

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

### <a name="create-an-output-asset-to-store-the-result-of-a-job"></a>Criar um elemento de saída para armazenar o resultado de uma tarefa 

A saída [Asset](https://docs.microsoft.com/rest/api/media/assets) armazena o resultado da tarefa de codificação. O projeto define a função **DownloadResults**, que transfere os resultados deste elemento de saída para a pasta “saída”, para que possa ver o que obteve.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateOutputAsset)]

### <a name="create-a-transform-and-a-job-that-encodes-the-uploaded-file"></a>Criar uma Transformação e uma Tarefa que codifica o ficheiro carregado
Ao codificar ou processar conteúdos nos Serviços de Multimédia, é um padrão comum configurar as definições de codificação como uma receita. Em seguida, deverá submeter uma **Tarefa** para aplicar essa receita a um vídeo. Ao submeter novas Tarefas para cada vídeo novo, está a aplicar essa receita a todos os vídeos na biblioteca. Uma receita nos Serviços de Multimédia chama-se uma **Transformação**. Para obter mais informações, veja [Transformações e tarefas](transform-concept.md). O exemplo descrito neste tutorial define uma receita que codifica o vídeo para transmiti-lo numa variedade de dispositivos iOS e Android. 

#### <a name="transform"></a>Transformação

Ao criar uma nova instância [Transformação](https://docs.microsoft.com/rest/api/media/transforms), tem de especificar o que pretende produzir como uma saída. O parâmetro necessário é um objeto **TransformOutput**, conforme apresentado no código abaixo. Cada **TransformOutput** contém uma **Predefinição**. A **Predefinição** descreve as instruções passo a passo das operações de processamento de áudio e/ou vídeo que estão a ser utilizadas para gerir o **TransformOutput** pretendido. O exemplo descrito neste artigo utiliza uma Predefinição incorporada chamada **AdaptiveStreaming**. A Predefinição codifica o vídeo de entrada para uma escala de bits gerada automaticamente (pares de resolução/velocidade de transmissão) com base na resolução e velocidade de transmissão de entrada e produz ficheiros ISO MP4 com vídeo H.264 e áudio AAC correspondente a cada par de resolução/velocidade de transmissão. Para obter informações sobre esta Predefinição, veja [Auto-generating bitrate ladder](autogen-bitrate-ladder.md) (Escala de bits gerada automaticamente).

Pode utilizar um EncoderNamedPreset incorporadi ou utilizar as predefinições personalizadas. Para obter mais informações, veja [Como personalizar as predefinições do codificador](customize-encoder-presets-how-to.md).

Ao criar uma [Transformação](https://docs.microsoft.com/rest/api/media/transforms), primeiro deve verificar se já existe uma com o método **Get**, conforme apresentado no código que se segue.  Nos Serviços de Multimédia v3, os métodos **Get** nas entidades devolverão um valor **nulo** se a entidade não existir (uma verificação não sensível a maiúsculas e minúsculas no nome).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#EnsureTransformExists)]

#### <a name="job"></a>Tarefa

Conforme mencionado acima, o objeto [Transformação](https://docs.microsoft.com/rest/api/media/transforms) é a receita e uma [Tarefa](https://docs.microsoft.com/rest/api/media/jobs) é o pedido real para os Serviços de Multimédia aplicarem essa **Transformação** a um determinado conteúdo de áudio ou vídeo de entrada. A **Tarefa** especifica informações como a localização do vídeo de entrada e a localização da saída.

Neste exemplo, o vídeo de entrada foi carregado do computador local. Se quiser saber como codificar a partir de um URL HTTPS, veja [este](job-input-from-http-how-to.md) artigo.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

### <a name="wait-for-the-job-to-complete"></a>Aguardar a conclusão da Tarefa

A tarefa demora algum tempo a terminar, por isso irá querer receber uma notificação quando for concluída. O exemplo de código abaixo mostra como consultar o serviço para saber o estado da [Tarefa](https://docs.microsoft.com/rest/api/media/jobs). Utilizar uma consulta não é uma prática recomendada para produzir aplicações devido à potencial latência. A consulta poderá ser limitada se for sobreutilizada numa conta. Em alternativa, os programadores devem utilizar o Event Grid.

O Event Grid foi concebido para ter uma elevada disponibilidade, um desempenho consistente e um dimensionamento dinâmico. Com o Event Grid, as aplicações podem escutar e reagir a eventos a partir de praticamente todos os serviços do Azure, bem como de origens personalizadas. O processamento de eventos simples, reativo e baseado em HTTP ajuda-o a criar soluções eficientes através da filtragem e do encaminhamento inteligente de eventos.  Veja [Route events to a custom web endpoint](job-state-events-cli-how-to.md) (Encaminhar eventos para um ponto final de Web personalizado).

Normalmente, a **Tarefa** passa pelos seguintes estados: **Agendada**, **Em fila**, **Em processamento**, **Concluída** (o estado final). Se a tarefa encontrar um erro, obterá um estado de **Erro**. Se a tarefa estiver prestes a ser cancelada, obterá **A cancelar** e **Cancelada** quando terminar.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#WaitForJobToFinish)]

### <a name="get-a-streaminglocator"></a>Obter um StreamingLocator

Depois de concluída a codificação, o passo seguinte consiste em disponibilizar o vídeo no Elemento de saída para reprodução para os clientes. Pode conseguir isto em dois passos: primeiro, crie um [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) e, segundo, crie os URLs de transmissão em fluxo que os clientes podem utilizar. 

O processo de criação de um **StreamingLocator** denomina-se publicação. Por predefinição, o **StreamingLocator** é válido imediatamente depois de efetuar as chamadas de API e dura até serem eliminadas, a menos que configure as horas de início e de fim opcionais. 

Ao criar um [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators), terá de especificar o **StreamingPolicyName** pretendido. Neste exemplo, vai transmitir conteúdo pronto a enviar (ou não encriptado), por isso é utilizada a política de transmissão de pronto a enviar predefinida (**PredefinedStreamingPolicy.ClearStreamingOnly**).

> [!IMPORTANT]
> Quando utilizar uma [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies) personalizada, deve conceber um conjunto limitado dessas políticas para a sua conta dos Serviços de Multimédia e utilizá-las novamente para os StreamingLocators sempre que são necessárias as mesmas opções de encriptação e os mesmos protocolos. A conta dos Serviços de Multimédia tem uma quota para o número de entradas de StreamingPolicy. Não deve criar uma nova StreamingPolicy para cada StreamingLocator.

O código seguinte parte do princípio de que está a chamar a função com um locatorName exclusivo.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateStreamingLocator)]

Apesar de o exemplo neste tópico descrever a transmissão em fluxo, pode utilizar a mesma chamada para criar um StreamingLocator para distribuir o vídeo através da transferência progressiva.

### <a name="get-streaming-urls"></a>Obter os URLs de transmissão

Agora que criou o [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators), pode obter os URLs de transmissão, conforme mostrado em **GetStreamingURLs**. Para criar um URL, terá de concatenar o nome do anfitrião do [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) e o caminho do **StreamingLocator**. Neste exemplo, é utilizado o **StreamingEndpoint** *predefinido*. Quando cria uma conta dos Serviços de Multimédia pela primeira vez, este **StreamingEndpoint** *predefinido* vai estar no estado parado, por isso terá de chamar **Iniciar**.

> [!NOTE]
> Neste método, necessitará do locatorName que foi utilizado ao criar o **StreamingLocator** para o Elemento de saída.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#GetStreamingURLs)]

### <a name="clean-up-resources-in-your-media-services-account"></a>Limpar os recursos na conta dos Serviços de Multimédia

Em geral, deve limpar tudo exceto os objetos que está a planear reutilizar (normalmente, reutiliza as Transformações e mantém os StreamingLocators, etc.). Se quiser que a sua conta seja limpa após fazer experiências, deverá eliminar os recursos que não planeia reutilizar.  Por exemplo, o seguinte código elimina as Tarefas.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CleanUp)]

## <a name="run-the-sample-app"></a>Execute a aplicação de exemplo

1. Prima Ctrl + F5 para executar a aplicação *EncodeAndStreamFiles*.
2. Copie um dos URLs de transmissão da consola.

Este exemplo apresenta os URLs que podem ser utilizados para reproduzir o vídeo através de protocolos diferentes:

![Saída](./media/stream-files-tutorial-with-api/output.png)

## <a name="test-the-streaming-url"></a>Testar o URL de transmissão em fluxo

Para testar a transmissão, este artigo utiliza o Leitor de Multimédia do Azure. 

> [!NOTE]
> Se um leitor estiver alojado num site de https, confirme que atualiza o URL para “https”.

1. Abra um browser e navegue para [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/).
2. Na caixa **URL:**, cole um dos valores do URL de transmissão que recebeu quando executou a aplicação. 
3. Prima **Atualizar Leitor**.

O Leitor de Multimédia do Azure pode ser utilizado para fins de teste, mas não deve ser utilizado num ambiente de produção. 

## <a name="clean-up-resources"></a>Limpar recursos

Se já não precisar de nenhum dos recursos presentes no seu grupo de recursos, incluindo a conta dos Serviços de Multimédia e a conta de armazenamento que criou para este tutorial, elimine o grupo de recursos anteriormente criado.

Execute o seguinte comando da CLI:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="multithreading"></a>Multithreading

Os SDKs dos Serviços de Multimédia do Azure v3 não são seguros para threads. Ao criar aplicações com vários threads, deve gerar e utilizar um novo objeto AzureMediaServicesClient por thread.

## <a name="next-steps"></a>Passos Seguintes

Agora que sabe como carregar, codificar e transmitir o vídeo, veja o artigo seguinte: 

> [!div class="nextstepaction"]
> [Analisar vídeos](analyze-videos-tutorial-with-api.md)
