---
title: Como utilizar cartões com uma aplicação de conversação Learner, parte 1 - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como utilizar cartões com uma aplicação de conversação Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: e90ccd42b21eea6139c402937be7e20513d73c84
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353983"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Como utilizar cartões (parte 1 de 2)

Este tutorial mostra como adicionar e utilizar um cartão simple no seu bot.

Tenha em atenção que a conversação Learner espera os cartão ficheiros de definição para estar localizado num diretório chamado "cartões" que está presente no diretório onde o bot é iniciado.

## <a name="requirements"></a>Requisitos
Este tutorial, necessita que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes

Cartões são elementos de IU que permitem ao utilizador selecionar uma opção na conversação. 

### <a name="open-the-demo"></a>Abra a demonstração

Na lista de aplicações a IU da web, clique no Tutorial-13-cartões-1. 

### <a name="the-card"></a>O cartão

A definição do cartão é, na seguinte localização: c:\<installedpath\>\src\cards\prompt.json.

O sistema de espera localizar as definições de cartão neste diretório cartões.

![](../media/tutorial13_prompt.PNG)

- Tenha em atenção o TextBlock e o modelo de pergunta.
- Existem dois submeter botões e o texto que obtém submetido para cada.

### <a name="actions"></a>Ações

Foi criado três ações. Como ver abaixo, a primeira ação é um cartão.

![](../media/tutorial13_actions.PNG)

Vamos ver como o tipo de ação do cartão foi criado:

![](../media/tutorial13_cardaction.PNG)

Tenha em atenção que a entrada de pergunta e botões 1 e 2. Trata-se as referências de modelo no cartão onde introduziu a questão e as respetivas respostas. Também pode referenciar e utilizar entidades ou uma combinação de texto e as entidades.

O ícone de olho mostra-lhe aspeto o cartão.

### <a name="train-dialog"></a>Caixa de diálogo de formação

Vamos guiá-lo através de uma caixa de diálogo de ensino.

1. Clique em caixas de diálogo de formação, caixa de diálogo de formação, em seguida, novo.
1. Introduza "Olá".
2. Clique em ação de pontuação.
3. Clique para selecionar 'Pedida aceda à esquerda ou direita'.
    - Ao clicar em 'esquerda' ou 'direita' é equivalente ao utilizador escrever em 'à esquerda' ou 'à direita' respetivamente. 
4. Clique em ações de pontuação.
4. Clique para esquerda' Select'. Esta é uma ação não espera.
6. Clique para selecionar 'Pedida aceda à esquerda ou direita'.
4. Clique em 'à direita'.
5. Clique em ações de pontuação.
3. Clique para seleccionar 'Direita'
6. Clique para selecionar 'Pedida aceda à esquerda ou direita'.
4. Clique em testar efectuada.

Constatou agora como funcionam os cartões. São definidos no diretório de cartões como modelos json. Os modelos irão superfície na IU do qual pode preencher a utilizar uma cadeia ou uma entidade ou uma combinação de ambos.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Parte de cartões 2](./14-cards-2.md)
