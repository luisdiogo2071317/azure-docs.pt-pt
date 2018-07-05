---
title: Utilizar os leitores existentes para reproduzir o conteúdo - Azure | Documentos da Microsoft
description: Este tópico lista os leitores existentes que pode usar para reprodução de seu conteúdo.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.assetid: 7e9fcf89-0fb6-4fa4-96cb-666320684d69
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2018
ms.author: juliako
ms.openlocfilehash: 3fe82b98163182c73a144b72da371e8aa195e8cf
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37435862"
---
# <a name="playing-your-content-with-existing-players"></a>Reproduzir o conteúdo com leitores existentes
Serviços de multimédia do Azure suporta muitos formatos de transmissão em fluxo populares, tais como a transmissão em fluxo uniforme, HTTP Live Streaming e MPEG-Dash. Este tópico de pontos de para os leitores existentes que pode usar para testar transmissões em fluxo.

### <a name="the-azure-portal-media-services-content-player"></a>O Azure portal dos serviços de multimédia leitor de conteúdos
O **Azure** portal fornece um leitor de conteúdos que pode utilizar para testar o vídeo.

Clique no vídeo pretendido (Certifique-se de que era [publicados](media-services-portal-publish.md)) e clique no **reproduzir** na parte inferior do portal.

São aplicáveis algumas considerações:

* O **LEITOR DE CONTEÚDOS DE MEDIA SERVICES** reproduz a partir do ponto final de transmissão em fluxo predefinido. Se pretender reproduzir a partir de um ponto final de transmissão em fluxo não predefinido, utilize outro leitor. Por exemplo, [leitor de multimédia do Azure](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

![AMSPlayer][AMSPlayer]

### <a name="azure-media-player"></a>Media Player do Azure
Uso [leitor de multimédia do Azure](http://amsplayer.azurewebsites.net/azuremediaplayer.html) para reproduzir o conteúdo (encriptado ou protegido) em qualquer um dos seguintes formatos:

* Transmissão em Fluxo Uniforme
* MPEG DASH
* HLS
* MP4 de transferência progressiva

### <a name="flash-player"></a>Flash Player
#### <a name="aes-encrypted-with-token"></a>Encriptada do AES com Token
[http://aestoken.azurewebsites.net](http://aestoken.azurewebsites.net)

### <a name="silverlight-players"></a>Jogadores do Silverlight

#### <a name="playready-with-token"></a>PlayReady com Token
[http://sltoken.azurewebsites.net](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>Leitores de TRAÇO
[http://dashplayer.azurewebsites.net](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

### <a name="other"></a>Outros
Para testar os URLs de HLS também pode utilizar:

* **Safari** num dispositivo iOS ou
* **Leitor HLS 3ivx** no Windows.

## <a name="developing-video-players"></a>Desenvolvimento de leitores de vídeo
Para obter informações sobre como desenvolver seus próprios jogadores, consulte [desenvolver leitores de vídeo](media-services-develop-video-players.md)

## <a name="media-services-learning-paths"></a>Percursos de aprendizagem dos Media Services
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
