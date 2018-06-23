---
title: Como utilizar valores múltiplos entidades com uma aplicação de conversação Learner - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como utilizar valores múltiplos entidades com uma aplicação de conversação Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 865b50747b2c9574b5f88d4902bea9e4c8e0e032
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354008"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-application"></a>Como utilizar valores múltiplos entidades com uma aplicação de conversação Learner
Este tutorial mostra a propriedade "valor múltiplos" de entidades.

##<a name="requirements"></a>Requisitos
Este tutorial, necessita que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes
Uma entidade "valor múltiplos" acumula valores uma lista, em vez de armazenar um valor único.  Isto é útil para entidades em que o utilizador pode especificar mais do que um valor, como toppings num pizza.

Concretely, se uma entidade está marcada como "valores múltiplos", em seguida, cada reconhecer a instância da entidade será anexada a uma lista no bot memória (em vez de substituir um valor de entidade única).

## <a name="steps"></a>Passos

### <a name="create-the-application"></a>Criar a aplicação

1. Na IU da Web, clique em nova aplicação
2. No nome, introduza MultiValueEntities. Em seguida, clique em criar.

### <a name="create-an-entity"></a>Criar uma entidade

1. Clique em entidades, em seguida, nova entidade.
2. No nome da entidade, introduza Toppings.
3. Verificação com múltiplos valor.
    - Entidades de valores múltiplos acumularem um ou mais valores na entidade.
2. Verificação Negatable.  
    - Isto permitirá ao utilizador remover a respetiva lista de toppings pizza acumulado toppings.
3. Clique em Criar.

![](../media/tutorial6_entities.PNG)

### <a name="create-two-actions"></a>Criar duas ações

1. Clique em ações, em seguida, nova ação
2. Em resposta, escreva 'que toppings pretende?'.
3. Na Disqualifying entidades, introduza Toppings.
3. Clique em Criar

Em seguida, crie a segunda ação.

1. Clique em ações, em seguida, nova ação para criar uma segunda ação.
3. Em resposta, escreva ' seguem-se a sua toppings: $Toppings'.
4. Clique em Criar

Tem agora duas ações.

![](../media/tutorial6_actions.PNG)

### <a name="train-the-bot"></a>Preparar o bot

1. Clique em caixas de diálogo de formação, caixa de diálogo de formação, em seguida, novo.
2. Escreva "Olá".
3. Clique em ações de pontuação e selecione 'que toppings pretende?'
2. Introduza 'mushrooms e cheese'. 
    - Pode Etiquetar zero, um ou mais do que uma das entidades.
3. Clique em 'mushrooms' e selecione Toppings.
4. Clique em 'cheese' e selecione Toppings.
5. Clique em ações de pontuação
    - Tenha em atenção que os dois valores agora estão presentes na entidade Toppings. 
6. Selecione ' seguem-se a sua toppings: $Toppings'.

Iremos pode adicionar mais a isto:

7. Introduza 'Adicionar peppers'.
    - Clique em peppers em deteção de entidade e selecione Toppings.
3. Clique em ações de pontuação.
    - Tenha em atenção que peppers agora apresentado como um valor no Toppings adicional.
6. Selecione ' seguem-se a sua toppings: $Toppings'.

Vamos remover um topping e adicione uma:

2. Escreva 'Remover peppers e adicionar sausage'.
1. Clique em 'peppers' e clique no x vermelho para removê-lo.
2. Clique em 'peppers' e selecione '-Toppings'.
3. Clique em ações de pontuação.
    - Tenha em atenção que foi eliminada 'peppers' e 'sausage' foi adicionado.
6. Selecione ' seguem-se a sua toppings: $Toppings'.

Agora vamos tentar remover tudo:

6. Introduza 'Remover mushrooms, remova cheese e remover sausage'.
7. Clique em cada um dos três e selecione '-Toppings'.
7. Clique em ações de pontuação.
    - Tenha em atenção de que todos os toppings são eliminados.
2. Selecione "que toppings pretende?"
3. Clique em concluído ensino

![](../media/tutorial6_dialogs.PNG)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Entidades incorporadas](./7-built-in-entities.md)
