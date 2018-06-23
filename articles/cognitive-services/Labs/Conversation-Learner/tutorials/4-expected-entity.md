---
title: Como utilizar a propriedade "esperado entidade" das ações de conversação Learner - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como utilizar a propriedade "esperado entidade" de uma aplicação de conversação Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 83911eba8112cf356af8c4cd562a87f746fbabc5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354056"
---
# <a name="how-to-use-the-expected-entity-property-of-actions"></a>Como utilizar a propriedade "Esperado entidade" de ações

Este tutorial demonstra o campo "esperado entidade" de ações.

## <a name="requirements"></a>Requisitos
Este tutorial, necessita que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes
Utilize o campo "esperado entidade" de uma ação para comunicar com o sistema que espera de que resposta do utilizador para uma ação será para o estado de uma entidade.

Concretely, se o campo "esperado entidade" de uma ação estiver definido para $entity, em seguida, no utterance de utilizador seguinte, o sistema irá:

1. Em primeiro lugar, como habitualmente, tentam localizar entidades utilizando o modelo de extração de entidade com base do machine learning
2. Se não existem entidades encontram-se no passo 1, em seguida, – como um heuristic – atribua utterance o utilizador de toda a $entity.
3. Chamar `EntityDetectionCallback` como habitualmente e prossiga com a seleção de ação.

## <a name="steps"></a>Passos

### <a name="create-the-application"></a>Criar a aplicação

1. Na IU da Web, clique em nova aplicação
2. No nome, introduza ExpectedEntities. Em seguida, clique em criar.

### <a name="create-an-entity"></a>Criar uma entidade

1. Clique em entidades, em seguida, nova entidade.
2. No nome da entidade, introduza o nome.
3. Clique em Criar

Tenha em atenção que o tipo de entidade é 'custom' – Isto significa que a entidade pode ser preparada.  Também existem entidades pré-criadas, que significa que o respetivo comportamento não pode ser ajustado – estes estão descritos no tutorial outro.

![](../media/tutorial4_entities.PNG)

### <a name="create-two-actions"></a>Criar duas ações

1. Clique em ações, em seguida, nova ação
2. Em resposta, escreva 'O que é o nome da sua?'.
3. Era esperado entidades, introduza $name. Clique em Guardar.
    - Isto significa que se esta questão é pedido e a resposta do utilizador não tem qualquer entidades detetadas, o bot deve partem do princípio de que totalidade da resposta do utilizador é esta entidade.
2. Clique em ações, em seguida, nova ação para criar uma segunda ação.
3. Em resposta, escreva 'Olá $name'.
    - Tenha em atenção que a entidade é automaticamente adicionada como uma entidade disqualifying. 
4. Clicar em Guardar

Tem agora duas ações.

![](../media/tutorial4_actions.PNG)

### <a name="train-the-bot"></a>Preparar o bot

1. Clique em caixas de diálogo de formação, caixa de diálogo de formação, em seguida, novo.
2. Escreva "Olá".
3. Clique em ações de pontuação e selecione 'O que é o nome'?
    - Tenha em atenção que a resposta "Olá, $name' não pode ser seleccionada, porque este requies a entidade $name definido e $name não está na memória do bot.
2. Introduza 'david'. 
    - Tenha em atenção que o nome é realçado como uma entidade. Trata-se devido ao heuristic que configuramos acima para selecionar a resposta como a entidade.
5. Clique em ações de pontuação
    - Valor de nome de nota está agora na memória do bot.
    - 'Olá $name' está agora disponível como uma resposta. 
6. Selecione 'Olá $name'.
7. Clique em concluído de ensino.

Seguem-se dois exemplos onde o modelo de extração de entidade de machine-learning identifica um nome de, pelo que não está activado heuristic "esperado entidade".

1. Clique em nova caixa de diálogo de formação.
2. Introduza "os meus nome david'.
    - Tenha em atenção que, identificar o david como a entidade de nome porque vistos esta palavra antes.
2. Clique em ações de pontuação
3. Selecione 'Olá $name'.
4. Introduza "os meus nome susan'.
    - Tenha em atenção que identifica susan como o nome, uma vez que os vistos já este padrão.
2. Clique em ações de pontuação.
2. Selecione 'Olá susan'.
3. Clique em concluído de ensino.

Segue-se dois exemplos adicionais onde heuristic "esperado entidade" aciona, mas está incorreta e como podemos efetuar uma correção.

1. Escreva 'telefonar-me blogue'.
    - Tenha em atenção que não reconhece o nome como uma entidade.
2. Clique no blogue e selecione o nome.
3. Clique em ações de pontuação.
4. Selecione Olá $name.
5. Clique em concluído de ensino.
1. Clique em nova caixa de diálogo de formação.
2. Introduza "Olá".
3. Em resposta a 'o que é o nome', introduza 'M' chamado frank'.
    - Tenha em atenção que o frase completo é realçado. Isto acontece porque o modelo de estatística não foi possível localizar um nome, para que o heuristic desencadeado e selecionar a resposta completa como a entidade de nome.
2. Para corrigi-lo, clique no frase realçado e clique em sobre o x vermelho. 
3. Clique para selecionar o frank, em seguida, clique no nome.
2. Clique em ações de pontuação
3. Selecione 'Olá $name'.
4. Clique em concluído de ensino.

![](../media/tutorial4_dialogs.PNG)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Entidades negatable](./5-negatable-entities.md)
