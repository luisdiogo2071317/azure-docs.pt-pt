---
title: Codificação na nuvem com Media Services do Azure | Microsoft Docs
description: Este tópico descreve o processo de codificação quando utilizar os Media Services do Azure
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/21/2018
ms.author: juliako
ms.openlocfilehash: e1c7536c59b110ae3dd753ff5f4b01195f8dadca
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655627"
---
# <a name="encoding-with-azure-media-services"></a>Encoding com Media Services do Azure

Media Services do Azure permite-lhe codificar seus ficheiros de suporte de dados digitais de alta qualidade para formatos que podem ser reproduzidos numa ampla variedade de dispositivos e browsers. Por exemplo, pode querer fluxo que formatos de conteúdo do HLS ou MPEG DASH da Apple. Os Media Services também permite-lhe analisar o conteúdo de vídeo ou áudio. Este tópico dão-lhe orientações sobre como codificar o conteúdo com os Media Services v3.

Para codificar com os Media Services v3, terá de criar uma tarefa e uma transformação. Uma transformação define as receitas para as suas definições de codificação e saídas e a tarefa é uma instância das receitas. Para obter mais informações, consulte [transforma e tarefas](transform-concept.md)

Quando encoding com Media Services do Azure, pode utilizar predefinições para o codificador de saber como os ficheiros de suporte de dados de entrada devem ser processados. Por exemplo, pode especificar a resolução de vídeo e/ou o número de canais de áudio que quiser no conteúdo codificado. 

Pode começar rapidamente com um das predefinições de incorporada recomendadas com base nas melhores práticas industriais ou pode optar por criar um personalizado predefinido para os seus requisitos específicos de cenário ou dispositivo de destino. 

Localizar os detalhes sobre o codificador no [OpenAPI especificação](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/preview/2018-03-30-preview). 

## <a name="built-in-presets"></a>Predefinições incorporadas

Os Media Services suportam atualmente as predefinições de codificação incorporadas seguintes:  

|**Nome da configuração predefinida**|**Cenário**|**Detalhes**|
|---|---|---|
|**AudioAnalyzerPreset**|Análise de áudio|A predefinição aplica-se um conjunto predefinido de operações de análise baseada em AI, incluindo transcription de reconhecimento de voz. Atualmente, a predefinição suporta o processamento do conteúdo com um único registo de áudio.<br/>Pode especificar o idioma para o payload de áudio o utilizando o formato de BCP 47 de 'tag-região de idioma' de entrada (por exemplo, "en-US"). A lista de idiomas suportados são 'en-US', 'en-GB', 'es-ES', "es MX", fr-FR, it-IT, ja-JP, pt-BR, zh-CN.|
|**VideoAnalyzerPreset**|Análise de áudio e vídeo|Extrai insights (metadados avançado) de áudio e vídeo e produz um ficheiro de formato JSON. Pode especificar se pretende apenas extrair insights áudio ao processar um ficheiro de vídeo. Para obter mais informações, consulte [as vídeo de analisar](analyze-videos-tutorial-with-api.md).|
|**BuiltInStandardEncoderPreset**|Transmissão em Fluxo|Utilizado para definir um incorporado para codificar o vídeo de entrada com o codificador padrão da configuração predefinido. <br/>Atualmente são suportadas as seguintes predefinições:<br/>**EncoderNamedPreset.AdaptiveStreaming** (recomendado). Para obter mais informações, consulte [ladder uma velocidade de transmissão a geração automática](autogen-bitrate-ladder.md).<br/>**EncoderNamedPreset.AACGoodQualityAudio** -produz um único ficheiro MP4 que contém apenas stereo áudio codificado em 192 kbps.<br/>**EncoderNamedPreset.H264MultipleBitrate1080p** -produz um conjunto de 8 ficheiros MP4 alinhada GOP, vão de 6000 kbps 400 kbps e stereo áudio AAC. Resolução inicia à 1080p e passa-se para baixo para 360 p.<br/>**EncoderNamedPreset.H264MultipleBitrate720p** -produz um conjunto de 6 ficheiros MP4 alinhada GOP, vão de 3400 kbps 400 kbps e stereo áudio AAC. Resolução inicia à 720p e passa-se para baixo para 360 p.<br/>**EncoderNamedPreset.H264MultipleBitrateSD** -produz um conjunto de 5 ficheiros MP4 alinhada GOP, vão de 1600 kbps 400 kbps e stereo áudio AAC. Resolução inicia à 480p e passa-se para baixo para 360 p.<br/><br/>Para obter mais informações, consulte [Uploading, codificação e ficheiros de transmissão em fluxo](stream-files-tutorial-with-api.md).|
|**StandardEncoderPreset**|Transmissão em Fluxo|Descreve as definições a serem utilizados quando a codificação o vídeo de entrada com o codificador padrão. <br/>Utilize esta configuração predefinida quando Personalizar predefinições de transformação. Para obter mais informações, consulte [como personalizar predefinições de transformação](customize-encoder-presets-how-to.md).|

## <a name="custom-presets"></a>Predefinições personalizadas

Os Media Services suportam totalmente a personalizar todos os valores nas predefinições para satisfazer as suas necessidades específicas de codificação e requisitos. Utilizar o **StandardEncoderPreset** predefinido ao personalizar predefinições de transformação. Um detalhadas para uma explicação e exemplo, consulte [como personalizar predefinições de codificador](customize-encoder-presets-how-to.md).

## <a name="scaling-encoding-in-v3"></a>Codificação na v3 de dimensionamento

Atualmente, os clientes têm de utilizar o portal do Azure ou APIs do AMS v2 para definir RUs (conforme descrito em [dimensionamento de processamento de suporte de dados](../previous/media-services-scale-media-processing-overview.md). 

## <a name="next-steps"></a>Passos Seguintes

### <a name="tutorials"></a>Tutoriais

Os tutorals seguintes mostram como codificar o conteúdo com os Media Services:

* [Carregar, codificar e transmitir em fluxo utilizando os Media Services do Azure](stream-files-tutorial-with-api.md)
* [Analisar vídeos com Media Services do Azure](analyze-videos-tutorial-with-api.md)

### <a name="code-samples"></a>Exemplos de código

Os seguintes exemplos de código contenham código que mostra como codificar com os Media Services:

* [.NET Core](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore)
* [CLI 2.0](https://github.com/Azure/azure-docs-cli-python-samples/tree/master/media-services)

### <a name="sdks"></a>SDKs

Pode utilizar qualquer um dos SDKs de v3 dos Media Services seguintes suportados para codificar o conteúdo.

* [CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
* [REST](https://docs.microsoft.com/rest/api/media/transforms)
* [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/mediaservices/management?view=azure-dotnet)
* [Java](https://docs.microsoft.com/java/api/overview/azure/mediaservices)
* [Python](https://docs.microsoft.com/python/api/overview/azure/media-services?view=azure-python)

