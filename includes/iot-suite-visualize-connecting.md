---
title: incluir ficheiro
description: incluir ficheiro
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 04/24/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 73ba80878615f04e1755a4d12014691c5ae2a077
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54453125"
---
## <a name="view-device-telemetry"></a>Ver a telemetria do dispositivo

Pode ver a telemetria enviada a partir do seu dispositivo **dispositivos** página na solução.

1. Selecione o dispositivo aprovisionado na lista de dispositivos no **dispositivos** página. Um painel apresenta informações sobre o seu dispositivo, incluindo um desenho de telemetria do dispositivo:

    ![Ver detalhes do dispositivo](media/iot-suite-visualize-connecting/devicesdetail.png)

1. Escolher **pressão** para alterar a apresentação de telemetria:

    ![Ver a telemetria de pressão](media/iot-suite-visualize-connecting/devicespressure.png)

1. Para ver informações de diagnóstico sobre o seu dispositivo, desloque para baixo até **diagnóstico**:

    ![Diagnóstico de dispositivos do Vista](media/iot-suite-visualize-connecting/devicesdiagnostics.png)

## <a name="act-on-your-device"></a>Agir em seu dispositivo

Para invocar métodos nos seus dispositivos, utilize o **dispositivos** página na solução de monitorização remota. Por exemplo, na solução de monitorização remota **Chiller** dispositivos implementam um **FirmwareUpdate** método.

1. Escolher **dispositivos** para navegar para o **dispositivos** página na solução.

1. Selecione o dispositivo aprovisionado na lista de dispositivos no **dispositivos** página:

    ![Selecione o seu dispositivo real](media/iot-suite-visualize-connecting/devicesselect.png)

1. Para apresentar uma lista dos métodos que pode chamar no seu dispositivo, escolha **trabalhos**, em seguida, **executar o método**. Para agendar uma tarefa para ser executada em vários dispositivos, pode selecionar vários dispositivos na lista. O **tarefas** painel mostra os tipos de método comuns a todos os dispositivos que selecionou.

1. Escolher **FirmwareUpdate**, defina o nome da tarefa como **UpdatePhysicalChiller**. Definir **versão de Firmware** ao **2.0.0**, defina **URI de Firmware** para **http://contoso.com/updates/firmware.bin**e, em seguida, escolha **aplicar**:

    ![Agendar a atualização de firmware](media/iot-suite-visualize-connecting/deviceschedule.png)

1. Apresenta uma sequência de mensagens na consola de execução do seu código de dispositivo enquanto o dispositivo simulado lida com o método.

1. Quando a atualização estiver concluída, a nova versão de firmware se apresenta na **dispositivos** página:

    ![Atualização concluída](media/iot-suite-visualize-connecting/complete.png)

> [!NOTE]
> Para controlar o estado da tarefa na solução, escolha **vista**.

## <a name="next-steps"></a>Passos Seguintes

O artigo [personalizar o acelerador de solução de monitorização remota](../articles/iot-accelerators/iot-accelerators-remote-monitoring-customize.md) descreve algumas formas de personalizar o solution accelerator.