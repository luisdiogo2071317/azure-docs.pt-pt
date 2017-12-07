---
title: "Proteger os seus conteúdos com Media Services do Azure | Microsoft Docs"
description: "Este artigo dar uma descrição geral da proteção de conteúdos com os Media Services."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 81bc00e1-dcda-4d69-b9ab-8768b793422b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 6475a865b9d1b263bd7cc68c99acdb5f6959531e
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2017
---
# <a name="protecting-content-overview"></a>Proteger conteúdo descrição geral
Os Serviços de Multimédia do Microsoft Azure permite-lhe proteger os seus elementos multimédia desde o momento em que saem do seu computador para armazenamento, processamento e entrega. Os Media Services permite-lhe distribuir os seus conteúdos em direto e a pedido encriptado dinamicamente com avançadas encriptação Standard (AES-128) ou qualquer um dos principais sistemas DRM três: Microsoft PlayReady, Widevine da Google e do FairPlay da Apple. Os Media Services também fornecem um serviço para entrega de chaves AES e DRM licenças (PlayReady, Widevine e do FairPlay) para clientes autorizados. 

A imagem seguinte ilustra o fluxo de trabalho de proteção de conteúdos de Media Services do Azure. 

![Proteger com PlayReady](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

Este artigo explica os conceitos e terminologia relevante para compreender a proteção de conteúdos com AMS. O artigo também fornece ligações para artigos que discutir como proteger o conteúdo. 

## <a name="dynamic-encryption"></a>Encriptação dinâmica
Media Services do Azure permitem entregar o conteúdo encriptado dinamicamente, com o desmarcar chave AES ou encriptação DRM: Microsoft PlayReady, Widevine da Google e do FairPlay da Apple. Atualmente, pode encriptar os seguintes formatos de transmissão em fluxo: HLS, MPEG DASH e transmissão em fluxo uniforme. Não é suportada a encriptação em transferências de transferência progressiva. Cada método de encriptação suporta os seguintes protocolos de transmissão em fluxo:
- AES: MPEG-DASH, uniforme de transmissão em fluxo e HLS
- PlayReady: MPEG-DASH, uniforme de transmissão em fluxo e HLS
- Widevine: MPEG-DASH
- FairPlay: HLS

Para encriptar um recurso, tem de associar uma chave de encriptação de conteúdo com o seu elemento e também configurar uma política de autorização da chave. Chaves de conteúdo podem ser especificadas ou geradas automaticamente dos Media Services.

Terá também de configurar a política de entrega de elementos. Se pretende transmitir um recurso encriptados de armazenamento, certifique-se especificar como pretende entregá-lo ao configurar a política de entrega de elemento.

Quando um fluxo é solicitado por um leitor, os Media Services utiliza a chave especificada encriptar de forma dinâmica o conteúdo com o desmarcar chave AES ou encriptação DRM. Para desencriptar o fluxo, o leitor de pedidos a chave do serviço de entrega de chave de AMS. Para decidir se pretende ou não o utilizador está autorizado para obter a chave, o serviço avalia as políticas de autorização que especificou para a chave.

## <a name="aes-128-clear-key-vs-drm"></a>Chave não encriptada AES-128 vs DRM
Os clientes wonder frequentemente se deve utilizar encriptação AES ou um sistema DRM. A principal diferença entre a utilização de encriptação AES e os sistemas DRM é que, com a encriptação AES, a chave de conteúdo é transmitida para o cliente num formato não encriptado ("em Limpar"). Como resultado, a chave utilizada para encriptar o conteúdo pode ser visualizada num rastreio de rede no cliente em texto não encriptado. AES-128 chave não encriptada é adequado em casos de utilização em que o Visualizador é uma entidade fidedigna (ex: encriptar vídeos empresarias distribuídos dentro de uma empresa para ser visualizada pelos funcionários).

PlayReady, Widevine e FairPlay que todas fornecem um nível mais elevado de encriptação em comparação comparada AES-128 limpar a encriptação de chaves. A chave de conteúdo é transmitida num formato encriptado. Além disso, desencriptação é o identificador num ambiente seguro ao nível do sistema operativo em que é significativamente mais difícil para um utilizador mal intencionado a ataques. DRM recomenda-se em casos de utilização em que o Visualizador pode não ser uma entidade fidedigna e requerem o nível mais elevado de segurança.

## <a name="storage-encryption"></a>Encriptação do armazenamento
Pode utilizar a encriptação de armazenamento para encriptar o seu conteúdo transparente localmente utilizando AES 256 bits encriptação e, em seguida, carregue-o no Storage do Azure onde é armazenado encriptados em pausa. Os elementos protegidos com encriptação do storage são desencriptados automaticamente colocados no sistema de ficheiros encriptados antes da codificação e opcionalmente encriptados novamente antes de serem carregados novamente como um novo elemento de saída. O principal motivo para a encriptação de armazenamento é quando pretender proteger os ficheiros de suporte de dados de entrada de alta qualidade com uma encriptação forte Inativos no disco.

Para fornecer um recurso encriptados de armazenamento, tem de configurar política de entrega de elementos para que os Media Services confie como pretende distribuir os seus conteúdos. Antes do elemento possa ser transmitido, o servidor de transmissão em fluxo desencripta e fluxos de conteúdo através da política de entrega especificado (por exemplo, AES, encriptação comum ou sem encriptação).

## <a name="types-of-encryption"></a>Tipos de encriptação
PlayReady e Widevine utilizam encriptação comum (modo AES CTR). FairPlay utiliza a encriptação de modo AES CBC. Encriptação de limpar chave AES-128 utiliza a encriptação de Envelope.

## <a name="licenses-and-keys-delivery-service"></a>Serviço de entrega de licenças e chaves
Os Media Services fornecem um serviço de entrega de chave para entrega de licenças DRM (PlayReady Widevine, do FairPlay) e chaves AES para clientes autorizados. Pode utilizar [portal do Azure](media-services-portal-protect-content.md), REST API ou o SDK de Media Services para .NET para configurar políticas de autenticação e autorização para as suas licenças e chaves.

## <a name="control-content-access"></a>Conteúdo de controlo de acesso
Pode controlar quem tem acesso ao conteúdo ao configurar a política de autorização da chave de conteúdo. A política de autorização da chave de conteúdo suporta restrição aberta ou token.

### <a name="open-authorization"></a>Autorização aberta
Com uma política de autorização de abrir a chave de conteúdo é enviada para qualquer cliente (nenhuma restrição para).

### <a name="token-authorization"></a>Autorização de token
Com a política de autorização de token restrito, a chave de conteúdo só será envie a um cliente que apresenta um válido JSON Web tokens (JWT) ou simples Web tokens (SWT) no pedido de chave/licença. Este token tem de ser emitido por uma Secure Token serviço (STS). Pode utilizar o Microsoft Active Directory como um STS ou implementar um STS personalizado. O STS tem de ser configurado para criar o token assinado com especificado chave e emitir afirmações que especificou na configuração de restrição de token. O serviço de entrega de chave de Media Services irá devolver a chave de pedido/licença para o cliente se o token é válido e as afirmações no token correspondem às configurado para a chave/licença.

Quando configurar o token restrito a política, tem de especificar a chave de verificação principal, o emissor e o parâmetros público-alvo. A chave de verificação principal contém a chave que o token foi assinado com, emissor é o serviço de token seguro que emite o token. O público-alvo, por vezes denominado âmbito, descreve o objetivo do token ou o recurso autoriza o token de acesso a. O serviço de entrega de chave de Media Services valida que estes valores no token correspondem aos valores no modelo.

## <a name="streaming-urls"></a>URLs de transmissão em fluxo
Se o seu elemento foi encriptado com mais do que um DRM, deve utilizar uma etiqueta de encriptação no URL de transmissão em fluxo: (formato = 'm3u8-aapl' encriptação = 'xxx').

Aplicam as seguintes considerações:
* Pode ser especificado mais do que um tipo de encriptação.
* Tipo de encriptação não tem de ser especificado no URL, se apenas uma encriptação foi aplicada ao elemento.
* Tipo de encriptação é sensível a maiúsculas e minúsculas.
* Podem ser especificados os seguintes tipos de encriptação:  
  * **cenc**: PlayReady ou Widevine (encriptação comum)
  * **cbcs-aapl**: do FairPlay (encriptação AES CBC)
  * **CBC**: para a encriptação de envelope AES (Envelope encriptação)

## <a name="next-steps"></a>Passos seguintes
Os artigos seguintes descrevem os passos seguintes para começar com proteção de conteúdos:
* [Proteger com a encriptação de armazenamento](media-services-rest-storage-encryption.md)
* [Proteger com a encriptação AES](media-services-protect-with-aes128.md)
* [Proteger com PlayReady e/ou Widevine](media-services-protect-with-playready-widevine.md)
* [Proteger com do FairPlay](media-services-protect-hls-with-FairPlay.md)

## <a name="related-links"></a>Ligações Relacionadas
[Preços de entrega de licença de PlayReady de serviços de suporte de dados do Azure explicado](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Como depurar para AES encriptados fluxo Media Services do Azure](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[Autenticação de token JWT](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integrar a aplicação baseada em MVC de OWIN de serviços de suporte de dados do Azure com o Azure Active Directory e restringir a entrega de chave de conteúdo com base em afirmações JWT](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
