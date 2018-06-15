---
title: Adicionar pendentes em aplicações LUIS | Microsoft Docs
description: Utilize a compreensão de idiomas (LUIS) para adicionar pendentes para ajudar a compreender os pedidos de utilizador e reagir aos mesmos corretamente de aplicações.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.service: cognitive-services
ms.openlocfilehash: 6e013e994a3bcb60c3104aa10cd7bad1535706f1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355765"
---
# <a name="manage-intents"></a>Gerir pendentes 
Adicionar [pendentes](luis-concept-intent.md) à sua aplicação LUIS para identificar os grupos de questões ou os comandos que tenham as intenções do mesmas. 

Adicionar e gerir a sua pendentes do **pendentes** página, disponível a partir do **pendentes** no painel esquerdo de LUIS. 

O procedimento seguinte demonstra como adicionar a intenção de "Bookflight" na aplicação TravelAgent.

## <a name="add-intent"></a>Adicionar intenção

1. Abra a aplicação (por exemplo, TravelAgent) ao clicar em seu nome no **aplicações My** página e, em seguida, clique em **pendentes** no painel esquerdo. 
2. No **pendentes** página, clique em **criar novo objetivo**.

    ![Lista de pendentes](./media/luis-how-to-add-intents/IntentsList.png)
3. No **criar novo objetivo** caixa de diálogo, escreva a intenção nome "BookFlight" e clique em **feito**.

    ![Adicionar intenção](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

    Na página de detalhes de intenção da intenção recentemente adicionada, [adicionar utterances](#add-an-utterance-on-intent-page).

## <a name="rename-intent"></a>Mudar o nome do objetivo

1. No **intenção** página, clique no ícone de mudança de nome ![mudar o nome de intenção](./media/luis-how-to-add-intents/Rename-Intent-btn.png) junto ao nome da intenção. 

2. No **intenção** página, o nome da intenção atual é apresentado numa caixa de diálogo. Editar o nome da intenção e prima enter. O novo nome é guardado e apresentado na página de intenção.

    ![Editar intenção](./media/luis-how-to-add-intents/EditIntent-dialogbox.png)

## <a name="delete-intent"></a>Eliminar de intenções
Quando eliminar um objetivo diferente a intenção None, pode optar por adicionar todos os utterances como a intenção None. Isto é útil se precisar de mover os utterances em vez de eliminá-los.   

1. No **intenção** página, clique em de **eliminar intenção** junto à direita do nome de intenção. 

    ![Eliminar botão intenção](./media/luis-how-to-add-intents/DeleteIntent.png)

2. Clique no botão "Ok", na caixa de diálogo de confirmação.

<!--
    TBD: waiting for confirmation about which delete dialog is going to be in //BUILD

    ![Delete Intent Dialog](./media/luis-how-to-add-intents/DeleteIntent-Confirmation.png)
-->


## <a name="add-an-utterance-on-intent-page"></a>Adicionar um utterance na página intenção

Na página de intenção, introduza um utterance relevante que se espera de utilizadores, tais como `book 2 adult business tickets to Paris tomorrow on Air France` na caixa de texto abaixo intenção nome e, em seguida, prima Enter. 
 
>[!NOTE]
>LUIS converte todos os utterances em minúsculas.

![Página de detalhes de captura de ecrã de pendentes, com utterance realçado](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

Utterances são adicionados à lista de utterances para o objetivo atual. Quando é adicionado um utterance, [etiqueta qualquer entidades](luis-how-to-add-example-utterances.md) dentro de utterances e [preparar](luis-how-to-train.md) a aplicação. 

## <a name="create-a-pattern-from-an-utterance"></a>Criar um padrão a partir de um utterance
Consulte [adicionar padrão do utterance existente na página de intenção ou entidade](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="edit-an-utterance-on-intent-page"></a>Editar um utterance na página intenção

Para editar um utterance, selecione o ícone de reticências (…) na extremidade direita da linha para esse utterance e, em seguida, selecione **editar**. Modificar o texto, em seguida, prima Enter no teclado.

![Página de detalhes de captura de ecrã de pendentes, com o ícone de três pontos realçado](./media/luis-how-to-add-intents/edit-utterance.png) 

## <a name="reassign-utterances-on-intent-page"></a>Reatribuir utterances na página intenção
Pode alterar o objetivo de um ou mais utterances reatribuição-los para outra intenção. 

Para reatribuir um utterance único para um objetivo diferente, na extremidade direita da linha de utterance, selecione o nome correto intenção no **etiqueta intenção** coluna. O utterance é removido da lista de utterance a intenção atual. 

![Página de intenção de captura de ecrã de BookFlight com o objetivo de um utterance na coluna intenção Labeled selecionado](./media/luis-how-to-add-intents/reassign-1-utterance.png)

Para alterar o objetivo utterances várias, selecione as caixas de verificação para a esquerda dos utterances e, em seguida, selecione **reatribuir intenção**. Selecione o objetivo correto da lista.

![Página de intenção de captura de ecrã de BookFlight com um utterance marcada e o botão de intenção de reatribuir realçado](./media/luis-how-to-add-intents/delete-several-utterances.png) 

## <a name="delete-utterances-on-intent-page"></a>Eliminar utterances na página intenção

Para eliminar um utterance, selecione o ícone de reticências (…) na extremidade direita da linha para esse utterance e, em seguida, selecione **eliminar**. O utterance é removido da lista e a aplicação de LUIS.

![Página de detalhes de captura de ecrã de pendentes, com a opção de eliminação realçada](./media/luis-how-to-add-intents/delete-utterance-ddl.png)

Para eliminar vários utterances:

1. Selecione as caixas de verificação para a esquerda dos utterances e, em seguida, selecione **eliminar utterances (s)**. 

    ![Página de detalhes de captura de ecrã de pendentes, com utterances marcada e eliminar botão utterance(s) realçado](./media/luis-how-to-add-intents/delete-several-utterances.png)

2. Selecione **feito** no **eliminar utterances?** caixa de diálogo de pop-up.

## <a name="search-in-utterances-on-intent-page"></a>Procurar utterances na página intenção
Objetivo, pode procurar utterances que contêm texto (palavras ou frases reconhecíveis). Por exemplo, poderá notar um erro que envolva uma palavra específica e que pretende localizar todos os exemplos que incluem esse word específico. 

1. Selecione o ícone da Lupa na barra de ferramentas.

    ![Página de captura de ecrã de pendentes, com o ícone de pesquisa de lupa realçada](./media/luis-how-to-add-intents/magnifying-glass.png)

2. É apresentada uma caixa de texto de pesquisa. Escreva a palavra ou expressão na caixa de pesquisa no canto superior direito da lista utterances. Os utterances listam atualizações, para apresentar apenas os utterances que incluem o seu texto de pesquisa. 

    ![Página de captura de ecrã de pendentes, com a caixa de texto de pesquisa realçada](./media/luis-how-to-add-intents/search-textbox.png)

    Para cancelar a pesquisa e restaurar a lista completa de utterances, elimine o texto de pesquisa que introduziu. Para fechar a caixa de texto de pesquisa, selecione o ícone da Lupa na barra de ferramentas novamente.

## <a name="prediction-discrepancy-errors-on-intent-page"></a>Erros de discrepância de predição na página intenção
Um utterance no objetivo poderá ter uma discrepância entre o objetivo de selecionada e a classificação de predição. LUIS indica esta discrepância com uma caixa vermelha à volta de pontuação. 

![Página de captura de ecrã da intenção BookFlight, com predição discrepância pontuação realçada](./media/luis-how-to-add-intents/score-discrepancy.png) 

## <a name="filter-by-intent-prediction-discrepancy-errors-on-intent-page"></a>Filtrar por erros de discrepância de intenção de predição na página intenção
Para filtrar a lista de utterance para apenas utterances com uma discrepância de predição intenção, alternar de **Mostrar tudo** para **apenas erros** na barra de ferramentas. 

## <a name="filter-by-entity-type-on-intent-page"></a>Filtrar por tipo de entidade na página intenção
Utilize o **filtros de entidade** pendente na barra de ferramentas para filtrar os utterances pela entidade. 

![Página de captura de ecrã de pendentes, com o filtro do tipo de entidade realçado](./media/luis-how-to-add-intents/filter-by-entities.png) 

Para remover o filtro, selecione a caixa de filtro azul com essa palavra ou expressão na barra de ferramentas.  
<!-- TBD: waiting for ux fix - bug in ux of prebuit entity number -- when filtering by it, it doesn't show the list -->

## <a name="switch-to-token-view-on-intent-page"></a>Mudar para vista de token na página intenção
Ativar/desativar **Tokens vista** para ver os tokens, em vez dos nomes de tipo de entidade. No teclado, também pode utilizar **controlo + E** para ativar/desativar a vista. 

![Captura de ecrã de BookFlight intenção, com vista Token realçado](./media/luis-how-to-add-intents/toggle-tokens-view.png)

## <a name="train-your-app-after-changing-model-with-intents"></a>Preparar a sua aplicação depois de alterar o modelo com pendentes
Depois de adicionar, editar ou remover pendentes, [preparar](luis-how-to-train.md) e [publicar](PublishApp.md) a aplicação para as suas alterações afetar a consultas de ponto final. 

## <a name="next-steps"></a>Passos Seguintes

Depois de adicionar pendentes para a sua aplicação, a próxima tarefa consiste em começar a adicionar [utterances exemplo](luis-how-to-add-example-utterances.md) para pendentes que adicionou. 
