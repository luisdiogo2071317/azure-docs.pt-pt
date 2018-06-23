---
title: Demonstração de aplicação de conversação Learner ordem pizza - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como criar uma aplicação de conversação Learner de demonstração.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 99cd89c4f4430f2d65ed0963e3092d51a83842d7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354122"
---
# <a name="demo-pizza-order"></a>Demonstração: Ordem de Pizza
Esta demonstração ilustra uma pizza ordenação bot. Suporta a ordenação de um único pizza com esta funcionalidade:

- reconhecer toppings pizza no utterances de utilizador
- a verificar se pizza toppings são no gráfico de cotações ou fora do gráfico de cotações e adequadamente a responder
- memorizar toppings pizza de uma ordem anterior e ao - iniciar uma nova ordem com os mesmos toppings da oferta

## <a name="requirements"></a>Requisitos
Este tutorial, necessita que o bot de ordem pizza está em execução

    npm run demo-pizza

### <a name="open-the-demo"></a>Abra a demonstração

Na lista de aplicações a IU da web, clique em TutorialDemo Pizza ordem. 

## <a name="entities"></a>Entidades

Foi criado três entidades.

- Toppings: serão acumuladas toppings pedido ao utilizador para. Inclui os toppings válidos que se encontrem no gráfico de cotações. Verifica se um topping está dentro ou fora do gráfico de cotações.
- OutofStock: Isto é utilizado para comunicar novamente para o utilizador que os respetivos topping selecionado não se encontra no gráfico de cotações.
- LastToppings: depois de uma ordem é colocada, esta entidade é utilizada para oferecer ao utilizador a lista de toppings na respetiva ordem.

![](../media/tutorial_pizza_entities.PNG)

### <a name="actions"></a>Ações

Foi criado um conjunto de ações pedir ao utilizador que pretende no seu pizza, informar sobre o que que tenha adicionado até ao momento, etc.

Também existem dois chamadas de API:

- FinalizeOrder: colocar a ordem para a pizza
- UseLastToppings: migrar os toppings de ordem anterior 

![](../media/tutorial_pizza_actions.PNG)

### <a name="training-dialogs"></a>Caixas de diálogo de formação
Definiu alguns a caixas de diálogo de formação. 

![](../media/tutorial_pizza_dialogs.PNG)

Por exemplo, vamos tentar uma sessão de ensino.

1. Clique em caixas de diálogo de formação, caixa de diálogo de formação, em seguida, novo.
1. Introduza um pizza de ordem.
2. Clique em ação de pontuação.
3. Clique para selecionar "que gostaria no seu pizza?"
4. Introduza 'mushrooms e cheese'.
    - Tenha em atenção LUIS tem etiquetadas ambos como Toppings. Se não estava correto, que foi clique para realçar e corrigi-lo.
    - O sinal '+' junto a entidade significa que está a ser adicionado ao conjunto de toppings.
5. Clique em ações de pontuação.
    - Tenha em atenção mushrooms e cheese não estão na memória para Toppings.
3. Clique para seleccionar 'tiver $Toppings no seu pizza'
    - Tenha em atenção de que este é uma ação não espera, pelo que o bot pedirá para a ação seguinte.
6. Selecione "Gostaria há mais alguma coisa?"
7. Introduza 'Remover mushrooms e adicionar peppers'.
    - Tenha em atenção **mushroom** tem um '-' sessão junto ao mesmo para o mesmo seja removida. E peppers tem '+' para o adicionar ao toppings.
2. Clique em ação de pontuação.
    - Tenha em atenção **peppers** está agora em negrito dado que há de novo. E **mushrooms** foi ultrapassado.
8. Clique para seleccionar 'tiver $Toppings no seu pizza'
6. Selecione "Gostaria há mais alguma coisa?"
7. Introduza 'Adicionar peas'.
    - Peas são um exemplo de um topping que está fora do gráfico de cotações. Tenha em atenção de que ainda é denominado como um topping.
2. Clique em ação de pontuação.
    - Peas aparecem como OutOfStock.
    - Para ver como isto aconteceu, vamos abrir o código no c:\<\installedpath > \src\demos\demoPizzaOrder.ts. E anote o método EntityDetectionCallback. Este método é denominado após cada topping para ver se está no gráfico de cotações. Caso contrário, é limpa-lo do conjunto de toppings e adiciona à entidade OutOfStock. A variável inStock está definida acima esse método de que tem a lista de toppings no gráfico de cotações.
6. Selecione 'Não temos $OutOfStock'.
7. Selecione "Gostaria há mais alguma coisa?"
8. Introduza 'Nenhum'.
9. Clique em ação de pontuação.
10. Selecione a chamada de 'FinalizeOrder' API. 
    - Isto irá chamar a função de 'FinalizeOrder' definida no código. Isto limpa toppings e devolve 'a ordem é o caminho'. 
2. Introduza 'order outro'. Iremos estiver a iniciar uma nova ordem.
9. Clique em ação de pontuação.
    - Tenha em atenção cheese e peppers são na memória do que toppings da última ordem.
1. Selecione 'Gostaria $LastToppings'.
2. Introduza 'yes'
3. Clique em ação de pontuação.
    - O bot pretende executar a ação de UseLastToppings. Este é o segundo os dois métodos de chamada de retorno. Este irá copiar toppings a ordem último para toppings e limpar toppings último. Esta é uma forma de memorizar a último ordem e se o utilizador indica que pretendem pizza outro, fornecer esses toppings como opções.
2. Clique para seleccionar 'tiver $Toppings no seu pizza'.
3. Selecione "Gostaria há mais alguma coisa?"
8. Introduza 'Nenhum'.
4. Clique em concluído de ensino.

![](../media/tutorial_pizza_callbackcode.PNG)

![](../media/tutorial_pizza_apicalls.PNG)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Demonstração - iniciador da aplicação VR](./demo-vr-app-launcher.md)
