---
title: Mensagens da nuvem para dispositivo IoT hub do Azure (Python) | Microsoft Docs
description: "Como enviar mensagens da nuvem para o dispositivo a um dispositivo de um hub IoT do Azure com os SDKs IoT do Azure para Python. Modificar uma aplicação de dispositivo simulado para receber mensagens da nuvem para o dispositivo e modificar uma aplicação de back-end para enviar as mensagens da nuvem para o dispositivo."
services: iot-hub
documentationcenter: python
author: msebolt
manager: timlt
editor: 
ms.assetid: 3ca8a78f-ade2-46e8-8a49-d5d599cdf1f1
ms.service: iot-hub
ms.devlang: javascript
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/22/2018
ms.author: v-masebo
ms.openlocfilehash: 592e0cd858d16715f95955194eca4217d9914b05
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="send-cloud-to-device-messages-with-iot-hub-python"></a>Enviar mensagens da nuvem para o dispositivo com o IoT Hub (Python)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]


## <a name="introduction"></a>Introdução
IoT Hub do Azure é um serviço completamente gerido que o ajuda a ativar fiáveis e seguras comunicações bidirecionais entre milhões de dispositivos e uma solução de back-end. O [introdução ao IoT Hub] tutorial mostra como criar um hub IoT, aprovisionar uma identidade de dispositivo no mesmo e código de uma aplicação de dispositivo simulado que envia mensagens do dispositivo para a nuvem.

Este tutorial baseia-se [introdução ao IoT Hub]. Mostra-lhe como para:

* Da sua solução de back-end, envie mensagens de nuvem para o dispositivo para um único dispositivo através do IoT Hub.
* Receba mensagens da nuvem para o dispositivo num dispositivo.
* Da sua solução de back-end, pedir confirmação de entrega (*comentários*) para mensagens enviadas para um dispositivo a partir do IoT Hub.

Pode encontrar mais informações sobre mensagens da nuvem para o dispositivo no [guia para programadores do IoT Hub][IoT Hub developer guide - C2D].

No final deste tutorial, executa duas aplicações de consola do Python:

* **SimulatedDevice.py**, uma versão modificada da aplicação criada na [introdução ao IoT Hub], que liga ao seu IoT hub e recebe mensagens da nuvem para o dispositivo.
* **SendCloudToDeviceMessage.py**, que envia uma mensagem da nuvem para o dispositivo para a aplicação de dispositivo simulado através do IoT Hub e, em seguida, receber a confirmação de entrega.

> [!NOTE]
> IoT Hub tem suporte SDK para várias plataformas de dispositivos e idiomas (incluindo C, Java e Javascript) através de SDKs do dispositivo IoT do Azure. Para obter instruções passo a passo sobre como ligar o seu dispositivo para código neste tutorial e, geralmente, IoT Hub do Azure, consulte o [Centro de programadores do IoT do Azure].
> 

Para concluir este tutorial, precisa do seguinte:

* [Python 2.x ou 3.x][lnk-python-download]. Certifique-se de que utiliza a instalação de 32 ou 64 bits, conforme exigido pela sua configuração. Quando lhe for pedido durante a instalação, confirme que adiciona Python à variável de ambiente específica da sua plataforma. Se estiver a utilizar Python 2.x, poderá ter de [instalar ou atualizar o *pip*, o sistema de gestão de pacotes de Python][lnk-install-pip].
* Se estiver a utilizar o SO Windows, o [pacote redistribuível Visual C++][lnk-visual-c-redist], para permitir a utilização de DLLs nativas de Python.
* Uma conta ativa do Azure. (Se não tiver uma conta, pode criar uma [conta gratuita][lnk-free-trial] em apenas alguns minutos.)

> [!NOTE]
> Os pacotes *pip* para `azure-iothub-service-client` e `azure-iothub-device-client` estão atualmente disponíveis apenas para o SO Windows. SO de Linux/Mac, consulte as secções de Linux e Mac específicas de SO em post de [lnk-python-devbox] [preparar o ambiente de desenvolvimento para o Python].
> 


## <a name="receive-messages-in-the-simulated-device-app"></a>Receber mensagens na aplicação do dispositivo simulado
Nesta secção, vai criar uma aplicação de consola do Python para simular o dispositivo e receber mensagens da nuvem para dispositivos do IoT hub.

1. Com um editor de texto, crie um **SimulatedDevice.py** ficheiro.

1. Adicione o seguinte `import` instruções e variáveis no início do **SimulatedDevice.py** ficheiro:
   
    ```python
    import time
    import sys
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubMessage, IoTHubMessageDispositionResult, IoTHubError

    RECEIVE_CONTEXT = 0
    WAIT_COUNT = 10
    RECEIVED_COUNT = 0
    RECEIVE_CALLBACKS = 0
    ```

1. Adicione o seguinte código para **SimulatedDevice.py** ficheiro. Substitua o valor do marcador de posição de "{deviceConnectionString}" com a cadeia de ligação do dispositivo para o dispositivo que criou no [introdução ao IoT Hub] tutorial:
   
    ```python
    # choose AMQP or AMQP_WS as transport protocol
    PROTOCOL = IoTHubTransportProvider.AMQP
    CONNECTION_STRING = "{deviceConnectionString}"
    ```

1. Adicione a seguinte função para imprimir mensagens recebidas para a consola:
   
    ```python
    def receive_message_callback(message, counter):
        global RECEIVE_CALLBACKS
        message_buffer = message.get_bytearray()
        size = len(message_buffer)
        print ( "Received Message [%d]:" % counter )
        print ( "    Data: <<<%s>>> & Size=%d" % (message_buffer[:size].decode('utf-8'), size) )
        map_properties = message.properties()
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        counter += 1
        RECEIVE_CALLBACKS += 1
        print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
        return IoTHubMessageDispositionResult.ACCEPTED

    def iothub_client_init():
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

        client.set_message_callback(receive_message_callback, RECEIVE_CONTEXT)

        return client

    def print_last_message_time(client):
        try:
            last_message = client.get_last_message_receive_time()
            print ( "Last Message: %s" % time.asctime(time.localtime(last_message)) )
            print ( "Actual time : %s" % time.asctime() )
        except IoTHubClientError as iothub_client_error:
            if iothub_client_error.args[0].result == IoTHubClientResult.INDEFINITE_TIME:
                print ( "No message received" )
            else:
                print ( iothub_client_error )
    ```

1. Adicione o seguinte código para inicializar o cliente e de espera para receber a mensagem da nuvem para o dispositivo:
   
    ```python
    def iothub_client_init():
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

        client.set_message_callback(receive_message_callback, RECEIVE_CONTEXT)

        return client

    def iothub_client_sample_run():
        try:
            client = iothub_client_init()

            while True:
                print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    status = client.get_send_status()
                    print ( "Send status: %s" % status )
                    time.sleep(10)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )

        print_last_message_time(client)
    ```

1. Adicione a seguinte função principal:
   
    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_sample_run()
    ```

1. Guarde e feche **SimulatedDevice.py** ficheiro.


## <a name="send-a-cloud-to-device-message"></a>Enviar uma mensagem da nuvem para o dispositivo
Nesta secção, crie uma aplicação de consola de Python que envia mensagens da nuvem para o dispositivo para a aplicação de dispositivo simulado. É necessário o ID de dispositivo do dispositivo adicionado no [introdução ao IoT Hub] tutorial. Também precisa da cadeia de ligação do IoT Hub para o seu hub que pode encontrar o [portal do Azure].

1. Com um editor de texto, crie um **SendCloudToDeviceMessage.py** ficheiro.

1. Adicione o seguinte `import` instruções e variáveis no início do **SendCloudToDeviceMessage.py** ficheiro:
   
    ```python
    import random
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubMessaging, IoTHubMessage, IoTHubError

    OPEN_CONTEXT = 0
    FEEDBACK_CONTEXT = 1
    MESSAGE_COUNT = 1
    AVG_WIND_SPEED = 10.0
    MSG_TXT = "{\"service client sent a message\": %.2f}"
    ```

1. Adicione o seguinte código para **SendCloudToDeviceMessage.py** ficheiro. Substitua o valor do marcador de posição de "{IoTHubConnectionString}" com a cadeia de ligação do IoT Hub hub que criou no [introdução ao IoT Hub] tutorial. Substitua o marcador de posição "{deviceId}" com o ID de dispositivo do dispositivo adicionado no [introdução ao IoT Hub] tutorial:
   
    ```python
    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"
    ```

1. Adicione a seguinte função para imprimir os resultados da operação para a consola:
   
    ```python
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

1. Adicione a seguinte função para imprimir mensagens de comentários para a consola:
   
    ```python
    def open_complete_callback(context):
        print ( 'open_complete_callback called with context: {0}'.format(context) )

    def send_complete_callback(context, messaging_result):
        context = 0
        print ( 'send_complete_callback called with context : {0}'.format(context) )
        print ( 'messagingResult : {0}'.format(messaging_result) )
    ```

1. Adicione o seguinte código para enviar uma mensagem para o seu dispositivo e processar a mensagem de comentários quando o dispositivo reconhece a mensagem da nuvem para o dispositivo:
   
    ```python
    def iothub_messaging_sample_run():
        try:
            iothub_messaging = IoTHubMessaging(CONNECTION_STRING)

            iothub_messaging.open(open_complete_callback, OPEN_CONTEXT)

            for i in range(0, MESSAGE_COUNT):
                print ( 'Sending message: {0}'.format(i) )
                msg_txt_formatted = MSG_TXT % (AVG_WIND_SPEED + (random.random() * 4 + 2))
                message = IoTHubMessage(bytearray(msg_txt_formatted, 'utf8'))

                # optional: assign ids
                message.message_id = "message_%d" % i
                message.correlation_id = "correlation_%d" % i
                # optional: assign properties
                prop_map = message.properties()
                prop_text = "PropMsg_%d" % i
                prop_map.add("Property", prop_text)

                iothub_messaging.send_async(DEVICE_ID, message, send_complete_callback, i)

            try:
                # Try Python 2.xx first
                raw_input("Press Enter to continue...\n")
            except:
                pass
                # Use Python 3.xx in the case of exception
                input("Press Enter to continue...\n")

            iothub_messaging.close()

        except IoTHubError as iothub_error:
            print ( "Unexpected error {0}" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubMessaging sample stopped" )
    ```

1. Adicione a seguinte função principal:
   
    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Service Client Messaging Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_messaging_sample_run()
    ```

1. Guarde e feche **SendCloudToDeviceMessage.py** ficheiro.


## <a name="run-the-applications"></a>Executar as aplicações
Pode agora executar as aplicações.

1. Abra uma linha de comandos e instalar o **Azure IoT Hub dispositivo SDK para Python**.

    ```
    pip install azure-iothub-device-client
    ```

1. Na linha de comandos, execute o seguinte comando para escutar mensagens da nuvem para o dispositivo:
   
    ```shell
    python SimulatedDevice.py 
    ```
   
    ![Executar a aplicação de dispositivo simulada][img-simulated-device]

1. Abra uma nova linha de comandos e instalar o **SDK do serviço do Azure IoT Hub para o Python**.

    ```
    pip install azure-iothub-service-client
    ```

1. Numa linha de comandos, execute o seguinte comando para enviar uma mensagem da nuvem para o dispositivo e aguarde que os comentários da mensagem:
   
    ```shell
    python SendCloudToDeviceMessage.py 
    ```
   
    ![Executar a aplicação para enviar o comando de nuvem para o dispositivo][img-send-command]
   
1. Tenha em atenção a mensagem recebida pelo dispositivo.

    ![Foi recebida uma mensagem][img-message-recieved]


## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, aprendeu a enviar e receber mensagens da nuvem para o dispositivo. 

Para ver os exemplos de soluções ponto-a-ponto completas que utilizam o IoT Hub, consulte [Azure IoT Suite].

Para obter mais informações sobre como desenvolver soluções de IoT hub, consulte o [guia para programadores do IoT Hub].

<!-- Images -->
[img-simulated-device]: media/iot-hub-python-python-c2d/simulated-device.png
[img-send-command]:  media/iot-hub-python-python-c2d/send-command.png
[img-message-recieved]: media/iot-hub-python-python-c2d/message-recieved.png

<!-- Links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-node-download]: https://nodejs.org/en/download/
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[introdução ao IoT Hub]: iot-hub-node-node-getstarted.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[guia para programadores do IoT Hub]: iot-hub-devguide.md
[Centro de programadores do IoT do Azure]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[portal do Azure]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
