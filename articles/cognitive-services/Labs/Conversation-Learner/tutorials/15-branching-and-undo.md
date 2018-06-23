---
title: Como utilizar a ramificação e anular operações com uma aplicação de conversação Learner - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como utilizar a ramificação e operações com uma aplicação de conversação Learner de anular.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 724a9e47267e0bd7417130efe54c609ac7a465fb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354032"
---
# <a name="how-to-use-branching-and-undo-operations"></a>Como utilizar a ramificação e anular operações
Neste tutorial, iremos abordará as operações de ramificação e anulação.

## <a name="details"></a>Detalhes
- Anulação: permite ao programador "anular" uma opção de entrada ou de ação do utilizador. Nos bastidores, "anular", na verdade, cria uma caixa de diálogo Nova e reproduz-lo novamente até o passo anterior.  Isto significa que a chamada de retorno de deteção de entidade e a API chama na caixa de diálogo será chamada novamente.

- Ramo: cria uma caixa de diálogo Novo formação que começa da mesma forma como existente preparar a caixa de diálogo – este guarda o esforço de caixa de diálogo manualmente reintroduzir ativa. Nos bastidores, "ramo" cria uma caixa de diálogo Nova e reproduz novamente a caixa de diálogo de formação existente até o passo selecionado.  Isto significa que a chamada de retorno de deteção de entidade e a API chama na caixa de diálogo será chamada novamente.


## <a name="requirements"></a>Requisitos
Este tutorial, necessita que o bot de ordem pizza está em execução:

    npm run demo-pizza

### <a name="open-the-demo"></a>Abra a demonstração

Na lista de aplicações a IU da web, clique em TutorialDemo Pizza ordem. 

Para obter detalhes sobre a demonstração de ordem de Pizza, consulte o tutorial de ordem Pizza.

## <a name="undo"></a>Anular

Iremos anular parte da caixa de diálogo e recriá-lo a partir desse passo.

### <a name="training-dialogs"></a>Caixas de diálogo de formação
Vamos iniciar uma sessão de formação. 

1. Clique em caixas de diálogo de formação, caixa de diálogo de formação, em seguida, novo.
1. Introduza um pizza de ordem.
2. Clique em ação de pontuação.
3. Clique para selecionar "que gostaria no seu pizza?"
4. Introduza 'mushrooms e cheese'.
5. Clique em ações de pontuação.
3. Clique para seleccionar 'tiver $Toppings no seu pizza'.
6. Selecione "Gostaria há mais alguma coisa?"
7. Introduza 'Remover mushrooms e adicionar peppers'.
    - Selecione mushrooms e verificação de anular a entidade de Toppings. Estamos a criar uma ação que iremos irá anular.
2. Clique em Anular passo.
    - Tenha em atenção que a entrada último é removida e estamos volta ao "Gostaria há mais alguma coisa?"  (captura de ecrã abaixo)
2. Introduza 'Remover mushrooms e adicionar peppers'.
8. Clique para seleccionar 'tiver $Toppings no seu pizza'
    - Certifique-se que ambas as entidades estão selecionadas corretamente.
2. Clique em ação de pontuação. Pode continuar com a caixa de diálogo corrigida agora.
4. Clique em concluído de ensino.

Constatou agora como utilizar a anulação para remover um entrada do utilizador e a ação.

![](../media/tutorial15_undo.PNG)

## <a name="branch"></a>Ramo

Por exemplo, vamos abrir uma caixa de diálogo de formação existente e crie outra caixa de diálogo de formação pela ramificação.

1. Clique em diálogos da formação, em seguida, 'pela nova ordem' para abrir a caixa de diálogo existente. 
2. Clique no último 'não' na caixa de diálogo (consulte a captura de ecrã abaixo).
3. Clique em sucursais.
    - Tenha em atenção que "não" é removido e a caixa de diálogo toda até que ponto é copiada para um novo. 
    - Desta forma, poupa-reintroduzir precedente passa a explorar um novo "ramo" a partir deste ponto.
1. Introduza 'Sim'.
2. Clique em ação de pontuação.
3. Selecione 'Tiver $Toppings no seu pizza'.
6. Selecione "Gostaria há mais alguma coisa?"
7. Introduza 'Nenhum'.
4. Clique em concluído de ensino.

![](../media/tutorial15_branch.PNG)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Controlo de versões e etiquetagem](./16-versioning-and-tagging.md)
