---
title: Adicionar modelos padrão em aplicativos de LUIS | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como adicionar modelos padrão em aplicativos de compreensão de idiomas (LUIS) para melhorar a exatidão da previsão.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: diberry;
ms.openlocfilehash: bf1931355fd873eaeac6c313b70717dfa99814c6
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39222601"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>Como adicionar os padrões para melhorar a exatidão da previsão
Depois de uma aplicação do LUIS recebe expressões de ponto de extremidade, utilize o [conceito](luis-concept-patterns.md) de padrões para melhorar a exatidão da previsão para expressões que revelam um padrão na ordem das palavras e a escolha do word. Padrões de utilização [entidades](luis-concept-entity-types.md) e as respetivas funções para extrair dados usando a sintaxe padrão específico. 

## <a name="add-template-utterance-to-create-pattern"></a>Adicionar a expressão de modelo para criar padrão
1. Abra a sua aplicação, selecionando o respetivo nome na **as minhas aplicações** página e, em seguida, selecione **padrões** no painel esquerdo, em **melhorar o desempenho da aplicação**.

    ![Captura de ecrã da lista de padrões](./media/luis-how-to-model-intent-pattern/patterns-1.png)

2. Selecione a intenção correta para o padrão. 

    ![Selecione a intenção](./media/luis-how-to-model-intent-pattern/patterns-2.png)

3. Na caixa de texto modelo, escreva a expressão de modelo e selecione Enter. Quando pretender introduzir o nome da entidade, utilize a sintaxe de entidade padrão correto. Começar a sintaxe de entidade com `{`. A lista de entidades apresenta. Selecione a entidade correta e, em seguida, selecione Enter. 

    ![Captura de ecrã da entidade para padrão](./media/luis-how-to-model-intent-pattern/patterns-3.png)

    Se a sua entidade inclui uma função, indicar a função com dois pontos único `:`, após o nome de entidade, como `{Location:Origin}`. Apresenta a lista de funções para as entidades numa lista. Selecione a função e, em seguida, selecione Enter. 

    ![Captura de ecrã da entidade com a função](./media/luis-how-to-model-intent-pattern/patterns-4.png)

    Depois de selecionar a entidade correta, termina de introduzir o padrão e, em seguida, selecione Enter. Quando tiver terminado de padrões de digitação [treinar](luis-how-to-train.md) seu aplicativo.

    ![Captura de ecrã do padrão introduzido com ambos os tipos de entidades](./media/luis-how-to-model-intent-pattern/patterns-5.png)

## <a name="search-patterns"></a>Padrões de pesquisa
Pesquisar permite que encontre padrões que contêm um determinado texto.  

1. Selecione o ícone de lupa.

    ![Página de captura de ecrã de padrões com ícone de ferramenta de pesquisa realçado](./media/luis-how-to-model-intent-pattern/search-icon.png)

    Escreva o texto de pesquisa na caixa de pesquisa no canto superior direito da lista de padrões e selecione Enter. A lista de padrões é atualizada para apresentar apenas os padrões incluindo o texto a procurar.

    ![Página de captura de ecrã de padrões com texto de pesquisa na caixa de pesquisa realçado](./media/luis-how-to-model-intent-pattern/search-text.png)

    Para cancelar a pesquisa e restaurar a lista completa dos padrões, elimine o texto de pesquisa que introduziu.

<!-- TBD: should I be able to click on the magnifying glass again to close the search box? It doesn't reset the list. -->

## <a name="edit-a-pattern"></a>Editar um padrão
1. Para editar um padrão, selecione as reticências (***...*** ) botão na extremidade direita da linha para esse padrão, em seguida, selecione **editar**. 

    ![Captura de ecrã de Editar item de menu na linha de padrão](./media/luis-how-to-model-intent-pattern/patterns-three-dots.png) 

2. Introduza todas as alterações na caixa de texto. Quando tiver terminado, selecione introduzir. Quando tiver terminado de padrões de edição [treinar](luis-how-to-train.md) seu aplicativo.

    ![Captura de ecrã de edição padrão](./media/luis-how-to-model-intent-pattern/edit-pattern.png)

## <a name="reassign-individual-pattern-to-different-intent"></a>Reatribuir padrão individual em intenção diferente

Para reatribuir um padrão único para um objetivo diferente, selecione a caixa de listagem intenção à direita do texto padrão e selecione um objetivo diferente.

![Captura de ecrã da reatribuição padrão individual em intenção diferente](./media/luis-how-to-model-intent-pattern/reassign-individual-pattern.png)

## <a name="reassign-several-patterns-to-different-intent"></a>Reatribuir vários padrões em intenção diferente

Para reatribuir a vários padrões para um objetivo diferente, selecione a caixa de verificação à esquerda de cada padrão ou selecione a caixa de verificação principal. O **reatribuir intenção** opção apresenta na barra de ferramentas. Selecione a intenção correta para os padrões. 

![Captura de ecrã da reatribuição vários padrões em intenção diferente](./media/luis-how-to-model-intent-pattern/reassign-many-patterns.png)

## <a name="delete-a-single-pattern"></a>Eliminar um padrão único

1. Para eliminar um padrão, selecione as reticências (***...*** ) botão na extremidade direita da linha para esse padrão, em seguida, selecione **eliminar**. 

    ![Captura de ecrã de eliminar expressão](./media/luis-how-to-model-intent-pattern/patterns-three-dots-ddl.png)

2. Selecione **Ok** para confirmar a eliminação.

    ![Captura de ecrã de eliminar confirmação](./media/luis-how-to-model-intent-pattern/confirm-delete.png)

## <a name="delete-several-patterns"></a>Eliminar vários padrões

1. Para eliminar vários padrões, selecione a caixa de verificação à esquerda de cada padrão ou selecione a caixa de verificação principal. O **eliminar o (s) de padrões** opção apresenta na barra de ferramentas. Selecione **eliminar o (s) de padrões**.  

    ![Captura de ecrã da eliminação de vários padrões](./media/luis-how-to-model-intent-pattern/delete-many-patterns.png)

2. O **eliminar padrões** é apresentada a caixa de diálogo de confirmação. Selecione **Ok** para concluir a eliminação.

    ![Captura de ecrã da eliminação de vários padrões](./media/luis-how-to-model-intent-pattern/delete-many-patterns-confirmation.png)

## <a name="filter-pattern-list-by-entity"></a>Filtrar a lista padrão por entidade

Para filtrar a lista de padrões por uma entidade específica, selecione **filtros de entidade** na barra de ferramentas acima dos padrões. 

![Captura de ecrã da filtragem de padrões por entidade](./media/luis-how-to-model-intent-pattern/filter-entities-1.png)

Depois do filtro é aplicado, o nome da entidade é apresentada abaixo da barra de ferramenta. 

## <a name="filter-pattern-list-by-intent"></a>Filtrar a lista padrão por intenção

Para filtrar a lista de padrões por um objetivo específico, selecione **filtros de intenção** na barra de ferramentas acima dos padrões. 

![Captura de ecrã da filtragem de padrões por intenção](./media/luis-how-to-model-intent-pattern/filter-intents-1.png)

Depois do filtro é aplicado, o nome de intenção é apresentada abaixo da barra de ferramenta. 

## <a name="remove-entity-or-intent-filter"></a>Remover entidade ou um filtro de intenção
Quando a lista de padrão é filtrada, a entidade ou o nome de intenção é apresentada abaixo da barra de ferramentas. Para remover o filtro, selecione o nome.

![Captura de ecrã dos padrões filtrados por entidade](./media/luis-how-to-model-intent-pattern/filter-entities-2.png)

O filtro é removido e apresentam todos os padrões. 

## <a name="add-pattern-from-existing-utterance-on-intent-or-entity-page"></a>Adicionar o padrão de expressão existente na página de intenção ou a entidade
Pode criar um padrão a partir de uma expressão existente em qualquer uma de **intenção** ou **entidade** página. Todas as expressões em qualquer página intenção ou a entidade são exibidos numa lista com a coluna da direita fornecer acesso às opções de nível de expressão, como **edite**, **eliminar**, e **adicionar como padrão**.

1. Na linha da expressão selecionada, selecione as reticências (***...*** ) à direita da expressão e selecione **adicionar como padrão**.

    [![](./media/luis-how-to-model-intent-pattern/add-pattern-from-utterance.png "Captura de ecrã da tabela de expressões com padrão de adicionar realçada no menu de opções")](./media/luis-how-to-model-intent-pattern/add-pattern-from-utterance.png)

2. Modificar o padrão de acordo com o [regras de sintaxe](luis-concept-patterns.md#pattern-syntax). Se a expressão que selecionou está etiquetado com entidades, essas entidades já estão no padrão com a sintaxe correta.

    ![Captura de ecrã dos padrões filtrados por entidade](./media/luis-how-to-model-intent-pattern/confirm-patterns-modal.png)

## <a name="train-your-app-after-changing-model-with-patterns"></a>Preparar a sua aplicação depois de alterar o modelo com padrões
Depois de adicionar, editar, remover ou reatribuir um padrão [treinar](luis-how-to-train.md) e [publicar](luis-how-to-publish-app.md) as suas alterações afetar a consultas de ponto final na sua aplicação. 

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [criar um padrão](luis-tutorial-pattern.md) com um pattern.any e funções.
* Saiba como [treinar](luis-how-to-train.md) seu aplicativo.