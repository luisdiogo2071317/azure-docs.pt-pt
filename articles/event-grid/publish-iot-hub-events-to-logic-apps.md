---
title: Utilizar o IoT Hub eventos acionar Azure Logic Apps | Microsoft Docs
description: "Através do serviço de encaminhamento de eventos da grelha de eventos do Azure, crie os processos automatizados para efetuar ações de Azure Logic Apps com base em eventos de IoT Hub."
services: iot-hub
documentationcenter: 
author: kgremban
manager: timlt
editor: 
ms.service: iot-hub
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2018
ms.author: kgremban
ms.openlocfilehash: f54db95b0dfe5dc39c8e2a85375e56a93d1562ee
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="send-email-notifications-about-azure-iot-hub-events-using-logic-apps"></a>Enviar notificações por e-mail sobre eventos de IoT Hub do Azure utilizando as Logic Apps

Grelha de eventos do Azure permite-lhe reagir a eventos num IoT Hub por a acionar ações nas suas aplicações empresariais a jusante.

Este artigo explica uma configuração de exemplo que utiliza o IoT Hub e a grelha de eventos. Por fim, terá uma aplicação lógica Azure configurado para enviar um e-mail de notificação, sempre que um dispositivo é adicionado ao seu IoT hub. 

## <a name="prerequisites"></a>Pré-requisitos

* Uma conta de e-mail a partir de qualquer fornecedor de correio eletrónico que é suportado pelo Azure Logic Apps, como o Outlook do Office 365, Outlook.com ou Gmail. Esta conta de correio eletrónico é utilizada para enviar notificações de eventos. Para obter uma lista completa dos conectores de aplicação lógica suportados, consulte o [descrição geral de conectores](https://docs.microsoft.com/connectors/)
* Uma conta ativa do Azure. Se não tiver uma, pode [criar uma conta gratuita](http://azure.microsoft.com/pricing/free-trial/).
* Um Iot hub do Azure. Se ainda não criou um ainda, consulte [introdução ao IoT Hub](../iot-hub/iot-hub-csharp-csharp-getstarted.md) para obter instruções. 

## <a name="create-a-logic-app"></a>Criar uma aplicação lógica

Em primeiro lugar, crie uma aplicação lógica e adicionar um acionador de grelha de eventos que monitoriza o grupo de recursos para a máquina virtual. 

### <a name="create-a-logic-app-resource"></a>Crie um recurso de aplicação lógica


1. No [portal do Azure](https://portal.azure.com), selecione **novo** > **integração empresarial com** > **aplicação lógica**.

   ![Criar uma aplicação lógica](./media/publish-iot-hub-events-to-logic-apps/select-logic-app.png)

2. Dar um nome que seja exclusivo na sua subscrição a sua aplicação lógica, em seguida, selecione a mesma subscrição, grupo de recursos e localização como o seu IoT hub. 
3. Quando estiver pronto, selecione **afixar ao dashboard**e escolha **criar**.

   Acabou de criar um recurso do Azure para a aplicação lógica. Quando o Azure implementar a sua aplicação lógica, o Estruturador de Aplicações Lógicas mostra-lhe modelos de padrões comuns, para que possa começar mais depressa.

   > [!NOTE] 
   > Quando seleciona **afixar ao dashboard**, a aplicação lógica abre automaticamente no Designer de aplicações lógicas. Caso contrário, pode manualmente localize e abra a aplicação lógica.

4. No Designer de aplicação lógica em **modelos**, escolha **aplicação lógica em branco** para que pode criar a sua aplicação de lógica a partir do zero.

## <a name="select-a-trigger"></a>Selecione um acionador

Um acionador é um evento específico que inicia a sua aplicação lógica. Para este tutorial, o acionador que define a desativar o fluxo de trabalho está a receber um pedido através de HTTP.  

1. Na barra de pesquisa de acionadores e conectores, escreva **HTTP**.
2. Selecione **pedido - pedido de HTTP de quando é recebido** como o acionador. 

   ![Selecione o acionador de pedido HTTP](./media/publish-iot-hub-events-to-logic-apps/http-request-trigger.png)

3. Selecione **payload de exemplo de utilização para gerar o esquema**. 

   ![Selecione o acionador de pedido HTTP](./media/publish-iot-hub-events-to-logic-apps/sample-payload.png)

4. Colar o seguinte código JSON de exemplo na caixa de texto, em seguida, selecione **feito**:

   ```json
   [{
     "id": "56afc886-767b-d359-d59e-0da7877166b2",
     "topic": "/SUBSCRIPTIONS/<Subscription ID>/RESOURCEGROUPS/<Resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<IoT hub name>",
     "subject": "devices/LogicAppTestDevice",
     "eventType": "Microsoft.Devices.DeviceCreated",
     "eventTime": "2018-01-02T19:17:44.4383997Z",
     "data": {
       "twin": {
         "deviceId": "LogicAppTestDevice",
         "etag": "AAAAAAAAAAE=",
         "status": "enabled",
         "statusUpdateTime": "0001-01-01T00:00:00",
         "connectionState": "Disconnected",
         "lastActivityTime": "0001-01-01T00:00:00",
         "cloudToDeviceMessageCount": 0,
         "authenticationType": "sas",
         "x509Thumbprint": {
           "primaryThumbprint": null,
           "secondaryThumbprint": null
         },
         "version": 2,
         "properties": {
           "desired": {
             "$metadata": {
               "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
             },
             "$version": 1
           },
           "reported": {
             "$metadata": {
               "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
             },
             "$version": 1
           }
         }
       },
       "hubName": "egtesthub1",
       "deviceId": "LogicAppTestDevice",
       "operationTimestamp": "2018-01-02T19:17:44.4383997Z",
       "opType": "DeviceCreated"
     },
     "dataVersion": "",
     "metadataVersion": "1"
   }]
   ```
5. Poderá receber uma notificação de pop-up que diz: **não se esqueça de incluir um cabeçalho Content-Type definido como aplicação/json no seu pedido.** Em segurança pode ignorar este sugestão e avançar para a secção seguinte. 


### <a name="create-an-action"></a>Criar uma ação

As ações são quaisquer passos que ocorrerem depois do acionador inicia o fluxo de trabalho de aplicação lógica. Para este tutorial, a ação é enviar uma notificação por e-mail através do seu fornecedor de correio eletrónico. 

1. Selecione **novo passo** , em seguida, **adicionar uma ação**. 

   ![Novo passo, adicionar uma ação](./media/publish-iot-hub-events-to-logic-apps/new-step.png)

2. Procurar **E-Mail**. 
3. Com base no seu fornecedor de e-mail, localize e selecione o conector correspondente. Este tutorial utiliza **Outlook do Office 365**. Os passos para outros fornecedores de correio eletrónico são semelhantes. 

   ![Selecione o conector do fornecedor de e-mail](./media/publish-iot-hub-events-to-logic-apps/o365-outlook.png)

4. Selecione o **enviar um e-mail** ação. 
5. Se lhe for solicitado, inicie sessão na sua conta de e-mail. 
6. Crie o modelo de correio eletrónico. 
   * **Para**: introduza o endereço de e-mail para receber as notificações por e-mail. Para este tutorial, utilize uma conta de e-mail que tem acesso para fins de teste. 
   * **Requerente** e **corpo**: escrever texto para o seu e-mail. Selecione as propriedades de JSON da ferramenta de Seletor para incluir conteúdo dinâmico com base nos dados de eventos.  

   O modelo de correio eletrónico pode aspeto neste exemplo:

   ![Preencha as informações de e-mail](./media/publish-iot-hub-events-to-logic-apps/email-content.png)

7. Guarde a aplicação lógica. 

### <a name="copy-the-http-url"></a>Copie o URL de HTTP

Antes de sair do Designer de aplicações lógicas, copie o URL que as logic apps estão a escutar para um acionador. Utilize este URL para configurar a grelha de eventos. 

1. Expanda o **quando HTTP de pedido é recebido** caixa de Acionador de configuração ao clicar no mesmo. 
2. Copie o valor da **HTTP POST URL** selecionando o botão Copiar junto ao mesmo. 

   ![Copie o URL de POST de HTTP](./media/publish-iot-hub-events-to-logic-apps/copy-url.png)

3. Guarde este URL, de modo a que pode fazer referência na secção seguinte. 

## <a name="publish-an-event-from-iot-hub"></a>Publicar um evento do IoT Hub

Nesta secção, configure o seu IoT Hub para publicar eventos à medida que ocorrem. 

1. No portal do Azure, navegue até ao seu IoT hub. 
2. Selecione **eventos grelha**.

   ![Abra os detalhes da grelha de eventos](./media/publish-iot-hub-events-to-logic-apps/event-grid.png)

3. Selecione **de subscrição de evento**. 

   ![Criar nova subscrição de evento](./media/publish-iot-hub-events-to-logic-apps/event-subscription.png)

4. Crie a subscrição de eventos com os seguintes valores: 
   * **Nome**: forneça um nome descritivo.
   * **Subscrever todos os tipos de evento**: desmarcar a caixa de verificação.
   * **Os tipos de evento**: selecione **DeviceCreated**.
   * **Tipo de subscritor**: selecione **Web Hook**.
   * **Ponto final de subscritor**: cole o URL que copiou da sua aplicação lógica. 

   Foi possível guardar a subscrição de evento aqui e receber notificações para cada dispositivo que é criado no seu IoT hub. Para este tutorial, vamos utilizar os campos opcionais para filtrar para dispositivos específicos: 

   * **Prefixo filtro**: introduza `devices/Building1_` para filtrar eventos de dispositivo na criação de 1.
   * **Filtro de sufixo**: introduza `_Temperature` para filtrar eventos relacionados com a temperatura do dispositivo.

   Quando estiver pronto, o formulário deverá ter um aspeto semelhante ao seguinte exemplo: 

   ![Formato de subscrição de eventos de exemplo](./media/publish-iot-hub-events-to-logic-apps/subscription-form.png)

5. Selecione **criar** ao guardar a subscrição de evento.

## <a name="create-a-new-device"></a>Criar um novo dispositivo

Teste a sua aplicação lógica através da criação de um novo dispositivo para acionar um e-mail de notificação de evento. 

1. A partir do seu IoT hub, selecione **dispositivos IoT**. 
2. Selecione **Adicionar**.
3. Para **ID de dispositivo**, introduza `Building1_Floor1_Room1_Temperature`.
4. Selecione **Guardar**. 
5. Pode adicionar vários dispositivos com os IDs para testar os filtros de subscrição de evento do dispositivo diferente. Experimente estes exemplos: 
   * Building1_Floor1_Room1_Light
   * Building1_Floor2_Room2_Temperature
   * Building2_Floor1_Room1_Temperature
   * Building2_Floor1_Room1_Light

Depois de alguns dispositivos que adicionou ao seu IoT hub, verifique o seu correio eletrónico para ver aqueles acionada a aplicação lógica. 

## <a name="use-the-azure-cli"></a>Utilizar a CLI do Azure

Em vez de utilizar o portal do Azure, pode realizar os passos do IoT Hub com a CLI do Azure. Para obter mais informações, consulte as páginas da CLI do Azure para [criar uma subscrição de evento](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription) e [criação de um dispositivo IoT](https://docs.microsoft.com/cli/azure/iot/device)

## <a name="clean-up-resources"></a>Limpar recursos

Este tutorial utilizar recursos de pagar a sua subscrição do Azure. Quando terminar, experimentar o tutorial e os resultados de testes, desativar ou eliminar os recursos que não pretende manter. 

Se não quiser perder o trabalho na sua aplicação lógica, desative-o em vez de eliminá-lo. 

1. Navegue até à sua aplicação lógica.
2. No **descrição geral** painel selecione **eliminar** ou **desativar**. 

Cada subscrição pode ter um IoT hub gratuito. Se tiver criado um hub gratuito para este tutorial, não precisa de eliminá-la para evitar encargos.

1. Navegue até ao seu IoT hub. 
2. No **descrição geral** painel selecione **eliminar**. 

Mesmo se manter o seu IoT hub, pode pretender eliminar a subscrição de evento que criou. 

1. No seu IoT hub, selecione **eventos grelha**.
2. Selecione a subscrição de evento que pretende remover. 
3. Selecione **Eliminar**. 

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre [a agir os eventos de IoT Hub, utilizando a grelha de evento a acionar ações](../iot-hub/iot-hub-event-grid.md).

Saiba mais sobre o que a pessoa que pode fazer com [eventos grelha](overview.md).


