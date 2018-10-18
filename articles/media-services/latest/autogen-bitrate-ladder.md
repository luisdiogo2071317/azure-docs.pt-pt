---
title: Utilize o codificador Standard nos serviços de multimédia do Azure para codificar vídeos usando uma escada gerado automaticamente a velocidade de transmissão | Documentos da Microsoft
description: Este tópico mostra como utilizar o codificador Standard nos serviços de multimédia para codificar uma entrada de vídeo com uma escala de bits de velocidade de transmissão gerado automaticamente, com base na resolução de entrada e de velocidade de transmissão. A resolução de entrada e a velocidade de transmissão nunca serão excedidas. Por exemplo, se a entrada é 720p em 3 Mbps, a saída irá permanecer 720p na melhor e será iniciado às tarifas inferiores a 3 Mbps.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: ec1b4b88e5b9639c3ee9debbd8ac7d48544344dc
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49378963"
---
#  <a name="encode-with-an-auto-generated-bitrate-ladder"></a>Codificar com uma escada gerado automaticamente a velocidade de transmissão

## <a name="overview"></a>Descrição geral

Este artigo explica como utilizar o codificador Standard nos serviços de multimédia para codificar um vídeo de entrada para uma escada gerado automaticamente a velocidade de transmissão (pares de resolução de velocidade de transmissão) com base na resolução de entrada e de velocidade de transmissão. Este codificador interno definição ou a configuração predefinida, nunca irá exceder a resolução de entrada e a velocidade de transmissão. Por exemplo, se a entrada for 720p a 3 Mbps, saída permanece 720p na melhor e será iniciado às tarifas inferiores a 3 Mbps.

### <a name="encoding-for-streaming"></a>Codificação de transmissão em fluxo

Quando utiliza a **AdaptiveStreaming** predefinidas no **transformar**, receberá um resultado que é adequado para entrega via protocolos como HLS e DASH de transmissão em fluxo. Quando utilizar esta configuração predefinida, o serviço de forma inteligente determina quantas camadas de vídeo para gerar e em quais velocidade de transmissão e a resolução. O conteúdo de saída contém os ficheiros MP4 onde codificado AAC áudio e vídeo de codificada em H.264 não é intercalado.

Para ver um exemplo de como esta configuração predefinida é utilizada, consulte [Stream um ficheiro](stream-files-dotnet-quickstart.md).

## <a name="output"></a>Saída

Esta secção mostra três exemplos de camadas de vídeo de saída produzidos pelo codificador de serviços de multimédia, como resultado de codificação com o **AdaptiveStreaming** predefinidas. Em todos os casos, a saída contém ficheiros MP4 só de áudio com estéreo áudio codificado com 128 kbps.

### <a name="example-1"></a>Exemplo 1
A origem com altura "1080" e a framerate "29.970" produz 6 camadas de vídeo:

|Camada|Altura|Largura|Velocidade de transmissão (kbps)|
|---|---|---|---|
|1|1080|1920|6780|
|2|720|1280|3520|
|3|540|960|2210|
|4|360|640|1150|
|5|270|480|720|
|6|180|320|380|

### <a name="example-2"></a>Exemplo 2
A origem com altura "720" e a framerate "23.970" produz 5 camadas de vídeo:

|Camada|Altura|Largura|Velocidade de transmissão (kbps)|
|---|---|---|---|
|1|720|1280|2940|
|2|540|960|1850|
|3|360|640|960|
|4|270|480|600|
|5|180|320|320|

### <a name="example-3"></a>Exemplo 3
A origem com altura "360" e a framerate "29.970" produz 3 camadas de vídeo:

|Camada|Altura|Largura|Velocidade de transmissão (kbps)|
|---|---|---|---|
|1|360|640|700|
|2|270|480|440|
|3|180|320|230|

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Transmissão de um ficheiro](stream-files-dotnet-quickstart.md)
