---
title: Saiba mais sobre codificadores de locais de transmissão em fluxo em direto recomendados pelos serviços de multimédia - Azure | Documentos da Microsoft
description: Saiba mais sobre codificadores de locais de transmissão em fluxo em direto recomendados pelos serviços de multimédia
services: media-services
keywords: codificação; codificadores; suporte de dados
author: johndeu
manager: johndeu
ms.author: johndeu
ms.date: 12/14/2018
ms.topic: article
ms.service: media-services
ms.openlocfilehash: 4d25e4cd94fec35f31594544b619aa054a35d58d
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/15/2019
ms.locfileid: "54302345"
---
# <a name="recommended-live-streaming-encoders"></a>Recomendado codificadores de transmissão em fluxo em direto

Nos Media Services, um [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) (canal) representa um pipeline de processamento de conteúdo de transmissão em direto. Um LiveEvent recebe transmissões em direto de entrada em uma das seguintes formas:

* Um codificador em direto de no local envia uma velocidade de transmissão RTMP ou transmissão em fluxo uniforme (MP4 fragmentado) transmita em fluxo para o LiveEvent que não está ativado para realizar live encoding com Media Services. As transmissões em fluxo passam LiveEvents sem qualquer processamento adicional. Esse método é chamado **pass-through**. Um codificador em direto pode enviar um fluxo de velocidade de transmissão única para um pass-through canal, mas esta configuração não é recomendada porque ele não permite que a velocidade de transmissão adaptável de transmissão em fluxo para o cliente.

  > [!NOTE]
  > Usando um método pass-through é a forma mais económica para transmissão em direto.

* Um codificador em direto no local envia um fluxo de velocidade de transmissão única para o LiveEvent ativado para realizar live encoding com Media Services dos seguintes formatos: RTMP ou transmissão em fluxo uniforme (MP4 fragmentado). O LiveEvent, em seguida, executa a codificação em tempo real do fluxo de velocidade de transmissão única de entrada para um fluxo de vídeo com várias (adaptável).

Para obter informações detalhadas sobre o live encoding com Media Services, consulte [Live transmissão em fluxo com serviços de multimédia v3](live-streaming-overview.md).

## <a name="live-encoders-that-output-rtmp"></a>Codificadores em direto que transmitem RTMP

Serviços de multimédia recomenda o uso de um dos seguintes codificadores em direto que tenham RTMP como saída. Os esquemas de URL suportados são `rtmp://` ou `rtmps://`.

> [!NOTE]
 > Quando a transmissão em fluxo através de RTMP, verifique as definições de firewall e/ou proxy para confirmar que as portas TCP de saída 1935 e 1936 estão abertas.<br/>
 Quando a transmissão em fluxo através de RTMPS, verifique as definições de firewall e/ou proxy para confirmar que as portas TCP de saída 2935 e 2936 estão abertas.

- Suporte de dados de Adobe Flash Live Encoder 3.2
- Haivision KB
- Haivision Makito X HEVC
- OBS Studio
- Comutador Studio (iOS)
- Telestream Wirecast 8.1 +
- Telestream Wirecast S
- Setor de Teradek 756
- TriCaster 8000
- Tricaster Mini HD-4
- VMIX
- xStream

## <a name="live-encoders-that-output-fragmented-mp4"></a>Codificadores em direto que MP4 fragmentado

Serviços de multimédia recomenda o uso de um dos seguintes codificadores em direto com velocidade de transmissão transmissão em fluxo uniforme (MP4 fragmentado) como saída. Os esquemas de URL suportados são `http://` ou `https://`.

- Ateme TITAN Live
- Cisco Digital Media Encoder 2200
- Elemental em direto
- Envivio 4Caster C4 fins III
- Imagine Communications Selenio MCP3
- Suporte de dados do Excel Hero em direto e Hero 4K (UHD/HEVC)

## <a name="how-to-become-an-on-premises-encoder-partner"></a>Como se tornar um parceiro de codificador no local

Como parceiro de codificador de serviços de multimédia do Azure no local, serviços de multimédia promove seu produto ao recomendar seu codificador para os clientes empresariais. Para se tornar um parceiro de codificador no local, tem de verificar a compatibilidade de seu codificador no local com os Media Services. Para fazê-lo, conclua as seguintes verificações:

### <a name="pass-through-liveevent-verification"></a>Verificação de LiveEvent pass-through

1. Criar ou visite a sua conta de Media Services do Azure.
2. Crie e inicie um **pass-through** LiveEvent.
3. Configure seu codificador para enviar por push um velocidade de transmissão em fluxo em direto.
4. Crie um evento em direto publicado.
5. Execute seu codificador em direto para cerca de 10 minutos.
6. Pare o evento em direto.
7. Criar, iniciar um ponto de final de transmissão em fluxo, utilize um leitor, tal como [leitor de multimédia do Azure](https://ampdemo.azureedge.net/azuremediaplayer.html) assista o elemento arquivado para garantir que essa reprodução tem sem falhas visível para todos os níveis de qualidade (ou em alternativa ver e validar via a URL de pré-visualização durante a sessão em direto antes do passo 6).
8. Registe o ID de recurso publicado o URL de transmissão em fluxo para este arquivo live e as definições e a versão utilizada a partir do seu codificador em direto.
9. Repor o estado de LiveEvent depois de criar cada exemplo.
10. Repita os passos 3 a 9 para todas as configurações suportados por seu codificador (com e sem ad sinalização/legendas/diferentes velocidades de codificação).

### <a name="live-encoding-liveevent-verification"></a>Encoding LiveEvent verificação em tempo real

1. Criar ou visite a sua conta de Media Services do Azure.
2. Crie e inicie um **codificação em direto** LiveEvent.
3. Configure seu codificador para enviar um fluxo em direto com velocidade de transmissão única.
4. Crie um evento em direto publicado.
5. Execute seu codificador em direto para cerca de 10 minutos.
6. Pare o evento em direto.
7. Criar, iniciar um ponto de final de transmissão em fluxo, utilize um leitor, tal como [leitor de multimédia do Azure](https://ampdemo.azureedge.net/azuremediaplayer.html) assista o elemento arquivado para garantir que essa reprodução tem sem falhas visível para todos os níveis de qualidade (ou em alternativa ver e validar via a URL de pré-visualização durante a sessão em direto antes do passo 6).
8. Registe o ID de recurso publicado o URL de transmissão em fluxo para este arquivo live e as definições e a versão utilizada a partir do seu codificador em direto.
9. Repor o estado de LiveEvent depois de criar cada exemplo.
10. Repita os passos 3 a 9 para todas as configurações suportados por seu codificador (com e sem ad sinalização/legendas/várias velocidades de codificação).

### <a name="longevity-verification"></a>Verificação de longevidade

1. Criar ou visite a sua conta de Media Services do Azure.
2. Crie e inicie um **pass-through** canal.
3. Configure seu codificador para enviar por push um velocidade de transmissão em fluxo em direto.
4. Crie um evento em direto publicado.
5. Execute seu codificador em direto para uma semana ou mais tempo.
6. Utilizar como um jogador [leitor de multimédia do Azure](https://ampdemo.azureedge.net/azuremediaplayer.html) para ver a transmissão em direto da hora a hora (ou elemento arquivado) para garantir que essa reprodução tem sem falhas visível.
7. Pare o evento em direto.
8. Registe o ID de recurso publicado o URL de transmissão em fluxo para este arquivo live e as definições e a versão utilizada a partir do seu codificador em direto.

Por último, as definições gravadas de e-mail e live parâmetros de arquivo para serviços de multimédia do Azure em amsstreaming@microsoft.com como uma notificação de que todas as verificações de verificação automática tenham passado. Além disso, incluem as informações de contacto para qualquer ups siga. Pode contactar a equipa de serviços de multimédia do Azure com perguntas sobre este processo.

## <a name="next-steps"></a>Passos Seguintes

[Transmissão em direto com serviços de multimédia v3](live-streaming-overview.md)
