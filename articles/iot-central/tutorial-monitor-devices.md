---
title: Monitorizar os seus dispositivos no Azure IoT Central | Microsoft Docs
description: Como operador, utilize a aplicação Azure IoT Central para monitorizar os seus dispositivos.
author: tbhagwat3
ms.author: tanmayb
ms.date: 10/12/2018
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: f7d8b3bf02fb92f24054172622e431f24bf344d9
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50157484"
---
# <a name="tutorial-use-azure-iot-central-to-monitor-your-devices"></a>Tutorial: Utilizar o Azure IoT Central para monitorizar os seus dispositivos

Este tutorial mostra-lhe, como operador, como utilizar a aplicação Microsoft Azure IoT Central para monitorizar os seus dispositivos e alterar as definições.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Receber uma notificação
> * Investigar um problema
> * Resolver um problema

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, o construtor deve concluir os três tutoriais do construtor para criar a aplicação Azure IoT Central:

* [Definir um novo tipo de dispositivo](tutorial-define-device-type.md)
* [Configurar regras e ações para o seu dispositivo](tutorial-configure-rules.md)
* [Personalizar as vistas do operador](tutorial-customize-operator.md)

## <a name="receive-a-notification"></a>Receber uma notificação

O Azure IoT Central envia notificações sobre os dispositivos como mensagens de e-mail. O construtor adicionou uma regra para enviar uma notificação se a temperatura num dispositivo de ar condicionado ligado exceder um limiar. Verifique os e-mails enviados para a conta que o construtor escolheu para receber notificações.

Abra a mensagem de e-mail que recebeu no final do tutorial [Configurar regras e ações para o seu dispositivo](tutorial-configure-rules.md). No e-mail, escolha **Clique aqui para abrir o seu dispositivo**:

![Regras do Application Builder](media/tutorial-monitor-devices/email.png)

A página **Dispositivo** do **Dispositivo de Ar Condicionado Ligado 1** simulou o dispositivo que criou nos tutorais anteriores é aberta no browser:

![Dispositivo que acionou a mensagem de e-mail de notificação](media/tutorial-monitor-devices/sourcedevice.png)

## <a name="investigate-an-issue"></a>Investigar um problema

Como operador, pode ver informações sobre o dispositivo nas páginas **Medições**, **Definições**, **Propriedades**, **Regras** e **Dashboard**. O construtor personalizou o **Dashboard** para apresentar informações importantes sobre um dispositivo de ar condicionado ligado.

Escolha a vista **Dashboard** para ver as informações sobre o dispositivo.

![Dashboard do dispositivo](media/tutorial-monitor-devices/initial_screen.png)

O gráfico no dashboard mostra um desenho da temperatura do dispositivo. Também pode ver a temperatura ideal atual do dispositivo no mosaico **Definir temperatura ideal**. Decide que a temperatura ideal é demasiado elevada.

## <a name="remediate-an-issue"></a>Resolver um problema

Para alterar a temperatura ideal do dispositivo, utilize a página **Definições**:

1. Escolha **Definições**. Altere **Definir Temperatura** para 75. Escolha **Atualizar** para enviar a nova temperatura ideal para o dispositivo. Quando o dispositivo reconhecer a alteração das definições, o estado do valor de definição muda para **sincronizado**:

    ![Atualizar definições](media/tutorial-monitor-devices/change_settings.png)

2. Escolha **Dashboard** e verifique o novo valor de definição:

    ![Dashboard do dispositivo atualizado](media/tutorial-monitor-devices/new_settings.png)

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="nextstepaction"]
> * Receber uma notificação
> * Investigar um problema
> * Resolver um problema

Agora que sabe monitorizar o dispositivo, o próximo passo sugerido consiste em [Adicionar um dispositivo](tutorial-add-device.md).