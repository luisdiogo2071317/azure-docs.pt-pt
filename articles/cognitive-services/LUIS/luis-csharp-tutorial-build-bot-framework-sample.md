---
title: Integrar LUIS com um bot utilizando o SDK de construtor Bot para c# no Azure | Microsoft Docs
description: Crie um bot integrado com uma aplicação de LUIS utilizando a estrutura de Bot.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/06/2018
ms.author: v-geberr
ms.openlocfilehash: b3283880ebb116e5397c38d722a0790cff414f38
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37111927"
---
# <a name="web-app-bot-using-the-luis-template-for-c"></a>Bot de aplicação Web utilizando o modelo de LUIS para c#

Crie um chatbot com compreensão de idiomas integrada.

## <a name="prerequisite"></a>Pré-requisito

* [Aplicação de LUIS HomeAutomation](luis-get-started-create-app.md). Pendentes do mapa de aplicação deste LUIS para processadores de caixa de diálogo a bot. 

## <a name="luis-homeautomation-intents"></a>LUIS HomeAutomation pendentes

| Objetivo | Utterance de exemplo | Funcionalidade de bot |
|:----:|:----------:|---|
| HomeAutomation.TurnOn | Ative os lights. | Quando a intenção de LUIS `HomeAutomation.TurnOn` é detetado, o bot invoca o `OnIntent` processador de caixa de diálogo. Esta caixa de diálogo é onde iria chamar um serviço de IoT para ativar um dispositivo e indicar ao utilizador que o dispositivo foi ativado. |
| HomeAutomation.TurnOff | Desative os lights bedroom. | Quando a intenção de LUIS `HomeAutomation.TurnOff` é detetado, o bot invoca o `OffIntent` processador de caixa de diálogo. Esta caixa de diálogo é onde iria chamar um serviço de IoT para desativar um dispositivo e indicar ao utilizador que o dispositivo foi desativado. |

## <a name="create-a-language-understanding-bot-with-bot-service"></a>Criar um bot de compreensão de idiomas com o serviço de Bot

1. No [portal do Azure](https://portal.azure.com), selecione **criar novo recurso** no menu superior esquerdo.

    ![Criar novo recurso](./media/luis-tutorial-cscharp-web-bot/bot-service-creation.png)

2. Na caixa de pesquisa, procure **Bot de aplicação Web**. 

    ![Criar novo recurso](./media/luis-tutorial-cscharp-web-bot/bot-service-selection.png)

3. Na janela Bot de aplicação Web, clique em **criar**.

4. No **Bot serviço**, forneça as informações necessárias e clique em **criar**. Isto cria e implementa o serviço de bot e LUIS aplicação no Azure. Se pretender utilizar [priming de reconhecimento de voz](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming), reveja [requisitos de região](luis-resources-faq.md#what-luis-regions-support-bot-framework-speech-priming) antes de criar a sua bot. 
    * Definir **nome da aplicação** ao nome do seu bot. O nome é utilizado como o subdomínio quando o bot é implementado para a nuvem (por exemplo, mynotesbot.azurewebsites.net). <!-- This name is also used as the name of the LUIS app associated with your bot. Copy it to use later, to find the LUIS app associated with the bot. -->
    * Selecione a subscrição, [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), plano do App service, e [localização](https://azure.microsoft.com/regions/).
    * Selecione o **compreensão de idiomas (c#)** modelo para o **modelo Bot** campo.
    * Selecione o **localização da aplicação LUIS**. Este é o processo de criação [região] [ LUIS] a aplicação é criada no.
    * Selecione a caixa de verificação de confirmação para o aviso legal. Os termos do aviso legal encontram-se abaixo a caixa de verificação.

    ![Serviço de Bot](./media/luis-tutorial-cscharp-web-bot/bot-service-setting-callout-template.png)


5. Certifique-se de que o serviço de bot foi implementado.
    * Clique em notificações (o ícone de campainha encontra-se ao longo de limite superior do portal do Azure). A notificação muda de **implementação iniciada** para **implementação concluída com êxito**.
    * Após a notificação de alterações **implementação concluída com êxito**, clique em **aceda a recursos** nessa notificação.

> [!Note]
> Este processo de criação de bot de aplicação web também criou uma nova aplicação LUIS por si. Foi preparado e publicado por si. 

## <a name="try-the-default-bot"></a>Repita o bot predefinido

Confirme que o bot tiver sido implementada, selecionando o **notificações** caixa de verificação. As notificações de alterações de **implementação em curso...**  para **implementação concluída com êxito**. Clique em **aceda a recursos** botão para abrir os recursos a bot.

Depois do bot é implementado, clique em **teste na Web Chat** para abrir o painel de Chat de Web. Escreva "Olá" no Web Chat.

  ![Testar o bot Web Chat](./media/luis-tutorial-cscharp-web-bot/bot-service-web-chat.png)

O bot responde ao indicar "atingiu a saudação. Que consiga aceder tal: Olá ".  Esta resposta confirma que o bot recebeu a mensagem e transmitido-la a uma predefinição LUIS aplicação que tenha criado. Esta predefinição LUIS aplicação detetada uma intenção de saudação. No próximo passo, vai ligar a bot para a aplicação de LUIS que criou anteriormente, em vez do predefinido LUIS aplicação.

## <a name="connect-your-luis-app-to-the-bot"></a>Ligar a aplicação de LUIS para o bot

Abra **definições da aplicação** e editar o **LuisAppId** campo para conter o ID da sua aplicação LUIS. Se tiver criado a sua aplicação HomeAutomation LUIS numa região diferente EUA oeste, terá de alterar o **LuisAPIHostName** bem. O **LuisAPIKey** está atualmente configurado para a chave de criação. Alterar isto para a chave de ponto final ao seu tráfego excede a quota do escalão gratuito. 

  ![Atualizar o ID da aplicação LUIS no Azure](./media/luis-tutorial-cscharp-web-bot/bot-service-app-settings.png)

> [!Note]
> Se não tiver o ID da aplicação LUIS do [home page automatização aplicação](luis-get-started-create-app.md), inicie sessão no [LUIS](luis-reference-regions.md) Web site com a mesma conta que utiliza para iniciar sessão no Azure. 
> 1. Clique em **as minhas aplicações**. 
> 2. Localize a aplicação LUIS que criou anteriormente, que contém as entidades do domínio HomeAutomation e pendentes.
> 3. No **definições** página para a aplicação de LUIS, localizar e copie o ID de aplicação. Certifique-se de que é [treinado](interactive-test.md) e [publicados](PublishApp.md). 

    > [!WARNING]
    > If you delete your app ID or LUIS key, the bot will stop working.

## <a name="modify-the-bot-code"></a>Modificar o código de bot

1. Clique em **criar** e, em seguida, clique em **editor de código online abra**.

   ![Editor de código aberto de online](./media/luis-tutorial-cscharp-web-bot/bot-service-build.png)

2. Clique com o botão direito `build.cmd` e escolha **executar a partir da consola** para criar a aplicação. Existem vários passos de compilação que do serviço concluída automaticamente para si. A compilação for concluída quando concluído com "Foi concluído com êxito."

3. No editor de código, abra `/Dialogs/BasicLuisDialog.cs`. Contém o seguinte código:

   [!code-csharp[Default BasicLuisDialog.cs](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/Default_BasicLuisDialog.cs "Default BasicLuisDialog.cs")]

## <a name="change-code-to-homeautomation-intents"></a>Alterar o código para HomeAutomation pendentes


1. Remova os atributos intenção e métodos para três **Greeting**, **Cancelar**, e **ajudar**. Estes pendentes não são utilizadas no domínio prebuilt HomeAutomation. Certifique-se de que mantém o **nenhum** atributo intenção e método. 

2. Adicione as dependências na parte superior do ficheiro, com as outras dependências:

   [!code-csharp[Dependencies](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=4-5&dedent=8 "dependencies")]

3. Adicionar gerir cadeias na parte superior do que as constantes de `BasicLuisDialog ` classe:

   [!code-csharp[Add Intent and Entity Constants](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=23-32&dedent=8 "Add Intent and Entity Constants")]

4. Adicione o código para os novo pendentes de `HomeAutomation.TurnOn` e `HomeAutomation.TurnOff` dentro de `BasicLuisDialog ` classe:

   [!code-csharp[Add Intents](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=61-71&dedent=8 "Add Intents")]

5. Adicione o código para obter todas as entidades encontradas pelo LUIS dentro de `BasicLuisDialog ` classe:

   [!code-csharp[Collect entities](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=34-53&dedent=8 "Collect entities")]

6. Alteração **ShowLuisResult** método o `BasicLuisDialog ` classe arredondar a classificação, recolher as entidades e apresentar a mensagem de resposta a chatbot:

   [!code-csharp[Display message in chatbot](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=73-83&dedent=8 "Display message in chatbot")]

## <a name="build-the-bot"></a>Criar o bot
No editor de código, clique com botão direito no `build.cmd` e selecione **executar a partir da consola**.

![Criar Web bot ](./media/luis-tutorial-cscharp-web-bot/bot-service-build-run-from-console.png)

A vista de código é substituída por uma janela de terminal que mostra o progresso e os resultados de compilação.

![Criar Web bot com êxito](./media/luis-tutorial-cscharp-web-bot/bot-service-build-success.png)

> [!TIP]
> É um método alternativo para criar o bot selecionar o nome de bot na barra superior azul e selecione **abrir a consola do Kudu**. A consola será aberta para **D:\home**. 
> 
> Altere o diretório para **D:\home\site\wwwroot** escrevendo: `cd site\wwwroot`
>
> Execute o script de compilação, escrevendo: `build.cmd`

## <a name="test-the-bot"></a>Testar o bot

No portal do Azure, clique em **testar na Web Chat** para testar o bot. Escreva mensagens like "ativar os lights" e "desligar a meu heater" invocar pendentes que adicionou ao mesmo.

   ![Testar HomeAutomation bot Web Chat](./media/luis-tutorial-cscharp-web-bot/bot-service-chat-results.png)

> [!TIP]
> Pode a reparametrização dos aplicação LUIS sem qualquer modificação ao código da sua bot. Consulte [adicionar utterances exemplo](https://docs.microsoft.com/azure/cognitive-services/LUIS/add-example-utterances) e [dar formação e testar a aplicação de LUIS](https://docs.microsoft.com/azure/cognitive-services/LUIS/interactive-test). 

## <a name="download-the-bot-to-debug"></a>Transferir o bot para depuração
Se a sua bot não está a funcionar, transfira o projeto no seu computador local e continuar [depuração](https://docs.microsoft.com/bot-framework/bot-service-debug-bot#debug-an-azure-app-service-web-app-c-bot). 

## <a name="learn-more-about-bot-framework"></a>Saiba mais sobre a arquitetura de Bot
Saiba mais sobre [Bot Framework](https://dev.botframework.com/) e [3](https://github.com/Microsoft/BotBuilder) e [4. x](https://github.com/Microsoft/botbuilder-dotnet) SDKs.

## <a name="next-steps"></a>Passos Seguintes

Adicionar as caixas de diálogo de serviço de Bot para processamento e pendentes de LUIS **ajudar**, **Cancelar**, e **Greeting** pendentes. Não se esqueça de formação, publicar e [criar](#build-the-bot) bot de aplicação web. LUIS e o bot deve ter os mesmos pendentes.

> [!div class="nextstepaction"]
> [Adicionar pendentes](./luis-how-to-add-intents.md)
> [priming de reconhecimento de voz](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming)


<!-- Links -->
[Github-BotFramework-Emulator-Download]: https://aka.ms/bot-framework-emulator
[Github-LUIS-Samples]: https://github.com/Microsoft/LUIS-Samples
[Github-LUIS-Samples-cs-hotel-bot]: https://github.com/Microsoft/LUIS-Samples/tree/master/bot-integration-samples/hotel-finder/csharp
[Github-LUIS-Samples-cs-hotel-bot-readme]: https://github.com/Microsoft/LUIS-Samples/blob/master/bot-integration-samples/hotel-finder/csharp/README.md
[BFPortal]: https://dev.botframework.com/
[RegisterInstructions]: https://docs.microsoft.com/bot-framework/portal-register-bot
[BotFramework]: https://docs.microsoft.com/bot-framework/
[AssignedEndpointDoc]: https://docs.microsoft.com/azure/cognitive-services/LUIS/manage-keys
[VisualStudio]: https://www.visualstudio.com/
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
<!-- tested on Win10 -->
