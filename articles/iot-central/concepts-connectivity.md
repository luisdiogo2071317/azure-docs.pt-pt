---
title: Conectividade do dispositivo no Azure IoT Central | Microsoft Docs
description: Este artigo apresenta os conceitos chave relacionadas com a conectividade do dispositivo no Azure IoT Central
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: conceptual
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 00b621a4635ef1ceda26772ac5876fa2599b56f8
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="device-connectivity-in-azure-iot-central"></a>Conectividade do dispositivo no Azure IoT Central

Este artigo apresenta os conceitos chave relacionadas com a conectividade do dispositivo no Microsoft Azure IoT Central.

Cada dispositivo que liga à sua aplicação do Azure IoT Central estabelece ligação a [pontos finais](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-endpoints) que sejam expostos pelo IoT hub do Azure IoT Central utiliza. IoT Hub permite ligações escaláveis, seguras e fiáveis dos seus dispositivos ligados.

## <a name="sdk-support"></a>Suporte SDK

A oferta Azure SDKs do dispositivo a forma mais fácil para si implementar o código nos seus dispositivos que se liga à sua aplicação do Azure IoT Central. Os SDKs do dispositivo seguintes estão disponíveis:

- [SDK do IoT do Azure para C](https://github.com/azure/azure-iot-sdk-c)
- [SDK do IoT do Azure para Python](https://github.com/azure/azure-iot-sdk-python)
- [SDK do IoT do Azure para Node.js](https://github.com/azure/azure-iot-sdk-node)
- [SDK do IoT do Azure para Java](https://github.com/azure/azure-iot-sdk-java)
- [SDK do IoT do Azure para .NET](https://github.com/azure/azure-iot-sdk-csharp)

Cada dispositivo estabelece ligação utilizar uma cadeia de ligação exclusiva que identifica o dispositivo. Apenas pode ligar um dispositivo ao IoT hub em que está registado. Quando cria um dispositivo real na sua aplicação do Azure IoT Central, a aplicação gera uma cadeia de ligação para utilização.

## <a name="sdk-features-and-iot-hub-connectivity"></a>As funcionalidades do SDK e a conectividade do IoT Hub

Todas as comunicações de dispositivos com o IoT Hub utiliza as seguintes opções de conectividade do IoT Hub:

- [Mensagens do dispositivo-nuvem](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)
- [Dispositivos duplos](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins)

A tabela seguinte resume como funcionalidades de dispositivos do Azure IoT Central mapeiam para funcionalidades do IoT Hub:

| Azure IoT Central | Azure IoT Hub |
| ----------- | ------- |
| Medição: telemetria | Mensagens do dispositivo-nuvem |
| Propriedades do dispositivo | Dispositivo duplo comunicadas propriedades |
| Definições | Dispositivo duplo pretendida e reportado propriedades |

Para obter mais informações sobre como utilizar os SDKs do dispositivo, consulte um dos seguintes artigos, por exemplo de código:

- [Ligar um cliente de Node.js genérico a sua aplicação do Azure IoT Central](howto-connect-nodejs.md)
- [Ligar um dispositivo de Raspberry Pi à sua aplicação do Azure IoT Central](howto-connect-raspberry-pi-python.md)
- [Ligar um dispositivo de kit de DevDiv à sua aplicação do Azure IoT Central](howto-connect-devkit.md).

O vídeo seguinte apresenta uma descrição geral de como ligar um dispositivo real para o Azure IoT Central:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Connect-real-devices-to-Microsoft-IoT-Central/Player]

## <a name="protocols"></a>Protocolos

Os SDKs do dispositivo suporta os seguintes protocolos de rede para ligar a um IoT hub:

- MQTT
- AMQP
- HTTPS

Para informações sobre estes protocolos de diferença e orientações sobre como escolher um, consulte [escolha um protocolo de comunicação](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols).

Se o dispositivo não é possível utilizar qualquer um dos protocolos suportados, pode utilizar o limite de IoT do Azure para a conversão de protocolo. Limite de IoT suporta outros cenários de intelligence no limite para a descarga de processamento na periferia da aplicação do Azure IoT Central.

## <a name="security"></a>Segurança

Todos os dados trocados entre dispositivos e o Azure IoT Central são encriptados. IoT Hub autentica todos os pedidos de um dispositivo que estabelece ligação a qualquer um dos pontos finais do IoT Hub orientado para o dispositivo. Para evitar trocar credenciais através da transmissão, um dispositivo utiliza tokens assinados para autenticar. Para obter mais informações, consulte, [controlar o acesso ao IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> Atualmente, os dispositivos que ligam à Azure IoT Central tem de utilizar os tokens SAS. Certificados x. 509 não são suportados para dispositivos que ligam à Azure IoT Central.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu acerca da conetividade do dispositivo no Azure IoT Central, Eis os passos sugeridos:

- [Preparar e ligar um dispositivo DevKit](howto-connect-devkit.md)
- [Preparar e ligar um Raspberry Pi](howto-connect-raspberry-pi-python.md)
- [Ligar um cliente de Node.js genérico a sua aplicação do Azure IoT Central](howto-connect-nodejs.md)
