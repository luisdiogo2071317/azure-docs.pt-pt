---
title: Como criar um modelo de aprendiz de conversação "Hello World" - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como criar um modelo de aprendiz de conversação "Hello World".
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 70b8f25bd699cbdb069892d65bf766ef3953f59d
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170876"
---
# <a name="how-to-create-a-hello-world-model-with-conversation-learner"></a>Como criar um modelo de "Hello World" com Aprendiz de conversação

Este tutorial mostra como começar com o aprendiz de conversação, incluindo a criação de ações, ensinando interativamente e fazer correções de caixas de diálogo registadas dos utilizadores finais.

## <a name="video"></a>Vídeo

[![Pré-visualização do tutorial 1](http://aka.ms/cl-tutorial-01-preview)](http://aka.ms/blis-tutorial-01)


## <a name="requirements"></a>Requisitos
Se ainda não o fez, certifique-se primeiro todos os passos de configuração foram concluídos, incluindo a criação de um `.env` ficheiro com a sua chave de criação de LUIS.  Ver [guia de introdução](https://github.com/Microsoft/ConversationLearner-Samples) para obter detalhes.

Este tutorial requer que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="steps"></a>Passos

Comece na home page na IU da Web.

### <a name="create-the-model"></a>Criar o modelo
1. Clique em novo modelo
2. No campo nome, digite Hello World
3. Clique em Criar

### <a name="create-an-action"></a>Criar uma ação

1. Clique no modelo de Hello World para iniciá-lo
2. Clique em ações, em seguida, nova ação
    - Uma ação pode ser uma mensagem de texto que Aprendiz de conversação retorna para o utilizador, uma chamada à API ou um cartão.
3. Em resposta, escreva "Hello World!"
    - Esta é a resposta que retornará o bot
4. Clique em Criar

Criar a primeira coisa que o bot pode fazer, ou seja, devolver uma resposta de texto.

### <a name="train-the-bot"></a>Preparar o bot

#### <a name="create-the-first-dialog"></a>Criar a primeira caixa de diálogo

1. Clique em caixas de diálogo do Train, em seguida, a nova caixa de diálogo do Train
2. Introduza um exemplo de como o que o utilizador será apresentada a mensagem no começando da conversa, por exemplo, "hello".
3. Clique em ações de pontuação
4. Selecione "Hello World!"
    - Esta ação cria uma caixa de diálogo de exemplo de folheio de um. 
2. Introduza "até breve"
3. Clique em ações de pontuação
4. Clique em Adicionar ação, em seguida, introduza "Goodbye"! em resposta, em seguida, clique em "Criar"
5. Clique em concluído ensino. Isto vai terminar esta caixa de diálogo de treinamento.

Agora tem uma caixa de diálogo de ensino no sistema.

#### <a name="continue-teaching-the-bot"></a>Continuar a ensinar o bot
Vamos fazer um treinamento mais e ver como o bot responde.

1. Clique em nova caixa de diálogo preparar
2. Introduza "Olá lá"
    - Isto é semelhante à primeira caixa de diálogo, e podemos esperar obter uma boa pontuação de bot.
2. Clique em ação de pontuação
    - A posição e a pontuação poderá ainda não ser suficientemente preciso e exigir ensino adicional.
3. Clique em Selecionar junto a "Hello World!"
4. Em seguida, introduza "até breve"
5. Clique em ações de pontuação
6. Selecione "Goodbye"!
7. Clique em concluído ensino

![](../media/tutorial1_actions.PNG)

Vamos fazer outra sessão de ensino para ver como o bot está a funcionar.

Repita os passos acima utilizando "Olá" e "byebye" e observe as alterações na posição e a pontuação da resposta de bots, quando clica em ação de pontuação.

Agora pode repetir os passos com "howdy" e "até breve" e tenha em atenção que as melhorias de mostra classificação no classificações que indicam o bot aprendeu esta interação.

![](../media/tutorial1_dialogs.PNG)

### <a name="test-the-bot-as-an-end-user"></a>Testar o bot como um utilizador final

1. Clique em caixas de diálogo de registo, em seguida, nova caixa de diálogo de registo
2. Escreva "hello lá"
3. Em seguida, "até breve"

Também pode tentar iniciar uma conversa com "até breve" e observe a resposta do bot.

### <a name="view-conversations-in-the-log-dialogs"></a>Vista de conversas nas caixas de diálogo de registo

Nas caixas de diálogo de registo, pode ver a lista de conversas, atualizar e guardar as interações como caixas de diálogo de treinamento. Para fazê-lo:

1. Clique no registo de uma conversa
2. Se estiver correto a conversa, clique na ação última p. ex. "Goodbye".
3. Clique para selecionar a resposta sugerida. 
    - Também pode selecionar ou adicionar outra ação.
4. Em seguida, clique em concluído para guardar isso como uma caixa de diálogo de treinamento.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Aguarde e não espera ações](./2-wait-vs-nonwait-actions.md)