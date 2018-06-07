---
title: Criar e gerir regras de telemetria na sua aplicação do Azure IoT Central | Microsoft Docs
description: As regras de telemetria de centro de IoT do Azure permitem-lhe para monitorizar os seus dispositivos em tempo real e invocar automaticamente as ações, como enviar uma mensagem de e-mail, quando a regra é acionado.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: caca4e9db898b3766995fde8c5eebd4767abd85b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629818"
---
# <a name="create-a-telemetry-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Criar uma regra de telemetria e configurar as notificações na sua aplicação do Azure IoT Central

Pode utilizar o Microsoft Azure IoT Central para monitorizar remotamente os dispositivos ligados. As regras de centro de IoT do Azure permitem-lhe para monitorizar os seus dispositivos em tempo real e invocar automaticamente as ações, como enviar uma mensagem de e-mail, quando a regra é acionado. Em apenas alguns cliques, pode definir a condição para monitorizar os dados de dispositivo e configure a ação a invocar. Este artigo explica as regras de telemetria em detalhe.

Centro de IoT do Azure utiliza [medidas de telemetria](howto-set-up-template.md) para capturar os dados de dispositivo. Cada tipo de medida tem atributos de chave que definem a medição. Pode criar regras para monitorizar cada tipo de medição do dispositivo e gerar alertas quando a regra é acionado. Uma regra de telemetria é acionado quando a telemetria do dispositivo selecionado atravesse um limiar especificado.

## <a name="create-a-telemetry-rule"></a>Criar uma regra de telemetria

Esta secção mostra como criar uma regra de telemetria. Este exemplo utiliza um dispositivo ligado ar condicionado para edifícios que envia a telemetria de temperatura e humidade. A regra monitoriza a temperatura comunicada pelo dispositivo e envia um e-mail quando passa acima 80 graus.

1. Navegue para a página de detalhes do dispositivo para o dispositivo que estiver a adicionar a regra.

1. Se ainda não criou quaisquer regras, verá o ecrã seguinte:

    ![Ainda não existem regras](media\howto-create-telemetry-rules\image1.png)

1. No **regras** separador, escolha **+ nova regra** para ver os tipos de regras, pode criar.

    ![Tipos de regras](media\howto-create-telemetry-rules\image2.png)

1. Escolha o **telemetria** mosaico para abrir o formulário para criar a regra.

    ![Regra de telemetria](media\howto-create-telemetry-rules\image3.png)

1. Escolha um nome que o ajuda a identificar a regra neste modelo de dispositivo.

1. Para ativar imediatamente a regra para todos os dispositivos criadas através deste modelo, ative **ativar regra**.

### <a name="configure-the-rule-condition"></a>Configurar a condição de regra

Esta secção mostra como adicionar uma condição para monitorizar a telemetria de temperatura.

1. Escolha o **+** junto a **condição**.

1. Escolha o **temperatura** telemetria tipo na lista pendente. Em seguida, escolha o operador e indique um valor de limiar. Pode adicionar várias condições de telemetria. Quando são especificadas várias condições, todas as condições têm de ser cumpridas para a regra a acionar.

    ![Adicionar condição de telemetria](media\howto-create-telemetry-rules\image4.png)

    > [!NOTE]
    > Selecione pelo menos uma medida de telemetria ao definir uma condição de regra de telemetria.

### <a name="configure-the-action"></a>Configure a ação

Esta secção mostra como especificar o que faz a regra quando a condição corresponde ao adicionar uma ação.

1. Escolha o **+** junto a **ações**. Aqui pode ver a lista de ações disponíveis. Durante a pré-visualização pública, **E-Mail** é a única ação suportada.

    ![Adicionar ação](media\howto-create-telemetry-rules\image5.png)

1. Escolha o **E-Mail** ação, introduza um endereço de e-mail válido no **para** campo e fornecer uma nota a aparecer no corpo da mensagem de e-mail quando a regra é acionado.

    > [!NOTE]
    > Os e-mails são enviados apenas para os utilizadores que foram adicionados à aplicação e de ter sessão iniciada, pelo menos, uma vez. Saiba mais sobre [gestão de utilizadores](howto-administer.md) no Azure IoT Central.

   ![Configurar ação](media\howto-create-telemetry-rules\image6.png)

1. Para guardar a regra, escolha **guardar**. A regra entra em direto dentro de alguns minutos e inicia a monitorização de telemetria a ser enviada para a aplicação. Quando a condição especificada na regra corresponde, a regra aciona a ação de correio eletrónico configurado.

## <a name="parameterize-the-rule"></a>Parametrizar a regra

As regras podem derivar determinados valores de **propriedades do dispositivo** como parâmetros. A utilização de parâmetros é útil em cenários onde os limiares de telemetria variam para diferentes dispositivos. Quando criar a regra, escolha uma propriedade de dispositivo que especifica o limiar, tais como **Ideal de limiar máximo**, em vez de fornecer um valor absoluto, tais como 80 graus. Quando executa a regra, corresponde a telemetria do dispositivo com o valor fornecido a propriedade do dispositivo.

A utilização de parâmetros é uma forma eficaz para reduzir o número de regras para gerir por modelo de dispositivo.

Também podem ser configuradas ações utilizando **propriedade do dispositivo** como parâmetro. Se um endereço de e-mail é armazenado como uma propriedade do dispositivo, em seguida, podem ser utilizado ao definir o **para** endereço.

## <a name="delete-a-rule"></a>Eliminar uma regra

Se já não necessita de uma regra, elimine-o ao abrir a regra e escolher **eliminar**. A eliminar a regra remove-o modelo de dispositivo e todos os dispositivos associados.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>Ativar ou desativar uma regra para um modelo de dispositivo

Navegue para o dispositivo e escolher a regra que pretende ativar ou desativar. Ativando ou desativando a **ativar regra para todos os dispositivos deste modelo** botão na regra ativa ou desativa a regra para todos os dispositivos associados com o modelo de dispositivo.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Ativar ou desativar uma regra para um dispositivo

Navegue para o dispositivo e escolher a regra que pretende ativar ou desativar. Ativar/desativar a **ativar regra para este dispositivo** botão para ativar ou desativar a regra para que o dispositivo.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como editar as regras na sua aplicação do Azure IoT Central, eis o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Como gerir os seus dispositivos](howto-manage-devices.md).