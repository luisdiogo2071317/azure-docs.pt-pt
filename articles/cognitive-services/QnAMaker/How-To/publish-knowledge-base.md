---
title: Como publicar uma base de dados de conhecimento - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Como publicar uma base de dados de conhecimento
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: e9dbeacfb0df98c6b8f084c263690c05fe966cdc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353954"
---
# <a name="publish-a-knowledge-base"></a>Publicar uma base de dados de conhecimento

Publicar a sua base de dados de conhecimento é o último passo tomar a sua base de dados de conhecimento disponível como um ponto final de responder a pergunta. 

Quando publica uma base de dados de conhecimento, o conteúdo de QnA da sua base de dados de conhecimento move desde o índice de teste para um índice de produção na pesquisa do Azure.

![Publicar o índice de teste de prod](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

## <a name="publish-a-knowledge-base"></a>Publicar uma base de dados de conhecimento

1. Quando tiver terminado com as alterações no seu artigo KB, selecione **publicar** na barra de navegação superior. Pode publicar até ao número de bases de dados de conhecimento atribuído para a pesquisa do Azure. 

    ![Publicar a base de dados de conhecimento](../media/qnamaker-how-to-publish-kb/publish.png)

2. Selecione **publicar** novamente para ver os detalhes do ponto final que podem ser utilizados no código da aplicação ou bot.

    ![Publicar a base de dados de conhecimento](../media/qnamaker-how-to-publish-kb/publish-success.png)
    
## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Obter a análise na sua base de dados de conhecimento](./get-analytics-knowledge-base.md)
