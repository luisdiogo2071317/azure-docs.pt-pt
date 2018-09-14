---
title: Conceitos do Video Indexer do Azure | Documentos da Microsoft
description: Este tópico descreve alguns dos conceitos do serviço do Video Indexer.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 07/31/2018
ms.author: juliako
ms.openlocfilehash: 67bbb7046d71b904dfdcaad0e44024e8f30ac288
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45543714"
---
# <a name="video-indexer-concepts"></a>Conceitos de indexador de vídeo
 
Este artigo descreve alguns dos conceitos do serviço do Video Indexer.
    
## <a name="summarized-insights"></a>Insights resumidos

Insights resumidos contêm uma exibição agregada dos dados: rostos, tópicos, emoções. Por exemplo, em vez de passar por cada uma das milhares de intervalos de tempo e verificando quais faces estão no mesmo, as informações resumidas contém todos os rostos e para cada um deles, os intervalos de tempo que aparece no e % do tempo é mostrado.

## <a name="time-range-vs-adjusted-time-range"></a>intervalo de tempo vs. o intervalo de tempo ajustado

TimeRange é o intervalo de tempo no vídeo original. AdjustedTimeRange é o intervalo de tempo relativo à lista de reprodução atual. Uma vez que pode criar uma lista de reprodução de linhas diferentes de vídeos diferentes, pode demorar um vídeo de 1 hora e utilizar apenas 1 linha dela, por exemplo, 10:00-10:15. Nesse caso, terá uma lista de reprodução com 1 linha, em que o intervalo de tempo é 10:00-10:15, mas o adjustedTimeRange é 00:00 00:15.
 
## <a name="blocks"></a>blocos

Blocos foram feitos para facilitar a percorrer os dados. Por exemplo, bloco pode ser dividido com base no quando alterar do alto-falantes ou há uma longa pausa.

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre como começar, consulte [como inscrever-se e carregar o seu primeiro vídeo](video-indexer-get-started.md).

## <a name="see-also"></a>Consulte também

[Descrição geral do indexador vídeo](video-indexer-overview.md)
