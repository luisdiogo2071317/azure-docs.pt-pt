---
title: Base de dados de conhecimento de que não o inglês - QnA Maker
titleSuffix: Azure Cognitive Services
description: A ferramenta QnA Maker suporta conteúdos da base de dados de conhecimento em vários idiomas. No entanto, cada serviço QnA Maker deve ser reservado para um único idioma. A primeira base de dados de conhecimento criado visando um serviço específico do QnA Maker define o idioma desse serviço.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: b983fb21e8e67a422b6757619d1d0dfe8b6b9dcc
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033911"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>Suporte de idiomas de conteúdo de base de dados para o QnA Maker
A ferramenta QnA Maker suporta conteúdos da base de dados de conhecimento em vários idiomas. No entanto, cada serviço QnA Maker deve ser reservado para um único idioma. A primeira base de dados de conhecimento criado visando um serviço específico do QnA Maker define o idioma desse serviço. Ver [aqui](../Overview/languages-supported.md) para a lista de idiomas suportados.

O idioma é automaticamente reconhecido do conteúdo das origens de dados que está a ser extraído. Depois de criar um novo serviço do QnA Maker e uma nova Base de dados de conhecimento em que o serviço, pode verificar que o idioma foi definido corretamente.

1. Navegue para o [Portal do Azure](https://portal.azure.com/).

2. Selecione **grupos de recursos** e navegue para o grupo de recursos em que o serviço QnA Maker é implementado e selecione o **Azure Search** recursos.

    ![Selecione o recurso de pesquisa do Azure](../media/qnamaker-how-to-language-kb/select-azsearch.png)

3. Selecione o **testkb** índice. Este índice da Azure Search é sempre o primeiro criada e contém o conteúdo salvo de todas as bases de dados de conhecimento desse serviço. 

    ![Selecione o teste KB](../media/qnamaker-how-to-language-kb/select-testkb.png)

4. Selecione **campos** secção que mostra os detalhes de testkb.

    ![Selecionar campos](../media/qnamaker-how-to-language-kb/selectfields.png)

5. Marque a caixa **Analyzer** para ver detalhes do idioma.

    ![Selecione Analyzer](../media/qnamaker-how-to-language-kb/select-analyzer.png)

6. Deve notar que o analisador está definido para um determinado idioma. Essa linguagem foi detetada automaticamente durante o passo de criação de base de dados de conhecimento. Essa linguagem não pode ser alterada depois do recurso é criado.

    ![Analisador de selecionado](../media/qnamaker-how-to-language-kb/selected-analyzer.png)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Criar um bot QnA com o Azure Bot Service](../Tutorials/create-qna-bot.md)
