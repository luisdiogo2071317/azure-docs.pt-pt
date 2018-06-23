---
title: Como adicionar entidades previamente concebidas para uma aplicação de conversação Learner - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como adicionar entidades previamente concebidas para uma aplicação de conversação Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: f014464419bfac39a9e57e679fcd28a737e9ebdb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355088"
---
# <a name="how-to-add-pre-built-entities"></a>Como adicionar entidades pré-criadas
Este tutorial mostra como adicionar entidades "criadas previamente" a sua aplicação Learner de conversação.

## <a name="requirements"></a>Requisitos
Este tutorial, necessita que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes

Entidades pré-criadas reconhecem tipos comuns de entidades, como números, datas, quantidades monetários e outras pessoas.  Ao contrário das entidades personalizadas, estes funcionam "out-of-a-box" e não requerem qualquer formação.  Ao contrário das entidades personalizadas, não é possível alterar os respetivos comportamento.  Por predefinição, as entidades pré-criadas com múltiplos valores - ou seja, a memória a bot serão acumuladas cada instância da entidade identificada.

## <a name="steps"></a>Passos

### <a name="create-the-application"></a>Criar a aplicação

1. Na IU da Web, clique em nova aplicação
2. No nome, introduza BuiltInEntities. Em seguida, clique em criar.

### <a name="create-an-entity"></a>Criar uma entidade

1. Clique em entidades, em seguida, nova entidade.
2. Clique no EntityType lista pendente e selecione datetimev2.
    - Opções de programável e Negatable estão desativadas, porque estes não se aplicam a pré-criar entidades.
3. Clique em Criar.

![](../media/tutorial7_entities.PNG)

### <a name="create-two-actions"></a>Criar duas ações

1. Clique em ações, em seguida, nova ação
2. Em resposta, escreva ' a data é $luis-datetimev2'.
3. Clique em Criar.

![](../media/tutorial7_actions.PNG)

Em seguida, crie a segunda ação:

1. Clique em ações, em seguida, nova ação para criar uma segunda ação.
3. Em resposta, escreva 'O que é a data?'.
4. Na Disqualifying entidades, introduza 'luis datetimev2'.
4. Clique em Criar

![](../media/tutorial7_actions2.PNG)

Tem agora duas ações.

### <a name="train-the-bot"></a>Preparar o bot

1. Clique em caixas de diálogo de formação, caixa de diálogo de formação, em seguida, novo.
2. Escreva "Olá".
3. Clique em ações de pontuação e selecione 'O que é a data'?
2. Introduza "hoje". 
    - Aviso de hoje em dia é etiquetado e aparece na segunda linha, uma vez que é uma entidade previamente concebida e não editáveis.
5. Clique em ações de pontuação
    - Tenha em atenção de que a data agora é apresentada na secção de memória de entidade. 
    - Se o rato sobre a data, irá ver os dados adicionais fornecidos pelo LUIS, que é utilizado e pode ser manipulado adicional no código. 
6. Selecione ' a data é $luis-datetimev2'.
7. Clique em concluído ensino

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Entradas alternativas](./8-alternative-inputs.md)
