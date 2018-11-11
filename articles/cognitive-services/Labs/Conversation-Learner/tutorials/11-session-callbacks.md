---
title: Como utilizar os retornos de chamada de sessão com um modelo de aprendiz de conversação - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como utilizar os retornos de chamada de sessão com um modelo de aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 73fb2a3f10476550bbe6bd20c387a1cc2a7727d8
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51246485"
---
# <a name="how-to-use-session-callbacks-with-a-conversation-learner-model"></a>Como utilizar os retornos de chamada de sessão com um modelo de aprendiz de conversação

Este tutorial ilustra os retornos de chamada onSessionStart e onSessionEnd.

## <a name="video"></a>Vídeo

[![Pré-visualização do tutorial 11](https://aka.ms/cl-tutorial-11-preview)](https://aka.ms/blis-tutorial-11)

## <a name="requirements"></a>Requisitos
Este tutorial requer que o `tutorialSessionCallbacks` bot está em execução.

    npm run tutorial-session-callbacks

## <a name="details"></a>Detalhes
Este tutorial abrange o conceito de uma sessão, como as sessões são tratados por predefinição e como pode substituir esse comportamento.

Uma sessão é uma conversa com o bot. Ele pode ter vários folheio, mas não há muito quebras na conversação (por exemplo, de 30 minutos).  Consulte a página de ajuda "Limites" durante o período de tempo limite de sessão predefinido.

Se existirem quebras longas, em seguida, o bot passa a sua próxima sessão.  A partir de uma nova sessão coloca a rede neural periódica em seu estado inicial.  Por predefinição, também limpara todos os valores de entidade, embora esse comportamento pode ser alterado (ilustrada abaixo).

### <a name="open-the-demo"></a>Abra a demonstração

Na lista de modelos, clique em 11-Tutorial-SessionCallbacks. 

### <a name="entities"></a>Entidades

Quatro entidades são definidas no modelo.

![](../media/tutorial11_entities.PNG)

Algo a ser observado é que BotName é uma entidade programática.  Esta entidade será definida pelo bot à hora de início de sessão.

### <a name="actions"></a>Ações

Quatro ações são definidas no modelo.

![](../media/tutorial11_actions.PNG)

Em primeiro lugar, este tutorial mostra como controlar os valores de entidade no início de sessão – por exemplo, definir a entidade de BotName antes do usuário disser algo.

Em segundo lugar, este tutorial mostra como manter os valores de uma sessão para a próxima.  Neste tutorial, presumimos que o nome do utilizador e o número de telefone permanecem os mesmos de uma sessão para a próxima, mas que pode alterar a respetiva localização.  Podemos, portanto, em manter o nome e número de telefone em sessões, mas a localização do utilizador clara.

### <a name="train-dialog"></a>Caixa de diálogo preparar

Aqui está uma caixa de diálogo de exemplo. Esta é uma sessão - ou seja, não há muito quebras nesta caixa de diálogo.

![](../media/tutorial11_traindialog.PNG)

### <a name="code-for-the-callbacks"></a>Código para os retornos de chamada

O código para os métodos de retorno de chamada está no ficheiro: c:\<installedpath > \src\demos\tutorialSessionCallbacks.ts.

![](../media/tutorial11_code.PNG)

Ambos os métodos são opcionais.

- OnSessionStartCallback: Este método define a entidade de BotName.
- OnSessionEndCallback: pode especificar o que deseja preservar. Isto irá eliminar todas as entidades, exceto o nome de utilizador e o telefone do utilizador.

### <a name="try-the-bot"></a>Experimente o bot

Mude para a IU da Web e clique em caixas de diálogo de registo.

1. Introduza "hello".
2. System: "Olá, sou Botty. O que é o seu nome? " que tem o nome Botty provenientes do OnSessionStartCallback.
3. Introduza "jason".
4. System: "Olá jason. O que é o número de telefone? "
5. Introduza "555-555-5555'.
6. System: "pode dizer Botty sua localização, jason?"
7. Escreva "Redmond".

Esta é uma sessão. Para iniciar uma nova sessão, é necessário terminar esta sessão. 

1. Clique em tempo limite da sessão. Isso será movido para a próxima sessão.
    - O botão de "Tempo limite da sessão" é fornecido para fins de depuração.  Numa sessão real, uma longa pausa têm de ocorrer, de aproximadamente 30 minutos.  Consulte a página de ajuda "Limites" durante o período de tempo limite de sessão predefinido.
1. Introduza "Olá".
2. System: "pode dizer Botty sua localização, jason?"
    - O sistema tem memorizadas o nome e número de telefone.
2. Introduza uma nova localização: "Lisboa".
3. System: "então, jason está em Seattle".
4. Clique em teste concluído.

Vamos alternar para caixas de diálogo de registo. Tenha em atenção que a última conversa foi dividida em duas porque a cada caixa de diálogo de registo corresponde a uma sessão.  

![](../media/tutorial11_splitdialogs.PNG)

- Na primeira interação, Botty está definido, mas o nome e número de telefone não são.
- A interação segundo mostra o nome e número de telefone.

Agora viu como as sessões são tratados por predefinição, e como pode substituir o comportamento padrão. 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Chamadas de API](./12-api-calls.md)
