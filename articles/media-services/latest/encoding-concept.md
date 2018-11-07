---
title: Codificação na cloud com serviços de multimédia do Azure | Documentos da Microsoft
description: Este tópico descreve o processo de codificação ao utilizar os serviços de multimédia do Azure
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 11/05/2018
ms.author: juliako
ms.openlocfilehash: 0df13e3364cebe7cb5804b840889bca971b36be2
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51235059"
---
# <a name="encoding-with-media-services"></a>Encoding com Media Services

Serviços de multimédia do Azure permite-lhe a codificar os seus ficheiros multimédia digital de alta qualidade em formatos que podem ser jogados numa grande variedade de navegadores e dispositivos. Por exemplo, pode querer transmitir o conteúdo nos formatos HLS ou MPEG DASH da Apple. Este tópico fornece orientações sobre como codificar o conteúdo com serviços de multimédia v3.

Para codificar com serviços de multimédia v3, terá de criar uma transformação e uma tarefa. Uma transformação define a receita para as suas definições e saídas de codificação e a tarefa é uma instância da receita. Para obter mais informações, consulte [transforma e tarefas](transform-concept.md)

Quando o encoding com Media Services, vai utilizar configurações predefinidas para informar ao codificador, como os ficheiros de suporte de dados de entrada devem ser processados. Por exemplo, pode especificar a resolução de vídeo e/ou o número de canais de áudio que pretende no conteúdo codificado. 

Pode começar a utilizar rapidamente com um dos pré-visualizando incorporada recomendado com base nas práticas recomendadas do setor ou pode optar por criar um personalizado predefinido para seus requisitos específicos de cenário ou dispositivo de destino. Para obter mais informações, consulte [codificar com uma transformação personalizada](customize-encoder-presets-how-to.md). 

## <a name="built-in-presets"></a>Configurações predefinidas incorporadas

Atualmente, os serviços de multimédia suporta as seguintes predefinições de codificação incorporadas:  

|**Nome da configuração predefinida**|**Cenário**|**Detalhes**|
|---|---|---|
|**BuiltInStandardEncoderPreset**|Transmissão em Fluxo|Usado para definir um incorporado na configuração predefinido para a codificação de vídeo de entrada com o codificador Standard. <br/>Atualmente são suportadas as seguintes predefinições:<br/>**EncoderNamedPreset.AdaptiveStreaming** (recomendado). Para obter mais informações, consulte [criação automática de uma escala de bits](autogen-bitrate-ladder.md).<br/>**EncoderNamedPreset.AACGoodQualityAudio** -produz um ficheiro MP4 único que contém apenas estéreo áudio codificado em 192 kbps.<br/>**EncoderNamedPreset.H264MultipleBitrate1080p** -produz um conjunto de ficheiros MP4 de alinhado GOP de 8, desde 6000 kbps até 400 kbps e áudio AAC estéreo. Resolução começa em 1080p e vai até 360p.<br/>**EncoderNamedPreset.H264MultipleBitrate720p** -produz um conjunto de 6 ficheiros MP4 GOP alinhados, desde 3400 kbps até 400 kbps e áudio AAC estéreo. Resolução é iniciada à 720p e vai até 360p.<br/>**EncoderNamedPreset.H264MultipleBitrateSD** -produz um conjunto de ficheiros MP4 de alinhado GOP de 5, desde 1600 kbps até 400 kbps e áudio AAC estéreo. Resolução é iniciada à 480p e vai até 360p.<br/><br/>Para obter mais informações, consulte [carregar, codificar e ficheiros de transmissão em fluxo](stream-files-tutorial-with-api.md).|
|**StandardEncoderPreset**|Transmissão em Fluxo|Descreve as definições para serem utilizadas quando a codificação de vídeo de entrada com o codificador Standard. <br/>Utilize esta configuração predefinida ao personalizar as configurações predefinidas de transformação. Para obter mais informações, consulte [como personalizar as configurações predefinidas de transformação](customize-encoder-presets-how-to.md).|

## <a name="custom-presets"></a>Configurações predefinidas personalizadas

Serviços de multimédia suporta totalmente a personalizar todos os valores nas predefinições para atender às suas necessidades específicas de codificação e requisitos. Utilizar o **StandardEncoderPreset** predefinidos ao personalizar as configurações predefinidas de transformação. Uma detalhada para explicações e de exemplo, veja [como personalizar as configurações predefinidas de codificador](customize-encoder-presets-how-to.md).

## <a name="scaling-encoding-in-v3"></a>Dimensionar codificação da v3

Atualmente, os clientes têm a utilizar o portal do Azure ou serviços de multimédia v2 APIs para definir o RUs (conforme descrito em [Dimensionar processamento de multimédia](../previous/media-services-scale-media-processing-overview.md). 

## <a name="next-steps"></a>Passos Seguintes

### <a name="tutorials"></a>Tutoriais

O tutorial seguinte mostra como codificar o conteúdo com os serviços de multimédia:

* [Carregar, codificar e transmitir em fluxo através dos Media Services](stream-files-tutorial-with-api.md)

### <a name="code-samples"></a>Exemplos de código

Os seguintes exemplos de código contêm código que mostra como codificar com serviços de multimédia:

* [.NET Core](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore)
* [CLI do Azure](https://github.com/Azure/azure-docs-cli-python-samples/tree/master/media-services)

### <a name="sdks"></a>SDKs

Pode utilizar qualquer um dos SDKs de seguintes v3 de serviços de multimédia suportados para codificar o conteúdo.

* [CLI do Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
* [REST](https://docs.microsoft.com/rest/api/media/transforms)
* [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/mediaservices/management?view=azure-dotnet)
* [Java](https://docs.microsoft.com/java/api/overview/azure/mediaservices)
* [Python](https://docs.microsoft.com/python/api/overview/azure/media-services?view=azure-python)

