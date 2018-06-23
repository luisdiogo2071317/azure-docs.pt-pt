---
title: Como utilizar as entidades com uma aplicação de conversação Learner - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como utilizar as entidades com uma aplicação de conversação Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 85df31c2e2ff3ca81698921a1f17f415daefb6c5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354002"
---
# <a name="introduction-to-entities"></a>Introdução às entidades

Este tutorial apresenta entidades e mostra como utilizar "Disqualifying entidades" e "Necessário entidades" campos nas ações.

## <a name="requirements"></a>Requisitos

Este tutorial, necessita que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes

Este tutorial ilustra dois utilizações comuns para entidades.  Em primeiro lugar, entidades podem extrair subcadeias a partir de uma mensagem de utilizador, tal como a identificação de uma cidade no "qual é a meteorologia em Seattle?".  Segundo, podem restringir a entidades quando as ações disponíveis.  Especificamente, uma ação pode listar uma entidade como "necessário" ou "disqualifying":
- Entidades necessária uma ação tem de estar presentes na memória do bot por ordem para a ação estar disponível
- Disqualifying entidades tem *não* estar presente na memória do bot por ordem para a ação estar disponível

Outros tutoriais abrangem outros aspetos de entidades, como previamente concebidas entidades, valores múltiplos e entidades negatable, programáticas entidades e manipulação de entidades no código.

## <a name="steps"></a>Passos

### <a name="create-the-application"></a>Criar a aplicação

1. Na IU da Web, clique em nova aplicação
2. No nome, introduza IntroToEntities. Em seguida, clique em criar.

### <a name="create-entity"></a>Criar entidade

1. Clique em entidades, em seguida, nova entidade.
2. No nome da entidade, introduza cidade.
3. Clique em Criar

Tenha em atenção que o tipo de entidade é 'custom' – Isto significa que a entidade pode ser preparada.  Também existem entidades pré-criadas, que significa que o respetivo comportamento não pode ser ajustado – estes estão descritos no tutorial outro.

### <a name="create-two-actions"></a>Criar duas ações

1. Clique em ações, em seguida, nova ação
2. Em resposta, escreva 'Não souber que cidade pretende'.
3. Na Disqualifying entidades, introduza $city. Clique em Guardar.
    - Isto significa que se esta entidade está definida na memória do bot, em seguida, esta ação irá *não* estar disponível.
2. Clique em ações, em seguida, nova ação para criar uma segunda ação.
3. Em resposta, escreva 'Meteorologia no $city é provavelmente sunny'.
4. No entidades necessários, tenha em atenção que a entidade de cidade foi adicionada automaticamente uma vez que este foi referido.
5. Clicar em Guardar

Tem agora duas ações.

![](../media/tutorial3_actions.PNG)

### <a name="train-the-bot"></a>Preparar o bot

1. Clique em caixas de diálogo de formação, caixa de diálogo de formação, em seguida, novo.
2. Escreva "Olá".
3. Clique em ações de pontuação e selecione 'Não sei que cidade quiser?'
    - Tenha em atenção que não é possível selecionar a resposta em que a entidade de cidade é necessária, porque a entidade de cidade não está definida na memória do bot.
2. Selecione 'Não souber que cidade pretende'.
4. Introduza 'seattle'. Realce seattle, em seguida, clique em cidade.
5. Clique em ações de pontuação
    - Tenha em atenção cidade valor está agora na memória do bot.
    - 'Meteorologia no $city é provavelmente sunny' está agora disponível como uma resposta. 
6. Selecione 'Meteorologia no $city é provavelmente sunny'.

Vamos supor que o utilizador introduz 'repeat que'. 
1. Escreva que e introduza. Tenha em atenção que entidade cidade e o respetivo valor na memória e disponível.
2. Selecione 'Meteorologia no $city é provavelmente sunny'.

![](../media/tutorial3_entities.PNG)

Agora criar uma entidade e com a etiqueta de instâncias do mesmo nas mensagens do utilizador.  Também utilizou a presença ausência da entidade na memória do bot para controlar quando as ações disponíveis, através de campos de entidades necessários e a ação disqualifying.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Entidade esperado](./4-expected-entity.md)
