---
title: Conceitos de indexador de vídeo
titlesuffix: Azure Media Services
description: Este tópico descreve alguns dos conceitos do serviço do Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 11/19/2018
ms.author: juliako
ms.openlocfilehash: fde98dac45eeda9ec2c842b9d8e0f93134ab8092
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/22/2018
ms.locfileid: "52292335"
---
# <a name="video-indexer-concepts"></a>Conceitos de indexador de vídeo
 
Este artigo descreve alguns dos conceitos do serviço do Video Indexer.
    
## <a name="summarized-insights"></a>Insights resumidos

Insights resumidos contêm uma exibição agregada dos dados: rostos, tópicos, emoções. Por exemplo, em vez de passar por cada uma das milhares de intervalos de tempo e verificando quais faces estão no mesmo, as informações resumidas contém todos os rostos e para cada um deles, os intervalos de tempo que aparece no e % do tempo é mostrado.

## <a name="time-range-vs-adjusted-time-range"></a>intervalo de tempo vs. o intervalo de tempo ajustado

TimeRange é o intervalo de tempo no vídeo original. AdjustedTimeRange é o intervalo de tempo relativo à lista de reprodução atual. Uma vez que pode criar uma lista de reprodução de linhas diferentes de vídeos diferentes, pode demorar um vídeo de 1 hora e utilizar apenas 1 linha dela, por exemplo, 10:00-10:15. Nesse caso, terá uma lista de reprodução com 1 linha, em que o intervalo de tempo é 10:00-10:15, mas o adjustedTimeRange é 00:00 00:15.
 
## <a name="blocks"></a>blocos

Blocos foram feitos para facilitar a percorrer os dados. Por exemplo, o bloco pode estar dividido com base nos momentos em que ocorre uma mudança de orador ou uma longa pausa.

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre como começar, consulte [como inscrever-se e carregar o seu primeiro vídeo](video-indexer-get-started.md).

## <a name="see-also"></a>Consulte também

[Descrição geral do Video Indexer](video-indexer-overview.md)
