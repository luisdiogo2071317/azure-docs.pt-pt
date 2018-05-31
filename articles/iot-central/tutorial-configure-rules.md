---
title: Configurar regras e ações no Azure IoT Central | Microsoft Docs
description: Este tutorial mostra-lhe, como construtor, como configurar regras baseadas em telemetria e ações na sua aplicação Azure IoT Central.
services: iot-central
author: ankitgupta
ms.author: ankitgup
ms.date: 04/16/2018
ms.topic: tutorial
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 5ac19e0e25ea3e25ede4d87776c01f8bcaea4655
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34202242"
---
# <a name="2---configure-rules-and-actions-for-your-device-in-azure-iot-central"></a>2 - Configurar regras e ações para o seu dispositivo no Azure IoT Central

Este tutorial mostra-lhe, como construtor, como configurar regras baseadas em telemetria e ações na sua aplicação Microsoft Azure IoT Central.

Neste tutorial, pode criar uma regra que envia um e-mail quando a temperatura do dispositivo de ar condicionado ligado excede 90&deg; F.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma regra baseada em telemetria
> * Adicionar uma ação

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, deve concluir o tutorial [Definir um novo tipo de dispositivo na sua aplicação](tutorial-define-device-type.md) para criar o modelo de dispositivo **Dispositivo de Ar Condicionado Ligado** com que irá trabalhar.

## <a name="create-a-telemetry-based-rule"></a>Criar uma regra baseada em telemetria

1. Para adicionar uma nova regra baseada em telemetria à sua aplicação, no menu de navegação esquerdo, escolha **Device Explorer**:

    ![Página do Device Explorer](media/tutorial-configure-rules/explorerpage.png)

    Aparece o modelo de dispositivo **Dispositivo de Ar Condicionado Ligado (1.0.0)** e o dispositivo **Dispositivo de Ar Condicionado Ligado 1** que criou o tutorial anterior.

1. Para começar a personalizar o seu dispositivo de ar condicionado ligado, selecione o dispositivo que criou no tutorial anterior:

    ![Página do dispositivo de ar condicionado ligado](media/tutorial-configure-rules/builderdevicelist.png)

1. Para começar a adicionar uma regra na vista **Regras**, escolha **Regras**:

    ![Vista de regras](media/tutorial-configure-rules/builderrulesview.png)

1. Neste tutorial, adiciona uma regra de telemetria baseada em limiares. Para começar a criar uma regra baseada em limiares, escolha **Nova Regra** e, em seguida, **Telemetria**.

1. Para definir a sua regra, utilize as informações na tabela seguinte:

    | Definição     | Valor                          |
    | ----------- | ------------------------------ |
    | Nome        | Temperatura do ar condicionado    |
    | Ativar regra | Ativado                             |
    | Condição   | A temperatura é superior a 90 |

    ![Condição da regra de temperatura](media/tutorial-configure-rules/buildertemperaturerule.png)

## <a name="add-an-action"></a>Adicionar uma ação

Quando definir uma regra, também pode definir uma ação a executar quando são satisfeitas as condições de regra. Neste tutorial, adiciona uma ação para enviar um e-mail como notificação que acionou a regra.

1. Para adicionar uma **Ação**, percorra para baixo no painel **Configurar Regra de Telemetria** e escolha **+** junto a **Ações** e, em seguida, escolha **E-mail**:

    ![Ação de regra de temperatura](media/tutorial-configure-rules/builderaddaction.png)

1. Para definir a sua ação, utilize as informações na tabela seguinte:

    | Definição   | Valor                          |
    | --------- | ------------------------------ |
    | Para        | O seu endereço de e-mail             |
    | Notas     | A temperatura no dispositivo de ar condicionado excedeu o limiar. |

    > [!NOTE]
    > Para receber uma notificação por e-mail, o endereço de e-mail deve ser um [ID de utilizador na aplicação](howto-administer.md), e esse utilizador deve ter sessão iniciada na aplicação, pelo menos, uma vez.

    ![Ação de temperatura do Application Builder](media/tutorial-configure-rules/buildertemperatureaction.png)

1. Escolha **Guardar**. A regra está listada na página **Regras**:

    ![Regras do Application Builder](media/tutorial-configure-rules/builderrules.png)

## <a name="test-the-rule"></a>Testar a regra

Pouco tempo depois de guardar a regra, esta é ativada. Quando forem cumpridas as condições definidas na regra, a aplicação envia uma mensagem para o endereço de e-mail que especificou na ação.

![Ação de e-mail](media/tutorial-configure-rules/email.png)

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

<!-- Repeat task list from intro -->
> [!div class="nextstepaction"]
> * Criar uma regra baseada em telemetria
> * Adicionar uma ação

Agora que definiu uma regra baseadas em limiares, o próximo passo sugerido consiste em [Personalizar as vistas do operador](tutorial-customize-operator.md).

Para saber mais sobre os diferentes tipos de regras no Azure IoT Central e como parametrizar a definição da regra, consulte:
* [Criar uma regra de telemetria e configurar as notificações](howto-create-telemetry-rules.md).
* [Criar uma regra de eventos e configurar as notificações](howto-create-event-rules.md).

<!-- Next tutorials in the sequence -->