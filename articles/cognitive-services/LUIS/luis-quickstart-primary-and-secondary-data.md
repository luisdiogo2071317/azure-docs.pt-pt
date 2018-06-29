---
title: 'Tutorial: criar uma aplicação LUIS para extrair dados - Azure | Microsoft Docs'
description: Neste tutorial, saiba como criar uma aplicação LUIS simples com intenções e uma entidade simples para extrair dados de aprendizagem automática.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 03/29/2018
ms.author: v-geberr
ms.openlocfilehash: 1e8647e34da3d34946a4f6ac298017f6d4c99de6
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265365"
---
# <a name="tutorial-create-app-that-uses-simple-entity"></a>Tutorial: Criar uma aplicação que utiliza a entidade simples
Neste tutorial, vai criar uma aplicação que demonstra como extrair dados de aprendizagem automática de uma expressão com a entidade **Simple** (Simples).

<!-- green checkmark -->
> [!div class="checklist"]
> * Compreender as entidades simples 
> * Criar uma nova aplicação LUIS para o domínio de comunicação com a intenção SendMessage
> * Adicionar a intenção _None_ (Nenhuma) e adicionar expressões de exemplo
> * Adicionar a entidade simples para extrair os conteúdos da mensagem da expressão
> * Preparar e publicar a aplicação
> * Consultar o ponto final da aplicação para ver a resposta JSON de LUIS

Para este artigo, precisa de uma conta do [LUIS][LUIS] gratuita para criar a sua aplicação LUIS.

## <a name="purpose-of-the-app"></a>Objetivo da aplicação
Esta aplicação demonstra como extrair dados de uma expressão. Considere a seguinte expressão de um chatbot:

```JSON
Send a message telling them to stop
```

A intenção é enviar uma mensagem. Os dados importantes da expressão são a própria mensagem, `telling them to stop`.  

## <a name="purpose-of-the-simple-entity"></a>Objetivo da entidade simples
O objetivo da entidade simples é ensinar ao LUIS o que é uma mensagem e onde pode ser encontrada numa expressão. A parte da expressão que corresponde à mensagem pode variar de expressão para expressão com base na escolha de palavras e no comprimento da expressão. O LUIS precisa de exemplos de mensagens em qualquer expressão em todas as intenções.  

Para esta aplicação simples, a mensagem estará no final da expressão. 

## <a name="create-a-new-app"></a>Criar uma nova aplicação
1. Inicie sessão no site do [LUIS][LUIS]. Certifique-se de que inicia sessão na região onde precisa que os pontos finais do LUIS sejam publicados.

2. No site do [LUIS][LUIS], selecione **Create new app** (Criar nova aplicação).  

    ![Lista de aplicações LUIS](./media/luis-quickstart-primary-and-secondary-data/app-list.png)

3. Na caixa de diálogo de pop-up, introduza o nome `MyCommunicator`. 

    ![Lista de aplicações LUIS](./media/luis-quickstart-primary-and-secondary-data/create-new-app-dialog.png)

4. Quando esse processo terminar, a aplicação mostra a página **Intents** (Intenções) com a intenção **None** (Nenhuma). 

    [![](media/luis-quickstart-primary-and-secondary-data/intents-list.png "Captura de ecrã da página Intents do LUIS com a intenção None")](media/luis-quickstart-primary-and-secondary-data/intents-list.png#lightbox)

## <a name="create-a-new-intent"></a>Criar uma nova intenção

1. Na página **Intents** (Intenções), selecione **Create new intent** (Criar nova intenção). 

    [![](media/luis-quickstart-primary-and-secondary-data/create-new-intent-button.png "Captura de ecrã do LUIS com o botão «Create new intent» realçado")](media/luis-quickstart-primary-and-secondary-data/create-new-intent-button.png#lightbox)

2. Introduza o nome da nova intenção `SendMessage`. Esta intenção deve ser selecionada sempre que um utilizador pretenda enviar uma mensagem.

    Ao criar uma intenção, está a criar a categoria principal de informações que pretende identificar. Atribuir um nome à categoria permite que qualquer outra aplicação que utilize os resultados da consulta do LUIS utilize esse nome de categoria para encontrar uma resposta adequada ou para executar a ação apropriada. O LUIS não responde a estas perguntas, apenas identifica o tipo de informação que está a ser pedida em linguagem natural. 

    ![Introduzir o nome da intenção SendMessage](./media/luis-quickstart-primary-and-secondary-data/create-new-intent-popup-dialog.png)

3. Adicione sete expressões à intenção `SendMessage`, que espera que um utilizador peça, tais como:

    | Expressões de exemplo|
    |--|
    |Responder com Recebi a mensagem, terei a resposta amanhã|
    |Enviar mensagem a perguntar Quando estará em casa?|
    |Enviar mensagem de texto a indicar que estou ocupado|
    |Informá-los que tem de ser feito hoje|
    |MI a indicar que estou a conduzir e responderei mais tarde|
    |Compor mensagem para o David que diz Quando é que isso foi?|
    |dizer olá ao greg|

    [![](media/luis-quickstart-primary-and-secondary-data/enter-utterances-on-intent-page.png "Captura de ecrã do LUIS com expressões introduzidas")](media/luis-quickstart-primary-and-secondary-data/enter-utterances-on-intent-page.png#lightbox)

## <a name="add-utterances-to-none-intent"></a>Adicionar expressões à intenção None

Atualmente, a aplicação LUIS não tem expressões para a intenção **None** (Nenhuma). Precisa de expressões que não queira que a aplicação responda e, por isso, tem de ter expressões na intenção **None** (Nenhuma). Não deixe em branco. 
    
1. Selecione **Intents** (Intenções) no painel esquerdo. 

    [![](media/luis-quickstart-primary-and-secondary-data/select-intent-link.png "Captura de ecrã do LUIS com o botão «Intents» realçado")](media/luis-quickstart-primary-and-secondary-data/select-intent-link.png#lightbox)

2. Selecione a intenção **None** (Nenhuma). 

    [![](media/luis-quickstart-primary-and-secondary-data/select-none-intent.png "Captura de ecrã da seleção da intenção None")](media/luis-quickstart-primary-and-secondary-data/select-none-intent.png#lightbox)

3. Adicione três expressões que o utilizador poderá introduzir, mas que não são relevantes para a sua aplicação. Algumas expressões **None** boas são:

    | Expressões de exemplo|
    |--|
    |Cancelar!|
    |Adeus|
    |O que está a acontecer?|
    
    Na sua aplicação de chamada do LUIS, como um chatbot, se o LUIS devolver a intenção **None** (Nenhuma) para uma expressão, o seu bot pode perguntar se o utilizador quer terminar a conversa. O bot também pode dar mais instruções para continuar a conversa se o utilizador não pretender terminá-la. 

    [![](media/luis-quickstart-primary-and-secondary-data/utterances-for-none-intent.png "Captura de ecrã do LUIS com expressões para a intenção None")](media/luis-quickstart-primary-and-secondary-data/utterances-for-none-intent.png#lightbox)

## <a name="create-a-simple-entity-to-extract-message"></a>Criar uma entidade simples para extrair uma mensagem 
1. Selecione **Intents** (Intenções) no menu da esquerda.

    ![Selecionar a ligação Intents](./media/luis-quickstart-primary-and-secondary-data/select-intents-from-none-intent.png)

2. Selecione `SendMessage` na lista de intenções.

    ![Selecionar a intenção SendMessage](./media/luis-quickstart-primary-and-secondary-data/select-sendmessage-intent.png)

3. Na expressão `Reply with I got your message, I will have the answer tomorrow`, selecione a primeira palavra do corpo da mensagem, `I`, e a última palavra do corpo da mensagem, `tomorrow`. Todas estas palavras são selecionadas para a mensagem e é apresentado um menu pendente com uma caixa de texto na parte superior.

    [![](media/luis-quickstart-primary-and-secondary-data/select-words-in-utterance.png "Captura de ecrã da seleção de palavras na expressão para a mensagem")](media/luis-quickstart-primary-and-secondary-data/select-words-in-utterance.png#lightbox)

4. Introduza o nome da entidade `Message` na caixa de texto.

    [![](media/luis-quickstart-primary-and-secondary-data/enter-entity-name-in-box.png "Captura de ecrã da introdução do nome da entidade na caixa")](media/luis-quickstart-primary-and-secondary-data/enter-entity-name-in-box.png#lightbox)

5. Selecione **Create new entity** (Criar nova entidade) no menu pendente. O objetivo da entidade é extrair o texto que corresponde ao corpo da mensagem. Nesta aplicação LUIS, a mensagem de texto está no fim da expressão, mas a expressão pode ter qualquer comprimento e a mensagem também pode ter qualquer comprimento. 

    [![](media/luis-quickstart-primary-and-secondary-data/create-message-entity.png "Captura de ecrã da criação da nova entidade a partir da expressão")](media/luis-quickstart-primary-and-secondary-data/create-message-entity.png#lightbox)

6. Na janela de pop-up, o tipo de entidade predefinida é **Simple** (Simples) e o nome da entidade é `Message`. Mantenha estas definições e selecione **Done** (Concluído).

    ![Verificar o tipo de entidade](./media/luis-quickstart-primary-and-secondary-data/entity-type.png)

7. Agora que a entidade está criada e uma expressão está identificada, identifique o resto das expressões com essa entidade. Selecione uma expressão e, em seguida, selecione a primeira e a última palavra de uma mensagem. No menu pendente, selecione a entidade `Message`. A mensagem está agora identificada na entidade. Continue a identificar todas as expressões de mensagem nas expressões restantes.

    [![](media/luis-quickstart-primary-and-secondary-data/all-labeled-utterances.png "Captura de ecrã de todas as expressões de mensagem identificadas")](media/luis-quickstart-primary-and-secondary-data/all-labeled-utterances.png#lightbox)

    A vista predefinida das expressões é a **Entities view** (Vista de entidades). Selecione o controlo **Entities view** (Vista de entidades) acima das expressões. A **Tokens view** (Vista de tokens) apresenta o texto da expressão. 

    [![](media/luis-quickstart-primary-and-secondary-data/tokens-view-of-utterances.png "Captura de ecrã de expressões na vista de Tokens")](media/luis-quickstart-primary-and-secondary-data/tokens-view-of-utterances.png#lightbox)

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS
O LUIS desconhece as alterações às intenções e entidades (o modelo), até ser preparado. 

1. No lado direito superior do site do LUIS, selecione o botão **Train** (Preparar).

    ![Selecionar o botão de preparação](./media/luis-quickstart-primary-and-secondary-data/train-button.png)

2. A preparação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

    ![Notificação de êxito de preparação](./media/luis-quickstart-primary-and-secondary-data/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicar a aplicação para obter o URL de ponto final
Para obter uma predição do LUIS num chatbot ou noutra aplicação, tem de publicar a aplicação. 

1. No lado direito superior do site do LUIS, selecione o botão **Publish** (Publicar). 

2. Selecione o bloco Production (Produção) e o botão **Publish** (Publicar).

    [![](media/luis-quickstart-primary-and-secondary-data/publish-to-production.png "Captura de ecrã da página Publicar com o botão Publicar no bloco de produção realçado")](media/luis-quickstart-primary-and-secondary-data/publish-to-production.png#lightbox)

3. A publicação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consultar o ponto final com uma expressão diferente
Na página **Publish** (Publicar), selecione a ligação do **ponto final** na parte inferior da página. 

[![](media/luis-quickstart-primary-and-secondary-data/publish-select-endpoint.png "Captura de ecrã da página Publicar com o ponto final realçado")](media/luis-quickstart-primary-and-secondary-data/publish-select-endpoint.png#lightbox)

Esta ação abre outra janela de browser com o URL de ponto final na barra de endereço. Vá para o final do URL no endereço e introduza `text I'm driving and will be 30 minutes late to the meeting`. O último parâmetro querystring é `q`, a expressão **query**. Esta expressão não é igual a qualquer uma das expressões identificadas, pelo que é um bom teste e deve devolver as expressões `SendMessage`.

```
{
  "query": "text I'm driving and will be 30 minutes late to the meeting",
  "topScoringIntent": {
    "intent": "SendMessage",
    "score": 0.987501
  },
  "intents": [
    {
      "intent": "SendMessage",
      "score": 0.987501
    },
    {
      "intent": "None",
      "score": 0.111048922
    }
  ],
  "entities": [
    {
      "entity": "i ' m driving and will be 30 minutes late to the meeting",
      "type": "Message",
      "startIndex": 5,
      "endIndex": 58,
      "score": 0.162995353
    }
  ]
}
```

## <a name="what-has-this-luis-app-accomplished"></a>O que conseguiu esta aplicação LUIS?
Esta aplicação, com apenas duas intenções e uma entidade, identificou uma intenção de consulta de linguagem natural e devolveu os dados da mensagem. 

O resultado JSON identifica a intenção com a melhor classificação `SendMessage` com uma classificação de 0.987501. Todas as classificações estão compreendidas entre 1 e 0, estando a melhor classificação próxima de 1. A classificação da intenção `None` é 0.111048922, muito mais próxima de zero. 

Os dados da mensagem têm um tipo, `Message`, bem como um valor, `i ' m driving and will be 30 minutes late to the meeting`. 

Agora, o seu chatbot tem informação suficiente para determinar a ação principal, `SendMessage`, e um parâmetro dessa ação, o texto da mensagem. 

## <a name="where-is-this-luis-data-used"></a>Onde são utilizados estes dados do LUIS? 
O LUIS concluiu este pedido. A aplicação de chamada, como um chatbot, pode utilizar o resultado topScoringIntent e os dados da entidade para enviar a mensagem através de uma API de terceiros. Se existirem outras opções programáticas para o bot ou a aplicação de chamada, o LUIS não executa essas opções. O LUIS apenas determina qual é a intenção do utilizador. 

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não precisar, elimine a aplicação LUIS. Para tal, selecione o menu de três pontos (…) à direita do nome da aplicação na lista de aplicações e selecione **Delete** (Eliminar). Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Saiba como adicionar uma entidade hierárquica](luis-quickstart-intent-and-hier-entity.md)


<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
