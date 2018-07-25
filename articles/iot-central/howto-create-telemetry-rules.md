---
title: Criar e gerir regras de telemetria na sua aplicação do Azure IoT Central | Documentos da Microsoft
description: Regras de telemetria do Centro de IoT do Azure permitem-lhe monitorizar os seus dispositivos em tempo real e invocar ações, tal como enviar um e-mail quando a regra for acionada automaticamente.
services: iot-central
author: tanmaybhagwat
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 42516e4dd6a85e0d07d4a8e70e958b2ec6e84aad
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225205"
---
# <a name="create-a-telemetry-rule-and-set-up-an-action-in-your-azure-iot-central-application"></a>Criar uma regra de telemetria e configurar uma ação na sua aplicação do Azure IoT Central

Pode utilizar o Microsoft Azure IoT Central para monitorizar remotamente os seus dispositivos ligados. Regras de centro de IoT do Azure lhe permitem monitorizar os seus dispositivos em tempo real e automaticamente invocar ações, como enviar uma mensagem de e-mail ou acionar o fluxo de trabalho no Microsoft Flow quando as condições da regra forem atendidas. Em apenas alguns cliques, pode definir as condições para monitorizar os dados de dispositivo e configure a ação para invocar. Este artigo explica a regra de telemetria em detalhes.

Utiliza o Azure IoT Central [medidas de telemetria](howto-set-up-template.md) para capturar os dados de dispositivo. Cada tipo de medida tem principais atributos que definem a medição. Pode criar regras para monitorizar cada tipo de medida de dispositivo e gerar alertas quando a regra for acionada. Uma regra de telemetria é acionada quando a telemetria do dispositivo selecionado ultrapassar um limiar especificado.

## <a name="create-a-telemetry-rule"></a>Criar uma regra de telemetria

Esta secção mostra-lhe como criar uma regra de telemetria. Este exemplo utiliza um dispositivo conectado ar-condicionado que envia telemetria de temperatura e humidade. A regra monitoriza a temperatura comunicada pelo dispositivo e envia um e-mail quando ele for superior a 80 graus.

1. Navegue para a página de detalhes do dispositivo para o dispositivo que estiver a adicionar a regra.

1. Se ainda não criou quaisquer regras, verá o ecrã seguinte:

    ![Ainda não existem regras](media/howto-create-telemetry-rules/image1.png)

1. Sobre o **regras** separador, escolha **+ nova regra** para ver os tipos de regras, pode criar.

    ![Tipos de regra](media/howto-create-telemetry-rules/image2.png)

1. Escolha o **telemetria** mosaico para abrir o formulário para criar a regra.

    ![Regra de telemetria](media/howto-create-telemetry-rules/image3.png)

1. Escolha um nome que ajuda a identificar a regra neste modelo de dispositivo.

1. Para ativar imediatamente a regra para todos os dispositivos criadas através deste modelo, alternar **ativar regra**.

### <a name="configure-the-rule-condition"></a>Configurar a condição de regra

Esta secção mostra como adicionar uma condição para monitorizar a telemetria de temperatura.

1. Escolha o **+** junto a **condição**.

1. Escolha o **temperatura** tipo de telemetria a partir da lista pendente. Em seguida, escolha o operador e indique um valor de limiar. Pode adicionar várias condições de telemetria. Quando são especificadas várias condições, todas as condições têm de ser cumpridas para a regra acionar.

    ![Adicionar condição de telemetria](media/howto-create-telemetry-rules/image4.png)

    > [!NOTE]
    > Selecione pelo menos uma medida de telemetria ao definir uma condição de regra de telemetria.

1. Clique em **guardar** para guardar a regra. A regra for executada dentro de alguns minutos e inicia a monitorização de envio de telemetria para seu aplicativo.

### <a name="add-an-action"></a>Adicionar uma ação

Este exemplo mostra como adicionar uma ação para uma regra. Isso mostra como adicionar a ação de e-mail, mas também pode adicionar outras ações:
-  [Ação do Microsoft Flow](howto-add-microsoft-flow.md) para disparar um fluxo de trabalho no Microsoft Flow, quando é acionada uma regra
- [Ação do Webhook](howto-create-webhooks.md) para notificar outros serviços, quando é acionada uma regra

> [!NOTE]
> Apenas 1 ação pode ser associada a uma única regra neste momento.

1. Escolha o **+** junto a **ações**. Aqui ver a lista de ações disponíveis.

    ![Adicionar ação](media/howto-create-telemetry-rules/image5.png)

1. Escolha o **E-Mail** ação, introduza um endereço de e-mail válido no **para** campo e fornecer uma nota a aparecer no corpo da mensagem de e-mail quando a regra for acionada.

    > [!NOTE]
    > Os e-mails são enviados apenas para os utilizadores que foram adicionados à aplicação e tem iniciado sessão pelo menos uma vez. Saiba mais sobre [gestão de utilizadores](howto-administer.md) no Azure IoT Central.

   ![Configurar ação](media/howto-create-telemetry-rules/image6.png)

1. Clique em **Guardar**. A regra for executada dentro de alguns minutos e inicia a monitorização de envio de telemetria para seu aplicativo. Quando a condição especificada na regra corresponde, a regra for acionada a ação de e-mail configurado.

## <a name="parameterize-the-rule"></a>Parametrizar a regra

Regras podem derivar determinados valores partir **propriedades do dispositivo** como parâmetros. O uso de parâmetros é útil em cenários em que os limiares de telemetria variam para diferentes dispositivos. Ao criar a regra, escolha uma propriedade de dispositivo que especifica o limiar, tal como **Ideal de limiar máximo**, em vez de fornecer um valor absoluto, como 80 graus. Quando a regra for executada, corresponde a telemetria do dispositivo com o valor fornecido na propriedade do dispositivo.

O uso de parâmetros é uma forma eficaz para reduzir o número de regras para gerir por modelo de dispositivo.

Ações também podem ser configuradas usando **propriedade do dispositivo** como um parâmetro. Se um endereço de e-mail é armazenado como uma propriedade do dispositivo, em seguida, ele pode ser usado quando define a **para** endereço.

## <a name="delete-a-rule"></a>Eliminar uma regra

Se já não precisar de uma regra, elimine-o ao abrir a regra e escolher **eliminar**. A eliminar a regra remove do modelo de dispositivo e todos os dispositivos associados.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>Ativar ou desativar uma regra para um modelo de dispositivo

Navegue para o dispositivo e selecione a regra que pretende ativar ou desativar. Alternar o **ativar regra para todos os dispositivos deste modelo** botão na regra ativa ou desativa a regra para todos os dispositivos associados ao modelo de dispositivo.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Ativar ou desativar uma regra para um dispositivo

Navegue para o dispositivo e selecione a regra que pretende ativar ou desativar. Ativar/desativar a **ativar regra para este dispositivo** botão para ativar ou desativar a regra para esse dispositivo.

## <a name="next-steps"></a>Passos Seguintes

Agora que sabe como editar as regras na sua aplicação do Azure IoT Central, Eis os passos seguintes sugeridos:

> [!div class="nextstepaction"]
> [Como adicionar uma ação do Microsoft Flow para uma regra](howto-add-microsoft-flow.md)
> [como gerir os seus dispositivos](howto-manage-devices.md)
