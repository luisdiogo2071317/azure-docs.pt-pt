---
title: Utilize o indexador de vídeos do Azure para criar informações de vídeo a partir de vídeos existentes | Documentos da Microsoft
description: Este tópico mostra-lhe como criar e publicar informações de vídeo com base em algum outro vídeo.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 07/25/2018
ms.author: juliako
ms.openlocfilehash: 161a47f72a0f8038a515c09f25ec2a8e8a520547
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39390845"
---
# <a name="create-highlights-from-existing-videos"></a>Crie destaques de vídeos existentes

Este tópico mostra-lhe como criar e publicar informações de vídeo com base em algum outro vídeo.

1. Inicie sessão no seu [Video Indexer](https://api-portal.videoindexer.ai/) conta.
2. Encontre um vídeo a partir do qual pretende criar as suas informações de vídeo.
3. Prima **reproduzir**.

    A página mostra informações de resumida do vídeo. 

    ![Informações](./media/video-indexer-create-new/video-indexer-summarized-insights.png)

3. Prima a **editar** botão.

    Esta página mostra a divisão completa de um vídeo. A divisão é dividida em blocos. Os blocos são aqui para que seja mais fácil passar os dados. Por exemplo, bloco pode ser dividido com base no quando alterar do alto-falantes ou há uma longa pausa. Pode criar sua própria lista de reprodução que contém apenas as linhas que pretende. Para mostrar apenas partes específicas da origem de vídeo, pode filtrar por tópicos/palavras-chave, sentimentos, pessoas, alto-falantes. Pode optar por ver apenas a transcrição do vídeo ou a OCR.    

    ![Informações](./media/video-indexer-create-new/video-indexer-create-new-playlist.png)

4. Crie sua lista de reprodução.

    Para adicionar ou remover linhas em sua lista de reprodução, prima **+** / **-**.

5. Pré-visualize a lista de reprodução.

    Depois de concluída a criar a lista de reprodução, prima **pré-visualização**.
6. Publica a lista de reprodução.

    Depois de visualizar a lista de reprodução, pode publicá-lo.

    Depois de publicar a lista de reprodução, é adicionado à lista de informações de vídeo.


## <a name="next-steps"></a>Passos Seguintes 

Depois de criar a nova lista de reprodução, pode continuar a processar, conforme descrito em um dos seguintes tópicos: 

- [Processar conteúdo com a API de REST do indexador de vídeo](video-indexer-use-apis.md)
- [Incorporar visual widgets na sua aplicação](video-indexer-embed-widgets.md)

## <a name="see-also"></a>Consulte também

[Descrição geral do indexador vídeo](video-indexer-overview.md) 
