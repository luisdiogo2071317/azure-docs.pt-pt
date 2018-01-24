---
title: "Introdução ao duplos de dispositivos do IoT Hub do Azure (Python) | Microsoft Docs"
description: "Como utilizar dispositivos duplos do IoT Hub do Azure para adicionar etiquetas e, em seguida, utilizar uma consulta do IoT Hub. Utilize os SDKs IoT do Azure para Python para implementar a aplicação de dispositivo simulada e uma aplicação de serviço que adiciona as etiquetas e executa a consulta do IoT Hub."
services: iot-hub
documentationcenter: python
author: msebolt
manager: timlt
editor: 
ms.assetid: 314c88e4-cce1-441c-b75a-d2e08e39ae7d
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/04/2017
ms.author: v-masebo
ms.openlocfilehash: 20c1eeee6ca690ddcf0b9489b88689213b79488e
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="get-started-with-device-twins-python"></a>Começar a utilizar dispositivos duplos (Python)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

No final deste tutorial, terá de duas aplicações de consola do Python:

* **AddTagsAndQuery.py**, uma aplicação de back-end do Python, que adiciona as etiquetas e a consulta dispositivos duplos.
* **ReportConnectivity.py**, uma aplicação de Python, que simula um dispositivo que liga ao seu IoT hub com a identidade de dispositivo que criou anteriormente e reporta a condição de conectividade.

> [!NOTE]
> O artigo [SDKs IoT do Azure] [ lnk-hub-sdks] fornece informações sobre os SDKs IoT do Azure que pode utilizar para criar aplicações de dispositivo e o back-end.
> 
> 

Para concluir este tutorial precisa do seguinte:

* [Python 2.x ou 3.x][lnk-python-download]. Certifique-se de que utiliza a instalação de 32 ou 64 bits, conforme exigido pela sua configuração. Quando lhe for pedido durante a instalação, confirme que adiciona Python à variável de ambiente específica da sua plataforma. Se estiver a utilizar Python 2.x, poderá ter de [instalar ou atualizar o *pip*, o sistema de gestão de pacotes de Python][lnk-install-pip].
* Se estiver a utilizar o SO Windows, o [pacote redistribuível Visual C++][lnk-visual-c-redist], para permitir a utilização de DLLs nativas de Python.
* Uma conta ativa do Azure. (Se não tiver uma conta, pode criar uma [conta gratuita][lnk-free-trial] em apenas alguns minutos.)

> [!NOTE]
> Os pacotes *pip* para `azure-iothub-service-client` e `azure-iothub-device-client` estão atualmente disponíveis apenas para o SO Windows. Para Linux/Mac OS, veja as secções específicas de SO Linux e Mac na publicação [Prepare your development environment for Python][lnk-python-devbox] (Preparar o seu ambiente de desenvolvimento para Python).
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="create-the-service-app"></a>Criar a aplicação de serviço
Nesta secção, vai criar uma aplicação de consola de Python que adiciona os metadados de localização para o dispositivo duplo associada à sua **{Id de dispositivo}**. Esta consulta, em seguida, os dispositivos duplos armazenados no hub IoT selecionar os dispositivos localizados em Redmond e, em seguida, aqueles que estão a denunciar uma ligação de rede móvel.

1. Abra uma linha de comandos e instale o **SDK do Serviço Hub IoT do Azure para Python**. Depois de instalar o SDK, feche a linha de comandos.

    ```
    pip install azure-iothub-service-client
    ```

1. Com um editor de texto, crie um novo **AddTagsAndQuery.py** ficheiro.

3. Adicione o código seguinte para importar os módulos necessários do SDK do serviço:

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceTwin, IoTHubError
    ```
2. Adicione o seguinte código, substituindo o marcador de posição para `[IoTHub Connection String]` e `[Device Id]` com a cadeia de ligação para o IoT hub e o id de dispositivo que criou nas secções anteriores.
   
    ```python
    CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "[Device Id]"

    UPDATE_JSON = "{\"properties\":{\"desired\":{\"location\":\"Redmond\"}}}"

    UPDATE_JSON_SEARCH = "\"location\":\"Redmond\""
    UPDATE_JSON_CLIENT_SEARCH = "\"connectivity\":\"cellular\""
    ```

1. Adicione o seguinte código para o **AddTagsAndQuery.py** ficheiro:
   
     ```python
    def iothub_service_sample_run():
        try:
            iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)
        
            iothub_registry_statistics = iothub_registry_manager.get_statistics()
            print ( "Total device count                       : {0}".format(iothub_registry_statistics.totalDeviceCount) )
            print ( "Enabled device count                     : {0}".format(iothub_registry_statistics.enabledDeviceCount) )
            print ( "Disabled device count                    : {0}".format(iothub_registry_statistics.disabledDeviceCount) )
            print ( "" )

            number_of_devices = iothub_registry_statistics.totalDeviceCount
            dev_list = iothub_registry_manager.get_device_list(number_of_devices)
        
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)

            for device in range(0, number_of_devices):
                if dev_list[device].deviceId == DEVICE_ID:
                    twin_info = iothub_twin_method.update_twin(dev_list[device].deviceId, UPDATE_JSON)
        
            print ( "Devices in Redmond: " )        
            for device in range(0, number_of_devices):
                twin_info = iothub_twin_method.get_twin(dev_list[device].deviceId)
         
                if twin_info.find(UPDATE_JSON_SEARCH) > -1:
                    print ( dev_list[device].deviceId )
        
            print ( "" )
        
            print ( "Devices in Redmond using cellular network: " )
            for device in range(0, number_of_devices):
                twin_info = iothub_twin_method.get_twin(dev_list[device].deviceId)
                
                if twin_info.find(UPDATE_JSON_SEARCH) > -1:
                    if twin_info.find(UPDATE_JSON_CLIENT_SEARCH) > -1:
                        print ( dev_list[device].deviceId )

        except IoTHubError as iothub_error:
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "IoTHub sample stopped" )
    ```
   
    O **registo** objeto expõe todos os métodos necessários para interagir com dispositivos duplos do serviço. O código do primeiro inicializa a **registo** objeto, em seguida, as atualizações do dispositivo duplo para **deviceId**, e, finalmente, executa duas consultas. A primeira seleciona apenas os dispositivos duplos de dispositivos localizados no **Redmond43** Jacinto e o segundo refines a consulta para selecionar apenas os dispositivos que também estão ligados através da rede celular.
   
1. Adicione o seguinte código no final do **AddTagsAndQuery.py** para implementar o **iothub_service_sample_run** função:
   
    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Device Twins Python service sample..." )

        iothub_service_sample_run()
    ```

1. Execute a aplicação com:
   
    ```cmd/sh
    python AddTagsAndQuery.py
    ```
   
    Deverá ver um dispositivo nos resultados para pedir a consulta para todos os dispositivos localizados no **Redmond43** e nenhum para a consulta que restringe os resultados para dispositivos que utilizam uma rede celular.
   
    ![primeira consulta][1]

Na secção seguinte, criar uma aplicação de dispositivo que reporta as informações de conectividade e altera o resultado da consulta na secção anterior.

## <a name="create-the-device-app"></a>Criar a aplicação de dispositivo
Nesta secção, vai criar uma aplicação de consola de Python que liga ao seu hub como seu **{Id de dispositivo}**e, em seguida, as atualizações do dispositivo duplo do comunicadas propriedades para conter as informações que esteja ligada através de uma rede celular.

1. Abra uma linha de comandos e instale o **SDK do Serviço Hub IoT do Azure para Python**. Depois de instalar o SDK, feche a linha de comandos.

    ```
    pip install azure-iothub-device-client
    ```

1. Com um editor de texto, crie um novo **ReportConnectivity.py** ficheiro.

3. Adicione o código seguinte para importar os módulos necessários do SDK do serviço:

    ```python
    import time
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError
    ```

2. Adicione o seguinte código, substituindo o marcador de posição para `[IoTHub Device Connection String]` com a cadeia de ligação para o dispositivo de hub IoT que criou nas secções anteriores.
   
    ```python
    CONNECTION_STRING = "[IoTHub Device Connection String]"

    # choose HTTP, AMQP, AMQP_WS or MQTT as transport protocol
    PROTOCOL = IoTHubTransportProvider.MQTT

    TIMER_COUNT = 5
    TWIN_CONTEXT = 0
    SEND_REPORTED_STATE_CONTEXT = 0
    ```

1. Adicione o seguinte código para o **ReportConnectivity.py** ficheiros para implementar o dispositivo duplos funcionalidades:

    ```python
    def device_twin_callback(update_state, payload, user_context):
        print ( "" )
        print ( "Twin callback called with:" )
        print ( "    updateStatus: %s" % update_state )
        print ( "    payload: %s" % payload )

    def send_reported_state_callback(status_code, user_context):
        print ( "" )
        print ( "Confirmation for reported state called with:" )
        print ( "    status_code: %d" % status_code )

    def iothub_client_init():
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

        if client.protocol == IoTHubTransportProvider.MQTT or client.protocol == IoTHubTransportProvider.MQTT_WS:
            client.set_device_twin_callback(
                device_twin_callback, TWIN_CONTEXT)

        return client

    def iothub_client_sample_run():
        try:
            client = iothub_client_init()

            if client.protocol == IoTHubTransportProvider.MQTT:
                print ( "Sending data as reported property..." )

                reported_state = "{\"connectivity\":\"cellular\"}"

                client.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)

            while True:
                print ( "Press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= TIMER_COUNT:
                    status = client.get_send_status()
                    time.sleep(10)
                    status_counter += 1 
        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
    ```   

    O **cliente** objeto expõe todos os métodos necessários para interagir com dispositivos duplos do dispositivo. O código anterior, depois, inicializa a **cliente** objeto, obtém o dispositivo duplo para o seu dispositivo e atualiza a respetiva propriedade comunicada com as informações de conectividade.

1. Adicione o seguinte código no final do **ReportConnectivity.py** para implementar o **iothub_client_sample_run** função:
   
    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Device Twins Python client sample..." )

        iothub_client_sample_run()
    ```

1. Executar a aplicação de dispositivo
   
    ```cmd/sh
    python ReportConnectivity.js
    ```
   
    Deverá ver a confirmação que foram atualizados os dispositivos duplos.

    ![Atualizar duplos][2]

6. Agora que o dispositivo comunicou as respetivas informações de conectividade, deverão ser apresentados em ambas as consultas. Voltar atrás e execute novamente as consultas:
   
    ```cmd/sh
    python AddTagsAndQuery.js
    ```
   
    Neste momento o **{Id de dispositivo}** deve aparecer em ambos os resultados da consulta.
   
    ![segundo consulta][3]

## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, configurou um novo Hub IoT no portal do Azure e, em seguida, criou uma identidade de dispositivo no registo de identidades do Hub IoT. Adicionar metadados do dispositivo como etiquetas a partir de uma aplicação de back-end e escreveu uma aplicação de dispositivo simulado para informações de conectividade do dispositivo de relatório no dispositivo duplo. Também aprendeu como consultar estas informações com o registo.

Utilize os seguintes recursos para saber como:

* Enviar telemetria a partir de dispositivos com o [introdução ao IoT Hub] [ lnk-iothub-getstarted] tutorial,
* configurar dispositivos utilizando propriedades pretendida do dispositivo duplo com a [utilização pretendida propriedades para configurar dispositivos] [ lnk-twin-how-to-configure] tutorial,
* controlar dispositivos interativamente (como ativar uma ventoinha a partir de uma aplicação controlados pelo utilizador), com o [utilizar métodos diretos] [ lnk-methods-tutorial] tutorial.

<!-- images -->
[1]: media/iot-hub-python-twin-getstarted/1.png
[2]: media/iot-hub-python-twin-getstarted/2.png
[3]: media/iot-hub-python-twin-getstarted/3.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-python-devbox]: https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-python-getstarted.md
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
