---
title: Compreender os SDKs IoT do Azure | Documentos da Microsoft
description: Guia do desenvolvedor – informações sobre e links para os vários Azure IoT dispositivo e o serviço de SDKs que pode utilizar para criar aplicações de dispositivos e aplicações de back-end.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: dobett
ms.openlocfilehash: ad1ce768ea5be2356f141d10a53ea0166546a2b7
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/11/2018
ms.locfileid: "42055573"
---
# <a name="understand-and-use-azure-iot-hub-sdks"></a>Compreender e utilizar os SDKs do IoT Hub do Azure

Existem duas categorias de kits de desenvolvimento de software (SDKs) para trabalhar com o IoT Hub:

* **SDKs de dispositivo** permitem-lhe criar aplicações que são executadas nos seus dispositivos IoT. Estas aplicações enviam telemetria ao seu hub IoT e, opcionalmente, recebem mensagens, tarefa, método ou atualizações de duplo do seu hub IoT.

* **SDKs de serviço** permitem-lhe gerir o seu hub IoT e, opcionalmente, enviar mensagens, agendar tarefas, invocar métodos diretos ou enviar atualizações de propriedade pretendida para os seus dispositivos IoT.

Saiba mais sobre os benefícios da programação com o Azure IoT SDKs [aqui][lnk-benefits-blog].

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="azure-iot-device-sdks"></a>SDKs de dispositivo de IoT do Azure

Microsoft Azure IoT device SDKs contêm código que facilita a criação de dispositivos e aplicações que ligar a e são geridas pelos serviços do IoT Hub do Azure.

O Azure IoT Hub device SDK para .NET: 
* Instalar a partir do [Nuget][lnk-nuget-csharp-device]
* [Código-fonte][lnk-dotnet-sdk]
* [Referência da API][lnk-dotnet-ref]

Dispositivos no IoT Hub do Azure SDK para c: escrito em ANSI C (C99) para portabilidade e a compatibilidade da ampla plataforma
* Instalar a partir do [apt-get, MBED, Arduino IDE ou Nuget][lnk-c-package]
* [Código-fonte][lnk-c-sdk]
* [Referência da API][lnk-c-ref]

O Azure IoT Hub device SDK para Java: 
* Adicionar ao [Maven] [ lnk-maven-device] projeto
* [Código-fonte][lnk-java-sdk]
* [Referência da API][lnk-java-ref]

O Azure IoT Hub device SDK para node. js: 
* Instalar a partir do [npm][lnk-npm-device]
* [Código-fonte][lnk-node-sdk]
* [Referência da API][lnk-node-ref]

O Azure IoT Hub device SDK para Python: 
* Instalar a partir do [pip][lnk-pip-device]
* [Código-fonte][lnk-python-sdk]

O Azure IoT Hub device SDK para iOS: 
* Instalar a partir do [CocoaPod][lnk-cocoa-device]
* [Exemplos][lnk-ios-sample]

> [!NOTE]
> Consulte os ficheiros Leia-me nos repositórios do GitHub para obter informações sobre como utilizar o idioma e gestores de pacotes específicos da plataforma para instalar os binários e dependências no computador de desenvolvimento.
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a>Compatibilidade de plataforma e de hardware do sistema operacional

Plataformas suportadas para os SDKs, podem ser encontradas neste [documento](iot-hub-device-sdk-platform-support.md).
Para obter mais informações sobre a compatibilidade de SDK com dispositivos de hardware específicas, consulte a [certificado para o catálogo de dispositivos do IoT do Azure] [ lnk-certified] ou repositório individual.

## <a name="azure-iot-service-sdks"></a>SDKs de serviço de IoT do Azure

Os SDKs de serviço de IoT do Azure contêm código para facilitar a criação de aplicativos que interagem diretamente com o IoT Hub para gerir dispositivos e segurança.

Serviço do Azure IoT Hub SDK para .NET:
* Transferir a partir do [Nuget][lnk-nuget-csharp-service]
* [Código-fonte][lnk-dotnet-sdk]
* [Referência da API][lnk-dotnet-service-ref]

Serviço do Azure IoT Hub SDK para Java: 
* Adicionar ao [Maven] [ lnk-maven-service] projeto
* [Código-fonte][lnk-java-sdk]
* [Referência da API][lnk-java-service-ref]

Serviço do Azure IoT Hub SDK para node. js: 
* Transferir a partir do [npm][lnk-npm-service]
* [Código-fonte][lnk-node-sdk]
* [Referência da API][lnk-node-service-ref]

Serviço do Azure IoT Hub SDK para Python: 
* Transferir a partir do [pip][lnk-pip-service]
* [Código-fonte][lnk-python-sdk]

Serviço IoT Hub do Azure SDK para c: 
* Transferir a partir do [apt-get, MBED, Arduino IDE ou Nuget][lnk-c-package]
* [Código-fonte][lnk-c-sdk]

Serviço do Azure IoT Hub SDK para iOS: 
* Instalar a partir do [CocoaPod][lnk-cocoa-service]
* [Exemplos][lnk-ios-sample]

> [!NOTE]
> Consulte os ficheiros Leia-me nos repositórios do GitHub para obter informações sobre como utilizar o idioma e gestores de pacotes específicos da plataforma para instalar os binários e dependências no computador de desenvolvimento.



## <a name="next-steps"></a>Passos Seguintes

Os SDKs IoT do Azure também fornecem um conjunto de ferramentas para ajudá-lo desenvolvimento:
* [iothub-diagnostics](https://github.com/Azure/iothub-diagnostics): uma ferramenta de linha de comandos para várias plataformas para ajudar a diagnosticar problemas relacionados com a ligação com o IoT Hub.
* [Explorador de dispositivos](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer): um aplicativo de desktop do Windows para ligar ao seu IoT Hub.

Outros tópicos de referência neste guia de programador do IoT Hub incluem:

* [Pontos finais do IoT Hub][lnk-devguide-endpoints]
* [Linguagem de consulta do IoT Hub para dispositivos duplos, tarefas e encaminhamento de mensagens][lnk-devguide-query]
* [Quotas e limitação][lnk-devguide-quotas]
* [Suporte para MQTT de Hub IoT][lnk-devguide-mqtt]
* [Referência da API de REST do Hub IoT][lnk-rest-ref]
* [Suporte de plataforma do Azure IoT SDK](iot-hub-device-sdk-platform-support.md)

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
[lnk-cocoa-device]: https://cocoapods.org/pods/AzureIoTHubClient
[lnk-ios-sample]: https://github.com/Azure-Samples/azure-iot-samples-ios
[lnk-cocoa-service]: https://cocoapods.org/pods/AzureIoTHubServiceClient