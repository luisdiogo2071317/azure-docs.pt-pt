---
title: Codificação na cloud com serviços de multimédia do Azure | Documentos da Microsoft
description: Este tópico descreve o processo de codificação ao utilizar os serviços de multimédia do Azure
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
ms.openlocfilehash: 69c5516ee503d774b143bb2d83f09ea863a00b31
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47091554"
---
# <a name="encoding-with-azure-media-services"></a>Encoding com os serviços de multimédia do Azure

Serviços de multimédia do Azure permite-lhe a codificar os seus ficheiros multimédia digital de alta qualidade em formatos que podem ser jogados numa grande variedade de navegadores e dispositivos. Por exemplo, pode querer transmitir o conteúdo nos formatos HLS ou MPEG DASH da Apple. Serviços de multimédia também lhe permite analisar seu conteúdo de vídeo ou áudio. Este tópico dão-lhe orientações sobre como codificar o conteúdo com serviços de multimédia v3.

Para codificar com serviços de multimédia v3, terá de criar uma transformação e uma tarefa. Uma transformação define a receita para as suas definições e saídas de codificação e a tarefa é uma instância da receita. Para obter mais informações, consulte [transforma e tarefas](transform-concept.md)

Ao codificar com os serviços de multimédia do Azure, vai utilizar configurações predefinidas para informar ao codificador, como os ficheiros de suporte de dados de entrada devem ser processados. Por exemplo, pode especificar a resolução de vídeo e/ou o número de canais de áudio que pretende no conteúdo codificado. 

Pode começar a utilizar rapidamente com um dos pré-visualizando incorporada recomendado com base nas práticas recomendadas do setor ou pode optar por criar um personalizado predefinido para seus requisitos específicos de cenário ou dispositivo de destino. 

Encontre informações sobre o codificador na [especificação de OpenAPI](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/preview/2018-03-30-preview). 

## <a name="built-in-presets"></a>Configurações predefinidas incorporadas

Atualmente, os serviços de multimédia suporta as seguintes predefinições de codificação incorporadas:  

|**Nome da configuração predefinida**|**Cenário**|**Detalhes**|
|---|---|---|
|**AudioAnalyzerPreset**|Análise de áudio|A configuração predefinida de aplica-se um conjunto predefinido de operações de análise baseada em IA, incluindo a transcrição de voz. Atualmente, a configuração predefinida suporta o processamento de conteúdo com uma faixa de áudio único.<br/>Também pode especificar o idioma para o payload de áudio na entrada com o formato de BCP 47 de "etiqueta de idioma-região" (por exemplo, "en-US"). A lista de idiomas suportados são "en-US", "en-GB", "es-ES", "es-MX", "fr-FR", it-IT, ja-JP, "pt-BR", "zh-CN".|
|**VideoAnalyzerPreset**|Análise de áudio e vídeo|Extrai informações (metadados avançados) de áudio e vídeo e produz um ficheiro de formato JSON. Pode especificar se pretende apenas extrair informações de áudio ao processar um ficheiro de vídeo. Para obter mais informações, consulte [analisar vídeo](analyze-videos-tutorial-with-api.md).|
|**BuiltInStandardEncoderPreset**|Transmissão em Fluxo|Usado para definir um incorporado na configuração predefinido para a codificação de vídeo de entrada com o codificador Standard. <br/>Atualmente são suportadas as seguintes predefinições:<br/>**EncoderNamedPreset.AdaptiveStreaming** (recomendado). Para obter mais informações, consulte [criação automática de uma escala de bits](autogen-bitrate-ladder.md).<br/>**EncoderNamedPreset.AACGoodQualityAudio** -produz um ficheiro MP4 único que contém apenas estéreo áudio codificado em 192 kbps.<br/>**EncoderNamedPreset.H264MultipleBitrate1080p** -produz um conjunto de ficheiros MP4 de alinhado GOP de 8, desde 6000 kbps até 400 kbps e áudio AAC estéreo. Resolução começa em 1080p e vai até 360p.<br/>**EncoderNamedPreset.H264MultipleBitrate720p** -produz um conjunto de 6 ficheiros MP4 GOP alinhados, desde 3400 kbps até 400 kbps e áudio AAC estéreo. Resolução é iniciada à 720p e vai até 360p.<br/>**EncoderNamedPreset.H264MultipleBitrateSD** -produz um conjunto de ficheiros MP4 de alinhado GOP de 5, desde 1600 kbps até 400 kbps e áudio AAC estéreo. Resolução é iniciada à 480p e vai até 360p.<br/><br/>Para obter mais informações, consulte [carregar, codificar e ficheiros de transmissão em fluxo](stream-files-tutorial-with-api.md).|
|**StandardEncoderPreset**|Transmissão em Fluxo|Descreve as definições para serem utilizadas quando a codificação de vídeo de entrada com o codificador Standard. <br/>Utilize esta configuração predefinida ao personalizar as configurações predefinidas de transformação. Para obter mais informações, consulte [como personalizar as configurações predefinidas de transformação](customize-encoder-presets-how-to.md).|

## <a name="custom-presets"></a>Configurações predefinidas personalizadas

Serviços de multimédia suporta totalmente a personalizar todos os valores nas predefinições para atender às suas necessidades específicas de codificação e requisitos. Utilizar o **StandardEncoderPreset** predefinidos ao personalizar as configurações predefinidas de transformação. Uma detalhada para explicações e de exemplo, veja [como personalizar as configurações predefinidas de codificador](customize-encoder-presets-how-to.md).

## <a name="scaling-encoding-in-v3"></a>Dimensionar codificação da v3

Atualmente, os clientes têm a utilizar o portal do Azure ou APIs do AMS v2 para definir o RUs (conforme descrito em [Dimensionar processamento de multimédia](../previous/media-services-scale-media-processing-overview.md). 

## <a name="next-steps"></a>Passos Seguintes

### <a name="tutorials"></a>Tutoriais

Os tutorals seguintes mostram como codificar o conteúdo com os serviços de multimédia:

* [Carregar, codificar e transmitir em fluxo através dos serviços de multimédia do Azure](stream-files-tutorial-with-api.md)
* [Analisar vídeos com os serviços de multimédia do Azure](analyze-videos-tutorial-with-api.md)

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

