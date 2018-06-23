---
title: Conceitos do indexador de vídeo do Azure | Microsoft Docs
description: Este tópico descreve alguns conceitos do serviço indexador de vídeo.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 04/17/2017
ms.author: juliako
ms.openlocfilehash: 01d92a6b55d2fb2c09cee333f482d79d2cdf763c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354572"
---
# <a name="video-indexer-concepts"></a>Conceitos de indexador vídeos
 
Este tópico descreve alguns conceitos do serviço indexador de vídeo.
    
## <a name="summarized-insights"></a>Insights resumidos

Insights resumidos contém uma vista dos dados agregada: faces, palavras-chave, sentiments. Por exemplo, em vez de passar por cada uma das milhares de intervalos de tempo e verificar quais faces estão no mesmo, o insights resumido contém todos os faces e para cada um deles, os intervalos de tempo que é apresentado no e a % de tempo é mostrado.

## <a name="topicskeywords"></a>Tópicos/palavras-chave

Tópicos/palavras-chave são na lista de expressões chaves que extrai o indexador de vídeo do texto. Por exemplo, um vídeo de blogue Guthrie pode conter as seguintes tópicos/palavras-chave: segurança do Azure, Microsoft Cloud, de receitas.

## <a name="sentiments"></a>sentiments

Quando as vídeo indexador analisa transcrições, Deteta sentiments bem. Por exemplo, "Este é um evento muito e extraordinária" é um sentimento positivo.

## <a name="time-range-vs-adjusted-time-range"></a>intervalo de tempo vs. o intervalo de tempo ajustada

TimeRange é o intervalo de tempo as vídeo original. AdjustedTimeRange é o intervalo de tempo relativo à lista de reprodução atual. Uma vez que pode criar uma lista de reprodução de linhas diferentes de vídeos diferentes, pode demorar um vídeo de 1 hora e utilizar apenas 1 linha a partir dos mesmos, por exemplo, 10:00-10:15. Nesse caso, terá de uma lista de reprodução com 1 linha, onde o intervalo de tempo é 10-10:00:15, mas o adjustedTimeRange é 00:00-00:15.
 
## <a name="blocks"></a>Blocos de

Blocos se destinam a tornar mais fácil percorrer os dados. Por exemplo, blocos poderão ser divididos com base em quando speakers alterar ou se ocorrer uma pausa longa.

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre como começar a utilizar, consulte [como inscrever e carregar o vídeo primeiro](video-indexer-get-started.md).

## <a name="see-also"></a>Consulte também

[Descrição geral do vídeo indexador](video-indexer-overview.md)

