---
title: "Configurar políticas de proteção de conteúdos através do portal do Azure | Microsoft Docs"
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
ms.openlocfilehash: 805e1246dbc984582528d2b351d2f14ab2e811fc
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="configure-content-protection-policies-by-using-the-azure-portal"></a>Configurar políticas de proteção de conteúdos através do portal do Azure
 Com os Media Services do Azure, pode proteger o suporte de dados desde o momento em que deixa o seu computador através de armazenamento, processamento e entrega. Pode utilizar os serviços de suporte de dados para entregar o conteúdo encriptado dinamicamente com a encriptação AES (Advanced Standard) utilizando as chaves de encriptação de 128 bits. Também pode utilizá-lo com encriptação comum (CENC) com PlayReady e/ou Widevine gestão de direitos digitais (DRM) e do FairPlay da Apple. 

Os Media Services fornecem um serviço para entrega de licenças DRM e AES limpar as chaves para clientes autorizados. Pode utilizar o portal do Azure para criar uma política de autorização de chave/licença para todos os tipos de encriptações.

Este artigo demonstra como configurar uma política de proteção de conteúdos através do portal. O artigo mostra também como aplicar encriptação dinâmica aos seus recursos.

## <a name="start-to-configure-content-protection"></a>Começar a configurar a proteção de conteúdos
Para utilizar o portal para configurar a proteção de conteúdos global utilizando a conta de Media Services, siga os passos seguintes:

1. No [portal](https://portal.azure.com/), selecione a sua conta de Media Services.

2. Selecione **definições** > **conteúdo proteção**.

    ![Proteção de conteúdo](./media/media-services-portal-content-protection/media-services-content-protection001.png)

## <a name="keylicense-authorization-policy"></a>Política de autorização de chave/licença
Os Media Services suportam várias formas de autenticar utilizadores que efetuam pedidos de chave ou de licença. Tem de configurar a política de autorização da chave de conteúdo. O cliente, em seguida, tem de cumprir a política antes da chave/licença podem ser fornecida ao mesmo. A política de autorização da chave de conteúdo pode ter um ou mais restrições de autorização, restrições abertas ou tokens.

Pode utilizar o portal para criar uma política de autorização de chave/licença para todos os tipos de encriptações.

### <a name="open-authorization"></a>Autorização aberta
Abra restrição significa que o sistema disponibiliza a chave de qualquer pessoa que faz um pedido de chave. Esta restrição poderão ser úteis para fins de teste. 

### <a name="token-authorization"></a>Autorização de token
A política de token restrito tem de ser acompanhada por um token emitido por um serviço de token de segurança (STS). Os Media Services suportam tokens no token web simples (SWT) e formatos JSON Web tokens (JWT). Serviços de suporte de dados não fornece um STS. Pode criar um STS personalizado ou utilizar o serviço de controlo de acesso do Azure para tokens de problema. O STS tem de ser configurado para criar o token assinado com especificado chave e emitir afirmações que especificou na configuração de restrição de token. Se o token é válido e as afirmações no token correspondem às configurado para a chave (ou uma licença), o serviço de entrega de chave de Media Services devolve a chave de pedido (ou uma licença) para o cliente.

Quando configura a política de token restrito, tem de especificar a chave de verificação principal, o emissor e o parâmetros público-alvo. A chave de verificação principal contém a chave que o token foi assinado com. O emissor é o serviço de token seguro que emite o token. O público-alvo (por vezes denominado âmbito) descreve a intenção do token ou o recurso autoriza o token de acesso a. O serviço de entrega de chave de Media Services valida que estes valores no token correspondem aos valores no modelo.

![Política de autorização de chave/licença](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-license-template"></a>Modelo de licença PlayReady
O modelo de licença PlayReady define a funcionalidade que está ativada na sua licença PlayReady. Para obter mais informações sobre o modelo de licença PlayReady, consulte o [descrição geral do modelo de licença PlayReady de serviços de suporte de dados](media-services-playready-license-template-overview.md).

### <a name="nonpersistent"></a>Nonpersistent
Se configurar uma licença como nonpersistent, está retido na memória apenas enquanto o leitor utiliza a licença.  

![Proteção de conteúdos nonpersistent](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Persistente
Se configurar uma licença como persistente, é guardada no armazenamento persistente no cliente.

![Proteção de conteúdo persistente](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-license-template"></a>Modelo de licença Widevine
O modelo de licença Widevine define a funcionalidade que está ativada no seu licenças Widevine.

### <a name="basic"></a>Básica
Quando seleciona **básico**, o modelo for criado com todos os valores predefinidos.

### <a name="advanced"></a>Avançado
Para mais informações sobre o modelo de direitos Widevine, consulte o [descrição geral do modelo de licença Widevine](media-services-widevine-license-template-overview.md).

![Proteção de conteúdos avançada](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>Configuração do FairPlay
Para ativar a encriptação do FairPlay, selecione **do FairPlay configuração**. Em seguida, selecione o **certificado aplicação** e introduza o **chave de segredo de aplicação**. Para obter mais informações sobre a configuração do FairPlay e requisitos, consulte [proteger a sua HLS conteúdo com do FairPlay da Apple ou Microsoft PlayReady](media-services-protect-hls-with-FairPlay.md).

![Configuração do FairPlay](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Aplicar encriptação dinâmica para o elemento
Para tirar partido da encriptação dinâmica, codificar o ficheiro de origem para um conjunto de ficheiros MP4 de velocidade de transmissão adaptável.

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Selecione um recurso que pretende encriptar
Para ver todos os seus recursos, selecione **definições** > **ativos**.

![Opção de recursos](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Encriptar com AES ou DRM
Quando seleciona **encriptar** para um recurso, verá duas opções: **AES** ou **DRM**. 

#### <a name="aes"></a>AES
Desative a encriptação de chaves está ativada em todos os protocolos de transmissão em fluxo AES: transmissão em fluxo uniforme, HLS e MPEG-DASH.

![Configuração da encriptação](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM
1. Depois de selecionar **DRM**, consulte as políticas de proteção de conteúdos diferentes (que tem de ser configuradas nesta altura) e um conjunto de protocolos de transmissão em fluxo:

    a. **PlayReady e Widevine com MPEG-DASH** encripta dinamicamente a sua transmissão em fluxo MPEG-DASH com PlayReady e Widevine DRMs.

    b. **PlayReady e Widevine com MPEG-DASH + do FairPlay com HLS** encriptar de forma dinâmica a sua transmissão em fluxo MPEG-DASH com PlayReady e Widevine DRMs. Esta opção também encripta os fluxos HLS do FairPlay.

    c. **PlayReady apenas com transmissão em fluxo uniforme, HLS e MPEG-DASH** dinamicamente encripta os fluxos de transmissão em fluxo uniforme, HLS e MPEG-DASH com PlayReady DRM.

    d. **Widevine apenas com MPEG-DASH** dinamicamente encripta o MPEG-DASH com Widevine DRM.
    
    e. **FairPlay apenas com HLS** encripta dinamicamente a sua transmissão em fluxo HLS do FairPlay.

2. Para ativar a encriptação do FairPlay, no **conteúdo definições globais de proteção** painel, selecione **do FairPlay configuração**. Em seguida, selecione o **certificado aplicação**e introduza o **chave de segredo de aplicação**.

    ![Tipo de encriptação](./media/media-services-portal-content-protection/media-services-content-protection009.png)

3. Depois de efetuar a seleção de encriptação, selecione **aplicar**.

>[!NOTE] 
>Se pretender reproduzir um HLS encriptado AES no Safari, consulte a mensagem de blogue [HLS encriptado no Safari](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Passos Seguintes
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

