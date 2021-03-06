---
title: Como utilizar entradas alternativas com Aprendiz de conversação - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como utilizar entradas alternativas com Aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: c60dc2ca93547b93ce2ee457393570479069c899
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55216273"
---
# <a name="how-to-use-alternative-inputs"></a>Como utilizar entradas alternativas

Este tutorial mostra como utilizar o campo de entradas de alternativa para expressões de usuário na interface de ensino.

## <a name="video"></a>Vídeo

[![Pré-visualização de Tutorial do entradas alternativo](https://aka.ms/cl_Tutorial_v3_AlternativeInputs_Preview)](https://aka.ms/cl_Tutorial_v3_AlternativeInputs)

## <a name="requirements"></a>Requisitos
Este tutorial requer que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes
Entradas de alternativas são expressões de utilizador alternativas, semanticamente equivalentes que o utilizador possa ter dito num momento específico numa caixa de diálogo de treinamento. Estas entradas alternativas permitem-lhe mais compactly especificar variações de expressões sem ter de resolver cada variação nas caixas de diálogo de treinamento separados.

## <a name="steps"></a>Passos

### <a name="create-the-model"></a>Criar o modelo

1. Na IU da Web, clique em "Novo modelo."
2. No campo "Nome", escreva "AlternativeInputs" e prima enter.
3. Clique no botão "Criar".

### <a name="entity-creation"></a>Criação de entidade

1. No painel esquerdo, clique em "Entidades", em seguida, no botão "Nova entidade".
2. Selecione "Treinados personalizada" para o "tipo de entidade".
3. Tipo de "city" para o "nome da entidade".
4. Clique no botão "Criar".

Agora, vamos criar três ações.

### <a name="create-the-first-action"></a>Criar a primeira ação

1. No painel esquerdo, clique em "Ações", em seguida, no botão "Nova ação".
2. No "do Bot de resposta...", digite "Qual cidade?"
3. No campo de "Entidade esperado na resposta do utilizador...", escreva "Cidade".
4. No campo "Dá direito Disqualifying", escreva "Cidade".
5. Clique no botão "Criar".

### <a name="create-the-second-action"></a>Criar a segunda ação

1. No painel esquerdo, clique em "Ações", em seguida, no botão "Nova ação".
2. No "do Bot de resposta...", digite "o clima em $city é provavelmente ensolarado".
3. Clique no botão "Criar".

### <a name="create-the-third-action"></a>Criar a ação de terceiro

1. No painel esquerdo, clique em "Ações", em seguida, no botão "Nova ação".
2. No "do Bot de resposta...", digite "Experimente perguntar-se para a meteorologia."
3. No campo "Dá direito Disqualifying", escreva "Cidade".
4. Clique no botão "Criar".

Agora tem três ações.

### <a name="train-the-model"></a>Preparar o modelo

1. No painel esquerdo, clique em "Caixas de diálogo Train", em seguida, no botão "Train caixa de diálogo Novo".
2. No painel de bate-papo, em que se lê "... sua mensagem de tipo", escreva "Qual é a meteorologia?"
3. Clique no botão "Pontuação ações".
4. Selecione a resposta, "Qual cidade?"
5. No painel de bate-papo, em que se lê "... sua mensagem de tipo", escreva "Denver"
6. Clique no botão "Pontuação ações".
7. Selecione a resposta, "o clima em Denver é provavelmente ensolarado".
8. Clique no botão "Guardar".

Vamos-preparar o modelo mais através da criação de outra caixa de diálogo de comboio.

### <a name="second-model-train-dialog"></a>A caixa de diálogo do segundo modelo Train

1. No painel esquerdo, clique em "Caixas de diálogo Train", em seguida, no botão "Train caixa de diálogo Novo".
2. No painel de bate-papo, em que se lê "... sua mensagem de tipo", escreva "O que pode fazer?"
3. Clique no botão "Pontuação ações".
4. Selecione a resposta, "Tente solicitar a meteorologia".
5. No painel de bate-papo, em que se lê "... sua mensagem de tipo", escreva "Qual é o clima em Seattle?"
6. Clique em "Seattle", em seguida, clique em "Cidade" na lista de entidades.
7. Clique no botão "Pontuação ações".
8. Selecione a resposta, "o clima em Seattle é provavelmente ensolarado".
9. Clique no botão "Guardar".

### <a name="third-model-train-dialog-using-alternative-input"></a>Terceiro modelo Train caixa de diálogo usar alternativo de entrada

1. No painel esquerdo, clique em "Caixas de diálogo Train", em seguida, no botão "Train caixa de diálogo Novo".
2. No painel de bate-papo, em que se lê "Escreva a mensagem...", escreva "ajuda"
3. Clique no botão "Pontuação ações".
    - O modelo é incerto para a melhor opção, para que ele deve escolher o percentil mais elevado por predefinição.
4. Clique em botão "Abandonar ensino", em seguida, o botão "Confirmar".

![](../media/tutorial8_closescores.png)

Vamos otimizar melhor o sistema através de entradas alternativas. Pode adicionar entrada alternativa, enquanto lecionar ou posterior.

5. No painel esquerdo, clique em "Caixas de diálogo Train", em seguida, selecione "O que pode fazer?" na lista de caixas de diálogo de comboio.
6. Clique em do "o que pode fazer?" expressão no painel de bate-papo.
7. O "Adicionar entrada alternativa..." campo, tipo "ajuda" e prima introduzir.
8. Clique no botão "Guardar alterações".

![](../media/tutorial8_helpalternates.png)

Vamos adicionar outra entrada alternativa para lidar com Houston.

9. Clique a actualização de mensagem "o que é o clima em Seattle?" expressão no painel de bate-papo.
10. No campo de "Adicionar alternativo input...", tipo de "previsão para Houston" e prima enter.
    - Os destaques de mensagem de erro as entradas de alternativas de fato tem de ser equivalente semanticamente e contêm as mesmas entidades como a expressão original; não apenas os mesmos valores de entidades. A presença das mesmas entidades é necessária.
11. Clique em "Houston" e selecione "Cidade" na lista de entidades.
12. No campo de "Adicionar alternativo input...", tipo de "previsão para Seattle" e prima enter.
13. Clique em "Seattle" e selecione "Cidade" na lista de entidades.
14. Clique no botão "Guardar alterações".
15. Clique no botão "Editar guardar".

### <a name="testing-the-model"></a>Testar o modelo

1. No painel esquerdo, clique em "Caixas de diálogo Log", em seguida, a "registo caixa de diálogo novo."
2. No painel de bate-papo, em que se lê "... sua mensagem de tipo", escreva "me ajudar a"
3. No painel de bate-papo, em que se lê "Escreva a mensagem...", escreva "previsão para Denver"

![](../media/tutorial8_altcities.png)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Caixas de diálogo de registo](./11-log-dialogs.md)
