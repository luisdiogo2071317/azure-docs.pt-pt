---
title: Como utilizar cartões com uma aplicação de conversação Learner, parte 2 - serviços cognitivos Microsoft | Microsoft Docs
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
ms.openlocfilehash: 254f0953fd3e281a35857e69d9795e3decebf45d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353965"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Como utilizar cartões (parte 1 de 2)
Este tutorial mostra como adicionar um cartão de formulário fillable à sua bot. Esta operação irá mostrar como campos de formulário mover para entidades.

Conversação Learner espera que os ficheiros de definição de cartão ser localizados num diretório chamado "cartões", que está presente no diretório onde o bot é iniciado.

## <a name="requirements"></a>Requisitos
Este tutorial, necessita que o bot tutorial geral está em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes

Cartões são elementos de IU que permitem ao utilizador selecionar uma opção na conversação. 

### <a name="open-the-demo"></a>Abra a demonstração

Na lista de aplicações a IU da web, clique no Tutorial-14-cartões-2. 

### <a name="the-card"></a>O cartão

A definição do cartão é, na seguinte localização: c:\<installedpath\>\src\cards\shippingAddress.json.

Este cartão recolhe três campos do endereço de envio: Cidade, rua e estado.

![](../media/tutorial14_card.PNG)

### <a name="actions"></a>Ações

Foi criado três ações. Como ver abaixo, a primeira ação é um cartão.

![](../media/tutorial14_actions.PNG)

Vamos ver como o tipo de ação do cartão foi criado:

- Tenha em atenção Rua endereços, onde o tipo é de Input.text e um ID.
- Da mesma forma, não há cidade de endereço e um menu pendente com o ID de endereço de estado.

Os Ids são importantes, como quando os campos estão preenchidos e submetidos, trata-se os nomes de entidade que irão receber esses valores no bot.

## <a name="entities"></a>Entidades
Definiu três entidades correspondente o cartão como vimos acima.

![](../media/tutorial14_entities.PNG)

## <a name="actions"></a>Ações

Definiu duas ações.

![](../media/tutorial14_actions.PNG)

- O primeiro é o cartão envio de endereço em que o tipo de ação é cartão e o modelo está selecionado na lista pendente como shippingAddress.
- O segundo é uma ação simple para ler novamente o endereço de envio.

![](../media/tutorial14_sa_card.PNG)

### <a name="train-dialog"></a>Caixa de diálogo de formação

Vamos guiá-lo através de uma caixa de diálogo de ensino.

1. Clique em caixas de diálogo de formação, caixa de diálogo de formação, em seguida, novo.
1. Introduza "Olá".
2. Clique em ação de pontuação.
3. Clique para selecionar o endereço de envio.
4. Preencha o cartão e submeter.
    - Tenha em atenção de que esses valores agora terem sido movidos para a memória de entidade. Não é necessária nenhuma análise como o formulário já particionada as entradas.
5. Clique em ações de pontuação.
3. Clique para selecionar ' envio $Address... ".
4. Clique em testar efectuada.

![](../media/tutorial14_train_dialog.PNG)

Constatou como obter os valores do cartão que tenha fillable campos e as dropdowns, e capturar e recolhê-los em entidades de bot agora.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [A ramificação e anular](./15-branching-and-undo.md)
