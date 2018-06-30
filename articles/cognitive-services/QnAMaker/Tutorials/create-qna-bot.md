---
title: Criar um bot de QnA com o serviço de Bot Azure - serviços cognitivos do Azure | Microsoft Docs
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: fc430bf3aa7cad279d7a93bb6892aa19abee3378
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37109274"
---
# <a name="create-a-qna-bot-with-azure-bot-service"></a>Criar um Bot de QnA com o serviço de Bot do Azure
Este tutorial orienta-o através da criação de um bot de QnA com o serviço de Azure Bot no portal do Azure.

## <a name="prerequisite"></a>Pré-requisito
Antes de criar, siga os passos no [criar uma base de dados de conhecimento](../How-To/create-knowledge-base.md) para criar um serviço de QnA Maker com perguntas e respostas.

O bot responde às perguntas partir da base de dados de conhecimento que criou, através de QnAMakerDialog.

## <a name="create-a-qna-bot"></a>Criar um Bot de QnA
1. No [portal do Azure](https://portal.azure.com), selecione **criar** novo recurso no painel de menu e, em seguida, selecione **ver todos os**.

    ![Bot a criação do serviço](../media/qnamaker-tutorials-create-bot/bot-service-creation.png)

2. Na caixa de pesquisa, procure **Bot de aplicação Web**.

    ![seleção de serviço bot](../media/qnamaker-tutorials-create-bot/bot-service-selection.png)

3. No **painel Bot serviço**, forneça as informações necessárias e selecione **criar**. Isto cria e implementa o serviço de bot com QnAMakerDialog para o Azure.

    - Definir **nome da aplicação** ao nome do seu bot. O nome é utilizado como o subdomínio quando o bot é implementado para a nuvem (por exemplo, mynotesbot.azurewebsites.net).
    - Selecione a subscrição, o grupo de recursos, o plano do App service e a localização.
    - Selecione o **pergunta e resposta** modelo (Node.js ou c#) para o campo de modelo Bot.
    - Selecione a caixa de verificação de confirmação para o aviso legal. Os termos do aviso legal encontram-se abaixo a caixa de verificação.

        ![seleção de serviço bot](../media/qnamaker-tutorials-create-bot/bot-service-qna-template.PNG)

4. Certifique-se de que o serviço de bot foi implementado.

    - Selecione **notificações** (o ícone de campainha encontra-se ao longo de limite superior do portal do Azure). A notificação deixará de **implementação iniciada** para **implementação concluída com êxito**.
    - Após a notificação de alterações **implementação concluída com êxito**, selecione **aceda a recursos** nessa notificação.

## <a name="chat-with-the-bot"></a>Chat com o Bot
Selecionar **aceda a recursos** leva-o para o painel de recursos do bot.

Depois do bot estiver registado, clique em **teste na Web Chat** para abrir o painel de Chat de Web. Escreva "Olá" no Web Chat.

![Bot de QnA web chat](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat.PNG)

O bot responde com ". Defina QnAKnowledgebaseId e QnASubscriptionKey nas definições de aplicação. Saber como obtê-las em https://aka.ms/qnaabssetup". Esta resposta confirma que sua Bot de QnA recebeu a mensagem, mas não existe nenhuma base de dados de conhecimento Maker de QnA associado ainda. Fazê-lo no próximo passo.

## <a name="connect-your-qna-maker-knowledge-base-to-the-bot"></a>Ligar a base de dados de conhecimento de QnA Maker para o bot

1. Abra **definições da aplicação** e editar o **QnAKnowledgebaseId**, **QnAAuthKey**e o **QnAEndpointHostName** campos para conter os valores da sua base de dados de conhecimento de QnA Maker.

    ![Definições de aplicação](../media/qnamaker-tutorials-create-bot/application-settings.PNG)

2. Obter o seu ID de base de dados de conhecimento, url de anfitrião e a chave de ponto final do separador Definições da sua base de dados de conhecimento no https://qnamaker.ai.
    - Inicie sessão no [Maker de QnA](https://qnamaker.ai)
    - Aceda à sua base de dados de conhecimento
    - Clique em de **definições** separador
    - **Publicar** a base de dados de conhecimento, se não fez

    ![Valores de QnA Maker](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

> [!NOTE]
> Se pretende ligar-se a versão de pré-visualização de base de dados de conhecimento com o bot de QnA, defina o valor de **Ocp-Apim-Subscription-Key** para **QnAAuthKey**. Deixe o **QnAEndpointHostName** vazio.

## <a name="test-the-bot"></a>Testar o bot
No portal do Azure, clique em **testar na Web Chat** para testar o bot. 

![Bot Maker de QnA](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat-response.PNG)

O Bot de QnA agora responde a partir da sua base de dados de conhecimento.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Integrar Maker de QnA e LUIS](./integrate-qnamaker-luis.md)

## <a name="see-also"></a>Consulte também

- [Gerir a sua base de dados de conhecimento](https://qnamaker.ai)
- [Ativar o bot em canais diferentes](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels)
