---
title: Compreender os SDKs IoT do Azure | Documentos da Microsoft
description: Guia do desenvolvedor – informações sobre e links para os vários Azure IoT dispositivo e o serviço de SDKs que pode utilizar para criar aplicações de dispositivos e aplicações de back-end.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: dobett
ms.openlocfilehash: 1eeb0afdd5ffcbe00357914d6a98c8d0b3d452ec
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/02/2018
ms.locfileid: "48017964"
---
# <a name="understand-and-use-azure-iot-hub-sdks"></a>Compreender e utilizar os SDKs do IoT Hub do Azure

Existem três categorias de kits de desenvolvimento de software (SDKs) para trabalhar com o IoT Hub:

* **SDKs de dispositivo** permitem-lhe criar aplicações que são executadas nos seus dispositivos de IoT com o cliente do dispositivo ou o cliente do módulo. Estas aplicações enviam telemetria ao seu hub IoT e, opcionalmente, recebem mensagens, tarefa, método ou atualizações de duplo do seu hub IoT.  Também pode usar o cliente do módulo para criar [módulos](../iot-edge/iot-edge-modules.md) para [runtime do Azure IoT Edge](../iot-edge/about-iot-edge.md).

* **SDKs de serviço** permitem-lhe gerir o seu hub IoT e, opcionalmente, enviar mensagens, agendar tarefas, invocar métodos diretos ou enviar atualizações de propriedade pretendida para seus dispositivos IoT ou módulos.

* **SDKs de aprovisionamento de dispositivos** permite-lhe aprovisionar dispositivos no seu IoT Hub com o [serviço de aprovisionamento de dispositivo](../iot-dps/about-iot-dps.md).

Saiba mais sobre o [benefícios da programação com o Azure IoT SDKs](https://azure.microsoft.com/blog/benefits-of-using-the-azure-iot-sdks-in-your-azure-iot-solution/).

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="azure-iot-device-sdks"></a>SDKs de dispositivo de IoT do Azure

Microsoft Azure IoT device SDKs contêm código que facilita a criação de dispositivos e aplicações que ligar a e são geridas pelos serviços do IoT Hub do Azure.

O Azure IoT Hub device SDK para .NET: 

* Instalar a partir do [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/)
* [Código-fonte](https://github.com/Azure/azure-iot-sdk-csharp)
* [Referência da API](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices?view=azure-dotnet)
* [Referência do módulo](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet)

O Azure IoT Hub device SDK para C, escrito em ANSI C (C99) para portabilidade e a compatibilidade da ampla plataforma:

* Instalar a partir do [apt-get, MBED, Arduino IDE ou Nuget](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md)
* [Código-fonte](https://github.com/Azure/azure-iot-sdk-c)
* [Referência da API](https://azure.github.io/azure-iot-sdk-c/index.html)
* [Referência do módulo](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_module_client.h)

O Azure IoT Hub device SDK para Java: 

* Adicionar ao [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-device-sdk) projeto
* [Código-fonte](https://github.com/Azure/azure-iot-sdk-java)
* [Referência da API](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device)
* [Referência do módulo](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device._module_client?view=azure-java-stable)

O Azure IoT Hub device SDK para node. js: 

* Instalar a partir do [npm](https://www.npmjs.com/package/azure-iot-device)
* [Código-fonte](https://github.com/Azure/azure-iot-sdk-node)
* [Referência da API](https://docs.microsoft.com/javascript/api/azure-iot-device/?view=azure-iot-typescript-latest)
* [Referência do módulo](https://docs.microsoft.com/javascript/api/azure-iot-device/moduleclient?view=azure-node-latest)

O Azure IoT Hub device SDK para Python: 

* Instalar a partir do [pip](https://pypi.python.org/pypi/azure-iothub-device-client/)
* [Código-fonte](https://github.com/Azure/azure-iot-sdk-python)
* Referência da API: veja [referência da API de C](https://azure.github.io/azure-iot-sdk-c/index.html)

O Azure IoT Hub device SDK para iOS: 

* Instalar a partir do [CocoaPod](https://cocoapods.org/pods/AzureIoTHubClient)
* [Amostras](https://github.com/Azure-Samples/azure-iot-samples-ios)
* Referência da API: veja [referência da API de C](https://azure.github.io/azure-iot-sdk-c/index.html)

> [!NOTE]
> Consulte os ficheiros Leia-me nos repositórios do GitHub para obter informações sobre como utilizar o idioma e gestores de pacotes específicos da plataforma para instalar os binários e dependências no computador de desenvolvimento.
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a>Compatibilidade de plataforma e de hardware do sistema operacional

Plataformas suportadas para os SDKs que podem ser encontradas na [suporte de plataforma do Azure IoT SDKs](iot-hub-device-sdk-platform-support.md).

Para obter mais informações sobre a compatibilidade de SDK com dispositivos de hardware específicas, consulte a [certificação do Azure para o catálogo de dispositivos de IoT](https://catalog.azureiotsuite.com/) ou repositório individual.

## <a name="azure-iot-service-sdks"></a>SDKs de serviço de IoT do Azure

Os SDKs de serviço de IoT do Azure contêm código para facilitar a criação de aplicativos que interagem diretamente com o IoT Hub para gerir dispositivos e segurança.

Serviço do Azure IoT Hub SDK para .NET:

* Transferir a partir do [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Devices/)
* [Código-fonte](https://github.com/Azure/azure-iot-sdk-csharp)
* [Referência da API](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices)

Serviço do Azure IoT Hub SDK para Java: 

* Adicionar ao [Maven](https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md#for-the-service-sdk
) projeto
* [Código-fonte](https://github.com/Azure/azure-iot-sdk-java)
* [Referência da API](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service)

Serviço do Azure IoT Hub SDK para node. js: 

* Transferir a partir do [npm](https://www.npmjs.com/package/azure-iothub)
* [Código-fonte](https://github.com/Azure/azure-iot-sdk-node)
* [Referência da API](https://docs.microsoft.com/javascript/api/azure-iothub/?view=azure-iot-typescript-latest)

Serviço do Azure IoT Hub SDK para Python: 

* Transferir a partir do [pip](https://pypi.python.org/pypi/azure-iothub-service-client/)
* [Código-fonte](https://github.com/Azure/azure-iot-sdk-python)

Serviço IoT Hub do Azure SDK para c: 

* Transferir a partir do [apt-get, MBED, Arduino IDE ou Nuget](https://github.com/Azure/azure-iot-sdk-c/blob/master/readme.md)
* [Código-fonte](https://github.com/Azure/azure-iot-sdk-c)

Serviço do Azure IoT Hub SDK para iOS: 

* Instalar a partir do [CocoaPod](https://cocoapods.org/pods/AzureIoTHubServiceClient)
* [Amostras](https://github.com/Azure-Samples/azure-iot-samples-ios)

> [!NOTE]
> Consulte os ficheiros Leia-me nos repositórios do GitHub para obter informações sobre como utilizar o idioma e gestores de pacotes específicos da plataforma para instalar os binários e dependências no computador de desenvolvimento.

## <a name="device-provisioning-sdks"></a>SDKs de aprovisionamento de dispositivos

O **SDKs de aprovisionamento do Microsoft Azure** permite-lhe aprovisionar dispositivos no seu IoT Hub com o [serviço aprovisionamento de dispositivos](../iot-dps/about-iot-dps.md).

Aprovisionamento dispositivos e de serviços os SDKs do Azure para c#:

* [SDK de cliente de dispositivo de aprovisionamento](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/provisioning/device)
* [SDK de cliente do serviço de aprovisionamento](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/provisioning/service)

Aprovisionamento dispositivos e de serviços os SDKs do Azure para Java:

* [SDK de cliente de dispositivo de aprovisionamento](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning/provisioning-device-client)
* [SDK de cliente do serviço de aprovisionamento](https://github.com/Azure/azure-iot-sdk-java/blob/master/provisioning/provisioning-service-client)

Aprovisionamento dispositivos e de serviços os SDKs do Azure para node. js:

* [SDK de cliente de dispositivo de aprovisionamento](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/device)
* [SDK de cliente do serviço de aprovisionamento](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service)

Aprovisionamento dispositivos e de serviços os SDKs do Azure para Python:

* [SDK de cliente de dispositivo de aprovisionamento](https://github.com/Azure/azure-iot-sdk-python/blob/master/provisioning_device_client)
* [SDK de cliente do serviço de aprovisionamento](https://github.com/Azure/azure-iot-sdk-python/tree/master/provisioning_service_client)

Aprovisionamento dispositivos e de serviços os SDKs do Azure para c:

* [SDK de cliente de dispositivo de aprovisionamento](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client)
* [SDK de cliente do serviço de aprovisionamento](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_service_client)

## <a name="next-steps"></a>Passos Seguintes

Os SDKs IoT do Azure também fornecem um conjunto de ferramentas para ajudá-lo desenvolvimento:
* [iothub-diagnostics](https://github.com/Azure/iothub-diagnostics): uma ferramenta de linha de comandos para várias plataformas para ajudar a diagnosticar problemas relacionados com a ligação com o IoT Hub.
* [Explorador de dispositivos](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer): um aplicativo de desktop do Windows para ligar ao seu IoT Hub.

Outros tópicos de referência neste guia de programador do IoT Hub incluem:

* [Pontos finais do Hub IoT](iot-hub-devguide-endpoints.md)
* [Linguagem de consulta do IoT Hub para dispositivos duplos, tarefas e encaminhamento de mensagens](iot-hub-devguide-query-language.md)
* [Quotas e limitação](iot-hub-devguide-quotas-throttling.md)
* [Suporte para MQTT de Hub IoT](iot-hub-mqtt-support.md)
* [Referência da API de REST do Hub IoT](/rest/api/iothub/)
* [Suporte de plataforma do Azure IoT SDK](iot-hub-device-sdk-platform-support.md)