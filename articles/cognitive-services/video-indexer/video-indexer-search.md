---
title: Utilize o indexador de vídeo do Azure para localizar instantes exatos dentro de vídeos | Microsoft Docs
description: Este tópico demonstra como localizar instantes exatos dentro de vídeos.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 05/02/2017
ms.author: juliako
ms.openlocfilehash: 75afa4efa50191466f8850d9b1313cf89a83c0a9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354523"
---
# <a name="find-exact-moments-within-videos"></a>Localizar instantes exatos dentro de vídeos

Este tópico mostra as opções de pesquisa que permitem-lhe localizar instantes exatos dentro de vídeos.

1. Inicie sessão no seu [vídeo indexador](https://api-portal.videoindexer.ai/) conta.
2. Procurar entre todos os vídeos na sua conta.

    No exemplo seguinte, é procurado para todos os vídeos criados pelo Channel9 com autoria de Scott Hanselman.

    ![Pesquisa](./media/video-indexer-search/video-indexer-search01.png)
    
3. Procure o insights resumido do vídeo.

    Pode procurar dentro de um vídeo, clicando em **reproduzir** no vídeo. Em seguida, pode pesquisar dentro o vídeo selecionando o **pesquisa** separador. Por exemplo, vamos pesquisadas para todos os locais onde o texto "identity protection" é utilizado. 

    ![Pesquisa](./media/video-indexer-search/video-indexer-search02.png)

    Se clicar em um dos resultados, o leitor proporciona nesse momento as vídeo. Pode obter a vista de leitor/insights e sincronização na sua aplicação. Para obter mais informações, consulte [widgets do indexador de incorporação de vídeo na sua aplicação](video-indexer-embed-widgets.md). 

    
4. Procure detalhada discrimina a vídeo.

    Se quiser criar os seus próprios repartição com base no vídeo localizar, prima a **editar** botão. Esta página mostra a repartição completa de um vídeo. Pode pesquisar no repartição para mostrar apenas as linhas que está interessado. Para obter mais informações, consulte [ver e editar as informações de vídeo indexador](video-indexer-view-edit.md).

    Neste exemplo, iremos procurar o texto "identity protection". Também iremos aplicar filtros adicionais, conforme mostrado no ecrã abaixo.

    ![Pesquisa](./media/video-indexer-search/video-indexer-search03.png)

## <a name="next-steps"></a>Passos Seguintes 

Depois de localizar o vídeo que pretende trabalhar, pode continuar a processar o vídeo, conforme descrito em um dos seguintes tópicos: 

- [Criar novo insights vídeos com base no vídeo existente](video-indexer-create-new.md)
- [Conteúdo processos com a API de REST do indexador de vídeo](video-indexer-use-apis.md)
- [Incorporar visual widgets na sua aplicação](video-indexer-embed-widgets.md)

## <a name="see-also"></a>Consulte também

[Descrição geral do vídeo indexador](video-indexer-overview.md)
