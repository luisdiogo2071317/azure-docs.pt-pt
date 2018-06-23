---
title: Como implementar um bot conversação Learner - serviços cognitivos Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como implementar um bot Learner conversação.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 77cc998227d996a6e52b1b5629204da5dc735ede
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353960"
---
# <a name="how-to-deploy-a-conversation-learner-bot"></a>Como implementar um bot conversação Learner

Este documento explica como implementar um bot conversação Learner – localmente ou no Azure.

## <a name="prerequisite-determine-the-application-id"></a>Pré-requisito: determinar o ID da aplicação 

Para executar um bot fora da IU de Learner conversação, tem de definir o conversação Learner ID da aplicação que irá utilizar o bot - ou seja, o ID de modelo do machine learning na nuvem Learner de conversação.  (Por outro lado, ao executar o bot através da IU de Learner conversação, a IU escolhe o ID de aplicação).  

Eis como obter o ID da aplicação:

1. Inicie o seu bot e a IU de Learner conversação.  Consulte o guia de início rápido para obter instruções completas; Resumindo:

    Na janela de um comando:

    ```
    [open a command window]
    cd my-bot
    npm start
    ```

    Na janela de comandos noutro

    ```bash
    [open second command prompt window]
    cd cl-bot-01
    npm run ui
    ```

2. Browser abrir http://localhost:5050 

3. Clique na aplicação que deseja obter o ID de conversação Learner

4. Clique em "Definições" na barra de navegação à esquerda.

5. O GUID "ID da aplicação" é apresentado junto ao topo da página.

## <a name="option-1-deploying-a-conversation-learner-bot-to-run-locally"></a>Opção 1: Implementar um bot conversação Learner executar localmente

Isto implementa um bot no seu computador local e mostra como pode aceder ao mesmo utilizando o emulador do Bot Framework.

### <a name="configure-your-bot-for-access-outside-the-conversation-learner-ui"></a>Configurar o seu bot para acesso fora da IU de Learner conversação

Ao executar localmente um bot, adicione o ID da aplicação para o bot `.env` ficheiro:

    ```
    CONVERSATION_LEARNER_APP_ID=<YOUR_APP_ID>
    ```

Em seguida, inicie o bot:

    ```
    [open a command window]
    cd my-bot
    npm start
    ```

O bot está agora em execução localmente.  Consegue aceder-lhe com o emulador de Bot Framework.

### <a name="download-and-install-the-emulator"></a>Transfira e instale o emulador

    ```
    git clone https://github.com/Microsoft/BotFramework-Emulator
    npm install
    npm run build
    npm start
    ```

### <a name="connect-the-emulator-to-your-bot"></a>Ligar o emulador ao seu bot

1. No canto superior esquerdo do emulador, na caixa "Introduza o URL de ponto final", introduza `http://127.0.0.1:3978/api/messages`.  Deixe os outros campos em branco e clique em "Ligar".

2. Estão agora uma conversação com o seu bot.

## <a name="option-2-deploy-to-azure"></a>Opção 2: Implementar no Azure

Publica a sua bot conversação Learner semelhante da mesma forma que pretende publicar quaisquer outros bot. Um nível elevado, carregue o seu código para um Web site alojado, definir os valores de configuração adequada e, em seguida, registe o bot com vários canais. Instruções detalhadas são neste vídeo que mostra como publicar o seu bot utilizando o serviço do Azure Bot.

Depois do bot é implementado e em execução possam ligar canais diferentes ao mesmo, tais como o Facebook, equipas, Skype etc utilizando um registo de canal de Bot do Azure. Para obter a documentação desse processo, consulte: https://docs.microsoft.com/en-us/bot-framework/bot-service-quickstart-registration

Seguem-se as instruções passo a passo para implementar um Bot Learner de conversação do Azure.  Estas instruções partem do princípio de que a sua origem bot está disponível a partir de uma origem baseado na nuvem, tais como VSTS, o GitHub, BitBucket ou OneDrive e irá configurar seu bot para a implementação contínua.

1. Inicie sessão no portal do Azure em https://portal.azure.com

2. Criar um novo recurso de "Bot de aplicação Web" 

    1. Atribua o bot um nome
    2. Clique em "Bot modelo", escolha "Node.js", escolha "Básico", em seguida, clique no botão "Select"
    3. Clique em "Criar" para criar o Bot de aplicação Web.
    4. Aguarde que o recurso de Bot de aplicação Web a ser criado.

3. No portal do Azure, edite o recurso de Bot de aplicação Web que acabou de criar.

    1. Clique em "Definições de aplicação" item de navegação à esquerda
    1. Desloque para baixo para a secção "Definições de aplicação"
    2. Adicione estas definições:

        Variável de ambiente | valor
        --- | --- 
        CONVERSATION_LEARNER_SERVICE_URI | "https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/"
        CONVERSATION_LEARNER_APP_ID      | GUID de Id de aplicação, obtido a partir da IU de Learner conversação em "definições" da aplicação >
        LUIS_AUTHORING_KEY               | LUIS criação chave para esta aplicação
    
    4. Clique em "Guardar" perto da parte superior da página
    5. Abrir "Compilação" item de navegação à esquerda
    6. Clique em "Configurar a implementação contínua" 
    7. Clique no ícone "Configuração" em implementações
    8. Clique em "Definições necessárias"
    9. Selecione a origem de onde o seu código bot está disponível e configurar a origem.
