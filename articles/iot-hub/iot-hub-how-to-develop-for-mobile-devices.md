---
title: Desenvolver para dispositivos móveis com os SDKs IoT do Azure | Microsoft Docs
description: Programador orientá - Saiba mais sobre como desenvolver para dispositivos móveis com os SDKs do IoT Hub do Azure.
services: iot-hub
documentationcenter: ''
author: yzhong94
manager: timlt
editor: ''
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/16/2018
ms.author: yizhon
ms.openlocfilehash: 824d577a4709a388bd858bdda6ef5f11f53dde58
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/01/2018
---
# <a name="develop-for-mobile-devices-using-azure-iot-sdks"></a>Desenvolver para dispositivos móveis com os SDKs IoT do Azure

Coisas na Internet das coisas se refiram a uma vasta gama de dispositivos com capacidade variando: sensores, microcontrollers, dispositivos inteligentes, gateways industriais e dispositivos móveis, mesmo.  Um dispositivo móvel pode ser um dispositivo IoT, onde está a enviar telemetria do dispositivo para a nuvem e geridos pela nuvem.  Também pode ser o dispositivo a executar uma aplicação de serviço de back-end, que gere outros dispositivos de IoT.  Em ambos os casos, [SDKs do IoT Hub do Azure] [ lnk-sdk-overview] podem ser utilizadas para desenvolver aplicações que funcionam para dispositivos móveis.  

## <a name="develop-for-native-ios-platform"></a>Desenvolver para a plataforma iOS nativo

SDKs do IoT Hub do Azure fornecem suporte de plataforma iOS nativo através do SDK do Azure IoT Hub C.  Pode considerá-lo como um SDK iOS do que poderá incorporar no seu projeto Swift ou Objective C XCode.  Existem duas formas de utilizar o SDK de C no iOS:
- Utilize as bibliotecas de CocoaPod no projeto XCode diretamente.  
- Transferir o código de origem para o SDK de C e criar seguinte de plataforma iOS a [compilar a instrução] [ lnk-c-devbox] para MacOS.  

SDK do C do Hub IoT do Azure é escrito em C99 para portabilidade máxima para várias plataformas.  O processo de porting envolve a escrever uma camada de adoção dinâmico para os componentes de plataforma específica, que pode ser encontrada aqui para [iOS][lnk-ios-pal].  As funcionalidades no C SDK podem ser aproveitadas na plataforma iOS, incluindo as primitivas do IoT Hub do Azure suportadas e funcionalidades específicas do SDK, tais como política para fiabilidade de rede de repetição.  A interface para iOS SDK também é semelhante à interface para C SDK do Azure IoT Hub.  

Estes documentations guiá como desenvolver uma aplicação de dispositivo ou aplicação de serviço num dispositivo iOS:
- [Início rápido: Enviar telemetria a partir de um dispositivo a um IoT hub][lnk-device-ios-quickstart]  
- [Enviar mensagens da nuvem para o seu dispositivo com o IoT hub][lnk-service-ios-quickstart]  

### <a name="develop-with-azure-iot-hub-cocoapod-libraries"></a>Desenvolver com bibliotecas do Azure IoT Hub CocoaPod

Azure SDKs do IoT Hub disponibiliza um conjunto de bibliotecas de CocoaPod Objective-C para o desenvolvimento de iOS.  Para ver a lista mais recente das bibliotecas de CocoaPod, consulte [o CocoaPods para o Microsoft Azure IoT][lnk-cocoapod-list].  Depois das bibliotecas relevantes estão incorporadas no seu projeto XCode, existem duas formas de escrever código relacionados com o IoT Hub:
- Função de objetivo C: se o seu projeto é escrito em Objective-C, pode chamar as APIs do SDK do Azure IoT Hub C diretamente.  Se o seu projeto é escrito no Swift, pode chamar ``@objc func`` antes de criar a sua função e continuar a escrever logics todos os relacionados com o Azure IoT Hub com o código C ou Objective-C.  Pode encontrar um conjunto de exemplos demonstrar ambos o [repositório de exemplo][lnk-ios-samples-repo].  
- Incorporar amostras de C: se escrever uma aplicação de dispositivo C, pode referenciá-lo diretamente no seu projeto XCode:
    - Adicione o ficheiro de sample.c ao seu projeto XCode do XCode.  
    - Adicione o ficheiro de cabeçalho para a dependência.  Um ficheiro de cabeçalho está incluído no [repositório de exemplo] [ lnk-ios-samples-repo] como exemplo.  Para obter mais informações, visite a página de documentação da Apple para [Objective-C](https://developer.apple.com/documentation/objectivec).

## <a name="next-steps"></a>Passos Seguintes
* [Referência da API de REST do IoT Hub][lnk-rest-ref]
* [Código de origem do SDK de C do IoT do Azure][lnk-c-sdk]

[lnk-sdk-overview]: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks
[lnk-c-devbox]: https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-device-ios-quickstart]:https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-ios
[lnk-service-ios-quickstart]: https://docs.microsoft.com/azure/iot-hub/iot-hub-ios-swift-c2d
[lnk-cocoapod-list]: https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/samples/ios/CocoaPods.md
[lnk-ios-samples-repo]: https://github.com/Azure-Samples/azure-iot-samples-ios
[lnk-ios-pal]: https://github.com/Azure/azure-c-shared-utility/tree/master/pal/ios-osx
[lnk-c-sdk]: https://github.com/Azure/azure-iot-sdk-c
[lnk-rest-ref]: https://docs.microsoft.com/rest/api/iothub/
