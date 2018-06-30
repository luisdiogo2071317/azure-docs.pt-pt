---
title: Utilizar a encriptação dinâmica de Media Services do Azure AES | Microsoft Docs
description: Entrega o conteúdo encriptado com chaves de encriptação AES de 128 bits, utilizando os Media Services do Microsoft Azure. Os Media Services também fornecem o serviço de entrega de chave que fornece as chaves de encriptação para os utilizadores autorizados. Este tópico mostra como encriptar com AES-128 e utilizar o serviço de entrega de chave dinamicamente.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2018
ms.author: juliako
ms.openlocfilehash: 0da5bbee6d0d6401a35c301a8b35dc0efa77da7d
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133033"
---
# <a name="use-aes-128-dynamic-encryption-and-the-key-delivery-service"></a>Utilizar a encriptação dinâmica AES-128 e o serviço de entrega de chave

Pode utilizar os serviços de suporte de dados para fornecer HTTP Live Streaming (HLS), MPEG DASH e transmissão em fluxo uniforme encriptada com a AES utilizando chaves de encriptação de 128 bits. Os Media Services também fornecem o serviço de entrega de chave que fornece as chaves de encriptação para os utilizadores autorizados. Se pretender que para os serviços de suporte de dados encriptar um elemento, associar a chave de encriptação StreamingLocator e também configurar a política de chave de conteúdo. Quando um fluxo é solicitado por um leitor, os Media Services utiliza a chave especificada encriptar de forma dinâmica o conteúdo ao utilizar a encriptação AES. Para desencriptar o fluxo, o leitor de pedidos a chave do serviço de entrega de chave. Para determinar se o utilizador está autorizado para obter a chave, o serviço avalia as políticas de autorização que especificou para a chave.

O artigo baseia-se no [EncryptWithAES](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES) exemplo. O exemplo demonstra como criar uma transformação de codificação que utiliza um configuração predefinido de incorporados para a codificação de velocidade de transmissão adaptável e insere um ficheiro diretamente a partir de um [URL de origem HTTPs](job-input-from-http-how-to.md). O elemento de saída, em seguida, é publicado utilizando a encriptação AES (ClearKey). O resultado do exemplo é um URL para o leitor de suporte de dados do Azure, incluindo o manifesto DASH e o token AES necessários para reproduzir o conteúdo. O exemplo define a expiração do JWT token como 1 hora. Pode abrir um browser e cole o URL resultante para iniciar a página de demonstração do leitor de multimédia do Azure com o URL e o token preenchidos para si já (no seguinte formato: ``` https://ampdemo.azureedge.net/?url= {dash Manifest URL} &aes=true&aestoken=Bearer%3D{ JWT Token here}```.)

> [!NOTE]
> Pode encriptar cada recurso com vários tipos de encriptação (AES-128, PlayReady, Widevine, do FairPlay). Consulte [de transmissão em fluxo e tipos de encriptação de protocolos](content-protection-overview.md#streaming-protocols-and-encryption-types), para ver o que faz sentido combinar.

## <a name="prerequisites"></a>Pré-requisitos

O seguinte é necessário para concluir o tutorial.

* Reveja o [descrição geral da proteção de conteúdo](content-protection-overview.md) artigo.
* Instalar o Visual Studio Code ou o Visual Studio
* Criar uma nova conta de Media Services do Azure, conforme descrito em [este guia de introdução](create-account-cli-quickstart.md).
* Obter as credenciais necessárias para utilizar as APIs de serviços de suporte de dados ao seguir [APIs de acesso](access-api-cli-how-to.md)

## <a name="download-code"></a>Transferir o código

Clone o repositório do GitHub que contém o exemplo .NET completo debatidos neste tópico para o seu computador utilizando o seguinte comando:

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
O exemplo de "encriptar com AES-128" está localizado no [EncryptWithAES](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES) pasta.

> [!NOTE]
> O exemplo cria recursos exclusivos sempre que executar a aplicação. Normalmente, irá reutilizar recursos existentes, como as transformações e políticas (se existente recursos ter configurações necessárias). 

## <a name="start-using-media-services-apis-with-net-sdk"></a>Começar a utilizar as APIs dos Serviços de Multimédia com o SDK .NET

Para começar a utilizar as APIs dos Serviços de Multimédia com o .NET, tem de criar um objeto **AzureMediaServicesClient**. Para criar o objeto, tem de fornecer as credenciais necessárias para o cliente se ligar ao Azure com o Azure AD. No código que clonou no início do artigo, a função **GetCredentialsAsync** cria o objeto ServiceClientCredentials com base nas credenciais fornecidas no ficheiro de configuração local. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateMediaServicesClient)]

## <a name="create-an-output-asset"></a>Criar um elemento de saída  

A saída [Asset](https://docs.microsoft.com/rest/api/media/assets) armazena o resultado da tarefa de codificação. Depois da conclusão da codificação, o elemento de saída é publicado utilizando a encriptação AES (ClearKey).  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateOutputAsset)]
 
## <a name="get-or-create-an-encoding-transform"></a>Obter ou criar uma transformação de codificação

Ao criar uma nova instância [Transformação](https://docs.microsoft.com/rest/api/media/transforms), tem de especificar o que pretende produzir como uma saída. O parâmetro necessário é um objeto **TransformOutput**, conforme apresentado no código abaixo. Cada **TransformOutput** contém uma **Predefinição**. A **Predefinição** descreve as instruções passo a passo das operações de processamento de áudio e/ou vídeo que estão a ser utilizadas para gerir o **TransformOutput** pretendido. O exemplo descrito neste artigo utiliza uma Predefinição incorporada chamada **AdaptiveStreaming**. A Predefinição codifica o vídeo de entrada para uma escala de bits gerada automaticamente (pares de resolução/velocidade de transmissão) com base na resolução e velocidade de transmissão de entrada e produz ficheiros ISO MP4 com vídeo H.264 e áudio AAC correspondente a cada par de resolução/velocidade de transmissão. 

Antes de criar um novo [transformar](https://docs.microsoft.com/rest/api/media/transforms), deve primeiro verificar se já existe um utilizando o **obter** método, conforme mostrado no código que se segue.  Nos Serviços de Multimédia v3, os métodos **Get** nas entidades devolverão um valor **nulo** se a entidade não existir (uma verificação não sensível a maiúsculas e minúsculas no nome).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#EnsureTransformExists)]

## <a name="submit-job"></a>Submeter a tarefa

Conforme mencionado acima, o objeto [Transformação](https://docs.microsoft.com/rest/api/media/transforms) é a receita e uma [Tarefa](https://docs.microsoft.com/rest/api/media/jobs) é o pedido real para os Serviços de Multimédia aplicarem essa **Transformação** a um determinado conteúdo de áudio ou vídeo de entrada. A **Tarefa** especifica informações como a localização do vídeo de entrada e a localização da saída.

Neste tutorial, iremos criar a entrada da tarefa com base num ficheiro que é ingerido diretamente a partir de um [URL de origem HTTPs](job-input-from-http-how-to.md).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#SubmitJob)]

## <a name="wait-for-the-job-to-complete"></a>Aguardar a conclusão da Tarefa

A tarefa demora algum tempo a terminar, por isso irá querer receber uma notificação quando for concluída. O exemplo de código abaixo mostra como consultar o serviço para saber o estado da [Tarefa](https://docs.microsoft.com/rest/api/media/jobs). Utilizar uma consulta não é uma prática recomendada para produzir aplicações devido à potencial latência. A consulta poderá ser limitada se for sobreutilizada numa conta. Em alternativa, os programadores devem utilizar o Event Grid. Veja [Route events to a custom web endpoint](job-state-events-cli-how-to.md) (Encaminhar eventos para um ponto final de Web personalizado).

Normalmente, a **Tarefa** passa pelos seguintes estados: **Agendada**, **Em fila**, **Em processamento**, **Concluída** (o estado final). Se a tarefa encontrar um erro, obterá um estado de **Erro**. Se a tarefa estiver prestes a ser cancelada, obterá **A cancelar** e **Cancelada** quando terminar.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#WaitForJobToFinish)]

## <a name="create-a-contentkey-policy"></a>Criar uma política de ContentKey

Uma chave de conteúdo proporciona acesso seguro aos seus recursos. Terá de criar uma política de chave de conteúdo que configura a forma como a chave de conteúdo é entregue ao final clientes. A chave de conteúdo está associada a StreamingLocator. Os Media Services também fornecem o serviço de entrega de chave que fornece as chaves de encriptação para os utilizadores autorizados. 

Quando um fluxo é solicitado por um leitor, os Media Services utiliza a chave especificada para encriptar o conteúdo de forma dinâmica (neste caso, ao utilizar a encriptação AES.) Para desencriptar o fluxo, o leitor de pedidos a chave do serviço de entrega de chave. Para determinar se o utilizador está autorizado para obter a chave, o serviço avalia as políticas de autorização que especificou para a chave.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="get-a-token"></a>Obter um token
        
Neste tutorial, iremos especificar para a política de chave de conteúdo para ter uma restrição de token. A política de token restrito tem de ser acompanhada por um token emitido por um serviço de tokens seguro (STS). Os Media Services suportam tokens no [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) formatos (JWT) e que é configurar na amostra.

O ContentKeyIdentifierClaim é utilizada na ContentKeyPolicy, o que significa que o token apresentado para o serviço de entrega de chave têm de ter o identificador do ContentKey no mesmo. No exemplo, vamos não especificar uma chave de conteúdo ao criar o StreamingLocator, o sistema cria um aleatório para-nos. Para gerar o teste de token, mas tem de obter ContentKeyId colocar na afirmação ContentKeyIdentifierClaim.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetToken)]

## <a name="create-a-streaminglocator"></a>Criar um StreamingLocator

Depois de concluída a codificação, o passo seguinte consiste em disponibilizar o vídeo no Elemento de saída para reprodução para os clientes. Pode conseguir isto em dois passos: primeiro, crie um [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) e, segundo, crie os URLs de transmissão em fluxo que os clientes podem utilizar. 

O processo de criação de um **StreamingLocator** denomina-se publicação. Por predefinição, o **StreamingLocator** é válido imediatamente depois de efetuar as chamadas de API e dura até serem eliminadas, a menos que configure as horas de início e de fim opcionais. 

Ao criar um [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators), terá de especificar o **StreamingPolicyName** pretendido. Neste tutorial, estamos a utilizar um dos PredefinedStreamingPolicies, que indica como publicar o conteúdo para a transmissão de Media Services do Azure. Neste exemplo, a encriptação de AES Envelope é aplicada (também denominado ClearKey encriptação porque a chave é entregue ao cliente reprodução através de HTTPS e não uma licença DRM).

> [!IMPORTANT]
> Quando utilizar uma [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies) personalizada, deve conceber um conjunto limitado dessas políticas para a sua conta dos Serviços de Multimédia e utilizá-las novamente para os StreamingLocators sempre que são necessárias as mesmas opções de encriptação e os mesmos protocolos. A conta dos Serviços de Multimédia tem uma quota para o número de entradas de StreamingPolicy. Não deve criar uma nova StreamingPolicy para cada StreamingLocator.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateStreamingLocator)]

## <a name="build-a-dash-streaming-url"></a>Criar um TRAÇO URL de transmissão em fluxo

Agora que o [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) tiver sido criado, pode obter os URLs de transmissão em fluxo. Para criar um URL, terá de concatenar o nome do anfitrião do [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) e o caminho do **StreamingLocator**. Neste exemplo, é utilizado o **StreamingEndpoint** *predefinido*. Quando cria uma conta dos Serviços de Multimédia pela primeira vez, este **StreamingEndpoint** *predefinido* vai estar no estado parado, por isso terá de chamar **Iniciar**.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetMPEGStreamingUrl)]

## <a name="clean-up-resources-in-your-media-services-account"></a>Limpar os recursos na conta dos Serviços de Multimédia

Em geral, deve limpar tudo exceto os objetos que está a planear reutilizar (normalmente, reutiliza as Transformações e mantém os StreamingLocators, etc.). Se quiser que a sua conta seja limpa após fazer experiências, deverá eliminar os recursos que não planeia reutilizar.  Por exemplo, o seguinte código elimina as Tarefas.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CleanUp)]

## <a name="next-steps"></a>Passos Seguintes

[Descrição geral](content-protection-overview.md)