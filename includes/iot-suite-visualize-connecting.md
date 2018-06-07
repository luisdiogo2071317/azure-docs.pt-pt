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
ms.openlocfilehash: 5702c6e9c9d75c6cccb82f1c57684ef7b9898c34
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34666029"
---
## <a name="view-device-telemetry"></a>Ver a telemetria do dispositivo

Pode ver a telemetria enviada do seu dispositivo **dispositivos** página na solução.

1. Selecione o dispositivo aprovisionadas na lista de dispositivos no **dispositivos** página. Um painel mostra informações sobre o seu dispositivo, incluindo um desenho de telemetria do dispositivo:

    ![Ver detalhes do dispositivo](media/iot-suite-visualize-connecting/devicesdetail.png)

1. Escolha **pressão** para alterar a apresentação de telemetria:

    ![Telemetria de pressão de vista](media/iot-suite-visualize-connecting/devicespressure.png)

1. Para ver informações de diagnóstico sobre o seu dispositivo, desloque para baixo até **diagnóstico**:

    ![Diagnóstico de dispositivos de vista](media/iot-suite-visualize-connecting/devicesdiagnostics.png)

## <a name="act-on-your-device"></a>Atuar no seu dispositivo

Para invocar métodos nos seus dispositivos, utilize o **dispositivos** página na solução de monitorização remota. Por exemplo, na solução de monitorização remota **Chiller** dispositivos implementam um **FirmwareUpdate** método.

1. Escolha **dispositivos** para navegar para o **dispositivos** página na solução.

1. Selecione o dispositivo aprovisionadas na lista de dispositivos no **dispositivos** página:

    ![Selecione o dispositivo físico](media/iot-suite-visualize-connecting/devicesselect.png)

1. Para apresentar uma lista dos métodos pode chamar no seu dispositivo, escolha **tarefas**, em seguida, **executar o método**. Para agendar uma tarefa para ser executada em vários dispositivos, pode selecionar vários dispositivos na lista. O **tarefas** painel mostra os tipos de método comuns a todos os dispositivos selecionados.

1. Escolha **FirmwareUpdate**, defina o nome da tarefa **UpdatePhysicalChiller**. Definir **versão de Firmware** para **2.0.0**, defina **URI de Firmware** para **http://contoso.com/updates/firmware.bin**e, em seguida, escolha **aplicar**:

    ![Agendar a atualização de firmware](media/iot-suite-visualize-connecting/deviceschedule.png)

1. Apresenta uma sequência de mensagens em fila na consola do seu código de dispositivo a executar enquanto o dispositivo simulado processa o método.

1. Quando a atualização estiver concluída, a nova versão de firmware apresenta na **dispositivos** página:

    ![Atualização foi concluída](media/iot-suite-visualize-connecting/complete.png)

> [!NOTE]
> Para controlar o estado da tarefa na solução, escolha **vista**.

## <a name="next-steps"></a>Passos Seguintes

O artigo [personalizar o acelerador de solução de monitorização remota](../articles/iot-accelerators/iot-accelerators-remote-monitoring-customize.md) descreve algumas formas de personalizar o acelerador de solução.