---
title: Como adicionar entidades de Pre-Trained para um modelo de aprendiz de conversação - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como adicionar entidades de Pre-trained para um modelo de aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: afa927009e684fa7f8c6217c91dcb589b331b5f5
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55224178"
---
# <a name="how-to-add-pre-trained-entities"></a>Como adicionar entidades de Pre-trained
Este tutorial mostra como adicionar entidades de Pre-Trained ao seu modelo de aprendiz de conversação.

## <a name="video"></a>Vídeo

[![Pré-visualização de Tutorial do entidades com formação prévia](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities_Preview)](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities)

## <a name="requirements"></a>Requisitos
Este tutorial requer que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes

Entidades com formação prévia reconhecem tipos comuns de entidades, como números, datas, quantias monetárias e outras pessoas.  Eles funcionam "out-of-the-box," não requerem qualquer treinamento e seu comportamento não pode ser alterado ao contrário das entidades personalizadas.  Por predefinição, as entidades Pre-Trained são-com múltiplos valores, acumular identificada cada uma das instâncias da entidade.

## <a name="steps"></a>Passos

### <a name="create-the-model"></a>Criar o modelo

1. Na IU da Web, clique em "Novo modelo."
2. No campo "Nome", escreva "PretrainedEntities" e prima enter.
3. Clique no botão "Criar".

### <a name="entity-creation"></a>Criação de entidade

1. No painel esquerdo, clique em "Entidades", em seguida, no botão "Nova entidade".
2. Selecionar "Pré-Trained/datetimeV2" para o "tipo de entidade".
3. Selecione a caixa de verificação "Com múltiplos valor".
    - Entidades de valores múltiplos acumular-se um ou mais valores na entidade.
    - Propriedades negatable estão desativadas para Pre-Trained entidades.
4. Clique no botão "Criar".

![](../media/tutorial7_entities_a.PNG)

### <a name="create-the-first-action"></a>Criar a primeira ação

1. No painel esquerdo, clique em "Ações", em seguida, no botão "Nova ação".
2. No "do Bot de resposta...", digite "a data é $builtin-datetimev2"
3. Clique no botão "Criar".

![](../media/tutorial7_actions_a.PNG)

### <a name="create-the-second-action"></a>Criar a segunda ação

1. No painel esquerdo, clique em "Ações", em seguida, no botão "Nova ação".
2. No "do Bot de resposta...", digite "Qual é a data?"
    - Entidades com formação prévia não podem ser necessário entidades, como eles são reconhecidos por predefinição para todas as expressões de utilizador.
3. No campo "Dá direito Disqualifying", escreva "builtin-datetimev2."
4. Clique no botão "Criar".

![](../media/tutorial7_actions2_a.PNG)

### <a name="train-the-model"></a>Preparar o modelo

1. No painel esquerdo, clique em "Caixas de diálogo Train", em seguida, no botão "Train caixa de diálogo Novo".
2. No painel de bate-papo, em que se lê "... sua mensagem de tipo", escreva "hello".
3. Clique no botão "Pontuação ações".
4. Selecione a resposta, "Qual é a data?"
5. No painel de bate-papo, em que se lê "Tipo de sua mensagem...", escreva "hoje"
    - A expressão de hoje é reconhecida automaticamente por modelos com formação prévia no LUIS.
    - Pairar o rato sobre os valores de entidades de Pre-Trained mostra dados adicionais fornecidos pelo LUIS.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Resoluções de entidade](./09-entity-resolvers.md)
