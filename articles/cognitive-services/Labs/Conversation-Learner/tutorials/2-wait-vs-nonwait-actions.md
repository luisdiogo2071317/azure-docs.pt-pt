---
title: Como utilizar wait e ações de não-espera com um modelo de aprendiz de conversação - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como utilizar wait e ações de não-espera com um modelo de aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: b09279e61ad60abcbda8b5bf576f5145ea8b9602
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51239258"
---
# <a name="wait-and-non-wait-actions"></a>Aguarde e não espera ações

Este tutorial mostra a diferença entre as ações de espera e as ações de não-espera no Aprendiz de conversação.

## <a name="video"></a>Vídeo

[![Pré-visualização do tutorial 2](https://aka.ms/cl-tutorial-02-preview)](https://aka.ms/blis-tutorial-02)

## <a name="requirements"></a>Requisitos
Este tutorial requer que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes

- Aguarde a ação: depois do sistema demora uma ação "wait", irá realizar ações de parar e aguardar pela intervenção do utilizador.
- Ação de não-espera: depois do sistema executa uma ação de "não espera", ele irá escolher imediatamente outra ação (sem ter de esperar pela primeira vez a entrada do usuário).

## <a name="steps"></a>Passos

### <a name="create-a-new-model"></a>Criar um novo modelo

1. Na IU da Web, clique em novo modelo
2. No nome, digite WaitNonWait. Em seguida, clique em criar.

### <a name="create-the-first-wait-action"></a>Criar a primeira ação de espera

1. Clique em ações, em seguida, nova ação.
2. Em resposta, introduza "qual animal deseja?".
    - Esta é uma ação de espera, portanto, deixe o tempo de espera para a caixa de resposta selecionada.
3. Clique em Criar.

### <a name="create-a-non-wait-action"></a>Criar uma ação de não-Wait

1. Clique em nova ação
2. Em resposta, escreva 'Cows dizer moo'.
3. Desmarque a espera para a caixa de verificação de resposta.
4. Clique em Criar

### <a name="create-a-second-non-wait-action"></a>Criar uma segunda ação de não-Wait

1. Clique em nova ação
2. Em resposta, escreva 'Patos dizem quack'.
3. Desmarque a espera para a caixa de verificação de resposta.
4. Clique em Criar

![](../media/tutorial2_actions.PNG)

### <a name="train-the-bot"></a>Preparar o bot

1. Clique em caixas de diálogo do Train, em seguida, nova caixa de diálogo de comboio.
2. Tipo "Olá"
3. Clique em ações de pontuação e selecione "qual animal deseja?".
4. Introduza "vaca"
5. Clique em ações de pontuação e selecione 'Cows dizer moo'.
    - O bot será não aguardam uma entrada do e levará a próxima ação.
2. Selecione "qual animal deseja?".
3. Introduza "pato"
5. Clique em ações de pontuação e selecione 'Patos dizem quack'.

![](../media/tutorial2_dialogs.PNG)

> [!NOTE]
> A seqüência das respostas bot com respeito a espera e as ações de não-espera.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Introdução às entidades](./3-introduction-to-entities.md)
