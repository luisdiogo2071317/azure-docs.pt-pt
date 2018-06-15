---
title: Adicionar modelos de padrão em aplicações LUIS | Microsoft Docs
titleSuffix: Azure
description: Saiba como adicionar modelos de padrão em aplicações de compreensão de idiomas (LUIS) para melhorar a exatidão da previsão.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: 68c0ea1fd3f2e60e0adec631f33c8bd09a3d9960
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35356391"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>Como adicionar padrões para melhorar a exatidão da previsão
Depois de uma aplicação LUIS recebe utterances de ponto final, utilize o [conceito](luis-concept-patterns.md) de padrões para melhorar a exatidão da previsão para utterances revelar um padrão por ordem de word e escolha do word. Padrões de utilização [entidades](luis-concept-entity-types.md) e as respetivas funções para extrair dados utilizando a sintaxe de padrão específico. 

## <a name="add-template-utterance-to-create-pattern"></a>Adicionar utterance de modelo para criar padrão
1. Abra a aplicação, selecionando o respetivo nome nos **aplicações My** página e, em seguida, selecione **padrões** no painel esquerdo, em **melhorar o desempenho da aplicação**.

    ![Captura de ecrã da lista de padrões](./media/luis-how-to-model-intent-pattern/patterns-1.png)

2. Selecione o objetivo correto para o padrão. 

    ![Selecione o objetivo](./media/luis-how-to-model-intent-pattern/patterns-2.png)

3. Na caixa de texto de modelo, introduza o utterance modelo e selecione Enter. Quando pretende introduzir o nome da entidade, utilize a sintaxe de entidade padrão correto. Começar a sintaxe de entidade com `{`. A lista de entidades apresenta. Selecione a entidade correta e, em seguida, selecione Enter. 

    ![Captura de ecrã da entidade para padrão](./media/luis-how-to-model-intent-pattern/patterns-3.png)

    Se a entidade inclui uma função, indicar a função com o único ponto e vírgula, `:`, depois da entidade de nome, tal como `{Location:Origin}`. Mostra a lista de funções para as entidades numa lista. Selecione a função e, em seguida, selecione Enter. 

    ![Captura de ecrã da entidade com a função](./media/luis-how-to-model-intent-pattern/patterns-4.png)

    Depois de selecionar a entidade correta, concluir introduzir o padrão e, em seguida, selecione Enter. Quando tiver terminado de padrões de entrar, [preparar](luis-how-to-train.md) a aplicação.

    ![Captura de ecrã do padrão introduzido com os dois tipos de entidades](./media/luis-how-to-model-intent-pattern/patterns-5.png)

## <a name="search-patterns"></a>Padrões de pesquisa
Pesquisar permite-lhe localizar padrões que contém algum texto especificado.  

1. Selecione o ícone da Lupa.

    ![Página de captura de ecrã de padrões com ícone de ferramenta pesquisa realçado](./media/luis-how-to-model-intent-pattern/search-icon.png)

    Escreva o texto de pesquisa na caixa de pesquisa no canto superior direito da lista de padrões e selecione Enter. A lista de padrões é atualizada para apresentar apenas os padrões, incluindo o texto a procurar.

    ![Página de captura de ecrã de padrões com texto de pesquisa na caixa de pesquisa realçado](./media/luis-how-to-model-intent-pattern/search-text.png)

    Para cancelar a pesquisa e restaurar a lista completa de padrões, elimine o texto de pesquisa que introduziu.

<!-- TBD: should I be able to click on the magnifying glass again to close the search box? It doesn't reset the list. -->

## <a name="edit-a-pattern"></a>Editar um padrão
1. Para editar um padrão, selecione o ícone de reticências (…) na extremidade direita da linha para esse padrão, em seguida, selecione **editar**. 

    ![Captura de ecrã de Editar item de menu na linha de padrão](./media/luis-how-to-model-intent-pattern/patterns-three-dots.png) 

2. Introduza as alterações na caixa de texto. Quando tiver terminado, introduza o selecionar. Quando tiver terminado de padrões de edição, [preparar](luis-how-to-train.md) a aplicação.

    ![Captura de ecrã da edição padrão](./media/luis-how-to-model-intent-pattern/edit-pattern.png)

## <a name="reassign-individual-pattern-to-different-intent"></a>Reatribuir padrão individuais a intenção diferentes

Para reatribuir um padrão único para um objetivo diferente, selecione a caixa de listagem intenção à direita do texto padrão e selecionar um objetivo diferente.

![Captura de ecrã da reatribuição padrão individuais a intenção diferentes](./media/luis-how-to-model-intent-pattern/reassign-individual-pattern.png)

## <a name="reassign-several-patterns-to-different-intent"></a>Reatribuir várias padrões para objetivo diferente

Para reatribuir padrões vários para um objetivo diferente, selecione a caixa de verificação à esquerda de cada padrão ou selecione a caixa de verificação superior. O **reatribuir intenção** opção apresenta na barra de ferramentas. Selecione o objetivo correto para os padrões. 

![Captura de ecrã da reatribuição várias padrões para objetivo diferente](./media/luis-how-to-model-intent-pattern/reassign-many-patterns.png)

## <a name="delete-a-single-pattern"></a>Eliminar um único padrão

1. Para eliminar um padrão, selecione o ícone de reticências (…) na extremidade direita da linha para esse padrão, em seguida, selecione **eliminar**. 

    ![Captura de ecrã eliminar utterance](./media/luis-how-to-model-intent-pattern/patterns-three-dots-ddl.png)

2. Selecione **Ok** para confirmar a eliminação.

    ![Captura de ecrã eliminar confirmação](./media/luis-how-to-model-intent-pattern/confirm-delete.png)

## <a name="delete-several-patterns"></a>Eliminar vários padrões

1. Para eliminar vários padrões, selecione a caixa de verificação à esquerda de cada padrão ou selecione a caixa de verificação superior. O **eliminar padrões (s)** opção apresenta na barra de ferramentas. Selecione **eliminar padrões (s)**.  

    ![Captura de ecrã da eliminação de vários padrões](./media/luis-how-to-model-intent-pattern/delete-many-patterns.png)

2. O **eliminar padrões** é apresentada a caixa de diálogo de confirmação. Selecione **Ok** para concluir a eliminação.

    ![Captura de ecrã da eliminação de vários padrões](./media/luis-how-to-model-intent-pattern/delete-many-patterns-confirmation.png)

## <a name="filter-pattern-list-by-entity"></a>Filtrar a lista de padrão pela entidade

Para filtrar a lista de padrões por uma entidade específica, selecione **filtros de entidade** na barra de ferramentas acima os padrões. 

![Captura de ecrã da filtragem de padrões pela entidade](./media/luis-how-to-model-intent-pattern/filter-entities-1.png)

Depois do filtro é aplicado, o nome da entidade é apresentada abaixo da barra de ferramentas. 

## <a name="filter-pattern-list-by-intent"></a>Filtrar a lista de padrão por intenção

Para filtrar a lista de padrões por um objetivo específico, selecione **intenção filtra** na barra de ferramentas acima os padrões. 

![Captura de ecrã da filtragem padrões por intenção](./media/luis-how-to-model-intent-pattern/filter-intents-1.png)

Depois do filtro é aplicado, o nome de intenção aparece abaixo da barra de ferramentas. 

## <a name="remove-entity-or-intent-filter"></a>Remover filtro intenção ou de entidade
Quando a lista de padrão é filtrada, a entidade ou o nome da intenção é apresentada abaixo da barra de ferramentas. Para remover o filtro, selecione o nome.

![Captura de ecrã de padrões filtrados por entidade](./media/luis-how-to-model-intent-pattern/filter-entities-2.png)

O filtro é removido e padrões de todas as apresentam. 

## <a name="add-pattern-from-existing-utterance-on-intent-or-entity-page"></a>Adicionar o padrão de utterance existente na página de intenção ou entidade
Pode criar um padrão a partir de um utterance existente no **intenção** ou **entidade** página. Todos os utterances em qualquer página intenção ou uma entidade que são apresentadas numa lista com a coluna da direita fornecer acesso às opções de nível de utterance, tal como **editar**, **eliminar**, e **adicionar como padrão**.

1. Na linha do utterance selecionada, selecione as reticências (…) à direita do utterance e selecione **adicionar como padrão**.

    [![](./media/luis-how-to-model-intent-pattern/add-pattern-from-utterance.png "Captura de ecrã da tabela de utterances com o padrão de adicionar realçado no menu de opções")](./media/luis-how-to-model-intent-pattern/add-pattern-from-utterance.png)

2. Modificar o padrão de acordo com o [regras sintaxe](luis-concept-patterns.md#pattern-syntax). Se o utterance selecionou assinalada como com entidades, esses entidades já estão num padrão com a sintaxe correta.

    ![Captura de ecrã de padrões filtrados por entidade](./media/luis-how-to-model-intent-pattern/confirm-patterns-modal.png)

## <a name="train-your-app-after-changing-model-with-patterns"></a>Preparar a sua aplicação depois de alterar o modelo com padrões
Depois de adicionar, editar, remover ou reatribuir um padrão, [preparar](luis-how-to-train.md) e [publicar](PublishApp.md) a aplicação para as suas alterações afetar a consultas de ponto final. 

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [compilar um padrão](luis-tutorial-pattern.md) com um pattern.any e as funções.
* Saiba como [preparar](luis-how-to-train.md) a aplicação.