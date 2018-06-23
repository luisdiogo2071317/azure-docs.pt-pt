---
title: Como utilizar entidades negatable com uma aplicação de conversação Learner - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como utilizar entidades negatable com uma aplicação de conversação Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 3d65376c9c43ee1407468f3e8bf3e058048bd556
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354067"
---
# <a name="how-to-use-negatable-entities-with-a-conversation-learner-application"></a>Como utilizar entidades negatable com uma aplicação de conversação Learner

Este tutorial demonstra a propriedade "negatable" de entidades.

## <a name="requirements"></a>Requisitos
Este tutorial, necessita que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes
Marcar uma ação como "negatable" se o utilizador pode "Limpar" um valor de entidade, tal como em "Não, não pretendo $entity" ou "não, não $entity." Por exemplo, "não, não pretender deixar de Boston."

Concretely, se a propriedade "negatable" de uma entidade está definida:

- Quando a etiquetagem entidade menciona suportadas, permitem Etiquetar normais instâncias (positivos) de uma entidade e um "negativo" instâncias da entidade
- LUIS aprende dois modelos de entidade: um para instâncias positivas e um segundo para instâncias negativos
- O efeito de uma instância negativo de uma entidade é limpar esse valor da variável de entidade (se existir)

## <a name="steps"></a>Passos

### <a name="create-the-application"></a>Criar a aplicação

1. Na IU da Web, clique em nova aplicação
2. No nome, introduza NegatableEntity. Em seguida, clique em criar.

### <a name="create-an-entity"></a>Criar uma entidade

1. Clique em entidades, em seguida, nova entidade.
2. No nome da entidade, introduza o nome.
3. Verificação Negatable.
    - Isto indica que o utilizador será capaz de fornecer um valor para a entidade ou diga algo está *não* o valor da entidade. No caso desta última opção, tal resultará na eliminação de um valor de correspondência da entidade.
3. Clique em Criar.

![](../media/tutorial5_entities.PNG)

### <a name="create-two-actions"></a>Criar duas ações

1. Clique em ações, em seguida, nova ação
2. Em resposta, escreva 'Não souber o nome'.
3. Na Disqualifying entidades, introduza o nome.
3. Clique em Criar

Em seguida, crie a segunda ação.

1. Clique em ações, em seguida, nova ação para criar uma segunda ação.
3. Em resposta, escreva "sei o seu nome. É $name'.
4. Clique em Criar

Tem agora duas ações.

![](../media/tutorial5_actions.PNG)

### <a name="train-the-bot"></a>Preparar o bot

1. Clique em caixas de diálogo de formação, caixa de diálogo de formação, em seguida, novo.
2. Escreva "Olá".
3. Clique em ações de pontuação e selecione 'Não souber o nome'
    - Tenha em atenção que está a obter uma pontuação de 100% porque é a ação só é válida.
2. Introduza "os meus nome david'
3. Selecione 'david' e selecione a etiqueta + ' name'
    - Tenha em atenção de que existem duas instâncias de 'name': '+ name' e '-name'.  Além significa que estamos a proporcionar esse valor. Menos significa que podemos está a informar o sistema que algo não é o valor.
5. Clique em ações de pontuação
    - Valor de nome de nota está agora na memória do bot.
    - ' Sei o seu nome. É $name' está a resposta só se encontra disponível. 
6. Selecione "sei o seu nome. É $name'.

Vamos tentar limpar a entidade negatable:

7. Introduza 'o meu nome não é o david'.
    - Tenha em atenção é 'not' selecionada como nome com base no padrão anterior. Que é incorreto.
2. Clique em "não", em seguida, o x vermelho. 
3. Clique em 'david'.
    - Isto é agora uma entidade negativa comunicar que este não é o valor da entidade de nome.
2. Selecione '-name'.
3. Clique em ações de pontuação.
    - Tenha em atenção que o valor foi limpo da memória.
2. Selecione 'Não posso não souber o nome', que é a única ação.

Em seguida vamos mostrar como um novo valor para o nome pode ser introduzido.

3. Introduza 'João' como o nome. Em seguida, selecione "João" e clique no nome.
4. Clique em ações de pontuação.
5. Selecione "sei o seu nome. É $name'.

Agora, tente substituir o nome introduzido.

6. Introduza "os meus nome susan'.
7. Selecione "sei o seu nome. É $name'.
7. Clique em ações de pontuação.
8. Tenha em atenção **susan** substituíram **João** os valores de entidade.
9. Introduza 'o meu nome não é susan'.
    - Tenha em atenção de que o sistema tem com esta etiqueta como uma instância negativa.
2. Clique em ações de pontuação.
3. Selecione 'Não posso não souber o nome', que é a única ação.
7. Clique em concluído de ensino.

![](../media/tutorial5_dialogs.PNG)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Entidades de valores múltiplos](./6-multi-value-entities.md)
