---
title: IoT Hub do Azure desenvolver para dispositivos restrita | Microsoft Docs
description: Guia para programadores - orientações sobre como desenvolver com os SDKs IoT do Azure para dispositivos restrita.
services: iot-hub
documentationcenter: c
author: yzhong94
manager: timlt
editor: ''
ms.assetid: 979136db-c92d-4288-870c-f305e8777bdd
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2018
ms.author: yizhon
ms.openlocfilehash: 62065b78e3f8191c6423ba9dba4a8f7d16fad114
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655569"
---
# <a name="develop-for-constrained-devices-using-azure-iot-sdks"></a>Desenvolver para dispositivos restrita com os SDKs IoT do Azure

## <a name="develop-using-azure-iot-hub-c-sdk"></a>Desenvolver com o Azure IoT Hub C SDK
SDK do C do Hub IoT do Azure é escrito em ANSI C (C99), que torna adequada para operar uma variedade de plataformas com ocupa pouco espaço de disco e da memória.  A RAM recomendada é, pelo menos, a 64 KB, mas os requisitos de espaço de memória exato dependem o protocolo utilizado, o número de ligações aberto, bem como a plataforma de destino.

C do SDK está disponível no formato de pacote de apt get, NuGet e MBED.  Para visar dispositivos restrita, poderá pretender criar o SDK localmente para a sua plataforma de destino. Esta documentação demonstra como remover determinadas funcionalidades para reduzir os requisitos de espaço de utilizar o SDK de C [cmake][lnk-cmake].  Além disso, esta documentação aborda a melhor prática de modelos para trabalhar com dispositivos restrita de programação.

### <a name="building-the-c-sdk-for-constrained-devices"></a>Criar o SDK de C para dispositivos restrita
#### <a name="prerequisites"></a>Pré-requisitos
Siga este [guia de configuração] [ lnk-devbox-setup] para preparar o ambiente de desenvolvimento para criar o SDK de C.  Antes de obter para o passo para criação com cmake, pode invocar cmake sinalizadores para remover funcionalidades não utilizadas.

#### <a name="remove-additional-protocol-libraries"></a>Remover as bibliotecas de protocolo adicionais
SDK de C suporta cinco protocolos hoje: MQTT, MQTT através de WebSocket, AMQPs, AMQP através de WebSocket e HTTPS.    A maioria dos cenários requerem os protocolos de um ou dois em execução num cliente, por conseguinte, pode remover a biblioteca de protocolo que não estiver a utilizar o SDK.  Informações adicionais sobre como escolher o protocolo de comunicação adequado para o seu cenário pode ser encontrado deste [documento][lnk-choosing-protocol].  Por exemplo, MQTT é um protocolo simples que frequentemente melhor é adequado para dispositivos restrita.

Pode remover bibliotecas AMQP e HTTP utilizando o seguinte comando cmake:
```
cmake -Duse_amqp=OFF -Duse_http=OFF <Path_to_cmake>
```

#### <a name="remove-sdk-logging-capability"></a>Remover a capacidade de registos do SDK
O SDK de C fornece o registo extensivo ao longo para ajudar a depurar. Pode remover a capacidade de registo para dispositivos de produção, utilizando o seguinte comando cmake:
```
cmake -Dno_logging=OFF <Path_to_cmake>
```

#### <a name="remove-upload-to-blob-capability"></a>Remova o carregamento para o blob de capacidade
Pode carregar ficheiros grandes para armazenamento do Azure com a capacidade incorporada no SDK.  IoT Hub do Azure funciona como um distribuidor para uma conta de armazenamento do Azure associada.  Pode utilizar esta funcionalidade para enviar os ficheiros de suporte de dados, lotes de telemetria grande e registos.  Saiba mais sobre como carregar ficheiros com o IoT Hub deste [documento][lnk-hub-file-upload].  Se a aplicação não necessitar desta funcionalidade, pode remover esta funcionalidade utilizando o seguinte comando cmake:
```
cmake -Ddont_use_uploadtoblob=ON <Path_to_cmake>
```
#### <a name="running-strip-on-linux-environment"></a>Faixa em execução no ambiente do Linux
Se os binários executar num sistema Linux, pode tirar partido de [da faixa comando] [ lnk-strip] para reduzir o tamanho da aplicação final após a compilação.
```
strip -s <Path_to_executable>
```

### <a name="programming-models-for-constrained-devices"></a>Modelos de programação para dispositivos restrita
#### <a name="avoid-using-the-serializer"></a>Evite utilizar o serializador
O SDK de C tem opcional [serializador][lnk-serializer], que lhe permite utilizar tabelas de mapeamento declarativos para definir métodos e propriedades do dispositivo duplo.  O serializador foi concebido para simplificar o desenvolvimento, mas adiciona sobrecarga, que não é ideal para dispositivos restrita.  Neste caso, considere a utilização de APIs de cliente primitivos e analisar json utilizando um analisador simples, tais como [parson][lnk-parson].

#### <a name="use-the-lower-layer-ll"></a>Utilizar a camada inferior (_odas_)
O SDK de C suporta dois modelos de programação.  Tem de um conjunto de APIs com um _odas_ infix, que representa a camada inferior.  Este conjunto de APIs são ponderação mais ligeira e não rotação cópias de segurança de threads de trabalho, que significa que o utilizador tem manualmente controlar o agendamento.  Por exemplo, para o cliente do dispositivo, o _odas_ APIs podem ser encontradas deste [ficheiro de cabeçalho](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_device_client_ll.h).  Outro conjunto de APIs sem o _odas_ índice chama a camada de comodidade, onde um thread de trabalho é puserem automaticamente.  Por exemplo, a camada de conveniência APIs para o cliente do dispositivo pode ser encontrada deste [ficheiro de cabeçalho](https://github.com/Azure/azure-iot-sdk-c/blob/master/iothub_client/inc/iothub_device_client.h).  Para dispositivos restrita onde cada thread adicional pode demorar uma percentagem substancial de recursos do sistema, considere a utilização _odas_ APIs.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre a arquitetura do SDK do Azure IoT C:
-   [Código de origem do SDK de C do IoT do Azure](https://github.com/Azure/azure-iot-sdk-c/)
-   [Dispositivos de IoT do Azure SDK para introdução de C](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-sdk-c-intro)

------
[lnk-cmake]: https://cmake.org/
[lnk-devbox-setup]:  https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md
[lnk-choosing-protocol]: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols
[lnk-hub-file-upload]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-file-upload
[lnk-strip]: https://en.wikipedia.org/wiki/Strip_(Unix)
[lnk-serializer]: https://github.com/Azure/azure-iot-sdk-c/tree/master/serializer
[lnk-parson]: https://github.com/kgabis/parson