---
title: Demonstração iniciador da conversação Learner realidade de aplicação, virtual aplicação - serviços cognitivos Microsoft | Microsoft Docs
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
ms.openlocfilehash: 3e41125bf7da9ee64d666d22cb275af01af54012
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354073"
---
# <a name="demo-virtual-reality-app-launcher"></a>Demonstração: Iniciador da aplicação Virtual realidade

Esta demonstração ilustra um iniciador da aplicação virtual realidade, comandos de suporte, como "Iniciar Skype e colocada no padrão de fundo." Um utilizador tem de indicar um nome de aplicação e a localização para iniciar a aplicação. Iniciar a aplicação é processada por uma chamada à API. Quando um nome de aplicação é reconhecido do utilizador, o entityDetectionCallback verifica se a aplicação pedida corresponde a uma ou mais aplicações na lista de aplicações instaladas. Processa o caso em que a aplicação pedida não está instalada e em que o nome de aplicação é ambíguo (corresponde a mais do que uma aplicação instalada).

## <a name="requirements"></a>Requisitos

Este tutorial, necessita que o bot VRAppLauncher está em execução

    npm run demo-vrapp

### <a name="open-the-demo"></a>Abra a demonstração

Na lista de aplicações a IU da web, clique em VRAppLauncher. 

## <a name="entities"></a>Entidades

Foi criado quatro entidades:

- AppName: por exemplo Skype
- PlacementLocation: para padrão de fundo de exemplo
- UnknownAppName: uma entidade programática que o sistema define quando não reconhece um nome de entidade utilizador indica, por exemplo porque não foi instalado.
- DisAmbigAppNames: no consiga de uma matriz de dois ou mais nomes de aplicação instalada que correspondem ao que o utilizador aceder tal. 

![](../media/tutorial_vrapplauncher_entities.PNG)

### <a name="actions"></a>Ações

Foi criado um conjunto de ações que inclui uma API chamada LaunchApp, que irá iniciar a chamada de função para iniciar uma aplicação.

![](../media/tutorial_vrapplauncher_actions.PNG)

### <a name="training-dialogs"></a>Caixas de diálogo de formação
Definiu um número de caixas de diálogo de formação.

![](../media/tutorial_vrapplauncher_dialogs.PNG)

Por exemplo, vamos tentar uma sessão de ensino.

1. Clique em caixas de diálogo de formação, caixa de diálogo de formação, em seguida, novo.
1. Introduza "Olá".
2. Clique em ação de pontuação.
3. Clique para selecionar "a aplicação que pretende iniciar?"
4. Introduza 'outlook'.
    - Tenha em atenção que LUIS reconhece-o como uma entidade.
5. Clique em ações de pontuação.
3. Clique para seleccionar 'onde pretende que colocada?'
4. Introduza 'on lateral'.
    - Tenha em atenção que LUIS reconhece-o como um PlacementLocation.
2. Introduza as ações de pontuação.
6. Selecione 'LaunchApp'
7. System: 'a iniciar o outlook no padrão de fundo'.
    - Tenha em atenção que isto acionada uma chamada à API. O código para esta chamada está em c:\<\installedpath > \src\demos\demoVRAppLauncher.ts. Não tem, efetivamente, a lógica para iniciar o outlook para esta demonstração contudo.
    - -Limpa as entidades de AppName e PlacementLocation. O devolve a cadeia acima como resposta.
4. Clique em concluído de ensino.

![](../media/tutorial_vrapplauncher_callbackcode.PNG)

Comecemos outra sessão de formação para processamento de entidades é ambíguas e desconhecidas.

1. Clique em nova caixa de diálogo de formação.
1. Introduza 'Iniciar OneNote'. 
    - -Reconhece-o como um nome de aplicação porque o EntityDetectionCallback definido no guia de código, o utilizador nome entrou e resolve-o como um nome de aplicação por correspondente, a lista de aplicações definida no código. Em seguida, devolve o conjunto de todas as aplicações correspondentes. 
    - Se a lista de correspondências for zero, isto significa que a aplicação não está instalada. Coloca-o no unknownAppName.
    - Se encontrar mais do que uma aplicação, será copie-os para o DisambigAppNames e desmarque a entidade de AppName.
2. Clique em ação de pontuação.
3. Clique para seleccionar 'Lamentamos, mas não souber a aplicação $UknownAppName.'
4. Introduza 'Iniciar amazon'. Iremos irá experimentar o outro caminho.
5. Clique em ações de pontuação.
    - Tenha em atenção Amazon vídeo e Amazon música, já estão na memória DisambigAppNames. E OneNote foi limpo.
3. Clique para selecionar 'Existem algumas aplicações de som assim o desejar que...'
    - Tenha em atenção de que o modelo de pontuação não é muito elevado porque apenas temos algumas caixas de diálogo de formação até este ponto. Parece que seria temos algumas mais aqui para tornar o modelo mais decisive.
2. Introduza as ações de pontuação.
4. Clique em concluído de ensino.

Constatou agora como efetuar a resolução de entidades. Demonstração de também ilustrado chamadas de retorno de API e mostrou um modelo para recolher informações sobre, verificar a presença e ambiguidade e efetuar a ação adequada com base no que.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Implementar um bot conversação Learner](../deploy-to-bf.md)
