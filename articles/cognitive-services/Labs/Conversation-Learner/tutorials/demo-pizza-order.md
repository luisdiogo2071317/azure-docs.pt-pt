---
title: Modelo de aprendiz de conversação de demonstração, a ordem de pizza - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como criar um modelo de aprendiz de conversação de demonstração.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: e23ff60a0a2ea10ace09130ba115e72b4e1c9ad7
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51249817"
---
# <a name="demo-pizza-order"></a>Demonstração: Ordem de Pizza
Essa demonstração ilustra uma bot de pedido de pizza. Ele oferece suporte a ordenação de uma única pizza com esta funcionalidade:

- RECONHECENDO sabores de pizza em expressões de utilizador
- a verificar se sabores de pizza são em estoque ou fora de estoque e responder adequadamente
- memorizar sabores de pizza de um pedido anterior e oferta para - iniciar uma nova encomenda com os mesmo sabores

## <a name="video"></a>Vídeo

[![Pré-visualização de Pizza de demonstração](https://aka.ms/cl-demo-pizza-preview)](https://aka.ms/blis-demo-pizza)

## <a name="requirements"></a>Requisitos
Este tutorial requer que o bot de ordem de pizza está em execução

    npm run demo-pizza

### <a name="open-the-demo"></a>Abra a demonstração

Na lista de modelo da IU da web, clique no pedido de Pizza TutorialDemo. 

## <a name="entities"></a>Entidades

Criou três entidades.

- Sabores: esta entidade serão acumuladas sabores pedida ao utilizador. Ele inclui os sabores válidos que estão em estoque. Ele verifica se um sabor é em ou fora de estoque.
- OutofStock: esta entidade é utilizada para comunicar ao usuário que os ingredientes selecionado não está em estoque.
- LastToppings: quando uma ordem seja feita, esta entidade é usada para oferecer ao usuário a lista de sabores em sua ordem.

![](../media/tutorial_pizza_entities.PNG)

### <a name="actions"></a>Ações

Criou um conjunto de ações, incluindo a pedir ao utilizador, o que eles querem em seus pizza, informar sobre o que eles adicionaram até aqui, e assim por diante.

Há também duas chamadas de API:

- FinalizeOrder: para fazer o pedido para a pizza
- UseLastToppings: migrar os sabores de ordem anterior 

![](../media/tutorial_pizza_actions.PNG)

### <a name="training-dialogs"></a>Caixas de diálogo de treinamento
Definiu um punhado de caixas de diálogo de treinamento. 

![](../media/tutorial_pizza_dialogs.PNG)

Por exemplo, vamos tentar uma sessão de ensino.

1. Clique em caixas de diálogo do Train, em seguida, nova caixa de diálogo de comboio.
1. Introduza 'order uma pizza'.
2. Clique em ação de pontuação.
3. Clique para selecionar "o que deseja no seu pizza?"
4. Introduza 'cresce e aprecia o queijo'.
    - Observe que o LUIS tem o nome ambas como sabores. Se não tiver sido correto, que poderia clique para realçar e corrigi-lo.
    - O sinal "+" junto a entidade significa que está a ser adicionado ao conjunto de sabores.
5. Clique em ações de pontuação.
    - Tenha em atenção `mushrooms` e `cheese` não estão na memória para sabores.
3. Clique para selecionar 'tiver $Toppings no seu pizza'
    - Observe que se trata de uma ação de não-espera, para que o bot irá pedir para a próxima ação.
6. Selecione "Gostaria que qualquer outra coisa?"
7. Introduza "Remover cresce e adicionar peppers".
    - Tenha em atenção `mushroom` tem um "-" início de sessão, junto ao mesmo, para que ele seja removido. E `peppers` tem um sinal "+" junto ao mesmo, adicioná-lo para os sabores.
2. Clique em ação de pontuação.
    - Tenha em atenção `peppers` está agora em negrito, pois há de novo. E `mushrooms` foi ultrapassado.
8. Clique para selecionar 'tiver $Toppings no seu pizza'
6. Selecione "Gostaria que qualquer outra coisa?"
7. Introduza "Adicionar peas".
    - `Peas` é um exemplo de um sabor que está fora de estoque. Ele ainda tem o nome como um sabor.
2. Clique em ação de pontuação.
    - `Peas` aparece como OutOfStock.
    - Para ver como isso aconteceu, abra o código em `C:\<\installedpath>\src\demos\demoPizzaOrder.ts`. Ver o método EntityDetectionCallback. Este método é chamado após cada sabor para ver se está em estoque. Caso contrário, ele limpa-lo a partir do conjunto de sabores e adiciona à entidade OutOfStock. A variável inStock é definida acima desse método que tem a lista de sabores de estoque.
6. Selecione 'Não temos $OutOfStock'.
7. Selecione "Gostaria que qualquer outra coisa?"
8. Introduza "não".
9. Clique em ação de pontuação.
10. Selecione a chamada de "FinalizeOrder" API. 
    - Isso chamará a função de 'FinalizeOrder' definida no código. Isto limpa sabores e retorna 'seu pedido está a caminho'. 
2. Introduza 'order outro'. Estamos a começar um novo pedido.
9. Clique em ação de pontuação.
    - 'queijo' e 'peppers' estão na memória como sabores da ordem de última.
1. Selecione 'Gostaria que $LastToppings'.
2. Introduza "Sim"
3. Clique em ação de pontuação.
    - O bot quer tome as medidas de UseLastToppings. Esta é a segunda dos dois métodos de retorno de chamada. Ele irá copiar sabores a última ordem para sabores e limpar sabores último. Essa é uma maneira de memorizar a última ordem e, se o usuário disser que desejam pizza outro, fornecendo esses sabores como opções.
2. Clique para selecionar 'tiver $Toppings no seu pizza'.
3. Selecione "Gostaria que qualquer outra coisa?"
8. Introduza "não".
4. Clique em concluído ensino.

![](../media/tutorial_pizza_callbackcode.PNG)

![](../media/tutorial_pizza_apicalls.PNG)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Demonstração - iniciador de aplicações VR](./demo-vr-app-launcher.md)
