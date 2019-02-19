---
title: Codificação na cloud com serviços de multimédia - Azure | Documentos da Microsoft
description: Este tópico descreve o processo de codificação ao utilizar os serviços de multimédia do Azure
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/17/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 52e7fdf6de25300d4f78ee9822aca4ad83f646e9
ms.sourcegitcommit: 4bf542eeb2dcdf60dcdccb331e0a336a39ce7ab3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/19/2019
ms.locfileid: "56408430"
---
# <a name="encoding-with-media-services"></a>Encoding com Media Services

Serviços de multimédia do Azure permite-lhe a codificar os seus ficheiros multimédia digital de alta qualidade em formatos que podem ser jogados numa grande variedade de navegadores e dispositivos. Por exemplo, pode querer transmitir o conteúdo nos formatos HLS ou MPEG DASH da Apple. Este tópico fornece orientações sobre como codificar o conteúdo com serviços de multimédia v3.

Codificar com serviços de multimédia v3, tem de criar uma [transformar](https://docs.microsoft.com/rest/api/media/transforms) e uma [tarefa](https://docs.microsoft.com/rest/api/media/jobs). Uma transformação define a receita para as suas definições e saídas de codificação e a tarefa é uma instância da receita. Para obter mais informações, consulte [transforma e tarefas](transforms-jobs-concept.md)

Quando o encoding com Media Services, vai utilizar configurações predefinidas para informar ao codificador, como os ficheiros de suporte de dados de entrada devem ser processados. Por exemplo, pode especificar a resolução de vídeo e/ou o número de canais de áudio que pretende no conteúdo codificado. 

Pode começar a utilizar rapidamente com um dos pré-visualizando incorporada recomendado com base nas práticas recomendadas do setor ou pode optar por criar um personalizado predefinido para seus requisitos específicos de cenário ou dispositivo de destino. Para obter mais informações, consulte [codificar com uma transformação personalizada](customize-encoder-presets-how-to.md). 

A partir de 2019 Janeiro, quando o encoding com Media Encoder Standard para produzir ficheiros MP4, um novo ficheiro de .mpi é gerado e adicionado à saída de ativo. Este ficheiro MPI destina-se para melhorar o desempenho para [empacotamento dinâmico](dynamic-packaging-overview.md) e cenários de transmissão em fluxo.

> [!NOTE]
> Não deve modificar ou remover o arquivo MPI ou fazer qualquer dependência no seu serviço na existência (ou não) de um arquivo desse tipo.

## <a name="built-in-presets"></a>Configurações predefinidas incorporadas

Atualmente, os serviços de multimédia suporta as seguintes predefinições de codificação incorporadas:  

### <a name="builtinstandardencoderpreset-preset"></a>Configuração predefinida de BuiltInStandardEncoderPreset

[BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) é usado para definir um incorporado na configuração predefinido para a codificação de vídeo de entrada com o codificador Standard. 

Atualmente são suportadas as seguintes predefinições:

- **EncoderNamedPreset.AdaptiveStreaming** (recomendado). Para obter mais informações, consulte [criação automática de uma escala de bits](autogen-bitrate-ladder.md).
- **EncoderNamedPreset.AACGoodQualityAudio** -produz um ficheiro MP4 único que contém apenas estéreo áudio codificado em 192 kbps.
- **EncoderNamedPreset.H264MultipleBitrate1080p** -produz um conjunto de ficheiros MP4 de alinhado GOP de 8, desde 6000 kbps até 400 kbps e áudio AAC estéreo. Resolução começa em 1080p e vai até 360p.
- **EncoderNamedPreset.H264MultipleBitrate720p** -produz um conjunto de 6 ficheiros MP4 GOP alinhados, desde 3400 kbps até 400 kbps e áudio AAC estéreo. Resolução é iniciada à 720p e vai até 360p.
- **EncoderNamedPreset.H264MultipleBitrateSD** -produz um conjunto de ficheiros MP4 de alinhado GOP de 5, desde 1600 kbps até 400 kbps e áudio AAC estéreo. Resolução é iniciada à 480p e vai até 360p.<br/><br/>Para obter mais informações, consulte [carregar, codificar e ficheiros de transmissão em fluxo](stream-files-tutorial-with-api.md).

### <a name="standardencoderpreset-preset"></a>Configuração predefinida de StandardEncoderPreset

[StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) descreve as definições para serem utilizadas quando a codificação de vídeo de entrada com o codificador Standard. Utilize esta configuração predefinida ao personalizar as configurações predefinidas de transformação. 

#### <a name="custom-presets"></a>Configurações predefinidas personalizadas

Serviços de multimédia suporta totalmente a personalizar todos os valores nas predefinições para atender às suas necessidades específicas de codificação e requisitos. Utilizar o **StandardEncoderPreset** predefinidos ao personalizar as configurações predefinidas de transformação. Uma detalhada para explicações e de exemplo, veja [como personalizar as configurações predefinidas de codificador](customize-encoder-presets-how-to.md).

## <a name="scaling-encoding-in-v3"></a>Dimensionar codificação da v3

Atualmente, os clientes têm a utilizar o portal do Azure ou serviços de multimédia v2 APIs para definir o RUs (conforme descrito em [Dimensionar processamento de multimédia](../previous/media-services-scale-media-processing-overview.md). 

## <a name="next-steps"></a>Passos Seguintes

* [Transformações e tarefas](transforms-jobs-concept.md)
* [Carregar, codificar e transmitir em fluxo através dos Media Services](stream-files-tutorial-with-api.md)
