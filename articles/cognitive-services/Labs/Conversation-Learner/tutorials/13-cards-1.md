---
title: Como utilizar cartões com um modelo de aprendiz de conversação, parte 1 - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como utilizar cartões com um modelo de aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 988a2433f098f41bca4796299825293efd4de44b
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171143"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Como utilizar cartões (parte 1 de 2)

Este tutorial mostra como adicionar e utilizar um cartão de simple no seu bot.

> [!NOTE]
> Aprendiz de conversação espera, atualmente, os arquivos de definição de cartão estejam localizadas num diretório chamado "cartões", que está presente no diretório onde o bot é iniciado. Faremos isso configurável no futuro.

## <a name="video"></a>Vídeo

[![Pré-visualização do tutorial 13](http://aka.ms/cl-tutorial-13-preview)](http://aka.ms/blis-tutorial-13)

## <a name="requirements"></a>Requisitos
Este tutorial requer que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes

Os cartões são elementos de IU que permitem ao usuário selecionar uma opção na conversação. 

### <a name="open-the-demo"></a>Abra a demonstração

Na lista de modelo da IU da web, clique no Tutorial-13-cartões-1. 

### <a name="the-card"></a>O cartão

A definição de cartão está na seguinte localização: c:\<installedpath\>\src\cards\prompt.json.

O sistema espera encontrar as definições de cartão neste diretório de cartões.

![](../media/tutorial13_prompt.PNG)

> [!NOTE]
> Tenha em atenção o tipo de corpo `TextBlock` e o `{{question}}` marcador de posição no campo de texto.
> Existem dois submeter botões e o texto que é enviado para cada um.

### <a name="actions"></a>Ações

Criámos três ações. Como pode ver abaixo, a primeira ação é um cartão.

![](../media/tutorial13_actions.PNG)

Vamos ver como o tipo de ação do cartão foi criado:

![](../media/tutorial13_cardaction.PNG)

> [!NOTE]
> A entrada de pergunta e botões 1 e 2. Esses são referências de modelo no cartão onde introduzir a pergunta e as respetivas respostas. Também pode referenciar e utilizar entidades ou uma combinação de texto e entidades.

O ícone do olho mostra-lhe o aspeto de cartão.

### <a name="train-dialog"></a>Caixa de diálogo preparar

Vamos guiá-lo por meio de uma caixa de diálogo de ensino.

1. Clique em caixas de diálogo do Train, em seguida, nova caixa de diálogo de comboio.
1. Introduza "Olá".
2. Clique em ação de pontuação.
3. Clique para selecionar 'Pedido go esquerda ou direita'.
    - Clicar em "esquerda" ou "direita" é equivalente ao usuário digitando "esquerda" ou "certo", respectivamente. 
4. Clique em ações de pontuação.
4. Clique para selecionar "esquerda". Esta é uma ação de não-espera.
6. Clique para selecionar 'Pedido go esquerda ou direita'.
4. Clique em "certo".
5. Clique em ações de pontuação.
3. Clique para selecionar 'Direita'
6. Clique para selecionar 'Pedido go esquerda ou direita'.
4. Clique em teste concluído.

Agora já viu como funcionam os cartões. São definidos no diretório cartões como modelos json. Os modelos irão descobrir na interface de Usuário que pode preencher com uma cadeia de caracteres ou uma entidade ou uma combinação de ambos.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Cartões parte 2](./14-cards-2.md)
