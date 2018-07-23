---
title: Como utilizar o controlo de versões e a etiquetagem com um modelo de aprendiz de conversação - serviços cognitivos da Microsoft | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como utilizar o controlo de versões e a etiquetagem com um modelo de aprendiz de conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: c7f23d989cbfa0ece9e404a0fe0feb68cf5fddb2
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170550"
---
# <a name="how-to-use-versioning-and-tagging"></a>Como utilizar o controlo de versões e etiquetagem

Este tutorial ilustra como marcar as versões do seu modelo de aprendiz de conversação e definir qual versão é "dinâmicos".  

## <a name="requirements"></a>Requisitos
Este tutorial requer a utilizar o emulador de bot para criar caixas de diálogo de registo, não a IU da Web do registo de caixa de diálogo.  

Este tutorial requer que o bot tutorial geral está em execução:

    npm run tutorial-general

## <a name="details"></a>Detalhes

Durante a edição, o que está a editar sempre marca chamada "mestre"- - pode criar versões marcadas do principal (que, essencialmente, tire um instantâneo do mestre), mas não é possível editar versões marcadas.

## <a name="steps"></a>Passos

### <a name="install-the-bot-framework-emulator"></a>Instale o emulador do Bot framework

- Vá para [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator).
- Baixe e instale o emulador.

### <a name="create-an-model"></a>Criar um modelo

1. Clique em novo modelo
2. No campo de nome, introduza o Tutorial-16-controle de versão
3. Clique em Criar 
4. Clique em definições
5. Copie o ID de modelo

### <a name="configure-the-emulator"></a>Configurar o emulador

- Na pasta de raiz Aprendiz de conversação, abra o ficheiro. env.
- Cole o ID de modelo como o valor do CONVERSATION_LEARNER_MODEL_ID
- Reinicie o serviço de aprendiz de conversação, sair da linha de comandos e execute novamente:
 
    npm executar geral do tutorial 

### <a name="actions"></a>Ações

Vamos criar uma ação:

1. Mude para a IU da Web.
1. Clique em ações, em seguida, nova ação.
2. Em resposta, introduza "Olá daí (versão 1)".
3. Clique em Guardar.


![](../media/tutorial16_action1.PNG)

Crie uma nova etiqueta:

3. Clique em "definições" e criar uma nova "etiqueta".
    - Chamá-lo de "versão 1"
4. Defina a "versão 1" para ser "dinâmicos".  
    - O efeito de definir a etiqueta em direto para "versão 1" é que os canais com este bot irão utilizar o "1" a etiqueta de versão.
    - As versões marcadas dos modelos não são afetadas pela edições (alterando as ações de entidades, adicionando caixas de diálogo train).  
    - Edições de um modelo (alterar ações, entidades, adicionando caixas de diálogo train) são sempre feitas na marca "principal".  Em outras palavras, "mestre" é a única marca que pode ser alterado; outras marcas são fixos instantâneos.
    - Inicie sessão caixas de diálogo na interface de Usuário de aprendiz de conversação sempre use master (não a marca em direto).

![](../media/tutorial16_v1_create.PNG)

A versão foi criada nas definições:

![](../media/tutorial16_settings.PNG)

Vamos adicionar uma segunda ação:

1. Clique em ações, em seguida, nova ação.
2. Em resposta, introduza "xau xau (versão 2)".

Edite a primeira ação:

1. Clique em ações.
2. Em ações, clique em "Olá daí (versão 1)".
3. Alterar a resposta "Olá daí (versão 2)".

![](../media/tutorial16_hi_there_v2.PNG)

### <a name="switch-to-the-bot-emulator"></a>Mude para o emulador de bot

1. Na interface do Usuário o bot, introduza "goodbye".
2. O bot responde com "Olá daí (versão 1)".
    - Isso mostra que a versão 1 é "dinâmicos". 

![](../media/tutorial16_bf_response.PNG)

### <a name="switch-to-the-web-ui"></a>Mude para a IU da Web

1. Clique no registo de caixas de diálogo (se não vir quaisquer caixas de diálogo, clique no botão Atualizar).
2. Clique em "Olá daí (versão 2)"

> [!NOTE]
> Podemos fazer correções escolhendo em todas as ações disponíveis atualmente. Estas edições serão feitas a mestre.

Agora viu como funciona o controle de versão e como pode interagir com o bot a utilizar o emulador do Bot framework.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Demonstração - reposição de palavra-passe](./demo-password-reset.md)
