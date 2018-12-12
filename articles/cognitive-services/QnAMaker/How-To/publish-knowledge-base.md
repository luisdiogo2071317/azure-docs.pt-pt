---
title: Publicar uma base de dados de conhecimento - QnA Maker
titleSuffix: Azure Cognitive Services
description: A publicação de sua base de dados de conhecimento é o último passo na criação de sua base de dados de conhecimento disponível como um ponto final de responder a pergunta. Quando publica uma base de dados de conhecimento, move o conteúdo de QnA da sua base de dados de conhecimento do índice de teste para um índice de produção no Azure search.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 9c3d1009742f9ca0df6dc2f2d1a2d1904397275a
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53087853"
---
# <a name="publish-a-knowledge-base-in-qna-maker"></a>Publicar uma base de dados de conhecimento do QnA Maker

A publicação de sua base de dados de conhecimento é o último passo na criação de sua base de dados de conhecimento disponível como um ponto final de responder a pergunta. 

Quando publica uma base de dados de conhecimento, move o conteúdo de QnA da sua base de dados de conhecimento do índice de teste para um índice de produção no Azure search.

![Publicar o índice de teste de produção](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

## <a name="publish-a-knowledge-base"></a>Publicar uma base de dados de conhecimento

1. Quando tiver terminado com as alterações no seu artigo BDC, selecione **publicar** na barra de navegação superior. Pode publicar até ao número atribuído de bases de dados de conhecimento para o Azure Search. 

    ![Publicar base de dados de conhecimento](../media/qnamaker-how-to-publish-kb/publish.png)

2. Selecione **publicar** novamente para ver os detalhes de ponto de extremidade que podem ser usados em seu código de aplicação ou bot.

    ![Base de dados de conhecimento foi publicado com êxito](../media/qnamaker-how-to-publish-kb/publish-success.png)
    
## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Obter análises na sua base de dados de conhecimento](./get-analytics-knowledge-base.md)
