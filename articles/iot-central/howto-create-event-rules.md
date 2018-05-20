---
title: Criar e gerir regras de eventos na sua aplicação do Azure IoT Central | Microsoft Docs
description: As regras de eventos de centro de IoT do Azure permitem-lhe para monitorizar os seus dispositivos em tempo real e invocar automaticamente as ações, como enviar uma mensagem de e-mail, quando a regra é acionado.
services: iot-central
author: ankitgupta
ms.author: ankitgup
ms.date: 04/29/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 932b1906b767ee7676f46ffd7242ad3d478d41c2
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="create-an-event-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Criar uma regra de evento e configurar as notificações na sua aplicação do Azure IoT Central

Pode utilizar o Microsoft Azure IoT Central para monitorizar remotamente os dispositivos ligados. As regras de centro de IoT do Azure permitem-lhe para monitorizar os seus dispositivos em tempo real e invocar automaticamente as ações, como enviar uma mensagem de e-mail, quando a regra é acionado. Em apenas alguns cliques, pode definir a condição para monitorizar os dados de dispositivo e configure a ação a invocar. Este artigo explica regra em detalhe de monitorização de eventos.

Centro de IoT do Azure utiliza [medida de evento](howto-set-up-template.md) para capturar os dados de dispositivo. Cada tipo de medida tem atributos de chave que definem a medição. Pode criar regras para monitorizar cada tipo de medição do dispositivo e gerar alertas quando a regra é acionado. Uma regra de evento é acionado quando o evento de dispositivo selecionado é comunicado pelo dispositivo.

## <a name="create-an-event-rule"></a>Criar uma regra de evento

Esta secção mostra como criar uma regra de evento. Este exemplo utiliza um dispositivo de máquina de distribuidores automáticos refrigerated esse evento de erro do motor de ventoinha de relatórios. A regra monitoriza o evento comunicado pelo dispositivo e envia uma mensagem de e-mail sempre que o evento é comunicado.

1. Navegue para a página de detalhes do dispositivo para o dispositivo que estiver a adicionar a regra.

1. Se ainda não criou quaisquer regras, verá o ecrã seguinte:

    ![Ainda não existem regras](media\howto-create-event-rules\image1.png)

1. No **regras** separador, escolha **+ nova regra** para ver os tipos de regras, pode criar.

    ![Tipos de regras](media\howto-create-event-rules\image2.png)

1. Clique em **eventos** para abrir o formulário para criar a regra.

    ![Regra de evento](media\howto-create-event-rules\image3.png)

1. Escolha um nome que o ajuda a identificar a regra neste modelo de dispositivo.

1. Para ativar imediatamente a regra para todos os dispositivos criadas através deste modelo, ative **ativar regra**.

### <a name="configure-the-rule-condition"></a>Configurar a condição de regra

Esta secção mostra como adicionar uma condição para monitorizar a medição de eventos de erro motor ventoinha.

1. Escolha o **+** junto a **condição**.

1. Escolha a medição de evento na lista pendente que pretende monitorizar. Neste exemplo **erro Motor ventoinha** evento foi selecionado.

1. Opcionalmente, também pode fornecer um valor no caso de que pretende monitorizar um valor específico do evento a ser reportado pelo dispositivo. Por exemplo, se o dispositivo comunica o mesmo evento com códigos de erro diferente, em seguida, fornecendo o código de erro como um valor na condição da regra irá garantir que os acionadores de regra apenas quando o dispositivo envia esse valor específico, como o payload do evento. Abandonar o fileparser em branco significa que a regra é acionado sempre que o dispositivo envia o evento independentemente do valor de eventos.

    ![Adicionar a situação do evento](media\howto-create-event-rules\image4.png)

    > [!NOTE]
    > Tem de selecionar a medida, pelo menos, um evento quando definir uma condição de regra de evento.

### <a name="configure-the-action"></a>Configure a ação

Esta secção mostra como especificar o que faz a regra quando a condição corresponde ao adicionar uma ação.

1. Escolha o **+** junto a **ações**. Aqui pode ver a lista de ações disponíveis. Durante a pré-visualização pública, **E-Mail** é a única ação suportada.

    ![Adicionar ação](media\howto-create-event-rules\image5.png)

1. Escolha o **E-Mail** ação, introduza um endereço de e-mail válido no **para** campo e fornecer uma nota a aparecer no corpo da mensagem de e-mail quando a regra é acionado.

    > [!NOTE]
    > Os e-mails são enviados apenas para os utilizadores que foram adicionados à aplicação e de ter sessão iniciada, pelo menos, uma vez. Saiba mais sobre [gestão de utilizadores](howto-administer.md) no Azure IoT Central.

   ![Configurar ação](media\howto-create-event-rules\image6.png)

1. Para guardar a regra, escolha **guardar**. A regra entra em direto dentro de alguns minutos e inicia a monitorização de eventos enviados para a aplicação. Quando a condição especificada na regra corresponde, a regra aciona a ação de correio eletrónico configurado.

## <a name="parameterize-the-rule"></a>Parametrizar a regra

Também podem ser configuradas ações utilizando **propriedade do dispositivo** como parâmetro. Se um endereço de e-mail é armazenado como uma propriedade do dispositivo, em seguida, podem ser utilizado ao definir o **para** endereço.

## <a name="delete-a-rule"></a>Eliminar uma regra

Se já não necessita de uma regra, elimine-o ao abrir a regra e escolher **eliminar**. A eliminar a regra remove-o modelo de dispositivo e todos os dispositivos associados.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>Ativar ou desativar uma regra para um modelo de dispositivo

Navegue para o dispositivo e escolher a regra que pretende ativar ou desativar. Ativando ou desativando a **ativar regra para todos os dispositivos deste modelo** botão na regra ativa ou desativa a regra para todos os dispositivos associados com o modelo de dispositivo.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Ativar ou desativar uma regra para um dispositivo

Navegue para o dispositivo e escolher a regra que pretende ativar ou desativar. Ativar/desativar a **ativar regra para este dispositivo** botão para ativar ou desativar a regra para que o dispositivo.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como criar regras na sua aplicação do Azure IoT Central, eis o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Como gerir os seus dispositivos](howto-manage-devices.md).