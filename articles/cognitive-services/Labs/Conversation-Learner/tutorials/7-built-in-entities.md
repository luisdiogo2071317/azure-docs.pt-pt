---
title: Como adicionar entidades criadas previamente para um modelo de aprendiz de conversação - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como adicionar entidades criadas previamente para um modelo de aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 84d73add5586aaaf130253a8122a4152e39bcbe9
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171372"
---
# <a name="how-to-add-pre-built-entities"></a>Como adicionar entidades criadas previamente
Este tutorial mostra como adicionar entidades de "previamente criados" ao seu modelo de aprendiz de conversação.

## <a name="video"></a>Vídeo

[![Pré-visualização do tutorial 7](http://aka.ms/cl-tutorial-07-preview)](http://aka.ms/blis-tutorial-07)

## <a name="requirements"></a>Requisitos
Este tutorial requer que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes

Entidades criadas previamente reconhecem tipos comuns de entidades, como números, datas, quantias monetárias e outras pessoas.  Ao contrário das entidades personalizadas, eles funcionam "out-of-the-box" e não requerem qualquer treinamento.  Ao contrário das entidades personalizadas, não é possível alterar seu comportamento.  Por padrão, entidades criadas previamente estão com múltiplos valores - ou seja, a memória do bot serão acumuladas identificada cada uma das instâncias da entidade.

## <a name="steps"></a>Passos

### <a name="create-the-model"></a>Criar o modelo

1. Na IU da Web, clique em novo modelo
2. No nome, digite BuiltInEntities. Em seguida, clique em criar.

### <a name="create-an-entity"></a>Criar uma entidade

1. Clique em entidades, em seguida, nova entidade.
2. Clique no menu suspenso de EntityType e selecione datetimev2.
    - Opções de programável e Negatable estão desativadas, porque eles não são aplicáveis a pré-criar entidades.
3. Clique em Criar.

![](../media/tutorial7_entities.PNG)

### <a name="create-two-actions"></a>Criar duas ações

1. Clique em ações, em seguida, nova ação
2. Em resposta, escreva "a data é $luis-datetimev2'.
3. Clique em Criar.

![](../media/tutorial7_actions.PNG)

Em seguida, crie a segunda ação:

1. Clique em ações, em seguida, nova ação para criar uma segunda ação.
3. Em resposta, escreva "O que é a data?".
4. Nas entidades Disqualifying, introduza 'luis datetimev2'.
4. Clique em Criar

![](../media/tutorial7_actions2.PNG)

Agora tem duas ações.

### <a name="train-the-bot"></a>Preparar o bot

1. Clique em caixas de diálogo do Train, em seguida, nova caixa de diálogo de comboio.
2. Escreva "hello".
3. Clique em ações de pontuação e selecione "O que é a data?"
2. Introduza "hoje". 
    - Observe que hoje é marcado e aparece na segunda linha, uma vez que é uma entidade previamente criada e não editável.
5. Clique em ações de pontuação
    - Observe que agora a data é apresentada na secção de memória de entidade. 
    - Se passa o mouse sobre a data, verá os dados adicionais fornecidos pela LUIS, que pode ser utilizado e ainda mais pode ser manipulado no código. 
6. Selecione "a data é $luis-datetimev2'.
7. Clique em concluído ensino

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Entradas alternativas](./8-alternative-inputs.md)
