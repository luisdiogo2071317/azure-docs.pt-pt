---
title: Proteger os seus conteúdos com Media Services do Azure | Microsoft Docs
description: Este artigo dar uma descrição geral da proteção de conteúdos com os Media Services.
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
ms.date: 06/25/2018
ms.author: juliako
ms.openlocfilehash: 28c10d7c478ea7a9b1d1bfa91da79452eba3a4b6
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37132991"
---
# <a name="content-protection-overview"></a>Descrição geral da proteção de conteúdo

Pode utilizar os Media Services do Azure para proteger o suporte de dados desde o momento em que deixa o seu computador através de armazenamento, processamento e entrega. Com os Media Services, pode fornecer o conteúdo em direto e a pedido encriptado dinamicamente com avançadas encriptação Standard (AES-128) ou qualquer um dos sistemas de gestão (DRM) três direitos digitais principais: Microsoft PlayReady, Widevine da Google e do FairPlay da Apple. Os Media Services também fornecem um serviço para entrega de chaves AES e DRM licenças (PlayReady, Widevine e do FairPlay) para clientes autorizados. 

A imagem seguinte ilustra o fluxo de trabalho de proteção de conteúdos de Media Services: 

![Proteger conteúdo](./media/content-protection/content-protection.png)

&#42;*suporta a encriptação dinâmica AES-128 "chave não encriptada", CBCS e CENC. Para mais informações, consulte a matriz de suporte [aqui](#streaming-protocols-and-encryption-types).*

Este artigo explica os conceitos e terminologia relevante para compreender a proteção de conteúdos com os Media Services. O artigo também fornece ligações para artigos que discutir como proteger o conteúdo. 

## <a name="main-components-of-the-content-protection-system"></a>Componentes principais do sistema de proteção de conteúdos

Para concluir com êxito a estrutura de sistema/aplicação "proteção de conteúdos", tem de compreender totalmente o âmbito do esforço do utilizador. A lista seguinte fornece uma descrição geral dos três partes que terá de implementar. 

1. Código de Media Services do Azure
  
  * Modelos de licença para PlayReady, Widevine e/ou do FairPlay. Os modelos permitem-lhe configurar direitos e permissões para cada um dos DRMs utilizados
  * Autorização de entrega de licença, especificando a lógica da verificação de autorização baseada em afirmações no JWT
  * As chaves de conteúdos, os protocolos de transmissão em fluxo e DRMs correspondentes aplicados, definição de encriptação de DRM

  > [!NOTE]
  > Pode encriptar cada recurso com vários tipos de encriptação (AES-128, PlayReady, Widevine, do FairPlay). Consulte [de transmissão em fluxo e tipos de encriptação de protocolos](#streaming-protocols-and-encryption-types), para ver o que faz sentido combinar.
  
  O seguinte artigo mostram os passos de encriptação de conteúdo com AES: [proteger com encriptação AES](protect-with-aes128.md)
 
2. Leitor com cliente AES ou DRM. Uma aplicação de leitor de vídeo com base num leitor de SDK (nativo ou baseadas no browser) tem de cumprir os seguintes requisitos:
  * Leitor de SDK suporta clientes DRM necessários
  * Leitor de SDK suporta os protocolos de transmissão em fluxo necessários: uniforme, DASH e/ou HLS
  * Leitor de SDK tem de ser capaz de lidar com transmissão um token JWT no pedido de aquisição de licença
  
    Pode criar um leitor, utilizando o [API do Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/). Utilize o [API do Azure Media Player ProtectionInfo](http://amp.azure.net/libs/amp/latest/docs/) para especificar que tecnologia DRM a utilizar em diferentes plataformas DRM.

    Para o teste AES ou CENC (Widevine + PlayReady) encriptados conteúdo, pode utilizar [Azure Media Player](https://ampdemo.azureedge.net/azuremediaplayer.html). Certifique-se, clique em "Opções avançadas" e consulte AES e forneça o token.

    Se pretender testar conteúdo do FairPlay encriptado, utilize [player este teste](http://aka.ms/amtest). O leitor suporta Widevine, PlayReady, e do FairPlay DRMs, bem como AES-128 limpar a encriptação de chaves. Tem de escolher o browser direito para testar diferentes DRMs: Opera/Chrome/Firefox para Widevine, MS Edge/IE11 para PlayReady, Safari no maOS do FairPlay.

3. Proteja o Token serviço (STS), que emite o Token Web JSON (JWT) como token de acesso para o acesso aos recursos de back-end. Pode utilizar os serviços de entrega de licença do AMS como o recurso de back-end. Um STS tem de definir o seguinte:

  * Emissor e público-alvo (ou âmbito)
  * Afirmações, que são dependentes de requisitos de negócio em proteção de conteúdos
  * Verificação de simétrica ou assimétrica para verificação de assinatura
  * Suporte de rollover de chave (se necessário)

    Pode utilizar [esta ferramenta STS](https://openidconnectweb.azurewebsites.net/DRMTool/Jwt) para teste STS, que suporta todos os 3 tipos de chave de verificação: simétrica, assimétrico, ou o AAD com o rollover da chave. 

> [!NOTE]
> É altamente recomendado focar-se e testar totalmente cada parte (descrito acima) antes de mover para a parte seguinte. Para testar o seu sistema de "proteção de conteúdos", utilize as ferramentas especificadas na lista acima.  

## <a name="streaming-protocols-and-encryption-types"></a>Protocolos de transmissão em fluxo e tipos de encriptação

Pode utilizar os Media Services para distribuir os seus conteúdos encriptados dinamicamente com o desmarcar chave AES ou encriptação DRM com PlayReady, Widevine ou do FairPlay. Atualmente, pode encriptar os formatos HTTP Live Streaming (HLS), MPEG DASH e transmissão em fluxo uniforme. Cada protocolo suporta os seguintes métodos de encriptação:

|Protocolo|Formato de contentor|Esquema de encriptação|
|---|---|---|---|
|MPEG-DASH|Todos|AES|
||CSF(fmp4) |CENC (Widevine + PlayReady) |
||CMAF(fmp4)|CENC (Widevine + PlayReady)|
|HLS|Todos|AES|
||MPG2 TS |CBCS (do Fairplay) |
||MPG2 TS |CENC (PlayReady) |
||CMAF(fmp4) |CENC (PlayReady) |
|Transmissão em Fluxo Uniforme|fMP4|AES|
||fMP4 | CENC (PlayReady) |

## <a name="dynamic-encryption"></a>Encriptação dinâmica

Os Media Services v3, uma chave de conteúdo está associada a StreamingLocator (consulte [neste exemplo](protect-with-aes128.md)). Se utilizar o serviço de entrega de chave de Media Services, deve auto gerar a chave de conteúdo. Deve gerar a chave de conteúdo manualmente se estiver a utilizar a entrega de chave próprio serviço ou se precisar de lidar com um cenário de elevada disponibilidade, onde tem de ter a mesma chave de conteúdo em dois centros de dados.

Quando um fluxo é solicitado por um leitor, os Media Services utiliza a chave especificada dinamicamente encriptar o seu conteúdo utilizando a chave não encriptada AES ou encriptação DRM. Para desencriptar o fluxo, o leitor de pedidos a chave do serviço de entrega de chave de Media Services ou o serviço de entrega de chave especificado. Para decidir se pretende ou não o utilizador está autorizado para obter a chave, o serviço avalia as políticas de autorização que especificou para a chave.

## <a name="aes-128-clear-key-vs-drm"></a>Vs de limpar chaves AES-128. DRM

Os clientes wonder frequentemente se deve utilizar encriptação AES ou um sistema DRM. A principal diferença entre os dois sistemas é que, com a encriptação AES, a chave de conteúdo é transmitida para o cliente num formato não encriptado ("em Limpar"). Como resultado, a chave utilizada para encriptar o conteúdo pode ser visualizada num rastreio de rede do cliente em texto simples. Encriptação de limpar chave AES-128 é adequada em casos de utilização em que o Visualizador é uma entidade fidedigna (por exemplo, encriptação empresarias vídeos distribuídos dentro de uma empresa para ser visualizada pelos funcionários).

PlayReady, Widevine e FairPlay que todas fornecem um nível mais elevado de encriptação em comparação comparada AES-128 limpar a encriptação de chaves. A chave de conteúdo é transmitida num formato encriptado. Além disso, desencriptação é processada num ambiente seguro ao nível do sistema operativo, onde é mais difícil para um utilizador mal intencionado a ataques. DRM recomenda-se em casos de utilização em que o Visualizador pode não ser uma entidade fidedigna e requerem o nível mais elevado de segurança.

## <a name="storage-side-encryption"></a>Encriptação do lado do armazenamento

Para proteger os seus recursos inativos, os ativos devem ser encriptados pela encriptação do lado do armazenamento. A tabela seguinte mostra como a encriptação do lado do armazenamento funciona em v3 de Media Services:

|Opção de encriptação|Descrição|Serviços de Multimédia v3|
|---|---|---|---|
|Encriptação de armazenamento de serviços de multimédia| AES 256 encriptação, chave gerida pelos Media Services|Não suportado<sup>(1)</sup>|
|[Encriptação do serviço de armazenamento para dados Inativos](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Chave de encriptação do lado do servidor oferecidas pelo armazenamento do Azure, gerida pelo Azure ou pelo cliente|Suportadas|
|[Encriptação do lado do cliente do Storage](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Oferecidas pelo armazenamento do Azure, chave gerido pelo cliente no Cofre de chaves de encriptação do lado do cliente|Não suportado|

<sup>1</sup> v3 de Media Services, a encriptação de armazenamento (encriptação AES 256) só é suportada para em efeitos de compatibilidade quando os elementos criados com os Media Services v2. Significado v3 funciona com o armazenamento existente encriptados ativos, mas não permitirá a criação de novos.

## <a name="licenses-and-keys-delivery-service"></a>Serviço de entrega de licenças e chaves

Os Media Services fornecem um serviço de entrega de chave para entrega de licenças DRM (PlayReady Widevine, do FairPlay) e chaves AES para clientes autorizados. Pode utilizar a API REST ou uma biblioteca de cliente de Media Services para configurar políticas de autenticação e autorização para as suas licenças e chaves.

## <a name="control-content-access"></a>Conteúdo de controlo de acesso

Pode controlar quem tem acesso ao conteúdo ao configurar a política de chave de conteúdo. Os Media Services suportam várias formas de autenticar utilizadores que efetuam pedidos de chave. Tem de configurar a política de chave de conteúdo. A chave pode ser entregue ao cliente, o cliente (leitor) têm de cumprir a política. A política de chave de conteúdo pode ter **abrir** ou **token** restrição. 

Com uma token restrito conteúda chave política, a chave de conteúdo é enviada apenas a um cliente que apresenta um válido JSON Web tokens (JWT) ou um token web simples (SWT) no pedido de chave/licença. Este token tem de ser emitido por um serviço de token de segurança (STS). Pode utilizar o Azure Active Directory como um STS ou implementar um STS personalizado. O STS tem de ser configurado para criar o token assinado com especificado chave e emitir afirmações que especificou na configuração de restrição de token. O serviço de entrega de chave de Media Services devolve a chave de pedido/licença para o cliente se o token é válido e as afirmações no token correspondem às configurado para a chave/licença.

Quando configura a política de token restrito, tem de especificar a chave de verificação principal, o emissor e o parâmetros público-alvo. A chave de verificação principal contém a chave que o token foi assinado com. O emissor é o serviço de token seguro que emite o token. O público-alvo, por vezes denominado âmbito, descreve o objetivo do token ou o recurso autoriza o token de acesso a. O serviço de entrega de chave de Media Services valida que estes valores no token correspondem aos valores no modelo.

## <a name="streaming-urls"></a>URLs de transmissão em fluxo

Se o seu elemento foi encriptado com mais do que um DRM, utilize uma tag de encriptação o URL de transmissão em fluxo: (formato = 'm3u8-aapl' encriptação = 'xxx').

Aplicam as seguintes considerações:

* Tipo de encriptação não tem de ser especificado no URL, se apenas uma encriptação foi aplicada ao elemento.
* Tipo de encriptação é sensível a maiúsculas e minúsculas.
* Podem ser especificados os seguintes tipos de encriptação:
  * **cenc**: para PlayReady ou Widevine (encriptação comum)
  * **cbcs-aapl**: para FairPlay (encriptação AES CBC)
  * **CBC**: encriptação de envelope AES para

## <a name="troubleshooting-tips"></a>Sugestões de resolução de problemas

Utilize as seguintes informações de resolução de problemas para obter ajuda com problemas de implementação.

* O emissor URL tem de terminar com "/". O público-alvo tem de ser o ID de cliente de aplicação do leitor. Além disso, adicione "/" no final do URL do emissor.

  ```
  <add key="ida:audience" value="[Application Client ID GUID]" />
  <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" />
  ```

* Adicionar permissões para a aplicação no Azure AD no **configurar** separador da aplicação. As permissões são necessárias para cada aplicação, as versões de locais e implementadas.
* Utilize o emissor correto quando configurar a proteção de CENC dinâmica.

  ```
  <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>
  ```

  Não utilizar o seguinte:

  ```
  <add key="ida:issuer" value="https://username.onmicrosoft.com/" />
  ```

  O GUID é o ID do inquilino do Azure AD. Pode encontrar o GUID de **pontos finais** menu de pop-up no portal do Azure.

* Associação ao grupo de conceder privilégios de afirmações. Certifique-se de que é o seguinte no ficheiro de manifesto da aplicação do Azure AD: 

    "groupMembershipClaims": "All" (o valor predefinido é nulo)

## <a name="next-steps"></a>Passos Seguintes

[Proteger com a encriptação AES](protect-with-aes128.md)
