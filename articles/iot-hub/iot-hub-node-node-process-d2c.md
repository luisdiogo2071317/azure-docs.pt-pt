---
title: "Encaminhamento de mensagens com o IoT Hub do Azure (nó) | Microsoft Docs"
description: "Como processar mensagens de dispositivo para a nuvem do IoT Hub do Azure utilizando regras de encaminhamento e os pontos finais personalizados para emitir mensagens a outros serviços de back-end."
services: iot-hub
documentationcenter: node
author: msebolt
manager: timlt
editor: 
ms.assetid: bd9af5f9-a740-4780-a2a6-8c0e2752cf48
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/17/2017
ms.author: v-masebo
ms.openlocfilehash: e5d57e087e5f4dc1e0abf112001218aa7390a4f7
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="routing-messages-with-iot-hub-node"></a>Encaminhamento de mensagens com o IoT Hub (nó)

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Este tutorial baseia-se a [introdução ao IoT Hub] tutorial.  Tutorial:

* Mostra como utilizar as regras de encaminhamento para emitir mensagens do dispositivo para nuvem de uma forma fácil, baseadas na configuração.
* Ilustra a isolar mensagens interativas que requerem ação imediata da solução de back-end para processamento adicional.  Por exemplo, um dispositivo pode enviar uma mensagem de alarme que aciona a inserir um pedido de um sistema CRM.  Em contrapartida, as mensagens de ponto de dados, tais como a telemetria de temperatura, feed para um motor de análise.

No final deste tutorial, execute três aplicações de consola do Node.js:

* **SimulatedDevice.js**, uma versão modificada da aplicação criada no [introdução ao IoT Hub] tutorial, envia mensagens do dispositivo para a nuvem de ponto de dados de todas as mensagens de dispositivo para a nuvem segundo e interativa por aleatório intervalo. Esta aplicação utiliza o protocolo MQTT para comunicar com IoT Hub.
* **ReadDeviceToCloudMessages.js** apresenta a telemetria enviada pela sua aplicação de dispositivo.
* **ReadCriticalQueue.js** remove as mensagens de evento crítico da fila de barramento de serviço ligado ao IoT hub.

> [!NOTE]
> IoT Hub tem suporte SDK para várias plataformas de dispositivos e idiomas, incluindo C, Java e JavaScript. Para obter instruções sobre como substituir o dispositivo neste tutorial com um dispositivo físico e como ligar dispositivos para um IoT Hub, consulte o [Centro de programadores do IoT do Azure].

Para concluir este tutorial, precisa do seguinte:

* Uma versão completa do trabalho do [introdução ao IoT Hub] tutorial.
* Versão 4.0.x ou posterior do Node.js.
* Uma conta ativa do Azure. (Se não tiver uma conta, pode criar uma [conta gratuita][lnk-free-trial] em apenas alguns minutos.)

Também recomendamos ler sobre [Storage do Azure] e [Service Bus do Azure].

## <a name="send-interactive-messages-from-a-device-app"></a>Enviar mensagens interativas de uma aplicação de dispositivo
Nesta secção, modificar a aplicação de dispositivo que criou no [introdução ao IoT Hub] tutorial ocasionalmente enviar mensagens que necessitam de processamento de imediato.

1. Utilize um editor de texto para abrir o **simulateddevice\SimulatedDevice.js** ficheiro. Este ficheiro contém o código para o **SimulatedDevice** aplicação que criou no [introdução ao IoT Hub] tutorial.

2. Substitua o **connectCallback** função com o seguinte código:

    ```nodejs
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
    
        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
        var data, message;
        if (Math.random() > 0.7) {
            if (Math.random() > 0.5) {
                data = "This is a critical message.";
                message = new Message(data);
                message.properties.add("level", "critical");
            } else {
                data = "This is a storage message.";
                message = new Message(data);
                message.properties.add("level", "storage");
            }
            console.log("Sending message: " + message.getData());
        }
        else {
                var temperature = 20 + (Math.random() * 15);
                var humidity = 60 + (Math.random() * 20);            
                data = JSON.stringify({ deviceId: 'myFirstNodeDevice', temperature: temperature, humidity: humidity });
                message = new Message(data);
                message.properties.add('temperatureAlert', (temperature > 30) ? 'true' : 'false');
                console.log("Sending message: " + message.getData());
            }
        client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```
   
    Este método aleatoriamente adiciona a propriedade `"level": "critical"` e `"level": "storage"` às mensagens enviadas pelo dispositivo, que simula uma mensagem que necessita de uma ação imediata, o back-end da aplicação ou que têm ser armazenados permanentemente. A aplicação suporta encaminhamento de mensagens com base no corpo da mensagem.
   
   > [!NOTE]
   > Pode utilizar as propriedades da mensagem para encaminhar mensagens para vários cenários, incluindo o processamento de frio caminho, para além do exemplo de caminho frequente apresentado aqui.

2. Guarde e feche o **simulateddevice\SimulatedDevice.js** ficheiro.

    > [!NOTE]
    > Recomendamos vivamente que implementar uma política de repetição como término exponencial, como sugerido no artigo do MSDN [processamento de erros transitórios].

## <a name="add-service-bus-queue-to-your-iot-hub-and-route-messages-to-it"></a>Adicionar a fila do Service Bus ao seu IoT hub e rota mensagens ao mesmo

Nesta secção, criar uma fila do Service Bus, ligue-o ao seu IoT hub e configurar o seu IoT hub para enviar mensagens para a fila com base na presença de uma propriedade na mensagem. Para obter mais informações sobre como processar mensagens de filas do Service Bus, consulte [introdução às filas][lnk-sb-queues-node].

1. Criar uma fila do Service Bus, conforme descrito em [introdução às filas][lnk-sb-queues-node]. Tome nota do nome do espaço de nomes e a fila.

2. No portal do Azure, abra o seu IoT hub e clique em **pontos finais**.

    ![Pontos finais do IoT hub][30]

3. No **pontos finais** painel, clique em **adicionar** na parte superior para adicionar uma fila ao seu IoT hub. O ponto final de nome **CriticalQueue** e utilizar listas pendentes para selecionar **fila do Service Bus**, o espaço de nomes de barramento de serviço em que reside a sua fila e o nome da sua fila. Quando tiver terminado, clique em **OK** na parte inferior.  

    ![Adicionar um ponto final][31]

4. Agora, clique em **rotas** no seu IoT Hub. Clique em **adicionar** na parte superior do painel para criar uma regra de encaminhamento encaminha mensagens para a fila acabou de adicionar. Selecione **mensagens do dispositivo** como origem de dados. Introduza `level="critical"` como condição e escolha **CriticalQueue** como um ponto final personalizado como o encaminhamento ponto final da regra. Clique em **guardar** na parte inferior.  

    ![Adicionar uma rota][32]

    Certifique-se a rota de contingência está definida como **ON**. Esta definição é a configuração predefinida de um hub IoT.

    ![Rota de contingência][33]

## <a name="optional-read-from-the-queue-endpoint"></a>(Opcional) Ler a partir do ponto final de fila

Nesta secção, crie uma aplicação de consola do Node.js que lê mensagens de evento crítico de barramento de serviço de IoT. Ver mais informações no [introdução às filas][lnk-sb-queues-node]. 

1. Crie uma pasta vazia com o nome `readcriticalqueue`. Na pasta `readcriticalqueue`, crie um ficheiro package.json com o comando seguinte na sua linha de comandos. Aceite todas as predefinições:

    ```cmd/sh
    npm init
    ```

2. Na sua linha de comandos a `readcriticalqueue` pasta, execute o seguinte comando para instalar o **azure** pacote:

    ```cmd/sh
    npm install azure --save
    ```

3. Com um editor de texto, crie um **ReadCriticalQueue.js** ficheiros o `readcriticalqueue` pasta.

4. Adicione o seguinte `require` as instruções no início do **ReadCriticalQueue.js** ficheiro:

    ```nodejs
    'use strict';

    var azure = require('azure');
    ```

5. Adicione a seguinte declaração de variável e substitua os valores de marcador de posição com o nome de fila e a cadeia de ligação do barramento de serviço de IoT:

    ```nodejs
    var connectionString = '{iotservicebus connection string}';
    var queueName = '{iotservice bus queue name}';
    ```

6. Adicione as seguintes duas funções que imprimem o resultado para a consola:

    ```nodejs
    var serviceBusService = azure.createServiceBusService(connectionString);

    setInterval(function() {serviceBusService.receiveQueueMessage(queueName, function(error, receivedMessage) {
        if(!error){
            // Message received and deleted
            console.log(receivedMessage);
        } else { console.log(error); }
        });
    }, 3000);
    ```

7. Guarde e feche o **ReadCriticalQueue.js** ficheiro.

## <a name="run-the-applications"></a>Executar as aplicações

Agora, está pronto para executar as aplicações de três.

1. Para executar o **ReadDeviceToCloudMessages.js** aplicação, numa linha de comandos ou a shell, navegue para a pasta de readdevicetocloudmessages e execute o seguinte comando:

   ```cmd/sh
   node ReadDeviceToCloudMessages.js
   ```

   ![Executar read-d2c-messages][readd2c]

2. Para executar o **ReadCriticalQueue.js** aplicação, numa linha de comandos ou a shell, navegue para a pasta de readcriticalqueue e execute o seguinte comando:

   ```cmd/sh
   node ReadCriticalQueue.js
   ```
   
   ![Executar leitura mensagens crítico][readqueue]

3. Para executar o **SimulatedDevice.js** aplicação, numa linha de comandos ou a shell, navegue para a pasta de simulateddevice e execute o seguinte comando:

   ```cmd/sh
   node SimulatedDevice.js
   ```
   
   ![Executar simulated-device][simulateddevice]

## <a name="optional-add-storage-container-to-your-iot-hub-and-route-messages-to-it"></a>(Opcional) Adicionar o contentor de armazenamento ao seu IoT hub e rota mensagens ao mesmo

Nesta secção, criar uma conta de armazenamento, ligue-o ao seu IoT hub e configurar o seu IoT hub para enviar mensagens para a conta com base na presença de uma propriedade na mensagem. Para obter mais informações sobre como gerir o armazenamento, consulte [introdução ao Storage do Azure][Storage do Azure].

 > [!NOTE]
   > Se não está limitado a um **Endpoint**, pode configurar o **StorageContainer** para além de **CriticalQueue** e executar ambos os simulatneously.

1. Criar uma conta de armazenamento, conforme descrito em [documentação do Storage do Azure][lnk-storage]. Anote o nome da conta.

2. No portal do Azure, abra o seu IoT hub e clique em **pontos finais**.

3. No **pontos finais** painel, selecione o **CriticalQueue** ponto final e clique em **eliminar**. Clique em **Sim**e, em seguida, clique em **adicionar**. O ponto final de nome **StorageContainer** e utilizar listas pendentes para selecionar **contentor de armazenamento do Azure**e criar um **conta de armazenamento** e um **armazenamento contentor**.  Tome nota dos nomes.  Quando tiver terminado, clique em **OK** na parte inferior. 

 > [!NOTE]
   > Se não está limitado a um **Endpoint**, não terá de eliminar o **CriticalQueue**.

4. Clique em **rotas** no seu IoT Hub. Clique em **adicionar** na parte superior do painel para criar uma regra de encaminhamento encaminha mensagens para a fila acabou de adicionar. Selecione **mensagens do dispositivo** como origem de dados. Introduza `level="storage"` como condição e escolha **StorageContainer** como um ponto final personalizado como o encaminhamento ponto final da regra. Clique em **guardar** na parte inferior.  

    Certifique-se a rota de contingência está definida como **ON**. Esta definição é a configuração predefinida de um hub IoT.

1. Certifique-se a aplicação anterior **SimulatedDevice.js** ainda está em execução. 

1. No Portal do Azure, aceda à sua conta do storage em **serviço Blob**, clique em **procurar blobs...** .  Selecione o contentor, navegue para e clique no ficheiro JSON e clique em **transferir** para ver os dados.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu a fiável emitir mensagens do dispositivo para nuvem utilizando a funcionalidade de encaminhamento de mensagens do IoT Hub.

O [como enviar mensagens da nuvem para o dispositivo com o IoT Hub] [ lnk-c2d] mostra como enviar mensagens para os dispositivos da sua solução de back-end.

Para ver os exemplos de soluções ponto-a-ponto completas que utilizam o IoT Hub, consulte [Azure IoT Suite][lnk-suite].

Para obter mais informações sobre como desenvolver soluções de IoT hub, consulte o [guia para programadores do IoT Hub].

Para saber mais sobre o encaminhamento de mensagens no IoT Hub, veja [enviar e receber mensagens com o IoT Hub][lnk-devguide-messaging].

<!-- Images. -->
<!-- TODO: UPDATE PICTURES -->
[simulateddevice]: ./media/iot-hub-node-node-process-d2c/runsimulateddevice.png
[readd2c]: ./media/iot-hub-node-node-process-d2c/runprocessinteractive.png
[readqueue]: ./media/iot-hub-node-node-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-node-node-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-node-node-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-node-node-process-d2c/route-creation.png
[33]: ./media/iot-hub-node-node-process-d2c/fallback-route.png

<!-- Links -->

[lnk-sb-queues-node]: ../service-bus-messaging/service-bus-nodejs-how-to-use-queues.md

[Storage do Azure]: https://azure.microsoft.com/documentation/services/storage/
[Service Bus do Azure]: https://azure.microsoft.com/documentation/services/service-bus/

[guia para programadores do IoT Hub]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[introdução ao IoT Hub]: iot-hub-node-node-getstarted.md
[Centro de programadores do IoT do Azure]: https://azure.microsoft.com/develop/iot
[processamento de erros transitórios]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Processamento de falhas transitórias]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-c2d]: iot-hub-node-node-c2d.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-free-trial]: https://azure.microsoft.com/free/
[lnk-storage]: https://docs.microsoft.com/azure/storage/