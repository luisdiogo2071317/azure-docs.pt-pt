---
title: Compreender os SDKs IoT do Azure | Microsoft Docs
description: Guia para programadores - informações sobre e ligações para os vários Azure SDKs IoT do serviço e dispositivo que pode utilizar para criar aplicações de dispositivos e aplicações de back-end.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: dobett
ms.openlocfilehash: 31fed4ad1c9a790f771f8943775804976070fc0b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34633388"
---
# <a name="understand-and-use-azure-iot-hub-sdks"></a>Compreender e utilizar os SDKs do IoT Hub do Azure

Existem duas categorias de kits de desenvolvimento de software (SDKs) para trabalhar com o IoT Hub:

* **SDKs do dispositivo** permitem-lhe criar aplicações que são executados nos seus dispositivos de IoT. Estas aplicações enviar telemetria ao seu IoT hub e, opcionalmente, recebem mensagens, tarefa, método ou atualizações de duplo do seu IoT hub.

* **Serviço SDKs** permitem-lhe gerir o seu IoT hub e, opcionalmente, enviar mensagens, agendar tarefas, invocar métodos diretos ou enviar atualizações da propriedade pretendido para os seus dispositivos de IoT.

Saiba mais sobre as vantagens de utilizar os SDKs IoT do Azure a desenvolver [aqui][lnk-benefits-blog].

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="azure-iot-device-sdks"></a>SDKs do Azure do dispositivo IoT

O dispositivo do Microsoft Azure IoT SDKs contenham código que facilita a criação de dispositivos e aplicações que ligam ao e são geridas pelos serviços do IoT Hub do Azure.

Azure IoT Hub dispositivo SDK para .NET: 
* Instalar a partir do [Nuget][lnk-nuget-csharp-device]
* [Código de origem][lnk-dotnet-sdk]
* [Referência da API][lnk-dotnet-ref]

Dispositivos de IoT Hub do Azure SDK para c: escritas em ANSI C (C99) para compatibilidade de plataforma abrangente e portabilidade
* Instalar a partir do [apt get, MBED, Arduino IDE ou Nuget][lnk-c-package]
* [Código de origem][lnk-c-sdk]
* [Referência da API][lnk-c-ref]

Azure IoT Hub dispositivo SDK para Java: 
* Adicionar a [Maven] [ lnk-maven-device] projeto
* [Código de origem][lnk-java-sdk]
* [Referência da API][lnk-java-ref]

Azure IoT Hub dispositivo SDK para Node.js: 
* Instalar a partir do [npm][lnk-npm-device]
* [Código de origem][lnk-node-sdk]
* [Referência da API][lnk-node-ref]

Azure IoT Hub dispositivo SDK para Python: 
* Instalar a partir do [pip][lnk-pip-device]
* [Código de origem][lnk-python-sdk]

> [!NOTE]
> Consulte os ficheiros Leia-me no repositórios do GitHub para obter informações sobre como utilizar o idioma e gestores de pacote específico da plataforma para instalar os binários e dependências no computador de desenvolvimento.
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a>Compatibilidade de plataforma e o hardware do SO

Para obter mais informações sobre a compatibilidade de SDK com dispositivos de hardware específicos, consulte o [Azure certificadas para o catálogo de dispositivos de IoT] [ lnk-certified] ou repositório individuais.

## <a name="azure-iot-service-sdks"></a>SDKs de serviço do IoT do Azure

Os SDKs do serviço IoT do Azure contêm código para facilitar a criar aplicações que interagir diretamente com o IoT Hub para gerir dispositivos e segurança.

Serviço do Azure IoT Hub SDK para .NET:
* Transferir a partir do [Nuget][lnk-nuget-csharp-service]
* [Código de origem][lnk-dotnet-sdk]
* [Referência da API][lnk-dotnet-service-ref]

Serviço do Azure IoT Hub SDK para Java: 
* Adicionar a [Maven] [ lnk-maven-service] projeto
* [Código de origem][lnk-java-sdk]
* [Referência da API][lnk-java-service-ref]

Serviço do Azure IoT Hub SDK para Node.js: 
* Transferir a partir do [npm][lnk-npm-service]
* [Código de origem][lnk-node-sdk]
* [Referência da API][lnk-node-service-ref]

Serviço do Azure IoT Hub SDK para Python: 
* Transferir a partir do [pip][lnk-pip-service]
* [Código de origem][lnk-python-sdk]

Serviço de IoT Hub do Azure SDK para c: 
* Transferir a partir do [apt get, MBED, Arduino IDE ou Nuget][lnk-c-package]
* [Código de origem][lnk-c-sdk]

> [!NOTE]
> Consulte os ficheiros Leia-me no repositórios do GitHub para obter informações sobre como utilizar o idioma e gestores de pacote específico da plataforma para instalar os binários e dependências no computador de desenvolvimento.


## <a name="next-steps"></a>Passos Seguintes

Outros tópicos de referência neste guia para programadores do IoT Hub incluem:

* [Pontos finais de IoT Hub][lnk-devguide-endpoints]
* [Idioma de consulta do IoT Hub para dispositivos duplos, tarefas e o encaminhamento de mensagens][lnk-devguide-query]
* [Quotas e limitação][lnk-devguide-quotas]
* [Suporte de MQTT do IoT Hub][lnk-devguide-mqtt]
* [Referência da API de REST do IoT Hub][lnk-rest-ref]

<!-- Links and images -->

[lnk-c-sdk]: https://github.com/Azure/azure-iot-sdk-c
[lnk-dotnet-sdk]: https://github.com/Azure/azure-iot-sdk-csharp
[lnk-java-sdk]: https://github.com/Azure/azure-iot-sdk-java
[lnk-node-sdk]: https://github.com/Azure/azure-iot-sdk-node
[lnk-python-sdk]: https://github.com/Azure/azure-iot-sdk-python
[lnk-certified]: https://catalog.azureiotsuite.com/

[lnk-dotnet-ref]: https://docs.microsoft.com/dotnet/api/microsoft.azure.devices?view=azure-dotnet
[lnk-dotnet-service-ref]: https://docs.microsoft.com/dotnet/api/microsoft.azure.devices
[lnk-c-ref]: https://azure.github.io/azure-iot-sdk-c/index.html
[lnk-java-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device
[lnk-node-ref]: https://docs.microsoft.com/javascript/api/azure-iot-device/?view=azure-iot-typescript-latest
[lnk-rest-ref]: https://docs.microsoft.com/rest/api/iothub/
[lnk-java-service-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service
[lnk-node-service-ref]: https://docs.microsoft.com/javascript/api/azure-iothub/?view=azure-iot-typescript-latest

[lnk-maven-device]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-device-sdk
[lnk-maven-service]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk
[lnk-npm-device]: https://www.npmjs.com/package/azure-iot-device
[lnk-npm-service]: https://www.npmjs.com/package/azure-iothub
[lnk-nuget-csharp-device]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-csharp-service]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-c-package]: https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/readme.md
[lnk-pip-device]: https://pypi.python.org/pypi/azure-iothub-device-client/
[lnk-pip-service]: https://pypi.python.org/pypi/azure-iothub-service-client/


[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-benefits-blog]: https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/
