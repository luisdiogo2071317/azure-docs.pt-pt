---
title: "Configurar políticas de proteção de conteúdos no portal do Azure | Microsoft Docs"
description: "Este artigo demonstra como utilizar o portal do Azure para configurar políticas de proteção de conteúdos. O artigo também mostra como ativar a encriptação dinâmica para os elementos."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 270b3272-7411-40a9-ad42-5acdbba31154
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: ecc766abb5df38813b3eb6dde98cdc9afd24ac6b
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2017
---
# <a name="configuring-content-protection-policies-using-the-azure-portal"></a>Configurar políticas de proteção de conteúdos no portal do Azure
Serviços de suporte de dados do Azure (AMS) da Microsoft permite-lhe proteger suporte de dados desde o momento em que deixa o seu computador através de armazenamento, processamento e entrega. Os Media Services permite-lhe entregar o conteúdo encriptado dinamicamente com AES Advanced Encryption Standard () (utilizando as chaves de encriptação de 128 bits), encriptação comum (CENC) com PlayReady e/ou Widevine DRM e FairPlay da Apple. 

AMS fornece um serviço para entrega de licenças DRM e AES limpar as chaves para clientes autorizados. O portal do Azure permite-lhe criar um **política de autorização de chave/licença** para todos os tipos de encriptações.

Este artigo demonstra como configurar uma política de proteção de conteúdos com o portal do Azure. O artigo mostra também como aplicar encriptação dinâmica aos seus recursos.

## <a name="start-configuring-content-protection"></a>Começar a configurar a proteção de conteúdos
Para utilizar o portal para iniciar a configuração de proteção de conteúdos, global à sua conta de AMS, efetue o seguinte:
1. No [portal do Azure](https://portal.azure.com/) selecione a sua conta de Serviços de Multimédia do Azure.
2. Selecione **definições** > **conteúdo proteção**.

![Proteger conteúdo](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a>Política de autorização de chave/licença
AMS suportam várias formas de autenticar utilizadores que efetuam pedidos de chave ou de licença. A política de autorização da chave de conteúdo tem de ser configurada por si e cumprida pelo cliente para que a chave/licença para delived ao cliente. A política de autorização da chave de conteúdo pode ter um ou mais restrições de autorização: **abrir** ou **token** restrição.

O portal do Azure permite-lhe criar um **política de autorização de chave/licença** para todos os tipos de encriptações.

### <a name="open-authorization"></a>Autorização aberta
Abra restrição significa que o sistema irá fornecer a chave para qualquer pessoa que faz um pedido de chave. Esta restrição poderão ser úteis para fins de teste. 

### <a name="token-authorization"></a>Autorização de token
A política de token restrito tem de ser acompanhada por um token emitido por um Serviço de Token Seguro (STS). Os Media Services suportam tokens no formato simples Web Tokens (SWT) e formato JSON Web tokens (JWT). Serviços de suporte de dados não fornece serviços de Token seguro. Pode criar um STS personalizado ou tirar partido das ACS do Microsoft Azure para tokens de problema. O STS tem de ser configurado para criar o token assinado com especificado chave e emitir afirmações que especificou na configuração de restrição de token. O serviço de entrega de chave de Media Services irá devolver a chave de pedido (ou uma licença) para o cliente se o token é válido e as afirmações na correspondência de token que os que são configurados para a chave (ou uma licença).

Quando configurar o token restrito a política, tem de especificar a chave de verificação principal, o emissor e o parâmetros público-alvo. A chave de verificação principal contém a chave que o token foi assinado com, emissor é o serviço de token seguro que emite o token. O público-alvo (por vezes denominado âmbito) descreve a intenção do token ou o recurso autoriza o token de acesso a. O serviço de entrega de chave de Media Services valida que estes valores no token correspondem aos valores no modelo.

![Proteger conteúdo](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-license-template"></a>Modelo de licença PlayReady
O modelo de licença PlayReady define a funcionalidade ativada na sua licença PlayReady. Para obter informações detalhadas sobre o modelo de licença PlayReady, consulte [descrição de geral do suporte de dados dos serviços PlayReady licença modelo](media-services-playready-license-template-overview.md).

### <a name="non-persistent"></a>Não persistente
Se configurar licença como não persistente, esta só é retida na memória enquanto o leitor está a utilizar a licença.  

![Proteger conteúdo](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Persistente
Se configurar a licença como persistente, é guardada no armazenamento persistente no cliente.

![Proteger conteúdo](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-license-template"></a>Modelo de licença Widevine
O modelo de licença Widevine define a funcionalidade ativada no seu licenças Widevine.

### <a name="basic"></a>Básica
Quando seleciona **básico**, será criado o modelo com todos os valores de predefinições.

### <a name="advanced"></a>Avançado
Para obter informações detalhadas sobre o modelo de direitos Widevine, consulte [descrição geral do modelo de licença Widevine](media-services-widevine-license-template-overview.md).

![Proteger conteúdo](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>Configuração do FairPlay
Para ativar a encriptação do FairPlay, terá de fornecer o certificado de aplicação e a chave de segredo de aplicação (faça) através da opção de configuração do FairPlay. Para obter informações detalhadas sobre a configuração do FairPlay e requisitos, consulte [isto](media-services-protect-hls-with-FairPlay.md) artigo.

![Proteger conteúdo](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Aplicar encriptação dinâmica para o elemento
Para tirar partido da encriptação dinâmica, terá de codificar o ficheiro de origem para um conjunto de ficheiros MP4 de velocidade de transmissão adaptável.

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Selecione um recurso que pretende encriptar
Para ver todos os seus recursos, selecione **definições** > **ativos**.

![Proteger conteúdo](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Encriptar com AES ou DRM
Depois de premir **encriptar** num recurso, são apresentadas duas opções: **AES** ou **DRM**. 

#### <a name="aes"></a>AES
Desmarque a encriptação de chaves será ativada em todos os protocolos de transmissão em fluxo AES: transmissão em fluxo uniforme, HLS e MPEG-DASH.

![Proteger conteúdo](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM
Quando selecionar o separador DRM, são apresentados com diferentes opções de políticas de proteção de conteúdos (que tem de ter configurado por agora) + um conjunto de protocolos de transmissão em fluxo.

* **PlayReady e Widevine com MPEG-DASH** -encriptará dinamicamente a sua transmissão em fluxo MPEG-DASH com PlayReady e Widevine DRMs.
* **PlayReady e Widevine com MPEG-DASH + do FairPlay com HLS** -será dinamicamente encriptar fluxo MPEG-DASH com PlayReady e Widevine DRMs. Também irá encriptar o seu fluxos HLS do FairPlay.
* **PlayReady apenas com transmissão em fluxo uniforme, HLS e MPEG-DASH** -dinamicamente encriptará transmissão em fluxo uniforme, HLS, transmissões em fluxo MPEG-DASH com PlayReady DRM.
* **Widevine apenas com MPEG-DASH** -será encriptar de forma dinâmica, MPEG-DASH com Widevine DRM.
* **FairPlay apenas com HLS** -encriptará dinamicamente a sua transmissão em fluxo HLS do FairPlay.

Para ativar a encriptação do FairPlay, terá de fornecer o certificado de aplicação e a chave de segredo de aplicação (faça) através da opção de configuração do FairPlay do painel de definições de proteção de conteúdos.

![Proteger conteúdo](./media/media-services-portal-content-protection/media-services-content-protection009.png)

Depois de efetuar a seleção de encriptação, prima **aplicar**.

>[!NOTE] 
>Se estiver a planear reproduzir uma AES encriptados HLS no Safari, consulte o [encriptados HLS blogue Safari](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Passos seguintes
Rever os percursos de aprendizagem dos Serviços de Multimédia

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

