---
title: IoT Hub do Azure - introdução ao ligar a dispositivos de IoT à nuvem | Microsoft Docs
description: Saiba como ligar o seu IoT quadros e kits de arranque ao IoT Hub do Azure. Os seus dispositivos podem enviar telemetria ao IoT Hub e do IoT Hub pode monitorizar e gerir os seus dispositivos.
author: dominicbetts
manager: timlt
keywords: tutorial de hub iot do Azure
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: 76f427204f0ad31196ce8b995b9e4ed9676209ca
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34634707"
---
# <a name="azure-iot-hub-get-started-with-real-devices"></a>Introdução ao IoT Hub do Azure dispositivos reais

Pode utilizar o IoT Hub do Azure e os SDKs do dispositivo IoT do Azure para criar soluções de Internet das coisas (IoT):

* IoT Hub do Azure é um serviço completamente gerido na nuvem que estabelece ligação com segurança, monitoriza e gere os dispositivos de IoT. Utilize os SDKs de dispositivos de IoT do Azure para implementar os seus dispositivos de IoT.
* Utilize um gateway de IoT cenários mais complexos de IoT. Por exemplo, onde tem de considerar os fatores, tais como dispositivos legados, os custos de largura de banda, políticas de segurança e privacidade ou processamento de dados de limite. Nestes cenários, utilize [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/) para implementar um gateway que liga dispositivos ao seu IoT hub.

## <a name="what-the-how-to-articles-cover"></a>O que abrangem os artigos de procedimentos

Estes artigos apresentam-o para o IoT Hub do Azure e os SDKs do dispositivo. Os artigos abrangem os cenários comuns do IoT para demonstrar as capacidades do IoT Hub. Os artigos também mostram como combinar o IoT Hub com outros serviços do Azure e ferramentas para criar soluções de IoT mais poderosas. Nos artigos, utilizar dispositivos de IoT reais.

## <a name="set-up-your-device"></a>Configurar o dispositivo

Ligar um dispositivo IoT ou gateway do Azure IoT Hub:

| Dispositivo IoT                       | Linguagem de programação |
|----------------------------------|----------------------|
| Raspberry Pi                     | [Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]  |
| IoT DevKit                       | [Arduino no VSCode][DevKit]     |
| Intel Edison                     | [NODE.js][Ed_Nd], [C][Ed_C]    |
| Adafruit Feather HUZZAH ESP8266  | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 coisa Dev       | [Arduino][Th_Ard]              |
| Adafruit Feather M0              | [Arduino][M0_Ard]              |
| Simulador de dispositivo online         | [Raspberry Pi (Node.js)][Ol_Sim] |

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
