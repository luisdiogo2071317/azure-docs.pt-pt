---
title: Como utilizar entidades Negatable com um modelo de aprendiz de conversação - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como utilizar entidades Negatable com um modelo de aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: fea950e2c13d9b5ce0c3619990961e611edd6626
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55207382"
---
# <a name="how-to-use-negatable-entities-with-a-conversation-learner-model"></a>Como utilizar entidades Negatable com um modelo de aprendiz de conversação

Este tutorial demonstra a propriedade de "Negatable" de entidades.

## <a name="video"></a>Vídeo

[![Pré-visualização de Tutorial do negatable entidades](https://aka.ms/cl_Tutorial_v3_NegatableEntities_Preview)](https://aka.ms/cl_Tutorial_v3_NegatableEntities)

## <a name="requirements"></a>Requisitos
Este tutorial requer que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes
A propriedade de "Negatable" de uma entidade permite-lhe rotular tanto normal (positivo) e negativos instâncias da entidade, ensinar com base em modelos positivos e negativos e limpar o valor de uma entidade existente. Entidades com seu conjunto de propriedade de "Negatable" são denominadas entidades Negatable na conversação Leaner.

## <a name="steps"></a>Passos

### <a name="create-the-model"></a>Criar o modelo

1. Na IU da Web, clique em "Novo modelo."
2. No campo "Nome", escreva "NegatableEntity" e prima enter.
3. Clique no botão "Criar".

### <a name="entity-creation"></a>Criação de entidade

1. No painel esquerdo, clique em "Entidades", em seguida, no botão "Nova entidade".
2. Selecione "Personalizada" para o "tipo de entidade".
3. Escreva "nome" para o "nome da entidade".
4. Verifique a caixa de verificação "Negatable".
    - A verificar essa propriedade permite ao utilizador para fornecer um valor de entidade ou que é algo *não* um valor de entidade. No último caso, o resultado é o valor de entidade correspondente a eliminação.
5. Clique no botão "Criar".

![](../media/tutorial5_entities.PNG)

### <a name="create-the-first-action"></a>Criar a primeira ação

1. No painel esquerdo, clique em "Ações", em seguida, no botão "Nova ação".
2. No "do Bot de resposta...", digite "Não sei seu nome."
3. No campo de "Dá direito Disqualifying", escreva "name".
4. Clique no botão "Criar".

### <a name="create-the-second-action"></a>Criar a segunda ação

1. No painel esquerdo, clique em "Ações", em seguida, no botão "Nova ação".
2. No "do Bot de resposta...", digite "sei seu nome. É $name."
3. Clique no botão "Criar".

> [!NOTE]
> O "nome" entidade foi adicionado automaticamente como um "necessárias entidades" por referência na resposta.

Agora tem duas ações.

![](../media/tutorial5_actions.PNG)

### <a name="train-the-model"></a>Preparar o modelo

1. No painel esquerdo, clique em "Caixas de diálogo Train", em seguida, no botão "Train caixa de diálogo Novo".
2. No painel de bate-papo, em que se lê "... sua mensagem de tipo", escreva "hello".
3. Clique no botão "Pontuação ações".
4. Selecione a resposta, "Não sei seu nome."
    - O percentil é 100%, como a ação só é válida com base nas restrições.
5. No painel de bate-papo, em que se lê "... sua mensagem de tipo", escreva "o meu nome é o Frank"
6. Selecione "Francisco" e selecione a etiqueta "+ name"
    - Existem duas instâncias para o "nome" entidade: "+ name" e "-name".  (+) Além disso, adiciona ou substitui o valor. (-) Sinal de subtração remove o valor.
7. Clique no botão "Pontuação ações".
    - O "nome" entidade está agora definido como "Francisco" na memória do modelo, por isso, o "sei seu nome. Ação é $name"está disponível.
8. Selecione a resposta, "sei seu nome. É $name."
9. No painel de bate-papo, em que se lê "... sua mensagem de tipo", escreva "meu nome não é Frank."
10. Selecione "Francisco" e selecione a etiqueta "-name"
    - Selecionamos "-name" para limpar o valor atual da entidade.
11. Clique no botão "Pontuação ações".
12. Selecione a resposta, "Não sei seu nome."
13. No painel de bate-papo, em que se lê "... sua mensagem de tipo", escreva "o meu nome é Susan".
14. Selecione "Susan" e selecione a etiqueta "+ name"

![](../media/tutorial5_dialogs.PNG)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Entidades de valores múltiplos](./07-multi-value-entities.md)
