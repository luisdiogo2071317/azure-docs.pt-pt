---
title: Como editar uma base de dados de conhecimento - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Como editar uma base de dados de conhecimento
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: eaa65bf3d257399fceadaa42f0d9ddbbf8afe234
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354092"
---
# <a name="edit-a-knowledge-base"></a>Editar uma base de dados de conhecimento

Maker de QnA permite-lhe gerir o conteúdo da sua base de dados de conhecimento ao fornecer uma experiência de edição de fácil utilização.

## <a name="edit-your-knowledge-base-content"></a>Editar o conteúdo da base de dados de conhecimento

1.  Selecione **meu bases de dados de conhecimento** na barra de navegação superior. 

    Pode ver todos os serviços criado ou partilhados consigo ordenada pela ordem descendente do **modificado pela última vez** data.

    ![A minha Bases de dados de conhecimento](../media/qnamaker-how-to-edit-kb/my-kbs.png)

2. Selecione uma base de dados de conhecimento específica para fazer edições.

3. Depois de terminar de efetuar alterações para a base de dados de conhecimento, clique em **guardar e formação** no canto superior direito da página para manter as alterações.    

    ![Guarde e formação](../media/qnamaker-how-to-edit-kb/save-and-train.png)

    >[!NOTE]
    Abandonar o fileparser a página antes de clicar em Guardar e formação não irão manter as alterações.

## <a name="add-a-qna-pair"></a>Adicione um par de QnA

Selecione **par de QnA adicionar** para adicionar uma nova linha à tabela de base de dados de conhecimento.

![Adicione o par de QnA](../media/qnamaker-how-to-edit-kb/add-qnapair.png)

## <a name="delete-a-qna-pair"></a>Eliminar um par de QnA

Para eliminar um de QnA, clique em de **eliminar** ícone na extremidade direita da linha de QnA.

![Eliminar o par de QnA](../media/qnamaker-how-to-edit-kb/delete-qnapair.png)

## <a name="add-alternate-questions"></a>Adicionar perguntas alternativas

Adicione perguntas alternativas a um par de QnA existente para melhorar a probabilidade de uma correspondência para uma consulta do utilizador.

![Adicionar perguntas alternativas](../media/qnamaker-how-to-edit-kb/add-alternate-question.png)

## <a name="add-metadata"></a>Adicionar metadados


Adicionar os pares de metadados, selecionando o ícone de filtro

![Adicionar metadados](../media/qnamaker-how-to-edit-kb/add-metadata.png)

> [!TIP]
> Certifique-se guardar periodicamente e preparar a base de dados de conhecimento depois de efetuar edições para evitar a perda de alterações.

## <a name="manage-large-knowledge-bases"></a>Gerir grandes bases de dados de conhecimento

1. Os QnAs são **agrupados** pela origem de dados a partir da qual foram extraídos. Pode expandir ou fechar a origem de dados.
2. Pode **pesquisa** a base de dados de conhecimento, escrevendo na caixa de texto na parte superior da tabela de Base de dados de conhecimento. Introduza clique em Procurar o conteúdo de pergunta, resposta ou metadados. Clique no ícone X para remover o filtro de pesquisa.
3. **Paginação** permite-lhe gerir grandes bases de dados de conhecimento

    ![Procurar, grupo, de paginação](../media/qnamaker-how-to-edit-kb/search-paginate-group.png)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Colaborar numa base de dados de conhecimento](./collaborate-knowledge-base.md)
