---
title: Latência de LiveEvent nos serviços de multimédia do Azure | Documentos da Microsoft
description: Este tópico fornece uma visão geral de latência de LiveEvent e mostra como definir a baixa latência.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 11/16/2018
ms.author: juliako
ms.openlocfilehash: a74f2e53127b506f42ff49018c3df2985396646d
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/29/2018
ms.locfileid: "52619826"
---
# <a name="liveevent-latency-in-media-services"></a>Latência de LiveEvent nos serviços de multimédia

Este artigo mostra como definir a baixa latência num **LiveEvent**. Ele também aborda típicos resultados que vê ao usar as definições de baixa latência em diversos leitores. Os resultados variam com base na latência de rede e da CDN. 

Utilizar a nova **LowLatency** conjunto de recursos, o **StreamOptionsFlag** para **LowLatency** sobre a **LiveEvent**. Depois do fluxo está em execução, pode utilizar o [leitor de multimédia do Azure](http://ampdemo.azureedge.net/) (AMP), página de demonstração e definir as opções de reprodução para utilizar o "baixa latência heurística perfil".

O exemplo de .NET seguinte mostra como definir **LowLatency** sobre o **LiveEvent**:

```csharp
LiveEvent liveEvent = new LiveEvent(
            location: mediaService.Location, 
            description: "Sample LiveEvent for testing",
            vanityUrl: false,
            encoding: new LiveEventEncoding(
                        // Set this to Basic to enable a transcoding LiveEvent, and None to enable a pass-through LiveEvent
                        encodingType:LiveEventEncodingType.None, 
                        presetName:null
                    ),
            input: new LiveEventInput(LiveEventInputProtocol.RTMP,liveEventInputAccess), 
            preview: liveEventPreview,
            streamOptions: new List<StreamOptionsFlag?>()
            {
                // Set this to Default or Low Latency
                // To use low latency optimally, you should tune your encoder settings down to 1 second GOP size instead of 2 seconds.
                StreamOptionsFlag.LowLatency
            }
        );
```                

Veja o exemplo completo: [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

## <a name="pass-through-liveevents-latency"></a>Latência de LiveEvents pass-through

A tabela seguinte mostra os resultados típicos para latência (quando o sinalizador de LowLatency está ativado) nos serviços de multimédia, medido a partir do momento no feed de contribuição atinge o serviço quando um jogador pode pedir a reprodução.

||2s GOP baixa latência ativada|1s GOP baixa latência ativada|
|---|---|---|
|TRAVESSÃO no AMP|10s|8S|
|HLS no player de iOS nativo|14s|10s|

> [!NOTE]
> A latência de ponto a ponto pode variar dependendo das condições de rede local ou com a introdução de uma camada de colocação em cache da CDN. Deve testar suas configurações exatas.

## <a name="next-steps"></a>Passos Seguintes

- [Descrição geral de transmissão em fluxo em direto](live-streaming-overview.md)
- [Tutorial de transmissão em fluxo em direto](stream-live-tutorial-with-api.md)

