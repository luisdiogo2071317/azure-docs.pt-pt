---
title: Configurar a monitorização de eventos com o Event Hubs do Azure para o Azure Logic Apps | Microsoft Docs
description: Monitorizar os fluxos de dados para receber eventos e enviar eventos com as logic apps com o Event Hubs do Azure
services: logic-apps
keywords: fluxo de dados, o monitor de eventos, os event hubs
author: ecfan
manager: anneta
editor: ''
documentationcenter: ''
tags: connectors
ms.assetid: ''
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/06/2018
ms.author: estfan; LADocs
ms.openlocfilehash: 8de56cd64f38791fb27d9bcce1e16641fb162c2f
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="monitor-receive-and-send-events-with-the-event-hubs-connector"></a>Monitorizar, receber e enviar eventos com o conector de Event Hubs

Para configurar um monitor de eventos, para que a aplicação lógica pode detetar eventos, receber eventos e enviar eventos, ligar a um [Hub de eventos do Azure](https://azure.microsoft.com/services/event-hubs) da sua aplicação lógica. Saiba mais sobre [Event Hubs do Azure](../event-hubs/event-hubs-what-is-event-hubs.md) e [como preços funciona para os conectores de aplicações lógicas](../logic-apps/logic-apps-pricing.md).

## <a name="prerequisites"></a>Pré-requisitos

Antes de poder utilizar o conector de Event Hubs, tem de ter estes itens:

* Um [espaço de nomes de Event Hubs do Azure e o Hub de eventos](../event-hubs/event-hubs-create.md)
* A [aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="permissions-connection-string"></a>

## <a name="connect-to-azure-event-hubs"></a>Ligar ao Event Hubs do Azure

Antes da aplicação lógica pode aceder a qualquer serviço, tem de criar um [ *ligação* ](./connectors-overview.md) entre a sua aplicação lógica e o serviço, se ainda não o fez. Esta ligação autoriza a sua aplicação lógica para aceder a dados. Para a sua aplicação lógica aceder ao seu Hub de eventos, verifique as permissões e obter a cadeia de ligação para o seu espaço de nomes de Event Hubs.

1.  Inicie sessão no [portal do Azure](https://portal.azure.com "portal do Azure"). 

2.  Aceda ao seu Event Hubs *espaço de nomes*, não um Hub de eventos específico. Na página de espaço de nomes, em **definições**, escolha **políticas de acesso partilhado**. Em **afirmações**, verifique se tem **gerir** permissões para esse espaço de nomes.

    ![Gerir permissões do seu espaço de nomes do Hub de eventos](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3. Se pretender mais tarde manualmente introduza as suas informações de ligação, obter a cadeia de ligação para o seu espaço de nomes de Event Hubs. Escolha **RootManageSharedAccessKey**. Junto a cadeia de ligação de chave primária, escolha o botão Copiar. Guarde a cadeia de ligação para utilização posterior.

    ![Copie a cadeia de ligação de espaço de nomes de Event Hubs](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

    > [!TIP]
    > Para confirmar se a cadeia de ligação é associada o espaço de nomes de Event Hubs ou com um hub de eventos específicos, verifique a cadeia de ligação para o `EntityPath` parâmetro. Se encontrar este parâmetro, a cadeia de ligação é para um Hub de eventos específico "entidade" e não é a cadeia correta a utilizar com a sua aplicação lógica.

## <a name="trigger-workflow-when-your-event-hub-gets-new-events"></a>Acionar o fluxo de trabalho quando o seu Hub de eventos obtém a eventos novos.

A [ *acionador* ](../logic-apps/logic-apps-overview.md#logic-app-concepts) é um evento que inicia a um fluxo de trabalho na sua aplicação lógica. Para iniciar um fluxo de trabalho quando novos eventos são enviados para o Hub de eventos, siga estes passos para adicionar o acionador que Deteta este evento.

1. No [portal do Azure](https://portal.azure.com "portal do Azure"), aceda à sua aplicação lógica existente ou criar uma aplicação lógica em branco.

2. No Designer de aplicações de lógica, introduza "event hubs" na caixa de pesquisa, como o filtro. Selecione este acionador: **quando os eventos estão disponíveis no Hub de eventos**

   ![Selecione o acionador para quando o seu Hub de eventos recebe eventos novos.](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

   1. Se ainda não tiver uma ligação para o espaço de nomes de Event Hubs, é-lhe pedido que criar agora esta ligação. Dê um nome da ligação e selecione o espaço de nomes de Hubs de eventos que pretende utilizar.

      ![Criar a ligação do Hub de eventos](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-1.png)

      Ou, para inserirem manualmente a cadeia de ligação, escolha **introduzir manualmente as informações de ligação**. 
      Saiba [como encontrar a cadeia de ligação](#permissions-connection-string).

   2. Agora selecione a política de Hubs de eventos a utilizar e escolha **criar**.

      ![Criar a ligação do Hub de eventos, parte 2](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-2.png)

3. Selecione o Hub de eventos para monitorizar e configurar o intervalo de frequência e para quando procurar o Hub de eventos.

    ![Especifique o Hub de eventos ou grupo de consumidores](./media/connectors-create-api-azure-event-hubs/select-event-hub.png)
    
    > [!NOTE]
    > Todos os acionadores de Hub de eventos *longa consulta* acionadores, o que significa que, quando um acionador é acionado, o acionador processa todos os eventos e, em seguida, aguarda 30 segundos mais eventos de eventos apareça Hub.
    > Se não há eventos recebidos em 30 segundos, é ignorada a execução do acionador. Caso contrário, o acionador continua ler eventos até que o Hub de eventos está vazio.
    > A seguinte consulta de Acionador baseia-se no intervalo de periodicidade especificado nas propriedades do acionador.


4. Para, opcionalmente, selecione algumas das opções avançadas de Acionador, escolha **Mostrar opções avançadas**.

    ![Acionar opções avançadas](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-advanced.png)

    | Propriedade | Detalhes |
    | --- | --- |
    | Tipo de conteúdo  |Selecione o tipo de conteúdo dos eventos na lista pendente. Por predefinição, está selecionado application/octet-stream. |
    | Esquema do conteúdo |Introduza o esquema do conteúdo no JSON para os eventos que são lidos a partir do Event Hub. |
    | Nome do grupo de consumidores |Introduza o Hub de eventos [nome do grupo de consumidores](../event-hubs/event-hubs-features.md#consumer-groups) para leitura dos eventos. Quando não for especificado o nome do grupo de consumidores, é utilizado o grupo de consumidores predefinido. |
    | Chave de partição mínima |Introduza o mínimo [partição](../event-hubs/event-hubs-features.md#partitions) ID ler. Por predefinição, todas as partições são de leitura. |
    | Chave de partição máximo |Introduza o número máximo [partição](../event-hubs/event-hubs-features.md#partitions) ID ler. Por predefinição, todas as partições são de leitura. |
    | Contagem máxima de eventos |Introduza um valor para o número máximo de eventos. Devolve o acionador entre um e o número de eventos especificado por esta propriedade. |
    |||

5. Guarde a aplicação lógica. Na barra de ferramentas do estruturador, escolha **Guardar**.

Agora, quando a sua aplicação lógica verifica o Hub de eventos selecionada e localiza um novo evento, o acionador executa as ações na sua aplicação lógica para o evento encontrado.

## <a name="send-events-to-your-event-hub-from-your-logic-app"></a>Enviar eventos para o Hub de eventos a partir da sua aplicação lógica

Uma [*ação*](../logic-apps/logic-apps-overview.md#logic-app-concepts) é uma tarefa realizada pelo fluxo de trabalho da sua aplicação lógica. Depois de adicionar um acionador à aplicação lógica, pode adicionar uma ação para realizar operações com dados gerados por esse acionador. Para enviar um evento para o Hub de eventos da sua aplicação lógica, siga estes passos.

1. No Designer de aplicações lógicas, sob o acionador, escolha **novo passo** > **adicionar uma ação**.

2. Na caixa de pesquisa, introduza "os hubs de eventos" como o filtro.
Selecione esta ação: **Hubs de eventos - eventos de envio**

   ![Selecione "Os Hubs de eventos - eventos de envio"](./media/connectors-create-api-azure-event-hubs/select-event-hubs-send-event-action.png)

3. Selecione o Hub de eventos para onde enviar o evento. Em seguida, introduza o conteúdo de eventos e quaisquer outros detalhes.

   ![Selecione o nome de Hub de eventos e forneça o conteúdo de eventos](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

4. Guarde a aplicação lógica.

Configurou uma ação que envia eventos a partir da sua aplicação lógica. 

## <a name="connector-specific-details"></a>Detalhes específicos do conector

Para saber mais sobre acionadores e ações definidas por quaisquer limites e o ficheiro Swagger, reveja o [detalhes do conector](/connectors/eventhubs/).

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para submeter ou votar em ideias para funcionalidades, visite o [site de comentários dos utilizadores do Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [outros conectores para aplicações lógicas do Azure](../connectors/apis-list.md)