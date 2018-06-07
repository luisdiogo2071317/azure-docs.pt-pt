---
title: Ligar uma aplicação de cliente do Node.js genérica para o Azure IoT Central | Microsoft Docs
description: Como um programador de dispositivo, como ligar um dispositivo de Node.js genérico a sua aplicação do Azure IoT Central.
author: tbhagwat3
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 42ede975f2cfde2d9c0a61d15ba1af412a88c556
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628543"
---
# <a name="connect-a-generic-client-application-to-your-azure-iot-central-application-nodejs"></a>Ligar uma aplicação de cliente genérico à sua aplicação do Azure IoT Central (Node.js)

Este artigo descreve como, como um programador de dispositivo, para ligar uma aplicação Node.js genérica que representa um dispositivo físico para a aplicação do Microsoft Azure IoT Central.

## <a name="before-you-begin"></a>Antes de começar

Para executar os passos descritos neste artigo é necessário o seguinte:

1. Uma aplicação do Azure IoT Central. Para obter mais informações, consulte [criar a sua aplicação Central do Azure IoT](howto-create-application.md).
1. Uma máquina de desenvolvimento com [Node.js](https://nodejs.org/) versão 4.0.0 ou posterior instalado. Pode executar `node --version` na linha de comandos para verificar a sua versão. O Node.js está disponível para uma grande variedade de sistemas operativos.

Na aplicação do Azure IoT Central, é necessário um modelo de dispositivo com as seguintes medidas e as propriedades de dispositivo definidas:

### <a name="telemetry-measurements"></a>Medidas de telemetria

Adicionar a telemetria no seguinte o **medidas** página:

| Nome a Apresentar | Nome do Campo  | Unidades | Mín. | Máx. | Casas decimais |
| ------------ | ----------- | ----- | --- | --- | -------------- |
| Temperatura  | temperatura | F     | 60  | 110 | 0              |
| Humidade     | humidade    | %     | 0   | 100 | 0              |
| Pressão     | pressure    | kPa   | 80  | 110 | 0              |

> [!NOTE]
  O tipo de dados de medição de telemetria é duplo.

Introduza os nomes de campo exatamente como mostrado na tabela para o modelo de dispositivo. Se os nomes de campo não corresponderem, a telemetria poderá ser apresentada na aplicação.

### <a name="state-measurements"></a>Medidas de estado

Adicione o seguinte estado no **medidas** página:

| Nome a Apresentar | Nome do Campo  | Valor 1 | Nome a Apresentar | Valor 2 | Nome a Apresentar |
| ------------ | ----------- | --------| ------------ | ------- | ------------ | 
| Modo da Ventoinha     | fanmode     | 1       | A executar      | 0       | Parada      |

> [!NOTE]
  O tipo de dados de medição do estado é cadeia.

Introduza os nomes de campo exatamente como mostrado na tabela para o modelo de dispositivo. Se os nomes de campo não corresponderem, o estado de não pode ser apresentado na aplicação.

### <a name="event-measurements"></a>Medidas de eventos

Adicione o seguinte evento no **medidas** página:

| Nome a Apresentar | Nome do Campo  | Gravidade |
| ------------ | ----------- | -------- |
| Sobreaquecimento  | overheat    | Erro    |

> [!NOTE]
  O tipo de dados da medida evento é cadeia.

### <a name="device-properties"></a>Propriedades do dispositivo

Adicione as seguintes propriedades do dispositivo no **página de propriedades**:

| Nome a Apresentar        | Nome do Campo        | Tipo de dados |
| ------------------- | ----------------- | --------- |
| Número de série       | serialNumber      | texto      |
| Fabricante do dispositivo | fabricante      | texto      |

Introduza os nomes de campo exatamente como mostrado na tabela para o modelo de dispositivo. Se os nomes de campo não corresponderem, a aplicação não é possível mostrar o valor da propriedade.

### <a name="settings"></a>Definições

Adicione o seguinte **número** definições no **página de definições**:

| Nome a Apresentar    | Nome do Campo     | Unidades | Casas decimais | Mín. | Máx.  | Inicial |
| --------------- | -------------- | ----- | -------- | --- | ---- | ------- |
| Ventoinha velocidade       | fanSpeed       | rpm   | 0        | 0   | 3000 | 0       |
| Definir Temperatura | setTemperature | F     | 0        | 20  | 200  | 80      |

Introduza o nome do campo exatamente como mostrado na tabela para o modelo de dispositivo. Se os nomes de campo não corresponderem, o dispositivo não consegue receber o valor da definição.

### <a name="add-a-real-device"></a>Adicionar um dispositivo real

Na aplicação do Azure IoT Central, adicione um dispositivo real do modelo de dispositivo, pode criar e anote a cadeia de ligação do dispositivo. Para obter mais informações, consulte [adicionar um dispositivo real à sua aplicação do Azure IoT Central](tutorial-add-device.md)

## <a name="create-a-nodejs-application"></a>Criar uma aplicação Node.js

Os passos seguintes mostram como criar uma aplicação cliente que implementa o dispositivo real adicionados à aplicação.

1. Crie uma pasta denominada `connected-air-conditioner-adv` no computador. Navegue para essa pasta no seu ambiente de linha de comandos.

1. Para inicializar o projeto de Node.js, execute os seguintes comandos:

    ```cmd/sh
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Crie um ficheiro chamado **connectedAirConditionerAdv.js** no `connected-air-conditioner-adv` pasta.

1. Adicione o seguinte `require` as instruções no início do **connectedAirConditionerAdv.js** ficheiro:

    ```javascript
    "use strict";

    // Use the Azure IoT device SDK for devices that connect to Azure IoT Central.
    var clientFromConnectionString = require('azure-iot-device-mqtt').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    ```

1. Adicione as seguintes declarações de variáveis ao ficheiro:

    ```javascript
    var connectionString = '{your device connection string}';
    var targetTemperature = 0;
    var client = clientFromConnectionString(connectionString);
    ```

    Atualizar o marcador de posição `{your device connection string}` com a cadeia de ligação do dispositivo. Copiar este valor da página de detalhes de ligação quando adicionou o seu dispositivo real. Neste exemplo, vamos inicializar `targetTemperature` para zero, pode, opcionalmente, efetuar a leitura atual do dispositivo ou valor do dispositivo duplo. 

1. Para enviar telemetria, estado e eventos medidas para a aplicação do Azure IoT Central, adicione a seguinte função para o ficheiro:

    ```javascript
    // Send device measurements.
    function sendTelemetry() {
      var temperature = targetTemperature + (Math.random() * 15);
      var humidity = 70 + (Math.random() * 10);
      var pressure = 90 + (Math.random() * 5);
      var fanmode = 0;
      var data = JSON.stringify({ 
        temperature: temperature, 
        humidity: humidity, 
        pressure: pressure,
        fanmode: (temperature > 25) ? "1" : "0",
        overheat: (temperature > 35) ? "ER123" : undefined });
      var message = new Message(data);
      client.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
        (err ? `; error: ${err.toString()}` : '') +
        (res ? `; status: ${res.constructor.name}` : '')));
    }
    ```

    1. Para enviar as propriedades do dispositivo para a aplicação do Azure IoT Central, adicione a seguinte função para o ficheiro:

    ```javascript
    // Send device properties.
    function sendDeviceProperties(twin) {
      var properties = {
        serialNumber: '123-ABC',
        manufacturer: 'Contoso'
      };
      twin.properties.reported.update(properties, (err) => console.log(`Sent device properties; ` +
        (err ? `error: ${err.toString()}` : `status: success`)));
    }
    ```

1. Para definir as definições do que seu dispositivo responde a, adicione a seguinte definição:

    ```javascript
    // Add any settings your device supports,
    // mapped to a function that is called when the setting is changed.
    var settings = {
      'fanSpeed': (newValue, callback) => {
          // Simulate it taking 1 second to set the fan speed.
          setTimeout(() => {
            callback(newValue, 'completed');
          }, 1000);
      },
      'setTemperature': (newValue, callback) => {
        // Simulate the temperature setting taking two steps.
        setTimeout(() => {
          targetTemperature = targetTemperature + (newValue - targetTemperature) / 2;
          callback(targetTemperature, 'pending');
          setTimeout(() => {
            targetTemperature = newValue;
            callback(targetTemperature, 'completed');
          }, 5000);
        }, 5000);
      }
    };
    ```

1. Para processar as definições atualizadas da sua aplicação do Azure IoT Central, adicione o seguinte ao ficheiro:

    ```javascript
    // Handle settings changes that come from Azure IoT Central via the device twin.
    function handleSettings(twin) {
      twin.on('properties.desired', function (desiredChange) {
        for (let setting in desiredChange) {
          if (settings[setting]) {
            console.log(`Received setting: ${setting}: ${desiredChange[setting].value}`);
            settings[setting](desiredChange[setting].value, (newValue, status, message) => {
              var patch = {
                [setting]: {
                  value: newValue,
                  status: status,
                  desiredVersion: desiredChange.$version,
                  message: message
                }
              }
              twin.properties.reported.update(patch, (err) => console.log(`Sent setting update for ${setting}; ` +
                (err ? `error: ${err.toString()}` : `status: success`)));
            });
          }
        }
      });
    }
    ```

1. Adicione o seguinte para concluir a ligação ao Azure IoT Central e ligue as funções no código de cliente:

    ```javascript
    // Handle device connection to Azure IoT Central.
    var connectCallback = (err) => {
      if (err) {
        console.log(`Device could not connect to Azure IoT Central: ${err.toString()}`);
      } else {
        console.log('Device successfully connected to Azure IoT Central');

        // Send telemetry measurements to Azure IoT Central every 1 second.
        setInterval(sendTelemetry, 1000);

        // Get device twin from Azure IoT Central.
        client.getTwin((err, twin) => {
          if (err) {
            console.log(`Error getting device twin: ${err.toString()}`);
          } else {
            // Send device properties once on device start up.
            sendDeviceProperties(twin);
            // Apply device settings and handle changes to device settings.
            handleSettings(twin);
          }
        });
      }
    };

    // Start the device (connect it to Azure IoT Central).
    client.open(connectCallback);
    ```

## <a name="run-your-nodejs-application"></a>Execute a sua aplicação Node.js

Execute o seguinte comando no seu ambiente de linha de comandos:

```cmd/sh
node connectedAirConditionerAdv.js
```

Como um operador na sua aplicação do Azure IoT Central, para o seu dispositivo real pode:

* Ver a telemetria no **medidas** página:

    ![Ver telemetria](media/howto-connect-nodejs/viewtelemetry.png)

* Ver os valores de propriedade do dispositivo enviados do seu dispositivo a **propriedades** página.

    ![Ver propriedades do dispositivo](media/howto-connect-nodejs/viewproperties.png)

* Definir a temperatura ventoinha velocidade e o destino do **definições** página.

    ![Conjunto ventoinha velocidade](media/howto-connect-nodejs/setfanspeed.png)

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como ligar um cliente genérico do Node.js à sua aplicação do Azure IoT Central, Eis os passos sugeridos:
* [Preparar e ligar um Raspberry Pi](howto-connect-raspberry-pi-python.md)
<!-- Next how-tos in the sequence -->
