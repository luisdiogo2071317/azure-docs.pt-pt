---
title: Suporte de plataforma do Azure IoT Device SDKs | Documentos da Microsoft
description: Conceitos - lista de plataformas suportadas pelos SDKs de dispositivo de IoT do Azure
author: yzhong94
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: yizhon
ms.openlocfilehash: cf3c80424c4626b62317bda537f9491cafc8198c
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/11/2018
ms.locfileid: "40043876"
---
# <a name="azure-iot-sdks-platform-support"></a>Suporte de plataformas de SDKs do IoT do Azure

[Os SDKs IoT do Azure](iot-hub-devguide-sdks.md) são um conjunto de bibliotecas para interagir com o IoT Hub e o serviço de aprovisionamento de dispositivos com linguagem ampla e suporte de plataforma.  Os SDKs são executados em plataformas mais comuns e os desenvolvedores poderá portar o SDK de C para a plataforma específica ao seguir a [portar orientações](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md). 

A Microsoft suporta uma variedade de plataformas/sistemas operativos/estruturas e pode ser estendido com o SDK de C do Azure IoT.  Algumas são suportadas oficialmente pela equipe, agrupada em camadas que representam o nível de suporte, os usuários podem esperar.  Totalmente suportado meios de plataformas que a Microsoft:
    - Continuamente compilação e teste de ponto a ponto contra mestre e o LTS (ões) suportada
    - Fornecer ou pacotes de guia de instalação, se aplicável
    - Suporte completo no GitHub

Além disso, uma lista de parceiros tem portado nosso SDK de C para mais plataformas e que estão mantendo a camada de abstração de plataforma (PAL).  [Catálogo de dispositivos do IoT Azure Certified for](https://catalog.azureiotsolutions.com/) também funcionalidades de uma lista de plataformas de SO os vários SDKs tem sido testada relativamente.  Além disso, os SDKs também regularmente criar nessas plataformas, com teste limitado e suportam:
- MBED2
- Arduino
- Windows CE 2013 (preterir em Outubro de 2018)
- .NET standard 1.3 com .NET Core 2.1 e o .NET Framework 4.7
- Xamarin iOS, Android, UWP
- Android com o Java

## <a name="supported-platforms"></a>Plataformas suportadas
### <a name="c-sdk"></a>SDK DE C
| SO                  | Arch | Compilador             | Biblioteca TLS       |
|---------------------|------|----------------------|-------------------|
| Ubuntu 16.04 LTS    | X64  | GCC 5.4.0            | OpenSSL - 1.0.2g |
| Ubuntu 18.04 LTS    | X64  | GCC 7.3              | WolfSSL – 1.13    |
| Ubuntu 18.04 LTS    | X64  | Clang 6.0.X          | OpenSSL – 1.1.0g  |
| OSX 10.13.4         | X64  | XCode 9.4.1          | OSX nativo        |
| Windows Server 2016 | X64  | Visual Studio 14.0.X | SChannel          |
| Windows Server 2016 | x86  | Visual Studio 14.0.X | SChannel          |
| Debian 9 Stretch    | X64  | GCC 7.3              | OpenSSL – 1.1.0f  |

### <a name="python-sdk"></a>SDK Python
| SO                  | Arch | Compilador   | Biblioteca TLS |
|---------------------|------|------------|-------------|
| Windows Server 2016 | x86  | Python 2.7 | OpenSSL     |
| Windows Server 2016 | X64  | Python 2.7 | OpenSSL     |
| Windows Server 2016 | x86  | Python 3.5 | OpenSSL     |
| Windows Server 2016 | X64  | Python 3.5 | OpenSSL     |
| Ubuntu 18.04 LTS    | x86  | Python 2.7 | OpenSSL     |
| Ubuntu 18.04 LTS    | x86  | Python 3.4 | OpenSSL     |
| Alta de MacOS Sierra   | X64  | Python 2.7 | OpenSSL     |

### <a name="net-sdk"></a>SDK .NET
| SO                  | Arch | Framework            | Standard          |
|---------------------|------|----------------------|-------------------|
| Ubuntu 16.04 LTS    | X64  | .NET core 2.1        | .NET standard 2.0 |
| Windows Server 2016 | X64  | .NET core 2.1        | .NET standard 2.0 |
| Windows Server 2016 | X64  | .NET framework 4.7   | .NET standard 2.0 |
| Windows Server 2016 | X64  | .NET framework 4.5.1 | N/A               |

### <a name="nodejs-sdk"></a>SDK Node.js
| SO                                           | Arch | Versão do nó |
|----------------------------------------------|------|--------------|
| Ubuntu 16.04 LTS (usando a imagem do docker nó 6) | X64  | Nó 6       |
| Windows Server 2016                          | X64  | Nó 6       |

### <a name="java-sdk"></a>SDK Java
| SO                  | Arch | Versão de Java |
|---------------------|------|--------------|
| Ubuntu 16.04 LTS    | X64  | Java 8       |
| Windows Server 2016 | X64  | Java 8       |

## <a name="partner-supported-platforms"></a>Plataformas de parceiro suportado
| Partner             | Dispositivos                            | Ligação                     | Suporte |
|---------------------|------------------------------------|--------------------------|---------|
| Qualcomm            | Qualcomm MDM9206 LTE IoT Modem     | [Qualcomm LTE para o SDK de IoT](https://developer.qualcomm.com/software/lte-iot-sdk) | [Fórum](https://developer.qualcomm.com/forums/software/lte-iot-sdk)   |
| ST Microelectronics | STM32L4 série STM32F4 série      | [X-CUBO-CLOUD](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32cube-expansion-packages/x-cube-cloud.html)             | [Suporte](https://www.st.com/content/st_com/en/support/support-home.html) |
|                     | Série de STM32F7                     | [CUBO-X-AZURE](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32cube-expansion-packages/x-cube-azure.html)             |         |
|                     | Kit de deteção de STM32L4 para o nó de IoT | [P-NUCLEO O AZURE](https://www.st.com/content/st_com/en/products/evaluation-tools/solution-evaluation-tools/communication-and-connectivity-solution-eval-boards/p-nucleo-azure1.html)          |         |
|                     |                                    | [FP-CLD O AZURE](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32-ode-function-pack-sw/fp-cld-azure1.html)            |         |
| Espressif           | ESP32                              | [ESP-azure](https://github.com/espressif/esp-azure)                | [GitHub](https://github.com/espressif/esp-azure)  |

## <a name="next-steps"></a>Passos Seguintes
- [SDKs de dispositivos e de serviços](iot-hub-devguide-sdks.md)
- [Orientações de migração](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md)