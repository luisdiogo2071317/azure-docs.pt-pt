---
title: Como utilizar a ramificação e anular operações com um modelo de aprendiz de conversação - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como utilizar a ramificação e anular operações com um modelo de aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 05140693026e21a73b756ed0ea7bc9936bef067e
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39173303"
---
# <a name="how-to-use-branching-and-undo-operations"></a>Como utilizar a ramificação e operações de desfazer
Neste tutorial, iremos abordará desfazer e operações de ramificação.


## <a name="details"></a>Detalhes
- Desfazer: permite que o desenvolvedor "desfazer" uma opção de entrada ou de ação do utilizador. Em segundo plano, "desfazer", na verdade, cria uma nova caixa de diálogo e desempenha-lo novamente até o passo anterior.  Isso significa que a API de retorno de chamada de detecção de entidade e chama-se na caixa de diálogo será chamada novamente.

- Ramo: cria uma caixa de diálogo Novo train que começa da mesma forma como um existente treinar a caixa de diálogo – Isso economiza o esforço de caixa de diálogo Inserir manualmente novamente ativa. Em segundo plano, o "ramo" cria uma nova caixa de diálogo e desempenha novamente a caixa de diálogo train existentes até ao passo selecionado.  Isso significa que a API de retorno de chamada de detecção de entidade e chama-se na caixa de diálogo será chamada novamente.


## <a name="requirements"></a>Requisitos
Este tutorial requer que o bot de ordem de pizza está em execução:

    npm run demo-pizza

### <a name="open-the-demo"></a>Abra a demonstração

Na lista de modelo da IU da web, clique no pedido de Pizza TutorialDemo. 

Para obter detalhes sobre a demonstração de ordem de Pizza, veja o tutorial de ordem de Pizza.

## <a name="undo"></a>Anular

Iremos anular parte da caixa de diálogo e recriá-lo desse passo.

### <a name="training-dialogs"></a>Caixas de diálogo de treinamento
Vamos iniciar uma sessão de treinamento. 

1. Clique em caixas de diálogo do Train, em seguida, nova caixa de diálogo de comboio.
1. Introduza 'order uma pizza'.
2. Clique em ação de pontuação.
3. Clique para selecionar "o que deseja no seu pizza?"
4. Introduza 'cresce e aprecia o queijo'.
5. Clique em ações de pontuação.
3. Clique para selecionar 'tiver $Toppings no seu pizza'.
6. Selecione "Gostaria que qualquer outra coisa?"
7. Introduza "Remover cresce e adicionar peppers".
    - Selecione cresce e desmarque a entidade de sabores. Estamos a criar uma ação que podemos irá anular.
2. Clique em Anular passo.
    - A última entrada é removida e estão de volta "Gostaria que qualquer outra coisa?"  (captura de ecrã abaixo)
2. Introduza "Remover cresce e adicionar peppers".
8. Clique para selecionar 'tiver $Toppings no seu pizza'
    - Certifique-se de que as duas entidades estão selecionadas corretamente.
2. Clique em ação de pontuação. Pode continuar com a caixa de diálogo corrigida agora.
4. Clique em concluído ensino.

Agora viu como usar anular para remover um entrada do usuário e a ação.

![](../media/tutorial15_undo.PNG)

## <a name="branch"></a>Ramo

Por exemplo, vamos abrir uma caixa de diálogo train existente e criar outra caixa de diálogo train pela ramificação.

1. Clique em caixas de diálogo de comboio, em seguida, "novo pedido" para abrir a caixa de diálogo existente. 
2. Clique na última "não" na caixa de diálogo (veja a captura de ecrã abaixo).
3. Clique em filiais.
    - "não" é removido e a caixa de diálogo toda até esse ponto é copiada para um novo. 
    - Desta forma, poupa re-inserção anterior passa a explorar um "ramo" novo a partir deste ponto.
1. Introduza "Sim".
2. Clique em ação de pontuação.
3. Selecione 'Tiver $Toppings no seu pizza'.
6. Selecione "Gostaria que qualquer outra coisa?"
7. Introduza "não".
4. Clique em concluído ensino.

![](../media/tutorial15_branch.PNG)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Controlo de versões e etiquetagem](./16-versioning-and-tagging.md)
