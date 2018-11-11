---
title: Como iniciar sessão caixas de diálogo num modelo de aprendiz de conversação - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como iniciar sessão caixas de diálogo num modelo de aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 5369e16e0f1adc48eb019f3790dc900577c144af
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51243664"
---
# <a name="how-to-log-dialogs-in-a-conversation-learner-model"></a>Como iniciar sessão caixas de diálogo num modelo de aprendiz de conversação

Este tutorial mostra como fazer o utilizador final de teste dentro da interface do Aprendiz de conversação; como caixas de diálogo são registadas; e como fazer correções a sessão iniciada caixas de diálogo para melhorar o seu modelo.

## <a name="video"></a>Vídeo

[![Pré-visualização do tutorial 9](https://aka.ms/cl-tutorial-09-preview)](https://aka.ms/blis-tutorial-09)

## <a name="requirements"></a>Requisitos
Este tutorial requer que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes
Pode usar as caixas de diálogo de registo para rever e efetuar as correções para as caixas de diálogo realizadas com os utilizadores finais.  Especificamente, pode corrigir as etiquetas de entidade e seleções de ação para melhorar o desempenho do modelo preparado e geral do sistema. 

## <a name="steps"></a>Passos

### <a name="create-the-model"></a>Criar o modelo

1. Na IU da Web, clique em novo modelo
2. No nome, digite LogDialogs. Em seguida, clique em criar.

### <a name="create-an-entity"></a>Criar uma entidade

1. Clique em entidades, em seguida, nova entidade.
2. Introduza nome da entidade, cidade.
3. Clique em Criar.

### <a name="create-two-actions"></a>Criar duas ações

1. Clique em ações, em seguida, nova ação
2. Em resposta, escreva "Qual cidade?".
3. Nas entidades Disqualifying, introduza $city.
3. Clique em Criar

Em seguida, crie a segunda ação:

1. Clique em ações, em seguida, nova ação.
3. Em resposta, escreva "o clima em $city é provavelmente ensolarado".
4. Entidades necessárias, introduza $city.
4. Clique em Criar.

Tem agora duas ações.

### <a name="train-the-bot"></a>Preparar o bot

1. Clique em caixas de diálogo do Train, em seguida, nova caixa de diálogo de comboio.
2. Escreva 'o que é a meteorologia'.
3. Clique em ações de pontuação e selecione "Qual cidade?"
2. Introduza "Lisboa".
3. Faça duplo clique em "Seattle" e selecione a cidade.
    - Isto marca-o como uma entidade de cidade.
5. Clique em ações de pontuação
6. Selecione "o clima em $city é provavelmente ensolarado".
7. Clique em concluído ensino.

Adicione outra caixa de diálogo de exemplo:

1. Clique em Train caixas de diálogo e, em seguida, nova caixa de diálogo de comboio.
2. Escreva "o que é o clima em Seattle?". Observe que Seattle é identificado como uma entidade.
5. Clique em ações de pontuação 
6. Selecione "o clima em $city é provavelmente ensolarado".
7. Clique em concluído ensino.

### <a name="try-the-bot-as-the-user"></a>Experimente o bot como o utilizador
Vamos imaginar que Implementámos este bot aos utilizadores.

1. Clique em caixas de diálogo de registo.
2. Clique em nova caixa de diálogo de registo.
    - Isso apresenta o bot, conforme o usuário teria Experimente-o no controle de bate-papo da web do lado esquerdo da interface do Usuário. Pode ignorar a área de espaço em branco à direita.
3. Escreva "hello".
4. Resposta de bot: "qual cidade?"
4. Tipo de "Boston".
5. Resposta de bot: "qual cidade?"
    - Isso não parece correto. Então, vamos guardar essa caixa de diálogo.
2. Clique em teste concluído.

Vamos começar uma nova sessão:

2. Clique em nova caixa de diálogo de registo.
3. Tipo de "previsão de Boston".
4. Resposta de bot: "qual cidade?"
2. Clique em teste concluído.

Agora vamos fazer correções para a segunda caixa de diálogo:

1. Clique em "previsão para Boston" em caixas de diálogo de registo.
    - Esta ação abre a conversa.
    - Se clicar no lado do usuário da conversa (aqui, no "previsão para Boston"), pode alterar as etiquetas de entidade.
    - Se clicar no lado do sistema (aqui, no "qual cidade"), pode alterar a ação está selecionada.
5. Clique em "previsão para Boston". 
    - A causa principal aqui é que o Boston não foi marcado como uma entidade. Precisamos alterar isso.
    - Faça duplo clique em "Boston" e, em seguida, selecione a cidade.
    - Clique em submeter as alterações e clique em Guardar. Isto irá criar uma caixa de diálogo de treinamento com base nas alterações que efetuou e soltar em caixas de diálogo de treinamento no ponto da alteração que fez.
6. Selecione "o clima em $city é provavelmente ensolarado".
7. Clique em concluído ensino. Se for para caixas de diálogo Train agora, verá que é adicionada a nova ação.

![](../media/tutorial9_logdiag1.PNG)

Agora vamos fazer correções para a outra caixa de diálogo:

1. Clique em "hello" em caixas de diálogo de registo.
    - Esta ação abre a conversa.
3. A resposta "Olá" é "qual cidade". Mas, queremos alterar isso para algo que faz mais sentido. Uma resposta melhor seria algo como 'Olá! eu sou o bot de Meteorologia'. Mas não existe nenhuma ação que faz isso, portanto, temos de criar uma.
4. Clique em ação.
    - Em resposta, escreva "eu sou o bot de Meteorologia. Posso pode ajudar com as previsões. "
6. Desmarque a espera para a caixa de verificação de resposta para torná-lo uma ação de não-espera.
7. Clique em Criar.
8. Em seguida, clique para selecionar esta ação de novo. Em seguida, clique em Guardar.
    - Isso nos leva de volta a esse ponto da sessão de treinamento.
6. Clique para selecionar "qual cidade?"
7. Tipo de "Boston". Faça duplo clique à marca Boston como uma entidade se não o tiver feito.
8. Clique em ações de pontuação.
9. Clique para selecionar "o clima em $city é provavelmente ensolarado".
10. Clique em concluído ensino.

![](../media/tutorial9_addnewaction.PNG)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Retorno de chamada de detecção de entidade](./10-entity-detection-callback.md)
