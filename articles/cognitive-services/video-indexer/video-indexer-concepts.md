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
ms.openlocfilehash: 224c8b05027f51fb99c8d58be34c3604032c0f77
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399500"
---
# <a name="video-indexer-concepts"></a>Conceitos de indexador de vídeo
 
Este tópico descreve alguns dos conceitos do serviço do Video Indexer.
    
## <a name="summarized-insights"></a>Insights resumidos

Insights resumidos contêm uma exibição agregada dos dados: rostos, palavras-chave, sentimentos. Por exemplo, em vez de passar por cada uma das milhares de intervalos de tempo e verificando quais faces estão no mesmo, as informações resumidas contém todos os rostos e para cada um deles, os intervalos de tempo que aparece no e % do tempo é mostrado.

## <a name="topicskeywords"></a>Tópicos/palavras-chave

Tópicos/palavras-chave são na lista de expressões-chave que extrai do Video Indexer do texto. Por exemplo, um vídeo de Scott Guthrie pode conter as seguintes tópicos/palavras-chave: segurança, Azure, Cloud da Microsoft, receita.

## <a name="sentiments"></a>sentimentos

Quando o indexador de vídeo analisa as transcrições, Deteta sentimentos também. Por exemplo, "Este é um evento muito interessante" é um sentimento positivo.

## <a name="time-range-vs-adjusted-time-range"></a>intervalo de tempo vs. o intervalo de tempo ajustado

TimeRange é o intervalo de tempo no vídeo original. AdjustedTimeRange é o intervalo de tempo relativo à lista de reprodução atual. Uma vez que pode criar uma lista de reprodução de linhas diferentes de vídeos diferentes, pode demorar um vídeo de 1 hora e utilizar apenas 1 linha dela, por exemplo, 10:00-10:15. Nesse caso, terá uma lista de reprodução com 1 linha, em que o intervalo de tempo é 10:00-10:15, mas o adjustedTimeRange é 00:00 00:15.
 
## <a name="blocks"></a>blocos

Blocos foram feitos para facilitar a percorrer os dados. Por exemplo, bloco pode ser dividido com base no quando alterar do alto-falantes ou há uma longa pausa.

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre como começar, consulte [como inscrever-se e carregar o seu primeiro vídeo](video-indexer-get-started.md).

## <a name="see-also"></a>Consulte também

[Descrição geral do indexador vídeo](video-indexer-overview.md)
