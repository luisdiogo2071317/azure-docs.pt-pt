---
title: Editar uma base de dados de conhecimento - QnA Maker
titleSuffix: Azure Cognitive Services
description: A ferramenta QnA Maker permite-lhe gerir o conteúdo da sua base de dados de conhecimento, fornecendo uma experiência de edição fácil de usar.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: saneppal
ms.openlocfilehash: 919a7dbcb3ea898182e19e4cd0de793e7a80dea9
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45543297"
---
# <a name="edit-a-knowledge-base"></a>Editar uma base de dados de conhecimento

A ferramenta QnA Maker permite-lhe gerir o conteúdo da sua base de dados de conhecimento, fornecendo uma experiência de edição fácil de usar.

## <a name="edit-your-knowledge-base-content"></a>Editar o conteúdo de base de dados de conhecimento

1.  Selecione **meu bases de dados de conhecimento** na barra de navegação superior. 

    Pode ver todos os serviços criados ou partilhados consigo classificadas na ordem decrescente do **modificado pela última vez** data.

    ![Meu Bases de dados de conhecimento](../media/qnamaker-how-to-edit-kb/my-kbs.png)

2. Selecione uma base de dados de conhecimento específica para fazer edições ao mesmo.

3. Quando tiver terminado de efetuar alterações para a base de dados de conhecimento, clique em **guardar e treinar** no canto superior direito da página para manter as alterações.    

    ![Guardar e preparar](../media/qnamaker-how-to-edit-kb/save-and-train.png)

    >[!NOTE]
    Deixando a página antes de clicar em Guardar e train não aplicará as alterações.

## <a name="add-a-qna-pair"></a>Adicione um par de QnA

Selecione **QnA adicionar par** para adicionar uma nova linha à tabela de base de dados de conhecimento.

![Adicionar par QnA](../media/qnamaker-how-to-edit-kb/add-qnapair.png)

## <a name="delete-a-qna-pair"></a>Eliminar um par de QnA

Para eliminar um QnA, clique nas **eliminar** ícone mais à direita da linha QnA.

![Eliminar QnA par](../media/qnamaker-how-to-edit-kb/delete-qnapair.png)

## <a name="add-alternate-questions"></a>Adicionar perguntas alternativas

Adicione perguntas alternativas para um par de QnA existente para aumentar a probabilidade de uma correspondência com uma consulta de utilizador.

![Adicionar perguntas alternativas](../media/qnamaker-how-to-edit-kb/add-alternate-question.png)

## <a name="add-metadata"></a>adicionar metadados


Adicionar os pares de metadados ao selecionar o ícone de filtro

![Adicionar metadados](../media/qnamaker-how-to-edit-kb/add-metadata.png)

> [!TIP]
> Não se esqueça de salvar periodicamente e preparar a base de dados de conhecimento depois de fazer edições para evitar perder as alterações.

## <a name="manage-large-knowledge-bases"></a>Gerenciar grandes bases de dados de conhecimento

1. São os QnAs **agrupados** pela origem de dados a partir do qual foram extraídos. Pode expandir ou fechar a origem de dados.
2. Pode **pesquisa** a base de dados de conhecimento ao escrever na caixa de texto na parte superior da tabela de Base de dados de conhecimento. Clique em Inserir para pesquisar o conteúdo de pergunta, resposta ou metadados. Clique no ícone de X para remover o filtro de pesquisa.
3. **Paginação** permite-lhe gerir grandes bases de dados de conhecimento

    ![Procurar, paginar, grupo](../media/qnamaker-how-to-edit-kb/search-paginate-group.png)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Colaborar numa base de dados de conhecimento](./collaborate-knowledge-base.md)
