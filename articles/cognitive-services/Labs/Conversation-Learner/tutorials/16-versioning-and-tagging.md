---
title: Como utilizar o controlo de versões e etiquetagem com uma aplicação de conversação Learner - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como utilizar o controlo de versões e etiquetagem com uma aplicação de conversação Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: ea013db078ff33f8597b0e15a8fc951e8ae320e8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354043"
---
# <a name="how-to-use-versioning-and-tagging"></a>Como utilizar o controlo de versões e etiquetagem

Este tutorial ilustra como tag versões da sua aplicação de conversação Learner e definir versão é "dinâmicos".  

## <a name="requirements"></a>Requisitos
Este tutorial requer utilizando o emulador de bot criar caixas de diálogo de registo, não a IU da Web do registo de caixa de diálogo.  

Este tutorial, necessita que o bot tutorial geral está em execução:

    npm run tutorial-general

## <a name="details"></a>Detalhes

Ao editar, que está sempre a editar a etiqueta chamada "original" – pode criar versões marcadas do mestre (que essencialmente tirar um instantâneo do mestre de), mas não é possível editar versões marcadas.

## <a name="steps"></a>Passos

### <a name="install-the-bot-framework-emulator"></a>Instale o emulador de framework Bot

- Vá para [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator).
- Transfira e instale o emulador.

### <a name="create-an-app"></a>Criar uma aplicação

1. Clique em nova aplicação
2. No campo do nome, introduza o Tutorial 16-controlo de versões
3. Clique em Criar 
4. Clique em definições
5. Copie o ID da aplicação

### <a name="configure-the-emulator"></a>Configurar o emulador

- Na pasta raiz de conversação Learner, abra o ficheiro .env.
- Colar o ID da aplicação como o valor de CONVERSATION_LEARNER_APP_ID
- Reinicie o serviço de conversação Learner por sair da linha de comandos e execute novamente:
 
    npm executar geral do tutorial 

### <a name="actions"></a>Ações

Vamos criar uma ação:

1. Mudar para a IU da Web.
1. Clique em ações, em seguida, ação de novo.
2. Em resposta, introduza "Olá daí (versão 1)".
3. Clique em Guardar.


![](../media/tutorial16_action1.PNG)

Crie uma nova etiqueta:

3. Clique em "definições" e criar uma nova "etiqueta".
    - Chamar esta "versão 1"
4. Defina a "versão 1" seja "dinâmicos".  
    - O efeito da definição na etiqueta em direto para "versão 1" é que os canais utilizando este bot irão utilizar o "1" tag de versão.
    - Marcado versões das aplicações não são afetadas por edições (alterar ações, as entidades, adicionar as caixas de diálogo de formação).  
    - As edições de uma aplicação (alterar ações, entidades, adicionar as caixas de diálogo de formação) sempre que são efetuadas na tag "original".  Por outras palavras, "original" está a etiqueta única que pode alterar; outras etiquetas de corrigidas instantâneos.
    - Inicie sessão caixas de diálogo na IU de Learner conversação sempre o mestre de utilização (não a tag em direto).

![](../media/tutorial16_v1_create.PNG)

Tenha em atenção de que a versão foi criada nas definições:

![](../media/tutorial16_settings.PNG)

Vamos adicionar uma segunda ação:

1. Clique em ações, em seguida, ação de novo.
2. Em resposta, introduza "bye (versão 2) do bye".

Edite a primeira ação:

1. Clique em ações.
2. Em ações, clique em "Olá daí (versão 1)".
3. Alterar a resposta a "Olá daí (versão 2)".

![](../media/tutorial16_hi_there_v2.PNG)

### <a name="switch-to-the-bot-emulator"></a>Mudar para o emulador de bot

1. Na IU do bot, introduza "goodbye".
2. Tenha em atenção o bot responde com "Olá daí (versão 1)".
    - Isto mostra que a versão 1 é "dinâmicos". 

![](../media/tutorial16_bf_response.PNG)

### <a name="switch-to-the-web-ui"></a>Mudar para a IU da Web

1. Clique no registo de caixas de diálogo (se não vir quaisquer caixas de diálogo, atualize a aplicação).
2. Clique em "Olá daí (versão 2)"

Tenha em atenção que podemos efetuar correções seleccionando todas as ações disponíveis atualmente. Estas edições serão efetuadas ao principal.

Constatou agora como funciona o controlo de versões e como pode interagir com bot utilizando o emulador do framework Bot.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Demonstração - reposição de palavra-passe](./demo-password-reset.md)
