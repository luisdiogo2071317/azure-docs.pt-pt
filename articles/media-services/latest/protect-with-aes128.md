---
title: Utilizar a encriptação dinâmica de serviços de multimédia do Azure AES | Documentos da Microsoft
description: Distribuir os seus conteúdos encriptado com chaves de encriptação AES de 128 bits com os serviços de multimédia do Microsoft Azure. Serviços de multimédia também fornecem o serviço de entrega de chave que fornece as chaves de encriptação para os utilizadores autorizados. Este tópico mostra como encriptar com AES-128 e utilizar o serviço de entrega de chave dinamicamente.
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
ms.date: 07/12/2018
ms.author: juliako
ms.openlocfilehash: 3e5de521570a587b049702dabd3e3692c4227796
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114798"
---
# <a name="use-aes-128-dynamic-encryption-and-the-key-delivery-service"></a>Utilizar a encriptação dinâmica de AES-128 e o serviço de entrega de chave

Pode utilizar os serviços de multimédia para entregar HTTP Live Streaming (HLS), MPEG-DASH e Smooth Streaming encriptado com o AES com chaves de encriptação de 128 bits. Serviços de multimédia também fornecem o serviço de entrega de chave que fornece as chaves de encriptação para os utilizadores autorizados. Se desejar para serviços de multimédia para encriptar um elemento, associar a chave de encriptação StreamingLocator e também configurar a política de chave de conteúdo. Quando um fluxo é solicitado por um jogador, serviços de multimédia utiliza a chave especificada para encriptar dinamicamente o seu conteúdo através de encriptação AES. Para descriptografar o fluxo, o jogador solicita a chave do serviço de entrega de chave. Para determinar se o utilizador está autorizado a obter a chave, o serviço avalia a política de chave de conteúdo que especificou para a chave.

O artigo se baseia a [EncryptWithAES](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES) exemplo. O exemplo demonstra como criar uma transformação de codificação que usa um incorporado na configuração predefinido para a codificação de velocidade de transmissão adaptável e ingere um ficheiro diretamente a partir de um [URL de origem HTTPs](job-input-from-http-how-to.md). O elemento de saída, em seguida, é publicado utilizando a encriptação de AES (ClearKey). A saída do exemplo é um URL para o leitor de multimédia do Azure, incluindo o manifesto de TRAÇO e o token AES necessário para reproduzir o conteúdo. O exemplo define a expiração do JWT token para 1 hora. Pode abrir um browser e cole o URL resultante para iniciar a página de demonstração do leitor de multimédia do Azure com o URL e o token preenchida para si já no seguinte formato: ```https://ampdemo.azureedge.net/?url= {dash Manifest URL} &aes=true&aestoken=Bearer%3D{ JWT Token here}```.

> [!NOTE]
> Pode criptografar cada ativo com vários tipos de encriptação (AES-128, PlayReady, Widevine, FairPlay). Ver [transmissão em fluxo protocolos e tipos de encriptação](content-protection-overview.md#streaming-protocols-and-encryption-types), para ver o que faz sentido combinar.

## <a name="prerequisites"></a>Pré-requisitos

O seguinte é necessário para concluir o tutorial.

* Reveja os [descrição geral da proteção de conteúdo](content-protection-overview.md) artigo.
* Instalar o Visual Studio Code ou o Visual Studio
* Criar uma nova conta de Media Services do Azure, conforme descrito em [este guia de introdução](create-account-cli-quickstart.md).
* Obter as credenciais necessárias para utilizar as APIs dos serviços de suporte de dados seguindo [APIs de acesso](access-api-cli-how-to.md)

## <a name="download-code"></a>Baixe o código

Clonar um repositório do GitHub que contém o exemplo de .NET completo discutidos neste artigo para o seu computador com o seguinte comando:

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
O exemplo de "encriptar com AES-128" está localizado no [EncryptWithAES](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithAES) pasta.

> [!NOTE]
> O exemplo cria recursos exclusivos, sempre que executar a aplicação. Normalmente, irá reutilizar os recursos existentes, como transformações e políticas (se existente recursos têm exigido configurações). 

## <a name="start-using-media-services-apis-with-net-sdk"></a>Começar a utilizar as APIs dos Serviços de Multimédia com o SDK .NET

Para começar a utilizar as APIs dos Serviços de Multimédia com o .NET, tem de criar um objeto **AzureMediaServicesClient**. Para criar o objeto, tem de fornecer as credenciais necessárias para o cliente se ligar ao Azure com o Azure AD. No código que clonou no início do artigo, o **GetCredentialsAsync** função cria o objeto de ServiceClientCredentials com base nas credenciais fornecidas no ficheiro de configuração local. 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateMediaServicesClient)]

## <a name="create-an-output-asset"></a>Criar um elemento de saída  

A saída [Asset](https://docs.microsoft.com/rest/api/media/assets) armazena o resultado da tarefa de codificação.  

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateOutputAsset)]
 
## <a name="get-or-create-an-encoding-transform"></a>Obter ou criar uma transformação de codificação

Ao criar uma nova instância [Transformação](https://docs.microsoft.com/rest/api/media/transforms), tem de especificar o que pretende produzir como uma saída. O parâmetro necessário é um objeto **TransformOutput**, conforme apresentado no código abaixo. Cada **TransformOutput** contém uma **Predefinição**. A **Predefinição** descreve as instruções passo a passo das operações de processamento de áudio e/ou vídeo que estão a ser utilizadas para gerir o **TransformOutput** pretendido. O exemplo descrito neste artigo utiliza uma Predefinição incorporada chamada **AdaptiveStreaming**. A Predefinição codifica o vídeo de entrada para uma escala de bits gerada automaticamente (pares de resolução/velocidade de transmissão) com base na resolução e velocidade de transmissão de entrada e produz ficheiros ISO MP4 com vídeo H.264 e áudio AAC correspondente a cada par de resolução/velocidade de transmissão. 

Antes de criar uma nova [transformar](https://docs.microsoft.com/rest/api/media/transforms), deve primeiro verificar se já existir um com o **obter** método, conforme mostrado no código a seguir.  Nos Serviços de Multimédia v3, os métodos **Get** nas entidades devolverão um valor **nulo** se a entidade não existir (uma verificação não sensível a maiúsculas e minúsculas no nome).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#EnsureTransformExists)]

## <a name="submit-job"></a>Submeter a tarefa

Conforme mencionado acima, o objeto [Transformação](https://docs.microsoft.com/rest/api/media/transforms) é a receita e uma [Tarefa](https://docs.microsoft.com/rest/api/media/jobs) é o pedido real para os Serviços de Multimédia aplicarem essa **Transformação** a um determinado conteúdo de áudio ou vídeo de entrada. A **Tarefa** especifica informações como a localização do vídeo de entrada e a localização da saída.

Neste tutorial, vamos criar a entrada da tarefa com base num arquivo que é ingerido diretamente a partir de um [URL de origem HTTPs](job-input-from-http-how-to.md).

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#SubmitJob)]

## <a name="wait-for-the-job-to-complete"></a>Aguardar a conclusão da Tarefa

A tarefa demora algum tempo a terminar, por isso irá querer receber uma notificação quando for concluída. O exemplo de código abaixo mostra como consultar o serviço para saber o estado da [Tarefa](https://docs.microsoft.com/rest/api/media/jobs). Utilizar uma consulta não é uma prática recomendada para produzir aplicações devido à potencial latência. A consulta poderá ser limitada se for sobreutilizada numa conta. Em alternativa, os programadores devem utilizar o Event Grid. Veja [Route events to a custom web endpoint](job-state-events-cli-how-to.md) (Encaminhar eventos para um ponto final de Web personalizado).

Normalmente, a **Tarefa** passa pelos seguintes estados: **Agendada**, **Em fila**, **Em processamento**, **Concluída** (o estado final). Se a tarefa encontrar um erro, obterá um estado de **Erro**. Se a tarefa estiver prestes a ser cancelada, obterá **A cancelar** e **Cancelada** quando terminar.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#WaitForJobToFinish)]

## <a name="create-a-contentkeypolicy"></a>Criar um ContentKeyPolicy

Uma chave de conteúdo fornece acesso seguro a seus ativos. Tem de criar uma **ContentKeyPolicy** que configura a forma como a chave de conteúdo é entregue para clientes finais. A chave de conteúdo está associada **StreamingLocator**. Serviços de multimédia também fornecem o serviço de entrega de chave que fornece as chaves de encriptação para os utilizadores autorizados. 

Quando um fluxo é solicitado por um jogador, serviços de multimédia utiliza a chave especificada para encriptar dinamicamente o seu conteúdo (neste caso, ao utilizar a encriptação AES.) Para descriptografar o fluxo, o jogador solicita a chave do serviço de entrega de chave. Para determinar se o utilizador está autorizado a obter a chave, o serviço avalia a política de chave de conteúdo que especificou para a chave.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="create-a-streaminglocator"></a>Criar um StreamingLocator

Depois da codificação é concluída e a política de chave de conteúdo é definida, a próxima etapa é tornar o vídeo na saída Asset disponível para os clientes para a reprodução. Para tal em duas etapas: 

1. Criar um [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators)
2. Crie os URLs de transmissão em fluxo que os clientes podem utilizar. 

O processo de criação da **StreamingLocator** é chamado de publicação. Por predefinição, o **StreamingLocator** é válido imediatamente depois de efetuar as chamadas de API e dura até serem eliminadas, a menos que configure as horas de início e de fim opcionais. 

Ao criar um [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators), terá de especificar o **StreamingPolicyName** pretendido. Neste tutorial, estamos a utilizar um dos PredefinedStreamingPolicies, que informa ao Media Services do Azure como publicar o conteúdo de transmissão em fluxo. Neste exemplo, a encriptação AES Envelope é aplicada (também denominado ClearKey encriptação como a chave é enviada para o cliente de reprodução através de HTTPS e não uma licença DRM).

> [!IMPORTANT]
> Quando utilizar uma [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies) personalizada, deve conceber um conjunto limitado dessas políticas para a sua conta dos Serviços de Multimédia e utilizá-las novamente para os StreamingLocators sempre que são necessárias as mesmas opções de encriptação e os mesmos protocolos. A conta dos Serviços de Multimédia tem uma quota para o número de entradas de StreamingPolicy. Não deve criar uma nova StreamingPolicy para cada StreamingLocator.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CreateStreamingLocator)]

## <a name="get-a-test-token"></a>Obter um token de teste
        
Neste tutorial, podemos especificar para a política de chave de conteúdo para ter uma restrição de token. A política de token restrito tem de ser acompanhada por um token emitido por um serviço de tokens seguro (STS). Serviços de multimédia suportam tokens no [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) formatos (JWT) e esse é o que podemos configurar o exemplo.

O ContentKeyIdentifierClaim é utilizada na ContentKeyPolicy, o que significa que o token apresentado para o serviço de entrega de chave tem de ter o identificador do ContentKey nela. No exemplo, nós não especificamos uma chave de conteúdo ao criar o StreamingLocator, o sistema cria uma aleatória para nós. Para gerar o teste de token, podemos tem de obter o ContentKeyId colocar na afirmação ContentKeyIdentifierClaim.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetToken)]

## <a name="build-a-dash-streaming-url"></a>Criar um TRAÇO URL de transmissão em fluxo

Agora que o [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) tiver sido criado, pode obter os URLs de transmissão em fluxo. Para criar um URL, terá de concatenar o nome do anfitrião do [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints) e o caminho do **StreamingLocator**. Neste exemplo, é utilizado o **StreamingEndpoint** *predefinido*. Quando cria uma conta dos Serviços de Multimédia pela primeira vez, este **StreamingEndpoint** *predefinido* vai estar no estado parado, por isso terá de chamar **Iniciar**.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#GetMPEGStreamingUrl)]

## <a name="clean-up-resources-in-your-media-services-account"></a>Limpar os recursos na conta dos Serviços de Multimédia

Em geral, deve limpar tudo exceto os objetos que está a planear reutilizar (normalmente, reutiliza as Transformações e mantém os StreamingLocators, etc.). Se quiser que a sua conta seja limpa após fazer experiências, deverá eliminar os recursos que não planeia reutilizar.  Por exemplo, o seguinte código elimina as Tarefas.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithAES/Program.cs#CleanUp)]

## <a name="next-steps"></a>Passos Seguintes

Verificar como [proteger com o DRM](protect-with-drm.md)
