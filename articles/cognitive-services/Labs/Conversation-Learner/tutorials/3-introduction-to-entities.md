---
title: Como utilizar entidades com um modelo de aprendiz de conversação - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como utilizar entidades com um modelo de aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 4b1c2d9390890618a9aa61eb425fbd132917f414
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51229053"
---
# <a name="introduction-to-entities"></a>Introdução às entidades

Este tutorial apresenta entidades e mostra como utilizar o "Disqualifying entidades" e "Required entidades" campos nas ações.

## <a name="video"></a>Vídeo

[![Pré-visualização do tutorial 3](https://aka.ms/cl-tutorial-03-preview)](https://aka.ms/blis-tutorial-03)

## <a name="requirements"></a>Requisitos

Este tutorial requer que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes

Este tutorial ilustra dois usos comuns para as entidades.  Em primeiro lugar, entidades podem extrair subcadeias de carateres a partir de uma mensagem de utilizador, como identificar uma cidade em "o que é o clima em Seattle?".  Em segundo lugar, entidades podem restringir quando estão disponíveis ações.  Especificamente, uma ação pode listar uma entidade como sendo "required" ou "disqualifying":
- Entidades de necessária uma ação tem de estar presentes na memória do bot, para que a ação para estar disponível
- Disqualifying entidades têm *não* estar presente na memória do bot, para que a ação para estar disponível

Outros tutoriais sobre outros aspectos de entidades, como entidades criadas previamente, valores múltiplos e entidades negatable, entidades programáticas e manipulando entidades no código.

## <a name="steps"></a>Passos

### <a name="create-the-model"></a>Criar o modelo

1. Na IU da Web, clique em novo modelo
2. No nome, digite IntroToEntities. Em seguida, clique em criar.

### <a name="create-entity"></a>Criar entidade

1. Clique em entidades, em seguida, nova entidade.
2. Introduza nome da entidade, cidade.
3. Clique em Criar

> [!NOTE]
> O tipo de entidade é 'custom' – Isto significa que a entidade pode ser treinada.  Também existem entidades criadas previamente, que significa que seu comportamento não pode ser ajustado – eles são abordados em outro tutorial.

### <a name="create-two-actions"></a>Criar duas ações

1. Clique em ações, em seguida, nova ação
2. Em resposta, escreva "Não sei que desejar de Localidade".
3. Nas entidades Disqualifying, introduza $city. Clique em Guardar.
    - Isso significa que se esta entidade é definida na memória do bot, em seguida, esta ação vai *não* estar disponível.
2. Clique em ações, em seguida, nova ação para criar uma segunda ação.
3. Em resposta, escreva "o clima no $city é provavelmente ensolarado".
4. Nas entidades necessárias, entidade de cidade foi adicionada automaticamente, uma vez que ele foi referido.
5. Clicar em Guardar

Agora tem duas ações.

![](../media/tutorial3_actions.PNG)

### <a name="train-the-bot"></a>Preparar o bot

1. Clique em caixas de diálogo do Train, em seguida, nova caixa de diálogo de comboio.
2. Escreva "hello".
3. Clique em ações de pontuação e selecione "Não sei que localidade pretende?"
    - Não é possível selecionar a resposta em que a entidade de cidade é necessária porque a entidade de cidade não está definida na memória do bot.
2. Selecione "Não sei que desejar de Localidade".
4. Introduza "Lisboa". Realçar seattle, em seguida, clique em cidade.
5. Clique em ações de pontuação
    - Valor de cidade está agora na memória do bot.
    - "O clima em $city é provavelmente ensolarado" está agora disponível como uma resposta. 
6. Selecione "O clima em $city é provavelmente ensolarado".

Vamos supor que o usuário insere "Repetir que". 
1. Escreva o que e introduza. Entidade de cidade e o respetivo valor é na memória e está disponível.
2. Selecione "O clima em $city é provavelmente ensolarado".

![](../media/tutorial3_entities.PNG)

Acabou de criar uma entidade e rotulado como instâncias do mesmo nas mensagens de utilizador.  Também utilizou a presença ausência da entidade na memória do bot para controlar quando as ações estão disponíveis, através da ação disqualifying e campos de entidades necessárias.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Entidade esperada](./4-expected-entity.md)
