---
title: Utilize o indexador de vídeo do Azure para localizar momentos exatos nos vídeos | Documentos da Microsoft
description: Este tópico demonstra como localizar momentos exatos nos vídeos.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 07/31/2018
ms.author: juliako
ms.openlocfilehash: 1cffa067d8028adab4dbcc82c529f77d980ce6be
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397548"
---
# <a name="find-exact-moments-within-videos"></a>Localizar momentos exatos nos vídeos

Este tópico mostra-lhe as opções de pesquisa que lhe permitem localizar momentos exatos nos vídeos.

1. Inicie sessão no seu [Video Indexer](https://api-portal.videoindexer.ai/) conta.
2. Pesquise entre todos os vídeos na sua conta.

    No exemplo a seguir, vamos pesquisados todos os vídeos criados por Channel 9 com Scott Hanselman.

    ![Pesquisa](./media/video-indexer-search/video-indexer-search01.png)
    
3. Pesquise as informações resumidas do vídeo.

    Em seguida, pode procurar dentro de um vídeo ao clicar em **reproduzir** no vídeo. Em seguida, pode pesquisar no vídeo, selecionando o **pesquisa** separador. Por exemplo, estamos procurando todos os locais onde o texto "identity protection" é utilizado. 

    ![Pesquisa](./media/video-indexer-search/video-indexer-search02.png)

    Se clicar em um dos resultados, o jogador traz até para esse momento no vídeo. Pode obter a vista de leitor/informações e a sincronização em seu aplicativo. Para obter mais informações, consulte [widgets de incorporar o indexador de vídeos na sua aplicação](video-indexer-embed-widgets.md). 

    
4. Pesquise a divisão detalhada do vídeo.

    Se quiser criar seu próprio divisão com base no vídeo que encontrou, prima a **editar** botão. Esta página mostra a divisão completa de um vídeo. Fazer uma pesquisa na divisão para mostrar apenas as linhas que está interessado. Para obter mais informações, consulte [ver e editar informações do indexador de vídeo](video-indexer-view-edit.md).

    Neste exemplo, podemos pesquisado o texto "identity protection". Também Aplicamos filtros adicionais, conforme mostrado no ecrã abaixo.

    ![Pesquisa](./media/video-indexer-search/video-indexer-search03.png)

## <a name="next-steps"></a>Passos Seguintes 

Depois de localizar o vídeo que pretende trabalhar com, pode continuar a processar o vídeo, conforme descrito em um dos seguintes tópicos: 

- [Criar novas informações de vídeo com base no vídeo existente](video-indexer-create-new.md)
- [Processar conteúdo com a API de REST do indexador de vídeo](video-indexer-use-apis.md)
- [Incorporar visual widgets na sua aplicação](video-indexer-embed-widgets.md)

## <a name="see-also"></a>Consulte também

[Descrição geral do indexador vídeo](video-indexer-overview.md)
