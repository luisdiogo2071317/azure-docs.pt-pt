---
title: Adicionar intenções em aplicativos de LUIS | Documentos da Microsoft
description: Utilize a compreensão de idiomas (LUIS) para adicionar intenções para ajudar a compreender os pedidos de utilizador e reagir a eles corretamente de aplicações.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: 0ebf15ea49467674ab3c56aa7983131593cf5c9a
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225891"
---
# <a name="manage-intents"></a>Gerir os objetivos 
Adicione [intenções](luis-concept-intent.md) à sua aplicação LUIS para identificar os grupos de perguntas ou comandos que têm as mesmo intenções. 

Adicionar e gerir os seus objetivos do **intenções** página, disponível no **intenções** no painel esquerdo do de LUIS. 

O procedimento seguinte demonstra como adicionar a intenção de "Bookflight" na aplicação TravelAgent.

## <a name="add-intent"></a>Adicionar intenções

1. Abra a sua aplicação (por exemplo, TravelAgent) clicando em seu nome na **as minhas aplicações** página e, em seguida, clique em **intenções** no painel esquerdo. 
2. Sobre o **intenções** página, clique em **criar intenção de novo**.

    ![Lista de objetivos](./media/luis-how-to-add-intents/IntentsList.png)
3. Na **criar novo intenção** caixa de diálogo, escreva a intenção nome "BookFlight" e clique em **feito**.

    ![Adicionar intenções](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

    Na página de detalhes de intenção da recém-adicionada intenção, [adicionar expressões](#add-an-utterance-on-intent-page).

## <a name="rename-intent"></a>Mudar o nome de intenção

1. Sobre o **intenção** página, clique no ícone de mudar o nome ![intenção de mudar o nome](./media/luis-how-to-add-intents/Rename-Intent-btn.png) junto ao nome da intenção. 

2. Sobre o **intenção** página, o nome de intenção atual é mostrado numa caixa de diálogo. Editar o nome da intenção e prima enter. O novo nome é guardado e é apresentado na página de intenção.

    ![Editar intenção](./media/luis-how-to-add-intents/EditIntent-dialogbox.png)

## <a name="delete-intent"></a>Eliminar intenção
Ao excluir um objetivo diferente de intenção None, pode optar por adicionar todas as expressões a intenção None. Isto é útil se precisar de mover as expressões em vez de excluí-los.   

1. Na **intenção** página, clique no **intenção eliminar** botão junto à direita do nome de intenção. 

    ![Eliminar o botão de intenção](./media/luis-how-to-add-intents/DeleteIntent.png)

2. Clique no botão "Ok" na caixa de diálogo de confirmação.

<!--
    TBD: waiting for confirmation about which delete dialog is going to be in //BUILD

    ![Delete Intent Dialog](./media/luis-how-to-add-intents/DeleteIntent-Confirmation.png)
-->


## <a name="add-an-utterance-on-intent-page"></a>Adicionar uma expressão na página de intenção

Na página de intenção, introduza uma expressão relevante que se espera de seus usuários, como `book 2 adult business tickets to Paris tomorrow on Air France` na caixa de texto abaixo do nome intenção e, em seguida, prima Enter. 
 
>[!NOTE]
>LUIS converte todas as expressões de com em minúsculas.

![Página de detalhes de captura de ecrã de objetivos, com a expressão realçado](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

Expressões com são adicionados à lista de expressões com para o objetivo atual. Depois de uma expressão é adicionada [Etiquetar qualquer entidades](luis-how-to-add-example-utterances.md) dentro as expressões e [treinar](luis-how-to-train.md) seu aplicativo. 

## <a name="create-a-pattern-from-an-utterance"></a>Criar um padrão a partir de uma expressão
Ver [adicionar padrão de expressão existente na página de intenção ou a entidade](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="edit-an-utterance-on-intent-page"></a>Editar uma expressão na página de intenção

Para editar uma expressão, selecione as reticências (***...*** ) na extremidade direita da linha para essa expressão e, em seguida, selecione **editar**. Modificar o texto, em seguida, prima Enter no teclado.

![Página de detalhes de captura de ecrã de objetivos, com o botão de reticências realçado](./media/luis-how-to-add-intents/edit-utterance.png) 

## <a name="reassign-utterances-on-intent-page"></a>Reatribuir expressões com na página de intenção
Pode alterar a intenção de expressões com um ou mais por meio da reatribuição-los em outra intenção. 

Para reatribuir uma expressão única para um objetivo diferente, na extremidade direita da linha da expressão, selecione o nome correto de intenção sob o **rotulado intenção** coluna. A expressão é removido da lista de expressão a intenção atual. 

![Página de intenção de captura de ecrã de BookFlight com a intenção de uma expressão na coluna de intenção Labeled selecionada](./media/luis-how-to-add-intents/reassign-1-utterance.png)

Para alterar a intenção de expressões com vários, selecione as caixas de verificação à esquerda das expressões e, em seguida, selecione **reatribuir intenção**. Selecione a intenção correta da lista.

![Página de intenção de captura de ecrã de BookFlight com uma expressão selecionada e o botão de intenção de reatribuição realçado](./media/luis-how-to-add-intents/delete-several-utterances.png) 

## <a name="delete-utterances-on-intent-page"></a>Eliminar expressão na página de intenção

Para eliminar uma expressão, selecione as reticências (***...*** ) na extremidade direita da linha para essa expressão e, em seguida, selecione **eliminar**. A expressão é removido da lista e a aplicação do LUIS.

![Página de detalhes de captura de ecrã de objetivos, com a opção de eliminação realçada](./media/luis-how-to-add-intents/delete-utterance-ddl.png)

Para eliminar várias expressões:

1. Selecione as caixas de verificação à esquerda das expressões e, em seguida, selecione **eliminar expressões com o (s)**. 

    ![Página de detalhes de captura de ecrã de objetivos, com expressões com verificada e eliminar botão utterance(s) realçado](./media/luis-how-to-add-intents/delete-several-utterances.png)

2. Selecione **feito** no **eliminar expressões com?** caixa de diálogo pop-up.

## <a name="search-in-utterances-on-intent-page"></a>Pesquisar em expressões com na página de intenção
Uma intenção, pode procurar expressões que contêm texto (palavras ou frases). Por exemplo, pode observar um erro que envolve uma palavra específica e para localizar todos os exemplos incluem essa palavra específica. 

1. Selecione o ícone de lupa na barra de ferramentas.

    ![Página de captura de ecrã de objetivos, com o ícone de pesquisa de lupa realçada](./media/luis-how-to-add-intents/magnifying-glass.png)

2. É apresentada uma caixa de texto de pesquisa. Escreva a palavra ou frase na caixa de pesquisa no canto superior direito da lista de expressões. As expressões listam atualizações, para apresentar apenas as expressões que incluem o texto a procurar. 

    ![Página de captura de ecrã de objetivos, com a caixa de texto de pesquisa realçada](./media/luis-how-to-add-intents/search-textbox.png)

    Para cancelar a pesquisa e restaurar a lista completa das expressões, elimine o texto de pesquisa que introduziu. Para fechar a caixa de texto de pesquisa, selecione o ícone de lupa na barra de ferramentas novamente.

## <a name="prediction-discrepancy-errors-on-intent-page"></a>Erros de discrepância de predição na página de intenção
Uma expressão numa intenção, pode ter uma discrepância entre a intenção de selecionado e a pontuação de predição. LUIS indica essa discrepância com uma caixa vermelha à volta a pontuação. 

![Página de captura de ecrã de BookFlight intenção, com score discrepância de predição realçado](./media/luis-how-to-add-intents/score-discrepancy.png) 

## <a name="filter-by-intent-prediction-discrepancy-errors-on-intent-page"></a>Filtrar por erros de discrepância de predição de intenção na página de intenção
Para filtrar a lista de expressão para expressões com apenas com uma discrepância de predição de intenção, alternar de **Mostrar tudo** ao **apenas erros** na barra de ferramentas. 

## <a name="filter-by-entity-type-on-intent-page"></a>Filtrar por tipo de entidade na página de intenção
Utilize o **filtros de entidade** pendente na barra de ferramentas para filtrar as expressões por entidade. 

![Página de captura de ecrã de objetivos, com o filtro de tipo de entidade realçado](./media/luis-how-to-add-intents/filter-by-entities.png) 

Para remover o filtro, selecione a caixa de filtro azul com essa palavra ou frase na barra de ferramentas.  
<!-- TBD: waiting for ux fix - bug in ux of prebuit entity number -- when filtering by it, it doesn't show the list -->

## <a name="switch-to-token-view-on-intent-page"></a>Mudar para vista de token na página de intenção
Botão de alternar **vista de Tokens** para ver os tokens em vez dos nomes de tipos de entidade. No teclado, também pode utilizar **controle + E** para alternar o modo de exibição. 

![Objetivo de captura de ecrã de BookFlight, com a vista de Token realçado](./media/luis-how-to-add-intents/toggle-tokens-view.png)

## <a name="train-your-app-after-changing-model-with-intents"></a>Preparar a sua aplicação depois de alterar o modelo com objetivos
Depois de adicionar, editar ou remover intenções, [treinar](luis-how-to-train.md) e [publicar](luis-how-to-publish-app.md) as suas alterações afetar a consultas de ponto final na sua aplicação. 

## <a name="next-steps"></a>Passos Seguintes

Depois de adicionar intenções à sua aplicação, a próxima tarefa consiste em começar a adicionar [expressões de exemplo](luis-how-to-add-example-utterances.md) para os objetivos que adicionou. 
