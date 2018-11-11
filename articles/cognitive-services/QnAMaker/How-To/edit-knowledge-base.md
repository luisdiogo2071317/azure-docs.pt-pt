---
title: Editar uma base de dados de conhecimento - QnA Maker
titleSuffix: Azure Cognitive Services
description: A ferramenta QnA Maker permite-lhe gerir o conteúdo da sua base de dados de conhecimento, fornecendo uma experiência de edição fácil de usar.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 11/06/2018
ms.author: tulasim
ms.openlocfilehash: adcefe8fed927aca2533ea811bac56f0b92288de
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51279766"
---
# <a name="edit-a-knowledge-base"></a>Editar uma base de dados de conhecimento

A ferramenta QnA Maker permite-lhe gerir o conteúdo da sua base de dados de conhecimento, fornecendo uma experiência de edição fácil de usar.

## <a name="edit-your-knowledge-base-content"></a>Editar o conteúdo de base de dados de conhecimento

1.  Selecione **meu bases de dados de conhecimento** na barra de navegação superior. 

    Pode ver todos os serviços criados ou partilhados consigo classificadas na ordem decrescente do **modificado pela última vez** data.

    ![Meu Bases de dados de conhecimento](../media/qnamaker-how-to-edit-kb/my-kbs.png)

2. Selecione uma base de dados de conhecimento específica para fazer edições ao mesmo.
 
3. Clique em **Definições**.

   Aqui pode editar o nome do serviço de campo obrigatório.
  
   Pode adicionar novos Urls para adicionar o novo conteúdo de perguntas frequentes sobre a base de conhecimento ao clicar em **Gerir base de dados de conhecimento da -> "+ Adicionar URL'** ligação.
   
   Pode eliminar URLs existentes ao clicar em **ícone Eliminar**.
   
   Se pretender que a base de conhecimento para pesquisar o conteúdo mais recente de URLs existentes, o nome de caixa de verificação de escala **'Atualizar'**, esta ação irá atualizar a base de conhecimento com conteúdo mais recente do URL.
   
Pode adicionar o documento de ficheiro suportados para fazer parte da base de conhecimento, ao clicar em **Gerir base de dados de conhecimento da -> "+ adicionar o ficheiro"**

Também pode importar qualquer base de conhecimento existente clicando **base de Ímport conhecimento** botão. 
   
Depende da atualização da base de conhecimento **escalão de preço de gestão** a ser utilizado ao criar o serviço QnA Maker associado ao seu knowledgbase. Também pode atualizar o escalão de gestão a partir do portal do Azure, se necessário.

4. Quando tiver terminado de efetuar alterações para a base de dados de conhecimento, clique em **guardar e treinar** no canto superior direito da página para manter as alterações.    

    ![Guardar e preparar](../media/qnamaker-how-to-edit-kb/save-and-train.png)

    >[!NOTE]
    Deixando a página antes de clicar em Guardar e train não aplicará as alterações.

## <a name="add-a-qna-pair"></a>Adicione um par de FAQ

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

## <a name="delete-knowledge-bases"></a>Eliminar bases de dados de conhecimento

A eliminar uma base de dados de conhecimento (KB) é uma operação permanente. Não pode ser anulada. Antes de eliminar uma base de dados de conhecimento, deve exportar a base de dados de conhecimento a partir da **definições** página do portal do QnA Maker. 

Se partilhar a sua BDC com [colaboradores](collaborate-knowledge-base.md) , em seguida, eliminá-lo, todos os utilizadores perde o acesso para o KB. 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Colaborar numa base de dados de conhecimento](./collaborate-knowledge-base.md)
