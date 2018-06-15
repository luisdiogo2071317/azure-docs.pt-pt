---
title: Utilize o indexador de vídeo do Azure para criar insights vídeos de vídeos existentes | Microsoft Docs
description: Este tópico mostra como criar e publicar as vídeos insights com base em alguns outro vídeo.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 05/30/2018
ms.author: juliako
ms.openlocfilehash: 1834fa9f1dd7db618850ab897a85a1bcfdaa89ac
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355880"
---
# <a name="create-video-insights-from-existing-videos"></a>Criar insights vídeos a partir de vídeos existentes

Este tópico mostra como criar e publicar as vídeos insights com base em alguns outro vídeo.

1. Inicie sessão no seu [vídeo indexador](https://api-portal.videoindexer.ai/) conta.
2. Localize um vídeo a partir do qual pretende criar as suas informações de vídeos.
3. Prima **reproduzir**.

    A página mostra insights resumidos as vídeo. 

    ![Informações](./media/video-indexer-create-new/video-indexer-summarized-insights.png)

3. Prima a **editar** botão.

    Esta página mostra a repartição completa de um vídeo. A divisão está dividido em blocos. Os blocos aqui são tornar mais fácil percorrer os dados. Por exemplo, blocos poderão ser divididos com base em quando speakers alterar ou se ocorrer uma pausa longa. Pode criar as suas próprias listas de reprodução que contém apenas linhas que pretende. Para mostrar apenas a determinados partes da origem de vídeo, pode filtrar por tópicos/palavras-chave, sentiments, pessoas, speakers. Pode escolher ver apenas o vídeo transcript ou OCR.    

    ![Informações](./media/video-indexer-create-new/video-indexer-create-new-playlist.png)

4. Crie a lista de reprodução.

    Para adicionar ou remover linhas da sua lista de reprodução, prima **+** / **-**.

5. Pré-visualize a lista de reprodução.

    Quando tiver terminado a criar a lista de reprodução, prima **pré-visualização**.
6. Publica a lista de reprodução.

    Depois de pré-visualizar as listas de reprodução, pode publicá-lo.

    Depois de publicar a lista de reprodução, é adicionado à lista das suas informações de vídeos.


## <a name="next-steps"></a>Passos Seguintes 

Depois de criar a nova lista de reprodução, pode continuar a processar, conforme descrito em um dos seguintes tópicos: 

- [Conteúdo processos com a API de REST do indexador de vídeo](video-indexer-use-apis.md)
- [Incorporar visual widgets na sua aplicação](video-indexer-embed-widgets.md)

## <a name="see-also"></a>Consulte também

[Descrição geral do vídeo indexador](video-indexer-overview.md) 
