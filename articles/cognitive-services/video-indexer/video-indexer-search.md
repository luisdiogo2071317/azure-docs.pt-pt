---
title: Utilize o indexador de vídeo do Azure para localizar momentos exatos nos vídeos | Documentos da Microsoft
description: Este tópico demonstra como localizar momentos exatos nos vídeos.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 09/09/2018
ms.author: juliako
ms.openlocfilehash: 72f110c0c98e6fb6ee5fca684fdc74c6986e032c
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45541411"
---
# <a name="find-exact-moments-within-videos"></a>Localizar momentos exatos nos vídeos

Este tópico mostra-lhe as opções de pesquisa que lhe permitem localizar momentos exatos nos vídeos.

1. Navegue para o [Video Indexer](https://www.videoindexer.ai/) Web site e o início de sessão.
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
- [Processar conteúdo com a API REST do Video Indexer](video-indexer-use-apis.md)
- [Incorporar widgets visuais na sua aplicação](video-indexer-embed-widgets.md)

## <a name="see-also"></a>Consulte também

[Descrição geral do indexador vídeo](video-indexer-overview.md)
