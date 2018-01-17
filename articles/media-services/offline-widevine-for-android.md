---
title: "Configurar a conta para transmissão em fluxo offline de conteúdo de Widevine protegido - Azure"
description: "Este tópico mostra como configurar a sua conta de Media Services do Azure para a transmissão em fluxo offline de Widevine protegida conteúdo."
services: media-services
keywords: "TRAÇO, DRM, Widevine modo Offline, ExoPlayer, Android"
documentationcenter: 
author: willzhan
manager: steveng
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: willzhan, dwgeo
ms.openlocfilehash: b27ffcbf5749d612e63ba08df0adad72f357a83a
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="offline-widevine-streaming-for-android"></a>Widevine offline de transmissão em fluxo para Android

Além de proteger conteúdo para a transmissão em fluxo online, o suporte de dados de conteúdo a subscrição e rental serviços oferta transferível conteúdo que funciona quando não estiver ligado à internet. Poderá ter de transferir o conteúdo no seu telemóvel ou tablet para reprodução no modo de avião quando flying desligado da rede. Cenários adicionais, em que pode querer transferir conteúdo:

- Alguns fornecedores de conteúdo poderão não permitir a entrega de licença da DRM além do limite de um país. Se um utilizador pretende ver conteúdo enquanto estiverem em deslocação abroad, é necessário transferir offline.
- Em alguns países, está limitado a disponibilidade de Internet e/ou de largura de banda. Os utilizadores podem optar por transferir conteúdo para conseguir vê-lo na resolução suficientemente elevada, experiência de visualização satisfatório.

Este artigo descreve como implementar a reprodução de modo offline para o conteúdo DASH protegido pelo Widevine em dispositivos Android. Offline DRM permite-lhe fornecer a subscrição, rental e compra modelos para o conteúdo, permitindo que os clientes dos seus serviços colocar facilmente conteúdo com os mesmos quando desligado da internet.

Para criar aplicações Android player, iremos realçam três opções:

> [!div class="checklist"]
> * Criar um leitor utilizando a API de ExoPlayer SDK de Java
> * Criar um leitor de utilizar o enlace de Xamarin do ExoPlayer SDK
> * Criar um leitor utilizando a extensão de suporte de dados encriptados (EME) e a extensão de origem de suporte de dados (MSE) no Chrome browser móveis v62 ou posterior

Também o artigo responde a algumas perguntas comuns relacionadas com a transmissão em fluxo offline de conteúdo de Widevine protegido.

## <a name="requirements"></a>Requisitos 

Antes de implementar offline DRM para Widevine em dispositivos Android, deve primeiro:

- Se familiarize com os conceitos apresentados online para proteção de conteúdo com Widevine DRM. Isto é explicado em detalhe nos seguintes documentos/exemplos:
    - [Utilizar Media Services do Azure para fornecer licenças DRM ou chaves AES](media-services-deliver-keys-and-licenses.md)
    - [CENC com Controlo de Acesso e Multi-DRM: Uma Estrutura de Referência e Implementação no Azure e Serviços de Multimédia do Azure](media-services-cenc-with-multidrm-access-control.md)
    - [Utilizar encriptação comum PlayReady e/ou Widevine dinâmico com o .NET](https://azure.microsoft.com/resources/samples/media-services-dotnet-dynamic-encryption-with-drm/)
    - [Utilizar Media Services do Azure para fornecer licenças PlayReady e/ou Widevine com o .NET](https://azure.microsoft.com/resources/samples/media-services-dotnet-deliver-playready-widevine-licenses/)
- Se familiarize com o SDK de ExoPlayer Google para Android, um leitor de vídeo de open source SDK capaz de oferecer suporte a reprodução de Widevine DRM offline. 
    - [ExoPlayer SDK](https://github.com/google/ExoPlayer)
    - [Guia para programadores de ExoPlayer](https://google.github.io/ExoPlayer/guide.html)
    - [Blogue de programador EoPlayer](https://medium.com/google-exoplayer)

## <a name="content-protection-configuration-in-azure-media-services"></a>Configuração de proteção de conteúdos no Media Services do Azure

Quando configurar a proteção de Widevine de um ativo nos Media Services, terá de criar ContentKeyAuthorizationPolicyOption que especificada seguintes três ações:

1. Sistema DRM (Widevine)
2. ContentKeyAuthorizationPolicyRestriction especificação de entrega de chave como conteúdo está autorizado no serviço de entrega de licença (aberto ou token de autorização)
3. Modelo de licença DRM (Widevine)

Para ativar **offline** modo para licenças Widevine, terá de configurar [modelo de licença Widevine](media-services-widevine-license-template-overview.md). No **policy_overrides** objeto, defina o **can_persist** propriedade **verdadeiro** (a predefinição é false). 

O exemplo de código seguinte utiliza o .NET para ativar **offline** modo para licenças Widevine. O código baseia-se no [ utilizando PlayReady e/ou Widevine a encriptação comum dinâmica com .NET](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm) exemplo. 

```
private static string ConfigureWidevineLicenseTemplateOffline(Uri keyDeliveryUrl)
{
    var template = new WidevineMessage
    {
        allowed_track_types = AllowedTrackTypes.SD_HD,
        content_key_specs = new[]
        {
            new ContentKeySpecs
            {
                required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                security_level = 1,
                track_type = "SD"
            }
        },
        policy_overrides = new
        {
            can_play = true,
            can_persist = true,
            //can_renew = true,                             //if you set can_renew = false, you do not need renewal_server_url
            //renewal_server_url = keyDeliveryUrl.ToString(),   //not mandatory, renewal_server_url is needed only if license_duration_seconds is set
            can_renew = false,
            //rental_duration_seconds = 1209600,
            //playback_duration_seconds = 1209600,
            //license_duration_seconds = 1209600
        }
        };

    string configuration = Newtonsoft.Json.JsonConvert.SerializeObject(template);
    return configuration;
}
```

## <a name="configuring-the-android-player-for-offline-playback"></a>Configurar o leitor de Android para reproduzir offline

A forma mais fácil para desenvolver uma aplicação de leitor nativo para dispositivos Android consiste em utilizar o [Google ExoPlayer SDK](https://github.com/google/ExoPlayer), um leitor de vídeo de open source SDK. ExoPlayer suporta funcionalidades que atualmente não suportadas pela nativo MediaPlayer API do Android, incluindo a protocolos de entrega de MPEG-DASH e Microsoft transmissão em fluxo uniforme.

ExoPlayer versão 2.6 e superior inclui muitas classes que suportam a reprodução de Widevine DRM offline. Em particular, a classe de OfflineLicenseHelper fornece funções de utilitário para facilitar a utilização do DefaultDrmSessionManager para transferir, renovar e libertar licenças offline. As classes fornecidas na pasta SDK "biblioteca/core/src/main/java/com/google/android/exoplayer2/offline /" suporta transferir conteúdo offline vídeo.

A lista seguinte de classes facilitar o modo offline no SDK ExoPlayer para Android:

- library/core/src/main/java/com/google/android/exoplayer2/drm/OfflineLicenseHelper.java  
- library/core/src/main/java/com/google/android/exoplayer2/drm/DefaultDrmSession.java
- library/core/src/main/java/com/google/android/exoplayer2/drm/DefaultDrmSessionManager.java
- library/core/src/main/java/com/google/android/exoplayer2/drm/DrmSession.java
- library/core/src/main/java/com/google/android/exoplayer2/drm/ErrorStateDrmSession.java
- library/core/src/main/java/com/google/android/exoplayer2/drm/ExoMediaDrm.java
- library/core/src/main/java/com/google/android/exoplayer2/offline/SegmentDownloader.java
- library/core/src/main/java/com/google/android/exoplayer2/offline/DownloaderConstructorHelper.java 
- library/core/src/main/java/com/google/android/exoplayer2/offline/Downloader.java
- Library/dash/src/Main/Java/com/Google/Android/exoplayer2/Source/dash/offline/DashDownloader.Java 

Os programadores deverá referenciar o [guia para programadores de ExoPlayer](https://google.github.io/ExoPlayer/guide.html) e correspondente [blogue para programadores](https://medium.com/google-exoplayer) durante o desenvolvimento de uma aplicação. Google não lançou um código de implementação nem de exemplo de referência totalmente documentado para a aplicação de ExoPlayer suporte Widevine offline neste momento, pelo que as informações são limitadas para a guia para programadores e o blogue. 

### <a name="working-with-older-android-devices"></a>Trabalhar com dispositivos Android anteriores

Para alguns dispositivos Android anteriores, tem de definir valores para o seguinte **policy_overrides** propriedades (definido no [modelo de licença Widevine](media-services-widevine-license-template-overview.md): **rental_duration_seconds**, **playback_duration_seconds**, e **license_duration_seconds**. Em alternativa, pode defini-los como zero, o que significa infinito ilimitados duração.  

Os valores tem de ser definidos para evitar erros de excesso um número inteiro. Para obter explicações mais sobre o problema, consulte https://github.com/google/ExoPlayer/issues/3150 e https://github.com/google/ExoPlayer/issues/3112. <br/>Se não definir explicitamente, os valores de valores grandes para **PlaybackDurationRemaining** e **LicenseDurationRemaining** atribuídos (por exemplo, 9223372036854775807, o que é o número máximo valor positivo para um número inteiro de 64 bits). Como resultado, a licença Widevine aparece expirada e, por conseguinte, não acontece a desencriptação. 

Este problema não ocorre no Android 5.0 Lollipop ou posterior, uma vez que Android 5.0 é a primeira versão Android, que tem sido concebida para suportar totalmente ARMv8 ([avançadas o RISC Machine](https://en.wikipedia.org/wiki/ARM_architecture)) e as plataformas de 64 bits, enquanto estava Android 4.4 KitKat originalmente concebida para suportar ARMv7 e plataformas de 32 bits como com outras versões mais antigas do Android.

## <a name="using-xamarin-to-build-an-android-playback-app"></a>Utilizar o Xamarin para criar uma aplicação Android de reprodução

Pode encontrar os enlaces de Xamarin para ExoPlayer utilizar as seguintes ligações:

- [Biblioteca de enlaces de Xamarin para a biblioteca de ExoPlayer do Google](https://github.com/martijn00/ExoPlayerXamarin)
- [Enlaces de Xamarin para ExoPlayer NuGet](https://www.nuget.org/packages/Xam.Plugins.Android.ExoPlayer/)

Além disso, consulte o thread seguinte: [Xamarin enlace](https://github.com/martijn00/ExoPlayerXamarin/pull/57). 

## <a name="chrome-player-apps-for-android"></a>Aplicações de leitor do Chrome para Android

Começando com o lançamento do [Chrome para Android v. 62](https://developers.google.com/web/updates/2017/09/chrome-62-media-updates), licença persistente na EME é suportada. [Widevine L1](https://developers.google.com/web/updates/2017/09/chrome-62-media-updates#widevine_l1) agora também é suportada no Chrome para Android. Isto permite-lhe criar aplicações de reprodução offline no Chrome se os utilizadores finais tiverem esta (ou superior) versão do Chrome. 

Além disso, Google tem produzido um exemplo de aplicação de Web transferência progressiva (PWA) e abra-obtido-lo: 

- [Código de origem](https://github.com/GoogleChromeLabs/sample-media-pwa)
- [Versão do Google alojado](https://biograf-155113.appspot.com/ttt/episode-2/) (só funciona no Chrome v 62 e superior em dispositivos Android)

Se atualizar o browser Chrome móvel para v62 (ou superior) um telemóvel Android e teste alojado acima, a aplicação de exemplo, poderá ver essa reprodução de transmissão em fluxo tanto online offline de trabalho.

A aplicação de PWA open source acima foi criada no Node.js. Se pretender alojar a sua própria versão num servidor Ubuntu, tenha em consideração os seguintes problemas de encontrou comuns que possam impedir a reprodução:

1. Problema CORS: O vídeo na aplicação de exemplo de exemplo está alojado no https://storage.googleapis.com/biograf-video-files/videos/. Google tenha configurado a CORS para todos os exemplos de teste alojados no registo de armazenamento do Google na nuvem. Estes são servidos com cabeçalhos CORS, especificando explicitamente a entrada CORS: https://biograf-155113.appspot.com (o domínio no qual google aloja os seus exemplo) a impedir o acesso por quaisquer outros sites. Se tentar, verá o seguinte erro HTTP: Falha ao carregar https://storage.googleapis.com/biograf-video-files/videos/poly-sizzle-2015/mp4/dash.mpd: não existe um cabeçalho de 'Acesso controlo-permitir-origem' está presente no recurso pedido. A origem 'https://13.85.80.81:8080' não é, por conseguinte, a permissão para aceder. Se uma resposta opaco serve às suas necessidades, defina o modo do pedido para 'não-cors' para obter o recurso com a CORS desativada.
2. Problema de certificado: a partir do Chrome v 58, EME para Widevine necessita de HTTPS. Por conseguinte, necessário alojar a aplicação de exemplo através de HTTPS com um X509 certificado. Um certificado de teste habitual não funciona porque os seguintes requisitos: É necessário obter um certificado que cumprem os requisitos mínimos seguintes:
    - Chrome e Firefox requerem a definição do nome alternativo do requerente de SAN existir no certificado
    - O certificado tem de ter fidedigno AC e um certificado autoassinado desenvolvimento não funciona
    - O certificado tem de ter um CN correspondente ao nome DNS do servidor web ou o gateway

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes

### <a name="question"></a>Pergunta

Como posso fornecer licenças persistentes (offline-ativado) para alguns clientes/utilizadores e licenças não persistentes (offline desativado), para que outros? É necessário duplicar o conteúdo e utilizar a chave de conteúdo separado?

### <a name="answer"></a>Resposta
Não é necessário duplicar o conteúdo. Pode simplesmente utilizar uma única cópia de conteúdo e um único ContentKeyAuthorizationPolicy, mas do duas separado ContentKeyAuthorizationPolicyOption:

1. IContentKeyAuthorizationPolicyOption 1: utiliza licença persistente e ContentKeyAuthorizationPolicyRestriction 1 que contém uma afirmação como license_type = "Persistent"
2. IContentKeyAuthorizationPolicyOption 2: utiliza a licença não persistentes e 2 ContentKeyAuthorizationPolicyRestriction que contém uma afirmação como license_type = "Nonpersistent"

Desta forma, quando um pedido de licença é proveniente da aplicação de cliente, o pedido de licença há qualquer diferença. No entanto, para o dispositivo/utilizador final diferentes, o STS deve ter a lógica de negócio para emitir os tokens JWT diferentes que contêm afirmações diferentes (um do license_type dois acima). O valor de afirmações no JWT token será utilizado para decidir pelo serviço de licenciamento para emitir o tipo de licença: persistente ou não persistente.

Isto significa que a proteger o Token serviço (STS) tem de ter as informações de lógica e os/dispositivo de cliente de negócio para adicionar o valor de afirmação correspondente para um token.

### <a name="question"></a>Pergunta

Para os níveis de segurança de Widevine, da Google [documento de descrição geral de arquitetura do Widevine DRM](https://storage.googleapis.com/wvdocs/Widevine_DRM_Architecture_Overview.pdf) documentação, define três níveis de segurança diferentes. No entanto, no [documentação de Media Services do Azure no modelo de licença Widevine](https://docs.microsoft.com/en-us/azure/media-services/media-services-widevine-license-template-overview), descritos cinco níveis de segurança diferentes. O que é a relação ou o mapeamento entre os dois conjuntos diferentes de níveis de segurança?

### <a name="answer"></a>Resposta

Da Google [descrição geral de arquitetura do Widevine DRM](https://storage.googleapis.com/wvdocs/Widevine_DRM_Architecture_Overview.pdf), define os níveis de três segurança seguintes:

1.  Nível de segurança 1: Todos os conteúdos processamento, criptografia e controlo são executados dentro do ambiente de execução fidedigna (TEE). Em alguns modelos de implementação, o processamento de segurança pode ser efetuado no chips diferentes.
2.  Nível de segurança 2: Efetua cryptography (mas não vídeo processamento) dentro de TEE: memórias intermédias desencriptadas são devolvidas para o domínio de aplicação e processadas através de software ou hardware de vídeo em separado. Nível 2, no entanto, criptográfica informação é ainda processada apenas dentro do TEE.
3.  Nível de segurança 3 não tem um TEE no dispositivo. As medidas adequadas podem ser direcionadas para proteger as informações de criptografia e conteúdo desencriptado no sistema operativo anfitrião. Uma implementação de nível 3 também pode incluir um motor de criptografia de hardware, mas que só melhora o desempenho, de segurança não.

AT o mesmo tempo, em [documentação de Media Services do Azure no modelo de licença Widevine](https://docs.microsoft.com/en-us/azure/media-services/media-services-widevine-license-template-overview), a propriedade security_level content_key_specs pode ter os seguintes cinco valores diferentes (cliente robustez os requisitos para reprodução):

1.  É necessária a criptografia baseada em software whitebox.
2.  Criptografia de software e um descodificador oculto é necessário.
3.  O material de chaves e as operações de criptografia tem de ser efetuadas dentro de um hardware de segurança TEE.
4.  A criptografia e decoding de conteúdo tem de ser efetuadas dentro de um hardware de segurança TEE.
5.  A criptografia, decoding e todo o processamento do suporte de dados (comprimidos e descomprimidos) devem ser processadas dentro de um hardware de segurança TEE.

Ambos os níveis de segurança são definidos por Widevine da Google. É a diferença no seu nível de utilização: nível de arquitetura ou nível de API. Os níveis de cinco segurança são utilizados na Widevine API. O objeto content_key_specs que contém security_level é anular a serialização e transmitido para o serviço de entrega global Widevine pelo serviço de licença Widevine de serviços de suporte de dados do Azure. A tabela abaixo mostra o mapeamento entre os dois conjuntos de níveis de segurança.

| **Níveis de segurança definidas na arquitetura de Widevine** |**Níveis de segurança utilizados em Widevine API**|
|---|---| 
| **Nível de segurança 1**: todos os conteúdos processamento, criptografia e controlo são executados dentro do ambiente de execução fidedigna (TEE). Em alguns modelos de implementação, o processamento de segurança pode ser efetuado no chips diferentes.|**security_level = 5**: A criptografia, decoding e todo o processamento do suporte de dados (comprimidos e descomprimidos) devem ser processadas dentro de um hardware de segurança TEE.<br/><br/>**security_level = 4**: criptografia e decoding de conteúdo tem de ser efetuadas dentro de um hardware de segurança TEE.|
**Nível de segurança 2**: efetua cryptography (mas não vídeo processamento) dentro de TEE: memórias intermédias desencriptadas são devolvidas para o domínio de aplicação e processadas através de software ou hardware de vídeo em separado. Nível 2, no entanto, criptográfica informação é ainda processada apenas dentro do TEE.| **security_level = 3**: O material de chaves e as operações de criptografia tem de ser efetuadas dentro de um hardware de segurança TEE. |
| **Nível de segurança 3**: não tem um TEE no dispositivo. As medidas adequadas podem ser direcionadas para proteger as informações de criptografia e conteúdo desencriptado no sistema operativo anfitrião. Uma implementação de nível 3 também pode incluir um motor de criptografia de hardware, mas que só melhora o desempenho, de segurança não. | **security_level = 2**: criptografia de Software e um descodificador oculto é necessária.<br/><br/>**security_level = 1**: é necessária a criptografia baseada em Software whitebox.|

### <a name="question"></a>Pergunta

Por que motivo transferir conteúdo necessário tempo?

### <a name="answer"></a>Resposta

Existem duas formas de melhorar a velocidade de transferência:

1.  Ative a CDN para que os utilizadores finais estejam mais predispostos a atingiu o CDN em vez de ponto final de transmissão em fluxo de origem/para transferência do conteúdo. Se o utilizador pedidos com êxito o ponto final de transmissão em fluxo, cada segmento HLS ou fragmento DASH é dinamicamente embalado e encriptado. Apesar desta latência for na escala de milissegundos para cada segmento/fragmento, quando tiver uma longa vídeo de hora, a latência acumulada pode ser transferências mais a causar grande.
2.  Fornece aos utilizadores finais a opção para transferir seletivamente camadas de qualidade do vídeo e controla áudio em vez de todo o conteúdo. Para o modo offline, não há nenhum ponto para transferir todas as camadas de qualidade. Existem duas formas de alcançar isto:
    1.  Cliente controlada: seleciona automática da aplicação de leitor ou o utilizador seleciona camada de qualidade do vídeo e controla áudio para transferir;
    2.  Serviço controlado: um pode utilizar manifesto dinâmica funcionalidade Media Services do Azure para criar um filtro (global), o que limita HLS listas de reprodução ou MPD TRAÇO a uma camada de qualidade do vídeo único e selecionado controla áudio. Em seguida, o URL de transferência apresentado aos utilizadores finais irão incluir este filtro.

## <a name="summary"></a>Resumo

Este artigo abordados como implementar a reprodução de modo offline para o conteúdo DASH protegido pelo Widevine em dispositivos Android.  Também respondeu às algumas perguntas comuns relacionadas com a transmissão em fluxo offline de conteúdo de Widevine protegido.
