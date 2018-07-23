---
title: Como utilizar a propriedade "esperado entidade" das ações de aprendiz de conversação - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como utilizar a propriedade "esperado entidade" de um modelo de aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 9e013e237a996d722d958920a1310e3aaea36c52
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170913"
---
# <a name="how-to-use-the-expected-entity-property-of-actions"></a>Como utilizar a propriedade "Esperado entidade" das ações

Este tutorial demonstra o campo "esperado entidade" das ações.

## <a name="video"></a>Vídeo

[![Pré-visualização do tutorial 4](http://aka.ms/cl-tutorial-04-preview)](http://aka.ms/blis-tutorial-04)

## <a name="requirements"></a>Requisitos
Este tutorial requer que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes
Utilize o campo "esperado entidade" de uma ação para comunicar com o sistema que espera de que resposta do usuário para uma ação será para o estado de uma entidade.

Concretamente, se o campo "esperado entidade" de uma ação estiver definido para $entity, em seguida, na seguinte expressão de utilizador, o sistema irá:

1. Em primeiro lugar, como de costume, tenta localizar entidades usando o modelo de extração de entidades com base do machine learning
2. Se não existem entidades encontram-se no passo 1, em seguida, – como uma heurística – atribua a expressão de utilizadores completa para $entity.
3. Chamar `EntityDetectionCallback` como de costume e continuar a seleção de ação.

## <a name="steps"></a>Passos

### <a name="create-the-model"></a>Criar o modelo

1. Na IU da Web, clique em novo modelo
2. No nome, digite ExpectedEntities. Em seguida, clique em criar.

### <a name="create-an-entity"></a>Criar uma entidade

1. Clique em entidades, em seguida, nova entidade.
2. No nome da entidade, introduza o nome.
3. Clique em Criar

> [!NOTE]
> O tipo de entidade é 'custom'. Este valor significa que a entidade pode ser treinada.  Também existem entidades criadas previamente, que significa que seu comportamento não pode ser ajustado.  Estas entidades são abordadas os [tutorial Pre-Built entidades](./7-built-in-entities.md).

![](../media/tutorial4_entities.PNG)

### <a name="create-two-actions"></a>Criar duas ações

1. Clique em ações, em seguida, nova ação
2. Em resposta, escreva "O que é o seu nome?".
3. Nas entidades esperado, introduza $name. Clique em Guardar.
    - Este valor significa que se essa pergunta é solicitada e a resposta do utilizador não tem qualquer entidades detetadas, o bot deve presumir que o todo de resposta do usuário é esta entidade.
2. Clique em ações, em seguida, nova ação para criar uma segunda ação.
3. Em resposta, escreva 'Hello $name'.
    - A entidade é adicionada automaticamente como uma entidade disqualifying. 
4. Clicar em Guardar

Agora tem duas ações.

![](../media/tutorial4_actions.PNG)

### <a name="train-the-bot"></a>Preparar o bot

1. Clique em caixas de diálogo do Train, em seguida, nova caixa de diálogo de comboio.
2. Escreva "hello".
3. Clique em ações de pontuação e selecione "O que é o seu nome?"
    - A resposta 'Olá $name' não pode ser selecionada, porque ela exige que a entidade $name ser definidos e $name não está na memória do bot.
2. Introduza 'david'. 
    - O nome é realçado como uma entidade. Isso é devido a heurística, configuramos acima para selecionar a resposta como a entidade.
5. Clique em ações de pontuação
    - O valor de nome está agora na memória do bot.
    - 'Hello $name' está agora disponível como uma resposta. 
6. Selecione 'Hello $name'.
7. Clique em concluído ensino.

Aqui estão dois exemplos nos quais o modelo de extração de entidades de machine learning identifica um nome, para que a heurística "esperado entidade" não é acionada.

1. Clique em nova caixa de diálogo de comboio.
2. Introduza 'o meu nome é david'.
    - O modelo identifica david como a entidade de nome porque ele é visto como esta palavra antes.
2. Clique em ações de pontuação
3. Selecione 'Hello $name'.
4. Introduza 'o meu nome é susan'.
    - O modelo identifica susan como o nome, uma vez que ele é visto como esse padrão já.
2. Clique em ações de pontuação.
2. Selecione "Hello susan".
3. Clique em concluído ensino.

Nos exemplos a seguir, a heurística "esperado entidade" aciona, mas está incorreta. Os exemplos, em seguida, mostram como fazer uma correção.

1. Escreva 'telefonar-me jose'.
    - O modelo não reconhece o nome como uma entidade.
2. Clique em jose e selecione o nome.
3. Clique em ações de pontuação.
4. Selecione hello $name.
5. Clique em concluído ensino.
1. Clique em nova caixa de diálogo de comboio.
2. Introduza "hello".
3. Em resposta a "o que é o nome", introduza 'Eu sou chamado frank'.
    - A frase inteira é realçada. Isto acontece porque o modelo de estatístico não foi encontrado um nome, para que a heurística disparado e selecionado a resposta inteira como a entidade de nome.
2. Para corrigi-lo, clique na frase realçado, e clique no x vermelho. 
3. Clique para selecionar o frank, em seguida, clique no nome.
2. Clique em ações de pontuação
3. Selecione 'Hello $name'.
4. Clique em concluído ensino.

![](../media/tutorial4_dialogs.PNG)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Entidades negatable](./5-negatable-entities.md)
