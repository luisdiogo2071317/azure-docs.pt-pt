---
title: Fluxo de controlo de Learner conversação - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba mais sobre o fluxo de controlo de conversação Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 592ca82db7e0ab3ff89d25b61f38f8b226f3bc86
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353935"
---
## <a name="control-flow"></a>Fluxo de controlo

Este documento descreve o fluxo de controlo de conversação Learner (CL), tal como apresentado no diagrama abaixo.

![](media/controlflow.PNG)

1. Utilizador introduzir um termo ou frase no bot, por exemplo, "o que é a meteorologia em Seattle?'
1. CL passa a entrada de utilizador para um modelo de machine learning que extrai entidades
    - Este modelo é compilação por Learner de conversação e alojada por www.luis.ai
2. Qualquer extraídos entidades e o utilizador de entrada de texto, são transmitidos para o método de chamada de retorno de deteção de entidade no código da bot.
    - Este código poderá valores conjunto/desmarque/manipular entidade
1. A rede neuronal CL demora, em seguida, o resultado a extração de entidade e a intervenção do utilizador e pontuações todas as ações definidas no bot
    - Neste exemplo, é a ação de probabilidade mais elevada fornecer a previsão de Meteorologia:

![](media/controlflow_forecast.PNG)

5. A ação selecionada, neste caso, necessita de uma chamada à API para obter a previsão de Meteorologia. 
6. Esta API, que tinha sido registada com o CL. Em seguida, é possível invocar o método de AddAPICallback.  Da devolução do resultado desta API, em seguida, para o utilizador como uma mensagem – por exemplo, 'Sunny com um elevado de 67'.
7. A chamada, em seguida, é efetuada para a rede neuronal para determinar a ação seguinte com base no passo anterior.
8. A rede neuronal prevê, em seguida, o seguinte conjunto de ações possíveis e a ação selecionada não é apresentada ao utilizador, neste caso, "Há mais alguma coisa?"

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Como ensinar com conversação Learner ](./how-to-teach-cl.md)
