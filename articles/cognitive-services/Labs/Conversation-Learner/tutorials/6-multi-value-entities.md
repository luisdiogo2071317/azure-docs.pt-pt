---
title: Como utilizar entidades com múltiplos valores com um modelo de aprendiz de conversação - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como utilizar entidades com múltiplos valores com um modelo de aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 9ef86ea728e197540180d486dc1cb171f78a2590
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51229463"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-model"></a>Como utilizar entidades com múltiplos valores com um modelo de aprendiz de conversação
Este tutorial mostra a propriedade "value multi" de entidades.

## <a name="video"></a>Vídeo

[![Pré-visualização do tutorial 6](https://aka.ms/cl-tutorial-06-preview)](https://aka.ms/blis-tutorial-06)

## <a name="requirements"></a>Requisitos
Este tutorial requer que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes
Uma entidade que é "com múltiplos valor" acumula os valores numa lista, em vez de armazenar um valor único.  Isto é útil para entidades em que o utilizador pode especificar mais do que um valor, como sabores numa pizza.

Concretamente, se uma entidade é marcada como "com múltiplos valores", em seguida, cada reconhecido instância da entidade será anexada a uma lista no bot memória (em vez de substituir um valor de entidade única).

## <a name="steps"></a>Passos

### <a name="create-the-model"></a>Criar o modelo

1. Na IU da Web, clique em novo modelo
2. No nome, digite MultiValueEntities. Em seguida, clique em criar.

### <a name="create-an-entity"></a>Criar uma entidade

1. Clique em entidades, em seguida, nova entidade.
2. No nome da entidade, digite sabores.
3. Verificação com múltiplos valor.
    - Entidades de valores múltiplos acumular-se um ou mais valores na entidade.
2. Verificação Negatable.  
    - Isso permitirá que o utilizador remover sabores de sua lista de sabores de pizza acumulado.
3. Clique em Criar.

![](../media/tutorial6_entities.PNG)

### <a name="create-two-actions"></a>Criar duas ações

1. Clique em ações, em seguida, nova ação
2. Em resposta, escreva "quais sabores deseja?".
3. Nas entidades Disqualifying, introduza sabores.
3. Clique em Criar

Em seguida, crie a segunda ação.

1. Clique em ações, em seguida, nova ação para criar uma segunda ação.
3. Em resposta, escreva "aqui estão os sabores: $Toppings'.
4. Clique em Criar

Agora tem duas ações.

![](../media/tutorial6_actions.PNG)

### <a name="train-the-bot"></a>Preparar o bot

1. Clique em caixas de diálogo do Train, em seguida, nova caixa de diálogo de comboio.
2. Escreva "hello".
3. Clique em ações de pontuação e selecione "quais sabores deseja?"
2. Introduza 'cresce e aprecia o queijo'. 
    - Pode Etiquetar zero, um ou mais do que uma das entidades.
3. Clique em "cresce" e selecione sabores.
4. Clique em "queijo" e selecione sabores.
5. Clique em ações de pontuação
    - Os dois valores agora estão presentes na entidade sabores. 
6. Selecione ' aqui estão os sabores: $Toppings'.

Podemos adicionar mais a este:

7. Introduza "Adicionar peppers".
    - Clique em peppers em deteção de entidade e selecione sabores.
3. Clique em ações de pontuação.
    - "peppers" aparece agora como um valor adicional na sabores.
6. Selecione ' aqui estão os sabores: $Toppings'.

Vamos remover um sabor e adicionar um:

2. Escreva "Remover peppers e adicionar sausage".
1. Clique em "peppers" e clique no x vermelho para removê-lo.
2. Clique em "peppers" e selecione '-dos sabores.
3. Clique em ações de pontuação.
    - foi eliminados 'peppers' e "sausage" foi adicionado.
6. Selecione ' aqui estão os sabores: $Toppings'.

Agora vamos tentar remover tudo:

6. Introduza 'Remover cresce, remova o queijo e remover sausage'.
7. Clique em cada um dos três e selecione '-dos sabores.
7. Clique em ações de pontuação.
    - Todos os sabores são limpos.
2. Selecione "quais sabores deseja?"
3. Clique em concluído ensino

![](../media/tutorial6_dialogs.PNG)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Entidades incorporadas](./7-built-in-entities.md)
