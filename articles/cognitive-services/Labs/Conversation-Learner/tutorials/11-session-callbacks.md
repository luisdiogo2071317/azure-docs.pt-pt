---
title: Como utilizar chamadas de retorno de sessão com uma aplicação de conversação Learner - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como utilizar chamadas de retorno de sessão com uma aplicação de conversação Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: f8970620c1f0f87ccae13d031092a048144ffb19
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354019"
---
# <a name="how-to-use-session-callbacks-with-a-conversation-learner-application"></a>Como utilizar chamadas de retorno de sessão com uma aplicação de conversação Learner

Este tutorial ilustra as chamadas de retorno onSessionStart e onSessionEnd.

## <a name="requirements"></a>Requisitos
Este tutorial, necessita que o bot "tutorialSessionCallbacks.ts" está em execução.

    npm run tutorial-session-callbacks

## <a name="details"></a>Detalhes
Este tutorial abrange o conceito de uma sessão, como as sessões são processadas por predefinição e como pode substituir este comportamento.

Uma sessão é uma conversação com o bot. Pode ter vários activa, mas não existem nenhum quebras longas na conversação (por exemplo, 30 minutos).  Consulte a página de ajuda "Limites" para a duração de tempo limite de sessão predefinida.

Se existirem quebras de linha de tempo, em seguida, o bot passa a sua próxima sessão de.  Iniciar uma nova sessão coloca a rede neuronal periódica no respetivo estado inicial.  Por predefinição, este limpa todos os valores de entidade, apesar deste comportamento pode ser alterado (ilustradas abaixo).

### <a name="open-the-demo"></a>Abra a demonstração

Na lista de aplicações, clique no Tutorial-11-SessionCallbacks. 

### <a name="entities"></a>Entidades

Definiu quatro entidades na aplicação.

![](../media/tutorial11_entities.PNG)

Um aspeto a ter em atenção está BotName uma entidade programática.  Isto será definido pelo bot na hora de início de sessão.

### <a name="actions"></a>Ações

Foi criado quatro ações. 

![](../media/tutorial11_actions.PNG)

Em primeiro lugar, este tutorial irá mostrar como controlar valores de entidade no início de sessão – por exemplo, definir a entidade de BotName antes do utilizador indica nada.

Segundo, este tutorial irá mostrar como manter os valores de uma sessão para o seguinte.  Neste tutorial, partimos do nome de utilizador e o número de telefone permanecerem os mesmos a partir de uma sessão para o seguinte, mas que poderá alterar a respetiva localização.  Iremos manter, por conseguinte, número de nome e o telefone entre sessões, mas a localização do utilizador limpar.

### <a name="train-dialog"></a>Caixa de diálogo de formação

Segue-se uma caixa de diálogo de exemplo. Esta é uma sessão - ou seja, existem não longas quebras de linha nesta caixa de diálogo.

![](../media/tutorial11_traindialog.PNG)

### <a name="code-for-the-callbacks"></a>Código para as chamadas de retorno

O código para os métodos de chamada de retorno consta do ficheiro: c:\<installedpath > \src\demos\tutorialSessionCallbacks.ts.

![](../media/tutorial11_code.PNG)

Ambos estes métodos são opcionais.

- OnSessionStartCallback: Este método define a entidade de BotName.
- OnSessionEndCallback: pode especificar que pretende apagar. Isto irá eliminar todas as entidades, exceto o nome de utilizador e o telefone do utilizador.

### <a name="try-the-bot"></a>Repita o bot

Mudar para a IU da Web e clique em diálogos de registo.

1. Introduza "Olá".
2. System: "Olá, estou Botty. O que é o nome da sua? " que tem a provenha de Botty de nome do OnSessionStartCallback.
3. Introduza 'jason'.
4. System: ' Hi jason. O que é o número de telefone? "
5. Introduza ' 555-555-5555'.
6. System: 'pode saber Botty a localização, jason?"
7. Escreva "Redmond".

Esta é uma sessão. Para iniciar uma nova sessão, precisamos de terminar esta sessão. 

1. Clique em tempo limite da sessão. Isto irá mover para a sessão seguinte.
    - O botão "Tempo limite da sessão" é fornecido para fins de depuração.  Numa sessão real, uma pausa longa seria necessária a ocorrência de aproximadamente 30 minutos.  Consulte a página de ajuda "Limites" para a duração de tempo limite de sessão predefinida.
1. Introduza "Olá".
2. System: 'pode saber Botty a localização, jason?"
    - O sistema tem memorizadas o número de nome e o telefone.
2. Introduza uma nova localização: 'Seattle'.
3. System: 'por isso, jason estiver em Seattle'.
4. Clique em testar efectuada.

Vamos mude novamente para diálogos de registo. Tenha em atenção que a conversação último tem dividida em duas porque cada caixa de diálogo de registo corresponde a uma sessão.  

![](../media/tutorial11_splitdialogs.PNG)

- Interação primeiro, Botty está definido, mas o número de nome e o telefone não são.
- A segunda interação mostra o número de nome e o telefone.

Constatou agora como sessões são processadas por predefinição, e como pode substituir o comportamento predefinido. 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Chamadas de API](./12-api-calls.md)
