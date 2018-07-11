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
ms.date: 06/28/2018
ms.author: juliako
ms.openlocfilehash: e5b8cc9285d9637a04c4bec47b0185652aa7c84e
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114229"
---
# <a name="tutorial-analyze-videos-with-azure-media-services"></a>Tutorial: analisar vídeos com os Serviços de Multimédia do Azure 

Este tutorial mostra-lhe como analisar vídeos com os Serviços de Multimédia do Microsoft Azure. Existem muitos cenários em que pode querer obter mais informações sobre conteúdos de áudio ou vídeos gravados. Por exemplo, para alcançar uma maior satisfação do cliente, as organizações podem executar o processamento de voz em texto para converter gravações de suporte de clientes num catálogo pesquisável, com índices e dashboards. Em seguida, podem obter informações sobre a empresa, tal como uma lista de queixas comuns, origens dessas queixas, etc.

Este tutorial mostrar-lhe como:    

> [!div class="checklist"]
> * Criar uma conta dos Media Services
> * Aceder à API dos Serviços de Multimédia
> * Configurar a aplicação de exemplo
> * Examinar o código que analisa o vídeo especificado
> * Executar a aplicação
> * Examinar o ficheiro de saída
> * Limpar recursos

> [!Note]
> Utilize o portal do Azure, conforme descrito em [Dimensionar processamento de suporte de dados](../previous/media-services-scale-media-processing-overview.md), para definir a sua conta de Serviços de Multimédia para 10 Unidades Reservadas de Multimédia S3.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Se não tiver o Visual Studio instalado, poderá obter o [Visual Studio Community 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15).

## <a name="download-the-sample"></a>Transferir o exemplo

Clone o repositório do GitHub que contém o exemplo de .NET para o seu computador com o seguinte comando:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```

O exemplo está localizado na pasta [AnalyzeVideos](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/AnalyzeVideos).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [media-services-cli-create-v3-account-include](../../../includes/media-services-cli-create-v3-account-include.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="examine-the-code-that-analyzes-the-specified-video"></a>Examinar o código que analisa o vídeo especificado

Esta secção examina funções definidas no ficheiro [Program.cs](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/AnalyzeVideos/Program.cs) do projeto *AnalyzeVideos*.

O exemplo realiza as seguintes ações:

1. Cria uma transformação e uma tarefa que analisa o vídeo.
2. Cria um elemento de entrada e carrega o vídeo para ele. O elemento é utilizado como entrada da tarefa.
3. Cria um elemento de saída que armazena a saída da tarefa. 
4. Submete a tarefa.
5. Verifica o estado da tarefa.
6. Transfere os ficheiros que resultaram da execução da tarefa. 

### <a name="start-using-media-services-apis-with-net-sdk"></a>Começar a utilizar as APIs dos Serviços de Multimédia com o SDK .NET

Para começar a utilizar as APIs dos Serviços de Multimédia com o .NET, tem de criar um objeto **AzureMediaServicesClient**. Para criar o objeto, tem de fornecer as credenciais necessárias para o cliente se ligar ao Azure com o Azure AD. No código que clonou no início do artigo, a função **GetCredentialsAsync** cria o objeto ServiceClientCredentials com base nas credenciais fornecidas no ficheiro de configuração local. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateMediaServicesClient)]

### <a name="create-an-input-asset-and-upload-a-local-file-into-it"></a>Criar um elemento de entrada e carregar um ficheiro local para ele 

A função **CreateInputAsset** cria um novo [Elemento](https://docs.microsoft.com/rest/api/media/assets) de entrada e carrega o ficheiro de vídeo local especificado para ele. Este Elemento é utilizado como entrada para a Tarefa de codificação. Nos Serviços de Multimédia v3, a entrada para uma Tarefa pode ser um Elemento ou pode ser conteúdo que tenha disponibilizado na sua conta dos Serviços de Multimédia através de URLs HTTPS. Se quiser saber como codificar a partir de um URL HTTPS, veja [este](job-input-from-http-how-to.md) artigo.  

Nos Serviços de Multimédia v3, deverá utilizar as APIs do Armazenamento do Azure para carregar os ficheiros. O seguinte fragmento de .NET mostra como.

A função seguinte executa estes passos:

* Cria um Elemento 
* Obtém um [URL de SAS](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) gravável para o [contentor de armazenamento](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet?tabs=windows#upload-blobs-to-the-container) do Elemento
* Carrega o ficheiro para o contentor de armazenamento através do URL de SAS

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateInputAsset)]

### <a name="create-an-output-asset-to-store-the-result-of-the-job"></a>Criar um elemento de saída para armazenar o resultado da tarefa 

O [Elemento](https://docs.microsoft.com/rest/api/media/assets) de saída armazena o resultado da sua tarefa. O projeto define a função **DownloadResults**, que transfere os resultados deste elemento de saída para a pasta “saída”, para que possa ver o que obteve.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CreateOutputAsset)]

### <a name="create-a-transform-and-a-job-that-analyzes-videos"></a>Criar uma transformação e uma tarefa que analisa vídeos

Ao codificar ou processar conteúdos nos Serviços de Multimédia, é comum configurar as definições de codificação como uma receita. Em seguida, deverá submeter uma **Tarefa** para aplicar essa receita a um vídeo. Ao submeter novas Tarefas para cada vídeo novo, está a aplicar essa receita a todos os vídeos na biblioteca. Uma receita nos Serviços de Multimédia chama-se uma **Transformação**. Para obter mais informações, veja [Transforms and jobs](transform-concept.md) (Transformações e tarefas). O exemplo descrito neste tutorial define uma receita que analisa o vídeo especificado. 

#### <a name="transform"></a>Transformar

Ao criar uma nova instância [Transformar](https://docs.microsoft.com/rest/api/media/transforms), tem de especificar a saída resultante que pretende. O parâmetro necessário é um objeto **TransformOutput**, conforme apresentado no código acima. Cada **TransformOutput** contém uma **Predefinição**. A **Predefinição** descreve instruções passo a passo das operações de processamento de áudio e/ou vídeo que estão a ser utilizadas para gerir o **TransformOutput** pretendido. Neste exemplo, a predefinição **VideoAnalyzerPreset** é utilizada e o idioma ("en-US") é transmitido para o construtor. Esta predefinição permite-lhe extrair múltiplas informações de vídeo e áudio a partir de um vídeo. Pode utilizar a predefinição **AudioAnalyzerPreset** se precisar de extrair múltiplas informações de áudio a partir de um vídeo. 

Ao criar uma **Transformação**, primeiro deve verificar se já existe uma com o método **Get**, conforme apresentado no código que se segue.  Nos Serviços de Multimédia v3, os métodos **Get** nas entidades devolverão um valor **nulo** se a entidade não existir (uma verificação não sensível a maiúsculas e minúsculas no nome).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#EnsureTransformExists)]

#### <a name="job"></a>Tarefa

Conforme mencionado acima, o objeto [Transformação](https://docs.microsoft.com/rest/api/media/transforms) é a receita e uma [Tarefa](https://docs.microsoft.com/en-us/rest/api/media/jobs) é o pedido real para os Serviços de Multimédia aplicarem essa **Transformação** a um determinado conteúdo de áudio ou vídeo de entrada. A **Tarefa** especifica informações como a localização do vídeo de entrada e a localização para a saída. Pode especificar a localização do seu vídeo com: URLs HTTPS, URLs SAS ou Recursos que estão na sua conta dos Serviços de Multimédia. 

Neste exemplo, a entrada da tarefa é um vídeo local.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#SubmitJob)]

### <a name="wait-for-the-job-to-complete"></a>Aguarde até que a tarefa termine

A tarefa demora algum tempo a terminar, por isso irá querer receber uma notificação quando for concluída. Existem algumas opções diferentes para ser notificado sobre a conclusão da [Tarefa](https://docs.microsoft.com/en-us/rest/api/media/jobs). A opção mais simples (que é apresentada aqui) é utilizar uma consulta. 

Utilizar uma consulta não é uma prática recomendada para produzir aplicações devido à potencial latência. A consulta poderá ser limitada se for sobreutilizada numa conta. Em alternativa, os programadores devem utilizar o Event Grid.

O Event Grid foi concebido para ter uma elevada disponibilidade, um desempenho consistente e um dimensionamento dinâmico. Com o Event Grid, as aplicações podem escutar e reagir a eventos a partir de praticamente todos os serviços do Azure, bem como de origens personalizadas. O processamento de eventos simples, reativo e baseado em HTTP ajuda-o a criar soluções eficientes através da filtragem e do encaminhamento inteligente de eventos. Veja [Route events to a custom web endpoint](job-state-events-cli-how-to.md) (Encaminhar eventos para um ponto final de Web personalizado).

Normalmente, a **Tarefa** passa pelos seguintes estados: **Agendada**, **Em fila**, **Em processamento**, **Concluída** (o estado final). Se a tarefa encontrar um erro, obterá um estado de **Erro**. Se a tarefa estiver a ser cancelada, verá **A cancelar** e **Cancelada** quando terminar.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#WaitForJobToFinish)]

### <a name="download-the-result-of-the-job"></a>Transferir o resultado da tarefa

A seguinte função transfere os resultados do [Elemento](https://docs.microsoft.com/rest/api/media/assets) de saída para a pasta "saída", para que possa examinar os resultados da tarefa. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#DownloadResults)]

### <a name="clean-up-resource-in-your-media-services-account"></a>Limpar os recursos na conta dos Serviços de Multimédia

Em geral, deve limpar tudo, exceto os objetos que está a planear reutilizar (normalmente, reutiliza as Transformações e mantém os StreamingLocators). Se quiser que a sua conta seja limpa após fazer experiências, deverá eliminar os recursos que não planeia reutilizar. Por exemplo, o seguinte código elimina as Tarefas.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/AnalyzeVideos/Program.cs#CleanUp)]

## <a name="run-the-sample-app"></a>Execute a aplicação de exemplo

Prima Ctrl+F5 para executar a aplicação *AnalyzeVideos*.

Quando executa o programa, a tarefa produz miniaturas para cada rosto que encontra no vídeo. Também produz o ficheiro insights.json.

## <a name="examine-the-output"></a>Examinar o ficheiro de saída

O ficheiro de saída para analisar vídeos chama-se insights.json. Este ficheiro contém informações sobre o seu vídeo. Encontrará a descrição dos elementos contidos no ficheiro json no artigo [Informações de multimédia](intelligence-concept.md).

## <a name="clean-up-resources"></a>Limpar recursos

Se já não precisar de nenhum dos recursos presentes no seu grupo de recursos, incluindo a conta dos Serviços de Multimédia e a conta de armazenamento que criou para este tutorial, elimine o grupo de recursos anteriormente criado. Pode utilizar a ferramenta **CloudShell**.

No **CloudShell**, execute o seguinte comando:

```azurecli-interactive
az group delete --name amsResourceGroup
```

## <a name="multithreading"></a>Multithreading

Os SDKs dos Serviços de Multimédia do Azure v3 não são seguros para threads. Ao trabalhar com aplicações com vários threads, deve gerar um novo objeto AzureMediaServicesClient por thread.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Tutorial: upload, encode, and stream files](stream-files-tutorial-with-api.md) (Tutorial: carregar, codificar e transmitir ficheiros)
