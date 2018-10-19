---
title: Monitorização remota do IoT e notificações com o Azure Logic Apps | Documentos da Microsoft
description: Utilize o Azure Logic Apps para monitorização de temperatura de IoT no seu hub IoT e enviar automaticamente notificações por e-mail para sua caixa de correio para quaisquer anomalias detetadas.
author: rangv
manager: ''
keywords: IOT de monitoramento, notificações de iot, monitorização de temperatura de iot
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: adda4e948c11f84517b1e8dd01e6cfe42155e1ca
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/18/2018
ms.locfileid: "49409446"
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a>A monitorização remota do IoT e notificações com o Azure Logic Apps, ligar o seu IoT hub e caixa de correio

![Diagrama de ponto a ponto](media/iot-hub-get-started-e2e-diagram/7.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

O Azure Logic Apps fornece uma forma de automatizar processos como uma série de passos. Uma aplicação lógica pode ligar-se em vários serviços e protocolos. Ele começa com um acionador, tal como ' Quando uma conta é adicionada' e seguida por uma combinação de ações, um como "envio de notificações push". Esta funcionalidade torna as Logic Apps uma solução de IoT perfeita IoT monitorização, por exemplo, mantendo-se alerta para anomalias, entre outros cenários de utilização.

## <a name="what-you-learn"></a>O que irá aprender

Saiba como criar uma aplicação lógica que liga o seu IoT hub e caixa de correio para o monitoramento de temperatura e notificações. Quando a temperatura for superior a 30 C, a aplicação cliente marca `temperatureAlert = "true"` na mensagem envia ao seu hub IoT. A mensagem aciona a aplicação lógica irá enviar-lhe uma notificação por e-mail.

## <a name="what-you-do"></a>O que fazer

* Criar um espaço de nomes do barramento de serviço e adicionar-lhe uma fila.
* Adicione um ponto de extremidade e uma regra de roteamento ao seu hub IoT.
* Criar, configurar e testar uma aplicação lógica.

## <a name="what-you-need"></a>Do que precisa

* Tutorial [configurar o seu dispositivo](iot-hub-raspberry-pi-kit-node-get-started.md) concluída que abrange os seguintes requisitos:
  * Uma subscrição ativa do Azure.
  * Um hub IoT do Azure com a sua subscrição.
  * Uma aplicação de cliente que envia mensagens para o seu hub IoT do Azure.

## <a name="create-service-bus-namespace-and-add-a-queue-to-it"></a>Criar o espaço de nomes do service bus e adicionar-lhe uma fila

### <a name="create-a-service-bus-namespace"></a>Criar um espaço de nomes do service bus

1. Sobre o [portal do Azure](https://portal.azure.com/), clique em **criar um recurso** > **integração empresarial** > **Service Bus**.
1. Forneça as seguintes informações:

   **Nome**: O nome do barramento de serviço.

   **Escalão de preço**: clique em **básica** > **selecionar**. O escalão básico é suficiente para este tutorial.

   **Grupo de recursos**: utilizar o mesmo grupo de recursos que utiliza o seu hub IoT.

   **Localização**: utilizar a mesma localização que utiliza o seu hub IoT.
1. Clique em **Criar**.

   ![Criar um espaço de nomes do service bus no portal do Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1_create-service-bus-namespace-azure-portal.png)

### <a name="add-a-service-bus-queue"></a>Adicionar uma fila do service bus

1. Abra o espaço de nomes do service bus e, em seguida, clique em **+ fila**.
1. Introduza um nome para a fila e, em seguida, clique em **criar**.
1. Abrir a fila do service bus e, em seguida, clique em **políticas de acesso partilhado** > **+ adicionar**.
1. Introduza um nome para a política, verificação **Manage**e, em seguida, clique em **criar**.

   ![Adicionar uma fila do service bus no portal do Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2_add-service-bus-queue-azure-portal.png)

## <a name="add-an-endpoint-and-a-routing-rule-to-your-iot-hub"></a>Adicionar um ponto de extremidade e uma regra de roteamento ao seu hub IoT

### <a name="add-an-endpoint"></a>Adicionar um ponto final

1. Abra o hub IoT, clique em pontos de extremidade > + Adicionar.
1. Introduza as seguintes informações:

   **Nome**: O nome do ponto de extremidade.

   **Tipo de ponto final**: selecione **fila do Service Bus**.

   **Espaço de nomes do Service Bus**: selecione o espaço de nomes que criou.

   **Fila do Service Bus**: selecione a fila que criou.
1. Clique em **OK**.

   ![Adicionar um ponto final ao seu hub IoT no portal do Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3_add-iot-hub-endpoint-azure-portal.png)

### <a name="add-a-routing-rule"></a>Adicionar uma regra de encaminhamento

1. No seu IoT hub, clique em **rotas** > **+ adicionar**.
1. Introduza as seguintes informações:

   **Nome**: O nome da regra de encaminhamento.

   **Origem de dados**: selecione **DeviceMessages**.

   **Ponto final**: selecione o ponto final que criou.

   **Cadeia de consulta**: introduza `temperatureAlert = "true"`.
1. Clique em **Guardar**.

   ![Adicionar uma regra de roteamento no portal do Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4_add-routing-rule-azure-portal.png)

## <a name="create-and-configure-a-logic-app"></a>Criar e configurar uma aplicação lógica

### <a name="create-a-logic-app"></a>Criar uma aplicação lógica

1. Na [portal do Azure](https://portal.azure.com/), clique em **criar um recurso** > **integração empresarial** > **aplicação lógica**.
1. Introduza as seguintes informações:

   **Nome**: O nome da aplicação lógica.

   **Grupo de recursos**: utilizar o mesmo grupo de recursos que utiliza o seu hub IoT.

   **Localização**: utilizar a mesma localização que utiliza o seu hub IoT.
1. Clique em **Criar**.

### <a name="configure-the-logic-app"></a>Configurar a aplicação lógica

1. Abra a aplicação lógica que é aberta no Designer de aplicações lógicas.
1. No estruturador de aplicações lógicas, clique em **aplicação lógica em branco**.

   ![Começar com uma aplicação lógica em branco no portal do Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5_start-with-blank-logic-app-azure-portal.png)

1. Clique em **Service Bus**.

   ![Selecionar barramento de serviço para começar a criar a sua aplicação lógica no portal do Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6_select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. Clique em **do Service Bus – quando uma ou mais mensagens chegam a uma fila (concluir automaticamente)**.
1. Crie uma ligação de barramento de serviço.
   1. Introduza um nome de ligação.
   1. Clique no espaço de nomes do service bus > política do service bus > **criar**.

      ![Criar uma ligação de barramento de serviço para a aplicação lógica no portal do Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7_create-service-bus-connection-in-logic-app-azure-portal.png)

   1. Clique em **continuar** depois de criar a ligação de barramento de serviço.
   1. Selecione a fila que criou e introduza `175` para **número máximo de mensagem**

      ![Especifique o número máximo de mensagens para a ligação de barramento de serviço na sua aplicação lógica](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8_specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)
   1. Clique em "Salvar" do botão para guardar as alterações.

1. Crie uma ligação de serviço de SMTP.
   1. Clique em **novo passo** > **adicionar uma ação**.
   1. Tipo `SMTP`, clique nas **SMTP** serviço no resultado da pesquisa e, em seguida, clique em **SMTP - enviar correio eletrónico**.

      ![Criar uma ligação de SMTP na sua aplicação lógica no portal do Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9_create-smtp-connection-logic-app-azure-portal.png)

   1. Introduza as informações de SMTP da sua caixa de correio e, em seguida, clique em **criar**.

      ![Introduza as informações de ligação de SMTP na sua aplicação lógica no portal do Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10_enter-smtp-connection-info-logic-app-azure-portal.png)

      Obtenha as informações de SMTP para [Hotmail/Outlook.com](https://support.office.com/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en), e [Yahoo correio](https://help.yahoo.com/kb/SLN4075.html).
   1. Introduza o seu endereço de e-mail para **partir** e **para**, e `High temperature detected` para **assunto** e **corpo**.
   1. Clique em **Guardar**.

A aplicação lógica está em condição de trabalho quando salvá-lo.

## <a name="test-the-logic-app"></a>Testar a aplicação lógica

1. Iniciar a aplicação de cliente que implementa para o dispositivo no [ESP8266 ligar ao IoT Hub do Azure](iot-hub-arduino-huzzah-esp8266-get-started.md).
1. Aumentar a temperatura ambiente em torno de SensorTag ser acima c de 30. Por exemplo, a luz uma vela em torno de seu SensorTag.
1. Deverá receber uma notificação de e-mail enviada pela aplicação lógica.

   > [!NOTE]
   > O fornecedor de serviços de e-mail poderá ter de verificar a identidade do remetente certificar-se de que é o utilizador que envia o e-mail.

## <a name="next-steps"></a>Passos Seguintes

Criou uma aplicação lógica que liga o seu IoT hub e caixa de correio para o monitoramento de temperatura e notificações com êxito.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
