---
title: Bot de LUIS com C# - Tutorial - Bot de aplicação Web - Bot Framework SDK 4.0
titleSuffix: Azure Cognitive Services
description: Com o C#, crie um chatbot integrado com compreensão de idiomas (LUIS). Este chatbot utiliza a aplicação Recursos Humanos para implementar rapidamente uma solução de bot. O bot é criado com a versão 4 do Bot Framework e o bot de aplicação Web do Azure.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/25/2018
ms.author: diberry
ms.openlocfilehash: f8350d46fecff726dd9f591fe3df0272f556b3e7
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47168195"
---
# <a name="tutorial-luis-bot-in-c"></a>Tutorial: bot de LUIS em C#
Com C#, pode criar um chatbot integrado com compreensão de idiomas (LUIS). Este bot utiliza a aplicação HomeAutomation para implementar uma solução de bot. O bot é criado com o [bot de aplicação Web](https://docs.microsoft.com/azure/bot-service/) do Azure com a versão v4 do [Bot Framework](https://github.com/Microsoft/botbuilder-js).

**Neste tutorial, ficará a saber como:**

> [!div class="checklist"]
> * Criar um bot de aplicação Web. Este processo cria uma nova aplicação LUIS.
> * Adicionar um domínio pré-criado ao novo modelo do LUIS
> * Transferir o projeto criado pelo serviço de bot Web
> * Iniciar o bot e o emulador localmente no seu computador
> * Modificar o código de bot para as novas intenções do LUIS
> * Ver os resultados de expressão no bot

## <a name="prerequisites"></a>Pré-requisitos

* [Emulador de bot](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/)


## <a name="create-web-app-bot"></a>Criar bot de aplicação Web

1. No [portal do Azure](https://portal.azure.com), selecione **Criar novo recurso**.

2. Na caixa de pesquisa, procure e selecione **Bot de Aplicação Web**. Selecione **Criar**.

3. Em **Serviço de Bot**, indique as informações necessárias:

    |Definição|Objetivo|Definição sugerida|
    |--|--|--|
    |Nome do bot|Nome do recurso|`luis-csharp-bot-` + `<your-name>`, por exemplo, `luis-csharp-bot-johnsmith`|
    |Subscrição|Subscrição na qual vai criar o bot.|A sua subscrição principal.
    |Grupo de recursos|Grupo lógico de recursos do Azure|Crie um novo grupo para armazenar todos os recursos utilizados com este bot, dê ao grupo o nome `luis-csharp-bot-resource-group`.|
    |Localização|Região do Azure - não tem de ser a mesma da região de criação ou publicação do LUIS.|`westus`|
    |Escalão de preço|Utilizado para limites de pedido de serviço e faturação.|`F0` é o escalão gratuito.
    |Nome da aplicação|O nome é utilizado como o subdomínio quando o bot é implementado na cloud (por exemplo, humanresourcesbot.azurewebsites.net).|`luis-csharp-bot-` + `<your-name>`, por exemplo, `luis-csharp-bot-johnsmith`|
    |Modelo de bot|Definições do Bot Framework - veja a tabela seguinte|
    |Localização da Aplicação LUIS|Tem de ser a mesma da região de recursos do LUIS|`westus`|

4. Em **Definições de modelos de bot**, selecione o seguinte e, em seguida, escolha o botão **Selecionar** nestas definições:

    |Definição|Objetivo|Seleção|
    |--|--|--|
    |Versão do SDK|Versão do Bot Framework|**SDK v4**|
    |Idioma do SDK|Linguagem de programação do bot|**C#**|
    |Bot Eco/Básico|Tipo de bot|**Bot básico**|
    
5. Selecione **Criar**. Esta ação cria e implementa o serviço de bot no Azure. Parte deste processo cria uma aplicação LUIS com o nome `luis-csharp-bot-XXXX`. Este nome baseia-se no nome do bot e da aplicação na secção anterior.

    [ ![Criar bot de aplicação Web](./media/bfv4-csharp/create-web-app-service.png) ](./media/bfv4-csharp/create-web-app-service.png#lightbox)

6. Deixe este separador do browser aberto. Para qualquer passo no portal do LUIS, abra um novo separador do browser. Avance para a secção seguinte quando o novo serviço de bot for implementado.

## <a name="add-prebuilt-domain-to-model"></a>Adicionar um domínio pré-criado ao modelo
Parte da implementação do serviço de bot cria uma nova aplicação LUIS com intenções e expressões de exemplo. O bot fornece o mapeamento das intenções à nova aplicação LUIS para as intenções seguintes: 

|Intenções LUIS de bot básico|expressão de exemplo|
|--|--|
|Cancelar|`stop`|
|Saudação|`hello`|
|Ajuda|`help`|
|Nenhuma|Tudo o que estiver fora do domínio da aplicação.|

Adicione a aplicação HomeAutomation pré-criada ao modelo, para lidar com expressões como: `Turn off the living room lights`

1. Aceda ao portal do [LUIS](https://www.luis.ai) e inicie sessão.
2. Na página **As Minhas Aplicações**, selecione a coluna **Data de criação** para ordenar pela data de criação da aplicação. O Azure Bot Service criou uma nova aplicação na secção anterior. O respetivo nome é `luis-csharp-bot-` + `<your-name>` + 4 carateres aleatórios.
3. Abra a aplicação e selecione a secção **Criar** no painel de navegação superior.
4. No painel de navegação esquerdo, selecione **Domínios Pré-criados**.
5. Selecione o domínio**HomeAutomation**, ao selecionar **Adicionar domínio** no respetivo cartão.
6. Selecione **Preparar** no menu superior direito.
7. Selecione **Publicar** no menu superior direito. 

    A aplicação criada pelo Azure Bot Service tem agora novas intenções:

    |Novas intenções de bot básico|expressão de exemplo|
    |--|--|
    |HomeAutomation.TurnOn|`turn the fan to high`
    |HomeAutomation.TurnOff|`turn off ac please`|

## <a name="download-the-web-app-bot"></a>Transferir o bot de aplicação Web 
Para poder desenvolver o código de bot de aplicação Web, transfira o código e utilize-o no seu computador local. 

1. No portal do Azure, ainda no recurso de bot de aplicação Web, selecione as **Definições da Aplicação** e copie os valores de **botFilePath** e **botFileSecret**. Tem de adicioná-los a um ficheiro de ambiente mais tarde. 

2. No portal do Azure, selecione **Compilar** na secção **Gestão de bot**. 

3. Selecione **Transferir o código-fonte de Bot**. 

    [![Transferir o código-fonte de bot de aplicação Web para um bot básico](../../../includes/media/cognitive-services-luis/bfv4/download-code.png) ](../../../includes/media/cognitive-services-luis/bfv4/download-code.png#lightbox)

4. Quando o código-fonte estiver zipado, uma mensagem fornecerá uma hiperligação para transferir o código. Selecione a hiperligação. 

5. Guarde o ficheiro zip no computador local e extraia os ficheiros. Abra o projeto. 

6. Abra o ficheiro bot.cs e procure `_services.LuisServices`. É aqui que a expressão de utilizador introduzida no bot é enviada para o LUIS.

    ```csharp
    /// <summary>
    /// Run every turn of the conversation. Handles orchestration of messages.
    /// </summary>
    /// <param name="turnContext">Bot Turn Context.</param>
    /// <param name="cancellationToken">Task CancellationToken.</param>
    /// <returns>A <see cref="Task"/> representing the asynchronous operation.</returns>
    public async Task OnTurnAsync(ITurnContext turnContext, CancellationToken cancellationToken)
    {
        var activity = turnContext.Activity;

        if (activity.Type == ActivityTypes.Message)
        {
            // Perform a call to LUIS to retrieve results for the current activity message.
            var luisResults = await _services.LuisServices[LuisConfiguration].RecognizeAsync(turnContext, cancellationToken).ConfigureAwait(false);

            // If any entities were updated, treat as interruption.
            // For example, "no my name is tony" will manifest as an update of the name to be "tony".
            var topScoringIntent = luisResults?.GetTopScoringIntent();

            var topIntent = topScoringIntent.Value.intent;
            switch (topIntent)
            {
                case GreetingIntent:
                    await turnContext.SendActivityAsync("Hello.");
                    break;
                case HelpIntent:
                    await turnContext.SendActivityAsync("Let me try to provide some help.");
                    await turnContext.SendActivityAsync("I understand greetings, being asked for help, or being asked to cancel what I am doing.");
                    break;
                case CancelIntent:
                    await turnContext.SendActivityAsync("I have nothing to cancel.");
                    break;
                case NoneIntent:
                default:
                    // Help or no intent identified, either way, let's provide some help.
                    // to the user
                    await turnContext.SendActivityAsync("I didn't understand what you just said to me.");
                    break;
            }
        }
        else if (activity.Type == ActivityTypes.ConversationUpdate)
        {
            if (activity.MembersAdded.Any())
            {
                // Iterate over all new members added to the conversation.
                foreach (var member in activity.MembersAdded)
                {
                    // Greet anyone that was not the target (recipient) of this message.
                    // To learn more about Adaptive Cards, see https://aka.ms/msbot-adaptivecards for more details.
                    if (member.Id != activity.Recipient.Id)
                    {
                        var welcomeCard = CreateAdaptiveCardAttachment();
                        var response = CreateResponse(activity, welcomeCard);
                        await turnContext.SendActivityAsync(response).ConfigureAwait(false);
                    }
                }
            }
        }

    }
    ```

    O bot envia a expressão do utilizador ao LUIS e obtém os resultados. A intenção principal determina o fluxo de conversação. 


## <a name="start-the-bot"></a>Iniciar o bot
Antes de alterar qualquer código ou as definições, certifique-se de que o bot funciona. 

1. No Visual Studio, abra o ficheiro da solução. 

2. Crie um ficheiro `appsettings.json` para conter as variáveis de bot que o código de bot procura:

    ```JSON
    {
    "botFileSecret": "",
    "botFilePath": ""

    }
    ```

    Defina os valores das variáveis para os valores que copiou das Definições da Aplicação do serviço de bot do Azure no Passo 1 da secção **[Transferir o bot de aplicação Web](#download-the-web-app-bot)**.

3. No Visual Studio, inicie o bot. É aberta uma janela de browser com o site do bot de aplicação Web em `http://localhost:3978/`.

## <a name="start-the-emulator"></a>Iniciar o emulador

1. Inicie o Emulador do Bot.

2. No emulador do bot, selecione o ficheiro *.bot na raiz do projeto. Este ficheiro `.bot` inclui o ponto final do URL do bot para mensagens:

    [ ![Emulador de bot v4](../../../includes/media/cognitive-services-luis/bfv4/bot-emulator-v4.png) ](../../../includes/media/cognitive-services-luis/bfv4/bot-emulator-v4.png#lightbox)

3. Introduza o segredo de bot que copiou das Definições da Aplicação do serviço de bot do Azure no Passo 1 da secção **[Transferir o bot de aplicação Web](#download-the-web-app-bot)**. Isto permite ao emulador aceder a quaisquer campos encriptados no ficheiro `.bot`.

    ![Segredo de emulador de bot v4](../../../includes/media/cognitive-services-luis/bfv4/bot-secret.png)

4. No emulador do bot, introduza `Hello` e obtenha a resposta adequada para o bot básico.

    [ ![Resposta ao bot básico no emulador](../../../includes/media/cognitive-services-luis/bfv4/emulator-test.png) ](../../../includes/media/cognitive-services-luis/bfv4/emulator-test.png#lightbox)

## <a name="modify-bot-code"></a>Modificar o código do bot 

No ficheiro `BasicBot.cs`, adicione o código para processar as novas intenções. 

1. Na parte superior do ficheiro, encontre a secção **Intenções de LUIS Suportadas** e adicione constantes para as intenções HomeAutomation:

    ```csharp
    // Supported LUIS Intents
    public const string GreetingIntent = "Greeting";
    public const string CancelIntent = "Cancel";
    public const string HelpIntent = "Help";
    public const string NoneIntent = "None";
    public const string TurnOnIntent = "HomeAutomation_TurnOn"; // new intent
    public const string TurnOffIntent = "HomeAutomation_TurnOff"; // new intent
    ```

    Tenha em atenção que o ponto final, `.`, entre o domínio e a intenção da aplicação do portal do LUIS é substituído por um caráter de sublinhado, `_`. 

2. Encontre o método **OnTurnAsync** que recebe a predição de LUIS da expressão. Adicione o código na instrução switch para devolver a resposta do LUIS às duas intenções de HomeAutomation. 

    ```csharp
    case TurnOnIntent:
        await turnContext.SendActivityAsync("TurnOn intent found, JSON response: " + luisResults?.Entities.ToString());
        break;
    case TurnOffIntent:
        await turnContext.SendActivityAsync("TurnOff intent found, JSON response: " + luisResults?.Entities.ToString());
        break;
    ```

    O bot não tem a mesma resposta exata de um pedido de API REST do LUIS, pelo que é importante saber as diferenças ao observar a resposta JSON. O texto e as propriedades de intenções são os mesmos, mas os valores das propriedades das entidades foram modificados. 

    ```JSON
    {
        "$instance": {
            "HomeAutomation_Device": [
                {
                    "startIndex": 23,
                    "endIndex": 29,
                    "score": 0.9776345,
                    "text": "lights",
                    "type": "HomeAutomation.Device"
                }
            ],
            "HomeAutomation_Room": [
                {
                    "startIndex": 12,
                    "endIndex": 22,
                    "score": 0.9079433,
                    "text": "livingroom",
                    "type": "HomeAutomation.Room"
                }
            ]
        },
        "HomeAutomation_Device": [
            "lights"
        ],
        "HomeAutomation_Room": [
            "livingroom"
        ]
    }
    ```



## <a name="view-results-in-bot"></a>Ver os resultados no bot

1. No emulador do bot, introduza a expressão: `Turn on the livingroom lights to 50%`

2. O bot responde com:

    ```JSON
    TurnOn intent found, JSON response: {"$instance":{“HomeAutomation_Device”:[{“startIndex”:23,“endIndex”:29,“score”:0.9776345,“text”:“lights”,“type”:“HomeAutomation.Device”}],“HomeAutomation_Room”:[{“startIndex”:12,“endIndex”:22,“score”:0.9079433,“text”:“livingroom”,“type”:“HomeAutomation.Room”}]},“HomeAutomation_Device”:[“lights”],“HomeAutomation_Room”:[“livingroom”]}
    ```    ```

## Learn more about Bot Framework
Azure Bot service uses the Bot Framework SDK. Learn more about the SDK and bot framework:

* [Azure Bot Service](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0) v4 documentation
* [Bot Builder Samples](https://github.com/Microsoft/botbuilder-samples)
* [Bot Builder SDK](https://docs.microsoft.com/en-us/javascript/api/botbuilder-core/?view=botbuilder-ts-latest)
* [Bot Builder tools](https://github.com/Microsoft/botbuilder-tools):

## Next steps

You created an Azure bot service, copied the bot secret and `.bot` file path, downloaded the zip file of the code. You added the prebuilt HomeAutomation domain to the LUIS app created as part of the new Azure bot service, then trained and published the app again. You extracted the code project, created an environment file (`.env`), and set the bot secret and the `.bot` file path. In the bot.js file, you added code to handle the two new intents. Then you tested the bot in the bot emulator to see the LUIS response for an utterance of one of the new intents. 


> [!div class="nextstepaction"]
> [Build a custom domain in LUIS](luis-quickstart-intents-only.md)
