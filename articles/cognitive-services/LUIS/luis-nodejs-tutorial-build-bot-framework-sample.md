---
title: Integrar LUIS com um bot utilizando o SDK de construtor Bot para Node.js no Azure | Microsoft Docs
description: Crie um bot integrado com uma aplicação de LUIS utilizando a estrutura de Bot.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/06/2018
ms.author: v-geberr
ms.openlocfilehash: 12cc84942c139d3c5e981aec902557201c9c8092
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355771"
---
# <a name="integrate-luis-with-a-bot-using-the-bot-builder-sdk-for-nodejs"></a>Integrar LUIS com um bot utilizando o SDK de construtor Bot para Node.js

Este tutorial explica-lhe criar um bot com o [Bot Framework] [ BotFramework] que está integrado com uma aplicação LUIS.

## <a name="prerequisite"></a>Pré-requisito

Antes de criar o bot, siga os passos no [criar uma aplicação](./luis-get-started-create-app.md) para criar a aplicação de LUIS que utiliza.

O bot responde ao pendentes do domínio HomeAutomation que estão na aplicação LUIS. Para cada um destes pendentes, a aplicação de LUIS fornece objetivo que mapeia para o mesmo. O bot fornece uma caixa de diálogo que processa a intenção LUIS Deteta.

| Objetivo | Utterance de exemplo | Funcionalidade de bot |
|:----:|:----------:|---|
| HomeAutomation.TurnOn | Ative os lights. | O bot invoca o `TurnOnDialog` quando o `HomeAutomation.TurnOn` é detetado. Esta caixa de diálogo é onde seria invocar um serviço de IoT para ativar um dispositivo e indicar ao utilizador que o dispositivo foi ativado. |
| HomeAutomation.TurnOff | Desative os lights bedroom. | O bot invoca o `TurnOffDialog` quando o `HomeAutomation.TurnOff` é detetado. Esta caixa de diálogo onde seria invocar um serviço de IoT para desativar um dispositivo e indicar ao utilizador que o dispositivo foi desativado. |


## <a name="create-a-language-understanding-bot-with-bot-service"></a>Criar um bot de compreensão de idiomas com o serviço de Bot

1. No [portal do Azure](https://portal.azure.com), selecione **criar novo recurso** no painel de menu e selecione **ver todos os**.

    ![Criar novo recurso](./media/luis-tutorial-node-bot/bot-service-creation.png)

2. Na caixa de pesquisa, procure **Bot de aplicação Web**. 

    ![Criar novo recurso](./media/luis-tutorial-node-bot/bot-service-selection.png)

3. No **Bot serviço** painel, forneça as informações necessárias e selecione **criar**. Isto cria e implementa o serviço de bot e LUIS aplicação no Azure. Se pretender utilizar [priming de reconhecimento de voz](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming), reveja [requisitos de região](luis-resources-faq.md#what-luis-regions-support-bot-framework-speech-priming) antes de criar a sua bot. 
    * Definir **nome da aplicação** ao nome do seu bot. O nome é utilizado como o subdomínio quando o bot é implementado para a nuvem (por exemplo, mynotesbot.azurewebsites.net). <!-- This name is also used as the name of the LUIS app associated with your bot. Copy it to use later, to find the LUIS app associated with the bot. -->
    * Selecione a subscrição, [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), plano do App service, e [localização](https://azure.microsoft.com/regions/).
    * Selecione o **compreensão de idiomas (Node.js)** modelo para o **modelo Bot** campo.
    * Selecione o **localização da aplicação LUIS**. Este é o processo de criação [região] [ LUIS] a aplicação é criada no.
    * Selecione a caixa de verificação de confirmação para o aviso legal. Os termos do aviso legal encontram-se abaixo a caixa de verificação.

    ![Painel de serviço bot](./media/luis-tutorial-node-bot/bot-service-setting-callout-template.png)


4. Certifique-se de que o serviço de bot foi implementado.
    * Selecione as notificações (o ícone de campainha encontra-se ao longo de limite superior do portal do Azure). A notificação deixará de **implementação iniciada** para **implementação concluída com êxito**.
    * Após a notificação de alterações **implementação concluída com êxito**, selecione **aceda a recursos** nessa notificação.

## <a name="try-the-default-bot"></a>Repita o bot predefinido

Confirme que o bot tiver sido implementada, verificando o **notificações**. As notificações deixará de **implementação em curso...**  para **implementação concluída com êxito**. Selecione **aceda a recursos** botão para abrir o painel de recursos do bot.

<!-- this step isn't supposed to be necessary -->
## <a name="install-npm-resources"></a>Instalar recursos NPM
Instale pacotes NPM com os seguintes passos:

1. Selecione **criar** do **Bot gestão** secção Bot de aplicação Web. 

2. A segunda nova, abre-se de janela de browser. Selecione **editor de código online abra**.

3. Na barra de navegação superior, selecione o nome de bot de aplicação web `homeautomationluisbot`. 

4. Na lista pendente, **abrir a consola do Kudu**.

5. Abre uma nova janela do browser. Na consola, introduza o seguinte comando:

    ```
    cd site\wwwroot && npm install
    ```

    Aguarde que o processo de instalação concluir. Devolver a primeira janela de browser. 

## <a name="test-in-web-chat"></a>Teste no Chat na Web
Depois do bot estiver registado, selecione **teste na Web Chat** para abrir o painel de Chat de Web. Escreva "Olá" no Web Chat.

  ![Testar o bot Web Chat](./media/luis-tutorial-node-bot/bot-service-web-chat.png)

O bot responde ao indicar "atingiu a saudação. Que consiga aceder tal: Olá ". Isto confirma que o bot recebeu a mensagem e transmitido-la a uma predefinição LUIS aplicação que tenha criado. Esta predefinição LUIS aplicação detetada uma intenção de saudação. No próximo passo, vai ligar a bot para a aplicação de LUIS que criou anteriormente, em vez do predefinido LUIS aplicação.

## <a name="connect-your-luis-app-to-the-bot"></a>Ligar a aplicação de LUIS para o bot

Abra **definições da aplicação** na primeira janela do browser e editar o **LuisAppId** campo para conter o ID da sua aplicação LUIS.

  ![Atualizar o ID da aplicação LUIS no Azure](./media/luis-tutorial-node-bot/bot-service-app-id.png)

Se não tiver o ID da aplicação LUIS, inicie sessão para o [LUIS](luis-reference-regions.md) Web site com a mesma conta que utiliza para iniciar sessão no Azure. Selecione em **as minhas aplicações**. 

1. Localize a aplicação LUIS que criou anteriormente, que contém as entidades do domínio HomeAutomation e pendentes.

2. No **definições** página para a aplicação de LUIS, localizar e copie o ID de aplicação.

3. Se ainda não preparado a aplicação, selecione o **preparar** botão no canto superior direito para preparar a sua aplicação.

4. Se não tiver publicado a aplicação, selecione **publicar** na barra de navegação superior para abrir o **publicar** página. Selecione a ranhura de produção e o **publicar** botão.

## <a name="modify-the-bot-code"></a>Modificar o código de bot

Ir para a janela de browser segundo esteja ainda abrir ou na primeira janela de browser, selecione **criar** e, em seguida, selecione **editor de código online abra**.

   ![Editor de código aberto de online](./media/luis-tutorial-node-bot/bot-service-build.png)

No editor de código, abra `app.js`. Contém o seguinte código:

```javascript
/*-----------------------------------------------------------------------------
A simple Language Understanding (LUIS) bot for the Microsoft Bot Framework. 
-----------------------------------------------------------------------------*/

var restify = require('restify');
var builder = require('botbuilder');
var botbuilder_azure = require("botbuilder-azure");

// Setup Restify Server
var server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
   console.log('%s listening to %s', server.name, server.url); 
});
  
// Create chat connector for communicating with the Bot Framework Service
var connector = new builder.ChatConnector({
    appId: process.env.MicrosoftAppId,
    appPassword: process.env.MicrosoftAppPassword,
    openIdMetadata: process.env.BotOpenIdMetadata 
});

// Listen for messages from users 
server.post('/api/messages', connector.listen());

/*----------------------------------------------------------------------------------------
* Bot Storage: This is a great spot to register the private state storage for your bot. 
* We provide adapters for Azure Table, CosmosDb, SQL Azure, or you can implement your own!
* For samples and documentation, see: https://github.com/Microsoft/BotBuilder-Azure
* ---------------------------------------------------------------------------------------- */

var tableName = 'botdata';
var azureTableClient = new botbuilder_azure.AzureTableClient(tableName, process.env['AzureWebJobsStorage']);
var tableStorage = new botbuilder_azure.AzureBotStorage({ gzipData: false }, azureTableClient);

// Create your bot with a function to receive messages from the user
// This default message handler is invoked if the user's utterance doesn't
// match any intents handled by other dialogs.
var bot = new builder.UniversalBot(connector, function (session, args) {
    session.send('You reached the default message handler. You said \'%s\'.', session.message.text);
});

bot.set('storage', tableStorage);

// Make sure you add code to validate these fields
var luisAppId = process.env.LuisAppId;
var luisAPIKey = process.env.LuisAPIKey;
var luisAPIHostName = process.env.LuisAPIHostName || 'westus.api.cognitive.microsoft.com';

const LuisModelUrl = 'https://' + luisAPIHostName + '/luis/v2.0/apps/' + luisAppId + '?subscription-key=' + luisAPIKey;

// Create a recognizer that gets intents from LUIS, and add it to the bot
var recognizer = new builder.LuisRecognizer(LuisModelUrl);
bot.recognizer(recognizer);

// Add a dialog for each intent that the LUIS app recognizes.
// See https://docs.microsoft.com/bot-framework/nodejs/bot-builder-nodejs-recognize-intent-luis 
bot.dialog('GreetingDialog',
    (session) => {
        session.send('You reached the Greeting intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Greeting'
})

bot.dialog('HelpDialog',
    (session) => {
        session.send('You reached the Help intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Help'
})

bot.dialog('CancelDialog',
    (session) => {
        session.send('You reached the Cancel intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Cancel'
})
```

Os pendentes existentes no app.js são ignorados. Pode deixá-los. 

## <a name="add-a-dialog-that-matches-homeautomationturnon"></a>Adicionar uma caixa de diálogo que corresponda ao HomeAutomation.TurnOn

Copie o seguinte código e adicione-o a `app.js`.

```javascript
bot.dialog('TurnOn',
    (session) => {
        session.send('You reached the TurnOn intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'HomeAutomation.TurnOn'
})
```

O [corresponde ao] [ matches] opção o [triggerAction] [ triggerAction] ligado para a caixa de diálogo Especifica o nome da intenção de. O reconhecedor é executada sempre que o bot recebe um utterance do utilizador. Se corresponder a intenção de classificação mais elevada que Deteta um `triggerAction` vinculada a uma caixa de diálogo, o bot invoca essa caixa de diálogo.

## <a name="add-a-dialog-that-matches-homeautomationturnoff"></a>Adicionar uma caixa de diálogo que corresponda ao HomeAutomation.TurnOff

Copie o seguinte código e adicione-o a `app.js`.

```javascript
bot.dialog('TurnOff',
    (session) => {
        session.send('You reached the TurnOff intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'HomeAutomation.TurnOff'
})
```
## <a name="test-the-bot"></a>Testar o bot

No Portal do Azure, selecione em **testar na Web Chat** para testar o bot. Tente mensagens do tipo like "ativar os lights" e "desligar a meu heater" invocar pendentes que adicionou ao mesmo.
   ![Testar HomeAutomation bot Web Chat](./media/luis-tutorial-node-bot/bot-service-chat-results.png)

> [!TIP]
> Se achar que o seu bot sempre não reconhece a intenção correta ou entidades, melhore o desempenho da sua aplicação LUIS, concedendo-la mais utterances de exemplo para prepará-lo. Pode a reparametrização dos aplicação LUIS sem qualquer modificação ao código da sua bot. Consulte [adicionar utterances exemplo](https://docs.microsoft.com/azure/cognitive-services/LUIS/add-example-utterances) e [dar formação e testar a aplicação de LUIS](https://docs.microsoft.com/azure/cognitive-services/LUIS/train-test).

## <a name="learn-more-about-bot-framework"></a>Saiba mais sobre a arquitetura de Bot
Saiba mais sobre [Bot Framework](https://dev.botframework.com/) e [3](https://github.com/Microsoft/BotBuilder) e [4. x](https://github.com/Microsoft/botbuilder-js) SDKs.

## <a name="next-steps"></a>Passos Seguintes

<!-- From trying the bot, you can see that the recognizer can trigger interruption of the currently active dialog. Allowing and handling interruptions is a flexible design that accounts for what users really do. Learn more about the various actions you can associate with a recognized intent.-->
Pode tentar adicionar outros pendentes, como obter ajuda, Cancelar e a saudação inicial, para a aplicação de LUIS. Em seguida, adicionar as caixas de diálogo para os novo pendentes e teste-los utilizando o bot. 

<!-- 
> [!NOTE] 
> The default LUIS app that the template created contains example utterances for Cancel, Greeting, and Help intents. In the list of apps, find the app that begins with the name specified in **App name** in the **Bot Service** blade when you created the Bot Service. -->

> [!div class="nextstepaction"]
> [Adicionar pendentes](./luis-how-to-add-intents.md)
> [priming de reconhecimento de voz](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming)


[intentDialog]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.intentdialog.html

[intentDialog_matches]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.intentdialog.html#matches 

[NotesSample]: https://github.com/Microsoft/BotFramework-Samples/tree/master/docs-samples/Node/basics-naturalLanguage

[triggerAction]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#triggeraction

[confirmPrompt]: https://docs.botframework.com/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions.html#confirmprompt

[waterfall]: bot-builder-nodejs-dialog-manage-conversation-flow.md#manage-conversation-flow-with-a-waterfall

[session_userData]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.session.html#userdata

[EntityRecognizer_findEntity]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.entityrecognizer.html#findentity

[matches]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions.html#matches

[LUISAzureDocs]: https://docs.microsoft.com/azure/cognitive-services/LUIS/Home

[Dialog]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html

[IntentRecognizerSetOptions]: https://docs.botframework.com/node/builder/chat-reference/interfaces/_botbuilder_d_.iintentrecognizersetoptions.html

[LuisRecognizer]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.luisrecognizer

[LUISConcepts]: https://docs.botframework.com/node/builder/guides/understanding-natural-language/

[DisambiguationSample]: https://github.com/Microsoft/BotBuilder/tree/master/Node/examples/feature-onDisambiguateRoute

[IDisambiguateRouteHandler]: https://docs.botframework.com/node/builder/chat-reference/interfaces/_botbuilder_d_.idisambiguateroutehandler.html

[RegExpRecognizer]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.regexprecognizer.html

[AlarmBot]: https://github.com/Microsoft/BotBuilder/blob/master/Node/examples/basics-naturalLanguage/app.js

[LUISBotSample]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/intelligence-LUIS

[UniversalBot]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.universalbot.html


<!-- Old Links -->
[Github-BotFramework-Emulator-Download]: https://aka.ms/bot-framework-emulator
[Github-LUIS-Samples]: https://github.com/Microsoft/LUIS-Samples
[Github-LUIS-Samples-node-hotel-bot]:https://github.com/Microsoft/LUIS-Samples/tree/master/bot-integration-samples/hotel-finder/nodejs
[NodeJs]: https://nodejs.org/
[BFPortal]: https://dev.botframework.com/
[RegisterInstructions]: https://docs.microsoft.com/bot-framework/portal-register-bot
[BotFramework]: https://docs.microsoft.com/bot-framework/
[LUIS]:luis-reference-regions.md

