---
title: Descrição geral de empacotamento dinâmico dos serviços de multimédia do Azure | Documentos da Microsoft
description: O tópico apresenta uma visão geral do empacotamento dinâmico, nos serviços de multimédia.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/19/2019
ms.author: juliako
ms.openlocfilehash: d1d07402bca5f01cf63d0b039c085e46bb0f0d62
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447927"
---
# <a name="dynamic-packaging"></a>Empacotamento dinâmico

Serviços de multimédia do Microsoft Azure podem ser utilizados para disponibilizar formatos de muitos arquivos de origem do suporte de dados, suportes de dados em fluxo em formatos, e a proteção de conteúdo formata a uma variedade de tecnologias de cliente (por exemplo, iOS e XBOX). Estes clientes compreender protocolos diferentes, por exemplo iOS requer que um formato de HTTP Live Streaming (HLS) e o Xbox necessitam de transmissão em fluxo uniforme. Se tiver um conjunto de velocidade de transmissão adaptável (múltipla) MP4 ou um conjunto de arquivos transmissão em fluxo uniforme de velocidade de transmissão adaptável que deve servir para clientes que compreender HLS, MPEG DASH ou transmissão em fluxo uniforme de ficheiros (ISO Base Media 14496-12), deve tirar partido de suportes de dados Serviços de empacotamento dinâmico.

Com o empacotamento dinâmico, tudo o que precisa é criar um elemento que contenha um conjunto de ficheiros MP4 de velocidade de transmissão adaptável. Em seguida, com base no formato especificado no pedido de manifesto ou fragmento, a transmissão em fluxo a pedido em servidor irá garantir que recebe o fluxo no protocolo que escolheu. Como resultado, só tem de armazenar e pagar pelos ficheiros num único formato de armazenamento e os Media Services irão compilar e disponibilizar a resposta adequada com base nos pedidos de um cliente.

O diagrama seguinte mostra a codificação tradicional e o fluxo de trabalho de empacotamento estáticos.

![Codificação estático](./media/dynamic-packaging-overview/media-services-static-packaging.png)

O diagrama seguinte mostra o fluxo de trabalho de empacotamento dinâmico.

![Codificação dinâmico](./media/dynamic-packaging-overview/media-services-dynamic-packaging.png)

## <a name="dynamic-packaging-workflow"></a>Fluxo de trabalho de empacotamento dinâmico

1. Carregar um ficheiro de entrada (chamado de um ficheiro de mezanino). Por exemplo, H.264, MP4 ou WMV (para obter a lista dos formatos suportados, consulte [formatos suportados pelo Media Encoder Standard](media-encoder-standard-formats.md).
2. Codificar o ficheiro de mezanino para os conjuntos H.264 MP4 de velocidade de transmissão adaptável.
3. Publique o elemento que contém a conjunto MP4 de velocidade de transmissão adaptável.
4. Crie os URLs de transmissão em fluxo para aceder e transmitir o seu conteúdo.

## <a name="audio-codecs-supported-by-dynamic-packaging"></a>Codecs de áudio suportado pelo empacotamento dinâmico

Empacotamento dinâmico suporta ficheiros MP4 que contêm áudio codificado com [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, HE-AAC v1, v2 HE-AAC), [Dolby Digital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus) (3 de AC avançada ou E AC3), ou [DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29) (DTS Express, LBR de DTS, DTS HD, HD DTS sem perdas).

> [!NOTE]
> Empacotamento dinâmico não suporta ficheiros que contêm [Dolby Digital](https://en.wikipedia.org/wiki/Dolby_Digital) áudio (AC3) (é um codec herdado).

## <a name="next-steps"></a>Passos Seguintes

[Carregar, codificar, transmitir vídeos em fluxo](stream-files-tutorial-with-api.md)
