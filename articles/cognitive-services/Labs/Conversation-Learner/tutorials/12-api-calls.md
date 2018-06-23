---
title: Como utilizar a API de utilização chama-se com uma aplicação de conversação Learner - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como utilizar chamadas de API de utilização com uma aplicação de conversação Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: ec752cbadfac7a47e08ed7b0ffe8bb475969fac5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354037"
---
# <a name="how-to-add-api-calls-to-a-conversation-learner-application"></a>Como adicionar chamadas da API para uma aplicação de conversação Learner

Este tutorial mostra como adicionar chamadas da API para a aplicação. Chamadas de API são as funções que definir e escrever no seu bot, e que pode chamar Learner conversação.

## <a name="requirements"></a>Requisitos
Este tutorial, necessita que o bot "tutorialAPICalls.ts" está em execução.

    npm run tutorial-api-calls

## <a name="details"></a>Detalhes

- Chamadas à API podem ler e manipular entidades.
- As chamadas de API têm acesso para o objecto do Gestor de memória.
- Chamadas de API também podem tirar argumentos – Isto permite reutilizar a mesma chamada de API para servir diferentes fins.

### <a name="open-the-demo"></a>Abra a demonstração

Na lista de aplicações a IU da web, clique no Tutorial-12-APICalls. 

### <a name="entities"></a>Entidades

Definiu uma entidade na aplicação designada pelo número.

![](../media/tutorial12_entities.PNG)

### <a name="api-calls"></a>Chamadas de API
O código para as chamadas de API está definido na este ficheiro: c:\<installedpath\>\src\demos\tutorialAPICalls.ts.

![](../media/tutorial12_apicalls.PNG)

- A primeira chamada de retorno de API é RandomGreeting. Devolve uma saudação aleatória definida a variável de saudação.
- A chamada de retorno de API multiplicar: será Multiplicar dois números fornecidos pelo utilizador. Em seguida, devolve o resultado da multiplicação de dois números. Isto mostra que chamadas de retorno de API podem demorar entradas. Tenha em atenção que o Gestor de memória é o primeiro argumento. 
- A chamada de retorno de ClearEntities API: limpa a entidade número para permitir que o utilizador introduza o número seguinte. Ilustra a forma como as chamadas de API podem manipular entidades.

### <a name="actions"></a>Ações

Foi criado quatro ações. 

![](../media/tutorial12_actions.PNG)

- Para além de "o número que pretende multiplicar pelo 12?" qual é uma ação communicative, existem três chamadas de API diferentes ilustram os padrões de chamada de API típicos.

- RandomGreeting: é uma ação de não espera. Para definir esta cópia de segurança, a caixa de diálogo de ação de criar, é selecionado o tipo de ação de API_LOCAL e selecionado RandomGreeting. 

![](../media/tutorial12_setupapicall.PNG)

O botão Atualizar junto a API é utilizado se foi parar o bot e efetue as alterações nas APIs. Clicar na atualização seria recolher as alterações mais recentes.

Eis a forma como criámos a multiplicar ação: depois de selecionar API_Local e API, iremos introduzir uma entidade ($number) para o primeiro valor de entrada (num1string) e um valor (12) para o segundo valor de entrada (num2string). Isto fornece um nível de direção entre o bot e a API chama-se, pelo que a mesma chamada de retorno pode ser mapeada para algumas ações no sistema e são diferentes na forma como as ações são atribuídas.

![](../media/tutorial12_actionmultiply.PNG)

### <a name="train-dialog"></a>Caixa de diálogo de formação

Vamos guiá-lo através de uma caixa de diálogo de ensino.

1. Clique em caixas de diálogo de formação, caixa de diálogo de formação, em seguida, novo.
1. Introduza "Olá".
2. Clique em ação de pontuação.
3. Clique para selecionar RandomGreeting. Isto irá executar a chamada de API Greeting aleatório.
3. Clique para seleccionar 'que número e pretende multiplicar pelo 12?'
4. Introduza '8'. Em seguida, clique em ações de pontuação.
4. Selecione ' Multiplicar $number 12'. Tenha em atenção o resultado da multiplicação.
5. Selecione 'Limpar entidades'.
    - Tenha em atenção que o valor de número da entidade foi limpo.
3. Clique para seleccionar 'que número e pretende multiplicar pelo 12?'
4. Clique em testar efectuada.

![](../media/tutorial12_dialog.PNG)

Agora tem visto como registar chamadas de retorno de API, os padrões comuns e como definir argumentos e associar os valores e entidades nos mesmos.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Cartões parte 1](./13-cards-1.md)
