---
title: Ver e editar insights indexador de vídeo do Azure | Microsoft Docs
description: Este tópico demonstra como visualizar e editar insights indexador de vídeo.
services: cognitive services
documentationcenter: ''
author: juliako
ms.service: cognitive-services
ms.topic: article
ms.date: 05/30/2018
ms.author: juliako
ms.openlocfilehash: df4e2152f0f3ff0a2cc6a4dbb8c46478d5db7dc7
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355879"
---
# <a name="view-and-edit-video-indexer-insights"></a>Ver e editar insights indexador de vídeo

Este tópico mostra como visualizar e editar as informações de indexador gráfica de um vídeo.

1. Inicie sessão no seu [vídeo indexador](https://api-portal.videoindexer.ai/) conta.
2. Localize um vídeo a partir do qual pretende criar as suas informações de indexador vídeo. Para obter mais informações, consulte [localizar instantes exatos dentro de vídeos](video-indexer-search.md).
3. Prima **reproduzir**.

    A página mostra insights resumidos as vídeo. 

    ![Informações](./media/video-indexer-view-edit/video-indexer-summarized-insights.png)

4. Ver as informações do vídeo resumidas. 

    Insights resumidos mostram uma vista dos dados agregada: faces, palavras-chave, sentiments. Por exemplo, pode ver os faces de pessoas e os intervalos de tempo que cada enfrentam aparece no e a % de tempo que é mostrado.

    O leitor e as informações são sincronizadas. Por exemplo, se clicar uma palavra-chave ou da linha de transcript, leitor de proporciona nesse momento as vídeo. Pode obter a vista de leitor/insights e sincronização na sua aplicação. Para obter mais informações, consulte [widgets de incorporar o indexador de Azure na sua aplicação](video-indexer-embed-widgets.md). 

3. Edite as informações do indexador de vídeo.

    Prima Editar sob o vídeo. É apresentada a página que mostra a repartição completa de um vídeo. A divisão está dividido em blocos. Os blocos aqui são tornar mais fácil percorrer os dados. Por exemplo, blocos poderão ser divididos com base em quando speakers alterar ou se ocorrer uma pausa longa. Pode criar as suas próprias listas de reprodução que contém apenas linhas que pretende. Para mostrar apenas a determinados partes da origem de vídeo, pode filtrar por tópicos/palavras-chave, sentiments, pessoas, speakers. Pode escolher ver apenas o vídeo transcript ou OCR.  

    ![Informações](./media/video-indexer-view-edit/video-indexer-create-new-playlist.png)

## <a name="next-steps"></a>Passos Seguintes

[Saiba como criar as suas próprias informações de indexador vídeo com base em alguns outro vídeo](video-indexer-create-new.md).

## <a name="see-also"></a>Consulte também

[Descrição geral do vídeo indexador](video-indexer-overview.md)

