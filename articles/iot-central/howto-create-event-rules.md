---
title: Criar e gerir regras de evento na sua aplicação do Azure IoT Central | Documentos da Microsoft
description: As regras de eventos do Azure IoT Central permitem-lhe monitorizar os seus dispositivos em tempo real e invocar ações, tal como enviar um e-mail quando a regra for acionada automaticamente.
services: iot-central
author: ankitgupta
ms.author: ankitgup
ms.date: 04/29/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: ede7748b1471136cf792c2b30b7c90e12b0b274a
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39006853"
---
# <a name="create-an-event-rule-and-set-up-an-action-in-your-azure-iot-central-application"></a>Criar uma regra de evento e configurar uma ação na sua aplicação do Azure IoT Central

Pode utilizar o Microsoft Azure IoT Central para monitorizar remotamente os seus dispositivos ligados. Regras de centro de IoT do Azure lhe permitem monitorizar os seus dispositivos em tempo real e automaticamente invocar ações, como enviar uma mensagem de e-mail ou acionar o fluxo de trabalho no Microsoft Flow quando as condições da regra forem atendidas. Em apenas alguns cliques, pode definir a condição monitorize os seus dados de dispositivo e configure a ação para invocar. Este artigo explica o evento monitorização regra em detalhes.

Utiliza o Azure IoT Central [medição do evento](howto-set-up-template.md) para capturar os dados de dispositivo. Cada tipo de medida tem principais atributos que definem a medição. Pode criar regras para monitorizar cada tipo de medida de dispositivo e gerar alertas quando a regra for acionada. Uma regra de evento é acionada quando o evento de dispositivo selecionado é comunicado pelo dispositivo.

## <a name="create-an-event-rule"></a>Criar uma regra de eventos

Esta secção mostra-lhe como criar uma regra de evento. Este exemplo utiliza um dispositivo de máquina de venda automática refrigerated esse evento de erro do motor de ventoinha de relatórios. A regra monitoriza o evento comunicado pelo dispositivo e envia um e-mail sempre que o evento é comunicado.

1. Navegue para a página de detalhes do dispositivo para o dispositivo que estiver a adicionar a regra.

1. Se ainda não criou quaisquer regras, verá o ecrã seguinte:

    ![Ainda não existem regras](media/howto-create-event-rules/image1.png)

1. Sobre o **regras** separador, escolha **+ nova regra** para ver os tipos de regras, pode criar.

    ![Tipos de regra](media/howto-create-event-rules/image2.png)

1. Clique em **evento** para abrir o formulário para criar a regra.

    ![Regra de evento](media/howto-create-event-rules/image3.png)

1. Escolha um nome que ajuda a identificar a regra neste modelo de dispositivo.

1. Para ativar imediatamente a regra para todos os dispositivos criadas através deste modelo, alternar **ativar regra**.

### <a name="configure-the-rule-condition"></a>Configurar a condição de regra

Esta secção mostra como adicionar uma condição para monitorizar a medição de eventos de erro motor ventoinha.

1. Escolha o **+** junto a **condição**.

1. Escolha a medição de evento na lista pendente que pretende monitorizar. Neste exemplo **erro de Motor de ventoinha** evento tiver sido selecionado.

1. Opcionalmente, também pode fornecer um valor no caso de que pretende monitorizar um valor específico do evento a ser comunicado pelo dispositivo. Por exemplo, se o dispositivo comunica o mesmo evento com códigos de erro diferentes, em seguida, fornecer o código de erro como um valor na condição da regra irá garantir que a regra é acionada apenas quando o dispositivo envia esse valor específico, como o payload do evento. Deixar em branco significa que a regra irá acionar sempre que o dispositivo envia o evento independentemente do valor de evento.

    ![Adicionar condição de eventos](media/howto-create-event-rules/image4.png)

    > [!NOTE]
    > Tem de selecionar a medida, pelo menos, um evento quando definir uma condição de regra de evento.

1. Clique em **guardar** para guardar a regra. A regra for executada dentro de alguns minutos e inicia a monitorização de eventos a ser enviados ao seu aplicativo.

### <a name="add-an-action"></a>Adicionar uma ação

Esta secção mostra como adicionar uma ação para uma regra. Isso mostra como adicionar a ação de e-mail, mas também pode [adicionar uma ação do Microsoft Flow](howto-add-microsoft-flow.md) à sua regra de iniciar um fluxo de trabalho no Microsoft Flow, quando a regra é acionada.

> [!NOTE]
> Apenas 1 ação pode ser associada a uma única regra neste momento.

1. Escolha o **+** junto a **ações**. Aqui ver a lista de ações disponíveis.

    ![Adicionar ação](media/howto-create-event-rules/image5.png)

1. Escolha o **E-Mail** ação, introduza um endereço de e-mail válido no **para** campo e fornecer uma nota a aparecer no corpo da mensagem de e-mail quando a regra for acionada.

    > [!NOTE]
    > Os e-mails são enviados apenas para os utilizadores que foram adicionados à aplicação e tem iniciado sessão pelo menos uma vez. Saiba mais sobre [gestão de utilizadores](howto-administer.md) no Azure IoT Central.

   ![Configurar ação](media/howto-create-event-rules/image6.png)

1. Clique em **Guardar**. A regra for executada dentro de alguns minutos e inicia a monitorização de eventos a ser enviados ao seu aplicativo. Quando a condição especificada na regra corresponde, a regra for acionada a ação de e-mail configurado.

## <a name="parameterize-the-rule"></a>Parametrizar a regra

Ações também podem ser configuradas usando **propriedade do dispositivo** como um parâmetro. Se um endereço de e-mail é armazenado como uma propriedade do dispositivo, em seguida, ele pode ser usado quando define a **para** endereço.

## <a name="delete-a-rule"></a>Eliminar uma regra

Se já não precisar de uma regra, elimine-o ao abrir a regra e escolher **eliminar**. A eliminar a regra remove do modelo de dispositivo e todos os dispositivos associados.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>Ativar ou desativar uma regra para um modelo de dispositivo

Navegue para o dispositivo e selecione a regra que pretende ativar ou desativar. Alternar o **ativar regra para todos os dispositivos deste modelo** botão na regra ativa ou desativa a regra para todos os dispositivos associados ao modelo de dispositivo.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Ativar ou desativar uma regra para um dispositivo

Navegue para o dispositivo e selecione a regra que pretende ativar ou desativar. Ativar/desativar a **ativar regra para este dispositivo** botão para ativar ou desativar a regra para esse dispositivo.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como criar regras na sua aplicação do Azure IoT Central, este é o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Como adicionar uma ação do Microsoft Flow para uma regra](howto-add-microsoft-flow.md)
> [como gerir os seus dispositivos](howto-manage-devices.md).
