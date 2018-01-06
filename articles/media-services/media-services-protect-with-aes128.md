---
title: "Utilizar a encriptação dinâmica AES-128 e o serviço de entrega de chave | Microsoft Docs"
description: "Entrega o conteúdo encriptado com chaves de encriptação AES de 128 bits, utilizando os Media Services do Microsoft Azure. Os Media Services também fornecem o serviço de entrega de chave que fornece as chaves de encriptação para os utilizadores autorizados. Este tópico mostra como encriptar com AES-128 e utilizar o serviço de entrega de chave dinamicamente."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 4d2c10af-9ee0-408f-899b-33fa4c1d89b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: 013c14c00096c9958a732d1f0eaacc9248f57da9
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="use-aes-128-dynamic-encryption-and-the-key-delivery-service"></a>Utilizar a encriptação dinâmica AES-128 e o serviço de entrega de chave
> [!div class="op_single_selector"]
> * [.NET](media-services-protect-with-aes128.md)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 

> [!NOTE]
> Para obter a versão mais recente do Java SDK e começar a desenvolver com o Java, consulte [começar com o cliente de Java SDK de Media Services do Azure](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Para transferir o SDK mais recente do PHP para os Media Services, procure a versão 0.5.7 do pacote Microsoft/WindowsAzure o [repositório Packagist](https://packagist.org/packages/microsoft/windowsazure#v0.5.7).  

## <a name="overview"></a>Descrição geral
> [!NOTE]
> Para obter informações sobre como encriptar o conteúdo com o padrão AES (Advanced Encryption) para a entrega ao Safari no macOS, consulte [esta mensagem de blogue](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).
> Para obter uma descrição geral de como proteger o suporte de dados de conteúdo com encriptação AES, consulte [este vídeo](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption).
> 
> 

 Pode utilizar os serviços de suporte de dados para fornecer HTTP Live Streaming (HLS) e a transmissão em fluxo uniforme encriptada com a AES utilizando chaves de encriptação de 128 bits. Os Media Services também fornecem o serviço de entrega de chave que fornece as chaves de encriptação para os utilizadores autorizados. Se pretender que os serviços de suporte de dados para encriptar um elemento, associe uma chave de encriptação com o elemento e também configurar políticas de autorização da chave. Quando um fluxo é solicitado por um leitor, os Media Services utiliza a chave especificada encriptar de forma dinâmica o conteúdo ao utilizar a encriptação AES. Para desencriptar o fluxo, o leitor de pedidos a chave do serviço de entrega de chave. Para determinar se o utilizador está autorizado para obter a chave, o serviço avalia as políticas de autorização que especificou para a chave.

Os Media Services suportam várias formas de autenticar utilizadores que efetuam pedidos de chave. A política de autorização da chave de conteúdo pode ter um ou mais restrições de autorização, restrições abertas ou tokens. A política de token restrito tem de ser acompanhada por um token emitido por um serviço de token de segurança (STS). Os Media Services suportam tokens no [token web simples](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) e [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) formatos (JWT). Para obter mais informações, consulte [configurar política de autorização da chave de conteúdo](media-services-protect-with-aes128.md#configure_key_auth_policy).

Para tirar partido da encriptação dinâmica, tem de ter um elemento que contenha um conjunto de ficheiros MP4 com velocidade de transmissão múltipla ou ficheiros de origem de Transmissão em Fluxo Uniforme de múltipla transmissão. Terá também de configurar a política de entrega para o elemento (descrito mais à frente neste artigo). Em seguida, com base no formato especificado no URL de transmissão em fluxo, o servidor de transmissão em fluxo a pedido assegura que a transmissão é entregue no protocolo que selecionou. Como resultado, tem de armazenar e pagar apenas pelos ficheiros num único formato de armazenamento. Os serviços de suporte de dados baseia-se e serve a resposta adequada com base nos pedidos de um cliente.

Este artigo é útil para programadores que funcionam em aplicações que entregam multimédia protegida. O artigo mostra como configurar o serviço de entrega de chave com políticas de autorização para que apenas os clientes autorizados podem receber as chaves de encriptação. Também mostra como utilizar a encriptação dinâmica.


## <a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>Encriptação dinâmica AES-128 e fluxo de trabalho de serviço de entrega de chave

Execute os seguintes passos gerais quando encriptar os elementos com AES utilizando o serviço de entrega de chave de Media Services e também utilizando a encriptação dinâmica:

1. [Criar um elemento e carregar ficheiros para o elemento](media-services-protect-with-aes128.md#create_asset).

2. [Codificar o elemento que contém o ficheiro para o conjunto de MP4 de velocidade de transmissão adaptável](media-services-protect-with-aes128.md#encode_asset).

3. [Criar uma chave de conteúdo e associe-a com elemento codificado](media-services-protect-with-aes128.md#create_contentkey). Nos Media Services, a chave de conteúdo contém a chave de encriptação do elemento.

4. [Configurar a política de autorização da chave de conteúdo](media-services-protect-with-aes128.md#configure_key_auth_policy). Tem de configurar a política de autorização da chave de conteúdo. A chave de conteúdo é entregue ao cliente, o cliente tem de cumprir a política.

5. [Configurar a política de entrega para um recurso](media-services-protect-with-aes128.md#configure_asset_delivery_policy). A configuração de política de entrega inclui o URL de aquisição de chave e um vetor de inicialização (IV). (AES-128 requer o mesmo IV para encriptação e desencriptação.) A configuração também inclui o protocolo de entrega (por exemplo, MPEG-DASH, HLS, transmissão em fluxo uniforme ou todos) e o tipo de encriptação dinâmica (por exemplo, envelope ou sem encriptação dinâmica).

    Pode aplicar uma política diferente para cada protocolo no mesmo elemento. Por exemplo, pode aplicar encriptação PlayReady para uniforme/DASH e num envelope AES para HLS. Quaisquer protocolos que não estão definidos numa política de entrega estão bloqueados da transmissão em fluxo. (Um exemplo é se adicionar uma única política que especifica apenas HLS como o protocolo). A exceção é não se tiver nenhuma política de entrega de elemento definida de todo. Em seguida, todos os protocolos são permitidos a limpar.

6. [Criar um localizador OnDemand](media-services-protect-with-aes128.md#create_locator) para obter um URL de transmissão em fluxo.

O artigo mostra também [como uma aplicação cliente pode pedir uma chave do serviço de entrega de chave](media-services-protect-with-aes128.md#client_request).

Pode encontrar um concluída [exemplo .NET](media-services-protect-with-aes128.md#example) no final do artigo.

A imagem seguinte demonstra o fluxo de trabalho descrito anteriormente. Aqui, o token é utilizado para autenticação.

![Proteger com AES-128](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

O resto deste artigo fornece uma explicação, exemplos de código e ligações para tópicos que mostram como atingir as tarefas descritas anteriormente.

## <a name="current-limitations"></a>Limitações atuais
Se adicionar ou atualizar a política de entrega de elementos, tem de eliminar qualquer localizador existente e criar um novo localizador.

## <a id="create_asset"></a>Criar um elemento e carregar ficheiros para o elemento
Para gerir, codificar e transmitir os seus vídeos, primeiro tem de carregar o conteúdo para os Media Services. Após ser carregada, o conteúdo estiver armazenado em segurança na nuvem para processamento adicional e a transmissão em fluxo. 

Para obter mais informações, consulte [carregar ficheiros para uma conta de Media Services](media-services-dotnet-upload-files.md).

## <a id="encode_asset"></a>Codificar o elemento que contém o ficheiro para o definir MP4 de velocidade de transmissão adaptável
Com a encriptação dinâmica, crie um elemento que contenha um conjunto de ficheiros MP4 de velocidade de transmissão múltipla ou ficheiros de origem de transmissão em fluxo uniforme de transmissão múltipla. Em seguida, com base no formato especificado no manifesto ou fragmento do pedido, o servidor de transmissão em fluxo a pedido assegura que recebem o fluxo no protocolo que selecionou. Em seguida, apenas terá de armazenar e pagar pelos ficheiros num único formato de armazenamento. Os serviços de suporte de dados baseia-se e serve a resposta adequada com base nos pedidos de um cliente. Para obter mais informações, consulte [descrição geral de empacotamento dinâmico](media-services-dynamic-packaging-overview.md).

>[!NOTE]
>Quando é criada a conta de Media Services, um ponto final de transmissão em fluxo de predefinido é adicionado à sua conta no estado "Stopped". Para iniciar o conteúdo de transmissão em fluxo e tirar partido do empacotamento dinâmico e a encriptação dinâmica, o ponto final de transmissão em fluxo partir do qual pretende transmitir o conteúdo deve estar no estado "Em execução". 
>
>Além disso, para utilizar o empacotamento dinâmico e a encriptação dinâmica, o elemento tem de conter um conjunto de MP4s de velocidade de transmissão adaptável ou ficheiros de transmissão em fluxo uniforme de velocidade de transmissão adaptável.

Para obter instruções sobre como codificar, consulte [codificar um elemento com o codificador de multimédia Standard](media-services-dotnet-encode-with-media-encoder-standard.md).

## <a id="create_contentkey"></a>Criar uma chave de conteúdo e associe-a com elemento codificado
Nos Media Services, a chave de conteúdo contém a chave na qual pretende encriptar um elemento.

Para obter mais informações, consulte [criar uma chave de conteúdo](media-services-dotnet-create-contentkey.md).

## <a id="configure_key_auth_policy"></a>Configurar a política de autorização da chave de conteúdo
Os Media Services suportam várias formas de autenticar utilizadores que efetuam pedidos de chave. Tem de configurar a política de autorização da chave de conteúdo. A chave pode ser entregue ao cliente, o cliente (leitor) têm de cumprir a política. A política de autorização da chave de conteúdo pode ter um ou mais restrições de acesso autorização, o abrir, token restrição ou restrição de IP.

Para obter mais informações, consulte [configurar uma política de autorização da chave de conteúdo](media-services-dotnet-configure-content-key-auth-policy.md).

## <a id="configure_asset_delivery_policy"></a>Configurar uma política de entrega de elemento
Configure a política de entrega para o seu elemento. Algumas coisas que inclui a configuração de política de entrega de elemento são:

* O URL de aquisição de chave. 
* O vetor de inicialização (IV) a utilizar para a encriptação de envelope. AES-128 requer o mesmo IV para encriptação e desencriptação. 
* O protocolo de entrega asset, (por exemplo, MPEG-DASH, HLS, transmissão em fluxo uniforme ou todos).
* O tipo de encriptação dinâmica (por exemplo, envelope AES) ou sem encriptação dinâmica. 

Para obter mais informações, consulte [configurar uma política de entrega de elemento](media-services-dotnet-configure-asset-delivery-policy.md).

## <a id="create_locator"></a>Criar um OnDemand para obter um URL de transmissão em fluxo o localizador de transmissão em fluxo
Tem de fornecer aos seus utilizadores com o URL de transmissão em fluxo para transmissão em fluxo uniforme, DASH ou HLS.

> [!NOTE]
> Se adicionar ou atualizar a política de entrega de elementos, tem de eliminar qualquer localizador existente e criar um novo localizador.
> 
> 

Para obter instruções sobre como publicar um elemento e compilar um URL de transmissão em fluxo, consulte [Compilar um URL de transmissão em fluxo](media-services-deliver-streaming-content.md).

## <a name="get-a-test-token"></a>Obter um token de teste
Obtenha um token de teste baseado na restrição de token que foi utilizada para a política de autorização de chave.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word "Bearer" in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

Pode utilizar o [leitor de Media Services do Azure](http://amsplayer.azurewebsites.net/azuremediaplayer.html) para testar a sua transmissão em fluxo.

## <a id="client_request"></a>Como pode o cliente solicitar uma chave do serviço de entrega de chave?
No passo anterior, construir o URL que aponta para um ficheiro de manifesto. O cliente tem de extrair as informações necessárias a partir dos ficheiros de manifesto de transmissão em fluxo para efetuar um pedido para o serviço de entrega de chave.

### <a name="manifest-files"></a>Ficheiros de manifesto
O cliente tem de extrair o URL (que também contém conteúdo [kid] do ID de chave) valor a partir do ficheiro de manifesto. O cliente tenta, em seguida obter a chave de encriptação do serviço de entrega de chave. O cliente também tem extraia o valor de IV e utilizá-la para desencriptar o fluxo. O fragmento seguinte mostra o <Protection> elemento do manifesto de transmissão em fluxo uniforme:

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

No caso de HLS, o manifesto de raiz é dividido em ficheiros de segmento. 

Por exemplo, o manifesto de raiz é: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl). Contém uma lista de nomes de ficheiro de segmento.

    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Se abrir um dos ficheiros segmento num editor de texto (por exemplo, http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), it contains # EXT-X-chave, que indica que o ficheiro está encriptado.

    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST

>[!NOTE] 
>Se pretender reproduzir um HLS encriptado AES no Safari, consulte [este blogue](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

### <a name="request-the-key-from-the-key-delivery-service"></a>Solicitar a chave do serviço de entrega de chave

O código seguinte mostra como enviar um pedido para o serviço de entrega de chave de Media Services utilizando uma chave entrega Uri (que foi extraído do manifesto) e um token. (Este artigo não explica como aproveitar SWTs um STS.)

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);

        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;

        var response = request.GetResponse();

        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }

        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();

        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }

## <a name="protect-your-content-with-aes-128-by-using-net"></a>Proteger os seus conteúdos com AES-128, utilizando o .NET

### <a name="create-and-configure-a-visual-studio-project"></a>Criar e configurar um projeto de Visual Studio

1. Configurar o ambiente de desenvolvimento e preencher o ficheiro App. config com as informações de ligação, conforme descrito em [desenvolvimento de Media Services com .NET](media-services-dotnet-how-to-use.md).

2. Adicione os seguintes elementos para appSettings, tal como definido no ficheiro App. config:

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

### <a id="example"></a>Exemplo

Substitua o código no seu ficheiro Program.cs com o código mostrado nesta secção.
 
>[!NOTE]
>Não há um limite de 1.000.000 políticas para diferentes políticas de serviços de suporte de dados (por exemplo, para a política de localizador ou ContentKeyAuthorizationPolicy). Utilize o mesmo ID de política se sempre utilizar as mesmas permissões de acesso/dias. Um exemplo é políticas para os localizadores destinadas a permanecem no local durante muito tempo (políticas de carregamento não). Para obter mais informações, consulte a secção "Limite as políticas de acesso" [gerir recursos e entidades relacionadas com o SDK de Media Services .NET](media-services-dotnet-manage-entities.md#limit-access-policies).

Certifique-se de que atualiza as variáveis para apontar para as pastas onde se encontram os seus ficheiros de entrada.

    [!code-csharp[Main](../../samples-mediaservices-encryptionaes/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs)]

## <a name="media-services-learning-paths"></a>Percursos de aprendizagem dos Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

