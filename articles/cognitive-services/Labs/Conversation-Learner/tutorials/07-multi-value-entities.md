---
title: Como utilizar entidades com múltiplos valores com um modelo de aprendiz de conversação - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como utilizar entidades com múltiplos valores com um modelo de aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 8544d63f38f88a0e623dff343bf8b5133931b70b
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228309"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-model"></a>Como utilizar entidades com múltiplos valores com um modelo de aprendiz de conversação
Este tutorial mostra a propriedade de valor múltiplos de entidades.

## <a name="video"></a>Vídeo

[![Pré-visualização de Tutorial do valor de várias entidades](https://aka.ms/cl_Tutorial_v3_MultiValued_Preview)](https://aka.ms/cl_Tutorial_v3_MultiValued)

## <a name="requirements"></a>Requisitos
Este tutorial requer que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes
Entidades de valores múltiplos acumular-se os valores numa lista, em vez de armazenar um valor único.  Estas entidades são úteis quando os utilizadores podem especificar mais de um valor. Sabores numa pizza, por exemplo.

Entidades marcadas como valores múltiplos terão cada instância reconhecida da entidade anexada a uma lista na memória do Bot. Reconhecimento subsequente acrescenta a entidade valor, em vez de substituir.

## <a name="steps"></a>Passos

### <a name="create-the-model"></a>Criar o modelo

1. Na IU da Web, clique em "Novo modelo."
2. No campo "Nome", escreva "MultiValueEntities" e prima enter.
3. Clique no botão "Criar".

### <a name="entity-creation"></a>Criação de entidade

1. No painel esquerdo, clique em "Entidades", em seguida, no botão "Nova entidade".
2. Selecione "Treinados personalizada" para o "tipo de entidade".
3. Escreva "sabores" para o "nome da entidade".
4. Selecione a caixa de verificação "Com múltiplos valor".
    - Entidades de valores múltiplos acumular-se um ou mais valores na entidade.
5. Verifique a caixa de verificação "Negatable".
    - A propriedade de "Negatable" foi coberta na outro tutorial.
6. Clique no botão "Criar".

![](../media/tutorial6_entities.PNG)

### <a name="create-the-first-action"></a>Criar a primeira ação

1. No painel esquerdo, clique em "Ações", em seguida, no botão "Nova ação".
2. No "do Bot de resposta...", digite "aqui estão os sabores: $toppings"
    - O símbolo de dólar norte-americano à esquerda indica uma referência de entidade
3. Clique no botão "Criar".

### <a name="create-the-second-action"></a>Criar a segunda ação

1. No painel esquerdo, clique em "Ações", em seguida, no botão "Nova ação".
2. No "do Bot de resposta...", digite "quais sabores deseja?"
3. No campo "Dá direito Disqualifying", escreva "sabores."
4. Clique no botão "Criar".

Agora tem duas ações.

![](../media/tutorial6_actions.PNG)

### <a name="train-the-model"></a>Preparar o modelo

1. No painel esquerdo, clique em "Caixas de diálogo Train", em seguida, no botão "Train caixa de diálogo Novo".
2. No painel de bate-papo, em que se lê "... sua mensagem de tipo", escreva "Olá."
3. Clique no botão "Pontuação ações".
4. Selecione a resposta, "quais sabores faria isso como?"
    - O percentil é 100%, como a ação só é válida com base nas restrições.
5. No painel de bate-papo, em que se lê "... sua mensagem de tipo", escreva "queijo e cresce"
6. Clique em "queijo" e selecione a etiqueta "+ sabores"
7. Clique em "cresce" e selecione a etiqueta "+ sabores"
8. Clique no botão "Pontuação ações".
9. Selecione a resposta, "aqui estão os sabores: $toppings"
10. No painel de bate-papo, em que se lê "... sua mensagem de tipo", escreva "Adicionar pepper"
11. Clique em "pepper" e selecione a etiqueta "+ sabores"
12. Clique no botão "Pontuação ações".
13. Selecione a resposta, "aqui estão os sabores: $toppings"
14. No painel de bate-papo, em que se lê "Escreva a mensagem...", escreva "Remover queijo"
15. Clique em "queijo" e selecione a etiqueta "-sabores"
16. Clique no botão "Pontuação ações".
17. Selecione a resposta, "aqui estão os sabores: $toppings"

![](../media/tutorial5_dialogs.PNG)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Entidades com formação prévia](./08-pre-trained-entities.md)
