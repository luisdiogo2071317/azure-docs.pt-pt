---
title: Como utilizar o retorno de chamada de detecção de entidade com um modelo de aprendiz de conversação - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como utilizar o retorno de chamada de detecção de entidade com um modelo de aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: e9a3b0a2be58313266b949b907e4eb49a318a6dc
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51260079"
---
# <a name="how-to-use-entity-detection-callback"></a>Como utilizar o retorno de chamada de detecção de entidade

Este tutorial mostra o retorno de chamada de detecção de entidade e ilustra um padrão comum para a resolução de entidades.

## <a name="video"></a>Vídeo

[![Pré-visualização do tutorial 10](https://aka.ms/cl-tutorial-10-preview)](https://aka.ms/blis-tutorial-10)

## <a name="requirements"></a>Requisitos
Este tutorial requer que o `tutorialEntityDetectionCallback` bot está em execução.

    npm run tutorial-entity-detection

## <a name="details"></a>Detalhes
Retorno de chamada de detecção de entidade permite o uso do código personalizado para lidar com regras de negócios relacionados com entidades. Esta demonstração usa retornos de chamada e entidades programática para resolver o nome de cidade inserido pelo usuário para um nome canônico – por exemplo, resolver "a grande apple" para "new york".

### <a name="open-the-demo"></a>Abra a demonstração

Na lista de modelos, clique em 10-Tutorial-EntityDetectionCallback. 

### <a name="entities"></a>Entidades

Três entidades são definidas no modelo.

![](../media/tutorial10_entities.PNG)

1. Cidade é uma entidade personalizada que o utilizador irá fornecer como entrada de texto.
2. CityUnknown é uma entidade programática. Esta entidade irá obter preenchida pelo sistema. Ele copiará a entrada do usuário se o sistema não sabe qual cidade é.
3. CityResolved é cidade em que o sistema sabe sobre. Esta entidade será o nome canônico da cidade por exemplo "o grande apple" será resolvido para "Lisboa".

### <a name="actions"></a>Ações

Três ações são definidas no modelo.

![](../media/tutorial10_actions.PNG)

1. A primeira ação for "que cidade deseja?"
2. O segundo é "não sei que esse cidade, $CityUknown. Cidade do qual deseja? "
3. O terceiro é "disse $City e resolvi que para $CityResolved."

### <a name="callback-code"></a>Código de retorno de chamada

Vamos examinar o código. Pode encontrar o método EntityDetectionCallback na unidade c:\<installedpath > ficheiro \src\demos\tutorialSessionCallbacks.ts.

![](../media/tutorial10_callbackcode.PNG)

Esta função é chamada após a ocorrência de resolução de entidades.
 
- A primeira coisa que ele fará é $CityUknown clara. $CityUknown persistirá somente durante uma única vez, ele sempre obtém limpo no início.
- Em seguida, obter a lista de cidades que ter sido reconhecido. Tire o primeiro e tentar resolvê-lo.
- A função que resolve-(resolveCity) está definida mais acima no código. Ele tem uma lista de nomes de cidade canônico. Encontrar o nome de cidade na lista, ele retorna-lo. Caso contrário, ele procura na 'cityMap' e devolve o nome mapeado. Se não for possível encontrar uma cidade, ele retorna nulo.
- Por fim, se a cidade resolveu para um nome, armazenamos na entidade $CityKnown. Caso contrário, desmarque o que o usuário disse e preencher $CityUknown entidade.

### <a name="train-dialogs"></a>Preparar as caixas de diálogo

1. Clique em caixas de diálogo do Train, em seguida, nova caixa de diálogo de comboio.
2. Escreva "hello".
3. Clique em ações de pontuação e selecione "que cidade deseja?"
2. Introduza "new york".
    - O texto é reconhecido como uma entidade de cidade.
5. Clique em ações de pontuação
    - `City` e `CityResolved` foram preenchidas.
6. Selecione "disse $City e resolvi que para $CityResolved".
7. Clique em concluído ensino.

Adicione outra caixa de diálogo de exemplo:

1. Clique em nova caixa de diálogo de comboio.
2. Escreva "hello".
3. Clique em ações de pontuação e selecione "que cidade deseja?"
2. Introduza "apple grande".
    - O texto é reconhecido como uma entidade de cidade.
5. Clique em ações de pontuação
    - `CityResolved` Mostra o efeito de código em execução.
6. Selecione "disse $City e resolvi que para $CityResolved".
7. Clique em concluído ensino.

Adicione outra caixa de diálogo de exemplo:

1. Clique em nova caixa de diálogo de comboio.
2. Escreva "hello".
3. Clique em ações de pontuação e selecione "que cidade deseja?"
2. Introduza "foo".
    - Este é um exemplo de uma cidade, que o sistema não sabe. 
5. Clique em ações de pontuação
6. Selecione "não sei que esse cidade, $CityUknown. Cidade do qual deseja? ".
7. Introduza "new york".
8. Clique em ações de pontuação.
    - `CityUknown` foi limpo, e `CityResolved` é preenchido.
6. Selecione "disse $City e resolvi que para $CityResolved".
7. Clique em concluído ensino.

![](../media/tutorial10_bigapple.PNG)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Retornos de chamada de sessão](./11-session-callbacks.md)
