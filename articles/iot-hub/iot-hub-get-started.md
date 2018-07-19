---
title: O IoT Hub do Azure - introdução ao ligar dispositivos IoT à cloud | Documentos da Microsoft
description: Saiba como ligar os quadros de IoT e kits do iniciante ao IoT Hub do Azure. Os dispositivos podem enviar telemetria para o IoT Hub e IoT Hub pode monitorizar e gerir os seus dispositivos.
author: dominicbetts
manager: timlt
keywords: tutorial de hub iot do Azure
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: 77abe7e2187a3cb28b326ffa833a856625d6c33d
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125197"
---
# <a name="azure-iot-hub-get-started-with-real-devices"></a>Introdução ao IoT Hub do Azure dispositivos reais

Estes artigos de instruções apresentá-lo para o IoT Hub do Azure e os SDKs do dispositivo a executar em dispositivos reais.

## <a name="set-up-your-device"></a>Configurar o dispositivo

Ligue-se de um dispositivo IoT ou o gateway ao IoT Hub do Azure:

| Dispositivo IoT                       | Linguagem de programação |
|----------------------------------|----------------------|
| Raspberry Pi                     | [Python][Pi_Py], [node. js][Pi_Nd], [C][Pi_C]  |
| IoT DevKit                       | [Arduino no VSCode][DevKit]     |
| Intel Edison                     | [NODE. js][Ed_Nd], [C][Ed_C]    |
| Adafruit Feather HUZZAH ESP8266  | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 Thing Dev       | [Arduino][Th_Ard]              |
| Adafruit Feather M0              | [Arduino][M0_Ard]              |
| Simulador de dispositivos online         | [Raspberry Pi (node. js)][Ol_Sim] |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]

[Pi_Nd]: iot-hub-raspberry-pi-kit-node-get-started.md
[Pi_C]: iot-hub-raspberry-pi-kit-c-get-started.md
[Pi_Py]: iot-hub-raspberry-pi-kit-python-get-started.md
[DevKit]: iot-hub-arduino-iot-devkit-az3166-get-started.md
[Ed_Nd]: iot-hub-intel-edison-kit-node-get-started.md
[Ed_C]: iot-hub-intel-edison-kit-c-get-started.md
[Hu_Ard]: iot-hub-arduino-huzzah-esp8266-get-started.md
[Th_Ard]: iot-hub-sparkfun-esp8266-thing-dev-get-started.md
[M0_Ard]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[Ol_Sim]: iot-hub-raspberry-pi-web-simulator-get-started.md
