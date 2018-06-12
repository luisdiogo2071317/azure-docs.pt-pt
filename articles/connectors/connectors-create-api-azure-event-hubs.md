---
title: Ligar ao Event Hubs do Azure - as do Azure Logic Apps | Microsoft Docs
description: Gerir e monitorizar os eventos com o Event Hubs do Azure e Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/21/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 86fc1c3542bea1be840041bb73df15631c066c7e
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294977"
---
# <a name="monitor-receive-and-send-events-with-azure-event-hubs-and-azure-logic-apps"></a>Monitorizar, receber e enviar eventos com o Event Hubs do Azure e Azure Logic Apps 

Este artigo mostra como pode monitorizar e gerir eventos enviados para o [Event Hubs do Azure](../event-hubs/event-hubs-what-is-event-hubs.md) de dentro de uma aplicação lógica com o conector de Event Hubs do Azure. Dessa forma, pode criar as logic apps que automatizem tarefas e fluxos de trabalho para a verificação, enviar e receber eventos a partir do seu Hub de eventos. 

Se não tiver uma subscrição do Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscreva-se para obter uma conta do Azure gratuita</a>. Se estiver familiarizado com as logic apps, reveja [que é o Azure Logic Apps](../logic-apps/logic-apps-overview.md) e [início rápido: criar a sua primeira aplicação de lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Para obter informações técnicas específicas do conector, consulte o <a href="https://docs.microsoft.com/connectors/eventhubs/" target="blank">Event Hubs do Azure conector referência</a>.

## <a name="prerequisites"></a>Pré-requisitos

* Um [espaço de nomes de Event Hubs do Azure e o Hub de eventos](../event-hubs/event-hubs-create.md)

* A aplicação lógica onde pretende aceder ao seu Hub de eventos. Para começar a sua aplicação lógica com um acionador de Event Hubs do Azure, é necessário um [aplicação lógica em branco](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

<a name="permissions-connection-string"></a>

## <a name="check-permissions-and-get-connection-string"></a>Verifique as permissões e obter a cadeia de ligação

Para a sua aplicação lógica aceder ao seu Hub de eventos, verifique as permissões e obter a cadeia de ligação para o seu espaço de nomes de Event Hubs.

1. Inicie sessão no <a href="https://portal.azure.com" target="_blank">portal do Azure</a>. 

2. Aceda ao seu Event Hubs *espaço de nomes*, não um Hub de eventos específico. Na página de espaço de nomes, em **definições**, escolha **políticas de acesso partilhado**. Em **afirmações**, verifique se tem **gerir** permissões para esse espaço de nomes.

   ![Gerir permissões do seu espaço de nomes do Hub de eventos](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3. Se pretender mais tarde manualmente introduza as suas informações de ligação, obter a cadeia de ligação para o seu espaço de nomes de Event Hubs. 

   1. Em **política**, escolha **RootManageSharedAccessKey**. 

   2. Localize a cadeia de ligação da sua chave primária. Escolha o botão Copiar e guardar a cadeia de ligação para utilização posterior.

      ![Copie a cadeia de ligação de espaço de nomes de Event Hubs](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

      > [!TIP]
      > Para confirmar se a cadeia de ligação é associada o espaço de nomes de Event Hubs ou com um hub de eventos específico, certifique-se a cadeia de ligação não tem o `EntityPath` parâmetro. Se encontrar este parâmetro, a cadeia de ligação é para um Hub de eventos específico "entidade" e não é a cadeia correta a utilizar com a sua aplicação lógica.

4. Agora continue com [adicionar um acionador de Event Hubs](#add-trigger) ou [adicionar uma ação de Event Hubs](#add-action).

<a name="add-trigger"></a>

## <a name="add-an-event-hubs-trigger"></a>Adicionar um acionador de Event Hubs

No Azure Logic Apps, cada aplicação lógica tem de começar com uma [acionador](../logic-apps/logic-apps-overview.md#logic-app-concepts), que é desencadeado quando um evento específico acontece ou quando for cumprida uma condição específica. Sempre que o acionador é desencadeado, o motor de Logic Apps cria uma instância de aplicação lógica e inicia a executar o fluxo de trabalho da sua aplicação.

Este exemplo mostra como pode iniciar um fluxo de trabalho de aplicação lógica quando novos eventos são enviados para o Hub de eventos. 

1. No portal do Azure ou Visual Studio, crie uma aplicação de lógica em branco, o qual abre o Designer de aplicações lógicas. Este exemplo utiliza o portal do Azure.

2. Na caixa de pesquisa, introduza "os hubs de eventos" como o filtro. Na lista de acionadores, selecione o acionador que quiser. 

   Este exemplo utiliza este acionador: **Hubs de eventos - quando eventos estão disponíveis no Hub de eventos**

   ![Selecione o acionador](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

3. Se lhe for pedida para detalhes de ligação, [criar a ligação de Event Hubs agora](#create-connection). Em alternativa, se a ligação já existir, forneça as informações necessárias para o acionador.

   1. Do **nome de Hub de eventos** lista, selecione o Hub de eventos que pretende monitorizar.

      ![Especifique o Hub de eventos ou grupo de consumidores](./media/connectors-create-api-azure-event-hubs/select-event-hub.png)

   2. Selecione o intervalo de frequência e para a frequência com que pretende que o acionador para verificar o Hub de eventos.
 
   3. Para, opcionalmente, selecione algumas das opções avançadas de Acionador, escolha **Mostrar opções avançadas**.
   
      ![Acionar opções avançadas](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-advanced.png)

      | Propriedade | Detalhes | 
      |----------|---------| 
      | Tipo de conteúdo  | Selecione o tipo de conteúdo do evento. A predefinição é "application/octet-stream". |
      | Esquema do conteúdo | Introduza o esquema do conteúdo no JSON para os eventos que são lidos a partir do Event Hub. |
      | Nome do grupo de consumidores | Introduza o Hub de eventos [nome do grupo de consumidores](../event-hubs/event-hubs-features.md#consumer-groups) para ler eventos. Se não for especificado, é utilizado o grupo de consumidores predefinido. |
      | Chave de partição mínima | Introduza o mínimo [partição](../event-hubs/event-hubs-features.md#partitions) ID ler. Por predefinição, todas as partições são de leitura. |
      | Chave de partição máximo | Introduza o número máximo [partição](../event-hubs/event-hubs-features.md#partitions) ID ler. Por predefinição, todas as partições são de leitura. |
      | Contagem máxima de eventos | Introduza um valor para o número máximo de eventos. Devolve o acionador entre um e o número de eventos especificado por esta propriedade. |
      |||

4. Quando tiver terminado, na barra de ferramentas estruturador, escolha **guardar**.

5. Agora continue a adicionar uma ou mais ações à sua aplicação lógica para as tarefas que pretende executar com os resultados de Acionador.

> [!NOTE]
> Todos os acionadores de Hub de eventos *longa consulta* acionadores, o que significa que, quando um acionador é acionado, o acionador processa todos os eventos e, em seguida, aguarda 30 segundos mais eventos a aparecer no seu Hub de eventos.
> Se não há eventos recebidos em 30 segundos, é ignorada a execução do acionador. Caso contrário, o acionador continua ler eventos até que o seu Hub de eventos está vazio.
> A seguinte consulta de Acionador acontece com base no intervalo de periodicidade que especificou nas propriedades do acionador.

<a name="add-action"></a>

## <a name="add-an-event-hubs-action"></a>Adicionar uma ação de Event Hubs

No Azure Logic Apps, um [ação](../logic-apps/logic-apps-overview.md#logic-app-concepts) é um passo no fluxo de trabalho que se segue um acionador ou outra ação. Neste exemplo, a aplicação lógica começa com um acionador de Event Hubs que verifica a existência de novos eventos do seu Hub de eventos. 

1. No portal do Azure ou Visual Studio, abra a aplicação de lógica no Designer de aplicações lógicas. Este exemplo utiliza o portal do Azure.

2. Sob o acionador ou ação, escolha **novo passo** > **adicionar uma ação**.

   Para adicionar uma ação entre passos existentes, mova o rato sobre a seta para a ligação. 
   Escolha o sinal de adição (**+**) que é apresentado e, em seguida, escolha **adicionar uma ação**.

3. Na caixa de pesquisa, introduza "os hubs de eventos" como o filtro.
Na lista de ações, selecione a ação que pretende. 

   Para este exemplo, selecione esta ação: **Hubs de eventos - eventos de envio**

   ![Selecione "Os Hubs de eventos - eventos de envio"](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

4. Se lhe for pedida para detalhes de ligação, [criar a ligação de Event Hubs agora](#create-connection). Em alternativa, se a ligação já existir, forneça as informações necessárias para a ação.

   | Propriedade | Necessário | Descrição | 
   |----------|----------|-------------|
   | O nome do hub de eventos | Sim | Selecione o Hub de eventos em que pretende enviar o evento | 
   | Conteúdo de eventos | Não | O conteúdo para o evento que pretende enviar | 
   | Propriedades | Não | Os valores para enviar e as propriedades da aplicação | 
   |||| 

   Por exemplo: 

   ![Selecione o nome de Hub de eventos e forneça o conteúdo de eventos](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

5. Quando tiver terminado, na barra de ferramentas estruturador, escolha **guardar**.

<a name="create-connection"></a>

## <a name="connect-to-your-event-hub"></a>Ligar ao seu Hub de eventos

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)] 

1. Quando lhe for pedida informações de ligação, forneça estes detalhes:

   | Propriedade | Necessário | Valor | Descrição | 
   |----------|----------|-------|-------------|
   | Nome da Ligação | Sim | <*nome da ligação*> | O nome para criar para a ligação |
   | Espaço de nomes dos Event Hubs | Sim | <*Event-hubs-espaço de nomes*> | Selecione o espaço de nomes de Hubs de eventos que pretende utilizar. | 
   |||||  

   Por exemplo:

   ![Criar a ligação do Hub de eventos](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-1.png)

   Para inserirem manualmente a cadeia de ligação, escolha **introduzir manualmente as informações de ligação**. 
   Saiba [como encontrar a cadeia de ligação](#permissions-connection-string).

2. Selecione a política de Hubs de eventos a utilizar, se ainda não estiver selecionada. Escolha **Criar**.

   ![Criar a ligação do Hub de eventos, parte 2](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-2.png)

3. Depois de criar a ligação, prossiga com [adicionar Event Hubs acionar](#add-trigger) ou [ação adicionar Event Hubs](#add-action).

## <a name="connector-reference"></a>Referência do conector

Para obter detalhes técnicos, como acionadores, ações e limites, conforme descrito pelo ficheiro Swagger do conector, consulte o [página de referência do conector](/connectors/eventhubs/). 

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para submeter ou votar em ideias para funcionalidades, visite o [site de comentários dos utilizadores do Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos seguintes

* Saiba mais sobre outros [conectores Logic Apps](../connectors/apis-list.md)