---
title: Moderator conteúdo do Azure - moderação de interrupção de vídeo | Microsoft Docs
description: Utilize assistida por máquina moderação de interrupção gráfica e ferramentas de revisão humanos para moderada de conteúdos inadequados
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/20/2018
ms.author: sajagtap
ms.openlocfilehash: fb26c9af55381c80a3f520b1a0068d8f72c91061
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351530"
---
# <a name="video-moderation"></a>Moderação de vídeos

Moderator de conteúdo a utilização máquina assistida [moderação de interrupção gráfica](video-moderation-api.md) e [revisão humana ferramenta](Review-Tool-User-Guide/human-in-the-loop.md) moderados vídeos e transcrições de adulto (explícita) e racy conteúdo (suggestive) para obter os melhores resultados para o negócio.

## <a name="video-trained-classifier"></a>Classificador de preparação de vídeo

Classificação de vídeo assistida por máquina está é conseguida com modelos de imagem de preparação ou modelos de formação de vídeos. Ao contrário da imagem de preparação as vídeos classificadores, classificador de vídeo para adultos e racy da Microsoft está preparado com vídeos. Este método resulta numa melhor qualidade de correspondência.

## <a name="shot-detection"></a>Captura de deteção

Ao exportar os detalhes de classificação, intelligence vídeo adicional ajuda com mais flexibilidade na análise vídeos. Em vez de exportar apenas os frames Jumbo, serviço de moderação de interrupção gráfica da Microsoft fornece informações de nível de captura demasiado. Tem agora a opção para analisar os seus vídeos o nível de captura e o nível de moldura.
 
## <a name="key-frame-detection"></a>Deteção de fotograma chave

Em vez de exportar frames em intervalos regulares, o serviço de moderação de interrupção gráfica identifica e produz apenas potencialmente concluídas frames (boas). A funcionalidade permite a geração de moldura eficiente para uma análise para adultos e racy ao nível da moldura.

O extrair seguinte mostra uma resposta parcial com capturas de potenciais, frames chaves e pontuações para adultos e racy:

    "fragments": [
    {
      "start": 0,
      "duration": 18000
    },
    {
      "start": 18000,
      "duration": 3600,
      "interval": 3600,
      "events": [
        [
          {
            "reviewRecommended": false,
            "adultScore": 0.00001,
            "racyScore": 0.03077,
            "index": 5,
            "timestamp": 18000,
            "shotIndex": 0
          }
        ]
      ]
    },
    {
      "start": 18386372,
      "duration": 119149,
      "interval": 119149,
      "events": [
        [
          {
            "reviewRecommended": true,
            "adultScore": 0.00000,
            "racyScore": 0.91902,
            "index": 5085,
            "timestamp": 18386372,
            "shotIndex": 62
          }
        ]
      ]


## <a name="visualization-for-human-reviews"></a>Visualização revisões humanos

Para obter mais informações nuanced casos, as empresas precisam de uma solução de revisão humanos para compor o vídeo, o frames Jumbo e etiquetas máquina atribuída. Os moderators humanos rever vídeos e frames obter uma vista completa de informações do, altere as etiquetas e submeter as decisões.

![Vista de predefinida de ferramenta de revisão de vídeo](images/video-review-default-view.png)

## <a name="player-view-for-video-level-review"></a>Vista de leitor para revisão de nível de vídeo

Decisões de binárias de nível de vídeo são disponibilizadas uma vista de leitor de vídeo mostra frames para adultos e racy potenciais. Os revisores humanos navegue o vídeo com várias opções de velocidade para examinar em segundo plano. Confirmarem os respetivos decisões por ativando ou desativando as etiquetas.

![Vista de leitor de ferramenta de revisão de vídeo](images/video-review-player-view.PNG)

## <a name="frames-view-for-detailed-reviews"></a>Vista de frames para revisões de detalhado

É disponibilizada uma revisão de vídeo detalhada para análise de moldura por moldura com uma vista baseada no intervalo. Os revisores humanos reverem e selecione um ou mais frames e etiquetas para confirmar as decisões de alternar. Um passo opcional seguinte é redaction do frames ofensivas ou conteúdo.

![Ver fotogramas da ferramenta de revisão de vídeo](images/video-review-frames-view-apply-tags.PNG)

## <a name="transcript-moderation"></a>Moderação de interrupção transcript

Vídeos têm normalmente voz ao longo do que necessita de moderação interrupção, bem como para reconhecimento de voz ofensiva. Utilizar o serviço do indexador de suporte de dados do Azure para converter o reconhecimento de voz para texto e utilizar a API de revisão de conteúdo Moderator para submeter transcript para moderação de interrupção de texto dentro da ferramenta de revisão.

![Vista de transcript de ferramenta de revisão de vídeo](images/video-review-transcript-view.png)

## <a name="next-steps"></a>Passos Seguintes

Começar a utilizar o [início rápido de moderação de interrupção gráfica](video-moderation-api.md). 

Saiba como gerar [vídeo revê](video-reviews-quickstart-dotnet.md) para sua revisores humanos da sua saída moderated.

Adicionar [transcript vídeo revê](video-transcript-reviews-quickstart-dotnet.md) para as vídeos revisões.

Veja o tutorial detalhado sobre como desenvolver uma [concluir solução moderação de interrupção gráfica](video-transcript-moderation-review-tutorial-dotnet.md). 
