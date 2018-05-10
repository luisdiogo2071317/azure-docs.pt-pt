---
title: Utilize o codificador padrão Media Services do Azure para vídeos com uma velocidade de transmissão gerado automaticamente ladder de codificar | Microsoft Docs
description: Este tópico mostra como utilizar o codificador de padrão nos Media Services para codificar uma entrada vídeo com ladder uma velocidade de transmissão gerado automaticamente, com base na entrada de resolução e velocidade de transmissão. A entrada de resolução e velocidade de transmissão nunca irão ser excedidos. Por exemplo, se a entrada é 720p em 3 Mbps, será de saída permanecem 720p, melhor e iniciará às taxas de inferiores a 3 Mbps.
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
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 5d13c711d7d71df7469e6408ce78cf0df611632b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
#  <a name="encode-with-an-auto-generated-bitrate-ladder"></a>Codificar com uma velocidade de transmissão gerado automaticamente ladder

## <a name="overview"></a>Descrição geral

Este artigo explica como utilizar o codificador de padrão nos Media Services para codificar um vídeo de entrada para um ladder gerada automaticamente de velocidade de transmissão (pares de resolução de velocidade de transmissão) com base na entrada de resolução e velocidade de transmissão. Nunca irá exceder a este codificador incorporada definição ou predefinição, a entrada de resolução e velocidade de transmissão. Por exemplo, se a entrada é 720p 3 Mbps, saída permanece 720p, melhor e iniciará às taxas de inferiores a 3 Mbps.

### <a name="encoding-for-streaming"></a>Codificação de transmissão em fluxo

Como o nome sugere, se utilizar o **AdaptiveStreaming** da configuração predefinida durante a criação de uma transformação de codificação, obterá uma saída que é adequada para a entrega através de protocolos como HLS, TRAÇO, CMAF etc de transmissão em fluxo. Quando utilizar esta **AdaptiveStreaming** da configuração predefinida, o codificador inteligentemente determina quantas camadas de vídeos para gerar e em que resolução e velocidade de transmissão. A saída do recurso contém ficheiros MP4 onde codificado AAC áudio e vídeo de 264 codificado não é interleaved.

Para ver um exemplo de como esta predefinição é utilizada, consulte [transmitir um ficheiro](stream-files-dotnet-quickstart.md).

## <a name="output"></a>Saída

Esta secção mostra exemplos de três camadas de vídeo a saída produzidos pelo codificador como resultado encoding com Media Services do **AdaptiveStreaming** predefinidas. Em todos os casos, a saída irá conter apenas de áudio ficheiros MP4 com áudio stereo codificado em 128 kbps.

### <a name="example-1"></a>Exemplo 1
A origem com altura "1080" e framerate "29.970" produz 6 camadas vídeos:

|Camada|Altura|Largura|Bitrate(Kbps)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>Exemplo 2
A origem com altura "720" e framerate "23.970" produz 5 camadas vídeos:

|Camada|Altura|Largura|Bitrate(Kbps)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>Exemplo 3
A origem com altura "360" e framerate "29.970" produz de 3 camadas vídeos:

|Camada|Altura|Largura|Bitrate(Kbps)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Um ficheiro de fluxo](stream-files-dotnet-quickstart.md)
