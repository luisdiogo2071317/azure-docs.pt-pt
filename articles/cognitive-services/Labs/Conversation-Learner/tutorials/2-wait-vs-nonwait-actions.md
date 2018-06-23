---
title: Como utilizar espera e não espera ações com uma aplicação de conversação Learner - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como utilizar espera e não espera ações com uma aplicação de conversação Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: bb2cbd55b6c8be51213095926bb592169613d1fc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353947"
---
# <a name="wait-and-non-wait-actions"></a>Aguarde e não espera ações

Este tutorial mostra a diferença entre as ações de espera e não espera ações no Learner a conversação.

## <a name="requirements"></a>Requisitos
Este tutorial, necessita que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes

- Aguarde que a ação: depois do sistema executa uma ação de "espera", irá parar efetuando ações e aguardar a intervenção do utilizador.
- Não espera ação: depois do sistema executa uma ação de "não espera", irá escolher imediatamente outra ação (sem aguardar pela primeira vez o utilizador inpu).

## <a name="steps"></a>Passos

### <a name="create-a-new-app"></a>Criar uma nova aplicação

1. Na IU da Web, clique em nova aplicação
2. No nome, introduza WaitNonWait. Em seguida, clique em criar.

### <a name="create-the-first-wait-action"></a>Criar a primeira ação de espera

1. Clique em ações, em seguida, ação de novo.
2. Em resposta, introduza 'que preferido pretende?'.
    - Esta é uma ação de espera, por isso deixe a aguardar a resposta caixa selecionada.
3. Clique em concluído.

### <a name="create-a-non-wait-action"></a>Criar uma ação não espera

1. Clique em nova ação
2. Em resposta, escreva 'Cows diga moo'.
3. Verificação de anular a espera para a caixa de verificação de resposta.
4. Clique em Criar

### <a name="create-a-second-non-wait-action"></a>Criar uma segunda ação não espera

1. Clique em nova ação
2. Em resposta, escreva 'Ducks diga quack'.
3. Verificação de anular a espera para a caixa de verificação de resposta.
4. Clique em Criar

![](../media/tutorial2_actions.PNG)

### <a name="train-the-bot"></a>Preparar o bot

1. Clique em caixas de diálogo de formação, caixa de diálogo de formação, em seguida, novo.
2. Tipo "Olá"
3. Clique em ações de pontuação e selecione 'que preferido pretende?'.
4. Introduza 'cow'
5. Clique em ações de pontuação e selecione 'Cows diga moo'.
    - Tenha em atenção que o bot serão não aguardam uma entrada do e irá demorar a próxima ação.
2. Selecione 'que preferido pretende?'.
3. Introduza 'duck'
5. Clique em ações de pontuação e selecione 'Ducks diga quack'.

![](../media/tutorial2_dialogs.PNG)

Tenha em atenção a sequência das respostas bot relativamente a ações de não espera e de espera.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Introdução às entidades](./3-introduction-to-entities.md)
