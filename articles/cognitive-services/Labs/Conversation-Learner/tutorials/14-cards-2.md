---
title: Como utilizar cartões com um modelo de aprendiz de conversação, parte 2 - serviços cognitivos da Microsoft | Documentos da Microsoft
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
ms.openlocfilehash: 99c474978d4da1d80669505330b2dc6220d7ca5f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51226639"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Como utilizar cartões (parte 1 de 2)
Este tutorial mostra como adicionar um cartão de formulário fillable ao seu bot. Esta operação irá mostrar como campos de formulário mover em entidades.

Aprendiz de conversação espera que seus arquivos de definição de cartão estejam localizadas num diretório chamado "cartões", que está presente no diretório onde o bot é iniciado.

## <a name="video"></a>Vídeo

[![Pré-visualização do tutorial 14](https://aka.ms/cl-tutorial-14-preview)](https://aka.ms/blis-tutorial-14)

## <a name="requirements"></a>Requisitos
Este tutorial requer que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes

Os cartões são elementos de IU que permitem ao usuário selecionar uma opção na conversação. 

### <a name="open-the-demo"></a>Abra a demonstração

Na lista de modelo da IU da web, clique no Tutorial-14-cartões-2. 

### <a name="the-card"></a>O cartão

A definição de cartão está na seguinte localização: c:\<installedpath\>\src\cards\shippingAddress.json.

Este cartão recolhe três campos de endereço de envio: Cidade e estado rua.

![](../media/tutorial14_card.PNG)

### <a name="actions"></a>Ações

Criámos três ações. Como pode ver abaixo, a primeira ação é um cartão.

![](../media/tutorial14_actions.PNG)

Vamos ver como o tipo de ação do cartão foi criado:

- Tenha em atenção Rua endereço, onde o tipo é de Input.text e sua ID.
- Da mesma forma, não há endereço-cidade e um menu pendente com o ID de-estado de endereço.

Os Ids são importantes, como quando os campos são preenchidos e submetidos, esses são os nomes de entidade que receberão esses valores no bot.

## <a name="entities"></a>Entidades
Definimos as três entidades que correspondem o cartão como vimos anteriormente.

![](../media/tutorial14_entities.PNG)

## <a name="actions"></a>Ações

Definimos duas ações.

![](../media/tutorial14_actions.PNG)

- A primeira é o envio cartão de endereço em que o tipo de ação é o cartão e o modelo for selecionado na lista pendente como shippingAddress.
- A segunda é uma ação simple para ler o endereço de envio.

![](../media/tutorial14_sa_card.PNG)

### <a name="train-dialog"></a>Caixa de diálogo preparar

Vamos guiá-lo por meio de uma caixa de diálogo de ensino.

1. Clique em caixas de diálogo do Train, em seguida, nova caixa de diálogo de comboio.
1. Introduza "Olá".
2. Clique em ação de pontuação.
3. Clique para selecionar o endereço para remessa.
4. Preencha o cartão e submeter.
    - Observe que esses valores agora foram movidos para a memória de entidade. Análise não é necessária porque o formulário particionado já as entradas.
5. Clique em ações de pontuação.
3. Clique para selecionar ' de envio para $Address... ".
4. Clique em teste concluído.

![](../media/tutorial14_train_dialog.PNG)

Agora viu como obter valores do cartão que tenha fillable campos e listas pendentes, e capturar e recolhê-los em entidades de bot.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Ramificação e desfazer](./15-branching-and-undo.md)
