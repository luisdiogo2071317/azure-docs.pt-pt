---
title: Ver e editar informações do indexador de vídeo do Azure | Documentos da Microsoft
description: Este tópico explica como ver e editar informações do indexador de vídeo.
services: cognitive services
documentationcenter: ''
author: juliako
ms.service: cognitive-services
ms.topic: article
ms.date: 07/31/2018
ms.author: juliako
ms.openlocfilehash: 797c09d72402cfc1ee2524e7792cc1310a53fb1e
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399482"
---
# <a name="view-and-edit-video-indexer-insights"></a>Ver e editar informações do Video Indexer

Este tópico mostra-lhe como ver e editar as informações do indexador de vídeos de um vídeo.

1. Inicie sessão no seu [Video Indexer](https://api-portal.videoindexer.ai/) conta.
2. Encontre um vídeo a partir do qual pretende criar as suas informações do indexador de vídeos. Para obter mais informações, consulte [localizar momentos exatos nos vídeos](video-indexer-search.md).
3. Prima **reproduzir**.

    A página mostra informações de resumida do vídeo. 

    ![Informações](./media/video-indexer-view-edit/video-indexer-summarized-insights.png)

4. Ver as informações resumidas do vídeo. 

    Insights resumidos mostram uma exibição agregada dos dados: rostos, palavras-chave, sentimentos. Por exemplo, pode ver os rostos de pessoas e os intervalos de tempo que cada rosto aparece no e a % de tempo que é mostrado.

    O jogador e as informações são sincronizadas. Por exemplo, se clicar numa palavra-chave ou da linha de transcrição, o jogador traz até para esse momento no vídeo. Pode obter a vista de leitor/informações e a sincronização em seu aplicativo. Para obter mais informações, consulte [widgets de incorporar o indexador do Azure na sua aplicação](video-indexer-embed-widgets.md). 

3. Edite as informações do indexador de vídeo.

    Prima Editar abaixo do vídeo. É apresentada a página que lhe mostra a divisão completa de um vídeo. A divisão é dividida em blocos. Os blocos são aqui para que seja mais fácil passar os dados. Por exemplo, bloco pode ser dividido com base no quando alterar do alto-falantes ou há uma longa pausa. Pode criar sua própria lista de reprodução que contém apenas as linhas que pretende. Para mostrar apenas partes específicas da origem de vídeo, pode filtrar por tópicos/palavras-chave, sentimentos, pessoas, alto-falantes. Pode optar por ver apenas a transcrição do vídeo ou a OCR.  

    ![Informações](./media/video-indexer-view-edit/video-indexer-create-new-playlist.png)

## <a name="next-steps"></a>Passos Seguintes

[Saiba como criar suas próprias informações do indexador de vídeo com base em algum outro vídeo](video-indexer-create-new.md).

## <a name="see-also"></a>Consulte também

[Descrição geral do indexador vídeo](video-indexer-overview.md)

