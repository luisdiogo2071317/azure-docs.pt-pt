---
title: Carregar, codificar e transmitir em fluxo com os Serviços de Multimédia do Azure | Microsoft Docs
description: Siga os passos deste tutorial para carregar um ficheiro, codificar o vídeo e transmitir o seu conteúdo com os Serviços de Multimédia do Azure.
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
ms.openlocfilehash: 7e5054d6f59bb3e06e4148bd9cfb3caed9fec970
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="tutorial-upload-encode-and-stream-videos-using-apis"></a>Tutorial: Carregar, codificar e transmitir vídeos com as APIs

Este tutorial mostra como carregar, codificar e transmitir ficheiros de vídeo com os Serviços de Multimédia do Azure. Quer transmitir o seu conteúdo nos formatos CMAF, MPEG-DASH ou HLS da Apple para que possam ser reproduzidos numa ampla variedade de dispositivos e browsers. O vídeo tem de ser codificado e empacotada corretamente para o poder transmitir.

Apesar do tutorial explicar os passos para carregar um vídeo, também pode codificar o conteúdo acessível na sua conta dos Serviços de Multimédia através de um URL HTTPS.

![Reproduzir o vídeo](./media/stream-files-tutorial-with-api/final-video.png)

Este tutorial mostrar-lhe como:    

> [!div class="checklist"]
> * Iniciar o Azure Cloud Shell
> * Criar uma conta dos Media Services
> * Aceder à API dos Serviços de Multimédia
> * Configurar a aplicação de exemplo
> * Examinar o código em detalhe
> * Executar a aplicação
> * Testar o URL de transmissão em fluxo
> * Limpar recursos

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Se não tiver o Visual Studio instalado, poderá obter o [Visual Studio Community 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15).

## <a name="download-the-sample"></a>Transferir o exemplo

Clone o repositório do GitHub que contém o exemplo de .NET de transmissão para o computador com o comando seguinte:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="examine-the-code"></a>Examinar o código

Esta secção examina as funções definidas no ficheiro [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs) do projeto *UploadEncodeAndStreamFiles*.

### <a name="start-using-media-services-apis-with-net-sdk"></a>Começar a utilizar as APIs dos Serviços de Multimédia com o SDK .NET

Para começar a utilizar as APIs dos Serviços de Multimédia com o .NET, tem de criar um objeto **AzureMediaServicesClient**. Para criar o objeto, tem de fornecer as credenciais necessárias para o cliente se ligar ao Azure com o Azure AD. Primeiro tem de obter um token e, em seguida, criar um objeto **ClientCredential** a partir do token obtido. No código que clonou no início do artigo, o objeto **ArmClientCredential** é utilizado para obter o token.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateMediaServicesClient)]

### <a name="create-an-input-asset-and-upload-a-local-file-into-it"></a>Criar um elemento de entrada e carregar um ficheiro local para ele 

A função **CreateInputAsset** cria um novo Elemento de entrada e carrega o ficheiro de vídeo local especificado para ele. Este Elemento é utilizado como entrada para a Tarefa de codificação. Nos Serviços de Multimédia v3, a entrada para uma Tarefa pode ser um Elemento ou pode ser conteúdo que tenha disponibilizado na sua conta dos Serviços de Multimédia através de URLs HTTPS. Se quiser saber como codificar a partir de um URL HTTPS, veja [este](job-input-from-http-how-to.md) artigo.  

Nos Serviços de Multimédia v3, deverá utilizar as APIs do Armazenamento do Azure para carregar os ficheiros. O seguinte fragmento de .NET mostra como.

A função seguinte executa estes passos:

* Cria um Elemento 
* Obtém um [URL de SAS](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) gravável para o [contentor de armazenamento](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=windows#upload-blobs-to-the-container) do Elemento
* Carrega o ficheiro para o contentor de armazenamento através do URL de SAS

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

### <a name="create-an-output-asset-to-store-the-result-of-a-job"></a>Criar um elemento de saída para armazenar o resultado de uma tarefa 

O elemento de saída armazena o resultado da tarefa de codificação. O projeto define a função **DownloadResults**, que transfere os resultados deste elemento de saída para a pasta “saída”, para que possa ver o que obteve.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateOutputAsset)]

### <a name="create-a-transform-and-a-job-that-encodes-the-uploaded-file"></a>Criar uma Transformação e uma Tarefa que codifica o ficheiro carregado
Ao codificar ou processar conteúdos nos Serviços de Multimédia, é um padrão comum configurar as definições de codificação como uma receita. Em seguida, deverá submeter uma **Tarefa** para aplicar essa receita a um vídeo. Ao submeter novas Tarefas para cada vídeo novo, está a aplicar essa receita a todos os vídeos na biblioteca. Uma receita nos Serviços de Multimédia chama-se uma **Transformação**. Para obter mais informações, veja [Transformações e tarefas](transform-concept.md). O exemplo descrito neste tutorial define uma receita que codifica o vídeo para transmiti-lo numa variedade de dispositivos iOS e Android. 

#### <a name="transform"></a>Transformação

Ao criar uma nova instância **Transformação**, tem de especificar o que pretende produzir como uma saída. O parâmetro necessário é um objeto **TransformOutput**, conforme apresentado no código abaixo. Cada **TransformOutput** contém uma **Predefinição**. A **Predefinição** descreve as instruções passo a passo das operações de processamento de áudio e/ou vídeo que estão a ser utilizadas para gerir o **TransformOutput** pretendido. O exemplo descrito neste artigo utiliza uma Predefinição incorporada chamada **AdaptiveStreaming**. A Predefinição codifica o vídeo de entrada para uma escala de bits gerada automaticamente (pares de resolução/velocidade de transmissão) com base na resolução e velocidade de transmissão de entrada e produz ficheiros ISO MP4 com vídeo H.264 e áudio AAC correspondente a cada par de resolução/velocidade de transmissão. Para obter informações sobre esta Predefinição, veja [Auto-generating bitrate ladder](autogen-bitrate-ladder.md) (Escala de bits gerada automaticamente).

Pode utilizar outra EncoderNamedPreset incorporada ou utilizar as predefinições personalizadas. 

Ao criar uma **Transformação**, primeiro deve verificar se já existe uma com o método **Get**, conforme apresentado no código que se segue.  Nos Serviços de Multimédia v3, os métodos **Get** nas entidades devolverão um valor **nulo** se a entidade não existir (uma verificação não sensível a maiúsculas e minúsculas no nome).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#EnsureTransformExists)]

#### <a name="job"></a>Tarefa

Conforme mencionado acima, o objeto **Transformação** é a receita e uma **Tarefa** é o pedido real para os Serviços de Multimédia aplicarem essa **Transformação** a um determinado conteúdo de áudio ou vídeo de entrada. A **Tarefa** especifica informações como a localização do vídeo de entrada e a localização da saída.

Neste exemplo, o vídeo de entrada foi carregado do computador local. Se quiser saber como codificar a partir de um URL HTTPS, veja [este](job-input-from-http-how-to.md) artigo.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

### <a name="wait-for-the-job-to-complete"></a>Aguardar a conclusão da Tarefa

O exemplo de código abaixo mostra como consultar o serviço para saber o estado da Tarefa. Utilizar uma consulta não é uma prática recomendada para produzir aplicações devido à potencial latência. A consulta poderá ser limitada se for sobreutilizada numa conta. Em alternativa, os programadores devem utilizar o Event Grid.

O Event Grid foi concebido para ter uma elevada disponibilidade, um desempenho consistente e um dimensionamento dinâmico. Com o Event Grid, as aplicações podem escutar e reagir a eventos a partir de praticamente todos os serviços do Azure, bem como de origens personalizadas. O processamento de eventos simples, reativo e baseado em HTTP ajuda-o a criar soluções eficientes através da filtragem e do encaminhamento inteligente de eventos.  Veja [Route events to a custom web endpoint](job-state-events-cli-how-to.md) (Encaminhar eventos para um ponto final de Web personalizado).

Normalmente, a **Tarefa** passa pelos seguintes estados: **Agendada**, **Em fila**, **Em processamento**, **Concluída** (o estado final). Se a tarefa encontrar um erro, obterá um estado de **Erro**. Se a tarefa estiver prestes a ser cancelada, obterá **A cancelar** e **Cancelada** quando terminar.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#WaitForJobToFinish)]

### <a name="get-a-streaminglocator"></a>Obter um StreamingLocator

Depois de concluída a codificação, o passo seguinte consiste em disponibilizar o vídeo no Elemento de saída para reprodução para os clientes. Pode conseguir isto em dois passos: primeiro, crie um **StreamingLocator** e, segundo, crie os URLs de transmissão em fluxo que os clientes podem utilizar. 

O processo de criação de um **StreamingLocator** denomina-se publicação. Por predefinição, o **StreamingLocator** é válido imediatamente depois de efetuar as chamadas de API e dura até serem eliminadas, a menos que configure as horas de início e de fim opcionais. 

Ao criar um **StreamingLocator**, terá de especificar o **StreamingPolicyName** pretendido. Neste exemplo, vai transmitir conteúdo pronto a enviar ou não encriptado, por isso pode ser utilizada a política de transmissão de pronto a enviar predefinida **PredefinedStreamingPolicy.ClearStreamingOnly**.

> [!IMPORTANT]
> Quando utilizar a StreamingPolicy personalizada, deve conceber um conjunto limitado dessas políticas para a sua conta dos Serviços de Multimédia e utilizá-las novamente para os StreamingLocators sempre que são necessárias as mesmas opções de encriptação e os mesmos protocolos. A conta dos Serviços de Multimédia tem uma quota para o número de entradas de StreamingPolicy. Não deve criar uma nova StreamingPolicy para cada StreamingLocator.

O código seguinte parte do princípio de que está a chamar a função com um locatorName exclusivo.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateStreamingLocator)]

Apesar de o exemplo neste tópico descrever a transmissão em fluxo, pode utilizar a mesma chamada para criar um StreamingLocator para distribuir o vídeo através da transferência progressiva.

### <a name="get-streaming-urls"></a>Obter os URLs de transmissão

Agora que criou um StreamingLocator, pode obter os URLs de transmissão, conforme mostrado em **GetStreamingURLs**. Para criar um URL, terá de concatenar o nome do anfitrião do **StreamingEndpoint** e o caminho do **StreamingLocator**. Neste exemplo, é utilizado o **StreamingEndpoint** *predefinido*. Quando cria uma conta dos Serviços de Multimédia pela primeira vez, este **StreamingEndpoint** *predefinido* vai estar no estado parado, por isso terá de chamar **Iniciar**.

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

Se já não precisar de nenhum dos recursos presentes no seu grupo de recursos, incluindo a conta dos Serviços de Multimédia e a conta de armazenamento que criou para este tutorial, elimine o grupo de recursos anteriormente criado. Pode utilizar a ferramenta **CloudShell**.

No **CloudShell**, execute o seguinte comando:

```azurecli-interactive
az group delete --name amsResourceGroup
```

## <a name="multithreading"></a>Multithreading

Os SDKs dos Serviços de Multimédia do Azure v3 não são seguros para threads. Ao criar aplicações com vários threads, deve gerar e utilizar um novo objeto AzureMediaServicesClient por thread.

## <a name="next-steps"></a>Passos seguintes

Agora que sabe como carregar, codificar e transmitir o vídeo, veja o artigo seguinte: 

> [!div class="nextstepaction"]
> [Analisar vídeos](analyze-videos-tutorial-with-api.md)
