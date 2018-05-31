---
title: Adicionar um dispositivo real a uma aplicação do Azure IoT Central | Microsoft Docs
description: Como operador, adicione um dispositivo real à aplicação do Azure IoT Central.
services: iot-central
author: sandeeppujar
ms.author: sandeepu
ms.date: 04/16/2018
ms.topic: tutorial
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: f95d9ec8cf22c287169a8de077ff9eb5907a8e84
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34201936"
---
# <a name="add-a-real-device-to-your-azure-iot-central-application"></a>Adicionar um dispositivo real à aplicação do Azure IoT Central

Este tutorial mostra como adicionar e configurar um dispositivo real à aplicação do Microsoft Azure IoT Central.

Este tutorial é constituído por duas partes:

1. Primeiro, como operador, vai aprender a adicionar e configurar um dispositivo real na aplicação do Azure IoT Central. No final desta parte, pode obter uma cadeia de ligação para utilizar na segunda parte.
1. Em seguida, como programador de dispositivos, vai obter informações sobre o código no dispositivo real. Vai adicionar a cadeia de ligação da primeira parte ao código de exemplo.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Adicionar um novo dispositivo real
> * Configurar o novo dispositivo
> * Obter a cadeia de ligação para o dispositivo real a partir da aplicação
> * Compreender a forma como o código de cliente mapeia para a aplicação
> * Configurar o código de cliente para o dispositivo real

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, o construtor deve concluir, pelo menos, o primeiro tutorial do construtor para criar a aplicação do Azure IoT Central:

* [Definir um novo tipo de dispositivo](tutorial-define-device-type.md) (obrigatório)
* [Configurar regras e ações para o seu dispositivo](tutorial-configure-rules.md) (Opcional)
* [Personalizar as vistas do operador](tutorial-customize-operator.md) (Opcional)

## <a name="add-a-real-device"></a>Adicionar um dispositivo real

Para adicionar um dispositivo real à sua aplicação, utilize o modelo de dispositivo **Ar Condicionado Ligado** que criou no tutorial [Definir um novo tipo de dispositivo](tutorial-define-device-type.md).

1. Para adicionar um novo dispositivo como operador, selecione **Device Explorer** no menu de navegação à esquerda:

    ![Página do Device Explorer que mostra o ar condicionado ligado](media/tutorial-add-device/explorer.png)

    O **Device Explorer** mostra o modelos de dispositivo **Ar Condicionado Ligado** e o dispositivo simulado que foi criado automaticamente quando o construtor criou o modelo de dispositivo.

1. Para começar a ligar um dispositivo de ar condicionado ligado real, selecione **Novo** e, em seguida, **Real**:

    ![Começar a adicionar um novo dispositivo de ar condicionado ligado real](media/tutorial-add-device/newreal.png)

1. Opcionalmente, pode mudar o nome do novo dispositivo ao escolher o nome do dispositivo e ao editar o valor:

    ![Mudar o nome do dispositivo](media/tutorial-add-device/rename.png)

## <a name="configure-a-real-device"></a>Configurar um dispositivo real

O dispositivo real é criado a partir do modelo de dispositivo **Ar Condicionado Ligado**. Como construtor, pode utilizar as **Definições** para configurar o dispositivo e definir valores de propriedade para registar informações sobre o dispositivo.

1. Na página **Definições**, tenha em atenção que o estado da definição **Definir Temperatura** é **sem atualização**. Permanece neste estado até o dispositivo real ligar e reconhece que agiu na definição:

    ![As definições mostram a sincronização](media/tutorial-add-device/settingssyncing.png)

1. Na página **Propriedades** do novo dispositivo de ar condicionado ligado real, defina **Número de Série** para **rcac0010** e **Versão de firmware** para 9.75. Em seguida, escolha **Guardar**:

    ![Definir as propriedades do dispositivo real](media/tutorial-add-device/setproperties.png)

1. Como construtor, pode ver as páginas **Medidas**, **Regras** e **Dashboard** para do dispositivo real.

## <a name="get-connection-string-for-real-device-from-application"></a>Obter a cadeia de ligação para o dispositivo real a partir da aplicação

Um programador de dispositivos tem de incorporar a _cadeia de ligação_ do dispositivo real no código executado no dispositivo. A cadeia de ligação permite ao dispositivo ligar de forma segura à sua aplicação do Azure IoT Central. Todas as instâncias do dispositivo têm uma cadeia de ligação exclusiva. Os passos seguintes mostram como localizar a cadeia de ligação de uma instância de dispositivo na sua aplicação:

1. No ecrã **Dispositivo** do dispositivo de ar condicionado ligado real, selecione **Ligar este dispositivo**:

    ![Página do dispositivo que mostra a ligação para informações de ligação](media/tutorial-add-device/connectionlink.png)

1. Na página **Ligar**, copie a **Cadeia de ligação principal** e guarde-a. Pode utilizar este valor na segunda metade deste tutorial. Um programador de dispositivos utiliza este valor na aplicação cliente executada no dispositivo:

    ![Valores da cadeia de ligação](media/tutorial-add-device/connectionstring.png)

## <a name="prepare-the-client-code"></a>Preparar o código de cliente

O código de exemplo neste artigo está escrito em [Node.js](https://nodejs.org/) e mostra código suficiente para:

* Ligar como dispositivo à sua aplicação do Azure IoT Central.
* Enviar telemetria de temperatura como um dispositivo de ar condicionado ligado.
* Responder a um operador que utiliza a definição **Definir Temperatura**.

Os artigos explicativos referenciados na secção [Passos Seguintes](#next-steps) fornecem exemplos mais completos e mostram a utilização de outras linguagens de programação. Para obter mais informações sobre a forma como os dispositivos se ligam ao Azure IoT Central, veja o artigo [Conectividade de dispositivos](concepts-connectivity.md).

Os passos seguintes mostram como preparar o exemplo em [Node.js](https://nodejs.org/):

1. Instale a versão 4.0.x ou posterior do [Node.js](https://nodejs.org/) no computador. O Node.js está disponível para uma grande variedade de sistemas operativos.

1. Crie uma pasta denominada `connectedairconditioner` no computador.

1. No ambiente de linha de comandos, navegue para a pasta `connectedairconditioner` que criou.

1. Para inicializar o projeto em Node.js, execute o seguinte comando ao aceitar todas as predefinições:

    ```cmd/sh
    npm init
    ```

1. Para instalar os pacotes necessários, execute o seguinte comando:

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Com um editor de texto, crie um ficheiro denominado **ConnectedAirConditioner.js** na pasta `connectedairconditioner`.

1. Adicione as seguintes instruções `require` no início do ficheiro **ConnectedAirConditioner.js**:

    ```javascript
    'use strict';

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

    > [!NOTE]
    > Vai atualizar o marcador de posição `{your device connection string}` num passo posterior.

1. Guarde as alterações efetuadas até ao momento, mas mantenha o ficheiro aberto.

## <a name="understand-how-client-code-maps-to-the-application"></a>Compreender a forma como o código de cliente mapeia para a aplicação

Na secção anterior, criou um projeto em Node.js para uma aplicação que liga à sua aplicação do Azure IoT Central. Nesta secção, vai adicionar o código para:

* Ligar à sua aplicação do Azure IoT Central.
* Enviar telemetria para a sua aplicação do Azure IoT Central.
* Receber definições da sua aplicação do Azure IoT Central.

1. Para enviar telemetria de temperatura para a sua aplicação do Azure IoT Central, adicione o seguinte código ao ficheiro **ConnectedAirConditioner.js**:

    ```javascript
    // Send device telemetry.
    function sendTelemetry() {
      var temperature = targetTemperature + (Math.random() * 15);
      var data = JSON.stringify({ temperature: temperature });
      var message = new Message(data);
      client.sendEvent(message, (err, res) => console.log(`Sent message: ${message.getData()}` +
        (err ? `; error: ${err.toString()}` : '') +
        (res ? `; status: ${res.constructor.name}` : '')));
    }
    ```

    O nome do campo no JSON que enviar tem de corresponder ao nome do campo especificado para a telemetria de temperatura no modelo de dispositivo. Neste exemplo, o nome do campo é **temperature**.

1. Para estipular as definições suportadas pelo dispositivo, como **setTemperature**, adicione a seguinte definição:

    ```javascript
    // Add any settings your device supports
    // mapped to a function that is called when the setting is changed.
    var settings = {
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

1. Para processar as definições enviadas a partir do Azure IoT Central, adicione a seguinte função que localiza e executa o código de dispositivo adequado:

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

    Esta função:

    * Observa o Azure IoT Central a enviar uma propriedade desejada.
    * Localiza a função adequada à chamada para processar a alteração da definição.
    * Envia uma confirmação para a aplicação do Azure IoT Central.

1. Adicione o seguinte código para concluir a ligação ao Azure IoT Central e ligar as funções no código de cliente:

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
            // Apply device settings and handle changes to device settings.
            handleSettings(twin);
          }
        });
      }
    };

    client.open(connectCallback);
    ```

1. Guarde as alterações efetuadas até ao momento, mas mantenha o ficheiro aberto.

## <a name="configure-client-code-for-real-device"></a>Configurar o código de cliente para o dispositivo real

<!-- Add the connection string to the sample code, build, and run -->
Para configurar o código de cliente para ligar à sua aplicação do Azure IoT Central, tem de adicionar a cadeia de ligação para o dispositivo real que anotou anteriormente no tutorial.

1. No ficheiro **ConnectedAirConditioner.js**, localize a seguinte linha de código:

    ```javascript
    var connectionString = '{your device connection string}';
    ```

1. Substitua `{your device connection string}` pela cadeia de ligação do dispositivo real. Anotou a cadeia de ligação no final da secção "Obter a cadeia de ligação para o dispositivo real a partir da aplicação".

1. Guarde as alterações no ficheiro **ConnectedAirConditioner.js**.

1. Para executar o exemplo, introduza o seguinte comando no ambiente de linha de comandos:

    ```cmd/sh
    node ConnectedAirConditioner.js
    ```

    > [!NOTE]
    > Certifique-se de que está na pasta `connectedairconditioner` quando executar este comando.

1. A aplicação imprime o resultado na consola:

    ![Resultado da aplicação cliente](media/tutorial-add-device/output.png)

1. Após cerca de 30 segundos, é apresentada a telemetria na página do dispositivo **Medidas**:

    ![Telemetria real](media/tutorial-add-device/realtelemetry.png)

1. Na página **Definições**, pode ver que a definição está agora sincronizada. Quando o dispositivo foi ligado pela primeira vez, recebeu o valor da definição e confirmou a alteração:

    ![Definição sincronizada](media/tutorial-add-device/settingsynced.png)

1. Na página **Definições**, defina a temperatura do dispositivo para **95** e selecione **Atualizar dispositivo**. A aplicação de exemplo recebe e processa esta alteração:

    ![Receber e processar a definição](media/tutorial-add-device/receivesetting.png)

    > [!NOTE]
    > Existem duas mensagens de "atualização da definição". Uma quando é enviado o estado `pending` e outra quando é enviado o estado `completed`.

1. Na página **Medidas**, pode ver que o dispositivo está a enviar valores de temperatura superiores:

    ![A telemetria de temperatura é agora superior](media/tutorial-add-device/highertemperature.png)

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="nextstepaction"]
> * Adicionar um novo dispositivo real
> * Configurar o novo dispositivo
> * Obter a cadeia de ligação para o dispositivo real a partir da aplicação
> * Compreender a forma como o código de cliente mapeia para a aplicação
> * Configurar o código de cliente para o dispositivo real

Agora que ligou um dispositivo real à sua aplicação do Azure IoT Central, eis os passos seguintes sugeridos:

Como operador, vai aprender a:

* [Gerir os seus dispositivos](howto-manage-devices.md)
* [Utilizar conjuntos de dispositivos](howto-use-device-sets.md)
* [Criar análise personalizada](howto-create-analytics.md)

Como programador de dispositivos, vai aprender a:

* [Preparar e ligar um DevKit](howto-connect-devkit.md)
* [Preparar e ligar um Raspberry Pi](howto-connect-raspberry-pi-python.md)
* [Ligar um cliente Node.js genérico à aplicação do Azure IoT Central](howto-connect-nodejs.md)
