---
title: Bot QnA com o Azure Bot Service - QnA Maker
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 74c7bc5c601cd36a8dd2454506745406bc00dac0
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47031293"
---
# <a name="create-a-qna-bot-with-azure-bot-service-v3"></a>Crie um QnA Bot com o Azure Bot Service v3
Este tutorial orienta-o através da criação de um bot QnA com o Azure Bot service v3 no portal do Azure.

## <a name="prerequisite"></a>Pré-requisito
Antes de criar, siga os passos em [criar uma base de dados de conhecimento](../How-To/create-knowledge-base.md) para criar um serviço QnA Maker com perguntas e respostas.

O bot responde às perguntas a partir da base de dados de conhecimento que criou, via o QnAMakerDialog.

## <a name="create-a-qna-bot"></a>Criar um QnA Bot
1. Na [portal do Azure](https://portal.azure.com), selecione **Create** novo recurso no painel de menu e, em seguida, selecione **ver tudo**.

    ![criação do serviço de bot](../media/qnamaker-tutorials-create-bot/bot-service-creation.png)

2. Na caixa de pesquisa, procure **Web App Bot**.

    ![seleção de serviço de bot](../media/qnamaker-tutorials-create-bot/bot-service-selection.png)

3. Na **painel do serviço de Bot**, indique as informações necessárias:

    - Definir **nome da aplicação** ao nome do seu bot. O nome é utilizado como o subdomínio ao seu bot é implementado na cloud (por exemplo, mynotesbot.azurewebsites.net).
    - Selecione a subscrição, o grupo de recursos, o plano do serviço de aplicações e a localização.

4. Veja as instruções para a criação de um bot QnA com o SDK v4-consulte [modelo de bot QnA v4](https://aka.ms/qna-bot-v4). Para utilizar os modelos de v3, selecione a versão do SDK da **SDK v3** e o idioma do SDK do **c#** ou **node. js**.

    ![definições do sdk de bot](../media/qnamaker-tutorials-create-bot/bot-v3.png)

5. Selecione o **perguntas e respostas** modelo para o campo de modelo do Bot, em seguida, guarde as definições de modelo ao selecionar **selecione**.

    ![seleção de serviço de bot](../media/qnamaker-tutorials-create-bot/bot-v3-template.png)

6. Reveja as definições, em seguida, selecione **criar**. Isto cria e implementa o serviço de bot com QnAMakerDialog para o Azure.

    ![seleção de serviço de bot](../media/qnamaker-tutorials-create-bot/bot-blade-settings-v3.png)

7. Confirme que o serviço de bot foi implementado.

    - Selecione **notificações** (o ícone de sino que esteja localizado ao longo da margem superior do portal do Azure). A notificação será alterado de **implementação iniciada** ao **implementação concluída com êxito**.
    - Depois da notificação muda para **implementação concluída com êxito**, selecione **Ir para recurso** em que a notificação.

## <a name="chat-with-the-bot"></a>Conversar com o Bot
Selecionando **Ir para recurso** leva-o para o painel de recursos do bot.

Assim que o bot estiver registado, clique em **teste na Web Chat** para abrir o painel de Web Chat. Escreva "Olá" na Web Chat.

![QnA bot web bate-papo](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat.PNG)

O bot responde com ". Defina QnAKnowledgebaseId e QnASubscriptionKey nas definições da aplicação. Saiba como fazê-los em https://aka.ms/qnaabssetup". Esta resposta confirma que o seu QnA Bot recebeu a mensagem, mas não existe nenhuma base de dados de conhecimento do QnA Maker associado a ele ainda. Fazer isso no próximo passo.

## <a name="connect-your-qna-maker-knowledge-base-to-the-bot"></a>Ligar a sua base de dados de conhecimento do QnA Maker para o bot

1. Open **as configurações do aplicativo** e editar a **QnAKnowledgebaseId**, **QnAAuthKey**e a **QnAEndpointHostName** campos para conter os valores da sua base de dados de conhecimento do QnA Maker.

    ![Definições da aplicação](../media/qnamaker-tutorials-create-bot/application-settings.PNG)

2. Obter o seu ID de base de dados de conhecimento, url de anfitrião e a chave de ponto final a partir do separador Definições da sua base de dados de conhecimento no https://qnamaker.ai.
    - Inicie sessão no [do QnA Maker](https://qnamaker.ai)
    - Aceda à sua base de dados de conhecimento
    - Clique nas **definições** separador
    - **Publicar** sua base de dados de conhecimento, se não tiver feito

    ![Valores do QnA Maker](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

> [!NOTE]
> Se pretender ligar-se a versão de pré-visualização da base de dados de conhecimento com o bot QnA, defina o valor da **Ocp-Apim-Subscription-Key** ao **QnAAuthKey**. Deixe o **QnAEndpointHostName** vazio.

## <a name="test-the-bot"></a>Testar o bot
No portal do Azure, clique em **teste na Web Chat** para testar o bot. 

![Bot QnA Maker](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat-response.PNG)

O QnA Bot agora as respostas da sua base de dados de conhecimento.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Integrar o QnA Maker e o LUIS](./integrate-qnamaker-luis.md)

## <a name="see-also"></a>Consulte também

- [Gerir a sua base de dados de conhecimento](https://qnamaker.ai)
- [Ativar o seu bot em diferentes canais](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels)
