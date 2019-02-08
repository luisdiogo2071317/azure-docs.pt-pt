---
title: Publicar uma base de dados de conhecimento
titleSuffix: QnA Maker - Azure Cognitive Services
description: A publicação de sua base de dados de conhecimento é o último passo na criação de sua base de dados de conhecimento disponível como um ponto final de responder a pergunta. Quando publica uma base de dados de conhecimento, move o conteúdo de QnA da sua base de dados de conhecimento do índice de teste para um índice de produção no Azure search.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 12/11/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 585e5f8f955a405b08bef42588ab5caff71c7bba
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55865918"
---
# <a name="publish-a-knowledge-base-using-the-qna-maker-portal"></a>Publicar uma base de dados de conhecimento através do portal do QnA Maker

A publicação de sua base de dados de conhecimento é o último passo na criação de sua base de dados de conhecimento disponível como um ponto final de responder a pergunta a uma aplicação cliente. 

Quando publica uma base de dados de conhecimento, move o conteúdo de perguntas e respostas da sua base de dados de conhecimento do índice de teste para um índice de produção no Azure search.

![Publicar o índice de teste de produção](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar. 

## <a name="publish-a-knowledge-base"></a>Publicar uma base de dados de conhecimento

1. Quando tiver terminado com as alterações no seu artigo BDC, selecione **publicar** na barra de navegação superior. Pode publicar até ao número atribuído de bases de dados de conhecimento para o Azure Search. 

    ![Publicar base de dados de conhecimento](../media/qnamaker-how-to-publish-kb/publish.png)

2. Selecione **publicar** novamente para ver os detalhes de ponto de extremidade que podem ser usados em seu código de aplicação ou bot.

    ![Base de dados de conhecimento foi publicado com êxito](../media/qnamaker-how-to-publish-kb/publish-success.png)
    
## <a name="clean-up-resources"></a>Limpar recursos

Quando tiver terminado com a base de dados de conhecimento, removê-lo no portal do QnA Maker.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Obter análises na sua base de dados de conhecimento](./get-analytics-knowledge-base.md)
