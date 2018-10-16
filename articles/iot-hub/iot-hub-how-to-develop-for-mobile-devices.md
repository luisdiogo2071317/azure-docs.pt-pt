---
title: Desenvolver para dispositivos móveis com os SDKs IoT do Azure | Documentos da Microsoft
description: Desenvolvedor orientá - Saiba mais sobre como desenvolver para dispositivos móveis através de SDKs do Azure IoT Hub.
author: yzhong94
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/16/2018
ms.author: yizhon
ms.openlocfilehash: 4a94abe69b525dc1b03fe2c1ae9593f3c6399f56
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339758"
---
# <a name="develop-for-mobile-devices-using-azure-iot-sdks"></a>Desenvolver para dispositivos móveis com os SDKs IoT do Azure

As coisas de Internet das coisas podem referir-se a uma vasta gama de dispositivos com capacidade de variadas: sensores, microcontroladores, dispositivos inteligentes, gateways industriais e até mesmo dispositivos móveis.  Um dispositivo móvel pode ser um dispositivo de IoT, onde está a enviar telemetria dispositivo para a cloud e gerida pela cloud.  Também pode ser o dispositivo a executar uma aplicação de serviço de back-end, que gere os outros dispositivos de IoT.  Em ambos os casos [SDKs do Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks) pode ser usado para desenvolver aplicativos que funcionam para dispositivos móveis.  

## <a name="develop-for-native-ios-platform"></a>Desenvolver para a plataforma iOS nativo

SDKs do IoT Hub do Azure fornecem suporte de plataforma nativa no iOS através do SDK de C do Azure IoT Hub.  Pode pensá-lo como um SDK que podem ser incorporados em seu projeto Swift ou com o objetivo C XCode do iOS.  Existem duas formas de utilizar o SDK de C no iOS:

* Utilize as bibliotecas de CocoaPod no projeto XCode diretamente.  

* Transfira o código-fonte para o SDK de C e crie seguintes funcionalidades de plataforma iOS a [criar a instrução](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) para MacOS.  

SDK de C de Hub IoT do Azure é escrito em C99 para beneficiar de máxima portabilidade para várias plataformas.  O processo de portagem envolve a criação de uma camada de adoção dinâmico para os componentes específicos da plataforma, que pode ser encontrado aqui para [iOS](https://github.com/Azure/azure-c-shared-utility/tree/master/pal/ios-osx).  As funcionalidades no SDK de C podem ser aproveitadas na plataforma iOS, incluindo os primitivos de IoT Hub do Azure suportados e funcionalidades específicas do SDK, tais como política para a confiabilidade da rede de repetição.  A interface para iOS SDK também é semelhante à interface do SDK de C do Azure IoT Hub.  

Estes documentação atualizados permitem saber como desenvolver um aplicativo de dispositivo ou serviço num dispositivo iOS:

* [Guia de Início Rápido: Enviar telemetria a partir de um dispositivo para um hub IoT](quickstart-send-telemetry-ios.md)  

* [Enviar mensagens a partir da cloud para o seu dispositivo com o IoT hub](iot-hub-ios-swift-c2d.md) 

## <a name="develop-with-azure-iot-hub-cocoapod-libraries"></a>Programar com bibliotecas do Azure IoT Hub CocoaPod

Lançamentos de SDKs do IoT Hub do Azure um conjunto de bibliotecas de CocoaPod Objective-C para o desenvolvimento de iOS.  Para ver a lista mais recente das bibliotecas de CocoaPod, consulte [CocoaPods para Microsoft Azure IoT](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/samples/ios/CocoaPods.md).  Assim que as bibliotecas relevantes estão incorporadas no projeto XCode, existem duas formas de escrever código relacionados com o IoT Hub:

* Função de objetivo C: se o seu projeto é escrito no Objective-C, pode chamar as APIs do SDK de C do Azure IoT Hub diretamente.  Se o seu projeto é escrito em Swift, pode chamar `@objc func` antes de criar a sua função e continuar a escrever todos os demonstração de Teoremas relacionados com o IoT Hub do Azure com o código C ou o Objective-C.  Um conjunto de exemplos que demonstram ambos pode ser encontrado no [repositório de exemplo](https://github.com/Azure-Samples/azure-iot-samples-ios).  

* Incorporar amostras em C: Se tiver escrito um aplicativo de dispositivo C, que o possa referenciar diretamente no projeto XCode:

    * Adicione o ficheiro Sample ao seu projeto XCode do XCode.  
    
    * Adicione o arquivo de cabeçalho em sua dependência.  Um arquivo de cabeçalho está incluído nos [repositório de exemplo](https://github.com/Azure-Samples/azure-iot-samples-ios) como exemplo. Para obter mais informações, visite a página de documentação da Apple para o [Objective-C](https://developer.apple.com/documentation/objectivec).

## <a name="next-steps"></a>Passos Seguintes

* [Referência da API de REST do Hub IoT](https://docs.microsoft.com/rest/api/iothub/)
* [Código de origem do SDK de C do IoT do Azure](https://github.com/Azure/azure-iot-sdk-c)
