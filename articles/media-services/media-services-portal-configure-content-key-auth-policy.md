---
title: "Configure uma política de autorização da chave de conteúdo utilizando o portal do Azure | Microsoft Docs"
description: "Saiba como configurar uma política de autorização para uma chave de conteúdo."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ee82a3fa-c34b-48f2-a108-8ba321f1691e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: a0ab954bda3340b9010b16f54e343933808cc463
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="configure-a-content-key-authorization-policy"></a>Configure uma política de autorização da chave de conteúdo
[!INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Descrição geral
 Pode utilizar os Media Services do Azure para entregar MPEG-DASH, transmissão em fluxo uniforme e HTTP Live Streaming (HLS) protegidas com encriptação AES (Advanced Standard) utilizando as chaves de encriptação de 128 bits ou [PlayReady gestão de direitos digitais (DRM)](https://www.microsoft.com/playready/overview/). Com os Media Services, também possa fornecer transmissões em fluxo DASH encriptados com Widevine DRM. Tanto PlayReady como Widevine são encriptados de acordo com a especificação de encriptação (ISO/IEC 23001 7 CENC) comum.

Os Media Services também fornecem um serviço de entrega de licença/chave partir da qual os clientes podem obter chaves AES ou licenças PlayReady/Widevine para reproduzir o conteúdo encriptado.

Este artigo mostra como utilizar o portal do Azure para configurar a política de autorização da chave de conteúdo. A chave mais tarde pode ser utilizada para encriptar o conteúdo de forma dinâmica. Atualmente, pode encriptar formatos de transmissão em fluxo uniforme, MPEG DASH e HLS. Não é possível encriptar transferências de transferência progressiva.

Quando um leitor de solicita um fluxo que está definido como seja encriptado dinamicamente, o Media Services utiliza a chave configurada para encriptar o conteúdo de forma dinâmica através de encriptação AES ou DRM. Para desencriptar o fluxo, o leitor de pedidos a chave do serviço de entrega de chave. Para determinar se o utilizador está autorizado para obter a chave, o serviço avalia as políticas de autorização que especificou para a chave.

Se planear ter várias chaves de conteúdo ou pretender especificar um URL de serviço de entrega de licença/chave que não seja o serviço de entrega de chave de Media Services, utilize o SDK .NET dos Media Services ou REST APIs. Para obter mais informações, consulte:

* [Configurar uma política de autorização da chave de conteúdo utilizando o SDK de .NET de Media Services](media-services-dotnet-configure-content-key-auth-policy.md)
* [Configurar uma política de autorização da chave de conteúdo utilizando a API de REST de serviços de suporte de dados](media-services-rest-configure-content-key-auth-policy.md)

### <a name="some-considerations-apply"></a>São aplicáveis algumas considerações
* Quando é criada a conta de Media Services, um ponto final de transmissão em fluxo de predefinido é adicionado à sua conta no estado "Stopped". Para iniciar o conteúdo de transmissão em fluxo e tirar partido do empacotamento dinâmico e a encriptação dinâmica, o ponto final de transmissão em fluxo deve estar no estado "Em execução". 
* O elemento tem de conter um conjunto de MP4s de velocidade de transmissão adaptável ou ficheiros de transmissão em fluxo uniforme de velocidade de transmissão adaptável. Para obter mais informações, consulte [codificar um elemento](media-services-encode-asset.md).
* O serviço de entrega de chave coloca em cache ContentKeyAuthorizationPolicy e os respetivos objetos relacionados (opções de política e restrições) para 15 minutos. Pode criar um ContentKeyAuthorizationPolicy e especificar para utilizar uma restrição de token, testá-lo e, em seguida, atualize a política para a restrição aberta. Este processo demora cerca de 15 minutos antes dos comutadores de política para a versão aberta.
* Dos serviços de suporte de dados de ponto final de transmissão em fluxo define o valor do cabeçalho do acesso-controlo-permitir-origem CORS na resposta prévia como caráter universal "\*". Este valor funciona bem com a maioria dos jogadores, incluindo o leitor de multimédia do Azure, Roku e JWPlayer e outras pessoas. No entanto, algumas jogadores que utilizam dash.js não funcionam porque, com o modo de credenciais definido como "incluem", XMLHttpRequest no respetivo dash.js não permite o caráter universal "\*" como o valor do acesso controlo-permitir-origem. Como uma solução para esta limitação no dash.js, se alojar o cliente de um único domínio, os Media Services pode especificar esse domínio no cabeçalho de resposta de verificação prévia. Para obter ajuda, abra um pedido de suporte através do portal do Azure.

## <a name="configure-the-key-authorization-policy"></a>Configurar a política de autorização da chave
Para configurar a política de autorização da chave, selecione o **PROTEÇÃO de conteúdos** página.

Os Media Services suportam várias formas de autenticar utilizadores que efetuam pedidos de chave. A política de autorização da chave de conteúdo pode ter de abrir, token ou restrições de autorização de IP. (O IP pode ser configurado com REST ou o SDK .NET).

### <a name="open-restriction"></a>Restrição aberta
A restrição abra significa que o sistema disponibiliza a chave de qualquer pessoa que faz um pedido de chave. Esta restrição poderão ser úteis para fins de teste.

![OpenPolicy][open_policy]

### <a name="token-restriction"></a>Restrição de token
Para escolher a política de token restrito, selecione o **TOKEN** botão.

A política de token restrito tem de ser acompanhada por um token emitido por um serviço de token de segurança (STS). Os Media Services suportam tokens no token web simples ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)) e formatos JSON Web tokens (JWT). Para obter mais informações, consulte [autenticação JWT](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).

Serviços de suporte de dados não fornece STS. Pode criar um STS personalizado ou utilizar o serviço de controlo de acesso do Azure para tokens de problema. O STS tem de ser configurado para criar o token assinado com especificado chave e emitir afirmações que especificou na configuração de restrição de token. Se o token é válido e as afirmações no token correspondem às configurado para a chave de conteúdo, o serviço de entrega de chave de Media Services devolve a chave de encriptação para o cliente. Para obter mais informações, consulte [utilize serviço de controlo de acesso do Azure para tokens de problema](http://mingfeiy.com/acs-with-key-services).

Quando configura a política de token restrito, tem de especificar a chave de verificação principal, o emissor e o parâmetros público-alvo. A chave de verificação principal contém a chave que o token foi assinado com. O emissor é o STS que emite o token. O público-alvo (por vezes denominado âmbito) descreve a intenção do token ou o recurso autoriza o token de acesso a. O serviço de entrega de chave de Media Services valida que estes valores no token correspondem aos valores no modelo.

### <a name="playready"></a>PlayReady
Quando protege o conteúdo com PlayReady, uma das coisas que tem de especificar na sua política de autorização é uma cadeia de carateres XML que define o modelo de licença PlayReady. Por predefinição, está definida a seguinte política:

    <PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
          <LicenseTemplates>
            <PlayReadyLicenseTemplate><AllowTestDevices>true</AllowTestDevices>
              <ContentKey i:type="ContentEncryptionKeyFromHeader" />
              <LicenseType>Nonpersistent</LicenseType>
              <PlayRight>
                <AllowPassingVideoContentToUnknownOutput>Allowed</AllowPassingVideoContentToUnknownOutput>
              </PlayRight>
            </PlayReadyLicenseTemplate>
          </LicenseTemplates>
        </PlayReadyLicenseResponseTemplate>

Pode selecionar o **importar o xml de política** botão e fornecer um XML diferente que está em conformidade com o esquema XML definido no [descrição geral do modelo de licença PlayReady de serviços de suporte de dados](media-services-playready-license-template-overview.md).

## <a name="next-steps"></a>Passos Seguintes
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

