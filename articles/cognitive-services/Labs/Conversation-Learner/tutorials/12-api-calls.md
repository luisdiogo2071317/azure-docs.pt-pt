---
title: Como utilizar a API chama-se com um modelo de aprendiz de conversação - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Aprenda a usar chamadas de API com um modelo de aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 815d1e9f6d1e4b9937647d55b67665e1b27f501e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51240771"
---
# <a name="how-to-add-api-calls-to-a-conversation-learner-model"></a>Como adicionar chamadas de API para um modelo de aprendiz de conversação

Este tutorial mostra como adicionar chamadas de API para o seu modelo. Chamadas à API são funções que definem e escrever no seu bot, e que pode chamar Aprendiz de conversação.

## <a name="video"></a>Vídeo

[![Pré-visualização do tutorial 12](https://aka.ms/cl-tutorial-12-preview)](https://aka.ms/blis-tutorial-12)

## <a name="requirements"></a>Requisitos
Este tutorial requer que o bot "tutorialAPICalls.ts" está em execução.

    npm run tutorial-api-calls

## <a name="details"></a>Detalhes

- Chamadas de API podem ler e manipular entidades.
- Chamadas de API tem acesso ao objeto de Gestor de memória.
- Chamadas de API também podem conter argumentos – isso permite reutilizar a mesma chamada de API para servir a diferentes finalidades.

### <a name="open-the-demo"></a>Abra a demonstração

Na lista de modelo da IU da web, clique em 12-Tutorial-APICalls. 

### <a name="entities"></a>Entidades

Definimos uma entidade no modelo chamado número.

![](../media/tutorial12_entities.PNG)

### <a name="api-calls"></a>Chamadas de API
O código para as chamadas à API é definido neste ficheiro: c:\<installedpath\>\src\demos\tutorialAPICalls.ts.

![](../media/tutorial12_apicalls.PNG)

- A primeira chamada de retorno de API é RandomGreeting. Ele retorna uma saudação aleatória definida na variável saudação.
- O retorno de chamada de API de multiplicar: será a multiplicar dois números fornecidos pelo usuário. Em seguida, devolve o resultado da multiplicação dos dois números. Isto mostra que os retornos de chamada de API podem demorar entradas. Tenha em atenção que o Gestor de memória é o primeiro argumento. 
- O retorno de chamada de API de ClearEntities: limpa a entidade de número para permitir que o utilizador introduza o número seguinte. Isso ilustra como chamadas de API podem manipular entidades.

### <a name="actions"></a>Ações

Criamos quatro ações. 

![](../media/tutorial12_actions.PNG)

- Para além de "o número que pretende multiplicar por 12?" que é uma ação comunicativas, existem três diferentes chamadas de API que ilustram os padrões de chamada de API típicos.

- RandomGreeting: é uma ação de não-espera. Para definir esta opção, a caixa de diálogo de ação de criar, podemos selecionado o tipo de ação de API_LOCAL, em seguida, selecionado RandomGreeting. 

![](../media/tutorial12_setupapicall.PNG)

No botão Atualizar junto a API é utilizado se estivéssemos parar o bot e fazer alterações às APIs. Clicar em Atualizar, escolheria as alterações mais recentes.

Eis como criámos a multiplicar ação: depois de selecionar API_Local e API, introduziu uma entidade ($number) para o primeiro valor de entrada (num1string) e um valor (12) para o segundo valor de entrada (num2string). Esta opção fornece um nível de indireção entre o bot e a API chame, para que a mesma chamada de retorno pode ser mapeada para algumas ações no sistema e diferem em como as ações são atribuídas.

![](../media/tutorial12_actionmultiply.PNG)

### <a name="train-dialog"></a>Caixa de diálogo preparar

Vamos guiá-lo por meio de uma caixa de diálogo de ensino.

1. Clique em caixas de diálogo do Train, em seguida, nova caixa de diálogo de comboio.
1. Introduza "Olá".
2. Clique em ação de pontuação.
3. Clique para selecionar RandomGreeting. Esta ação irá executar a chamada da API de saudação aleatório.
3. Clique para selecionar "o número de que pretende multiplicar por 12?"
4. Introduza "8". Em seguida, clique em ações de pontuação.
4. Selecione "multiplicar $number 12'. Tenha em atenção o resultado da multiplicação.
5. Selecione "Limpar entidades".
    - O `number` valor da entidade foi limpo.
3. Clique para selecionar "o número de que pretende multiplicar por 12?"
4. Clique em teste concluído.

![](../media/tutorial12_dialog.PNG)

Agora já viu como registar retornos de chamada de API, os padrões comuns e como definir argumentos e associar valores e entidades nos mesmos.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Cartões parte 1](./13-cards-1.md)
