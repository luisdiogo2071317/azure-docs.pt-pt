---
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.author: dobett
ms.openlocfilehash: 304637422c0b8fd4dfa99e2e434e13d12f1fb342
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/26/2018
ms.locfileid: "50165349"
---
> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)
> * [Python](../articles/iot-hub/iot-hub-python-twin-getstarted.md)

Os dispositivos duplos são documentos JSON que armazenam informações de estado dos dispositivos (metadados, configurações e condições). O IoT Hub cria um dispositivo duplo para cada dispositivo que se liga ao mesmo.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Utilize dispositivos duplos para:

* Store metadados do dispositivo da sua solução de back-end.

* Comunicar informações de estado atual como recursos disponíveis e condições (por exemplo, o método de conectividade utilizado) da sua aplicação de dispositivo.

* Sincronize o estado dos fluxos de execução longa (por exemplo, atualizações de firmware e configuração) entre uma aplicação de dispositivo e uma aplicação de back-end.

* Consulte os metadados do dispositivo, a configuração ou o estado.

Dispositivos duplos são concebidos para sincronização e para consultar as configurações de dispositivos e condições. Podem encontrar mais informações sobre quando utilizar dispositivos duplos no [compreender os dispositivos duplos](../articles/iot-hub/iot-hub-devguide-device-twins.md).

Dispositivos duplos são armazenados num IoT hub e contenham:

* *etiquetas*, metadados do dispositivo acessível apenas pelo back-end da solução;

* *propriedades pretendidas*, objetos JSON modificáveis pela solução de back-end e observable pela aplicação do dispositivo; e

* *Propriedades comunicadas*, objetos JSON modificáveis pela aplicação de dispositivo e legíveis pelo back-end da solução. Etiquetas e propriedades não podem conter matrizes, mas objetos podem ser aninhados.

![Funcionalidade de que mostra imagens do dispositivo duplo](./media/iot-hub-selector-twin-get-started/twin.png)

Além disso, o back-end de solução pode consultar dispositivos duplos com base em todos os dados acima.
Consulte a [compreender os dispositivos duplos](../articles/iot-hub/iot-hub-devguide-device-twins.md) para obter mais informações sobre dispositivos duplos e para o [linguagem de consulta do IoT Hub](../articles/iot-hub/iot-hub-devguide-query-language.md) referência para a consulta.


Este tutorial mostrar-lhe como:

* Criar uma aplicação de back-end que adiciona *etiquetas* para um dispositivo duplo e uma aplicação de dispositivo simulado que comunica o respetivo canal de conectividade como um *comunicado propriedade* no dispositivo duplo.

* Consultar os dispositivos da sua aplicação de back-end a utilizar os filtros nas etiquetas e propriedades que criou anteriormente.
