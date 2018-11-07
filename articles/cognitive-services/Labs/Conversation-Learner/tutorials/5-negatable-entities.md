---
title: Como utilizar entidades negatable com um modelo de aprendiz de conversação - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como utilizar entidades negatable com um modelo de aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: df6499410da3ae67715ade29dbc3cc4146fc2265
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51231996"
---
# <a name="how-to-use-negatable-entities-with-a-conversation-learner-model"></a>Como utilizar entidades negatable com um modelo de aprendiz de conversação

Este tutorial demonstra a propriedade "negatable" de entidades.

## <a name="video"></a>Vídeo

[![Pré-visualização do tutorial de 5](https://aka.ms/cl-tutorial-05-preview)](https://aka.ms/blis-tutorial-05)

## <a name="requirements"></a>Requisitos
Este tutorial requer que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes
Marcar uma ação como "negatable" se o utilizador pode "Limpar" um valor de entidade, como em "Não, não pretendo $entity" ou "não, não $entity." Por exemplo, "não, não quero deixar de Boston".

Concretamente, se a propriedade "negatable" de uma entidade é definida:

- Quando menciona a entidade de etiquetagem, permitem-lhe Etiquetar normais instâncias (positivas) de uma entidade e uma "negativo" instâncias da entidade
- LUIS aprende dois modelos de entidade: um para instâncias positivas e um segundo para instâncias negativos
- O efeito de uma instância negativo de uma entidade é limpar esse valor da variável de entidade (se existir)

## <a name="steps"></a>Passos

### <a name="create-the-model"></a>Criar o modelo

1. Na IU da Web, clique em novo modelo
2. No nome, digite NegatableEntity. Em seguida, clique em criar.

### <a name="create-an-entity"></a>Criar uma entidade

1. Clique em entidades, em seguida, nova entidade.
2. No nome da entidade, introduza o nome.
3. Verificação Negatable.
    - Esta propriedade indica o utilizador será capaz de fornecer um valor para a entidade ou que é algo *não* o valor da entidade. No último caso, isso resultará em a eliminar um valor correspondente da entidade.
3. Clique em Criar.

![](../media/tutorial5_entities.PNG)

### <a name="create-two-actions"></a>Criar duas ações

1. Clique em ações, em seguida, nova ação
2. Em resposta, escreva "Não sei o nome".
3. Nas entidades Disqualifying, introduza o nome.
3. Clique em Criar

Em seguida, crie a segunda ação.

1. Clique em ações, em seguida, nova ação para criar uma segunda ação.
3. Em resposta, escreva "eu sei que o seu nome. É $name'.
4. Clique em Criar

Agora tem duas ações.

![](../media/tutorial5_actions.PNG)

### <a name="train-the-bot"></a>Preparar o bot

1. Clique em caixas de diálogo do Train, em seguida, nova caixa de diálogo de comboio.
2. Escreva "hello".
3. Clique em ações de pontuação e selecione "Não sei o nome"
    - A pontuação é 100% porque é a ação só é válida.
2. Introduza 'o meu nome é david'
3. Selecione "david" e selecione a etiqueta "+ nome"
    - Existem duas instâncias de "name": '+ name' e '-name ".  (+) Além disso, adiciona ou substitui o valor. (-) Sinal de subtração remove o valor.
5. Clique em ações de pontuação
    - O valor de nome está agora na memória do bot.
    - "Eu sei que o seu nome. É $name "é a resposta só estão disponível. 
6. Selecione "eu sei que o seu nome. É $name'.

Vamos tentar limpar a entidade negatable:

7. Introduza 'meu nome não é david'.
    - Observe que "não" está selecionado como o nome com base no padrão anterior. Esta etiqueta está incorreta.
2. Clique em "não", em seguida, o x vermelho. 
3. Clique em 'david'.
    - Agora se trata de uma entidade negativa comunicar que isso não é o valor da entidade de nome.
2. Selecione '-name ".
3. Clique em ações de pontuação.
    - Tenha em atenção que o valor foi limpo da memória.
2. Selecione "Não sei o nome", que é a única ação.

Em seguida vamos mostrar como um novo valor para o nome pode ser introduzido.

3. Introduza "john" como o nome. Em seguida, selecione "john" e clique no nome.
4. Clique em ações de pontuação.
5. Selecione "eu sei que o seu nome. É $name'.

Agora, tente substituir o nome introduzido.

6. Introduza 'o meu nome é susan'.
7. Selecione "eu sei que o seu nome. É $name'.
7. Clique em ações de pontuação.
8. Tenha em atenção **susan** substituíram **john** os valores da entidade.
9. Introduza 'meu nome não é susan'.
    - Observe que o sistema tem rotulado como isso como uma instância negativa.
2. Clique em ações de pontuação.
3. Selecione "Não sei o nome", que é a única ação.
7. Clique em concluído ensino.

![](../media/tutorial5_dialogs.PNG)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Entidades de valores múltiplos](./6-multi-value-entities.md)
