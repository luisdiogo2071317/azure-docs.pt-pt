---
title: Como criar uma aplicação "Olá mundo" conversação Learner - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como criar uma aplicação Learner de conversação "Olá mundo".
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 30026285ac6dda45d2f5e3718aae741b928cf242
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354098"
---
# <a name="how-to-create-a-hello-world-application-with-conversation-learner"></a>Como criar uma aplicação "Olá mundo" com conversação Learner

Este tutorial mostra como começar com conversação Learner, incluindo a criação de ações, teaching interativamente e efetuar correções de caixas de diálogo registadas dos utilizadores finais.

## <a name="requirements"></a>Requisitos
Se ainda não o fez, certifique-se primeiro todos os passos de configuração foram concluídos, incluindo a criação de um `.env` ficheiro com a sua chave de criação de LUIS.  Consulte [início rápido](https://github.com/Microsoft/ConversationLearner-Samples) para obter mais detalhes.

Este tutorial, necessita que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="steps"></a>Passos

Iniciar na home page na IU da Web.

### <a name="create-the-app"></a>Criar a aplicação
1. Clique em nova aplicação
2. No campo do nome, introduza Olá, mundo
3. Clique em Criar

### <a name="create-an-action"></a>Criar uma ação

1. Clique na aplicação Olá, mundo para o iniciar
2. Clique em ações, em seguida, nova ação
    - Uma ação pode ser uma mensagem de texto conversação Learner devolve o utilizador, uma chamada à API ou um cartão.
3. Em resposta, escreva "Olá, mundo!"
    - Esta é a resposta que o bot irá devolver
4. Clique em Criar

Criar a primeira coisa que o bot pode fazê-lo ou seja, devolver uma resposta de texto.

### <a name="train-the-bot"></a>Preparar o bot

#### <a name="create-the-first-dialog"></a>Criar a caixa de diálogo primeiro

1. Clique em caixas de diálogo de formação, em seguida, nova caixa de diálogo de formação
2. Introduza um exemplo de que o utilizador irá indicar na begging da conversação, por exemplo, "Olá".
3. Clique em ações de pontuação
4. Selecione "Olá, mundo!"
    - Esta ação cria uma caixa de diálogo de exemplo de uma vez. 
2. Introduza "goodbye"
3. Clique em ações de pontuação
4. Clique em Adicionar ação, em seguida, introduza 'Goodbye'! em resposta, em seguida, clique em "Criar"
5. Clique em concluído de ensino. Isto vai terminar esta caixa de diálogo de formação.

Tem agora uma caixa de diálogo de ensino no sistema.

#### <a name="continue-teaching-the-bot"></a>Continuar teaching o bot
Vamos não formação mais um e ver como o bot responde.

1. Clique em nova caixa de diálogo de formação
2. Introduza ' Olá existe '
    - Isto é semelhante à caixa de diálogo primeiro e, esperamos obter uma boa pontuação do bot.
2. Clique em ação de pontuação
    - A posição e o modelo de pontuação não ainda poderão ser suficientemente exato e necessitam de ensino adicional.
3. Clique em Selecionar junto a "Olá, mundo!"
4. Em seguida, introduza 'bye'
5. Clique em ações de pontuação
6. Selecione 'Goodbye'!
7. Clique em concluído ensino

![](../media/tutorial1_actions.PNG)

Será efetuado outra sessão de ensino para ver como o bot está a funcionar.

Repita os passos acima utilizando 'Olá' e 'byebye' e tenha em atenção que as alterações na posição e pontuação da resposta bots quando clicar em ação de pontuação.

Pode repetir os passos com 'howdy' e "good bye" e tenha em atenção que as melhorias de mostra classificação no pontuações que indica o bot tem aprendidas esta interação do utilizador.

![](../media/tutorial1_dialogs.PNG)

### <a name="test-the-bot-as-an-end-user"></a>Testar o bot como um utilizador final

1. Clique em caixas de diálogo de registo, em seguida, nova caixa de diálogo de registo
2. Tipo 'Olá existe'
3. Em seguida, 'bye'

Também pode tentar iniciar uma conversação com 'bye' e tenha em atenção a resposta a bot.

### <a name="view-conversations-in-the-log-dialogs"></a>Vista conversações diálogos de registo

Nas caixas de diálogo de registo, pode ver a lista de conversações, atualizar e guardar as interações como caixas de diálogo de formação. Para fazê-lo:

1. Clique no registo de uma conversação
2. Se a conversação procura boa, clique na ação último por exemplo "Goodbye".
3. Clique para selecionar a resposta sugerida. 
    - Também pode selecionar ou adicionar outra ação.
4. Em seguida, clique em concluído para guardar esta como uma caixa de diálogo de formação.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Aguarde e não espera ações](./2-wait-vs-nonwait-actions.md)