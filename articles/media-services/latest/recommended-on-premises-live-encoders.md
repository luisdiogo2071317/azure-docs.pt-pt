---
title: Saiba mais sobre codificadores de locais de transmissão em fluxo em direto recomendados pelos serviços de multimédia - Azure | Documentos da Microsoft
description: Saiba mais sobre codificadores de locais de transmissão em fluxo em direto recomendados pelos serviços de multimédia
services: media-services
keywords: codificação; codificadores; suporte de dados
author: johndeu
manager: johndeu
ms.author: johndeu
ms.date: 01/17/2019
ms.topic: article
ms.service: media-services
ms.openlocfilehash: c3e42ba9fe84ded8c60fc71f19de785945852116
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55656673"
---
# <a name="recommended-live-streaming-encoders"></a>Recomendado codificadores de transmissão em fluxo em direto

Nos Media Services, um [evento em direto](https://docs.microsoft.com/rest/api/media/liveevents) (canal) representa um pipeline de processamento de conteúdo de transmissão em direto. O evento Live recebe transmissões em direto de entrada em uma das seguintes formas:

* Um codificador em direto de no local envia uma velocidade de transmissão RTMP ou transmissão em fluxo uniforme (MP4 fragmentado) transmita em fluxo para o evento em direto que não está ativado para realizar live encoding com Media Services. As transmissões em fluxo passam por meio de eventos em direto sem qualquer processamento adicional. Esse método é chamado **pass-through**. Um codificador em direto pode enviar um fluxo de velocidade de transmissão única para um pass-through canal, mas esta configuração não é recomendada porque ele não permite que a velocidade de transmissão adaptável de transmissão em fluxo para o cliente.

  > [!NOTE]
  > Usando um método pass-through é a forma mais económica para transmissão em direto.

* Um codificador em direto no local envia um fluxo de velocidade de transmissão única para o evento em direto que está ativado para realizar live encoding com Media Services dos seguintes formatos: RTMP ou transmissão em fluxo uniforme (MP4 fragmentado). O evento em direto, em seguida, executa a codificação em tempo real do fluxo de velocidade de transmissão única de entrada para um fluxo de vídeo com várias (adaptável).

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

## <a name="configuring-on-premises-live-encoder-settings"></a>Configurar definições no local codificador em direto

Para obter informações sobre quais configurações são válidas para o seu tipo de evento em direto, consulte [comparação de tipos de evento em direto](live-event-types-comparison.md).

### <a name="playback-requirements"></a>Requisitos de reprodução

Tanto um fluxo de áudio e vídeo tem de estar presente na ordem de reproduzir conteúdo. Não é suportada a reprodução da transmissão em fluxo só de vídeo.

### <a name="configuration-tips"></a>Sugestões de configuração

- Sempre que possível, utilize uma ligação de internet conectada.
- Uma boa regra prática ao determinar os requisitos de largura de banda é duas vezes as velocidades de transmissão de transmissão em fluxo. Embora não seja um requisito obrigatório, ajudará a minimizar o impacto de congestionamento de rede.
- Quando a utilização de software com base codificadores, feche todos os programas desnecessários.
- Não altere a configuração do codificador assim que tiver iniciado a enviar. Ele tem os efeitos negativos no evento e pode fazer com que o evento ser instável. 
- Certifique-se para dar tempo bastante para configurar o seu evento. Para eventos de grande escala, recomendados para iniciar a configuração de uma hora antes de seu evento.

## <a name="how-to-become-an-on-premises-encoder-partner"></a>Como se tornar um parceiro de codificador no local

Como parceiro de codificador de serviços de multimédia do Azure no local, serviços de multimédia promove seu produto ao recomendar seu codificador para os clientes empresariais. Para se tornar um parceiro de codificador no local, tem de verificar a compatibilidade de seu codificador no local com os Media Services. Para fazê-lo, conclua as seguintes verificações:

### <a name="pass-through-live-event-verification"></a>Verificação de evento em direto de pass-through

1. Na sua conta de Media Services, certifique-se de que o **ponto final de transmissão em fluxo** está em execução. 
2. Criar e iniciar o **pass-through** evento em direto. <br/> Para obter mais informações, consulte [Estados de evento em direto e de faturação](live-event-states-billing.md).
3. Obtenha os URLs de ingestão e configurar o seu codificador no local para utilizar o URL para enviar a um velocidade de transmissão em fluxo em direto para os serviços de multimédia.
4. Obter o URL de pré-visualização e utilizá-lo para verificar que a entrada do codificador, na verdade, está a ser recebida.
5. Criar uma nova **Asset** objeto.
6. Criar uma **Live saída** e utilize o nome de recurso que criou.
7. Criar uma **localizador de transmissão em fluxo** com o incorporado **política de transmissão em fluxo** tipos.
8. Lista os caminhos a **localizador de transmissão em fluxo** para recuperar os URLs para utilizar.
9. Obter o nome do anfitrião para o **ponto final de transmissão em fluxo** que deseja transmitir em fluxo do.
10. Combine o URL do passo 8 com o nome de anfitrião no passo 9 para obter o URL completo.
11. Execute seu codificador em direto para cerca de 10 minutos.
12. Pare o evento em direto. 
13. Utilizar como um jogador [leitor de multimédia do Azure](https://ampdemo.azureedge.net/azuremediaplayer.html) para ver o elemento arquivado para garantir que essa reprodução tem sem falhas visível para todos os níveis de qualidade (ou, em alternativa ver e validar via a URL de pré-visualização durante a sessão em direto).
14. Registe o ID de recurso publicado o URL de transmissão em fluxo para este arquivo live e as definições e a versão utilizada a partir do seu codificador em direto.
15. Repor o estado do evento em direto depois de criar cada exemplo.
16. Repita os passos 5 a 15 para todas as configurações suportados por seu codificador (com e sem ad sinalização/legendas/diferentes velocidades de codificação).

### <a name="live-encoding-live-event-verification"></a>Verificação de evento em direto de codificação em direto

1. Na sua conta de Media Services, certifique-se de que o **ponto final de transmissão em fluxo** está em execução. 
2. Criar e iniciar o **codificação em direto** evento em direto. <br/> Para obter mais informações, consulte [Estados de evento em direto e de faturação](live-event-states-billing.md).
3. Obtenha os URLs de ingestão e configurar seu codificador para enviar um fluxo em direto com velocidade de transmissão única para serviços de multimédia.
4. Obter o URL de pré-visualização e utilizá-lo para verificar que a entrada do codificador, na verdade, está a ser recebida.
5. Criar uma nova **Asset** objeto.
6. Criar uma **Live saída** e utilize o nome de recurso que criou.
7. Criar uma **localizador de transmissão em fluxo** com o incorporado **política de transmissão em fluxo** tipos.
8. Lista os caminhos a **localizador de transmissão em fluxo** para recuperar os URLs para utilizar.
9. Obter o nome do anfitrião para o **ponto final de transmissão em fluxo** que deseja transmitir em fluxo do.
10. Combine o URL do passo 8 com o nome de anfitrião no passo 9 para obter o URL completo.
11. Execute seu codificador em direto para cerca de 10 minutos.
12. Pare o evento em direto.
13. Utilizar como um jogador [leitor de multimédia do Azure](https://ampdemo.azureedge.net/azuremediaplayer.html) para ver o elemento arquivado para garantir que essa reprodução tem sem falhas visível para todos os níveis de qualidade (ou, em alternativa ver e validar via a URL de pré-visualização durante a sessão em direto).
14. Registe o ID de recurso publicado o URL de transmissão em fluxo para este arquivo live e as definições e a versão utilizada a partir do seu codificador em direto.
15. Repor o estado do evento em direto depois de criar cada exemplo.
16. Repita os passos 5 a 15 para todas as configurações suportados por seu codificador (com e sem ad sinalização/legendas/diferentes velocidades de codificação).

### <a name="longevity-verification"></a>Verificação de longevidade

Mesmo os passos como na [verificação de evento em direto de pass-through](#pass-through-live-event-verification) , exceto para o passo 11. <br/>Em vez de 10 minutos, execute seu codificador em direto para uma semana ou mais tempo. Utilizar como um jogador [leitor de multimédia do Azure](https://ampdemo.azureedge.net/azuremediaplayer.html) para ver a transmissão em direto da hora a hora (ou elemento arquivado) para garantir que essa reprodução tem sem falhas visível.

### <a name="email-your-recorded-settings"></a>As definições gravadas de e-mail

Por último, as definições gravadas de e-mail e live parâmetros de arquivo para serviços de multimédia do Azure em amsstreaming@microsoft.com como uma notificação de que todas as verificações de verificação automática tenham passado. Além disso, incluem as informações de contacto para qualquer ups siga. Pode contactar a equipa de serviços de multimédia do Azure com perguntas sobre este processo.

## <a name="next-steps"></a>Passos Seguintes

[Transmissão em direto com serviços de multimédia v3](live-streaming-overview.md)
