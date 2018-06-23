---
title: Como utilizar a chamada de retorno de deteção de entidade com uma aplicação de conversação Learner - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como utilizar a chamada de retorno de deteção de entidade com uma aplicação de conversação Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: e41ea5930ff0c8395d0c93aa42e224ebfc894ba8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354020"
---
# <a name="how-to-use-entity-detection-callback"></a>Como utilizar a chamada de retorno de deteção de entidade

Este tutorial mostra a chamada de retorno de deteção de entidade e ilustra um padrão comum para resolução de entidades.

## <a name="requirements"></a>Requisitos
Este tutorial, necessita que o bot "tutorialEntityDetectionCallback" está em execução.

    npm run tutorial-entity-detection

## <a name="details"></a>Detalhes
Chamada de retorno de deteção de entidade permite utilizando código personalizado para processar regras de negócio relacionadas com a entidades. Esta demonstração, utilizaremos as chamadas de retorno e entidades programática para resolver o nome de cidade introduzido pelo utilizador para um nome canónico – por exemplo, "a grande apple" para "Nova Iorque" a resolver.

### <a name="open-the-demo"></a>Abra a demonstração

Na lista de aplicações, clique no Tutorial-10-EntityDetenctionCallback. 

### <a name="entities"></a>Entidades

Definiu três entidades na aplicação.

![](../media/tutorial10_entities.PNG)

1. Cidade é uma entidade personalizada que o utilizador irá fornecer como entrada de texto.
2. CityUnknown é uma entidade programática. Isto irá obter preenchido pelo sistema. A autorização copiará a intervenção do utilizador se o sistema não souber qual cidade é.
3. CityResolved é a cidade conhecer o sistema. Este será o nome canónico da cidade por exemplo resolverá 'a apple grande' para 'Nova Iorque'.

### <a name="actions"></a>Ações

Foi criado três ações. 

![](../media/tutorial10_actions.PNG)

1. A primeira ação for "que cidade pretende?"
2. A segunda ' posso não souber este cidade, $CityUknown. Que cidade pretende? "
3. O terceiro é 'disse $City e posso resolver que $CityResolved.'

### <a name="callback-code"></a>Código de chamada de retorno

Vamos ver o código. Pode encontrar o método EntityDetectionCallback na unidade c:\<installedpath > \src\demos\tutorialSessionCallbacks.ts ficheiro.

![](../media/tutorial10_callbackcode.PNG)

Esta função é chamada após a resolução de entidades.
 
- A primeira coisa que executará é $CityUknown encriptado. $CityUknown só irá manter para uma única vez como-la sempre obtém limpo no início.
- Em seguida, vamos obter a lista de cidades ter foi reconhecido. Tirar o primeiro e tente resolvê-lo.
- A função que resolve-(resolveCity) está definida mais acima no código. Tem uma lista de nomes de cidade canónico. Encontrar o nome de cidade na lista, devolve a mesma. Caso contrário, procura no 'cityMap' e devolve o nome mapeado. Se este não é possível localizar uma cidade, devolverá um nulo.
- Por fim, se a cidade tem resolvido para um nome, armazenamos-$CityKnown entidade. Caso contrário, limpe o que o utilizador tem consiga aceder tal e preencher $CityUknown entidade.

### <a name="train-dialogs"></a>Caixas de diálogo de formação

1. Clique em caixas de diálogo de formação, caixa de diálogo de formação, em seguida, novo.
2. Escreva "Olá".
3. Clique em ações de pontuação e selecione 'que cidade pretende?'
2. Introduza 'Nova Iorque'.
    - Tenha em atenção que obtém reconhecida como uma entidade de cidade.
5. Clique em ações de pontuação
    - Tenha em atenção que cidade e CityResolved foram preenchidas.
6. Selecione 'disse $City e o que posso resolvido para $CityResolved'.
7. Clique em concluído de ensino.

Outro exemplo caixa de diálogo adicione:

1. Clique em nova caixa de diálogo de formação.
2. Escreva "Olá".
3. Clique em ações de pontuação e selecione 'que cidade pretende?'
2. Introduza 'apple grande'.
    - Tenha em atenção que obtém reconhecida como uma entidade de cidade.
5. Clique em ações de pontuação
    - Tenha em atenção que CityResolved mostra o efeito de código em execução.
6. Selecione 'disse $City e o que posso resolvido para $CityResolved'.
7. Clique em concluído de ensino.

Outro exemplo caixa de diálogo adicione:

1. Clique em nova caixa de diálogo de formação.
2. Escreva "Olá".
3. Clique em ações de pontuação e selecione 'que cidade pretende?'
2. Introduza "foo".
    - Este é um exemplo de uma cidade que não souber o sistema. 
5. Clique em ações de pontuação
6. Selecione "não sei este cidade, $CityUknown. Que cidade pretende?'.
7. Introduza 'Nova Iorque'.
8. Clique em ações de pontuação.
    - Tenha em atenção que CityUknown foi limpo e CityResolved é preenchido.
6. Selecione 'disse $City e o que posso resolvido para $CityResolved'.
7. Clique em concluído de ensino.

![](../media/tutorial10_bigapple.PNG)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Chamadas de retorno de sessão](./11-session-callbacks.md)
