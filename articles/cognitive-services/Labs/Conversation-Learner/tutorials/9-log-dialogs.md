---
title: Como iniciar sessão caixas de diálogo numa aplicação conversação Learner - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como caixas de diálogo de registo numa aplicação Learner conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 477545c48aeca05d56fdae28ac65a8f381a482fe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354115"
---
# <a name="how-to-log-dialogs-in-a-conversation-learner-application"></a>Como iniciar sessão caixas de diálogo numa aplicação conversação Learner

Este tutorial mostra como fazer o utilizador final testar na interface de conversação Learner; como são registadas caixas de diálogo; e como tornar as correções para registadas caixas de diálogo para melhorar o seu modelo.

## <a name="requirements"></a>Requisitos
Este tutorial, necessita que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes
Pode utilizar as caixas de diálogo de registo para rever e efetuar correções para caixas de diálogo efetuadas com os utilizadores finais.  Especificamente, pode corrigir as etiquetas de entidade e as seleções de ação para melhorar o desempenho do modelo treinado e geral do sistema. 

## <a name="steps"></a>Passos

### <a name="create-the-application"></a>Criar a aplicação

1. Na IU da Web, clique em nova aplicação
2. No nome, introduza LogDialogs. Em seguida, clique em criar.

### <a name="create-an-entity"></a>Criar uma entidade

1. Clique em entidades, em seguida, nova entidade.
2. No nome da entidade, introduza cidade.
3. Clique em Criar.

### <a name="create-two-actions"></a>Criar duas ações

1. Clique em ações, em seguida, nova ação
2. Em resposta, escreva 'Que cidade?'.
3. Na Disqualifying entidades, introduza $city.
3. Clique em Criar

Em seguida, crie a segunda ação:

1. Clique em ações, em seguida, ação de novo.
3. Em resposta, escreva 'Meteorologia no $city é provavelmente sunny'.
4. Entidades necessárias, introduza $city.
4. Clique em Criar.

Tem agora duas ações.

### <a name="train-the-bot"></a>Preparar o bot

1. Clique em caixas de diálogo de formação, caixa de diálogo de formação, em seguida, novo.
2. Escreva 'o que é a meteorologia'.
3. Clique em ações de pontuação e selecione 'Que cidade'?
2. Introduza 'Seattle'.
3. Faça duplo clique em "Seattle" e selecione cidade.
    - Isto marca-a como uma entidade de cidade.
5. Clique em ações de pontuação
6. Selecione 'Meteorologia no $city é provavelmente sunny'.
7. Clique em concluído de ensino.

Outro exemplo caixa de diálogo adicione:

1. Clique em nova ação, em seguida, caixa de diálogo de formação novo.
2. Escreva 'o que é a meteorologia em Seattle?'. Tenha em atenção de que Seattle está marcado como uma entidade.
5. Clique em ações de pontuação 
6. Selecione 'Meteorologia no $city é provavelmente sunny'.
7. Clique em concluído de ensino.

### <a name="try-the-bot-as-the-user"></a>Repita o bot como o utilizador
Vamos imaginar que Implementámos este bot aos utilizadores.

1. Clique no registo caixas de diálogo.
2. Clique em nova sessão de Chat.
    - Isto apresenta o bot, como o faria experiência de utilizador-lo no controlo de chat web no lado esquerdo da IU. Pode ignorar a área de espaço em branco à direita.
3. Escreva "Olá".
4. Resposta de bot: 'que cidade'?
4. Tipo de 'Boston'.
5. Resposta de bot: 'que cidade'?
    - Este não parece estar correto. Por isso, vamos guardar esta caixa de diálogo.
2. Clique em testar efectuada.

Vamos começar uma nova sessão:

2. Clique em nova sessão de Chat.
3. Tipo 'previsão para Boston'.
4. Resposta de bot: 'que cidade'?
2. Clique em concluído de ensino.

Agora vamos certificar-nas correções para a caixa de diálogo segundo:

1. Clique em "previsão da população para Boston' em diálogos de registo.
    - Esta ação abre a conversação.
    - Se clicar no lado do utilizador da conversação (aqui em 'previsão para Boston'), pode alterar as etiquetas de entidade.
    - Se clicar no lado do sistema (aqui em 'em que cidade'), pode alterar a ação está selecionada.
5. Clique em 'previsão para Boston'. 
    - A causa raiz aqui é que Boston não foi marcado como uma entidade. É necessário alterar que.
    - Faça duplo clique no 'Boston', em seguida, selecione cidade.
    - Clique em submeter as alterações e clique em Guardar. Isto irá criar uma caixa de diálogo de formação, com base nas alterações que efetuou e drop é para caixas de diálogo de formação at the point of a alteração que efetuou.
6. Selecione 'Meteorologia no $city é provavelmente sunny'.
7. Clique em concluído de ensino. Se passar para formação diálogos agora, verá que é adicionada a ação de novo.

![](../media/tutorial9_logdiag1.PNG)

Agora vamos certificar-nas correções para outra caixa de diálogo:

1. Clique em "Olá" em registo diálogos.
    - Esta ação abre a conversação.
3. Tenha em atenção a resposta a 'Olá' 'em que cidade'. Mas, pretendemos alterar que para algo que faz mais sentido. Uma resposta melhor seria algo como 'Olá, estou a bot Meteorologia'. Mas não existe nenhuma ação que o faz que, pelo que temos de criar um.
4. Clique na ação.
    - Na resposta, escreva ' estou a bot Meteorologia. Posso pode ajudar a previsões.'
6. Verificação de anular a espera para a caixa de verificação de resposta para tornar uma ação não espera.
7. Clique em Criar.
8. Em seguida, clique para selecionar esta ação de novo. Em seguida, clique em Guardar.
    - Isto proporciona volta a esse ponto na sessão formação.
6. Clique para selecionar 'que cidade'?
7. Tipo de 'Boston'. Faça duplo clique à etiqueta Boston como uma entidade se não estiver já.
8. Clique em ações de pontuação.
9. Clique para seleccionar 'Meteorologia no $city é provavelmente sunny'.
10. Clique em concluído de ensino.

![](../media/tutorial9_addnewaction.PNG)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Chamada de retorno de deteção de entidade](./10-entity-detection-callback.md)
