---
title: Configurar mensagens com o Service Bus do Azure para o Azure Logic Apps | Microsoft Docs
description: Enviar e receber mensagens com as logic apps utilizando o Service Bus do Azure
services: logic-apps
documentationcenter: ''
author: ecfan
manager: anneta
editor: ''
tags: connectors
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: logic-apps
ms.date: 02/06/2018
ms.author: ladocs
ms.openlocfilehash: d5a4760e1e0f38fd81fd779786985f5753d77eab
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="send-and-receive-messages-with-the-azure-service-bus-connector"></a>Enviar e receber mensagens com o conector do Service Bus do Azure

Para enviar e receber mensagens com a sua aplicação lógica, ligue ao [Service Bus do Azure](https://azure.microsoft.com/services/service-bus/). Pode realizar ações tais como a enviar para uma fila, enviar para um tópico, a receber de uma fila e a receber de uma subscrição. Saiba mais sobre [Service Bus do Azure](../service-bus-messaging/service-bus-messaging-overview.md) e [como preços funciona para as Logic Apps aciona](../logic-apps/logic-apps-pricing.md).

## <a name="prerequisites"></a>Pré-requisitos

Antes de poder utilizar o conector do Service Bus, tem de ter estes itens, que tem de existir na mesma subscrição do Azure para que forem visíveis entre si:

* A [espaço de nomes do Service Bus e a entidade de mensagens, tais como uma fila](../service-bus-messaging/service-bus-create-namespace-portal.md)
* A [aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="permissions-connection-string"></a>

## <a name="connect-to-azure-service-bus"></a>Ligar ao Service Bus do Azure

Antes da aplicação lógica pode aceder a qualquer serviço, tem de criar um [ *ligação* ](./connectors-overview.md) entre a sua aplicação lógica e o serviço, se ainda não o fez. Esta ligação autoriza a sua aplicação lógica para aceder a dados. Para a sua aplicação lógica aceder à sua conta do Service Bus, verifique as suas permissões.

1. Inicie sessão no [portal do Azure](https://portal.azure.com "portal do Azure"). 

2. Vá para o Service Bus *espaço de nomes*, não uma específica "entidade de mensagens". Na página de espaço de nomes, em **definições**, escolha **políticas de acesso partilhado**. Em **afirmações**, verifique se tem **gerir** permissões para esse espaço de nomes.

   ![Gerir permissões do seu espaço de nomes do Service Bus](./media/connectors-create-api-azure-service-bus/azure-service-bus-namespace.png)

3. Se pretender mais tarde manualmente introduza as suas informações de ligação, obter a cadeia de ligação para o seu espaço de nomes do Service Bus. Escolha **RootManageSharedAccessKey**. Junto a cadeia de ligação de chave primária, escolha o botão Copiar. Guarde a cadeia de ligação para utilização posterior.

   ![Copie a cadeia de ligação de espaço de nomes de barramento de serviço](./media/connectors-create-api-azure-service-bus/find-service-bus-connection-string.png)

   > [!TIP]
   > Para confirmar se a cadeia de ligação é associada o espaço de nomes de barramento de serviço ou com uma entidade específica, verifique a cadeia de ligação para o `EntityPath` parâmetro. Se encontrar este parâmetro, a cadeia de ligação para uma entidade específica, não sendo a cadeia correta a utilizar com a sua aplicação lógica.

## <a name="trigger-workflow-when-your-service-bus-gets-new-messages"></a>Acionar o fluxo de trabalho quando o Service Bus obtém novas mensagens

A [ *acionador* ](../logic-apps/logic-apps-overview.md#logic-app-concepts) é um evento que inicia a um fluxo de trabalho na sua aplicação lógica. Para iniciar um fluxo de trabalho quando novas mensagens são enviadas para o Service Bus, siga estes passos para adicionar o acionador que Deteta estas mensagens.

1. No [portal do Azure](https://portal.azure.com "portal do Azure"), aceda à sua aplicação lógica existente ou criar uma aplicação lógica em branco.

2. No Designer de aplicações de lógica, introduza "service bus" na caixa de pesquisa, como o filtro. Selecione o **Service Bus** conector. 

   ![Seleccione o conector do Service Bus](./media/connectors-create-api-azure-service-bus/select-service-bus-connector.png) 

3. Selecione o acionador que pretende utilizar. Por exemplo, para executar uma aplicação lógica, quando um novo item é enviado para uma fila do Service Bus, selecione este acionador: **Service Bus - quando é recebida uma mensagem numa fila (a conclusão automática)**

   ![Selecione o acionador do Service Bus](./media/connectors-create-api-azure-service-bus/select-service-bus-trigger.png)

   > [!NOTE]
   > Algumas aciona uma retorno ou mensagens, tais como o *Service Bus - quando uma ou mais mensagens chegam numa fila (a conclusão automática)* acionador.
   > Quando estes acionadores acionados, devolvem entre um e o número de mensagens em fila especificada pelo acionador **máximo de mensagem** propriedade.

   1. Se ainda não tiver uma ligação para o espaço de nomes do Service Bus, é-lhe pedido que criar agora esta ligação. Dê um nome da ligação e selecione o espaço de nomes de barramento de serviço que pretende utilizar.

      ![Criar a ligação do Service Bus](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-1.png)

      Ou, para inserirem manualmente a cadeia de ligação, escolha **introduzir manualmente as informações de ligação**. 
      Saiba [como encontrar a cadeia de ligação](#permissions-connection-string).
      

   2. Agora, selecione a política de barramento de serviço para utilizar e escolha **criar**.

      ![Criar a ligação do Service Bus, parte 2](./media/connectors-create-api-azure-service-bus/create-service-bus-connection-2.png)

4. Seleccione a fila de barramento de serviço a utilizar e configurar o intervalo de frequência e para quando a fila de verificação.

   ![Selecione a fila do Service Bus, configurar o intervalo de consulta](./media/connectors-create-api-azure-service-bus/select-service-bus-queue.png)

   > [!NOTE]
   > Todos os acionadores de Service Bus **longa consulta** acionadores, o que significa que, quando um acionador é acionado, o acionador processa todas as mensagens e, em seguida, aguarda 30 segundos, dos mais mensagens apareça na subscrição fila ou um tópico.
   > Se não existem mensagens são recebidas na 30 segundos, é ignorada a execução do acionador. Caso contrário, o acionador continua a leitura de mensagens até que a subscrição de fila ou um tópico está vazia.
   > A seguinte consulta de Acionador baseia-se no intervalo de periodicidade especificado nas propriedades do acionador.

5. Guarde a aplicação lógica. Na barra de ferramentas do estruturador, escolha **Guardar**.

Agora, quando a sua aplicação lógica verifica a fila seleccionada e localiza uma nova mensagem, o acionador executa as ações na sua aplicação lógica para a mensagem foi encontrada.

## <a name="send-messages-from-your-logic-app-to-your-service-bus"></a>Enviar mensagens da sua aplicação lógica para o Service Bus

Uma [*ação*](../logic-apps/logic-apps-overview.md#logic-app-concepts) é uma tarefa realizada pelo fluxo de trabalho da sua aplicação lógica. Depois de adicionar um acionador à aplicação lógica, pode adicionar uma ação para realizar operações com dados gerados por esse acionador. Para enviar uma mensagem para a entidade a partir da sua aplicação lógica de mensagens do Service Bus, siga estes passos.

1. No Designer de aplicações lógicas, sob o acionador, escolha **+ novo passo** > **adicionar uma ação**.

2. Na caixa de pesquisa, introduza "service bus" como o filtro. Selecione este conector: **Service Bus**

   ![Seleccione o conector do Service Bus](./media/connectors-create-api-azure-service-bus/select-service-bus-connector-for-action.png) 

3. Selecione esta ação: **Service Bus - enviar mensagem**

   ![Selecione "Service Bus - enviar mensagem"](./media/connectors-create-api-azure-service-bus/select-service-bus-send-message-action.png)

4. Selecione a entidade de mensagens, que é o nome de fila ou um tópico, para onde enviar a mensagem. Em seguida, introduza o conteúdo da mensagem e quaisquer outros detalhes.

   ![Selecione a entidade de mensagens e forneça detalhes da mensagem](./media/connectors-create-api-azure-service-bus/service-bus-send-message-details.png)    

5. Guarde a aplicação lógica. 

Configurou uma ação que envia mensagens a partir da sua aplicação lógica. 

## <a name="connector-specific-details"></a>Detalhes específicos do conector

Para saber mais sobre acionadores e ações definidas por quaisquer limites e o ficheiro Swagger, reveja o [detalhes do conector](/connectors/servicebus/).

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para submeter ou votar em ideias para funcionalidades, visite o [site de comentários dos utilizadores do Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [outros conectores para aplicações lógicas do Azure](../connectors/apis-list.md)